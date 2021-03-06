---
title: Konfigurieren der Exportrichtlinie für ein NFS-Volume – Azure NetApp Files
description: Beschreibt, wie Sie die Exportrichtlinie zum Steuern des Zugriffs auf ein NFS-Volume mit Azure NetApp Files konfigurieren.
services: azure-netapp-files
author: b-juche
ms.author: b-juche
ms.service: azure-netapp-files
ms.workload: storage
ms.topic: how-to
ms.date: 07/27/2020
ms.openlocfilehash: 4a20a223932f82c80ad5831ef3a02bad803e26e6
ms.sourcegitcommit: 3d56d25d9cf9d3d42600db3e9364a5730e80fa4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2020
ms.locfileid: "87533201"
---
# <a name="configure-export-policy-for-an-nfs-volume"></a>Konfigurieren der Exportrichtlinie für ein NFS-Volume

Sie können die Exportrichtlinie zum Steuern des Zugriffs auf ein Azure NetApp Files-Volume konfigurieren. Die Azure NetApp Files-Exportrichtlinie unterstützt Volumes, die das NFS-Protokoll (NFSv3 sowie NFSv4.1) und ein duales Protokoll (NFSv3 und SMB) verwenden. 

Sie können bis zu fünf Exportrichtlinienregeln erstellen.

## <a name="steps"></a>Schritte 

1.  Wählen Sie auf der Seite „Volumes“ das Volume aus, für das Sie die Exportrichtlinie konfigurieren möchten, und klicken Sie auf **Exportrichtlinie**. 

    Sie können die Exportrichtlinie auch während der Erstellung des Volumes konfigurieren.

2.  Geben Sie Informationen für die folgenden Felder an, um eine Exportrichtlinienregel zu erstellen:   
    *  **Index**   
        Geben Sie die Indexnummer für die Regel an.  
        Eine Exportrichtlinie kann bis zu fünf Regeln umfassen. Regeln werden gemäß ihrer Reihenfolge in der Indexnummernliste ausgewertet. Regeln mit einer niedrigeren Indexnummer werden zuerst ausgewertet. So wird beispielsweise die Regel mit der Indexnummer 1 vor der Regel mit der Indexnummer 2 ausgewertet. 

    * **Zulässige Clients**   
        Geben Sie den Wert in einem der folgenden Formate an:  
        * IPv4-Adresse (Beispiel: `10.1.12.24`) 
        * IPv4-Adresse mit einer Subnetzmaske, ausgedrückt als Anzahl von Bits (Beispiel: `10.1.12.10/4`)

    * **zugreifen**  
        Wählen Sie einen der folgenden Berechtigungstypen aus:  
        * Kein Zugriff 
        * Lesen/Schreiben
        * Nur Leseberechtigung

    * **Nur Leseberechtigung** und **Lesen/Schreiben**  
        Wenn Sie die Kerberos-Verschlüsselung mit NFSv4.1 verwenden, befolgen Sie die Anweisungen unter [Konfigurieren der NFSv4.1-Kerberos-Verschlüsselung](configure-kerberos-encryption.md).  Informationen zu den Leistungsauswirkungen von Kerberos finden Sie unter [Leistungsauswirkungen von Kerberos auf NFSv4.1](configure-kerberos-encryption.md#kerberos_performance). 

        ![Kerberos-Sicherheitsoptionen](../media/azure-netapp-files/kerberos-security-options.png) 

    * **Root-Zugriff**  
        Geben Sie an, ob das `root`-Konto auf das Volume zugreifen kann.  Standardmäßig ist der Root-Zugriff auf **Ein** festgelegt, und das `root`-Konto hat Zugriff auf das Volume.

![Exportrichtlinie](../media/azure-netapp-files/azure-netapp-files-export-policy.png) 



## <a name="next-steps"></a>Nächste Schritte 
* [Bereitstellen oder Aufheben der Bereitstellung eines Volumes für virtuelle Computer](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md)
* [Verwalten von Momentaufnahmen](azure-netapp-files-manage-snapshots.md)
