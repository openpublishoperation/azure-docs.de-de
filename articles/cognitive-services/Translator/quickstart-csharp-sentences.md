---
title: 'Schnellstart: Abrufen von Satzlängen – Textübersetzungs-API, C#'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung ermitteln Sie mit der Textübersetzungs-API und C# die Länge von Sätzen im Text.
services: cognitive-services
author: noellelacharite
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/15/2018
ms.author: nolachar
ms.openlocfilehash: 699c4b775f2c5745a5b52593d44a0af408c5e73a
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49319604"
---
# <a name="quickstart-get-sentence-lengths-with-c35"></a>Schnellstart: Ermitteln der Satzlänge mit C#

In dieser Schnellstartanleitung ermitteln Sie mithilfe der Textübersetzungs-API die Länge von Sätzen im Text.

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen [Visual Studio 2017](https://www.visualstudio.com/downloads/), um diesen Code unter Windows ausführen zu können. (Die kostenlose Community Edition ist hierfür geeignet.)

Damit Sie die Textübersetzungs-API verwenden können, benötigen Sie darüber hinaus einen Abonnementschlüssel. Informationen hierzu finden Sie unter [Registrieren für die Textübersetzungs-API](translator-text-how-to-signup.md).

## <a name="breaksentence-request"></a>BreakSentence-Anforderung

> [!TIP]
> Rufen Sie den aktuellen Code von [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-C-Sharp) ab.

Der folgende Code unterteilt mithilfe der [BreakSentence](./reference/v3-0-break-sentence.md)-Methode den Quelltext in Sätze.

1. Erstellen Sie in Ihrer bevorzugten IDE ein neues C#-Projekt.
2. Fügen Sie den unten stehenden Code hinzu.
3. Ersetzen Sie den `key`-Wert durch einen für Ihr Abonnement gültigen Zugriffsschlüssel.
4. Führen Sie das Programm aus.

```csharp
using System;
using System.Net.Http;
using System.Text;
// NOTE: Install the Newtonsoft.Json NuGet package.
using Newtonsoft.Json;

namespace TranslatorTextQuickStart
{
    class Program
    {
        static string host = "https://api.cognitive.microsofttranslator.com";
        static string path = "/breaksentence?api-version=3.0";

        static string uri = host + path;

        // NOTE: Replace this example key with a valid subscription key.
        static string key = "ENTER KEY HERE";

        static string text = "How are you? I am fine. What did you do today?";

        async static void Break()
        {
            System.Object[] body = new System.Object[] { new { Text = text } };
            var requestBody = JsonConvert.SerializeObject(body);

            using (var client = new HttpClient())
            using (var request = new HttpRequestMessage())
            {
                request.Method = HttpMethod.Post;
                request.RequestUri = new Uri(uri);
                request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");
                request.Headers.Add("Ocp-Apim-Subscription-Key", key);

                var response = await client.SendAsync(request);
                var responseBody = await response.Content.ReadAsStringAsync();
                var result = JsonConvert.SerializeObject(JsonConvert.DeserializeObject(responseBody), Formatting.Indented);

                Console.OutputEncoding = UnicodeEncoding.UTF8;
                Console.WriteLine(result);
            }
        }

        static void Main(string[] args)
        {
            Break();
            Console.ReadLine();
        }
    }
}
```

## <a name="breaksentence-response"></a>BreakSentence-Antwort

Es wird eine erfolgreiche Antwort im JSON-Format zurückgegeben, wie im folgenden Beispiel gezeigt:

```json
[
  {
    "detectedLanguage": {
      "language": "en",
      "score": 1.0
    },
    "sentLen": [
      13,
      11,
      22
    ]
  }
]
```

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den Beispielcode für diese und andere Schnellstartanleitungen (einschließlich Übersetzung und Transliteration) sowie weitere Beispielprojekte für die Textübersetzung auf GitHub an.

> [!div class="nextstepaction"]
> [C#-Beispiele auf GitHub](https://aka.ms/TranslatorGitHub?type=&language=c%23)
