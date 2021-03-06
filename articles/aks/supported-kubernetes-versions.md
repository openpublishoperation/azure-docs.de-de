---
title: Unterstützte Kubernetes-Versionen in Azure Kubernetes Service
description: Grundlegendes zur Richtlinie zur Unterstützung der Kubernetes-Version und zum Lebenszyklus von Clustern in Azure Kubernetes Service (AKS)
services: container-service
author: sauryadas
ms.service: container-service
ms.topic: article
ms.date: 09/21/2018
ms.author: saudas
ms.openlocfilehash: 6b55825107ae8872b146b3ad4fde0ef4b917b71d
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47046234"
---
# <a name="supported-kubernetes-versions-in-azure-kubernetes-service-aks"></a>Unterstützte Kubernetes-Versionen in Azure Kubernetes Service (AKS)

Die Kubernetes-Community veröffentlicht etwa alle drei Monate Nebenversionen. Diese Releases enthalten neue Features und Verbesserungen. Patchreleases kommen häufiger vor (manchmal wöchentlich) und sind nur für wichtige Fehlerbehebungen in einer Nebenversion gedacht. Diese Patchreleases beinhalten Behebungen von Sicherheitsrisiken oder größerer Fehler, die viele Kunden und Produkte betreffen, die in einer auf Kubernetes basierenden Produktionsumgebung ausgeführt werden.

Eine neue Nebenversion von Kubernetes wird am ersten Tag in der [ACS-Engine][acs-engine] zur Verfügung gestellt. Das AKS-Servicelevelziel (Service Level Objective, SLO) ist darauf ausgerichtet, die Nebenversion für AKS-Cluster innerhalb von 30 Tagen zu veröffentlichen, vorbehaltlich der Stabilität des Releases.

## <a name="kubernetes-version-support-policy"></a>Richtlinie zur Unterstützung der Kubernetes-Version

AKS unterstützt vier Nebenversionen von Kubernetes:

- Die aktuelle Nebenversion, die upstream veröffentlicht wird (n).
- Die drei vorherigen Nebenversionen. Jede unterstützte Nebenversion unterstützt ebenfalls zwei stabile Patches.

Wenn AKS zum Beispiel heute *1.11.x* einführt, werden auch *1.10.a* + *1.10.b*, *1.9.c* + *1.9d* und *1.8.e* + *1.8f* unterstützt (wobei die Buchstaben der Patchreleases für die beiden letzten stabilen Builds stehen).

Wenn eine neue Nebenversion eingeführt wird, werden die älteste Nebenversion und die unterstützten Patchreleases deaktiviert. 15 Tage vor der Veröffentlichung der neuen Nebenversion und der bevorstehenden Deaktivierung der Version gibt es eine Ankündigung über die Updatekanäle von Azure. Im Beispiel oben wird *1.11.x* veröffentlicht, und die Versionen *1.7.g* + *1.7.h* werden deaktiviert.

Wenn Sie einen AKS-Cluster im Portal oder mit der Azure CLI bereitstellen, wird der Cluster immer auf die Nebenversion n-1 und den neuesten Patch festgelegt. Wenn AKS zum Beispiel *1.11.x*, *1.10.a* + *1.10.b*, *1.9.c* + *1.9d* und *1.8.e* + *1.8f* unterstützt, ist die Standardversion des neuen Clusters *1.10.b*.

## <a name="faq"></a>Häufig gestellte Fragen

**Was geschieht, wenn ein Kunde ein Upgrade eines Kubernetes-Clusters mit einer Nebenversion durchführt, die nicht unterstützt wird?**

Wenn Sie die Version *n-4* verwenden, befinden Sie sich außerhalb des SLO. Wenn Ihr Upgrade von Version n-4 auf n-3 erfolgreich ist, befinden Sie sich wieder innerhalb des SLO. Beispiel: 

- Wenn die AKS-Versionen *1.10.a* + *1.10.b*, *1.9.c* + *1.9d* und *1.8.e* + *1.8f* unterstützt werden und Sie *1.7.g* oder *1.7.h* verwenden, befinden Sie sich außerhalb des SLO.
- Wenn das Upgrade von *1.7.g* oder *1.7.h* auf *1.8.e* oder *1.8.f* erfolgreich ist, befinden Sie sich wieder innerhalb des SLO.

Upgrades auf ältere Versionen als *n-4* werden nicht unterstützt. In solchen Fällen wird empfohlen, dass die Kunden neue AKS-Cluster erstellen und ihre Workloads erneut bereitstellen.

**Was geschieht, wenn ein Kunde einen Kubernetes-Cluster mit einer Nebenversion skaliert, die nicht unterstützt wird?**

Bei Nebenversionen, die von AKS nicht unterstützt werden, funktioniert das Herunter- und Hochskalieren weiterhin einwandfrei.

**Kann ein Kunde für immer eine Kubernetes-Version verwenden?**

Ja. Wenn der Cluster jedoch keine der von AKS unterstützten Versionen verwendet, befindet sich der Cluster außerhalb des AKS SLO. Azure führt kein automatisches Upgrade Ihres Clusters durch und löscht ihn nicht.

**Welche Version wird vom Master unterstützt, wenn der Agent-Cluster keine der unterstützten AKS-Versionen verwendet?**

Der Master wird automatisch auf die neueste unterstützte Version aktualisiert.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Upgrade Ihres Clusters finden Sie unter [Durchführen eines Upgrades für einen Azure Kubernetes Service-Cluster (AKS)][aks-upgrade].

<!-- LINKS - External -->
[acs-engine]: https://github.com/Azure/acs-engine

<!-- LINKS - Internal -->
[aks-upgrade]: upgrade-cluster.md