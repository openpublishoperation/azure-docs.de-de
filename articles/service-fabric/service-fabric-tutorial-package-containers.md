---
title: Packen und Bereitstellen von Containern als Service Fabric-App in Azure | Microsoft-Dokumentation
description: In diesem Tutorial wird beschrieben, wie Sie die Definition einer Azure Service Fabric-Anwendung mit Yeoman generieren und aus der Anwendung ein Paket erstellen.
services: service-fabric
documentationcenter: ''
author: suhuruli
manager: timlt
editor: suhuruli
tags: servicefabric
keywords: Docker, Container, Microservices, Service Fabric, Azure
ms.assetid: ''
ms.service: service-fabric
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/12/2017
ms.author: suhuruli
ms.custom: mvc
ms.openlocfilehash: 952550225f2bdd8559d72a9d283993451ae7f60b
ms.sourcegitcommit: a62cbb539c056fe9fcd5108d0b63487bd149d5c3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2018
ms.locfileid: "42616342"
---
# <a name="tutorial-package-and-deploy-containers-as-a-service-fabric-application-using-yeoman"></a>Tutorial: Packen und Bereitstellen von Containern als Service Fabric-Anwendung mithilfe von Yeoman

Dieses Tutorial ist der zweite Teil einer Reihe. In diesem Tutorial wird ein Tool zum Generieren einer Vorlage (Yeoman) zum Generieren einer Service Fabric-Anwendungsdefinition verwendet. Diese Anwendung kann dann zum Bereitstellen von Containern in Service Fabric verwendet werden. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Installieren von Yeoman
> * Erstellen eines Anwendungspakets mit Yeoman
> * Konfigurieren von Einstellungen im Anwendungspaket für die Verwendung mit Containern
> * Erstellen der Anwendung
> * Bereitstellen und Ausführen der Anwendung
> * Bereinigen der Anwendung

## <a name="prerequisites"></a>Voraussetzungen

* Die in [Teil 1](service-fabric-tutorial-create-container-images.md) dieser Tutorialreihe erstellten, per Pushübertragung an Azure Container Registry gesendeten Containerimages werden verwendet.
* Die Linux-Entwicklungsumgebung ist [eingerichtet](service-fabric-tutorial-create-container-images.md).

## <a name="install-yeoman"></a>Installieren von Yeoman

Die Gerüstbautools von Service Fabric unterstützen Sie beim Erstellen von Anwendungen über das Terminal unter Verwendung des Yeoman-Vorlagengenerators. Führen Sie die Schritte unten durch, um sicherzustellen, dass Sie über den Yeoman-Vorlagengenerator verfügen.

1. Installieren Sie Node.js und npm auf dem Computer. Beachten Sie, dass Mac OSX-Benutzer den Paket-Manager Homebrew verwenden müssen.

    ```bash
    curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.0/install.sh | bash
    nvm install node 
    ```
2. Installieren des Yeoman-Vorlagengenerators auf dem Computer über npm

    ```bash
    npm install -g yo
    ```
3. Installieren des Service Fabric-Yeoman-Containergenerators

    ```bash 
    npm install -g generator-azuresfcontainer
    ```

## <a name="package-a-docker-image-container-with-yeoman"></a>Packen eines Docker-Imagecontainers mit Yeoman

1. Zum Erstellen einer Service Fabric-Containeranwendung im Verzeichnis „container-tutorial“ des geklonten Repositorys führen Sie den folgenden Befehl aus.

    ```bash
    yo azuresfcontainer
    ```
2. Geben Sie „TestContainer“ als Namen für Ihre Anwendung ein.
3. Geben Sie „azurevotefront“ als Namen für Ihren Anwendungsdienst ein.
4. Geben Sie den Pfad des Containerimages für das Front-End-Repository in ACR ein – Beispiel: \<acrName>.azurecr.io/azure-vote-front:v1. Das \<acrName>-Feld muss dem Wert entsprechen, der im vorangehenden Tutorial verwendet wurde.
5. Drücken Sie die EINGABETASTE, um den Abschnitt „Commands“ leer zu lassen.
6. Geben Sie als Instanzanzahl 1 an.

Das folgende Beispiel zeigt die Eingabe und Ausgabe der Ausführung des yo-Befehls:

