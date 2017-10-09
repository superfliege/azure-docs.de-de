---
title: "Häufig gestellte Fragen zur Nutzungs-API | Microsoft-Dokumentation"
description: Liste der Azure Stack-Verbrauchseinheiten, Vergleich mit der Azure Nutzungs-API, Nutzungszeit und gemeldete Zeit, Fehlercodes.
services: azure-stack
documentationcenter: 
author: AlfredoPizzirani
manager: byronr
editor: 
ms.assetid: 847f18b2-49a9-4931-9c09-9374e932a071
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2016
ms.author: alfredop
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 166147c8cb4949be1b23e0a06868e66c8a5844f4
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
# <a name="frequently-asked-questions-in-azure-stack-usage-api"></a>Häufig gestellte Fragen zur Azure Stack-Nutzungs-API 
In diesem Artikel werden einige häufig gestellte Fragen zur Azure Stack-Nutzungs-API beantwortet.

## <a name="what-meter-ids-can-i-see"></a>Welche Verbrauchseinheits-IDs kann ich sehen?
Aktuell wird der Verbrauch für Netzwerk-, Speicher- und Computeressourcenanbieter gemeldet.

| **Ressourcenanbieter** | **Meter ID** | **Name der Verbrauchseinheit** | **Einheit** | **Zusätzliche Informationen** |
| --- | --- | --- | --- | --- | 
| **Netzwerk** |F271A8A388C44D93956A063E1D2FA80B |Nutzung von statischen IP-Adressen |IP-Adressen|Zahl der verwendeten IP-Adressen | 
| |9E2739BA86744796B465F64674B822BA |Nutzung von dynamischen IP-Adressen |IP-Adressen|Zahl der verwendeten IP-Adressen | 
| **Speicher** |B4438D5D-453B-4EE1-B42A-DC72E377F1E4 |TableCapacity |GB\*Stunden |Von Tabellen genutzte Kapazität |
| | B5C15376-6C94-4FDD-B655-1A69D138ACA3 |PageBlobCapacity |GB\*Stunden |Von Seitenblobs genutzte Kapazität |
| | B03C6AE7-B080-4BFA-84A3-22C800F315C6 |QueueCapacity |GB\*Stunden |Von Warteschlangen genutzte Kapazität |
| | 09F8879E-87E9-4305-A572-4B7BE209F857 |BlockBlobCapacity |GB\*Stunden |Von Blockblobs genutzte Kapazität |
| | B9FF3CD0-28AA-4762-84BB-FF8FBAEA6A90 |TableTransactions |Zahl der Anforderungen in 10.000ern |Tabellenspeicherdienst-Anforderungen (in 10.000ern) |
| | 50A1AEAF-8ECA-48A0-8973-A5B3077FEE0D |TableDataTransIn |Dateneingang in GB |Eingang von Tabellenspeicherdienst-Daten in GB |
| | 1B8C1DEC-EE42-414B-AA36-6229CF199370 |TableDataTransOut |Datenausgang in GB |Ausgang von Tabellenspeicherdienst-Daten in GB |
| | 43DAF82B-4618-444A-B994-40C23F7CD438 |BlobTransactions |Zahl der Anforderungen in 10.000ern |Blob-Dienstanforderungen (in 10.000ern) |
| | 9764F92C-E44A-498E-8DC1-AAD66587A810 |BlobDataTransIn |Dateneingang in GB |Eingang von Blob-Dienstdaten in GB |
| | 3023FEF4-ECA5-4D7B-87B3-CFBC061931E8 |BlobDataTransOut |Datenausgang in GB |Ausgehender Datenverkehr von Blob-Dienstdaten in GB |
| | EB43DD12-1AA6-4C4B-872C-FAF15A6785EA |QueueTransactions |Zahl der Anforderungen in 10.000ern |Warteschlangendienstanforderungen (in 10.000ern) |
| | E518E809-E369-4A45-9274-2017B29FFF25 |QueueDataTransIn |Dateneingang in GB |Eingang von Warteschlangendienstdaten in GB | 
| | DD0A10BA-A5D6-4CB6-88C0-7D585CEF9FC2 |QueueDataTransOut |Datenausgang in GB |Ausgang von Warteschlangendienstdaten in GB |
| **Compute** |FAB6EB84-500B-4A09-A8CA-7358F8BBAEA5 |Base VM Size Hours |Virtueller Kern Minuten | Zahl der virtuellen Kerne mal Zahl der Minuten, die die VM ausgeführt wurde |
| |9CD92D4C-BAFD-4492-B278-BEDC2DE8232A |Windows VM Size Hours |Virtueller Kern Minuten | Zahl der virtuellen Kerne mal Zahl der Minuten, die die VM ausgeführt wurde |
| |6DAB500F-A4FD-49C4-956D-229BB9C8C793 |VM size hours |VM Stunden |Erfasst sowohl die Basis- als auch die Windows-VM. Wird für virtuelle Kerne nicht angepasst |
| **Schlüsseltresor** | EBF13B9F-B3EA-46FE-BF54-396E93D48AB4 |Key Vault transactions | Zahl der Anforderungen in 10.000ern| Zahl der REST-API-Anforderungen, die von der Key Vault-Datenebene empfangen werden |

