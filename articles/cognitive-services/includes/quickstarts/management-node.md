---
title: 'Schnellstart: Clientbibliothek für die Azure-Verwaltung für Node.js'
description: In dieser Schnellstartanleitung werden die ersten Schritte mit der Clientbibliothek für die Azure-Verwaltung für Node.js erläutert.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 08/05/2020
ms.author: pafarley
ms.openlocfilehash: 59063912b2976890976e3c27bc4f2d93b6392e37
ms.sourcegitcommit: d661149f8db075800242bef070ea30f82448981e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2020
ms.locfileid: "88607619"
---
[Referenzdokumentation](https://docs.microsoft.com/javascript/api/@azure/arm-cognitiveservices/?view=azure-node-latest) | [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/cognitiveservices/arm-cognitiveservices) | [Paket (NPM)](https://www.npmjs.com/package/@azure/arm-cognitiveservices) | [Beispiele](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/cognitiveservices/arm-cognitiveservices#sample-code)

## <a name="prerequisites"></a>Voraussetzungen

* Ein gültiges Azure-Abonnement: [Erstellen Sie ein kostenloses Abonnement](https://azure.microsoft.com/free/).
* Die aktuelle Version von [Node.js](https://nodejs.org/)

## <a name="create-an-azure-service-principal"></a>Erstellen eines Azure-Dienstprinzipals

Damit Ihre Anwendung mit Ihrem Azure-Konto interagieren kann, benötigen Sie einen Azure-Dienstprinzipal zum Verwalten der Berechtigungen. Folgen Sie den Anweisungen unter [Erstellen eines Azure-Dienstprinzipals](https://docs.microsoft.com/powershell/azure/create-azure-service-principal-azureps?view=azps-4.4.0&viewFallbackFrom=azps-3.3.0).

Wenn Sie einen Dienstprinzipal erstellen, sehen Sie, dass dieser einen Geheimniswert, eine ID und eine Anwendungs-ID aufweist. Speichern Sie die Anwendungs-ID und das Geheimnis für spätere Schritte an einem temporären Speicherort.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Vor dem Erstellen einer Cognitive Services-Ressource benötigen Sie in Ihrem Konto eine Azure-Ressourcengruppe, die die Ressource enthalten soll. Erstellen Sie im [Azure-Portal](https://ms.portal.azure.com/) eine Ressourcengruppe, falls Sie noch keine haben.

## <a name="create-a-new-nodejs-application"></a>Erstellen einer neuen Node.js-Anwendung

Erstellen Sie in einem Konsolenfenster (etwa cmd, PowerShell oder Bash) ein neues Verzeichnis für Ihre App, und rufen Sie es auf. 

```console
mkdir myapp && cd myapp
```

Führen Sie den Befehl `npm init` aus, um eine Knotenanwendung mit der Datei `package.json` zu erstellen. 

```console
npm init
```

Erstellen Sie eine Datei namens _index.js_, bevor Sie fortfahren.

### <a name="install-the-client-library"></a>Installieren der Clientbibliothek

Installieren Sie die folgenden NPM-Pakete:

```console
npm install @azure/arm-cognitiveservices
npm install @azure/ms-rest-js
npm install @azure/ms-rest-nodeauth
```

Die Datei `package.json` Ihrer App wird mit den Abhängigkeiten aktualisiert.

### <a name="import-libraries"></a>Importbibliotheken

Öffnen Sie das Skript _index.js_, und importieren Sie die folgenden Bibliotheken.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/azure_management_service/create_delete_resource.js?name=snippet_imports)]

## <a name="authenticate-the-client"></a>Authentifizieren des Clients

Fügen Sie dem Stamm des Skripts die folgenden Felder hinzu, und geben Sie die zugehörigen Werte unter Verwendung des zuvor erstellten Dienstprinzipals und Ihrer Azure-Kontoinformationen ein.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/azure_management_service/create_delete_resource.js?name=snippet_constants)]

Fügen Sie als Nächstes die folgende `quickstart`-Funktion für die eigentliche Arbeit des Programms hinzu. Der erste Codeblock erstellt ein **CognitiveServicesManagementClient**-Objekt mithilfe der Anmeldeinformationsvariablen, die Sie oben eingegeben haben. Dieses Objekt wird für alle Azure-Verwaltungsvorgänge benötigt.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/azure_management_service/create_delete_resource.js?name=snippet_main_auth)]

## <a name="call-management-functions"></a>Aufrufen von Verwaltungsfunktionen

Fügen Sie am Ende Ihrer `quickstart`-Methode den folgenden Code hinzu, um verfügbare Ressourcen aufzulisten, eine Beispielressource zu erstellen, Ihre eigenen Ressourcen aufzulisten und dann die Beispielressource zu löschen. Diese Funktionen definieren Sie in den nächsten Schritten.

## <a name="create-a-cognitive-services-resource"></a>Erstellen einer Cognitive Services-Ressource

### <a name="choose-a-service-and-pricing-tier"></a>Auswählen eines Diensts und eines Tarifs

