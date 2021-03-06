---
title: Bereitstellen von Azure File Storage auf Linux-VMs per SMB | Microsoft-Dokumentation
description: Bereitstellen von Azure File Storage auf virtuellen Linux-Computern per SMB mithilfe der Azure-Befehlszeilenschnittstelle
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: cynthn
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/28/2018
ms.author: cynthn
ms.openlocfilehash: 7cd7f0f37f0d351d1d50d4c15e7132f072b5125d
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46982204"
---
# <a name="mount-azure-file-storage-on-linux-vms-using-smb"></a>Bereitstellen von Azure File Storage auf Linux-VMs per SMB

In diesem Artikel wird beschrieben, wie Sie den Azure File Storage-Dienst auf einem virtuellen Linux-Computer über eine SMB-Bereitstellung mit der Azure-Befehlszeilenschnittstelle verwenden. Der Azure-Dateispeicher verfügt über Dateifreigaben in der Cloud unter Verwendung des standardmäßigen SMB-Protokolls. 

File Storage bietet Dateifreigaben in der Cloud, die das standardmäßige SMB-Protokoll verwenden. Sie können eine Dateifreigabe aus einem beliebigen Betriebssystem einbinden, das SMB 3.0 unterstützt. Wenn Sie eine SMB-Bereitstellung unter Linux verwenden, erhalten Sie einfache Sicherungen an einem robusten, dauerhaften Speicherort für die Archivierung, der durch eine Vereinbarung zum Servicelevel (SLA) unterstützt wird.

Das Verschieben von Dateien von einem virtuellen Computer auf eine in File Storage gehostete SMB-Bereitstellung ist eine gute Möglichkeit zum Debuggen von Protokollen. Dieselbe SMB-Freigabe kann lokal auf Ihrer Mac-, Linux- oder Windows-Arbeitsstation bereitgestellt werden. SMB ist nicht die ideale Lösung zum Streamen von Linux- oder Anwendungsprotokollen in Echtzeit, da das SMB-Protokoll nicht auf so anspruchsvolle Protokollierungsaufgaben ausgelegt ist. Ein dediziertes Tool wie Fluentd mit vereinheitlichter Protokollierungsebene ist eine bessere Wahl als SMB, wenn es um das Erfassen der Linux- und Anwendungsprotokollausgabe geht.

Für diese Führungslinie müssen Sie mindestens Version 2.0.4 der Azure-Befehlszeilenschnittstelle ausführen. Führen Sie **az --version** aus, um die Version zu ermitteln. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](/cli/azure/install-azure-cli). 


## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *USA, Osten*.

```bash
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-storage-account"></a>Speicherkonto erstellen

Erstellen Sie mithilfe des Befehls [az storage account create](/cli/azure/storage/account#create) ein neues Speicherkonto in der von Ihnen erstellten Ressourcengruppe. In diesem Beispiel wird ein Speicherkonto mit dem Namen *mySTORAGEACCT<random number>* erstellt und der Name dieses Speicherkontos in die Variable **STORAGEACCT** eingefügt. Da Speicherkontonamen eindeutig sein müssen, wird durch Verwendung von `$RANDOM` eine Zahl an das Ende des Namens angefügt.

```bash
STORAGEACCT=$(az storage account create \
    --resource-group "myResourceGroup" \
    --name "mystorageacct$RANDOM" \
    --location eastus \
    --sku Standard_LRS \
    --query "name" | tr -d '"')
```

## <a name="get-the-storage-key"></a>Speicherschlüssel abrufen

Beim Erstellen eines Speicherkontos werden die Speicherkontoschlüssel als Paar erstellt, sodass eine Rotation der Schlüssel ohne Unterbrechung des Diensts möglich ist. Wenn Sie zum zweiten Schlüssel des Paars wechseln, erstellen Sie ein neues Schlüsselpaar. Neue Speicherkontoschlüssel werden stets als Paar erstellt, damit sichergestellt ist, dass immer mindestens ein ungenutzter Speicherkontoschlüssel vorhanden ist, zu dem gewechselt werden kann.

Verwenden Sie [az storage account keys list](/cli/azure/storage/account/keys#list), um die Speicherkontoschlüssel anzuzeigen. In diesem Beispiel wird der Wert des Schlüssels 1 in der Variable **STORAGEKEY** gespeichert.

```bash
STORAGEKEY=$(az storage account keys list \
    --resource-group "myResourceGroup" \
    --account-name $STORAGEACCT \
    --query "[0].value" | tr -d '"')
