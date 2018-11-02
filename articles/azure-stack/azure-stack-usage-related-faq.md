---
title: Häufig gestellte Fragen zur Nutzungs-API | Microsoft-Dokumentation
description: Liste der Azure Stack-Verbrauchseinheiten, Vergleich mit der Azure Nutzungs-API, Nutzungszeit und gemeldete Zeit, Fehlercodes.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2018
ms.author: mabrigg
ms.reviewer: alfredop
ms.openlocfilehash: 362ee7e84229c421f69ff0db0c8cfb040b105c97
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49394510"
---
# <a name="frequently-asked-questions-in-azure-stack-usage-api"></a>Häufig gestellte Fragen zur Azure Stack-Nutzungs-API

In diesem Artikel werden einige häufig gestellte Fragen zur Azure Stack-Nutzungs-API beantwortet.

## <a name="what-meter-ids-can-i-see"></a>Welche Verbrauchseinheits-IDs kann ich sehen?
Der Verbrauch wird für die folgenden Ressourcenanbieter gemeldet:

### <a name="network"></a>Netzwerk
  
**Meter ID**: F271A8A388C44D93956A063E1D2FA80B  
**Messung:** Nutzung von statischen IP-Adressen  
**Einheit:** IP-Adressen  
**Hinweise:** Anzahl verwendeter IP-Adressen. Wenn Sie die Nutzungs-API täglich aufrufen, gibt die Verbrauchseinheit die IP-Adresse multipliziert mit der Anzahl von Stunden zurück.  
  
**Meter ID**: 9E2739BA86744796B465F64674B822BA  
**Messung:** Nutzung von dynamischen IP-Adressen  
**Einheit:** IP-Adressen  
**Hinweise:** Anzahl verwendeter IP-Adressen. Wenn Sie die Nutzungs-API täglich aufrufen, gibt die Verbrauchseinheit die IP-Adresse multipliziert mit der Anzahl von Stunden zurück.  
  
### <a name="storage"></a>Speicher
  
**Messungs-ID:** B4438D5D-453B-4EE1-B42A-DC72E377F1E4  
**Messung:** TableCapacity  
**Einheit:** GB\*Stunden  
**Hinweise:** von Tabellen genutzte Kapazität  
  
**Messungs-ID:** B5C15376-6C94-4FDD-B655-1A69D138ACA3  
**Messung:** PageBlobCapacity  
**Einheit:** GB\*Stunden  
**Hinweise:** von Seitenblobs genutzte Kapazität  
  
**Messungs-ID** B03C6AE7-B080-4BFA-84A3-22C800F315C6  
**Messung:** QueueCapacity  
**Einheit:** GB\*Stunden  
**Hinweise:** von Warteschlangen genutzte Kapazität  
  
**Messungs-ID:** 09F8879E-87E9-4305-A572-4B7BE209F857  
**Messung:** BlockBlobCapacity  
**Einheit:** GB\*Stunden  
**Hinweise:** von Blockblobs genutzte Kapazität  
  
**Messungs-ID:** B9FF3CD0-28AA-4762-84BB-FF8FBAEA6A90  
**Messung:** TableTransactions  
**Einheit:** Anzahl von Anforderungen in 10.000  
**Hinweise:** Table Storage-Anforderungen (in 10.000ern)  
  
**Messungs-ID:** 50A1AEAF-8ECA-48A0-8973-A5B3077FEE0D  
**Messung:** TableDataTransIn  
**Einheit:** eingehende Daten in GB  
**Hinweise:** Eingang von Table Storage-Daten in GB  
  
**Meter ID**: 1B8C1DEC-EE42-414B-AA36-6229CF199370  
**Messung:** TableDataTransOut  
**Einheit:** ausgehende Daten in GB  
**Hinweise:** Ausgang von Table Storage-Daten in GB  
  
**Messungs-ID:** 43DAF82B-4618-444A-B994-40C23F7CD438  
**Messung:** BlobTransactions  
**Einheit:** Anzahl von Anforderungen in 10.000  
**Hinweise:** Blob-Dienstanforderungen (in 10.000)  
  
**Messungs-ID:** 9764F92C-E44A-498E-8DC1-AAD66587A810  
**Messung:** BlobDataTransIn  
**Einheit:** eingehende Daten in GB  
**Hinweise:** Eingang von Blob-Dienstdaten in GB  
  
