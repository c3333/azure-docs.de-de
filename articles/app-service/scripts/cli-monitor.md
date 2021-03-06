---
title: 'Über die CLI: Überwachen einer Web-App mit Webserverprotokollen'
description: Hier erfahren Sie, wie Sie die Azure CLI zum Automatisieren der Bereitstellung und Verwaltung Ihrer App Service-App verwenden. Dieses Beispiel zeigt, wie Sie eine App mit Webserverprotokollen überwachen.
author: msangapu-msft
tags: azure-service-management
ms.assetid: 0887656f-611c-4627-8247-b5cded7cef60
ms.devlang: azurecli
ms.topic: sample
ms.date: 12/11/2017
ms.author: msangapu
ms.custom: mvc, seodec18, devx-track-azurecli
ms.openlocfilehash: b817f3d903970f32bf5ceca466aca9f619a0c1d3
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2020
ms.locfileid: "87495470"
---
# <a name="monitor-an-app-service-appwith-web-server-logs-using-azure-cli"></a>Überwachen einer App Service-App mit Webserverprotokollen per Azure CLI

Dieses Beispielskript erstellt eine Ressourcengruppe, einen App Service-Plan und eine App und konfiguriert die App für die Aktivierung der Webserverprotokolle. Anschließend werden die Protokolldateien zur Überprüfung heruntergeladen.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, benötigen Sie die Azure CLI-Version 2.0 oder höher. Führen Sie `az --version` aus, um die Version zu finden. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI]( /cli/azure/install-azure-cli).

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/monitor-with-logs/monitor-with-logs.sh "Monitor Logs")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, eine App Service-App und alle zugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [`az group create`](/cli/azure/group?view=azure-cli-latest#az-group-create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [`az appservice plan create`](/cli/azure/appservice/plan?view=azure-cli-latest#az-appservice-plan-create) | Erstellt einen App Service-Plan. |
| [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) | Erstellt eine App Service-App. |
| [`az webapp log config`](/cli/azure/webapp/log?view=azure-cli-latest#az-webapp-log-config) | Konfiguriert, welche Protokolle von einer App Service-App dauerhaft gespeichert werden. |
| [`az webapp log download`](/cli/azure/webapp/log?view=azure-cli-latest#az-webapp-log-download) | Lädt die Protokolle einer App Service-App auf Ihren lokalen Computer herunter. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).

Zusätzliche App Service-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure App Service](../samples-cli.md).
