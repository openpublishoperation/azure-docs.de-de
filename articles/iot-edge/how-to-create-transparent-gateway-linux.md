---
title: Erstellen eines transparenten Gateways mit Azure IoT Edge – Linux | Microsoft-Dokumentation
description: Verwenden von Azure IoT Edge zum Erstellen eines transparenten Gateways, das Informationen für mehrere Geräte verarbeiten kann
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 6/20/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: df1ca1358d1b111d8412d730575eb7bf66c8ebdf
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46950011"
---
# <a name="create-a-linux-iot-edge-device-that-acts-as-a-transparent-gateway"></a>Erstellen eines Linux-IoT Edge-Geräts, das als transparentes Gateway fungiert

Dieser Artikel enthält eine ausführliche Anleitung zur Verwendung eines IoT Edge-Geräts als transparentes Gateway. Im restlichen Teil dieses Artikels bezieht sich der Begriff *IoT Edge-Gateway* auf ein IoT Edge-Gerät, das als transparentes Gateway verwendet wird. Ausführlichere Informationen finden Sie unter [Verwendung eines IoT Edge-Geräts als Gateway][lnk-edge-as-gateway], das einen konzeptionellen Überblick bietet.

>[!NOTE]
>Derzeit gilt Folgendes:
> * Wenn das Gateway vom IoT Hub getrennt wird, können sich nachgeschaltete Geräte nicht am Gateway authentifizieren.
> * Edge-fähige Geräte können keine Verbindung mit IoT Edge-Gateways herstellen. 
> * Nachgeschaltete Geräte können keinen Dateiupload verwenden.

Die größte Schwierigkeit beim Erstellen eines transparenten Gateways ist das Herstellen einer sicheren Verbindung zwischen dem Gateway und nachgeschalteten Geräten. Azure IoT Edge ermöglicht Ihnen, mithilfe der PKI-Infrastruktur sichere TLS-Verbindungen zwischen diesen Geräten einzurichten. In diesem Fall lassen wir zu, dass ein nachgeschaltetes Gerät eine Verbindung mit einem IoT Edge-Gerät, das als transparentes Gateway fungiert, herstellt.  Um eine angemessene Sicherheit zu gewährleisten, sollte das nachgeschaltete Gerät die Identität des Edge-Geräts bestätigen, da Ihre Geräte nur Verbindungen mit Ihren Gateways und nicht mit potenziell schädlichen Gateways herstellen sollen.

Sie können eine beliebige Zertifikatinfrastruktur erstellen, die die für Ihre Gerät-zu-Gateway-Topologie erforderliche Vertrauensstellung ermöglicht. In diesem Artikel wird von der gleichen Zertifikateinrichtung ausgegangen, die Sie auch zum Aktivieren der [X.509-Sicherheit][lnk-iothub-x509] in IoT Hub verwenden. Hierbei ist ein X.509-Zertifizierungsstellenzertifikat einem bestimmten IoT Hub zugeordnet (der IoT Hub-Besitzerzertifizierungsstelle), und es ist eine Reihe von Zertifikaten, die mit dieser Zertifizierungsstelle signiert werden, sowie eine Zertifizierungsstelle für die IoT Edge-Geräte vorhanden.

![Gatewaysetup][1]

Das Gateway legt dem nachgeschalteten Gerät während der Initiierung der Verbindung das Zertifizierungsstellenzertifikat des eigenen Edge-Geräts vor. Die nachgeschaltete Gerät überprüft, ob das Zertifizierungsstellenzertifikat des Edge-Geräts mit dem Zertifizierungsstellenzertifikat des Besitzers signiert wurde. Durch diesen Vorgang kann das nachgeschaltete Gerät bestätigen, dass das Gateway aus einer vertrauenswürdigen Quelle stammt.

In den folgenden Schritten werden Sie durch den Prozess zum Erstellen der Zertifikate und zum Installieren an den richtigen Orten geführt.

## <a name="prerequisites"></a>Voraussetzungen
1.  Installieren Sie die Azure IoT Edge-Runtime auf einem Linux-Gerät, das Sie als transparentes Gateway verwenden möchten.
   * [Linux x64][lnk-install-linux-x64]
   * [Linux ARM32][lnk-install-linux-arm]

2.  Rufen Sie die Skripts zum Generieren der erforderlichen Nicht-Produktions-Zertifikate mit dem folgenden Befehl ab. Diese Skripts helfen Ihnen dabei, die erforderlichen Zertifikate zum Einrichten eines transparenten Gateways zu erstellen. 

   ```cmd
   git clone https://github.com/Azure/azure-iot-sdk-c.git
   ```

