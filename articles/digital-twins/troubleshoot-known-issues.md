---
title: 'Bekannte Probleme: Azure Digital Twins'
description: Hilfreiche Informationen zur Erkennung und Behebung von bekannten Problemen mit Azure Digital Twins.
author: baanders
ms.author: baanders
ms.topic: troubleshooting
ms.service: digital-twins
ms.date: 07/14/2020
ms.openlocfilehash: 9130a3248e881c9d4e2c9bfe9017f43198d50f51
ms.sourcegitcommit: 02ca0f340a44b7e18acca1351c8e81f3cca4a370
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2020
ms.locfileid: "88590165"
---
# <a name="known-issues-in-azure-digital-twins"></a>Bekannte Probleme in Azure Digital Twins

Dieser Artikel enthält Informationen zu bekannten Problemen mit Azure Digital Twins.

## <a name="400-client-error-bad-request-in-cloud-shell"></a>„400 Clientfehler: Ungültige Anforderung“ in Cloud Shell

Für Befehle in Cloud Shell kann zeitweilig der folgende Fehler auftreten: „400 Clientfehler: Ungültige Anforderung für URL: http://localhost:50342/oauth2/token„ (gefolgt von der vollständigen Stapelüberwachung).

### <a name="troubleshooting-steps"></a>Schritte zur Problembehandlung

Sie können dieses Problem beheben, indem Sie den Befehl `az login` erneut ausführen und die nachfolgenden Anmeldeschritte ausführen.

Anschließend sollten Sie wieder in der Lage sein, den Befehl auszuführen.

### <a name="possible-causes"></a>Mögliche Ursachen

Dies ist das Ergebnis eines bekannten Problems in Cloud Shell: [*Beim Abrufen des Tokens aus Cloud Shell tritt zeitweilig „400 Clientfehler: Ungültige Anforderung“ auf*](https://github.com/Azure/azure-cli/issues/11749).

## <a name="missing-role-assignment-after-scripted-setup"></a>Fehlende Rollenzuweisung nach Setup per Skript

Bei einigen Benutzern treten möglicherweise Probleme bei der Rollenzuweisung gemäß folgender Anleitung auf:[ *Einrichten einer Instanz und der Authentifizierung (per Skript)* ](how-to-set-up-instance-scripted.md). Das Skript gibt keinen Fehler an, die Rolle *Azure Digital Twins-Besitzer (Vorschau)* wird dem Benutzer jedoch nicht erfolgreich zugewiesen. Dies beeinträchtigt die Fähigkeit zum Erstellen weiterer Ressourcen.

Um zu ermitteln, ob die Rollenzuweisung nach dem Ausführen des Skripts erfolgreich eingerichtet wurde, befolgen Sie die Anweisungen im Abschnitt [*Überprüfen der Benutzerrollenzuweisung*](how-to-set-up-instance-scripted.md#verify-user-role-assignment) des Artikels zum Setup. Wenn der Benutzer nicht mit dieser Rolle angezeigt wird, wirkt sich dieses Problem auf Sie aus.

### <a name="troubleshooting-steps"></a>Schritte zur Problembehandlung

Um das Problem zu beheben, können Sie die Rollenzuweisung manuell mithilfe der CLI oder des Azure-Portals einrichten. 

Befolgen Sie diese Anweisungen:
* [BEFEHLSZEILENSCHNITTSTELLE (CLI)](how-to-set-up-instance-cli.md#set-up-user-access-permissions)
* [portal](how-to-set-up-instance-portal.md#set-up-user-access-permissions)

### <a name="possible-causes"></a>Mögliche Ursachen

Für Benutzer, die sich mit einem persönlichen [Microsoft-Konto (MSA)](https://account.microsoft.com/account) angemeldet haben, unterscheidet sich die Prinzipal-ID des Benutzers, die ihn in Befehlen wie diesem identifiziert, möglicherweise von der E-Mail-Adresse für die Anmeldung des Benutzers. Dadurch ist es für das Skript schwierig, diese zu ermitteln und die Rolle richtig zuzuweisen.

## <a name="issue-with-interactive-browser-authentication"></a>Problem bei interaktiver Browserauthentifizierung

Beim Schreiben von Authentifizierungscode in den Azure Digital Twins-Anwendungen mit der neuesten Version (Version **1.2.0**) der **[Azure.Identity](https://docs.microsoft.com/dotnet/api/azure.identity?view=azure-dotnet)-Bibliothek** treten möglicherweise Probleme mit der [InteractiveBrowserCredential](https://docs.microsoft.com/dotnet/api/azure.identity.interactivebrowsercredential?view=azure-dotnet)-Methode auf.

Die betroffene Methode wird in den folgenden Artikeln verwendet: 
* [*Tutorial: Codieren einer Client-App*](tutorial-code.md)
* [*Verwenden Schreiben von App-Authentifizierungscode*](how-to-authenticate-client.md)
* [*Verwenden der Azure Digital Twins-APIs und SDKs*](how-to-use-apis-sdks.md)

Das Problem umfasst die Fehlermeldung „Azure.Identity.AuthenticationFailedException“, wenn versucht wird, sich in einem Browserfenster zu authentifizieren. Möglicherweise wird das Browserfenster nicht vollständig gestartet, oder der Benutzer wird scheinbar erfolgreich authentifiziert, während in der Clientanwendung weiterhin der Fehler auftritt.

### <a name="troubleshooting-steps"></a>Schritte zur Problembehandlung

Um das Problem zu beheben, verwenden Sie in den Anwendungen explizit die Azure.Identity-Version **1.1.1**. Mit dieser Version der Bibliothek sollte der Browser wie erwartet geladen werden und die Authentifizierung durchführen.

>[!NOTE]
> Es genügt nicht, die Bibliothek ohne Angabe einer Version hinzuzufügen, da standardmäßig die neueste Version **1.2.0** verwendet wird. Sie müssen explizit Version **1.1.1** angeben.

### <a name="possible-causes"></a>Mögliche Ursachen

Dies ist eine Inkompatibilität zwischen Azure Digital Twins und der neuesten Version der Azure.Identity-Bibliothek, Version **1.2.0**. 

Dieses Problem tritt auf, wenn Sie in der Anwendung Version **1.2.0** verwenden oder wenn Sie dem Projekt die Bibliothek hinzufügen, ohne eine Version anzugeben (da hierdurch ebenfalls standardmäßig die neueste Version verwendet wird).

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich weiter über die Sicherheit und Berechtigungen in Azure Digital Twins:
* [*Konzepte: Sicherheit für Azure Digital Twins-Lösungen*](concepts-security.md)