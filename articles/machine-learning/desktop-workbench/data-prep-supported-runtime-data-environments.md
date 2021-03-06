---
title: Unterstützte Kombinationen der Ausführungs- und Datenumgebung für Azure Machine Learning-Datenvorbereitungen | Microsoft-Dokumentation
description: Dieses Dokument enthält eine vollständige Liste der unterstützten Kombinationen von unterschiedlichen Laufzeiten und Datenquellen für Azure Machine Learning-Datenvorbereitungen.
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/01/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 9168ac1d26432ca3eee5a59b63aa0cec3ae72856
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46989055"
---
# <a name="supported-matrix-for-this-release"></a>Unterstützte Matrix für diese Version 

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 


Wenn Ihr Code Daten mithilfe von Azure Machine Learning-Datenquellen oder Azure Machine Learning-Datenvorbereitungen lädt, bei denen ein Pandas- oder Spark-Datenframe abgerufen wird, werden die folgenden Kombinationen von Compute-Umgebungen für Experimente und Datenspeicherorten unterstützt:

|     |Lokale Dateien  |Azure Blob Storage  |SQL Server-Datenbank***  |
|---------|---------|---------|---------|---------|
|Lokales Python    |     Unterstützt    |Nicht unterstützt         | Nicht unterstützt        |         |
|Docker (Linux-VM) Python     |Nur in Projektdateien unterstützt*         | Nicht unterstützt        |        Nicht unterstützt |         |
|Docker (Linux-VM) PySpark     |Nur in Projektdateien unterstützt*     |Unterstützt         | Unterstützt \*\*        |         |
|Azure Data Science Virtual Machine Python     |Nur in Projektdateien unterstützt*         |Nicht unterstützt         |Nicht unterstützt         |         |
|Azure Data Science Virtual Machine PySpark     | Nur in Projektdateien unterstützt*        |Nicht unterstützt         |Nicht unterstützt         |         |
|Azure HDInsight PySpark     | Nicht unterstützt        |Unterstützt         |Unterstützt \*\*         |         |
|Azure HDInsight Python     | Nicht unterstützt        | Nicht unterstützt        | Nicht unterstützt        |         |

Azure Data Lake Store wird derzeit nicht für alle Computeziele unterstützt.

*Wenn lokale Dateipfade verwendet werden, werden Dateien innerhalb des Projekts in die Compute-Umgebung kopiert und dann dort gelesen. Dateien außerhalb des Projekts werden nicht kopiert und Pfade in der Compute-Umgebung werden nicht mehr aufgelöst. Erwägen Sie die Verwendung der Datenquellenersetzung, damit Ihr Code eine lokale Datei verwenden kann, wenn er lokal ausgeführt wird. Wechseln Sie dann zu einem Azure Storage-Blob für eine andere Laufzeitkonfiguration. Sie können auch die Unterstützung für Stichprobenentnahmen für Datenquellen verwenden, um umfangreiche Daten nur in bestimmten Laufzeitkonfigurationen zu verwalten.

\*\* Verwendet den Maven JDBC SQL Server-Treiber 6.2.1. Sie müssen sicherstellen, dass dieses Paket (oder ein kompatibles Paket) in Ihrer Datei „spark_dependencies.yml“ für die Compute-Umgebung enthalten ist.

***Unterstützt Azure SQL-Datenbank oder SQL Server, sofern die Datenbank über die Compute-Umgebung erreicht werden kann. 
