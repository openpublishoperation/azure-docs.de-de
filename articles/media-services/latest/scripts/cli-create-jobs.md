---
title: 'Azure CLI-Skriptbeispiel: Erstellen und Übermitteln eines Auftrags | Microsoft-Dokumentation'
description: Das Azure CLI-Skript in diesem Thema zeigt, wie Sie einen Auftrag mithilfe einer HTTPS-URL an eine einfache Codierungstransformation übermitteln.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: ''
ms.service: media-services
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/16/2018
ms.author: juliako
ms.openlocfilehash: 3338fa32810b74deaf576d3eb966d5cac09eac1a
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49376327"
---
# <a name="cli-example-create-and-submit-a-job"></a>CLI-Beispiel: Erstellen und Übermitteln eines Auftrags

Das Azure CLI-Skript in diesem Artikel zeigt, wie Sie einen Auftrag erstellen und mithilfe einer HTTPS-URL an eine einfache Codierungstransformation übermitteln.

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittstelle lokal installieren und verwenden möchten, müssen Sie für diesen Artikel mindestens die Azure CLI-Version 2.0.20 verwenden. Führen Sie `az --version` aus, um die Version zu finden. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](/cli/azure/install-azure-cli). 

## <a name="example-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../../cli_scripts/media-services/create-jobs/Create-Jobs.sh "Create and submit jobs")]

## <a name="next-steps"></a>Nächste Schritte

Weitere Beispiele finden Sie unter [Azure CLI-Beispiele](../cli-samples.md).
