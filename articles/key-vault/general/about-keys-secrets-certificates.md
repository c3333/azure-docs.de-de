---
title: Informationen zu Schlüsseln, Geheimnissen und Zertifikaten im Azure Key Vault
description: Hier finden Sie eine Übersicht über die Azure Key Vault-REST-Schnittstelle sowie Informationen für Entwickler zu Schlüsseln, Geheimnissen und Zertifikaten.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.topic: overview
ms.date: 04/17/2020
ms.author: mbaldwin
ms.openlocfilehash: cb8a29c5d2eff46eecb2cf977bfb492f28731e68
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87043634"
---
# <a name="about-keys-secrets-and-certificates"></a>Informationen zu Schlüsseln, Geheimnissen und Zertifikaten

Microsoft Azure-Anwendungen und -Benutzer können verschiedene Arten von Geheimnissen und Schlüsseldaten in Azure Key Vault speichern:

- Kryptografische Schlüssel: Unterstützen mehrere Schlüsseltypen und Algorithmen und ermöglicht die Verwendung von Hardwaresicherheitsmodulen (Hardware Security Modules, HSM) für Schlüssel von hohem Wert. Weitere Informationen finden Sie unter [Informationen zu Azure Key Vault-Schlüsseln](../keys/about-keys.md).
- Geheimnisse: Bieten einen sicheren Speicher für Geheimnisse wie Kennwörter und Datenbank-Verbindungszeichenfolgen. Weitere Informationen finden Sie unter [Informationen zu Azure Key Vault-Geheimnissen](../secrets/about-secrets.md).
- Certificates: Unterstützen Zertifikate, die auf Schlüsseln und Geheimnissen aufbauen, und fügt ein Feature für die automatisierte Verlängerung hinzu. Weitere Informationen finden Sie unter [Informationen zu Azure Key Vault-Zertifikaten](../certificates/about-certificates.md).
- Azure Storage: Kann die Schlüssel eines Azure Storage-Kontos für Sie verwalten. Intern kann Key Vault Schlüssel für ein Azure Storage-Konto auflisten (synchronisieren) und die Schlüssel in regelmäßigen Abständen erneut generieren (rotieren). Weitere Informationen finden Sie unter [Verwalten von Speicherkontoschlüsseln mit Key Vault und der Azure-Befehlszeilenschnittstelle](../secrets/overview-storage-keys.md).

Weitere allgemeine Informationen zu Key Vault finden Sie unter [Informationen zu Azure Key Vault](overview.md).

## <a name="data-types"></a>Datentypen

In den JOSE-Spezifikationen finden Sie relevante Datentypen für Schlüssel, Verschlüsselung und Signatur.  

-   **algorithm**: ein unterstützter Algorithmus für einen Schlüsselvorgang, z.B. RSA1_5.  
-   **ciphertext-value**: Verschlüsselungstextoktette, codiert mit Base64URL.  
-   **digest-value**: die Ausgabe eines Hashalgorithmus, codiert mit Base64URL.  
-   **key-type**: einer der unterstützten Schlüsseltypen, z.B. RSA (Rivest-Shamir-Adleman).  
-   **plaintext-value**: Klartextoktette, codiert mit Base64URL.  
-   **signature-value**: die Ausgabe eines Signaturalgorithmus, codiert mit Base64URL.  
-   **base64URL**: ein mit Base64URL [RFC4648] codierter Binärwert.  
-   **boolean**: entweder TRUE oder FALSE.  
-   **Identity**: eine Identität in Azure Active Directory (AAD).  
-   **IntDate**: ein dezimaler JSON-Wert, der die Anzahl von Sekunden von 1970-01-01T0:0:0Z UTC bis zum angegebenen UTC-Datum / zur angegebenen UTC-Uhrzeit darstellt. Details in Bezug auf Datum/Uhrzeit im Allgemeinen und UTC im Besonderen finden Sie der Dokumentation zu RFC 3339.  

