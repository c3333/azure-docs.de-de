---
title: 'Bedingter Zugriff: Blockieren des Zugriffs – Azure Active Directory'
description: Erstellen einer benutzerdefinierten Richtlinie für bedingten Zugriff
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: how-to
ms.date: 05/26/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb,
ms.collection: M365-identity-device-management
ms.openlocfilehash: b3ee7287f2a5cf9491ae91d434caf2f653c853a3
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "83995308"
---
# <a name="conditional-access-block-access"></a>Bedingter Zugriff: Zugriff blockieren

Für Organisationen mit einem konservativen Cloudmigrationsansatz ist die Richtlinie „Alle blockieren“ eine mögliche Option. 

> [!CAUTION]
> Die Fehlkonfiguration einer Blockierungsrichtlinie kann dazu führen, dass Organisationen aus dem Azure-Portal ausgesperrt werden.

Derartige Richtlinien können unbeabsichtigte Nebeneffekte haben. Ordnungsgemäße Tests und Validierung sind vor dem Aktivieren unerlässlich. Administratoren sollten bei Änderungen Tools wie den Modus [Nur Bericht](concept-conditional-access-report-only.md) und das [Was-wäre-wenn-Tool](what-if-tool.md) für den bedingten Zugriff verwenden.

## <a name="user-exclusions"></a>Ausschluss von Benutzern

Richtlinien für bedingten Zugriff sind leistungsstarke Tools, daher wird empfohlen, die folgenden Konten von Ihrer Richtlinie auszuschließen:

* **Notfallzugriffs**- oder **Break-Glass**-Konten, um eine mandantenweite Kontosperrung zu vermeiden. In dem unwahrscheinlichen Fall, dass alle Administratoren aus dem Mandanten ausgeschlossen sind, können Sie sich mit Ihrem Administratorkonto für den Notfallzugriff beim Mandanten anmelden und Maßnahmen ergreifen, um den Zugriff wiederherzustellen.
   * Weitere Informationen finden Sie im Artikel [Verwalten von Konten für den Notfallzugriff in Azure AD](../users-groups-roles/directory-emergency-access.md).
* **Dienstkonten** und **Dienstprinzipale**, z. B. das Konto für die Azure AD Connect-Synchronisierung. Dienstkonten sind nicht interaktive Konten, die an keinen bestimmten Benutzer gebunden sind. Sie werden normalerweise von Back-End-Diensten verwendet, die den programmgesteuerten Zugriff auf Anwendungen ermöglichen, können aber auch zu Verwaltungszwecken für die Anmeldung bei Systemen verwendet werden. Derartige Dienstkonten sollten ausgeschlossen werden, weil die MFA nicht programmgesteuert abgeschlossen werden kann. Aufrufe von Dienstprinzipalen werden durch den bedingten Zugriff nicht blockiert.
   * Wenn Ihre Organisation diese Konten in Skripts oder Code verwendet, sollten Sie in Betracht ziehen, diese durch [verwaltete Identitäten](../managed-identities-azure-resources/overview.md) zu ersetzen. Als vorübergehende Problemumgehung können Sie diese spezifischen Konten aus der Basisrichtlinie ausschließen.

## <a name="create-a-conditional-access-policy"></a>Erstellen der Richtlinie für bedingten Zugriff

