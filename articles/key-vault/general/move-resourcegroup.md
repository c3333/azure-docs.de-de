---
title: 'Azure Key Vault: Verschieben eines Tresors in eine andere Ressourcengruppe | Microsoft-Dokumentation'
description: Anleitung zum Verschieben eines Schlüsseltresors in eine andere Ressourcengruppe.
services: key-vault
author: ShaneBala-keyvault
manager: ravijan
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: how-to
ms.date: 04/29/2020
ms.author: sudbalas
Customer intent: As a key vault administrator, I want to move my vault to another resource group.
ms.openlocfilehash: fe8051d551077666c06ac033f22303fd643ac602
ms.sourcegitcommit: 02ca0f340a44b7e18acca1351c8e81f3cca4a370
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2020
ms.locfileid: "88585732"
---
# <a name="moving-an-azure-key-vault-across-resource-groups"></a>Verschieben einer Azure Key Vault-Instanz zwischen Ressourcengruppen

## <a name="overview"></a>Übersicht

Das Verschieben eines Schlüsseltresors zwischen Ressourcengruppen ist eine unterstützte Funktion von Key Vault. Das Verschieben eines Schlüsseltresors zwischen Ressourcengruppen wirkt sich nicht auf die Konfigurationen von Key Vault-Firewall oder Zugriffsrichtlinien aus. Verbundene Anwendungen und Dienstprinzipale sollten weiterhin wie vorgesehen funktionieren.

## <a name="design-considerations"></a>Entwurfsaspekte

Ihre Organisation hat möglicherweise Azure Policy mit Erzwingung oder Ausschlüssen auf Ressourcengruppenebene implementiert. In der Ressourcengruppe, in der der Schlüsseltresor derzeit enthalten ist, und in der Ressourcengruppe, in die Sie den Schlüsseltresor verschieben, gilt möglicherweise ein anderer Satz von Richtlinienzuweisungen. Ein Konflikt in den Richtlinienanforderungen kann Ihre Anwendungen potenziell unterbrechen.

### <a name="example"></a>Beispiel

Sie verfügen über eine Anwendung, die mit dem Schlüsseltresor, der Zertifikate erstellt, verbunden ist. Die Zertifikate sind zwei Jahre gültig. Die Ressourcengruppe, in die Sie den Schlüsseltresor verschieben möchten, verfügt über eine Richtlinienzuweisung, die die Erstellung von Zertifikaten mit einer Gültigkeit von mehr als einem Jahr blockiert. Nachdem Sie Ihren Schlüsseltresor in die neue Ressourcengruppe verschoben haben, wird der Vorgang zum Erstellen eines Zertifikats, das zwei Jahre lang gültig ist, durch eine Azure Policy-Zuweisung blockiert.

### <a name="solution"></a>Lösung

Überprüfen Sie auf der Seite „Azure Policy“ im Azure-Portal die Richtlinienzuweisungen für Ihre aktuelle Ressourcengruppe und die Zielressourcengruppe, und stellen Sie sicher, dass keine Konflikte bestehen.

## <a name="procedure"></a>Verfahren

1. Anmelden beim Azure-Portal
2. Navigieren zum Schlüsseltresor
3. Klicken auf die Registerkarte „Übersicht“
4. Auswählen der Schaltfläche „Verschieben“
5. Auswählen der Option „In eine andere Ressourcengruppe verschieben“ im Dropdownmenü
6. Auswählen der Ressourcengruppe, in die der Schlüsseltresor verschoben werden soll
7. Bestätigen der Warnung zum Verschieben von Ressourcen
8. Auswählen von „OK“

Key Vault überprüft nun die Gültigkeit des Verschiebens der Ressourcen und warnt Sie bei Fehlern. Wenn keine Fehler gefunden werden, wird das Verschieben der Ressourcen abgeschlossen. 
