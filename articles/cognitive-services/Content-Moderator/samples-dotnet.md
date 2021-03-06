---
title: Codebeispiele – Content Moderator, .NET
description: Verwenden von Content Moderator in Ihren Anwendungen
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: sample
ms.date: 01/10/2018
ms.author: sajagtap
ms.openlocfilehash: c5465a2de0924037eb93b9de652e93e85f166737
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47223150"
---
# <a name="net-sdk-samples"></a>.NET SDK-Beispiele

Die folgende Liste enthält Links zu den Codebeispielen, die mit dem Azure Content Moderator SDK für .NET erstellt wurden.

- **Unterstützende Bibliothek**: [Erstellen Sie einen Content Moderator-Client für die Verwendung in anderen Beispielen](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ModeratorHelper/Clients.cs). Weitere Informationen finden Sie im [Schnellstart](content-moderator-helper-quickstart-dotnet.md).

## <a name="moderation"></a>Moderation

- **Bildmoderation**: [Werten Sie Bilder für nicht jugendfreie Inhalte, Texte und Gesichter aus](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageModeration/Program.cs). Weitere Informationen finden Sie im [Schnellstart](image-moderation-quickstart-dotnet.md).
- **Benutzerdefinierte Bilder**: [Moderieren Sie Bilder mit benutzerdefinierten Bildlisten](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageListManagement/Program.cs). Weitere Informationen finden Sie im [Schnellstart](image-lists-quickstart-dotnet.md).

> [!NOTE]
> Die Obergrenze liegt bei **fünf Bildlisten**, wobei jede Liste **max. 10.000 Bilder** enthalten kann.
>

- **Textmoderation**: [Prüfen Sie Texte auf unerwünschte Ausdrücke und personenbezogene Informationen (Personally Identifiable Information, PII)](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TextModeration/Program.cs). Weitere Informationen finden Sie im [Schnellstart](text-moderation-quickstart-dotnet.md).
- **Benutzerdefinierte Benennungen**: [Moderieren Sie Benennungen mit benutzerdefinierten Benennungslisten](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TermListManagement/Program.cs). Weitere Informationen finden Sie im [Schnellstart](term-lists-quickstart-dotnet.md).

> [!NOTE]
> Die Obergrenze liegt bei **fünf Benennungslisten**, wobei jede Liste **max. 10.000 Benennungen** enthalten kann.
>

- **Videomoderation**: [Prüfen Sie ein Video auf nicht jugendfreie Inhalte, und rufen Sie die Ergebnisse ab](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoModeration/Program.cs). Weitere Informationen finden Sie im [Schnellstart](video-moderation-api.md).

## <a name="review"></a>Überprüfung

- **Bildaufträge**: [Starten Sie einen Moderationsauftrag, der Überprüfungen durchführt und erstellt](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageJobs/Program.cs). Weitere Informationen finden Sie im [Schnellstart](moderation-jobs-quickstart-dotnet.md).
- **Bildüberprüfungen**: [Erstellen Sie Überprüfungen für HITL-Simulationen (Human-in-the-Loop)](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageReviews/Program.cs). Weitere Informationen finden Sie im [Schnellstart](moderation-reviews-quickstart-dotnet.md).
- **Videoüberprüfungen**: [Erstellen Sie Videoüberprüfungen für HITL-Simulationen (Human-in-the-Loop)](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoReviews/Program.cs). Weitere Informationen finden Sie im [Schnellstart](video-reviews-quickstart-dotnet.md).
- **Überprüfungen von Videotranskripts**: [Erstellen Sie Überprüfungen von Videotranskripts für HITL-Simulationen (Human-in-the-Loop).](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoTranscriptReviews/Program.cs) Weitere Informationen finden Sie im [Schnellstart](video-reviews-quickstart-dotnet.md).

Alle .NET-Beispiele finden Sie auf [GitHub unter den .NET-Beispielen zu Content Moderator](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator).
