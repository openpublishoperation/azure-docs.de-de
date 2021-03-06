---
title: Includedatei
description: Includedatei
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 655f0b625d9f1b7c7ad216e9276da62d8454380f
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30929426"
---
1. Klicken Sie im Portal auf der linken Seite auf **+**, und geben Sie für die Suche „Virtuelles Netzwerkgateway“ ein. Suchen Sie in der Ausgabe nach **Virtuelles Netzwerkgateway**, und klicken Sie auf den Eintrag. Klicken Sie auf der Seite **Gateway für virtuelle Netzwerke** unten auf **Erstellen**, um die Seite **Gateway für virtuelle Netzwerke erstellen** zu öffnen.
2. Geben Sie auf der Seite **Gateway für virtuelle Netzwerke erstellen** die Werte für das Gateway für virtuelle Netzwerke an.

  ![Felder der Seite „Gateway für virtuelle Netzwerke erstellen“](./media/vpn-gateway-add-gw-rm-portal-include/gw.png "Felder der Seite „Gateway für virtuelle Netzwerke erstellen“")
3. Geben Sie auf der Seite **Gateway für virtuelle Netzwerke erstellen** die Werte für das Gateway für virtuelle Netzwerke an.

  - **Name**: Benennen Sie Ihr Gateway. Dies ist nicht das Gleiche wie das Benennen eines Gatewaysubnetzes. Hierbei handelt es sich um den Namen des Gatewayobjekts, das Sie erstellen.
  - **Gatewaytyp**: Wählen Sie **VPN** aus. Bei VPN-Gateways wird ein virtuelles Netzwerkgateway vom Typ **VPN** verwendet. 
  - **VPN-Typ**: Wählen Sie den für Ihre Konfiguration angegebenen VPN-Typ aus. Bei den meisten Konfigurationen wird ein routenbasierter VPN-Typ benötigt.
  - **SKU**: Wählen Sie in der Dropdownliste die Gateway-SKU aus. Welche SKUs in der Dropdownliste aufgeführt werden, hängt vom ausgewählten VPN-Typ ab. Weitere Informationen zu Gateway-SKUs finden Sie unter [Gateway-SKUs](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku).
  - **Standort**: Unter Umständen müssen Sie einen Bildlauf durchführen, um diese Option anzuzeigen. Passen Sie das Feld **Standort** an, um auf den Standort zu verweisen, an dem sich das virtuelle Netzwerk befindet. Wenn der Standort nicht auf die Region verweist, in der sich Ihr virtuelles Netzwerk befindet, wird das virtuelle Netzwerk nicht in der Dropdownliste angezeigt, wenn Sie im nächsten Schritt ein virtuelles Netzwerk auswählen.
  - **Virtuelles Netzwerk**: Wählen Sie das virtuelle Netzwerk aus, dem Sie dieses Gateway hinzufügen möchten. Klicken Sie auf **Virtuelles Netzwerk**, um die Seite „Virtuelles Netzwerk auswählen“ zu öffnen. Wählen Sie das VNet aus. Sollte Ihr VNet nicht angezeigt werden, vergewissern Sie sich, dass das Feld „Speicherort“ auf die Region verweist, in der sich Ihr virtuelles Netzwerk befindet.
  - **Adressbereich für Gatewaysubnetz**: Diese Einstellung wird nur angezeigt, wenn Sie zuvor kein Gatewaysubnetz für Ihr virtuelles Netzwerk erstellt haben. Falls Sie zuvor ein gültiges Gatewaysubnetz erstellt haben, wird diese Einstellung nicht angezeigt.
  - **Erste IP-Konfiguration**: Auf der Seite „Öffentliche IP-Adresse wählen“ wird ein Objekt für eine öffentliche IP-Adresse erstellt, die dem VPN-Gateway zugeordnet wird. Die öffentliche IP-Adresse wird bei der Erstellung des VPN-Gateways diesem Objekt dynamisch zugewiesen. VPN Gateway unterstützt derzeit nur die *dynamische* Zuweisung öffentlicher IP-Adressen. Das bedeutet jedoch nicht, dass sich die IP-Adresse ändert, nachdem sie Ihrem VPN-Gateway zugewiesen wurde. Die öffentliche IP-Adresse ändert sich nur, wenn das Gateway gelöscht und neu erstellt wird. Sie ändert sich nicht, wenn die Größe geändert wird, das VPN-Gateway zurückgesetzt wird oder andere interne Wartungs-/Upgradevorgänge für das VPN-Gateway durchgeführt werden.

    - Klicken Sie zunächst auf **Gateway-IP-Konfiguration erstellen**, um die Seite „Öffentliche IP-Adresse wählen“ zu öffnen, und klicken Sie dann auf **+ Neu erstellen**, um die Seite „Öffentliche IP-Adresse erstellen“ zu öffnen.
    - Geben Sie anschließend einen **Namen** für die öffentliche IP-Adresse ein. Behalten Sie als SKU **Basic** bei, es sei denn, es gibt einen bestimmten Grund dafür, einen anderen Wert festzulegen. Klicken Sie dann unten auf dieser Seite auf **OK**, um die Änderungen zu speichern.

      ![Erstellen der öffentlichen IP-Adresse](./media/vpn-gateway-add-gw-rm-portal-include/gwip.png "Erstellen der PIP")

4. Überprüfen Sie die Einstellungen. Sie können unten auf der Seite auf **An Dashboard anheften** klicken, wenn das Gateway auf dem Dashboard angezeigt werden soll. 
5. Klicken Sie auf **Erstellen**, um das VPN-Gateway zu erstellen. Die Einstellungen werden überprüft, und auf dem Dashboard wird die Kachel mit dem Hinweis angezeigt, dass das Gateway des virtuellen Netzwerks bereitgestellt wird. Die Erstellung eines Gateways kann bis zu 45 Minuten dauern. Unter Umständen müssen Sie die Portalseite aktualisieren, um den Status „Abgeschlossen“ anzuzeigen.

Zeigen Sie nach der Erstellung des Gateways unter dem virtuellen Netzwerk im Portal die zugewiesene IP-Adresse an. Das Gateway wird als verbundenes Gerät angezeigt. Sie können auf das verbundene Gerät (Ihr virtuelles Netzwerkgateway) klicken, um weitere Informationen anzuzeigen.