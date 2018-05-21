---
title: Häufig gestellte Fragen zur Nutzungs-API | Microsoft-Dokumentation
description: Liste der Azure Stack-Verbrauchseinheiten, Vergleich mit der Azure Nutzungs-API, Nutzungszeit und gemeldete Zeit, Fehlercodes.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 847f18b2-49a9-4931-9c09-9374e932a071
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/09/2018
ms.author: mabrigg
ms.reviewer: alfredop
ms.openlocfilehash: f0df9b02e7672faec3a1d94997c9b27ffca275e5
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="frequently-asked-questions-in-azure-stack-usage-api"></a>Häufig gestellte Fragen zur Azure Stack-Nutzungs-API
In diesem Artikel werden einige häufig gestellte Fragen zur Azure Stack-Nutzungs-API beantwortet.

## <a name="what-meter-ids-can-i-see"></a>Welche Verbrauchseinheits-IDs kann ich sehen?
Der Verbrauch wird für die folgenden Ressourcenanbieter gemeldet:

| **Ressourcenanbieter** | **Meter ID** | **Name der Verbrauchseinheit** | **Einheit** | **Zusätzliche Informationen** |
| --- | --- | --- | --- | --- |
| **Netzwerk** |F271A8A388C44D93956A063E1D2FA80B |Nutzung von statischen IP-Adressen |IP-Adressen| Anzahl von verwendeten IP-Adressen; Wenn Sie die Nutzungs-API täglich aufrufen, gibt die Verbrauchseinheit die IP-Adresse multipliziert mit der Anzahl von Stunden zurück. |
| |9E2739BA86744796B465F64674B822BA |Nutzung von dynamischen IP-Adressen |IP-Adressen| Anzahl von verwendeten IP-Adressen; Wenn Sie die Nutzungs-API täglich aufrufen, gibt die Verbrauchseinheit die IP-Adresse multipliziert mit der Anzahl von Stunden zurück. |
| **Speicher** |B4438D5D-453B-4EE1-B42A-DC72E377F1E4 |TableCapacity |GB\*Stunden |Von Tabellen genutzte Kapazität |
| |B5C15376-6C94-4FDD-B655-1A69D138ACA3 |PageBlobCapacity |GB\*Stunden |Von Seitenblobs genutzte Kapazität |
| |B03C6AE7-B080-4BFA-84A3-22C800F315C6 |QueueCapacity |GB\*Stunden |Von Warteschlangen genutzte Kapazität |
| |09F8879E-87E9-4305-A572-4B7BE209F857 |BlockBlobCapacity |GB\*Stunden |Von Blockblobs genutzte Kapazität |
| |B9FF3CD0-28AA-4762-84BB-FF8FBAEA6A90 |TableTransactions |Anzahl von Anforderungen in 10.000 |Tabellenspeicherdienst-Anforderungen (in 10.000ern) |
| |50A1AEAF-8ECA-48A0-8973-A5B3077FEE0D |TableDataTransIn |Dateneingang in GB |Eingang von Tabellenspeicherdienst-Daten in GB |
| |1B8C1DEC-EE42-414B-AA36-6229CF199370 |TableDataTransOut |Ausgehende Daten in GB |Ausgang von Tabellenspeicherdienst-Daten in GB |
| |43DAF82B-4618-444A-B994-40C23F7CD438 |BlobTransactions |Anzahl von Anforderungen in 10.000 |Blob-Dienstanforderungen (in 10.000ern) |
| |9764F92C-E44A-498E-8DC1-AAD66587A810 |BlobDataTransIn |Dateneingang in GB |Eingang von Blob-Dienstdaten in GB |
| |3023FEF4-ECA5-4D7B-87B3-CFBC061931E8 |BlobDataTransOut |Ausgehende Daten in GB |Ausgehender Datenverkehr von Blob-Dienstdaten in GB |
| |EB43DD12-1AA6-4C4B-872C-FAF15A6785EA |QueueTransactions |Anzahl von Anforderungen in 10.000 |Warteschlangendienstanforderungen (in 10.000ern) |
| |E518E809-E369-4A45-9274-2017B29FFF25 |QueueDataTransIn |Dateneingang in GB |Eingang von Warteschlangendienstdaten in GB |
| |DD0A10BA-A5D6-4CB6-88C0-7D585CEF9FC2 |QueueDataTransOut |Ausgehende Daten in GB |Ausgang von Warteschlangendienstdaten in GB |
| **Sql RP**            | CBCFEF9A-B91F-4597-A4D3-01FE334BED82 | DatabaseSizeHourSqlMeter   | MB \* Stunden   | Gesamtkapazität der Datenbank bei Erstellung; Wenn Sie die Nutzungs-API täglich aufrufen, gibt die Verbrauchseinheit die Anzahl von MB multipliziert mit der Anzahl von Stunden zurück. |
| **MySql RP**          | E6D8CFCD-7734-495E-B1CC-5AB0B9C24BD3 | DatabaseSizeHourMySqlMeter | MB \*Stunden    | Gesamtkapazität der Datenbank bei Erstellung; Wenn Sie die Nutzungs-API täglich aufrufen, gibt die Verbrauchseinheit die Anzahl von MB multipliziert mit der Anzahl von Stunden zurück. |
| **Compute** |FAB6EB84-500B-4A09-A8CA-7358F8BBAEA5 |Base VM Size Hours |Virtueller Kern Stunden | Anzahl von virtuellen Kernen multipliziert mit der Anzahl von Stunden, während denen der virtuelle Computer ausgeführt wurde |
| |9CD92D4C-BAFD-4492-B278-BEDC2DE8232A |Windows VM Size Hours |Virtueller Kern Stunden | Anzahl von virtuellen Kernen multipliziert mit der Anzahl von Stunden, während denen der virtuelle Computer ausgeführt wurde |
| |6DAB500F-A4FD-49C4-956D-229BB9C8C793 |VM size hours |VM Stunden |Erfasst sowohl die Basis- als auch die Windows-VM. Wird für Kerne nicht angepasst |
| **Schlüsseltresor** |EBF13B9F-B3EA-46FE-BF54-396E93D48AB4 |Key Vault transactions | Anzahl von Anforderungen in 10.000| Zahl der REST-API-Anforderungen, die von der Key Vault-Datenebene empfangen werden |
| |2C354225-B2FE-42E5-AD89-14F0EA302C87 |Transaktionen für erweiterte Schlüssel | 10.000 Transaktionen|     Transaktionen für RSA 3K/4K-, ECC-Schlüssel (Vorschauversion) |
| **App Service** | 190C935E-9ADA-48FF-9AB8-56EA1CF9ADAA | App Service | Virtueller Kern Stunden | Anzahl der virtuellen Kerne, die zum Ausführen des App-Diensts verwendet werden Hinweis: Microsoft verwendet diese Verbrauchseinheit, um App Service in Azure Stack in Rechnung zu stellen. Cloud-Dienstanbieter können die anderen App Service-Verbrauchseinheiten (unten) zum Berechnen der Nutzung für ihre Mandanten verwenden. |
|  | 67CC4AFC-0691-48E1-A4B8-D744D1FEDBDE | Functions-Anforderungen | 10 Anforderungen | Gesamtzahl der angeforderten Ausführungen (pro 10 Ausführungen) Die Zählung erfolgt bei jeder Ausführung als Antwort auf ein Ereignis oder wird durch eine Bindung ausgelöst. |
|  | D1D04836-075C-4F27-BF65-0A1130EC60ED | Functions – Compute | GB-s | Ressourcenverbrauch gemessen in Gigabytesekunden (GB-s) Der **gemessene Ressourcenverbrauch** wird durch Multiplikation der durchschnittlichen Speichergröße in GB mit der Zeit (in Millisekunden) berechnet, die für die Ausführung der Funktion aufgewendet wird. Der von einer Funktion genutzte Speicher wird auf die nächsten 128 MB gerundet bis zur maximalen Speichergröße von 1536 MB gemessen. Die Ausführungszeit wird durch Runden auf die nächste 1 ms berechnet. Die minimale Ausführungszeit und der minimale Arbeitsspeicher für die Ausführung einer einzelnen Funktion beträgt 100 ms bzw. 128 MB. |
|  | 957E9F36-2C14-45A1-B6A1-1723EF71A01D | Shared-App Service-Stunden | 1 Stunde | Nutzung pro Stunde bei Shared-App Service-Plan Pläne werden auf App-Basis abgerechnet. |
|  | 539CDEC7-B4F5-49F6-AAC4-1F15CFF0EDA9 | Free-App Service-Stunden | 1 Stunde | Nutzung pro Stunde bei kostenlosem App Service-Plan Pläne werden auf App-Basis abgerechnet. |
|  | 88039D51-A206-3A89-E9DE-C5117E2D10A6 | Standard-App Service-Stunden, klein | 1 Stunde | Wird anhand der Größe und Anzahl der Instanzen berechnet. |
|  | 83A2A13E-4788-78DD-5D55-2831B68ED825 | Standard-App Service-Stunden, mittel | 1 Stunde | Wird anhand der Größe und Anzahl der Instanzen berechnet. |
|  | 1083B9DB-E9BB-24BE-A5E9-D6FDD0DDEFE6 | Standard-App Service-Stunden, groß | 1 Stunde | Wird anhand der Größe und Anzahl der Instanzen berechnet. |
|  | *Benutzerdefinierte Workertarife* | Benutzerdefinierte Workertarife | Stunden | ID der deterministischen Verbrauchseinheit wird basierend auf der SKU und dem Namen des benutzerdefinierten Workertarifs erstellt. Diese Verbrauchseinheit-ID ist für jeden benutzerdefinierten Workertarif eindeutig. |
|  | 264ACB47-AD38-47F8-ADD3-47F01DC4F473 | SNI SSL | Pro SNI SSL-Bindung | App Service unterstützt zwei Arten von SSL-Verbindungen: SSL-Verbindungen mit Servernamensanzeige (Server Name Indication, SNI) und SSL-Verbindungen für bestimmte IP-Adressen. SNI SSL wird von modernen Browsern unterstützt, IP-basiertes SSL funktioniert bei allen Browsern. |
|  | 60B42D72-DC1C-472C-9895-6C516277EDB4 | IP-SSL | Pro IP-basierter SSL-Bindung | App Service unterstützt zwei Arten von SSL-Verbindungen: SSL-Verbindungen mit Servernamensanzeige (Server Name Indication, SNI) und SSL-Verbindungen für bestimmte IP-Adressen. SNI SSL wird von modernen Browsern unterstützt, IP-basiertes SSL funktioniert bei allen Browsern. |
|  | 73215A6C-FA54-4284-B9C1-7E8EC871CC5B | Webprozess |  | Wird pro aktive Website pro Stunde berechnet. |
|  | 5887D39B-0253-4E12-83C7-03E1A93DFFD9 | Externe Ausgangsbandbreite | GB | Gesamtanzahl der eingehenden Anforderungsantwortbytes + Gesamtanzahl der ausgehenden Anforderungsantwortbytes + Gesamtanzahl der eingehenden FTP-Anforderungsantwortbytes + Gesamtanzahl der eingehenden Web Deploy-Anforderungsantwortbytes |