## <a name="how-do-the-azure-stack-usage-apis-compare-to-the-azure-usage-apihttpsmsdnmicrosoftcomlibraryazure1ea5b323-54bb-423d-916f-190de96c6a3c-currently-in-public-preview"></a>Wie unterscheiden sich die Azure Stack-Nutzungs-APIs von der [Azure-Nutzungs-API](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) (aktuell in der öffentlichen Vorschau)?
* Die Nutzungs-API für Mandanten stimmt in den meisten Punkten mit der Azure-API überein. Der einzige Unterschied besteht darin, dass das Flag *showDetails* aktuell in Azure Stack nicht unterstützt wird.
* Die Nutzungs-API für Anbieter gibt es nur in Azure Stack.
* Die [RateCard-API](https://msdn.microsoft.com/en-us/library/azure/mt219004.aspx) ist zwar in Azure verfügbar, aber noch nicht in Azure Stack.

## <a name="what-is-the-difference-between-usage-time-and-reported-time"></a>Was ist der Unterschied zwischen der Nutzungszeit und der gemeldeten Zeit?
Nutzungsdatenberichte haben zwei Hauptzeitwerte:

* **Gemeldete Zeit**. Der Zeitpunkt, zu dem das Nutzungsereignis im Nutzungssystem begonnen hat
* **Nutzungszeit**. Der Zeitpunkt, an dem die Azure Stack-Ressource verwendet wurde

Möglicherweise besteht für ein spezifisches Nutzungsereignis eine Diskrepanz zwischen der Nutzungszeit und der gemeldeten Zeit. Die Verzögerung kann in jeder Umgebung mehrere Stunden betragen.

Aktuell können Sie *nur nach gemeldeter Zeit* abfragen.

## <a name="what-do-these-usage-api-error-codes-mean"></a>Fehlercodes der Nutzungs-API und deren Bedeutungen
| **HTTP-Statuscode** | **Fehlercode** | **Beschreibung** |
| --- | --- | --- |
| 400/Bad Request |*NoApiVersion* |Der Abfrageparameter der *API-Version* fehlt. |
| 400/Bad Request |*InvalidProperty* |Eine Eigenschaft fehlt oder weist einen unzulässigen Wert auf. Die Meldung im Fehlercode im Antworttext gibt die fehlende Eigenschaft an. |
| 400/Bad Request |*RequestEndTimeIsInFuture* |Der Wert für *gemeldeteEndzeit* liegt in der Zukunft. Für dieses Argument sind keine Werte zulässig, die in der Zukunft liegen. |
| 400/Bad Request |*SubscriberIdIsNotDirectTenant* |Der API-Aufruf eines Anbieters hat eine Abonnement-ID verwendet, die keinem Mandanten des Aufrufers zugeordnet werden kann. |
| 400/Bad Request |*SubscriptionIdMissingInRequest* |Die Abonnement-ID des Aufrufers fehlt. |
| 400/Bad Request |*InvalidAggregationGranularity* |Es wurde eine unzulässige Aggregationsgranularität angefordert. Zulässige Werte sind täglich und stündlich. |
| 503 |*ServiceUnavailable* |Ein wiederholbarer Fehler ist aufgetreten, da der Dienst ausgelastet ist oder der Aufruf gedrosselt wird. |

## <a name="next-steps"></a>Nächste Schritte
[Kundenabrechnung und verbrauchsbasierte Kostenzuteilung in Azure Stack](azure-stack-billing-and-chargeback.md)

[Ressourcennutzungs-API für Anbieter](azure-stack-provider-resource-api.md)

[Ressourcennutzungs-API für Mandanten](azure-stack-tenant-resource-usage-api.md)


