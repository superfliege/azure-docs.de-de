---
title: "Ausführen von Abfragen über Azure Log Analytics-Arbeitsbereiche | Microsoft-Dokumentation"
description: "In diesem Artikel erfahren Sie anhand von Beispielen, wie Sie Abfragen für mehrere Arbeitsbereiche in Ihrem Abonnement ausführen können."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2017
ms.author: magoedte
ms.openlocfilehash: 6b6dc6f472a87178c006301f4f692aeff15e257e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="perform-queries-across-multiple-log-analytics-workspaces"></a>Ausführen von Abfragen für mehrere Log Analytics-Arbeitsbereiche

Mit Azure Log Analytics konnten Sie bislang nur Daten innerhalb des aktuellen Arbeitsbereichs analysieren, was bei Abfragen für mehrere in Ihrem Abonnement definierte Arbeitsbereiche hinderlich war.  

Nun können Sie Abfragen für mehrere Arbeitsbereiche ausführen und sich so einen systemweiten Überblick über Ihre Daten verschaffen.  Diese Art von Abfrage kann nicht im Azure-Portal, sondern nur im [erweiterten Portal](log-analytics-log-search-portals.md#advanced-analytics-portal) verwendet werden.  

## <a name="querying-across-log-analytics-workspaces"></a>Abfragen mehrerer Log Analytics-Arbeitsbereiche
Verwenden Sie den Bezeichner *workspace*, wenn Sie in Ihrer Abfrage auf einen anderen Arbeitsbereich verweisen möchten.  Die folgende Abfrage gibt beispielsweise die zusammengefasste Anzahl erforderlicher Updates nach Klassifizierung aus der Tabelle „Update“ zurück (sowohl aus dem aktuellen Arbeitsbereich als auch aus einem anderen Arbeitsbereich namens *contosoretail-it*).  


## <a name="identifying-resources"></a>Identifizieren von Ressourcen
Ein Arbeitsbereich kann auf mehrere Arten identifiziert werden:

* Ressourcenname: Ein für Benutzer lesbarer Name des Arbeitsbereichs (manchmal auch *Komponentenname* genannt). 

    `workspace("contosoretail").Update | count`
 
    >[!NOTE]
    >Bei der Identifizierung eines Arbeitsbereichs über seinen Namen wird vorausgesetzt, dass er in allen zugänglichen Abonnements eindeutig ist. Sollten mehrere Anwendungen mit dem angegebenen Namen vorhanden sein, ist die Abfrage aufgrund der Mehrdeutigkeit nicht erfolgreich. In diesem Fall muss einer der anderen Bezeichner verwendet werden.

* Qualifizierter Name: Der vollständige Name des Arbeitsbereichs, bestehend aus Abonnementname, Ressourcengruppe und Komponentenname im folgenden Format: *Abonnementname/Ressourcengruppe/Komponentenname*. 

    `workspace('contoso/contosoretail/development').requests | count `

    >[!NOTE]
    >Da die Namen von Azure-Abonnements nicht eindeutig sind, ist dieser Bezeichner unter Umständen mehrdeutig. 
    >

* Arbeitsbereichs-ID: Bei einer Arbeitsbereichs-ID handelt es sich um den eindeutigen, unveränderlichen Bezeichner, der jedem Arbeitsbereich zugewiesen ist und als GUID (Globally Unique Identifier) dargestellt wird.

    `workspace("b438b4f6-912a-46d5-9cb1-b44069212ab4").Update | count`

* Azure-Ressourcen-ID: Die von Azure definierte, eindeutige Identität des Arbeitsbereichs. Verwenden Sie diese Option, wenn der Ressourcenname nicht eindeutig ist.  Das Format für Arbeitsbereiche lautet wie folgt: */subscriptions/<Abonnement-ID>/resourcegroups/<Ressourcengruppe>/providers/Microsoft.OperationalInsights/workspaces/<Komponentenname>*.  

    Beispiel:
    ``` 
    `workspace("/subscriptions/e427267-5645-4c4e-9c67-3b84b59a6982/resourcegroups/ContosoAzureHQ/providers/Microsoft.OperationalInsights/workspaces/contosoretail").Event | count
    ```

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich in der [Referenz zur Log Analytics-Suche](https://docs.loganalytics.io/docs/Language-Reference) über alle Optionen der Abfragesyntax, die in Log Analytics zur Verfügung stehen.    