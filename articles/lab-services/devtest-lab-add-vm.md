---
title: Hinzufügen eines virtuellen Computers zu einem Lab in Azure DevTest Labs | Microsoft-Dokumentation
description: Informationen zum Hinzufügen eines virtuellen Computers zu einem Lab in Azure DevTest Labs
services: devtest-lab,virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: ''
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2018
ms.author: spelluru
ms.openlocfilehash: ce95a2177260e97113fd5e639671075eb6ad40cd
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2018
ms.locfileid: "39215015"
---
# <a name="add-a-vm-to-a-lab-in-azure-devtest-labs"></a>Hinzufügen eines virtuellen Computers zu einem Lab in Azure DevTest Labs
Wenn Sie bereits [Ihren ersten virtuellen Computer erstellt haben](devtest-lab-create-first-vm.md), haben Sie dazu wahrscheinlich ein vorkonfiguriertes [Marketplace-Image](devtest-lab-configure-marketplace-images.md) verwendet. Wenn Sie Ihrem Lab nun weitere virtuelle Computer hinzufügen möchten, können Sie auch eine *Basis* auswählen, die entweder ein [benutzerdefiniertes Image](devtest-lab-create-template.md) oder eine [Formel](devtest-lab-manage-formulas.md) ist. Dieses Tutorial führt Sie durch die Verwendung des Azure-Portals zum Hinzufügen eines virtuellen Computers zu einem Lab in DevTest Labs.

In diesem Artikel wird außerdem die Verwaltung der Artefakte für einen virtuellen Computer in Ihrem Lab veranschaulicht.