3. Diese Skripts verwenden OpenSSL, um die erforderlichen Zertifikate zu generieren, und für OpenSSL ist eine Einrichtung erforderlich.
   
   1. Navigieren Sie zu dem Verzeichnis, in dem Sie arbeiten möchten. Wir bezeichnen es ab jetzt als „$WRKDIR“.  Alle Dateien werden in diesem Verzeichnis erstellt.

      cd $WRKDIR
   
   1. Kopieren Sie die Konfigurations- und Skriptdateien in Ihr Arbeitsverzeichnis.

      ```cmd
      cp azure-iot-sdk-c/tools/CACertificates/*.cnf .
      cp azure-iot-sdk-c/tools/CACertificates/certGen.sh .
      chmod 700 certGen.sh 
      ```

## <a name="certificate-creation"></a>Zertifikaterstellung
1.  Erstellen Sie das Zertifizierungsstellenzertifikat für den Eigentümer und ein Zwischenzertifikat. Diese befinden sich alle unter `$WRKDIR`.

   ```cmd
   ./certGen.sh create_root_and_intermediate
   ```

   Bei der Skriptausführung werden die folgenden Zertifikate und Schlüssel ausgegeben:
   * Zertifikate
      * `$WRKDIR/certs/azure-iot-test-only.root.ca.cert.pem`
      * `$WRKDIR/certs/azure-iot-test-only.intermediate.cert.pem`
   * Schlüssel
      * `$WRKDIR/private/azure-iot-test-only.root.ca.key.pem`
      * `$WRKDIR/private/azure-iot-test-only.intermediate.key.pem`

2.  Erstellen Sie das Zertifizierungsstellenzertifikat und den privaten Schlüssel für das Edge-Gerät mit dem nachstehenden Befehl.

   >[!NOTE]
   > Verwenden Sie **KEINEN** Namen, der dem DNS-Hostnamen des Gateways entspricht. Andernfalls tritt bei der Clientzertifizierung für diese Zertifikate ein Fehler auf.

   ```cmd
   ./certGen.sh create_edge_device_certificate "<gateway device name>"
   ```

   Bei der Skriptausführung werden die folgenden Zertifikate und der folgende Schlüssel ausgegeben:
   * `$WRKDIR/certs/new-edge-device.*`
   * `$WRKDIR/private/new-edge-device.key.pem`

## <a name="certificate-chain-creation"></a>Erstellung der Zertifikatkette
Erstellen Sie mit dem nachstehenden Befehl eine Zertifikatkette aus dem Zertifizierungsstellenzertifikat des Besitzers, dem Zwischenzertifikat und dem Zertifizierungsstellenzertifikat des Edge-Geräts. Indem Sie eine Kettendatei erstellen, können Sie sie leicht auf Ihrem Edge-Gerät installieren, das als transparentes Gateway fungiert.

   ```cmd
   cat ./certs/new-edge-device.cert.pem ./certs/azure-iot-test-only.intermediate.cert.pem ./certs/azure-iot-test-only.root.ca.cert.pem > ./certs/new-edge-device-full-chain.cert.pem
   ```

## <a name="installation-on-the-gateway"></a>Installation auf dem Gateway
1.  Kopieren Sie die folgenden Dateien aus $WRKDIR an einen beliebigen Speicherort auf Ihrem Edge-Gerät. Dieser wird nun als „$CERTDIR“ bezeichnet. Überspringen Sie diesen Schritt, wenn Sie die Zertifikate auf Ihrem Edge-Gerät generiert haben.

   * Zertifizierungsstellenzertifikat des Geräts – `$WRKDIR/certs/new-edge-device-full-chain.cert.pem`
   * Privater Zertifizierungsstellenschlüssel des Geräts – `$WRKDIR/private/new-edge-device.key.pem`
   * Zertifizierungsstelle des Besitzers – `$WRKDIR/certs/azure-iot-test-only.root.ca.cert.pem`
   
2. Öffnen Sie die IoT-Edge-Konfigurationsdatei. Da es eine geschützte Datei ist, müssen Sie ggf. erhöhte Rechte nutzen, um darauf zuzugreifen.
   
   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

3.  Legen Sie die `certificate`-Eigenschaften in der YAML-Konfigurationsdatei des Daemons für IoT Edge auf den Pfad fest, in dem Sie die Zertifikat- und Schlüsseldateien platziert haben.

   ```yaml
   certificates:
     device_ca_cert: "$CERTDIR/certs/new-edge-device-full-chain.cert.pem"
     device_ca_pk: "$CERTDIR/private/new-edge-device.key.pem"
     trusted_ca_certs: "$CERTDIR/certs/azure-iot-test-only.root.ca.cert.pem"
   ```