## <a name="how-do-the-azure-stack-usage-apis-compare-to-the-azure-usage-apihttpsmsdnmicrosoftcomlibraryazure1ea5b323-54bb-423d-916f-190de96c6a3c-currently-in-public-preview"></a>Wie unterscheiden sich die Azure Stack-Nutzungs-APIs von der [Azure-Nutzungs-API](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) (aktuell in der öffentlichen Vorschau)?
* Die Nutzungs-API für Mandanten stimmt in den meisten Punkten mit der Azure-API überein. Der einzige Unterschied besteht darin, dass das Flag *showDetails* aktuell in Azure Stack nicht unterstützt wird.
* Die Nutzungs-API für Anbieter gibt es nur in Azure Stack.
* Die [RateCard-API](https://msdn.microsoft.com/library/azure/mt219004.aspx) ist zwar in Azure verfügbar, aber noch nicht in Azure Stack.

## <a name="what-is-the-difference-between-usage-time-and-reported-time"></a>Was ist der Unterschied zwischen der Nutzungszeit und der gemeldeten Zeit?
Nutzungsdatenberichte haben zwei Hauptzeitwerte:

* **Gemeldete Zeit**. Der Zeitpunkt, zu dem das Nutzungsereignis im Nutzungssystem begonnen hat
* **Nutzungszeit**. Der Zeitpunkt, an dem die Azure Stack-Ressource verwendet wurde

Möglicherweise besteht für ein spezifisches Nutzungsereignis eine Diskrepanz zwischen der Nutzungszeit und der gemeldeten Zeit. Die Verzögerung kann in jeder Umgebung mehrere Stunden betragen.

Aktuell können Sie nur nach *gemeldeter Zeit* abfragen.

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
