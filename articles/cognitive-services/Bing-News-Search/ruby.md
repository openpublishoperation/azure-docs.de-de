---
title: 'Schnellstart: Bing-News-Suche-API, Ruby'
titlesuffix: Azure Cognitive Services
description: Hier erhalten Sie Informationen und Codebeispiele für einen schnellen Einstieg in die Verwendung der Bing-News-Suche-API.
services: cognitive-services
author: v-jerkin
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: quickstart
ms.date: 9/21/2017
ms.author: v-jerkin
ms.openlocfilehash: c6c004469b86e443dc1fcc04106d0c7b8b749334
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48803816"
---
# <a name="quickstart-for-bing-news-search-api-with-ruby"></a>Schnellstart für die Bing-News-Suche-API mit Ruby

In diesem Artikel erfahren Sie, wie Sie die Bing-News-Suche-API verwenden, die im Rahmen von Microsoft Cognitive Services in Azure zur Verfügung steht. In diesem Artikel wird zwar Ruby eingesetzt, an sich ist die API aber ein RESTful-Webdienst, der mit jeder Programmiersprache kompatibel ist, die HTTP-Anforderungen senden und JSON analysieren kann. 

Der Beispielcode wurde für Ruby 2.4 geschrieben.

Die [API-Referenz](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference) enthält technische Informationen zu den APIs.

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen ein [Cognitive Services-API-Konto](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) mit **Bing-Suche-APIs**. Die [kostenlose Testversion](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) ist für diesen Schnellstart ausreichend. Sie benötigen den Zugriffsschlüssel, den Sie beim Aktivieren Ihrer kostenlosen Testversion erhalten. Alternativ können Sie den Schlüssel eines kostenpflichtigen Abonnements von Ihrem Azure-Dashboard verwenden.

## <a name="bing-news-search"></a>Bing-News-Suche

Die [Bing-News-Suche-API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference) gibt Newsergebnisse der Bing-Suchmaschine zurück.

1. Erstellen Sie ein neues Ruby-Projekt in Ihrer bevorzugten IDE oder in Ihrem bevorzugten Editor.
2. Fügen Sie den unten stehenden Code hinzu.
3. Ersetzen Sie den `accessKey`-Wert durch einen für Ihr Abonnement gültigen Zugriffsschlüssel.
4. Führen Sie das Programm aus.

```ruby
require 'net/https'
require 'uri'
require 'json'

# **********************************************
# *** Update or verify the following values. ***
# **********************************************

# Replace the accessKey string value with your valid access key.
accessKey = "enter key here"

# Verify the endpoint URI.  At this writing, only one endpoint is used for Bing
# search APIs.  In the future, regional endpoints may be available.  If you
# encounter unexpected authorization errors, double-check this value against
# the endpoint for your Bing Search instance in your Azure dashboard.

uri  = "https://api.cognitive.microsoft.com"
path = "/bing/v7.0/news/search"

term = "Microsoft"

uri = URI(uri + path + "?q=" + URI.escape(term))

puts "Searching news for: " + term

request = Net::HTTP::Get.new(uri)
request['Ocp-Apim-Subscription-Key'] = accessKey

response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
    http.request(request)
end

puts "\nRelevant Headers:\n\n"
response.each_header do |key, value|
    # header names are coerced to lowercase
    if key.start_with?("bingapis-") or key.start_with?("x-msedge-") then
        puts key + ": " + value
    end
end

puts "\nJSON Response:\n\n"
puts JSON::pretty_generate(JSON(response.body))
```

**Antwort**

Es wird eine erfolgreiche Antwort im JSON-Format zurückgegeben, wie im folgenden Beispiel gezeigt:

```json
{
   "_type": "News",
   "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/news\/search?q=Microsoft",
   "totalEstimatedMatches": 36,
   "sort": [
      {
         "name": "Best match",
         "id": "relevance",
         "isSelected": true,
         "url": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/news\/search?q=Microsoft"
      },
      {
         "name": "Most recent",
         "id": "date",
         "isSelected": false,
         "url": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/news\/search?q=Microsoft&sortby=date"
      }
   ],
   "value": [
      {
         "name": "Microsoft to open flagship London brick-and-mortar retail store",
         "url": "http:\/\/www.contoso.com\/article\/microsoft-to-open-flagshi...",
         "image": {
            "thumbnail": {
               "contentUrl": "https:\/\/www.bing.com\/th?id=ON.F9E4A49EC010417...",
               "width": 220,
               "height": 146
            }
         },
         "description": "After years of rumors about Microsoft opening a brick-and-mortar...", 
         "about": [
           {
             "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entiti...", 
             "name": "Microsoft"
           }, 
           {
             "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entit...", 
             "name": "London"
           }
         ], 
         "provider": [
           {
             "_type": "Organization", 
             "name": "Contoso"
           }
         ], 
          "datePublished": "2017-09-21T21:16:00.0000000Z", 
          "category": "ScienceAndTechnology"
      }, 

      . . .
      
      {
         "name": "Microsoft adds Availability Zones to its Azure cloud platform",
         "url": "https:\/\/contoso.com\/2017\/09\/21\/microsoft-adds-availability...",
         "image": {
            "thumbnail": {
               "contentUrl": "https:\/\/www.bing.com\/th?id=ON.0AE7595B9720...",
               "width": 700,
               "height": 466
            }
         },
         "description": "Microsoft has begun adding Availability Zones to its...",
         "about": [
            {
               "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/a093e9b...",
               "name": "Microsoft"
            },
            {
               "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/cf3abf7d-e379-...",
               "name": "Windows Azure"
            },
            {
               "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/9cdd061c-1fae-d0...",
               "name": "Cloud"
            }
         ],
         "provider": [
            {
               "_type": "Organization",
               "name": "Contoso"
            }
         ],
         "datePublished": "2017-09-21T09:01:00.0000000Z",
         "category": "ScienceAndTechnology"
      }
   ]
}
```

 
## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Durchblättern von Nachrichten](paging-news.md)
> [Verwenden von Formatierungsmarkern zum Hervorheben von Text](hit-highlighting.md)
> [Suchen nach Nachrichten im Internet](search-the-web.md)  
> [Testen](https://azure.microsoft.com/services/cognitive-services/bing-web-search-api/)
