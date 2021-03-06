---
title: 'Schnellstart: Verwenden des Bing-Websuche-SDK für Python'
titleSuffix: Azure Cognitive Services
description: Das Bing-Websuche-SDK erleichtert die Integration der Bing-Websuche in Ihre Python-Anwendung. In dieser Schnellstartanleitung erfahren Sie, wie Sie eine Anforderung senden, eine JSON-Antwort empfangen und die Ergebnisse filtern und analysieren.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: quickstart
ms.date: 08/16/2018
ms.author: erhopf
ms.openlocfilehash: c83848bc3c8f14aa49c1f87f1cd44224bb3e8081
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "46127635"
---
# <a name="quickstart-use-the-bing-web-search-sdk-for-python"></a>Schnellstart: Verwenden des Bing-Websuche-SDK für Python

Das Bing-Websuche-SDK erleichtert die Integration der Bing-Websuche in Ihre Python-Anwendung. In dieser Schnellstartanleitung erfahren Sie, wie Sie eine Anforderung senden, eine JSON-Antwort empfangen und die Ergebnisse filtern und analysieren.

Möchten Sie den Code jetzt sehen? Die [Beispiele zum Bing-Websuche-SDK für Python](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples) sind auf GitHub verfügbar.

[!INCLUDE [bing-web-search-quickstart-signup](../../../includes/bing-web-search-quickstart-signup.md)]

## <a name="prerequisites"></a>Voraussetzungen

Das Bing-Websuche-SDK ist mit Python 2.7, 3.3, 3.4, 3.5 und 3.6 kompatibel. Es wird empfohlen, für diese Schnellstartanleitung eine virtuelle Umgebung zu verwenden.

