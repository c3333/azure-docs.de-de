---
title: Was ist die Speicherhierarchie von Azure NetApp Files? | Microsoft-Dokumentation
description: Enthält Informationen zur Speicherhierarchie (einschließlich Azure NetApp Files-Konten, Kapazitätspools und Volumes).
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 08/10/2020
ms.author: b-juche
ms.openlocfilehash: 91fecbc68efec1adcee9a2c4013dea46f6da86af
ms.sourcegitcommit: d8b8768d62672e9c287a04f2578383d0eb857950
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2020
ms.locfileid: "88066473"
---
# <a name="what-is-the-storage-hierarchy-of-azure-netapp-files"></a>Was ist die Speicherhierarchie von Azure NetApp Files?

Vor der Erstellung eines Volumes in Azure NetApp Files müssen Sie zunächst einen Pool für bereitgestellte Kapazität erwerben und einrichten.  Um einen Kapazitätspool einrichten zu können, benötigen Sie ein NetApp-Konto. Die Informationen zur Speicherhierarchie sind beim Einrichten und Verwalten Ihrer Azure NetApp Files-Ressourcen hilfreich.

> [!IMPORTANT] 
> Azure NetApp Files unterstützt derzeit keine Ressourcenmigration zwischen Abonnements.

## <a name="netapp-accounts"></a><a name="azure_netapp_files_account"></a>NetApp-Konten

- Ein NetApp-Konto fungiert als administrative Gruppierung der einzelnen Kapazitätspools.  
- Ein NetApp-Konto ist nicht dasselbe wie Ihr allgemeines Azure-Speicherkonto. 
- Ein NetApp-Konto deckt einen regionalen Bereich ab.   
- Sie können über mehrere NetApp-Konten in einer Region verfügen, jedes NetApp-Konto ist aber an eine einzelne Region gebunden.

## <a name="capacity-pools"></a><a name="capacity_pools"></a>Kapazitätspools

- Ein Kapazitätspool wird anhand der bereitgestellten Kapazität gemessen.  
- Die Kapazität wird durch die festen SKUs bereitgestellt, die Sie erworben haben (beispielsweise eine Kapazität von 4 TiB).
- Ein Kapazitätspool kann lediglich einen einzelnen Servicelevel aufweisen.  
- Jeder einzelne Kapazitätspool kann nur zu einem einzelnen NetApp-Konto gehören. Innerhalb eines NetApp-Kontos können jedoch mehrere Kapazitätspools vorhanden sein.  
- Ein Kapazitätspool kann nicht zwischen NetApp-Konten verschoben werden.   
  Im [Konzeptdiagramm der Speicherhierarchie](#conceptual_diagram_of_storage_hierarchy) weiter unten kann beispielsweise der Kapazitätspool 1 nicht aus dem NetApp-Konto für „USA, Osten“ in das NetApp-Konto für „USA, Westen 2“ verschoben werden.  
- Ein Kapazitätspool kann erst gelöscht werden, wenn alle Volumes innerhalb des Kapazitätspools gelöscht wurden.

## <a name="volumes"></a><a name="volumes"></a>Volumes

- Ein Volume wird anhand des logischen Kapazitätsverbrauchs gemessen und ist skalierbar. 
- Der Kapazitätsverbrauch eines Volumes wird mit der bereitgestellten Kapazität des dazugehörigen Pools verrechnet.
- Jedes Volume gehört zu einem einzelnen Pool, ein Pool kann jedoch mehrere Volumes enthalten. 

## <a name="conceptual-diagram-of-storage-hierarchy"></a><a name="conceptual_diagram_of_storage_hierarchy"></a>Konzeptdiagramm der Speicherhierarchie 
Das folgende Beispiel zeigt die Beziehungen zwischen Azure-Abonnement, NetApp-Konten, Kapazitätspools und Volumes.   

![Konzeptdiagramm der Speicherhierarchie](../media/azure-netapp-files/azure-netapp-files-storage-hierarchy.png)

## <a name="next-steps"></a>Nächste Schritte

- [Ressourcenlimits für Azure NetApp Files](azure-netapp-files-resource-limits.md)
- [Registrieren für Azure NetApp Files](azure-netapp-files-register.md)