```bash
? Name your application TestContainer
? Name of the application service: azurevotefront
? Input the Image Name: <acrName>.azurecr.io/azure-vote-front:v1
? Commands:
? Number of instances of guest container application: 1
   create TestContainer/TestContainer/ApplicationManifest.xml
   create TestContainer/TestContainer/azurevotefrontPkg/ServiceManifest.xml
   create TestContainer/TestContainer/azurevotefrontPkg/config/Settings.xml
   create TestContainer/TestContainer/azurevotefrontPkg/code/Dummy.txt
   create TestContainer/install.sh
   create TestContainer/uninstall.sh
```

Führen Sie zum Hinzufügen eines weiteren Containerdiensts zu einer Anwendung, die bereits mit Yeoman erstellt wurde, die folgenden Schritte aus:

1. Wechseln Sie um eine Ebene in das Verzeichnis **TestContainer** (Beispiel: *./TestContainer*).
2. Führen Sie `yo azuresfcontainer:AddService` aus.
3. Benennen Sie den Dienst „azurevoteback“.
4. Geben Sie den Containerimagepfad für Redis an: alpine:redis.
5. Drücken Sie die EINGABETASTE, um den Abschnitt „Commands“ leer zu lassen.
6. Geben Sie als Instanzanzahl „1“ an.

Alle Einträge für das Hinzufügen des verwendeten Diensts werden angezeigt:

```bash
? Name of the application service: azurevoteback
? Input the Image Name: alpine:redis
? Commands:
? Number of instances of guest container application: 1
   create TestContainer/azurevotebackPkg/ServiceManifest.xml
   create TestContainer/azurevotebackPkg/config/Settings.xml
   create TestContainer/azurevotebackPkg/code/Dummy.txt
```

Für den Rest dieses Tutorials arbeiten wir nur im Verzeichnis **TestContainer**. Beispiel: *./TestContainer/TestContainer*. Der Inhalt dieses Verzeichnisses muss wie folgt aussehen.

```bash
$ ls
ApplicationManifest.xml azurevotefrontPkg azurevotebackPkg
```

## <a name="configure-the-application-manifest-with-credentials-for-azure-container-registry"></a>Konfigurieren des Anwendungsmanifests mit Anmeldeinformationen für Azure Container Registry

Zur Pullübertragung der Containerimages aus Azure Container Registry durch Service Fabric müssen wir die Anmeldeinformationen in der Datei **ApplicationManifest.xml** angeben.

Melden Sie sich bei der ACR-Instanz an. Verwenden Sie den Befehl **az acr login**, um den Vorgang abzuschließen. Geben Sie den eindeutigen Namen an, den die Containerregistrierung bei ihrer Erstellung erhalten hat.

```bash
az acr login --name <acrName>
```

Nach Abschluss des Vorgangs wird eine **Erfolgsmeldung** zurückgegeben.

Führen Sie den folgenden Befehl zum Abrufen des Kennworts Ihrer Containerregistrierung aus. Dieses Kennwort wird von Service Fabric für die Authentifizierung bei ACR zur Pullübertragung der Containerimages verwendet.

```bash
az acr credential show -n <acrName> --query passwords[0].value
```

Fügen Sie in **ApplicationManifest.xml** den Codeausschnitt unter dem **ServiceManifestImport**-Element für den Front-End-Dienst ein. Fügen Sie Ihren **acrName** in das **AccountName**-Feld und das vom vorherigen Befehl zurückgegebene Kennwort in das **Password** Feld ein. Eine vollständige **ApplicationManifest.xml** finden Sie am Ende dieses Dokuments.

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="Code">
    <RepositoryCredentials AccountName="<acrName>" Password="<password>" PasswordEncrypted="false"/>
  </ContainerHostPolicies>