## <a name="deploy-edgehub-to-the-gateway"></a>Bereitstellen von Edge Hub für das Gateway
Eine der wichtigen Funktionen von Azure IoT Edge ist die Möglichkeit, Module aus der Cloud auf IoT Edge-Geräten bereitzustellen. In diesem Abschnitt erstellen Sie eine Bereitstellung, die scheinbar leer ist. Edge Hub wird aber auch dann allen Bereitstellungen automatisch hinzugefügt, wenn keine weiteren Module vorhanden sind. Edge Hub ist das einzige Modul, das auf einem Edge-Gerät benötigt wird, damit es als transparentes Gateway fungieren kann. Das Erstellen einer leeren Bereitstellung ist somit ausreichend. 
1. Navigieren Sie im Azure-Portal zu Ihrem IoT Hub.
2. Wechseln Sie zu **IoT Edge**, und wählen Sie das IoT Edge-Gerät aus, das Sie als Gateway verwenden möchten.
3. Wählen Sie **Module festlegen** aus.
4. Klicken Sie auf **Weiter**.
5. Im Schritt zum **Angeben von Routen** sollten Sie über eine Standardroute verfügen, über die alle Nachrichten von allen Modulen an IoT Hub gesendet werden. Ist das nicht der Fall, können Sie den folgenden Code eingeben und dann **Weiter** auswählen.
   ```JSON
   {
       "routes": {
           "route": "FROM /* INTO $upstream"
       }
   }
   ```
6. Wählen Sie im Schritt zum Überprüfen der Vorlage die Option **Senden**.

## <a name="installation-on-the-downstream-device"></a>Installation auf dem nachgeschalteten Gerät
Ein nachgeschaltetes Gerät kann eine beliebige Anwendung sein, die das [Azure IoT-Geräte-SDK][lnk-devicesdk] verwendet, wie beispielsweise die einfache Anwendung, die unter [Herstellen einer Verbindung zwischen Ihrem Gerät und Ihrem IoT Hub mit .NET][lnk-iothub-getstarted] beschrieben ist. Eine Anwendung auf einem nachgeschalteten Gerät muss dem Zertifikat der **Besitzerzertifizierungsstelle** vertrauen, um die TLS-Verbindungen mit den Gatewaygeräten zu überprüfen. Dieser Schritt kann in der Regel auf zwei Arten ausgeführt werden: auf Betriebssystemebene oder (für bestimmte Sprachen) auf Anwendungsebene.

### <a name="os-level"></a>BS-Ebene
Durch das Installieren dieses Zertifikats im Zertifikatspeicher des Betriebssystems können alle Anwendungen das Zertifizierungsstellenzertifikat des Besitzers als vertrauenswürdiges Zertifikat verwenden.

* Ubuntu: Hier ist ein Beispiel für die Installation eines Zertifizierungsstellenzertifikats auf einem Ubuntu-Host angegeben.

   ```cmd
   sudo cp $CERTDIR/certs/azure-iot-test-only.root.ca.cert.pem  /usr/local/share/ca-certificates/azure-iot-test-only.root.ca.cert.pem.crt
   sudo update-ca-certificates
   ```
 
    Es sollte die Meldung „Updating certificates in /etc/ssl/certs... 1 added, 0 removed; done“ (Zertifikate in /etc/ssl/certs werden aktualisiert... 1 hinzugefügt, 0 entfernt; fertig) angezeigt werden.

* Windows: Hier ist ein Beispiel für die Installation eines Zertifizierungsstellenzertifikats auf einem Windows-Host angegeben.
  * Geben Sie im Startmenü „Computerzertifikate verwalten“ ein. Dadurch sollte ein Hilfsprogramm namens `certlm` aufgerufen werden.
  * Navigieren Sie zu „Zertifikate > Lokaler Computer“ > „Vertrauenswürdige Stammzertifikate“ > „Zertifikate“. Führen Sie einen Rechtsklick aus, und navigieren Sie zu „Alle Tasks“ und dann zu „Import to launch the certificate import wizard“ (Importieren, um den Importassistenten für Zertifikate zu starten).
  * Führen Sie die Schritte wie angegeben aus, und importieren Sie die Zertifikatdatei „$CERTDIR/certs/azure-iot-test-only.root.ca.cert.pem“.
  * Wenn Sie damit fertig sind, sollte die Meldung „Der Import war erfolgreich.“ angezeigt werden.

