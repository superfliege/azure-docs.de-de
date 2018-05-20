---
title: Kontingenttypen in Azure Stack | Microsoft-Dokumentation
description: In diesem Artikel finden Sie Informationen zu den unterschiedlichen Kontingenttypen, die für Dienste und Ressourcen in Azure Stack verfügbar sind.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/08/2018
ms.author: brenduns
ms.reviewer: xiaofmao
ms.openlocfilehash: 52d469236d607a145430134c24c4237346dafb4c
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="quota-types-in-azure-stack"></a>Kontingenttypen in Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

[Kontingente](azure-stack-plan-offer-quota-overview.md#plans) definieren die Grenzwerte von Ressourcen, die ein Benutzerabonnement bereitstellen oder nutzen kann. Mithilfe eines Kontingents kann ein Benutzer beispielsweise bis zu fünf virtuelle Computer erstellen. Jede Ressource verfügt über eigene Kontingenttypen.

## <a name="compute-quota-types"></a>Computekontingenttypen
| **Typ** | **Standardwert** | **Beschreibung** |
| --- | --- | --- |
| Maximale Anzahl von virtuellen Computern | 20 | Die maximale Anzahl von virtuellen Computern, die ein Abonnement an diesem Speicherort erstellen kann |
| Maximale Anzahl von Kernen virtueller Computer | 50 | Die maximale Anzahl von Kernen, die ein Abonnement an diesem Speicherort erstellen kann (ein virtueller A3-Computer hat beispielsweise vier Kerne) |
| Maximale Anzahl von Verfügbarkeitsgruppen | 10 | Die maximale Anzahl von Verfügbarkeitsgruppen, die an diesem Speicherort erstellt werden können |
| Maximale Anzahl von Skalierungsgruppen für virtuelle Computer | 20 | Die maximale Anzahl von Skalierungsgruppen für virtuelle Computer, die an diesem Speicherort erstellt werden können |



## <a name="storage-quota-types"></a>Speicherkontingenttypen
| **Element** | **Standardwert** | **Beschreibung** |
| --- | --- | --- |
| Maximale Kapazität (GB) |500 |Die Gesamtspeicherkapazität, die von einem Abonnement an diesem Speicherort genutzt werden kann |
| Gesamte Anzahl von Speicherkonten |20 |Die maximale Anzahl von Speicherkonten, die ein Abonnement an diesem Speicherort erstellen kann |

> [!NOTE]  
> Es kann bis zu zwei Stunden dauern, bis ein Speicherplatzkontingent erzwungen wird. 
> 


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
1. Klicken Sie auf **Weitere Dienste** > **Ressourcenanbieter**.
2. Wählen Sie den Dienst mit dem Kontingent aus, das Sie anzeigen möchten.
3. Klicken Sie auf **Kontingente**, und wählen Sie das Kontingent aus, das Sie anzeigen möchten.

## <a name="next-steps"></a>Nächste Schritte
[Erfahren Sie mehr über Pläne, Angebote und Kontingente.](azure-stack-plan-offer-quota-overview.md)

[Erstellen Sie Kontingente, während Sie einen Plan erstellen.](azure-stack-create-plan.md)