</Policies>
```

## <a name="configure-communication-and-container-port-to-host-port-mapping"></a>Konfigurieren der Kommunikation und der Zuordnung von Containerport zu Hostport

### <a name="configure-communication-port"></a>Konfigurieren des Kommunikationsports

Konfigurieren Sie einen HTTP-Endpunkt, damit Clients mit dem Dienst kommunizieren können. Öffnen Sie die Datei *./TestContainer/azurevotefrontPkg/ServiceManifest.xml*, und deklarieren Sie im Element **ServiceManifest** eine Endpunktressource.  Fügen Sie das Protokoll, den Port und den Namen hinzu. Im Rahmen dieses Tutorials lauscht der Dienst an Port 80. Der folgende Codeausschnitt befindet sich unter dem *ServiceManifest*-Tag in der Ressource.

```xml
<Resources>
  <Endpoints>
    <!-- This endpoint is used by the communication listener to obtain the port on which to 
            listen. Please note that if your service is partitioned, this port is shared with 
            replicas of different partitions that are placed in your code. -->
    <Endpoint Name="azurevotefrontTypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
  </Endpoints>
</Resources>

```

Ändern Sie auf ähnliche Weise das Dienstmanifest für den Back-End-Dienst. Öffnen Sie die Datei *./TestContainer/azurevotebackPkg/ServiceManifest.xml*, und deklarieren Sie eine Endpunktressource im Element **ServiceManifest**. Für dieses Tutorial wird der Redis-Standard von 6379 beibehalten. Der folgende Codeausschnitt befindet sich unter dem *ServiceManifest*-Tag in der Ressource.

```xml
<Resources>
  <Endpoints>
    <!-- This endpoint is used by the communication listener to obtain the port on which to 
            listen. Please note that if your service is partitioned, this port is shared with 
            replicas of different partitions that are placed in your code. -->
    <Endpoint Name="azurevotebackTypeEndpoint" Port="6379" Protocol="tcp"/>
  </Endpoints>
</Resources>
```

Durch die Bereitstellung von **UriScheme** wird automatisch der Containerendpunkt im Service Fabric Naming Service registriert, damit er einfacher erkennbar ist. Eine vollständige „ServiceManifest.xml“-Beispieldatei für den Back-End-Dienst finden Sie am Ende dieses Artikels.

### <a name="map-container-ports-to-a-service"></a>Zuordnen von Containerports zu einem Dienst

Um die Container im Cluster verfügbar zu machen, müssen wir außerdem eine Portbindung in der Datei „ApplicationManifest.xml“ erstellen. Die **PortBinding**-Richtlinie verweist auf die **Endpunkte**, die wir in den **ServiceManifest.xml**-Dateien definiert haben. Bei diesen Endpunkten eingehende Anforderungen werden den Containerports zugeordnet, die hier geöffnet und gebunden sind. Fügen Sie in der **ApplicationManifest.xml**-Datei den folgenden Code hinzu, um Port 80 und 6379 an die Endpunkte zu binden. Eine vollständige **ApplicationManifest.xml** finden Sie am Ende dieses Dokuments.

```xml
<ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="80" EndpointRef="azurevotefrontTypeEndpoint"/>
</ContainerHostPolicies>
```

```xml
<ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="6379" EndpointRef="azurevotebackTypeEndpoint"/>
</ContainerHostPolicies>
```

### <a name="add-a-dns-name-to-the-backend-service"></a>Hinzufügen eines DNS-Namens zum Back-End-Dienst

Damit Service Fabric diesen DNS-Namen dem Back-End-Dienst zuweisen kann, muss der Name in der **ApplicationManifest.xml** angegeben werden. Fügen Sie das **ServiceDnsName**-Attribut dem **Service**-Element wie gezeigt hinzu:

```xml
<Service Name="azurevoteback" ServiceDnsName="redisbackend.testapp">
  <StatelessService ServiceTypeName="azurevotebackType" InstanceCount="1">
    <SingletonPartition/>
  </StatelessService>
</Service>
```

Der Front-End-Dienst liest eine Umgebungsvariable, um den DNS-Namen der Redis-Instanz zu kennen. Diese Umgebungsvariable ist bereits in der Dockerfile definiert, die zum Generieren des Docker-Images verwendet wurde. Es ist keine weitere Maßnahme erforderlich.

```Dockerfile
ENV REDIS redisbackend.testapp
```

Im folgenden Codeausschnitt wird veranschaulicht, wie die in der Dockerfile beschriebene Umgebungsvariable vom Front-End-Python-Code ausgewählt wird. Es ist keine weitere Maßnahme erforderlich.

```python
# Get DNS Name
redis_server = os.environ['REDIS']