**Messungs-ID:** 3023FEF4-ECA5-4D7B-87B3-CFBC061931E8  
**Messung:** BlobDataTransOut  
**Einheit:** ausgehende Daten in GB  
**Hinweise:** ausgehender Datenverkehr von Blob-Dienstdaten in GB  
  
**Messungs-ID:** EB43DD12-1AA6-4C4B-872C-FAF15A6785EA  
**Messung:** QueueTransactions  
**Einheit:** Anzahl von Anforderungen in 10.000  
**Hinweise:** Warteschlangendienstanforderungen (in 10.000ern)  
  
**Messungs-ID:** E518E809-E369-4A45-9274-2017B29FFF25  
**Messung:** QueueDataTransIn  
**Einheit:** eingehende Daten in GB  
**Hinweise:** Eingang von Warteschlangendienstdaten in GB  
  
**Meter ID**: DD0A10BA-A5D6-4CB6-88C0-7D585CEF9FC2  
**Messung:** QueueDataTransOut  
**Einheit:** ausgehende Daten in GB  
**Hinweise:** Ausgang von Warteschlangendienstdaten in GB  

### <a name="compute"></a>Compute 
  
**Meter ID**: FAB6EB84-500B-4A09-A8CA-7358F8BBAEA5  
**Messung:** Basis-VM-Größe – Stunden  
**Einheit:** virtueller Kern – Stunden  
**Hinweise:** Anzahl virtueller Kerne multipliziert mit der Anzahl von Stunden, die der virtuelle Computer ausgeführt wurde  
  
**Messungs-ID:** 9CD92D4C-BAFD-4492-B278-BEDC2DE8232A  
**Messung:** Windows-VM-Größe – Stunden  
**Einheit:** virtueller Kern – Stunden  
**Hinweise:** Anzahl virtueller Kerne multipliziert mit der Anzahl von Stunden, die der virtuelle Computer ausgeführt wurde  
  
**Messungs-ID:** 6DAB500F-A4FD-49C4-956D-229BB9C8C793  
**Messung:** VM-Größe – Stunden  
**Einheit:** VM-Stunden  
**Hinweise:** Erfasst sowohl die Basis- als auch die Windows-VM Wird für Kerne nicht angepasst  
  
### <a name="managed-disks"></a>Managed Disks

**ID der Verbrauchseinheit**: 5d76e09f-4567-452a-94cc-7d1f097761f0   
**Name der Verbrauchseinheit**: S4   
**Einheit**: Anzahl der Datenträger\*Stunden   
**Hinweise**: Managed Disks Standard – 32 GB 

**ID der Verbrauchseinheit**: dc9fc6a9-0782-432a-b8dc-978130457494   
**Name der Verbrauchseinheit**: S6   
**Einheit**: Anzahl der Datenträger\*Stunden   
**Hinweise**: Managed Disks Standard – 64 GB 

**ID der Verbrauchseinheit**: e5572fce-9f58-49d7-840c-b168c0f01fff   
**Name der Verbrauchseinheit**: S10   
**Einheit**: Anzahl der Datenträger\*Stunden   
**Hinweise**: Managed Disks Standard – 128 GB 

**ID der Verbrauchseinheit**: 9a8caedd-1195-4cd5-80b4-a4c22f9302b8   
**Name der Verbrauchseinheit**: S15   
**Einheit**: Anzahl der Datenträger\*Stunden   
**Hinweise**: Managed Disks Standard – 256 GB 

**ID der Verbrauchseinheit**: 5938f8da-0ecd-4c48-8d5a-c7c6c23546be   
**Name der Verbrauchseinheit**: S20   
**Einheit**: Anzahl der Datenträger\*Stunden      
**Hinweise**: Managed Disks Standard – 512 GB 

**ID der Verbrauchseinheit**: 7705a158-bd8b-4b2b-b4c2-0782343b81e6   
**Name der Verbrauchseinheit**: S30   
**Einheit**: Anzahl der Datenträger\*Stunden   
**Hinweise**: Managed Disks Standard – 1.024 GB 

