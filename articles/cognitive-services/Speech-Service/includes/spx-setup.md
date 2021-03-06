---
author: v-demjoh
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 05/15/2020
ms.author: v-demjoh
ms.openlocfilehash: c92d6569e3c92d3bad3575599283c7796bd78225
ms.sourcegitcommit: d8b8768d62672e9c287a04f2578383d0eb857950
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2020
ms.locfileid: "88068614"
---
## <a name="prerequisites"></a>Voraussetzungen

Die einzige Voraussetzung ist ein Azure Speech-Abonnement. Informationen zum Erstellen eines neuen Abonnements – falls Sie noch keins besitzen – finden Sie im [Handbuch](../get-started.md#new-resource).

## <a name="download-and-install"></a>Herunterladen und Installieren

#### <a name="windows-install"></a>[Windows-Installation](#tab/windowsinstall)

Führen Sie diese Schritte aus, um die Speech-Befehlszeilenschnittstelle unter Windows zu installieren:

1. Installieren Sie entweder [.NET Framework 4.7](https://dotnet.microsoft.com/download/dotnet-framework/net471) oder [.NET Core 3.0](https://dotnet.microsoft.com/download/dotnet-core/3.0)
2. Laden Sie das [Zip-Archiv](https://aka.ms/speech/spx-zips.zip) der Speech-Befehlszeilenschnittstelle herunter, und extrahieren Sie es.
3. Wechseln Sie in das Stammverzeichnis `spx-zips`, das Sie aus dem Download extrahiert haben, und extrahieren Sie das benötigte Unterverzeichnis (`spx-net471` für .NET Framework 4.7 oder `spx-netcore-win-x64` für .NET Core 3.0 auf einer x64-CPU).

Wechseln Sie an der Eingabeaufforderung das Verzeichnis zu diesem Speicherort, und geben Sie dann `spx` ein, um die Hilfe zur Speech-Befehlszeilenschnittstelle anzuzeigen.

> [!NOTE]
> Unter Windows können von der Speech-Befehlszeilenschnittstelle nur Schriftarten angezeigt werden, die für die Eingabeaufforderung auf dem lokalen Computer verfügbar sind.
> Das [Windows-Terminal](https://www.microsoft.com/en-us/p/windows-terminal/9n0dx20hk701) unterstützt alle Schriftarten, die von der Speech-Befehlszeilenschnittstelle interaktiv generiert werden.
> Bei der Ausgabe einer Datei können in einem Text-Editor wie Notepad oder einem Webbrowser wie Microsoft Edge ebenfalls alle Schriftarten angezeigt werden.

> [!NOTE]
> PowerShell prüft bei der Suche nach einem Befehl nicht das lokale Verzeichnis. Ändern Sie in Powershell das Verzeichnis zum Speicherort von `spx`, und rufen Sie das Tool auf, indem Sie `.\spx` eingeben.
> Wenn Sie dieses Verzeichnis Ihrem Pfad hinzufügen, finden Powershell und die Windows-Eingabeaufforderung `spx` von jedem Verzeichnis aus auch ohne enthaltenes Präfix `.\`.

#### <a name="linux-install"></a>[Linux-Installation](#tab/linuxinstall)

Führen Sie diese Schritte aus, um die Speech-Befehlszeilenschnittstelle unter Linux auf einer x64-CPU zu installieren:

1. Installieren Sie [.NET Core 3.0](https://dotnet.microsoft.com/download/dotnet-core/3.0).
2. Laden Sie das [Zip-Archiv](https://aka.ms/speech/spx-zips.zip) der Speech-Befehlszeilenschnittstelle herunter, und extrahieren Sie es.
3. Wechseln Sie zum Stammverzeichnis `spx-zips`, das Sie aus dem Download extrahiert haben, und extrahieren Sie `spx-netcore-30-linux-x64` in ein neues Verzeichnis `~/spx`.
4. Geben Sie an einem Terminal die folgenden Befehle ein:
   1. `cd ~/spx`
   2. `sudo chmod +r+x spx`
   3. `PATH=~/spx:$PATH`

Geben Sie `spx` ein, um die Hilfe für die Speech-Befehlszeilenschnittstelle anzuzeigen.

#### <a name="docker-install"></a>[Docker-Installation](#tab/dockerinstall)

Führen Sie die folgenden Schritte aus, um die Speech-CLI in einem Docker-Container zu installieren:

1. Installieren Sie [Docker Desktop für Ihre Plattform](https://www.docker.com/get-started), und führen Sie die Anwendung aus.
1. Geben Sie an einer neuen Eingabeaufforderung oder in einem Terminal den folgenden Befehl ein: `docker pull msftspeech/spx`
1. Geben Sie diesen Befehl ein. Es sollten Hilfeinformationen für die Speech-CLI angezeigt werden: `docker run -it --rm msftspeech/spx help`

### <a name="mount-a-directory-in-the-container"></a>Einbinden eines Verzeichnisses in den Container

Mit dem Tool „Speech-CLI“ werden Konfigurationseinstellungen als Dateien gespeichert, die beim Ausführen eines Befehls dann geladen werden (mit Ausnahme von Hilfebefehlen).
Bei Verwendung der Speech-CLI in einem Docker-Container müssen Sie ein lokales Verzeichnis aus dem Container einbinden. Das Tool kann so die Konfigurationseinstellungen speichern bzw. finden und darüber hinaus alle Dateien lesen oder schreiben, die der Befehl benötigt, z. B. Audiodateien mit Sprache.

Geben Sie diesen Befehl unter Windows ein, um ein lokales Verzeichnis zu erstellen, das von der Speech-CLI aus dem Container verwendet werden kann:

`mkdir c:\spx-data`

Geben Sie unter Linux oder Mac den folgenden Befehl in einem Terminal ein, um ein Verzeichnis zu erstellen und dessen absoluten Pfad anzuzeigen:

```bash
mkdir ~/spx-data
cd ~/spx-data
pwd
```

Sie verwenden den absoluten Pfad beim Aufrufen der Speech-CLI.

### <a name="run-speech-cli-in-the-container"></a>Ausführen der Speech-CLI im Container

In dieser Dokumentation wird der Befehl `spx` der Speech-CLI angezeigt, der in nicht zu Docker gehörenden Installationen verwendet wird.
Beim Aufrufen des Befehls `spx` in einem Docker-Container müssen Sie ein Verzeichnis in den Container für Ihr Dateisystem einbinden, in dem die Speech-CLI Konfigurationswerte speichern und finden und Dateien lesen und schreiben kann.
Unter Windows werden Ihre Befehle wie folgt gestartet:

`docker run -it -v c:\spx-data:/data --rm msftspeech/spx`

Unter Linux oder Mac werden Ihre Befehle in etwa wie folgt gestartet:

`sudo docker run -it -v /ABSOLUTE_PATH:/data --rm msftspeech/spx`

> [!NOTE]
> Ersetzen Sie `/ABSOLUTE_PATH` durch den absoluten Pfad im obigen Abschnitt, der mit dem Befehl `pwd` angezeigt wird.

Geben Sie zum Verwenden des in einem Container installierten Befehls `spx` immer den vollständigen Befehl (wie oben gezeigt) ein, gefolgt von den Parametern Ihrer Anforderung.
Unter Windows wird Ihr Schlüssel mit diesem Befehl beispielsweise wie folgt festgelegt:

`docker run -it -v c:\spx-data:/data --rm msftspeech/spx config @key --set SUBSCRIPTION-KEY`

> [!NOTE]
> Sie können das Mikrofon oder den Lautsprecher Ihres Computers nicht verwenden, wenn Sie die Speech-CLI in einem Docker-Container ausführen.
> Übergeben Sie zur Verwendung dieser Geräte Audiodateien für die Speech-CLI, um die Aufzeichnung bzw. Wiedergabe außerhalb des Docker-Containers durchzuführen.
> Mit dem Tool „Speech-CLI“ können Sie auf das lokale Verzeichnis zugreifen, das Sie mit den obigen Schritten eingerichtet haben.

***

## <a name="create-subscription-config"></a>Erstellen der Abonnementkonfiguration

Um mit der Verwendung der Speech-Befehlszeilenschnittstelle zu beginnen, müssen Sie zuerst Ihren Speech-Abonnementschlüssel und die Regionsinformationen eingeben. Informationen zur Ermittlung Ihres Regionsbezeichners finden Sie auf der Seite zur [Unterstützung von Regionen](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#speech-sdk). Nachdem Sie Ihren Abonnementschlüssel und den Regionsbezeichner (z. B. `eastus`, `westus`) erhalten haben, führen Sie die folgenden Befehle aus.

```shell
spx config @key --set SUBSCRIPTION-KEY
spx config @region --set REGION
```

Ihre Abonnementauthentifizierung wird jetzt für zukünftige SPX-Anforderungen gespeichert. Wenn Sie einen dieser gespeicherten Werte entfernen müssen, führen Sie `spx config @region --clear` oder `spx config @key --clear` aus.