Mit den folgenden Schritte können Sie Richtlinien für den bedingten Zugriff erstellen, um den Zugriff auf alle Apps (mit Ausnahme von [Office 365](concept-conditional-access-cloud-apps.md#office-365-preview)) zu blockieren, wenn sich Benutzer nicht in einem vertrauenswürdigen Netzwerk befinden. Für diese Richtlinien wird zu Beginn der Modus [Nur Bericht](howto-conditional-access-report-only.md) festgelegt, damit Administratoren die Auswirkungen auf vorhandene Benutzer ermitteln können. Wenn die Administratoren sicher sind, dass die Richtlinien erwartungsgemäß funktionieren, können sie die Richtlinien **aktivieren**.

Die erste Richtlinie blockiert den Zugriff auf alle Apps mit Ausnahme von Office 365-Anwendungen, wenn sie sich nicht an einem vertrauenswürdigen Standort befinden.

1. Melden Sie sich beim **Azure-Portal** als globaler Administrator, Sicherheitsadministrator oder Administrator für bedingten Zugriff an.
1. Navigieren Sie zu **Azure Active Directory** > **Sicherheit** > **Bedingter Zugriff**.
1. Wählen Sie **Neue Richtlinie**.
1. Benennen Sie Ihre Richtlinie. Es wird empfohlen, dass Unternehmen einen aussagekräftigen Standard für die Namen ihrer Richtlinien erstellen.
1. Klicken Sie unter **Zuweisungen** auf **Benutzer und Gruppen**.
   1. Wählen Sie unter **Einschließen** die Option **Alle Benutzer** aus.
   1. Wählen Sie unter **Ausschließen** die Option **Benutzer und Gruppen** und dann die Konten für den Notfallzugriff Ihres Unternehmens aus. 
   1. Wählen Sie **Fertig**aus.
1. Wählen Sie unter **Cloud-Apps oder -Aktionen** die folgenden Optionen aus:
   1. Wählen Sie unter **Einschließen** die Option **Alle Cloud-Apps** aus.
   1. Wählen Sie unter **Ausschließen** die Option **Office 365 (Vorschau)** aus, und wählen Sie **Auswählen** und dann **Fertig** aus.
1. Unter **Bedingungen**:
   1. Unter **Bedingungen** > **Standort**.
      1. Legen Sie **Konfigurieren** auf **Ja** fest.
      1. Wählen Sie unter **Einschließen** die Option **Alle Standorte** aus.
      1. Wählen Sie unter **Ausschließen** die Option **Alle vertrauenswürdigen Standorte** aus.
      1. Wählen Sie **Fertig**aus.
   1. Legen Sie unter **Client-Apps (Vorschau)** die Option **Konfigurieren** auf **Ja** fest, und wählen Sie **Fertig** und dann noch einmal **Fertig** aus.
1. Wählen Sie unter **Zugriffssteuerung** > **Erteilen** die Option **Zugriff blockieren** und dann **Auswählen** aus.
1. Bestätigen Sie die Einstellungen, und legen Sie **Richtlinie aktivieren** auf **Nur Bericht** fest.
1. Wählen Sie **Erstellen** aus, um die Richtlinie zu erstellen und zu aktivieren.

Nachfolgend wird eine zweite Richtlinie erstellt, die für Benutzer von Office 365 die mehrstufige Authentifizierung (MFA) oder ein kompatibles Gerät voraussetzt.

1. Wählen Sie **Neue Richtlinie**.
1. Benennen Sie Ihre Richtlinie. Es wird empfohlen, dass Unternehmen einen aussagekräftigen Standard für die Namen ihrer Richtlinien erstellen.
1. Klicken Sie unter **Zuweisungen** auf **Benutzer und Gruppen**.
   1. Wählen Sie unter **Einschließen** die Option **Alle Benutzer** aus.
   1. Wählen Sie unter **Ausschließen** die Option **Benutzer und Gruppen** und dann die Konten für den Notfallzugriff Ihres Unternehmens aus. 
   1. Wählen Sie **Fertig**aus.
1. Wählen Sie unter **Cloud-Apps oder -Aktionen** > **Einschließen** die Option **Apps auswählen** aus. Wählen Sie dann **Office 365 (Vorschau)** und anschließend **Auswählen** sowie **Fertig** aus.
1. Wählen Sie unter **Zugriffssteuerung** > **Erteilen** die Option **Zugriff gewähren** aus.
   1. Wählen Sie **Mehrstufige Authentifizierung erforderlich** und **Markieren des Geräts als kompatibel erforderlich** und dann **Auswählen** aus.
   1. Stellen Sie sicher, dass **Alle ausgewählten Steuerungen anfordern** ausgewählt ist.
   1. Wählen Sie **Auswählen**.
1. Bestätigen Sie die Einstellungen, und legen Sie **Richtlinie aktivieren** auf **Nur Bericht** fest.
1. Wählen Sie **Erstellen** aus, um die Richtlinie zu erstellen und zu aktivieren.

## <a name="next-steps"></a>Nächste Schritte

[Allgemeine Richtlinien für bedingten Zugriff](concept-conditional-access-policy-common.md)

[Bestimmen der Auswirkung durch Verwendung des reinen Berichtsmodus des bedingten Zugriffs](howto-conditional-access-report-only.md)

[Simulieren des Anmeldeverhaltens mit dem Was-wäre-wenn-Tool für den bedingten Zugriff](troubleshoot-conditional-access-what-if.md)