**ID der Verbrauchseinheit**: d9aac1eb-a5d1-42f2-b617-9e3ea94fed88   
**Name der Verbrauchseinheit**: S40   
**Einheit**: Anzahl der Datenträger\*Stunden   
**Hinweise**: Managed Disks Standard – 2.048 GB 

**ID der Verbrauchseinheit**: a54899dd-458e-4a40-9abd-f57cafd936a7   
**Name der Verbrauchseinheit**: S50   
**Einheit**: Anzahl der Datenträger\*Stunden   
**Hinweise**: Managed Disks Standard – 4.096 GB 

**ID der Verbrauchseinheit**: 5c105f5f-cbdf-435c-b49b-3c7174856dcc   
**Name der Verbrauchseinheit**: P4   
**Einheit**: Anzahl der Datenträger\*Stunden   
**Hinweise**: Managed Disks Premium – 32 GB 

**ID der Verbrauchseinheit**: 518b412b-1927-4f25-985f-4aea24e55c4f   
**Name der Verbrauchseinheit**: P6   
**Einheit**: Anzahl der Datenträger\*Stunden   
**Hinweise**: Managed Disks Premium – 64 GB 

**ID der Verbrauchseinheit**: 5cfb1fed-0902-49e3-8217-9add946fd624   
**Name der Verbrauchseinheit**: P10   
**Einheit**: Anzahl der Datenträger\*Stunden   
**Hinweise**: Managed Disks Premium – 128 GB  

**ID der Verbrauchseinheit**: 8de91c94-f740-4d9a-b665-bd5974fa08d4   
**Name der Verbrauchseinheit**: P15  
**Einheit**: Anzahl der Datenträger\*Stunden   
**Hinweise**: Managed Disks Premium – 256 GB 

**ID der Verbrauchseinheit**: c7e7839c-293b-4761-ae4c-848eda91130b   
**Name der Verbrauchseinheit**: P20   
**Einheit**: Anzahl der Datenträger\*Stunden   
**Hinweise**: Managed Disks Premium – 512 GB 

**ID der Verbrauchseinheit**: 9f502103-adf4-4488-b494-456c95d23a9f   
**Name der Verbrauchseinheit**: P30   
**Einheit**: Anzahl der Datenträger\*Stunden   
**Hinweise**: Managed Disks Premium – 1.024 GB 

**ID der Verbrauchseinheit**: 043757fc-049f-4e8b-8379-45bb203c36b1   
**Name der Verbrauchseinheit**: P40   
**Einheit**: Anzahl der Datenträger\*Stunden    
**Hinweise**: Managed Disks Premium – 2.048 GB 

**ID der Verbrauchseinheit**: c0342c6f-810b-4942-85d3-6eaa561b6570   
**Name der Verbrauchseinheit**: P50   
**Einheit**: Anzahl der Datenträger\*Stunden   
**Hinweise**: Managed Disks Premium – 4.096 GB 

**ID der Verbrauchseinheit**: 8a409390-1913-40ae-917b-08d0f16f3c38   
**Name der Verbrauchseinheit**: ActualStandardDiskSize   
**Einheit**: Byte\*Stunden      
**Hinweise**: Die tatsächliche Größe auf dem Datenträger von Managed Disks Standard  

**ID der Verbrauchseinheit**: 1273b16f-8458-4c34-8ce2-a515de551ef6  
**Name der Verbrauchseinheit**: ActualPremiumDiskSize   
**Einheit**: Byte\*Stunden      
**Hinweise**: Die tatsächliche Größe auf dem Datenträger von Managed Disks Premium 

**ID der Verbrauchseinheit**: 89009682-df7f-44fe-aeb1-63fba3ddbf4c  
**Name der Verbrauchseinheit**: ActualStandardSnapshotSize   
**Einheit**: Byte\*Stunden   
**Hinweise**: Die tatsächliche Größe auf dem Datenträger von Managed Disks Snapshot.  

**ID der Verbrauchseinheit**: 95b0c03f-8a82-4524-8961-ccfbf575f536   
**Name der Verbrauchseinheit**: ActualPremiumSnapshotSize   
**Einheit**: Byte\*Stunden   
**Hinweise**: Die tatsächliche Größe auf dem Datenträger von Managed Premium.   

### <a name="sql-rp"></a>Sql RP
  
