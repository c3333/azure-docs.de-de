---
title: Überwachen des Azure Data Box Gateway-Geräts | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie über das Azure-Portal und die lokale Webbenutzeroberfläche Azure Data Box Gateway überwachen.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: how-to
ms.date: 04/15/2019
ms.author: alkohli
ms.openlocfilehash: e352c2d8141f90c4dc721fe4fc0e717dfad7b88b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84339041"
---
# <a name="monitor-your-azure-data-box-gateway"></a>Überwachen des Azure Data Box Gateway-Geräts

In diesem Artikel wird beschrieben, wie Sie Ihr Azure Data Box Gateway-Gerät überwachen. Zum Überwachen Ihres Geräts können Sie das Azure-Portal oder die lokale Webbenutzeroberfläche verwenden. Verwenden Sie das Azure-Portal, um Geräteereignisse anzuzeigen, Warnungen zu konfigurieren und zu verwalten und Metriken anzuzeigen.

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
>
> * Anzeigen von Geräteereignissen und den entsprechenden Warnungen
> * Anzeigen von Kapazitäts- und Transaktionsmetriken für Ihr Gerät
> * Konfigurieren und Verwalten von Warnungen

## <a name="view-device-events"></a>Anzeigen von Geräteereignissen

[!INCLUDE [Supported OS for clients connected to device](../../includes/data-box-edge-gateway-view-device-events.md)]

## <a name="view-metrics"></a>Anzeigen von Metriken

[!INCLUDE [Supported OS for clients connected to device](../../includes/data-box-edge-gateway-view-metrics.md)]

### <a name="metrics-on-your-device"></a>Metriken auf Ihrem Gerät

In diesem Abschnitt werden die Überwachungsmetriken auf Ihrem Gerät beschrieben. Folgende Metriken sind möglich:

* Kapazitätsmetriken. Die Kapazitätsmetriken beziehen sich auf die Kapazität des Geräts.

* Transaktionsmetriken. Die Transaktionsmetriken beziehen sich auf die Lese- und Schreibvorgänge in Azure Storage.

Eine vollständige Liste der Metriken enthält die folgende Tabelle:

|Kapazitätsmetriken                     |BESCHREIBUNG  |
|-------------------------------------|-------------|
|**Verfügbare Kapazität**               | Bezieht sich auf die Menge der Daten, die auf das Gerät geschrieben werden können. Das bedeutet, dass dies die Kapazität ist, die auf dem Gerät zur Verfügung gestellt werden kann. <br></br>Sie können die Gerätekapazität freigeben, indem Sie die lokale Kopie von Dateien löschen, die sowohl auf dem Gerät als auch in der Cloud über eine Kopie verfügen.        |
|**Gesamtkapazität**                   | Bezieht sich auf die Gesamtzahl der Bytes auf dem Gerät, auf das Daten geschrieben werden sollen. Dies wird auch als Gesamtgröße des lokalen Caches bezeichnet. <br></br> Sie können jetzt die Kapazität eines vorhandenen virtuellen Geräts erhöhen, indem Sie einen Datenträger hinzufügen. Fügen Sie einen Datenträger über die Hypervisor-Verwaltung für den virtuellen Computer hinzu, und starten Sie den virtuellen Computer neu. Der lokale Speicherpool des Gatewaygeräts wird erweitert, um den neu hinzugefügten Datenträger aufzunehmen. <br></br>Weitere Informationen finden Sie unter [Hinzufügen einer Festplatte für virtuelle Hyper-V-Computer](https://www.youtube.com/watch?v=EWdqUw9tTe4). |

|Transaktionsmetriken              | BESCHREIBUNG         |
|-------------------------------------|---------|
|**Hochgeladene Cloudbytes (Gerät)**    | Summe aller Bytes, die über alle Freigaben auf Ihrem Gerät hochgeladen wurden        |
|**Hochgeladene Cloudbytes (Freigabe)**     | Hochgeladene Bytes pro Freigabe Infrage kommt: <br></br> Avg, d. h. (Summe aller hochgeladenen Bytes pro Freigabe/Anzahl der Freigaben)  <br></br>Max, d. h. die maximale Anzahl von Bytes, die von einer Freigabe hochgeladen wurden <br></br>Min, d. h. die minimale Anzahl von Bytes, die von einer Freigabe hochgeladen wurden      |
|**Clouddownloaddurchsatz (Freigabe)**| Heruntergeladene Bytes pro Freigabe Infrage kommt: <br></br> Avg, d. h. (Summe aller von einer Freigabe gelesenen oder heruntergeladenen Bytes/Anzahl der Freigaben) <br></br> Max, d. h. die maximale Anzahl von Bytes, die von einer Freigabe heruntergeladen wurden<br></br> Min, d. h. die minimale Anzahl von Bytes, die von einer Freigabe heruntergeladen wurden  |
|**Cloudlesedurchsatz**            | Summe aller Bytes, die über alle Freigaben auf Ihrem Gerät aus der Cloud gelesen wurden     |
|**Clouduploaddurchsatz**          | Summe aller Bytes, die über alle Freigaben auf Ihrem Gerät in die Cloud geschrieben wurden     |
|**Clouduploaddurchsatz (Freigabe)**  | Summe aller Bytes, die von einer Freigabe in die Cloud geschrieben wurden/Anzahl der Freigaben ist durchschnittlich, maximal und minimal pro Freigabe      |
|**Lesedurchsatz (Netzwerk)**           | Enthält den Systemnetzwerkdurchsatz für alle aus der Cloud gelesenen Bytes Diese Ansicht kann Daten enthalten, die nicht auf Freigaben beschränkt sind. <br></br>Die Aufteilung zeigt den Datenverkehr über alle Netzwerkadapter auf dem Gerät an. Dazu gehören auch Adapter, die nicht verbunden oder aktiviert sind.      |
|**Schreibdurchsatz (Netzwerk)**       | Enthält den Systemnetzwerkdurchsatz für alle in die Cloud geschriebenen Bytes Diese Ansicht kann Daten enthalten, die nicht auf Freigaben beschränkt sind. <br></br>Die Aufteilung zeigt den Datenverkehr über alle Netzwerkadapter auf dem Gerät an. Dazu gehören auch Adapter, die nicht verbunden oder aktiviert sind.          |

## <a name="manage-alerts"></a>Warnungen verwalten

[!INCLUDE [Supported OS for clients connected to device](../../includes/data-box-edge-gateway-manage-alerts.md)]

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [Bandbreite verwalten](data-box-gateway-manage-bandwidth-schedules.md).
