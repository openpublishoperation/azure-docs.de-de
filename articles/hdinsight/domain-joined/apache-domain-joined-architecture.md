---
title: Azure HDInsight-Architektur mit dem Enterprise-Sicherheitspaket
description: Erfahren Sie, wie Sie HDInsight-Sicherheitsmaßnahmen mit dem Enterprise-Sicherheitspaket planen können.
services: hdinsight
ms.service: hdinsight
author: omidm1
ms.author: omidm
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 975a4f7b15d1e1c13767cd7026e961e9d4227603
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46998923"
---
# <a name="use-enterprise-security-package-in-hdinsight"></a>Verwendung des Enterprise-Sicherheitspakets in HDInsight

Der standardmäßige Azure HDInsight-Cluster ist ein Einzelbenutzercluster. Dieser ist für die meisten Unternehmen geeignet, in denen kleinere Anwendungsteams große Datenworkloads erstellen. Jeder Benutzer kann bei Bedarf einen eigenen Cluster erstellen und ihn löschen, wenn er nicht mehr benötigt wird. 

Viele Unternehmen führten die Umstellung auf ein Modell durch, bei dem Cluster von IT-Teams verwaltet werden und mehrere Anwendungsteams Cluster gemeinsam nutzen. Diese größeren Unternehmen benötigen Zugriff auf den Cluster durch mehrere Benutzer in Azure HDInsight.

HDInsight basiert auf einem häufig verwendeten Identitätsanbieter – Active Directory – in einer verwalteten Methode. Durch die Integration von HDInsight in [Azure Active Directory Domain Services (Azure AD DS)](../../active-directory-domain-services/active-directory-ds-overview.md) können Sie mithilfe der Anmeldeinformationen für die Domäne auf die Cluster zugreifen. 

Die virtuellen Computer (VMs) in HDInsight werden in Ihre bereitgestellte Domäne eingebunden. Alle Dienste, die unter HDInsight ausgeführt werden (Ambari, Hive-Server, Ranger, Spark Thrift-Server usw.), funktionieren daher für den authentifizierten Benutzer nahtlos. Administratoren können dann mithilfe von Apache Ranger starke Autorisierungsrichtlinien erstellen, um eine rollenbasierte Zugriffssteuerung für Ressourcen im Cluster zu ermöglichen.


## <a name="integrate-hdinsight-with-active-directory"></a>Integrieren von HDInsight in Active Directory

Open-Source-Hadoop basiert auf Kerberos für die Bereitstellung von Authentifizierung und Sicherheit. Daher sind HDInsight-Clusterknoten mit dem Enterprise-Sicherheitspaket (ESP) in eine mit den Azure AD DS verwaltete Domäne eingebunden. Die Kerberos-Sicherheit ist für die Hadoop-Komponenten auf dem Cluster konfiguriert. 

Für jede Hadoop-Komponente wird automatisch ein Dienstprinzipal erstellt. Zudem wird ein entsprechender Computerprinzipal für jeden Computer erstellt, die in die Domäne eingebunden ist. Um diese Dienst- und Computerprinzipale zu speichern, muss eine Organisationseinheit (OE) in dem Domänencontroller (Azure AD DS) bereitgestellt werden, in dem diese Prinzipale platziert werden. 

Zusammengefasst benötigen Sie zum Einrichten einer Umgebung Folgendes:

- Eine (von Azure AD DS verwaltete) Active Directory-Domäne.
- In Azure AD DS aktiviertes sicheres LDAP (LDAPS).
- Ordnungsgemäße Konnektivität vom virtuellen HDInsight-Netzwerk zum virtuellen Azure AD DS-Netzwerk, wenn Sie separate virtuelle Netzwerke für sie wählen. Eine VM innerhalb des virtuellen HDInsight-Netzwerks sollte durch das Peering virtueller Netzwerke uneingeschränkten Zugriff auf Azure AD DS haben. Wenn HDInsight und Azure AD DS im gleichen virtuellen Netzwerk bereitgestellt sind, wird die Konnektivität automatisch bereitgestellt, und es ist keine weitere Aktion erforderlich.
- Eine [auf Azure AD DS erstellte](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md) OE.
- Ein Dienstkonto, dem folgende Berechtigungen zugewiesen sind:
    - Erstellen von Dienstprinzipalen in der OE.
    - Hinzufügen von Computern zur Domäne und Erstellen von Computerprinzipalen in der OE.