Wenn Sie eine neue Ressource erstellen, müssen Sie die „Art“ des Diensts, den Sie verwenden möchten, sowie den gewünschten [Tarif](https://azure.microsoft.com/pricing/details/cognitive-services/) (oder die SKU) kennen. Diese und andere Informationen verwenden Sie beim Erstellen der Ressource als Parameter. Die folgende Funktion listet die verfügbaren Arten von Cognitive Services-Diensten auf.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/azure_management_service/create_delete_resource.js?name=snippet_list_avail)]

[!INCLUDE [cognitive-services-subscription-types](../../../../includes/cognitive-services-subscription-types.md)]

Weitere Informationen finden Sie unten in der Liste der SKUs und Preise. 

#### <a name="multi-service"></a>Mehrere Dienste

| Dienst                    | Variante                      |
|----------------------------|---------------------------|
| Mehrere Dienste. Weitere Informationen hierzu finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/cognitive-services/).            | `CognitiveServices`     |


#### <a name="vision"></a>Bildanalyse

| Dienst                    | Variante                      |
|----------------------------|---------------------------|
| Maschinelles Sehen            | `ComputerVision`          |
| Custom Vision – Vorhersage | `CustomVision.Prediction` |
| Custom Vision – Training   | `CustomVision.Training`   |
| Gesicht                       | `Face`                    |
| Formularerkennung            | `FormRecognizer`          |
| Freihanderkennung             | `InkRecognizer`           |

#### <a name="search"></a>Suchen,

| Dienst            | Variante                  |
|--------------------|-----------------------|
| Bing-Vorschlagssuche   | `Bing.Autosuggest.v7` |
| Benutzerdefinierte Bing-Suche | `Bing.CustomSearch`   |
| Bing-Entitätssuche | `Bing.EntitySearch`   |
| Bing-Suche        | `Bing.Search.v7`      |
| Bing-Rechtschreibprüfung   | `Bing.SpellCheck.v7`  |

#### <a name="speech"></a>Spracheingabe

| Dienst            | Variante                 |
|--------------------|----------------------|
| Spracherkennungsdienste    | `SpeechServices`     |
| Spracherkennung | `SpeakerRecognition` |

#### <a name="language"></a>Sprache

| Dienst            | Variante                |
|--------------------|---------------------|
| Formularerkennung | `FormUnderstanding` |
| LUIS               | `LUIS`              |
| QnA Maker          | `QnAMaker`          |
| Textanalyse     | `TextAnalytics`     |
| Textübersetzung   | `TextTranslation`   |

#### <a name="decision"></a>Entscheidung

| Dienst           | Variante               |
|-------------------|--------------------|
| Anomalieerkennung  | `AnomalyDetector`  |
| Content Moderator | `ContentModerator` |
| Personalisierung      | `Personalizer`     |


#### <a name="pricing-tiers-and-billing"></a>Tarife und Abrechnung

Die Tarife (und der Betrag, der Ihnen in Rechnung gestellt wird) basieren auf der Anzahl der unter ihren Authentifizierungsinformationen gesendeten Transaktionen. Für jeden Tarif wird Folgendes angegeben:
* Maximale Anzahl zulässiger Transaktionen pro Sekunde (TPS).
* Dienstfeatures, die innerhalb des Tarifs aktiviert sind.
* Kosten für eine vordefinierte Anzahl von Transaktionen. Wird diese Anzahl überschritten, fällt eine zusätzliche Gebühr gemäß den [Preisdetails](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/) für Ihren Dienst an.

> [!NOTE]
> Für viele der Cognitive Services-Dienste ist ein kostenloser Tarif verfügbar, mit dem Sie den Dienst ausprobieren können. Um den kostenlosen Tarif zu nutzen, verwenden Sie `F0` als SKU für Ihre Ressource.

## <a name="create-a-cognitive-services-resource"></a>Erstellen einer Cognitive Services-Ressource

Um eine neue Cognitive Services-Ressource zu erstellen und zu abonnieren, verwenden Sie die **Create**-Funktion. Diese Funktion fügt der Ressourcengruppe, die Sie übergeben, eine neue abrechenbare Ressource hinzu. Wenn Sie die neue Ressource erstellen, müssen Sie die „Art“ des Diensts, den Sie verwenden möchten, sowie dessen Tarif (oder die SKU) und einen Azure-Standort kennen. Die folgende Funktion akzeptiert all diese Argumente und erstellt eine Ressource.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/azure_management_service/create_delete_resource.js?name=snippet_create)]

## <a name="view-your-resources"></a>Anzeigen Ihrer Ressourcen

Verwenden Sie die folgende Funktion, um alle Ressourcen in Ihrem Azure-Konto (in allen Ressourcengruppen) anzuzeigen:

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/azure_management_service/create_delete_resource.js?name=snippet_list)]

## <a name="delete-a-resource"></a>Löschen einer Ressource

Die folgende Funktion löscht die angegebene Ressource aus der jeweiligen Ressourcengruppe.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/azure_management_service/create_delete_resource.js?name=snippet_delete)]

## <a name="run-the-application"></a>Ausführen der Anwendung

Fügen Sie am Ende Ihres Skripts den folgenden Code hinzu, um die `quickstart`-Hauptfunktion mit Fehlerbehandlung aufzurufen.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/azure_management_service/create_delete_resource.js?name=snippet_main)]

Führen Sie dann in Ihrem Konsolenfenster die Anwendung mit dem Befehl `node` aus.

```console
node index.js
```

## <a name="see-also"></a>Siehe auch

* [Referenzdokumentation für das Azure Management SDK](https://docs.microsoft.com/javascript/api/@azure/arm-cognitiveservices/?view=azure-node-latest)
* [Was ist Azure Cognitive Services?](../../Welcome.md)
* [Authentifizieren von Anforderungen an Azure Cognitive Services](../../authentication.md)
* [Erstellen einer neuen Ressource über das Azure-Portal](../../cognitive-services-apis-create-account.md)