---
title: Kontingenttypen in Azure Stack | Microsoft-Dokumentation
description: In diesem Artikel finden Sie Informationen zu den unterschiedlichen Kontingenttypen, die für Dienste und Ressourcen in Azure Stack verfügbar sind.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/08/2018
ms.author: sethm
ms.reviewer: xiaofmao
ms.openlocfilehash: ec84c87e047d6426d24ef339d1247c6cb53954dd
ms.sourcegitcommit: d372d75558fc7be78b1a4b42b4245f40f213018c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2018
ms.locfileid: "51299418"
---
# <a name="quota-types-in-azure-stack"></a>Kontingenttypen in Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

[Kontingente](azure-stack-plan-offer-quota-overview.md#plans) definieren die Grenzwerte von Ressourcen, die ein Benutzerabonnement bereitstellen oder nutzen kann. Mithilfe eines Kontingents kann ein Benutzer beispielsweise bis zu fünf virtuelle Computer erstellen. Jede Ressource verfügt über eigene Kontingenttypen.

## <a name="compute-quota-types"></a>Computekontingenttypen 
| **Typ** | **Standardwert** | **Beschreibung** |
| --- | --- | --- |
| Maximale Anzahl von virtuellen Computern | 50 | Die maximale Anzahl von virtuellen Computern, die ein Abonnement an diesem Speicherort erstellen kann |
| Maximale Anzahl von Kernen virtueller Computer | 100 | Die maximale Anzahl von Kernen, die ein Abonnement an diesem Speicherort erstellen kann (ein virtueller A3-Computer hat beispielsweise vier Kerne) |
| Maximale Anzahl von Verfügbarkeitsgruppen | 10 | Die maximale Anzahl von Verfügbarkeitsgruppen, die an diesem Speicherort erstellt werden können |
| Maximale Anzahl von Skalierungsgruppen für virtuelle Computer | 100 | Die maximale Anzahl von Skalierungsgruppen für virtuelle Computer, die an diesem Speicherort erstellt werden können |

## <a name="storage-quota-types"></a>Speicherkontingenttypen 
| **Element** | **Standardwert** | **Beschreibung** |
| --- | --- | --- |
| Maximale Kapazität (GB) |2048 |Die Gesamtspeicherkapazität, die von einem Abonnement an diesem Speicherort genutzt werden kann |
| Gesamte Anzahl von Speicherkonten |20 |Die maximale Anzahl von Speicherkonten, die ein Abonnement an diesem Speicherort erstellen kann |

> [!NOTE]  
> Es kann bis zu zwei Stunden dauern, bis ein Speicherplatzkontingent erzwungen wird.


## <a name="network-quota-types"></a>Netzwerkkontingenttypen
| **Element** | **Standardwert** | **Beschreibung** |
| --- | --- | --- |
| Maximale Anzahl von öffentlichen IP-Adressen |50 |Die maximale Anzahl von öffentlichen IP-Adressen, die ein Abonnement an diesem Speicherort erstellen kann |
| Maximale Anzahl von virtuellen Netzwerken |50 |Die maximale Anzahl von virtuellen Netzwerken, die ein Abonnement an diesem Speicherort erstellen kann |
| Maximale Anzahl von Gateways für virtuelle Netzwerke |1 |Die maximale Anzahl von Gateways für virtuelle Netzwerke (VPN Gateways), die ein Abonnement an diesem Speicherort erstellen kann |
| Maximale Anzahl von Netzwerkverbindungen |2 |Die maximale Anzahl von Netzwerkverbindungen (Point-to-Point oder Site-to-Site), die ein Abonnement für alle Gateways für virtuelle Netzwerke an diesem Speicherort erstellen kann. |
| Maximale Anzahl von Lastenausgleichsmodulen |50 |Die maximale Anzahl von Lastenausgleichsmodulen, die ein Abonnement an diesem Speicherort erstellen kann |
| Maximale Anzahl NICs |100 |Die maximale Anzahl von Netzwerkschnittstellen, die ein Abonnement an diesem Speicherort erstellen kann |
| Maximale Anzahl von Netzwerksicherheitsgruppen |50 |Die maximale Anzahl von Netzwerksicherheitsgruppen, die ein Abonnement an diesem Speicherort erstellen kann |

## <a name="view-an-existing-quota"></a>Anzeigen eines vorhandenen Kontingents
1. Auf dem Standarddashboard des Verwaltungsportals finden Sie die Kachel **Ressourcenanbieter**.
2. Wählen Sie den Dienst mit dem Kontingent aus, das Sie anzeigen möchten (beispielsweise **Compute** oder **Speicher**).
3. Klicken Sie auf **Kontingente**, und wählen Sie anschließend das Kontingent aus, das Sie anzeigen möchten.


## <a name="edit-a-quota"></a>Bearbeiten eines Kontingents  
Anstatt einen [Add-On-Plan zu verwenden](create-add-on-plan.md), können Sie auch die ursprüngliche Konfiguration eines Kontingents bearbeiten. Wenn Sie ein Kontingent bearbeiten, gilt die neue Konfiguration automatisch global für alle Pläne, die dieses Kontingent verwenden, sowie für alle vorhandenen Abonnements, die diese Pläne verwenden. Die Bearbeitung eines Kontingents ist unterschiedlich, wenn Sie einen Add-On-Plan verwenden, um ein geändertes Kontingent bereitzustellen, das ein Benutzer abonniert. 

### <a name="to-edit-a-quota"></a>Bearbeiten eines Kontingents  
1. Auf dem Standarddashboard des Verwaltungsportals finden Sie die Kachel **Ressourcenanbieter**.
2. Wählen Sie den Dienst mit dem Kontingent aus, das Sie ändern möchten (beispielsweise **Compute**, **Netzwerk** oder **Speicher**).
3. Klicken Sie auf **Kontingente**, und wählen Sie anschließend das Kontingent aus, das Sie ändern möchten.
4. Bearbeiten Sie die Werte im Bereich **Kontingente festlegen**, und klicken Sie anschließend auf **Speichern**. 

Die neuen Werte für das Kontingent gelten global für alle Pläne, die das geänderte Kontingent verwenden, sowie für alle vorhandenen Abonnements, die diese Pläne verwenden. 



## <a name="next-steps"></a>Nächste Schritte

- [Erfahren Sie mehr über Pläne, Angebote und Kontingente.](azure-stack-plan-offer-quota-overview.md)
- [Erstellen Sie Kontingente, während Sie einen Plan erstellen.](azure-stack-create-plan.md)