* Python 2.7, 3.3, 3.4, 3.5 oder 3.6
* [virtualenv](https://docs.python.org/3/tutorial/venv.html) für Python 2.7
* [venv](https://pypi.python.org/pypi/virtualenv) für Python 3.x

## <a name="create-and-configure-your-virtual-environment"></a>Erstellen und Konfigurieren der virtuellen Umgebung

Die Anweisungen zum Einrichten und Konfigurieren einer virtuellen Umgebung unterscheiden sich für Python 2.x und Python 3.x. Führen Sie die folgenden Schritte aus, um die virtuelle Umgebung zu erstellen und zu initialisieren.

### <a name="python-2x"></a>Python 2.x

Erstellen Sie mit `virtualenv` eine virtuelle Umgebung für Python 2.7:

```console
virtualenv mytestenv
```

Aktivieren Sie Ihre Umgebung:

```console
cd mytestenv
source bin/activate
```

Installieren Sie Abhängigkeiten des Bing-Websuche-SDK:

```console
python -m pip install azure-cognitiveservices-search-websearch
```

### <a name="python-3x"></a>Python 3.x

Erstellen Sie mit `venv` eine virtuelle Umgebung für Python 3.x:

```console
python -m venv mytestenv
```

Installieren Sie Abhängigkeiten des Bing-Websuche-SDK:

```console
cd mytestenv
python -m pip install azure-cognitiveservices-search-websearch
```

## <a name="create-a-client-and-print-your-first-results"></a>Erstellen eines Clients und Ausgeben der ersten Ergebnisse

Sie haben Ihre virtuelle Umgebung eingerichtet und Abhängigkeiten installiert. Erstellen Sie nun einen Client. Der Client verarbeitet Anforderungen an die und Antworten von der Bing-Websuche-API.

Wenn die Antwort Webseiten, Bilder, Nachrichten oder Videos enthält, wird jeweils das erste Ergebnis für diese Elemente ausgegeben.

1. Erstellen Sie in Ihrer bevorzugten IDE oder Ihrem bevorzugten Editor ein neues Python-Projekt.
2. Kopieren Sie diesen Beispielcode in Ihr Projekt:  
    ```python
    # Import required modules.
    from azure.cognitiveservices.search.websearch import WebSearchAPI
    from azure.cognitiveservices.search.websearch.models import SafeSearch
    from msrest.authentication import CognitiveServicesCredentials

    # Replace with your subscription key.
    subscription_key = "YOUR_SUBSCRIPTION_KEY"

    # Instantiate the client.
    client = WebSearchAPI(CognitiveServicesCredentials(subscription_key))

    # Make a request. Replace Yosemite if you'd like.
    web_data = client.web.search(query="Yosemite")
    print("\r\nSearched for Query# \" Yosemite \"")

    '''
    Web pages
    If the search response contains web pages, the first result's name and url
    are printed.
    '''
    if hasattr(web_data.web_pages, 'value'):

        print("\r\nWebpage Results#{}".format(len(web_data.web_pages.value)))

        first_web_page = web_data.web_pages.value[0]
        print("First web page name: {} ".format(first_web_page.name))
        print("First web page URL: {} ".format(first_web_page.url))

    else:
        print("Didn't find any web pages...")

    '''
    Images
    If the search response contains images, the first result's name and url
    are printed.
    '''
    if hasattr(web_data.images, 'value'):

        print("\r\nImage Results#{}".format(len(web_data.images.value)))

        first_image = web_data.images.value[0]
        print("First Image name: {} ".format(first_image.name))
        print("First Image URL: {} ".format(first_image.url))

    else:
        print("Didn't find any images...")

    '''
    News
    If the search response contains news, the first result's name and url
    are printed.
    '''
    if hasattr(web_data.news, 'value'):

        print("\r\nNews Results#{}".format(len(web_data.news.value)))

        first_news = web_data.news.value[0]
        print("First News name: {} ".format(first_news.name))
        print("First News URL: {} ".format(first_news.url))

    else:
        print("Didn't find any news...")

    '''
    If the search response contains videos, the first result's name and url
    are printed.
    '''
    if hasattr(web_data.videos, 'value'):

        print("\r\nVideos Results#{}".format(len(web_data.videos.value)))

        first_video = web_data.videos.value[0]
        print("First Videos name: {} ".format(first_video.name))
        print("First Videos URL: {} ".format(first_video.url))

    else:
        print("Didn't find any videos...")
    ```
3. Ersetzen Sie `subscription_key` durch einen gültigen Abonnementschlüssel.
4. Führen Sie das Programm aus. Beispiel: `python your_program.py`.

## <a name="define-functions-and-filter-results"></a>Definieren von Funktionen und Filtern der Ergebnisse

Sie haben Ihren ersten Aufruf der Bing-Websuche-API ausgeführt. Sehen wir uns nun einige Funktionen an, die SDK-Funktionalität zum Optimieren von Abfragen und Filtern von Ergebnissen unterstreichen. Jede Funktion kann zu Ihrem im vorherigen Abschnitt erstellten Python-Programm hinzugefügt werden.

### <a name="limit-the-number-of-results-returned-by-bing"></a>Einschränken der von Bing zurückgegebenen Anzahl von Ergebnissen

In diesem Beispiel wird mit den Parametern `count` und `offset` die Anzahl von Ergebnissen eingeschränkt, die mithilfe der [`search`-Methode](https://docs.microsoft.com/python/api/azure-cognitiveservices-search-websearch/azure.cognitiveservices.search.websearch.operations.weboperations?view=azure-python#search) des SDK zurückgegeben werden. `name` und `URL` für das erste Ergebnis werden ausgegeben.

1. Fügen Sie folgenden Code zu Ihrem Python-Projekt hinzu:
    ```python
    # Declare the function.
    def web_results_with_count_and_offset(subscription_key):
        client = WebSearchAPI(CognitiveServicesCredentials(subscription_key))

        try:
            '''
            Set the query, offset, and count using the SDK's search method. See:
            https://docs.microsoft.com/python/api/azure-cognitiveservices-search-websearch/azure.cognitiveservices.search.websearch.operations.weboperations?view=azure-python#search.
            '''
            web_data = client.web.search(query="Best restaurants in Seattle", offset=10, count=20)
            print("\r\nSearching for \"Best restaurants in Seattle\"")

            if web_data.web_pages.value:
                '''
                If web pages are available, print the # of responses, and the first and second
                web pages returned.
                '''
                print("Webpage Results#{}".format(len(web_data.web_pages.value)))

                first_web_page = web_data.web_pages.value[0]
                print("First web page name: {} ".format(first_web_page.name))
                print("First web page URL: {} ".format(first_web_page.url))

            else:
                print("Didn't find any web pages...")

        except Exception as err:
            print("Encountered exception. {}".format(err))
    ```
2. Führen Sie das Programm aus.

### <a name="filter-for-news-and-freshness"></a>Filtern nach Nachrichten und Aktualität

In diesem Beispiel werden die Parameter `response_filter` und `freshness` verwendet, um Suchergebnisse mithilfe der [`search`-Methode](https://docs.microsoft.com//api/azure-cognitiveservices-search-websearch/azure.cognitiveservices.search.websearch.operations.weboperations?view=azure-python#search) des SDK zu filtern. Die zurückgegebenen Suchergebnisse sind auf Nachrichtenartikel und -seiten beschränkt, die von Bing in den letzten 24 Stunden gefunden wurden. `name` und `URL` für das erste Ergebnis werden ausgegeben.

1. Fügen Sie folgenden Code zu Ihrem Python-Projekt hinzu:
    ```python
    # Declare the function.
    def web_search_with_response_filter(subscription_key):
        client = WebSearchAPI(CognitiveServicesCredentials(subscription_key))
        try:
            '''
            Set the query, response_filter, and freshness using the SDK's search method. See:
            https://docs.microsoft.com/python/api/azure-cognitiveservices-search-websearch/azure.cognitiveservices.search.websearch.operations.weboperations?view=azure-python#search.
            '''
            web_data = client.web.search(query="xbox",
                response_filter=["News"],
                freshness="Day")
            print("\r\nSearching for \"xbox\" with the response filter set to \"News\" and freshness filter set to \"Day\".")

            '''
            If news articles are available, print the # of responses, and the first and second
            articles returned.
            '''
            if web_data.news.value:

                print("# of news results: {}".format(len(web_data.news.value)))

                first_web_page = web_data.news.value[0]
                print("First article name: {} ".format(first_web_page.name))
                print("First article URL: {} ".format(first_web_page.url))

                print("")

                second_web_page = web_data.news.value[1]
                print("\nSecond article name: {} ".format(second_web_page.name))
                print("Second article URL: {} ".format(second_web_page.url))

            else:
                print("Didn't find any news articles...")

        except Exception as err:
            print("Encountered exception. {}".format(err))

    # Call the function.
    web_search_with_response_filter(subscription_key)
    ```
2. Führen Sie das Programm aus.

### <a name="use-safe-search-answer-count-and-the-promote-filter"></a>Verwenden der Filter für sichere Suche, Antwortanzahl und Höherstufen

In diesem Beispiel werden die Parameter `answer_count`, `promote` und `safe_search` verwendet, um Suchergebnisse mithilfe der [`search`-Methode](https://docs.microsoft.com/python/api/azure-cognitiveservices-search-websearch/azure.cognitiveservices.search.websearch.operations.weboperations?view=azure-python#search) des SDK zu filtern. `name` und `URL` für das erste Ergebnis werden angezeigt.

1. Fügen Sie folgenden Code zu Ihrem Python-Projekt hinzu:
    ```python
    # Declare the function.
    def web_search_with_answer_count_promote_and_safe_search(subscription_key):

        client = WebSearchAPI(CognitiveServicesCredentials(subscription_key))

        try:
            '''
            Set the query, answer_count, promote, and safe_search parameters using the SDK's search method. See:
            https://docs.microsoft.com/python/api/azure-cognitiveservices-search-websearch/azure.cognitiveservices.search.websearch.operations.weboperations?view=azure-python#search.
            '''
            web_data = client.web.search(
                query="Niagara Falls",
                answer_count=2,
                promote=["videos"],
                safe_search=SafeSearch.strict  # or directly "Strict"
            )
            print("\r\nSearching for \"Niagara Falls\"")

            '''
            If results are available, print the # of responses, and the first result returned.
            '''
            if web_data.web_pages.value:

                print("Webpage Results#{}".format(len(web_data.web_pages.value)))

                first_web_page = web_data.web_pages.value[0]
                print("First web page name: {} ".format(first_web_page.name))
                print("First web page URL: {} ".format(first_web_page.url))

            else:
                print("Didn't see any Web data..")

        except Exception as err:
            print("Encountered exception. {}".format(err))
    ```
2. Führen Sie das Programm aus.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mit diesem Projekt fertig sind, entfernen Sie unbedingt den Abonnementschlüssel aus dem Programmcode, und deaktivieren Sie die virtuelle Umgebung.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Cognitive Services SDK-Beispiele für Python](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples)

## <a name="see-also"></a>Weitere Informationen

* [Referenz zum Azure Python SDK](https://docs.microsoft.com/python/api/overview/azure/cognitiveservices/websearch)