**Messungs-ID:** CBCFEF9A-B91F-4597-A4D3-01FE334BED82  
**Messung:** DatabaseSizeHourSqlMeter  
**Einheit:** MB\*Stunden  
**Hinweise:** Gesamtkapazität der Datenbank bei Erstellung Wenn Sie die Nutzungs-API täglich aufrufen, gibt die Verbrauchseinheit die Anzahl von MB multipliziert mit der Anzahl von Stunden zurück.  
  
### <a name="mysql-rp"></a>MySql RP   
  
**Messungs-ID:** E6D8CFCD-7734-495E-B1CC-5AB0B9C24BD3  
**Messung:** DatabaseSizeHourMySqlMeter  
**Einheit:** MB\*Stunden  
**Hinweise:** Gesamtkapazität der Datenbank bei Erstellung Wenn Sie die Nutzungs-API täglich aufrufen, gibt die Verbrauchseinheit die Anzahl von MB multipliziert mit der Anzahl von Stunden zurück.    
### <a name="key-vault"></a>Key Vault   
  
**Messungs-ID:** EBF13B9F-B3EA-46FE-BF54-396E93D48AB4  
**Messung:** Key Vault-Transaktionen  
**Einheit:** Anzahl von Anforderungen in 10.000  
**Hinweise:** Anzahl der REST-API-Anforderungen, die von der Key Vault-Datenebene empfangen werden  
  
**Messungs-ID:** 2C354225-B2FE-42E5-AD89-14F0EA302C87  
**Messung:** Transaktionen für erweiterte Schlüssel  
**Einheit:** 10.000 Transaktionen  
**Hinweise:** Transaktionen für RSA 3K/4K-, ECC-Schlüssel (Vorschauversion)  
  
### <a name="app-service"></a>App Service   
  
**Messungs-ID:** 190C935E-9ADA-48FF-9AB8-56EA1CF9ADAA  
**Messung:** App Service  
**Einheit:** virtueller Kern – Stunden  
**Hinweise:** Anzahl virtueller Kerne, die zum Ausführen des App-Diensts verwendet werden Hinweis: Microsoft verwendet diese Verbrauchseinheit, um App Service in Azure Stack in Rechnung zu stellen. Cloud-Dienstanbieter können die anderen App Service-Verbrauchseinheiten (unten) zum Berechnen der Nutzung für ihre Mandanten verwenden.  
  
**Messungs-ID:** 67CC4AFC-0691-48E1-A4B8-D744D1FEDBDE  
**Messung:** Functions-Anforderungen  
**Einheit:** 10 Anforderungen  
**Hinweise:** Gesamtzahl der angeforderten Ausführungen (pro 10 Ausführungen) Die Zählung erfolgt bei jeder Ausführung als Antwort auf ein Ereignis oder wird durch eine Bindung ausgelöst.  
  
**Messungs-ID:** D1D04836-075C-4F27-BF65-0A1130EC60ED  
**Messung:** Functions – Compute  
**Einheit:** GB-s  
**Hinweise:** Ressourcenverbrauch gemessen in Gigabytesekunden (GB/s). Der **gemessene Ressourcenverbrauch** wird durch Multiplikation der durchschnittlichen Speichergröße in GB mit der Zeit (in Millisekunden) berechnet, die für die Ausführung der Funktion aufgewendet wird. Der von einer Funktion genutzte Speicher wird auf die nächsten 128 MB gerundet bis zur maximalen Speichergröße von 1536 MB gemessen. Die Ausführungszeit wird durch Runden auf die nächste 1 ms berechnet. Die minimale Ausführungszeit und der minimale Arbeitsspeicher für die Ausführung einer einzelnen Funktion beträgt 100 ms bzw. 128 MB.  
  
**Messungs-ID:** 957E9F36-2C14-45A1-B6A1-1723EF71A01D  
**Messung:** Shared-App Service-Stunden  
**Einheit:** 1 Stunde  
**Hinweise:** Nutzung pro Stunde bei Shared-App Service-Plan Pläne werden auf App-Basis abgerechnet.  
  
**Messungs-ID:** 539CDEC7-B4F5-49F6-AAC4-1F15CFF0EDA9  
**Messung:** Free-App Service-Stunden  
**Einheit:** 1 Stunde  
**Hinweise:** Nutzung pro Stunde bei Free-App Service-Plan. Pläne werden auf App-Basis abgerechnet.  
  
