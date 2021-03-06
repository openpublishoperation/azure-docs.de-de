---
title: 'Tutorial: Verwenden der Azure Video Indexer-API'
titlesuffix: Azure Cognitive Services
description: In diesem Artikel wird veranschaulicht, wie Sie in die Verwendung der Video Indexer-API einsteigen.
services: cognitive services
author: juliako
manager: cgronlun
ms.service: cognitive-services
ms.component: video-indexer
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: juliako
ms.openlocfilehash: 7bd7ed1b2d2f437ef57598c42ca12ce8bfb174a1
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "45985564"
---
# <a name="tutorial-use-the-video-indexer-api"></a>Tutorial: Verwenden der Azure Video Indexer-API

> [!Note]
> Die v1-API von Video Indexer gilt ab dem 1. August 2018 als veraltet. Es ist ratsam, jetzt die v2-API von Video Indexer zu verwenden. <br/>Eine Anleitung zur Entwicklung mit den v2-APIs für Video Indexer finden Sie [hier](https://api-portal.videoindexer.ai/). 

In Video Indexer ist unterschiedliche KI-Technologie (Künstliche Intelligenz) für Audio- und Videodaten zusammengefasst, und der Dienst wird von Microsoft als integrierter Dienst angeboten, um die Entwicklung zu vereinfachen. Die APIs sind so konzipiert, dass sich Entwickler auf die Nutzung der KI-Technologie für Medien konzentrieren können, ohne sich um Dinge wie die Skalierung, globale Reichweite, Verfügbarkeit und Zuverlässigkeit der Cloudplattform kümmern zu müssen. Sie können die API verwenden, um Ihre Dateien hochzuladen, ausführliche Videoinformationen zu erhalten, URLs von Insights- und Player-Widgets abzurufen, um diese in Ihre Anwendung einzubetten, und andere Aufgaben durchzuführen.

Beim Erstellen eines Video Indexer-Kontos können Sie ein kostenloses Testkonto (mit einer bestimmten Anzahl von kostenlosen Indizierungsminuten) oder eine kostenpflichtige Option wählen (ohne Einschränkung durch eine Kontingentvorgabe). Bei der kostenlosen Testversion stellt Video Indexer bis zu 600 Minuten an kostenloser Indizierungszeit für Websitebenutzer und bis zu 2.400 Minuten an kostenloser Indizierungszeit für API-Benutzer bereit. Bei der kostenpflichtigen Option erstellen Sie ein Video Indexer-Konto, das [mit Ihrem Azure-Abonnement und einem Azure Media Services-Konto verbunden ist](connect-to-azure.md). Die Indizierungszeit wird minutenweise abgerechnet. Außerdem fallen Gebühren für das Azure Media Services-Konto an. 

In diesem Artikel wird veranschaulicht, wie Entwickler die [Video Indexer-API](https://api-portal.videoindexer.ai/) nutzen können.

## <a name="subscribe-to-the-api"></a>Abonnieren der API

1. Melden Sie sich beim [Portal für Video Indexer-Entwickler an](https://api-portal.videoindexer.ai/).
    
    ![Anmelden](./media/video-indexer-use-apis/video-indexer-api01.png)

    > [!Important]
    > * Sie müssen denselben Anbieter verwenden, den Sie beim Anmelden für Video Indexer genutzt haben.
    > * Persönliche Google- und Microsoft-Konten (outlook/live) können nur für Testkonten verwendet werden. Für Konten, die über eine Verbindung mit Azure verfügen, ist Azure AD erforderlich.
    > * Pro E-Mail-Adresse kann nur ein aktives Konto vorhanden sein. Wenn ein Benutzer versucht, sich mit user@gmail.com für LinkedIn anzumelden, und anschließend user@gmail.com auch für Google verwendet, wird für die zweite Anmeldung eine Fehlerseite mit dem Hinweis angezeigt, dass der Benutzer bereits vorhanden ist.

2. Führen Sie den Schritt zum Abonnieren aus.

    Wählen Sie die Registerkarte [Produkte](https://api-portal.videoindexer.ai/products). Wählen Sie anschließend „Authorization“ (Autorisierung), und führen Sie den Vorgang für die Einrichtung des Abonnements durch. 
    
    ![Registrieren](./media/video-indexer-use-apis/video-indexer-api02.png)

    > [!NOTE]
    > Für neue Benutzer wird das Abonnement der Autorisierung automatisch eingerichtet.
    
    Nachdem das Abonnement eingerichtet wurde, werden Ihr Abonnement und Ihre Primär- und Sekundärschlüssel angezeigt. Die Schlüssel sollten geschützt werden. Die Schlüssel sollten nur von Ihrem Servercode verwendet werden. Sie sollten nicht auf Clientseite (.js, .html usw.) verfügbar sein.

    ![Registrieren](./media/video-indexer-use-apis/video-indexer-api03.png)

## <a name="obtain-access-token-using-the-authorization-api"></a>Abrufen von Zugriffstoken mit der Authorization-API

Nachdem Sie die Authorization-API abonniert haben, können Sie Zugriffstoken abrufen. Diese Zugriffstoken werden zur Authentifizierung für die Operations-API verwendet. 

Jeder Aufruf der Operations-API sollte einem Zugriffstoken zugeordnet sein und mit dem Autorisierungsbereich des Aufrufs übereinstimmen.

- Benutzerebene: Mit Zugriffstoken auf Benutzerebene können Sie Vorgänge auf der Benutzerebene (**user**) durchführen. Ein Beispiel hierfür ist das Abrufen von zugeordneten Konten.
- Kontoebene: Mit Zugriffstoken auf Kontoebene können Sie Vorgänge auf der Kontoebene (**account**) oder Videoebene (**video**) durchführen. Beispiele hierfür sind das Hochladen von Videos, Auflisten aller Videos, Abrufen von Videoinformationen usw.
- Videoebene: Mit Zugriffstoken auf Videoebene können Sie Vorgänge für ein bestimmtes **Video** durchführen. Beispiele hierfür sind das Abrufen von Videoinformationen, Herunterladen von Untertiteln, Abrufen von Widgets usw. 

Sie können steuern, ob diese Token schreibgeschützt sind oder ob die Bearbeitung aufgrund der Angabe von **allowEdit=true/false** möglich ist.

Für die meisten Server-zu-Server-Szenarien nutzen Sie voraussichtlich dasselbe Token vom Typ „Konto“ (**account**), da hiermit sowohl **account**-Vorgänge als auch **video**-Vorgänge abgedeckt sind. Falls Sie planen, clientseitig Aufrufe von Video Indexer durchzuführen (z.B. per JavaScript), sollten Sie aber ein **video**-Zugriffstoken verwenden, um zu verhindern, dass Clients Zugriff auf das gesamte Konto erhalten. Dies ist auch der Grund, warum Sie beim Einbetten von Video Indexer-Clientcode in Ihren Client (z.B. per **Get Insights Widget** (Insights-Widget abrufen) oder **Get Player Widget** (Player-Widget abrufen)) ein **video**-Zugriffstoken angeben müssen.

Zur Vereinfachung können Sie „**Authorization**-API“ > „**GetAccounts**“ verwenden, um Ihre Konten abzurufen, ohne dass Sie zuerst ein Benutzertoken beschaffen müssen. Sie können auch darum bitten, dass Sie die Konten mit gültigen Token erhalten, um einen zusätzlichen Aufruf zum Abrufen eines Kontotokens zu sparen.

Zugriffstoken laufen nach einer Stunde ab. Stellen Sie sicher, dass Ihr Zugriffstoken gültig ist, bevor Sie die Operations-API verwenden. Wenn es abgelaufen ist, müssen Sie die Authorization-API erneut aufrufen, um ein neues Zugriffstoken zu erhalten.
 
Sie sind nun bereit, um mit dem Integrieren mit der API zu beginnen. Lesen Sie sich die [ausführliche Beschreibung zu den einzelnen Video Indexer-REST-APIs](http://api-portal.videoindexer.ai/) durch.

## <a name="location"></a>Standort

Für alle Operations-APIs ist ein Standortparameter (Location) erforderlich. Hiermit wird die Region angegeben, an die der Aufruf geleitet werden soll und in der das Konto erstellt wurde.

Es gelten die in der folgenden Tabelle beschriebenen Werte. Der **Parameterwert** ist der Wert, den Sie beim Verwenden der API übergeben.

|**Name**|**Parameterwert**|**Beschreibung**|
|---|---|---|
|Testversion|Testversion|Wird für Testkonten verwendet.|
|USA (Westen)|westus2|Wird für die Azure-Region „West US 2“ (USA, Westen 2) verwendet.|
|Nordeuropa |northeurope|Wird für die Azure-Region „North Europe“ (Europa, Norden) verwendet.|
|Asien, Osten|eastasia|Wird für die Azure-Region „East Asia“ (Asien, Osten) verwendet.|

## <a name="account-id"></a>Konto-ID 

Der Parameter für die Konto-ID (Account ID) ist in allen API-Aufrufen für Vorgänge erforderlich. Die Konto-ID ist eine GUID, die auf eine der folgenden Arten abgerufen werden kann:

* Rufen Sie auf der **Video Indexer**-Website die Konto-ID ab:

    1. Navigieren Sie zur [Video Indexer](https://www.videoindexer.ai/)-Website, und melden Sie sich an.
    2. Navigieren Sie zur Seite **Einstellungen**.
    3. Kopieren Sie die Konto-ID.

        ![Konto-ID](./media/video-indexer-use-apis/account-id.png)

* Im  **Portal für Video Indexer-Entwickler** können Sie die Konto-ID programmgesteuert abrufen.

    Verwenden Sie die API [Get accounts](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Accounts?) (Konten abrufen).
    
    > [!TIP]
    > Sie können Zugriffstoken für die Konten generieren, indem Sie `generateAccessTokens=true` definieren.
    
* Rufen Sie die Konto-ID aus der URL einer Playerseite in Ihrem Konto ab.

    Beim Wiedergeben eines Videos wird die ID nach dem Abschnitt `accounts` und vor dem Abschnitt `videos` angezeigt.

    ```
    https://www.videoindexer.ai/accounts/00000000-f324-4385-b142-f77dacb0a368/videos/d45bf160b5/
    ```

## <a name="recommendations"></a>Empfehlungen

In diesem Abschnitt sind einige Empfehlungen für die Verwendung der Video Indexer-API aufgeführt.

- Wenn Sie den Upload eines Videos planen, wird empfohlen, die Datei unter einer öffentlichen Netzwerkadresse anzuordnen (z.B. OneDrive). Rufen Sie den Link zum Video ab, und geben Sie die URL als Parameter für den Dateiupload an. 

    Die für Video Indexer angegebene URL muss auf eine Mediendatei (Audio oder Video) verweisen. Einige der Links, die von OneDrive generiert werden, sind für eine HTML-Seite bestimmt, die die Datei enthält. Eine einfache Möglichkeit zum Überprüfen der URL besteht darin, sie in einen Browser einzufügen. Wenn der Download der Datei beginnt, ist die URL mit hoher Wahrscheinlichkeit korrekt. Falls im Browser eine Visualisierung gerendert wird, handelt es sich wahrscheinlich nicht um einen Link zu einer Datei, sondern zu einer HTML-Seite.
    
- Wenn Sie die API aufrufen, mit der Videoinformationen für das angegebene Video abgerufen werden, erhalten Sie eine ausführliche JSON-Ausgabe als Inhalt der Antwort. [Lesen Sie die Details zum zurückgegebenen JSON-Code in diesem Thema](video-indexer-output-json.md).

## <a name="code-sample"></a>Codebeispiel

Mit dem folgenden C#-Codeausschnitt wird die Nutzung aller Video Indexer-APIs zusammen veranschaulicht.

```csharp
var apiUrl = "https://api.videoindexer.ai";
var accountId = "..."; 
var location = "westus2";
var apiKey = "..."; 

System.Net.ServicePointManager.SecurityProtocol = System.Net.ServicePointManager.SecurityProtocol | System.Net.SecurityProtocolType.Tls12;

// create the http client
var handler = new HttpClientHandler(); 
handler.AllowAutoRedirect = false; 
var client = new HttpClient(handler);
client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey); 

// obtain account access token
var accountAccessTokenRequestResult = client.GetAsync($"{apiUrl}/auth/{location}/Accounts/{accountId}/AccessToken?allowEdit=true").Result;
var accountAccessToken = accountAccessTokenRequestResult.Content.ReadAsStringAsync().Result.Replace("\"", "");

client.DefaultRequestHeaders.Remove("Ocp-Apim-Subscription-Key");

// upload a video
var content = new MultipartFormDataContent();
Debug.WriteLine("Uploading...");
// get the video from URL
var videoUrl = "VIDEO_URL"; // replace with the video URL

// as an alternative to specifying video URL, you can upload a file.
// remove the videoUrl parameter from the query string below and add the following lines:
  //FileStream video =File.OpenRead(Globals.VIDEOFILE_PATH);
  //byte[] buffer =newbyte[video.Length];
  //video.Read(buffer, 0, buffer.Length);
  //content.Add(newByteArrayContent(buffer));

var uploadRequestResult = client.PostAsync($"{apiUrl}/{location}/Accounts/{accountId}/Videos?accessToken={accountAccessToken}&name=some_name&description=some_description&privacy=private&partition=some_partition&videoUrl={videoUrl}", content).Result;
var uploadResult = uploadRequestResult.Content.ReadAsStringAsync().Result;

// get the video id from the upload result
var videoId = JsonConvert.DeserializeObject<dynamic>(uploadResult)["id"];
Debug.WriteLine("Uploaded");
Debug.WriteLine("Video ID: " + videoId);           

// obtain video access token            
client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);
var videoTokenRequestResult = client.GetAsync($"{apiUrl}/auth/{location}/Accounts/{accountId}/Videos/{videoId}/AccessToken?allowEdit=true").Result;
var videoAccessToken = videoTokenRequestResult.Content.ReadAsStringAsync().Result.Replace("\"", "");

client.DefaultRequestHeaders.Remove("Ocp-Apim-Subscription-Key");

// wait for the video index to finish
while (true)
{
  Thread.Sleep(10000);

  var videoGetIndexRequestResult = client.GetAsync($"{apiUrl}/{location}/Accounts/{accountId}/Videos/{videoId}/Index?accessToken={videoAccessToken}&language=English").Result;
  var videoGetIndexResult = videoGetIndexRequestResult.Content.ReadAsStringAsync().Result;

  var processingState = JsonConvert.DeserializeObject<dynamic>(videoGetIndexResult)["state"];

  Debug.WriteLine("");
  Debug.WriteLine("State:");
  Debug.WriteLine(processingState);

  // job is finished
  if (processingState != "Uploaded" && processingState != "Processing")
  {
      Debug.WriteLine("");
      Debug.WriteLine("Full JSON:");
      Debug.WriteLine(videoGetIndexResult);
      break;
  }
}

// search for the video
var searchRequestResult = client.GetAsync($"{apiUrl}/{location}/Accounts/{accountId}/Videos/Search?accessToken={accountAccessToken}&id={videoId}").Result;
var searchResult = searchRequestResult.Content.ReadAsStringAsync().Result;
Debug.WriteLine("");
Debug.WriteLine("Search:");
Debug.WriteLine(searchResult);

// get insights widget url
var insightsWidgetRequestResult = client.GetAsync($"{apiUrl}/{location}/Accounts/{accountId}/Videos/{videoId}/InsightsWidget?accessToken={videoAccessToken}&widgetType=Keywords&allowEdit=true").Result;
var insightsWidgetLink = insightsWidgetRequestResult.Headers.Location;
Debug.WriteLine("Insights Widget url:");
Debug.WriteLine(insightsWidgetLink);

// get player widget url
var playerWidgetRequestResult = client.GetAsync($"{apiUrl}/{location}/Accounts/{accountId}/Videos/{videoId}/PlayerWidget?accessToken={videoAccessToken}").Result;
var playerWidgetLink = playerWidgetRequestResult.Headers.Location;
Debug.WriteLine("");
Debug.WriteLine("Player Widget url:");
Debug.WriteLine(playerWidgetLink);

```

## <a name="next-steps"></a>Nächste Schritte

[Untersuchen der von der v2-API erstellten Azure Video Indexer-Ausgabe](video-indexer-output-json.md)

[Was ist Video Indexer?](video-indexer-overview.md)