## <a name="steps-to-add-a-vm-to-a-lab-in-azure-devtest-labs"></a>Schritte zum Hinzufügen eines virtuellen Computers zu einem Lab in Azure DevTest Labs
1. Melden Sie sich auf dem [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)an.
1. Wählen Sie **Alle Dienste** und dann in der Liste die Option **DevTest Labs**.
1. Wählen Sie in der Liste der Labs das Lab aus, in dem Sie den neuen virtuellen Computer (VM, Virtual Machine) erstellen möchten.  
1. Wählen Sie im Bereich **Übersicht** des Labs die Option **+Hinzufügen** aus.  

    ![Schaltfläche zum Hinzufügen eines virtuellen Computers](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. Wählen Sie im Bereich **Basisdesign auswählen** eine Basis für den virtuellen Computer aus.
1. Im Bereich **Virtueller Computer** wird der **Name des virtuellen Computers** für Sie mit einem eindeutigen, automatisch generierten Namen vorbelegt. Der Name entspricht dem Benutzernamen in Ihrer E-Mail-Adresse, gefolgt von einer eindeutigen dreistelligen Nummer. Diese Funktion erspart Ihnen die Zeit, sich einen Computernamen auszudenken und ihn jedes Mal einzugeben, wenn Sie einen Computer erstellen. Wenn Sie wünschen, können Sie dieses automatisch ausgefüllte Feld mit einem selbst gewählten Namen überschreiben. Um den automatisch ausgefüllten Namen für die VM zu überschreiben, geben Sie einen Namen in das Textfeld **Name des virtuellen Computers** ein. 

    ![Lab-VM-Bereich](./media/devtest-lab-add-vm/devtestlab-lab-vm-blade.png)

1. Der **Benutzername** für den Computer ist mit einem eindeutigen, automatisch generierten Namen vorbelegt. Der Name entspricht dem Benutzernamen in Ihrer E-Mail-Adresse. Diese Funktion erspart Ihnen die Zeit, sich jedes Mal für einen Benutzernamen zu entscheiden, wenn Sie einen neuen Computer erstellen. Auch hier können Sie dieses automatisch ausgefüllte Feld mit einem Benutzernamen Ihrer Wahl überschreiben. Um den automatisch ausgefüllten Wert für den Benutzernamen zu überschreiben, geben Sie einen Wert in das Textfeld **Benutzername** ein. Dieser Benutzer erhält auf dem virtuellen Computer **Administratorrechte**.     
1. Für das **Kennwort**:
    
    Wenn Sie die erste VM im Lab erstellen, geben Sie ein Kennwort in das Textfeld **Geben Sie einen Wert ein** ein. Um dieses Kennwort als Standardkennwort im Azure-Schlüsseltresor des Labs zu speichern, wählen Sie **Als Standardkennwort speichern** aus. Das Standardkennwort wird im Schlüsseltresor mit dem Namen **VmPassword** gespeichert. Wenn Sie versuchen, nachfolgende VMs im Lab zu erstellen, wird **VmPassword** automatisch für das **Kennwort** ausgewählt. Um den Wert zu überschreiben, deaktivieren Sie das Kontrollkästchen **Gespeichertes Geheimnis verwenden** und geben ein Kennwort ein. 

    Alternativ können Sie Geheimnisse auch zuerst im Schlüsseltresor speichern und anschließend beim Erstellen einer VM im Lab verwenden. Weitere Informationen finden Sie unter [Speichern eines Geheimnisses in Azure Key Vault](devtest-lab-store-secrets-in-key-vault.md). Zur Verwendung eines Kennworts, das in einem Schlüsseltresor hinterlegt ist, wählen Sie **Gespeichertes Geheimnis verwenden**, und geben Sie einen Schlüsselwert an, der Ihrem Geheimnis (Kennwort) entspricht. 
3. Der **Datenträgertyp des virtuellen Computers** bestimmt, welcher Typ von Speicherdatenträgern für die virtuellen Computer im Lab zulässig ist.
4. Wählen Sie **Größe des virtuellen Computers** , und wählen Sie eines der vordefinierten Elemente aus, die die Prozessorkerne, die RAM-Größe und die Größe der Festplatte für den zu erstellenden virtuellen Computer angeben.
5. Wählen Sie **Artefakte** , und wählen Sie aus der Liste der Artefakte die Artefakte aus, die Sie dem Basisimage hinzufügen möchten, und konfigurieren Sie sie.
    **Hinweis:** Wenn Sie noch nicht mit DevTest Labs oder dem Konfigurieren von Artefakten vertraut sind, lesen Sie den Abschnitt [Hinzufügen eines vorhandenen Artefakts zu einer VM](#add-an-existing-artifact-to-a-vm), und kehren Sie dann hierher zurück.
6. Wählen Sie **Erweiterte Einstellungen**, um die Netzwerkoptionen und Ablaufoptionen des virtuellen Computers zu konfigurieren. 

   Wählen Sie zum Festlegen einer Ablaufoption das Symbol „Kalender“ aus, um ein Datum anzugeben, an dem der virtuelle Computer automatisch gelöscht wird.  Standardmäßig läuft der virtuelle Computer nie ab. 
1. Wenn Sie die Azure Resource Manager-Vorlage anzeigen oder kopieren möchten, lesen Sie den Abschnitt [Speichern der Azure Resource Manager-Vorlage](#save-azure-resource-manager-template), und kehren Sie anschließend hierher zurück.
1. Wählen Sie **Erstellen** , um den angegebene virtuellen Computer dem Lab hinzuzufügen.
1. Im Bereich für das Lab wird der Status der VM-Erstellung angezeigt: erst als **Wird erstellt**, dann als **Wird ausgeführt**, nachdem die VM gestartet wurde.

> [!NOTE]
> In [Hinzufügen eines anforderbaren virtuellen Computers](devtest-lab-add-claimable-vm.md) wird erläutert, wie Sie den virtuellen Computer als anforderbar festlegen, sodass er für jeden Benutzer im Lab zur Verwendung verfügbar ist.
>
>

## <a name="add-an-existing-artifact-to-a-vm"></a>Hinzufügen eines vorhandenen Artefakts zu einer VM
Beim Erstellen eines virtuellen Computers können Sie vorhandene Artefakte hinzufügen. Jedes Lab enthält Artefakte aus dem öffentlichen DevTest Labs-Artefaktrepository, sowie Artefakte, die Sie erstellt und Ihrem eigenen Artefaktrepository hinzugefügt haben.

* Azure DevTest Labs-*Artefakte* ermöglichen Ihnen die Angabe von *Aktionen*, die ausgeführt werden, wenn der virtuelle Computer bereitgestellt wird, beispielsweise Ausführen von Windows PowerShell-Skripts, Ausführen von Bash-Befehlen und Installieren von Software.
* Mit *Parametern* für Artefakte können Sie das Artefakt für ein bestimmtes Szenario anpassen.

Informationen zum Erstellen von Artefakten finden Sie im Artikel [Erstellen von benutzerdefinierten Artefakten für Ihre DevTest Labs-VM](devtest-lab-artifact-author.md).

1. Melden Sie sich auf dem [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)an.
1. Wählen Sie **Alle Dienste** und dann in der Liste die Option **DevTest Labs**.
1. Wählen Sie in der Liste der Labs das Lab mit dem virtuellen Computer aus, mit dem Sie arbeiten möchten.  
1. Wählen Sie **Meine virtuellen Computer** aus.
1. Wählen Sie den gewünschten virtuellen Computer aus.
1. Wählen Sie **Artefakte verwalten** aus. 
1. Wählen Sie **Artefakte anwenden**.
1. Wählen Sie im Bereich **Artefakte anwenden** das Artefakt aus, das Sie dem virtuellen Computer hinzufügen möchten.
1. Geben Sie im Bereich **Artefakt hinzufügen** die erforderlichen Parameterwerte und alle optionalen Parameter ein, die Sie benötigen.  
1. Wählen Sie **Hinzufügen** aus, um das Artefakt hinzuzufügen, und kehren Sie zum Bereich **Artefakte anwenden** zurück.
1. Fügen Sie weiterhin nach Bedarf für Ihren virtuellen Computer Artefakte hinzu.
1. Sobald Sie Ihre Artefakte hinzugefügt haben, können Sie [die Reihenfolge ändern, in der die Artefakte ausgeführt werden](#change-the-order-in-which-artifacts-are-run). Sie können auch zum [Anzeigen oder Ändern eines Artefakts](#view-or-modify-an-artifact)zurückgehen.
1. Wenn Sie mit dem Hinzufügen von Artefakten fertig sind, wählen Sie **Anwenden**.

## <a name="change-the-order-in-which-artifacts-are-run"></a>Ändern der Reihenfolge, in der Artefakte ausgeführt werden
Standardmäßig werden die Aktionen der Artefakte in der Reihenfolge ausgeführt, in der sie der VM hinzugefügt wurden. Die folgenden Schritte veranschaulichen, wie Sie die Reihenfolge ändern, in der die Artefakte ausgeführt werden.

1. Wählen Sie oben im Bereich **Artefakte anwenden** den Link, der die Anzahl der Artefakte angibt, die dem virtuellen Computer hinzugefügt wurden.
   
    ![Anzahl der Artefakte, die dem virtuellen Computer hinzugefügt wurden](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)
1. Ziehen Sie die Artefakte im Bereich **Ausgewählte Artefakte** per Drag & Drop in die gewünschte Reihenfolge. **Hinweis:** Wenn beim Ziehen eines Artefakts Probleme auftreten, stellen Sie sicher, dass Sie von der linken Seite des Artefakts aus ziehen. 
1. Wählen Sie nach Abschluss des Vorgangs **OK** .  

## <a name="view-or-modify-an-artifact"></a>Anzeigen oder Ändern eines Artefakts
Die folgenden Schritte veranschaulichen das Anzeigen oder Ändern der Parameter eines Artefakts:

1. Wählen Sie oben im Bereich **Artefakte anwenden** den Link, der die Anzahl der Artefakte angibt, die dem virtuellen Computer hinzugefügt wurden.
   
    ![Anzahl der Artefakte, die dem virtuellen Computer hinzugefügt wurden](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)
1. Wählen Sie im Bereich **Ausgewählte Artefakte** das Artefakt aus, das Sie anzeigen oder bearbeiten möchten.  
1. Nehmen Sie im Bereich **Artefakt hinzufügen** die erforderlichen Änderungen vor, und wählen Sie **OK**, um den Bereich **Artefakt hinzufügen** zu schließen.
1. Wählen Sie **OK**, um den Bereich **Ausgewählte Artefakte** zu schließen.

## <a name="save-azure-resource-manager-template"></a>Speichern der Azure Resource Manager-Vorlage
Eine Azure Resource Manager-Vorlage bietet eine deklarative Möglichkeit zum Definieren einer wiederholbaren Bereitstellung. Die folgenden Schritte erläutern, wie die Azure Resource Manager-Vorlage für den zu erstellenden virtuellen Computer gespeichert wird.
Nach dem Speichern können Sie die Azure Resource Manager-Vorlage zum [Bereitstellen neuer virtueller Computer mit Azure PowerShell](../azure-resource-manager/resource-group-overview.md#template-deployment) nutzen.

1. Wählen Sie im Bereich **Virtueller Computer** die Option **Azure Resource Manager-Vorlage anzeigen** aus.
2. Wählen Sie im Bereich **Azure Resource Manager-Vorlage anzeigen** den Text der Vorlage aus.
3. Kopieren Sie den markierten Text in die Zwischenablage.
4. Wählen Sie **OK** aus, um den Bereich **Azure Resource Manager-Vorlage anzeigen** zu schließen.
5. Öffnen Sie einen Text-Editor.
6. Fügen Sie den Text der Vorlage aus der Zwischenablage ein.
7. Speichern Sie die Datei für eine spätere Verwendung.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Nächste Schritte
* Nach der Erstellung des virtuellen Computers können Sie im Bereich des virtuellen Computers die Option**Verbinden** wählen, um eine Verbindung mit dem virtuellen Computer herzustellen.
* Informieren Sie sich über das [Erstellen von benutzerdefinierten Artefakten für Ihre DevTest Lab-VM](devtest-lab-artifact-author.md).
* Erkunden Sie den [DevTest Labs-Azure Resource Manager-Katalog mit Schnellstartvorlagen](https://github.com/Azure/azure-devtestlab/tree/master/Samples).