**Messungs-ID:** 88039D51-A206-3A89-E9DE-C5117E2D10A6  
**Messung:** Standard-App Service-Stunden, klein  
**Einheit:** 1 Stunde  
**Hinweise:** wird anhand der Größe und Anzahl der Instanzen berechnet.  
  
**Messungs-ID:** 83A2A13E-4788-78DD-5D55-2831B68ED825  
**Messung:** Standard-App Service-Stunden, mittel  
**Einheit:** 1 Stunde  
**Hinweise:** wird anhand der Größe und Anzahl der Instanzen berechnet.  
  
**Messungs-ID:** 1083B9DB-E9BB-24BE-A5E9-D6FDD0DDEFE6  
**Messung:** Standard-App Service-Stunden, groß  
**Einheit:** 1 Stunde  
**Hinweise:** wird anhand der Größe und Anzahl der Instanzen berechnet.  
  
### <a name="custom-worker-tiers"></a>Benutzerdefinierte Workertarife   
  
**Messungs-ID:** *benutzerdefinierte Workertarife*  
**Messung:** benutzerdefinierte Workertarife  
**Einheit:** Stunden  
**Hinweise:** ID der deterministischen Verbrauchseinheit wird basierend auf der SKU und dem Namen des benutzerdefinierten Workertarifs erstellt. Diese Verbrauchseinheit-ID ist für jeden benutzerdefinierten Workertarif eindeutig.  
  
**Meter ID**: 264ACB47-AD38-47F8-ADD3-47F01DC4F473  
**Messung:** SNI SSL  
**Einheit:** pro SNI SSL-Bindung  
**Hinweise:** App Service unterstützt zwei Arten von SSL-Verbindungen: SSL-Verbindungen mit Servernamensanzeige (Server Name Indication, SNI) und SSL-Verbindungen für bestimmte IP-Adressen. SNI SSL wird von modernen Browsern unterstützt, IP-basiertes SSL funktioniert bei allen Browsern.  
  
**Messungs-ID:** 60B42D72-DC1C-472C-9895-6C516277EDB4  
**Messung:** IP-SSL  
**Einheit:** pro IP-basierter SSL-Bindung  
**Hinweise:** App Service unterstützt zwei Arten von SSL-Verbindungen: SSL-Verbindungen mit Servernamensanzeige (Server Name Indication, SNI) und SSL-Verbindungen für bestimmte IP-Adressen. SNI SSL wird von modernen Browsern unterstützt, IP-basiertes SSL funktioniert bei allen Browsern.  
  
**Messungs-ID:** 73215A6C-FA54-4284-B9C1-7E8EC871CC5B  
**Messung:** Webprozess  
**Einheit:**  
**Hinweise:** Wird pro aktive Website pro Stunde berechnet.  
  
**Messungs-ID:** 5887D39B-0253-4E12-83C7-03E1A93DFFD9  
**Messung:** externe Ausgangsbandbreite  
**Einheit:** GB  
**Hinweise:** Gesamtanzahl der eingehenden Anforderungsantwortbytes + Gesamtanzahl der ausgehenden Anforderungsantwortbytes + Gesamtanzahl der eingehenden FTP-Anforderungsantwortbytes + Gesamtanzahl der eingehenden Web Deploy-Anforderungsantwortbytes  
  

## <a name="how-do-the-azure-stack-usage-apis-compare-to-the-azure-usage-apihttpsdocsmicrosoftcomazurebillingbilling-usage-rate-card-overviewazure-resource-usage-api-preview-currently-in-public-preview"></a>Wie unterscheiden sich die Azure Stack-Nutzungs-APIs von der [Azure-Nutzungs-API](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview#azure-resource-usage-api-preview) (aktuell in der öffentlichen Vorschau)?
* Die Nutzungs-API für Mandanten stimmt in den meisten Punkten mit der Azure-API überein. Der einzige Unterschied besteht darin, dass das Flag *showDetails* aktuell in Azure Stack nicht unterstützt wird.
* Die Nutzungs-API für Anbieter gibt es nur in Azure Stack.
* Die [RateCard-API](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview#azure-resource-ratecard-api-preview) ist zwar in Azure verfügbar, aber noch nicht in Azure Stack.

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