## <a name="objects-identifiers-and-versioning"></a>Objekte, Bezeichner und Versionsverwaltung

In Key Vault gespeicherte Objekte werden versioniert, wenn eine neue Instanz eines Objekts erstellt wird. Jeder Version wird ein eindeutiger Bezeichner und eine URL zugewiesen. Wenn ein Objekt zum ersten Mal erstellt wird, erhält es einen eindeutigen Versionsbezeichner und wird als aktuelle Version des Objekts gekennzeichnet. Beim Erstellen einer neuen Instanz mit dem gleichen Objektnamen erhält das neue Objekt einen eindeutigen Versionsbezeichner und wird zur aktuellen Version.  

Für die Adressierung von Objekten in Key Vault können Sie eine Version angeben oder bei Vorgängen für die aktuelle Version des Objekts die Version weglassen. Bei einem Schlüssel mit dem Namen `MasterKey` führt das Durchführen von Vorgängen ohne Angabe einer Version beispielsweise dazu, dass die neueste verfügbare Version verwendet wird. Das Ausführen von Vorgängen mit dem versionsspezifischen Bezeichner veranlasst das System, die spezifische Version des Objekts zu verwenden.  

Objekte werden in Key Vault über eine URL eindeutig identifiziert. Unabhängig vom geografischen Standort dürfen keine zwei Objekte im System die gleiche URL aufweisen. Die vollständige URL zu einem Objekt wird Objektbezeichner genannt. Die URL besteht aus einem Präfix, das den Schlüsseltresor, den Objekttyp, den vom Benutzer bereitgestellten Objektnamen und eine Objektversion identifiziert. Beim Objektnamen wird Groß-/Kleinschreibung nicht unterschieden, und er ist unveränderlich. Bezeichner, die nicht die Objektversion enthalten, heißen Basisbezeichner.  

Weitere Informationen finden Sie unter [Authentifizierung, Anforderungen und Antworten](authentication-requests-and-responses.md).

Ein Objektbezeichner hat das folgende allgemeine Format:  

`https://{keyvault-name}.vault.azure.net/{object-type}/{object-name}/{object-version}`  

Hierbei gilt:  

| Element | BESCHREIBUNG |  
|-|-|  
|`keyvault-name`|Der Name eines Schlüsseltresors im Microsoft Azure Key Vault-Dienst.<br /><br /> Schlüsseltresornamen werden vom Benutzer ausgewählt und sind global eindeutig.<br /><br /> Der Key Vault-Name muss zwischen 3 und 24 Zeichen lang sein und darf nur die Ziffern 0-9, die Buchstaben a-z und A-Z sowie das Minuszeichen („-“) enthalten.|  
|`object-type`|Die Art des Objekts (Schlüssel, Geheimnisse oder Zertifikate)|  
|`object-name`|Ein `object-name` ist ein vom Benutzer bereitgestellter Name und muss innerhalb eines Schlüsseltresors eindeutig sein. Der Name muss eine Zeichenfolge mit 1 bis 127 Zeichen sein und mit einem Buchstaben beginnen und darf nur die Zeichen „0 - 9“, „a - z“, „A - Z“ und „-“ enthalten.|  
|`object-version`|Ein `object-version` ist ein vom System generierter, 32 Zeichen langer Zeichenfolgenbezeichner, der optional verwendet wird, um eine eindeutige Version eines Objekts zu adressieren.|  

## <a name="next-steps"></a>Nächste Schritte

- [Informationen zu Schlüsseln](../keys/about-keys.md)
- [Informationen zu Geheimnissen](../secrets/about-secrets.md)
- [Informationen zu Zertifikaten](../certificates/about-certificates.md)
- [Authentifizierung, Anforderungen und Antworten](../general/authentication-requests-and-responses.md)
- [Entwicklerhandbuch für Key Vault](../general/developers-guide.md)