### <a name="application-level"></a>Anwendungsschicht
Sie können für .NET-Anwendungen den folgenden Codeausschnitt hinzufügen, um ein Zertifikat im PEM-Format als vertrauenswürdig einzustufen. Initialisieren Sie die `certPath`-Variable mit `$CERTDIR/certs/azure-iot-test-only.root.ca.cert.pem`.

   ```
   using System.Security.Cryptography.X509Certificates;

   ...

   X509Store store = new X509Store(StoreName.Root, StoreLocation.CurrentUser);
   store.Open(OpenFlags.ReadWrite);
   store.Add(new X509Certificate2(X509Certificate2.CreateFromCertFile(certPath)));
   store.Close();
   ```

## <a name="connect-the-downstream-device-to-the-gateway"></a>Herstellen einer Verbindung des nachgeschalteten Geräts mit dem Gateway
Das IoT Hub-Geräte-SDK muss mit einer Verbindungszeichenfolge initialisiert werden, die auf den Hostnamen des Gatewaygeräts verweist. Dazu wird die `GatewayHostName`-Eigenschaft an die Verbindungszeichenfolge des Geräts angefügt. Es folgt ein Beispiel für die Verbindungszeichenfolge eines Geräts mit angefügter `GatewayHostName`-Eigenschaft:

   ```
   HostName=yourHub.azure-devices.net;DeviceId=yourDevice;SharedAccessKey=XXXYYYZZZ=;GatewayHostName=mygateway.contoso.com
   ```

   >[!NOTE]
   >Dies ist ein Beispielbefehl, der überprüft, ob alles ordnungsgemäß eingerichtet wurde. Es sollte eine Meldung mit dem Inhalt „verified OK“ (Überprüfung OK) angezeigt werden.
   >
   >openssl s_client -connect mygateway.contoso.com:8883 -CAfile $CERTDIR/certs/azure-iot-test-only.root.ca.cert.pem -showcerts

## <a name="routing-messages-from-downstream-devices"></a>Weiterleiten von Nachrichten von nachgeschalteten Geräten
Die IoT Edge-Runtime kann Nachrichten für nachgeschaltete Geräte genau wie Nachrichten von Modulen weiterleiten. Dadurch können Sie die Analyse in einem auf dem Gateway ausgeführten Modul durchführen, bevor Daten an die Cloud gesendet werden. Die nachstehende Route wird zum Senden von Nachrichten von einem nachgeschalteten Gerät mit dem Namen `sensor` an ein Modul mit dem Namen `ai_insights` verwendet.

   ```json
   { "routes":{ "sensorToAIInsightsInput1":"FROM /messages/* WHERE NOT IS_DEFINED($connectionModuleId) INTO BrokeredEndpoint(\"/modules/ai_insights/inputs/input1\")", "AIInsightsToIoTHub":"FROM /messages/modules/ai_insights/outputs/output1 INTO $upstream" } }
   ```

Ausführliche Informationen zur Weiterleitung von Nachrichten finden Sie im [Artikel über die Modulzusammenstellung][lnk-module-composition].

[!INCLUDE [](../../includes/iot-edge-extended-offline-preview.md)]

## <a name="next-steps"></a>Nächste Schritte
[Grundlegendes zu den Anforderungen und Tools für die Entwicklung von IoT Edge-Modulen][lnk-module-dev].

<!-- Images -->
[1]: ./media/how-to-create-transparent-gateway/gateway-setup.png

<!-- Links -->
[lnk-install-linux-x64]: ./how-to-install-iot-edge-linux.md
[lnk-install-linux-arm]: ./how-to-install-iot-edge-linux-arm.md
[lnk-module-composition]: ./module-composition.md
[lnk-devicesdk]: ../iot-hub/iot-hub-devguide-sdks.md
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
[lnk-edge-as-gateway]: ./iot-edge-as-gateway.md
[lnk-module-dev]: module-development.md
[lnk-iothub-getstarted]: ../iot-hub/quickstart-send-telemetry-dotnet.md
[lnk-iothub-x509]: ../iot-hub/iot-hub-x509ca-overview.md
[lnk-iothub-secure-deployment]: ../iot-hub/iot-hub-security-deployment.md
[lnk-iothub-tokens]: ../iot-hub/iot-hub-devguide-security.md#security-tokens
[lnk-iothub-throttles-quotas]: ../iot-hub/iot-hub-devguide-quotas-throttling.md
[lnk-iothub-devicetwins]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-iothub-c2d]: ../iot-hub/iot-hub-devguide-messages-c2d.md
[lnk-ca-scripts]: https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md
[lnk-modbus-module]: https://github.com/Azure/iot-edge-modbus