# Connect to the Redis store
r = redis.StrictRedis(host=redis_server, port=6379, db=0)
```

An diesem Punkt des Tutorials ist die Vorlage für eine Service Package-Anwendung zur Bereitstellung in einem Cluster verfügbar. Im nachfolgenden Tutorial wird diese Anwendung bereitgestellt und in einem Service Fabric-Cluster ausgeführt.

## <a name="create-a-service-fabric-cluster"></a>Erstellen von Service Fabric-Clustern

Um die Anwendung in einem Cluster in Azure bereitzustellen, erstellen Sie einen eigenen Cluster.

Bei Partyclustern handelt es sich um zeitlich begrenzte kostenlose Service Fabric-Cluster, die in Azure gehostet werden. Sie werden vom Service Fabric-Team ausgeführt, und alle Interessenten können Anwendungen bereitstellen und sich mit der Plattform vertraut machen. [Befolgen Sie die Anweisungen](http://aka.ms/tryservicefabric), um Zugriff auf einen Partycluster zu erhalten.

Zum Durchführen von Verwaltungsvorgängen im sicheren Partycluster können Sie Service Fabric Explorer, die CLI oder PowerShell verwenden. Zum Verwenden von Service Fabric Explorer müssen Sie die PFX-Datei von der Partycluster-Website herunterladen und das Zertifikat in Ihren Zertifikatspeicher (Windows oder Mac) oder in den Browser selbst (Ubuntu) importieren. Für selbstsignierte Zertifikate aus dem Partycluster wird kein Kennwort verwendet.

Zum Durchführen von Verwaltungsvorgängen per PowerShell oder CLI benötigen Sie die PFX- (PowerShell) oder die PEM-Datei (CLI). Führen Sie den folgenden Befehl aus, um die PFX-Datei in eine PEM-Datei zu konvertieren:

```bash
openssl pkcs12 -in party-cluster-1277863181-client-cert.pfx -out party-cluster-1277863181-client-cert.pem -nodes -passin pass:
```

Informationen zum Erstellen Ihres eigenen Clusters finden Sie unter [Bereitstellen eines Service Fabric-Linux-Clusters in einem virtuellen Azure-Netzwerk](service-fabric-tutorial-create-vnet-and-linux-cluster.md).

## <a name="build-and-deploy-the-application-to-the-cluster"></a>Erstellen der Anwendung und Bereitstellen im Cluster

Sie können die Anwendung mithilfe der Service Fabric-Befehlszeilenschnittstelle im Azure-Cluster bereitstellen. Wenn die Service Fabric-CLI nicht auf dem Computer installiert ist, installieren Sie sie mittels [dieser](service-fabric-get-started-linux.md#set-up-the-service-fabric-cli) Anweisungen.

Stellen Sie eine Verbindung mit dem Service Fabric-Cluster in Azure her. Ersetzen Sie den Beispielendpunkt durch Ihren eigenen Endpunkt. Der Endpunkt muss wie unten angegeben einer vollständigen URL entsprechen.

```bash
sfctl cluster select --endpoint https://linh1x87d1d.westus.cloudapp.azure.com:19080 --pem party-cluster-1277863181-client-cert.pem --no-verify
```

Verwenden Sie das im Verzeichnis **TestContainer** bereitgestellte Installationsskript, um das Anwendungspaket in den Imagespeicher des Clusters zu kopieren, den Anwendungstyp zu registrieren und eine Instanz der Anwendung zu erstellen.

```bash
./install.sh
```

Öffnen Sie einen Browser, und navigieren Sie zu Service Fabric Explorer (http://lin4hjim3l4.westus.cloudapp.azure.com:19080/Explorer). Erweitern Sie den Knoten „Anwendungen“. Hier finden Sie einen Eintrag für Ihren Anwendungstyp und einen weiteren für die Instanz.

![Service Fabric Explorer][sfx]

Um eine Verbindung mit der ausgeführten Anwendung herzustellen, öffnen Sie einen Webbrowser, und navigieren Sie zur Cluster-URL, etwa http://lin0823ryf2he.cloudapp.azure.com:80. Die Abstimmungsanwendung sollte in der Webbenutzeroberfläche angezeigt werden.

![votingapp][votingapp]

## <a name="clean-up"></a>Bereinigen

Verwenden Sie das Deinstallationsskript aus der Vorlage, um die Anwendungsinstanz aus dem Cluster zu löschen und die Registrierung des Anwendungstyps aufzuheben. Bei diesem Befehl dauert das Bereinigen der Instanz einige Zeit, und der Befehl „install.sh“ sollte nicht direkt nach diesem Skript ausgeführt werden.

```bash
./uninstall.sh
```

## <a name="examples-of-completed-manifests"></a>Beispiele abgeschlossener Manifeste

### <a name="applicationmanifestxml"></a>ApplicationManifest.xml

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<ApplicationManifest ApplicationTypeName="TestContainerType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="azurevotefrontPkg" ServiceManifestVersion="1.0.0"/>
    <Policies>
    <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myaccountname" Password="<password>" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="80" EndpointRef="azurevotefrontTypeEndpoint"/>
    </ContainerHostPolicies>
        </Policies>
  </ServiceManifestImport>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="azurevotebackPkg" ServiceManifestVersion="1.0.0"/>
      <Policies>
        <ContainerHostPolicies CodePackageRef="Code">
          <PortBinding ContainerPort="6379" EndpointRef="azurevotebackTypeEndpoint"/>
        </ContainerHostPolicies>
      </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <Service Name="azurevotefront">
      <StatelessService ServiceTypeName="azurevotefrontType" InstanceCount="1">
        <SingletonPartition/>
      </StatelessService>
    </Service>
    <Service Name="azurevoteback" ServiceDnsName="redisbackend.testapp">
      <StatelessService ServiceTypeName="azurevotebackType" InstanceCount="1">
        <SingletonPartition/>
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```

