---
title: Entwickeln von Apps für Azure Stack | Microsoft-Dokumentation
description: Überlegungen zur Entwicklung bei der Prototyperstellung von Anwendungen in Azure Stack
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: d3ebc6b1-0ffe-4d3e-ba4a-388239d6cdc3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: 3be22e7f8e69ded8ccc8956cc7fd7c6d71fe5fa1
ms.sourcegitcommit: 8b694bf803806b2f237494cd3b69f13751de9926
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/20/2018
ms.locfileid: "46497730"
---
# <a name="develop-for-azure-stack"></a>Entwickeln für Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Sie können noch heute mit der Entwicklung von Anwendungen loslegen, auch wenn Sie keinen Zugriff auf eine Azure Stack-Umgebung haben. Da Azure Stack Microsoft Azure-Dienste bereitstellt, die in Ihrem Datencenter ausgeführt werden, können Sie ähnliche Tools und Prozesse zum Entwickeln für Azure Stack verwenden wie für Azure. 

## <a name="development-considerations"></a>Überlegungen zur Entwicklung

Mit etwas Vorbereitung sowie den Informationen aus den folgenden Themen können Sie Azure zum Emulieren einer Azure Stack-Umgebung nutzen.

* In Azure haben Sie die Möglichkeit, Azure Resource Manager-Vorlagen zu erstellen, die in Azure Stack bereitgestellt werden können. In den [Überlegungen zu Vorlagen](azure-stack-develop-templates.md) finden Sie Anleitungen zum Entwickeln von Vorlagen, um die Portabilität zu gewährleisten.
* Zwischen Azure und Azure Stack gibt es Unterschiede in Bezug auf Dienstverfügbarkeit und Dienstversionsverwaltung. Mit dem [Azure Stack-Richtlinienmodul](azure-stack-policy-module.md) können Sie die Azure-Dienstverfügbarkeit und Azure-Ressourcentypen auf das Angebot von Azure Stack beschränken. Durch die Einschränkung von Diensten wird sichergestellt, dass Ihre Anwendung Dienste nutzt, die für Azure Stack verfügbar sind.
* Bei den [Azure Stack-Schnellstartvorlagen](https://github.com/Azure/AzureStack-QuickStart-Templates) handelt es sich um allgemeine Szenariobeispiele dazu, wie Sie Vorlagen entwickeln können, die in Azure und in Azure Stack bereitgestellt werden können.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure Stack-Entwicklung finden Sie in den folgenden Artikeln:

- [Bewährte Methoden für Azure Resource Manager-Vorlagen](azure-stack-develop-templates.md)
- [Azure Stack-Schnellstartvorlagen](https://github.com/Azure/AzureStack-QuickStart-Templates)