```

## <a name="create-a-file-share"></a>Erstellen einer Dateifreigabe

Erstellen Sie die Dateispeicherfreigabe mithilfe des Befehls [az storage share create](/cli/azure/storage/share#create). 

Freigabenamen dürfen nur Kleinbuchstaben, Zahlen und einzelne Bindestriche enthalten und dürfen nicht mit einem Bindestrich beginnen. Ausführliche Informationen zur Benennung von Dateifreigaben und Dateien finden Sie unter [Benennen und Referenzieren von Freigaben, Verzeichnissen, Dateien und Metadaten](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Shares--Directories--Files--and-Metadata).

In diesem Beispiel wird eine Freigabe mit dem Namen *myshare* mit einem Kontingent von 10 GiB erstellt. 

```bash
az storage share create --name myshare \
    --quota 10 \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY
```

## <a name="create-a-mount-point"></a>Erstellen eines Bereitstellungspunkts

Wenn Sie die Azure-Dateifreigabe auf Ihrem Linux-Computer bereitstellen möchten, müssen Sie sicherstellen, dass Sie das Paket **cifs-utils** installiert haben. Installationsanweisungen finden Sie unter [Installieren des Pakets „cifs-utils“ für Ihre Linux-Verteilung](../../storage/files/storage-how-to-use-files-linux.md#install-cifs-utils).

Azure Files verwendet ein SMB-Protokoll, das über TCP-Port 445 kommuniziert.  Wenn Sie Probleme bei der Bereitstellung Ihrer Azure-Dateifreigabe haben, sollten Sie sicherstellen, dass Ihre Firewall TCP-Port 445 nicht blockiert.


```bash
mkdir -p /mnt/MyAzureFileShare
```

## <a name="mount-the-share"></a>Bereitstellen der Freigabe

Stellen Sie die Azure-Dateifreigabe für das lokale Verzeichnis bereit. 

```bash
sudo mount -t cifs //$STORAGEACCT.file.core.windows.net/myshare /mnt/MyAzureFileShare -o vers=3.0,username=$STORAGEACCT,password=$STORAGEKEY,dir_mode=0777,file_mode=0777,serverino
```



## <a name="persist-the-mount"></a>Beibehalten der Bereitstellung

Beim Neustart eines virtuellen Linux-Computers wird die Bereitstellung der SMB-Freigabe während des Herunterfahrens aufgehoben. Fügen Sie der Linux-Datei „etc/fstab“ eine Zeile hinzu, damit die SMB-Freigabe beim Starten erneut bereitgestellt wird. Linux verwendet die fstab-Datei, um die Dateisysteme aufzulisten, die während des Startvorgangs bereitgestellt werden müssen. Durch Hinzufügen der SMB-Freigabe wird sichergestellt, dass die File Storage-Freigabe für den virtuellen Linux-Computer ein dauerhaft bereitgestelltes Dateisystem ist. Das Hinzufügen der File Storage-SMB-Freigabe zu einem neuen virtuellen Computer ist möglich, wenn Sie „cloud-init“ verwenden.

```bash
//myaccountname.file.core.windows.net/mystorageshare /mnt/mymountpoint cifs vers=3.0,username=mystorageaccount,password=myStorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```
Zur Erhöhung der Sicherheit in Produktionsumgebungen sollten Sie Ihre Anmeldeinformationen außerhalb der fstab-Datei speichern.

## <a name="next-steps"></a>Nächste Schritte

- [Verwenden von Cloud-Init zum Anpassen einer Linux-VM während der Erstellung](using-cloud-init.md)
- [Hinzufügen eines Datenträgers zu einem virtuellen Linux-Computer](add-disk.md)
- [Verschlüsseln von Datenträgern auf einem virtuellen Linux-Computer mithilfe der Azure-Befehlszeilenschnittstelle](encrypt-disks.md)