### <a name="front-end-servicemanifestxml"></a>Front-End-ServiceManifest.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="azurevotefrontPkg" Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" >

   <ServiceTypes>
      <StatelessServiceType ServiceTypeName="azurevotefrontType" UseImplicitHost="true">
   </StatelessServiceType>
   </ServiceTypes>

   <CodePackage Name="code" Version="1.0.0">
      <EntryPoint>
         <ContainerHost>
            <ImageName>acrName.azurecr.io/azure-vote-front:v1</ImageName>
            <Commands></Commands>
         </ContainerHost>
      </EntryPoint>
      <EnvironmentVariables>
      </EnvironmentVariables>
   </CodePackage>

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="azurevotefrontTypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
    </Endpoints>
  </Resources>

 </ServiceManifest>
```

### <a name="redis-servicemanifestxml"></a>Redis-ServiceManifest.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="azurevotebackPkg" Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" >

   <ServiceTypes>
      <StatelessServiceType ServiceTypeName="azurevotebackType" UseImplicitHost="true">
   </StatelessServiceType>
   </ServiceTypes>

   <CodePackage Name="code" Version="1.0.0">
      <EntryPoint>
         <ContainerHost>
            <ImageName>alpine:redis</ImageName>
            <Commands></Commands>
         </ContainerHost>
      </EntryPoint>
      <EnvironmentVariables>
      </EnvironmentVariables>
   </CodePackage>
     <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="azurevotebackTypeEndpoint" Port="6379" Protocol="tcp"/>
    </Endpoints>
  </Resources>
 </ServiceManifest>
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde aus mehreren Containern in einer Service Fabric-Anwendung mit Yeoman ein Paket erstellt. Diese Anwendung wurde dann bereitgestellt und auf einem Service Fabric-Cluster ausgeführt. Die folgenden Schritte wurden durchgeführt:

> [!div class="checklist"]
> * Installieren von Yeoman
> * Erstellen eines Anwendungspakets mit Yeoman
> * Konfigurieren von Einstellungen im Anwendungspaket für die Verwendung mit Containern
> * Erstellen der Anwendung
> * Bereitstellen und Ausführen der Anwendung
> * Bereinigen der Anwendung

Wechseln Sie zum nächsten Tutorial, um Informationen zu Failover und Skalierung der Anwendung in Service Fabric zu bekommen.

> [!div class="nextstepaction"]
> [Demonstrate fail over and scaling of container services with Service Fabric](service-fabric-tutorial-containers-failover.md) (Demonstration von Failover und Skalierung von Containerdiensten mit Service Fabric)

[votingapp]: ./media/service-fabric-tutorial-deploy-run-containers/votingapp.png
[sfx]: ./media/service-fabric-tutorial-deploy-run-containers/containerspackagetutorialsfx.png