Der folgende Screenshot zeigt eine in contoso.com erstellte OE. Er zeigt auch einige der Dienst- und Computerprinzipale.

![Organisationseinheit für HDInsight-Cluster mit dem ESP](./media/apache-domain-joined-architecture/hdinsight-domain-joined-ou.png).

## <a name="set-up-different-domain-controllers"></a>Einrichten anderer Domänencontroller
HDInsight unterstützt derzeit nur Azure AD DS als Hauptdomänencontroller, den der Cluster für die Kerberos-Kommunikation verwendet. Aber auch andere komplexe Active Directory-Setups sind möglich, sofern ein solches Setup dazu führt, dass Azure AD DS für den HDInsight-Zugriff freigegeben wird.

### <a name="azure-active-directory-domain-services"></a>Azure Active Directory Domain Services
[Azure AD DS](../../active-directory-domain-services/active-directory-ds-overview.md) stellt eine verwaltete Domäne bereit, die vollständig mit Windows Server Active Directory kompatibel ist. Microsoft übernimmt in einem Hochverfügbarkeitssetup (Highly Available, HA) die Verwaltung, das Patchen und das Überwachen der Domäne. Sie können Ihren Cluster bereitstellen, ohne sich Sorgen um die Verwaltung von Domänencontrollern zu machen. 

Benutzer, Gruppen und Kennwörter werden von Azure Active Directory (Azure AD) synchronisiert. Die unidirektionale Synchronisierung aus Ihrem Azure AD-Instanz zu Azure AD DS gibt Benutzern die Möglichkeit, sich mit den gleichen Anmeldeinformationen des Unternehmens beim Cluster anzumelden. 

Weitere Informationen finden Sie unter [Konfigurieren eines HDInsight-Clusters mit dem Enterprise-Sicherheitspaket mit den Azure Active Directory Domain Services](./apache-domain-joined-configure-using-azure-adds.md).

### <a name="on-premises-active-directory-or-active-directory-on-iaas-vms"></a>Lokales Active Directory oder Active Directory auf IaaS-VMs

Wenn Sie eine lokale Active Directory-Instanz oder komplexere Active Directory-Setups für Ihre Domäne haben, können Sie diese Identitäten anhand von Azure AD Connect mit Azure synchronisieren. Anschließend können Sie Azure AD DS auf diesem Active Directory-Mandanten aktivieren. 

Da Kerberos auf Kennworthashes angewiesen ist, müssen Sie [die Kennworthashsynchronisierung auf Azure AD DS aktivieren](../../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md). Wenn Sie die Option „Verbund mit Active Directory Federation Services (AD FS)“ auswählen, können Sie optional die Kennworthashsynchronisierung als zusätzliche Sicherheit für den Fall aktivieren, dass Ihre AD FS-Infrastruktur ausfällt. Weitere Informationen finden Sie unter [Implementieren der Kennworthashsynchronisierung mit der Azure AD Connect-Synchronisierung](../../active-directory/hybrid/how-to-connect-password-hash-synchronization.md). 

Die alleinige Verwendung des lokalen Active Directory oder Active Directory auf IaaS-VMs ohne Azure AD und die Azure AD DS wird für HDInsight-Cluster mit dem ESP nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte

* [Konfigurieren eines HDInsight-Clusters mit dem Enterprise-Sicherheitspaket mithilfe der Azure Active Directory Domain Services](apache-domain-joined-configure-using-azure-adds.md)
* [Konfigurieren von Hive-Richtlinien in HDInsight mit dem Enterprise-Sicherheitspaket](apache-domain-joined-run-hive.md)
* [Verwalten von HDInsight-Clustern mit dem Enterprise-Sicherheitspaket](apache-domain-joined-manage.md) 
