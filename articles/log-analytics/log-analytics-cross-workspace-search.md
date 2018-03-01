---
title: "Ausführen von Abfragen über Azure Log Analytics-Arbeitsbereiche | Microsoft-Dokumentation"
description: "In diesem Artikel erfahren Sie, wie Sie Abfragen für mehrere Arbeitsbereiche und bestimmte App Insights-Apps in Ihrem Abonnement ausführen können."
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
ms.date: 02/15/2018
ms.author: magoedte
ms.openlocfilehash: 403448995c28ff7172d2c3abbf3b9d67341017b4
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2018
---
# <a name="how-to-perform-queries-across-multiple-log-analytics-workspaces"></a>Ausführen von Abfragen für mehrere Log Analytics-Arbeitsbereiche

Mit Azure Log Analytics konnten Sie bislang nur Daten innerhalb des aktuellen Arbeitsbereichs analysieren. Abfragen für mehrere in Ihrem Abonnement definierte Arbeitsbereiche waren nicht möglich.  

Sie können jetzt nicht nur Abfragen über mehrere Log Analytics-Arbeitsbereiche ausführen, sondern auch Daten aus einer bestimmten Application Insights-App in der gleichen Ressourcengruppe, einer anderen Ressourcengruppe oder einem anderen Abonnement in Abfragen einbeziehen. Dies bietet Ihnen eine systemweite Ansicht Ihrer Daten.  Diese Art von Abfrage kann nicht im Azure-Portal, sondern nur im [erweiterten Portal](log-analytics-log-search-portals.md#advanced-analytics-portal) verwendet werden.  

## <a name="querying-across-log-analytics-workspaces-and-from-application-insights"></a>Abfragen über mehrere Log Analytics-Arbeitsbereiche und mit Application Insights
Um einen anderen Arbeitsbereich in der Abfrage anzugeben, verwenden Sie die [*Arbeitsbereichs*](https://docs.loganalytics.io/docs/Language-Reference/Scope-functions/workspace())-ID, und verwenden Sie für eine App in Application Insights die [*App*](https://docs.loganalytics.io/docs/Language-Reference/Scope-functions/app())-ID.  

Die erste Abfrage gibt beispielsweise die zusammengefasste Anzahl erforderlicher Updates nach Klassifizierung aus der Tabelle „Update“ zurück (sowohl aus dem aktuellen Arbeitsbereich als auch aus einem anderen Arbeitsbereich namens *contosoretail-it*).  Das zweite Abfragebeispiel gibt die zusammengefasste Anzahl der Anforderungen für eine App namens *fabrikamapp* in Application Insights zurück. 

### <a name="identifying-workspace-resources"></a>Identifizieren von Arbeitsbereichsressourcen
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

* Azure-Ressourcen-ID: Die von Azure definierte, eindeutige Identität des Arbeitsbereichs. Verwenden Sie die Ressourcen-ID, wenn der Ressourcenname nicht eindeutig ist.  Das Format für Arbeitsbereiche lautet wie folgt: */subscriptions/<Abonnement-ID>/resourcegroups/<Ressourcengruppe>/providers/Microsoft.OperationalInsights/workspaces/<Komponentenname>*.  

    Beispiel: 
    ``` 
    workspace("/subscriptions/e427267-5645-4c4e-9c67-3b84b59a6982/resourcegroups/ContosoAzureHQ/providers/Microsoft.OperationalInsights/workspaces/contosoretail").Event | count
    ```

### <a name="identifying-an-application"></a>Identifizieren einer Anwendung

Eine Anwendung in Application Insights kann mit dem Ausdruck *app(Identifier)* identifiziert werden.  Das *Identifier*-Argument gibt die App mit einem der folgenden Namen an:

* Ressourcenname: Ein für Benutzer lesbarer Name der App (manchmal auch *Komponentenname* genannt).  

    `app("fabrikamapp")`

* Qualifizierter Name: Der vollständige Name der App, bestehend aus Abonnementname, Ressourcengruppe und Komponentenname im folgenden Format: *Abonnementname/Ressourcengruppe/Komponentenname*. 

    `app("AI-Prototype/Fabrikam/fabrikamapp").requests | count`

     >[!NOTE]
    >Da die Namen von Azure-Abonnements nicht eindeutig sind, ist dieser Bezeichner unter Umständen mehrdeutig. 
    >

* ID: Die App-GUID der Anwendung.

    `app("b438b4f6-912a-46d5-9cb1-b44069212ab4").requests | count`

* Azure-Ressourcen-ID: Die von Azure definierte, eindeutige Identität der App. Verwenden Sie die Ressourcen-ID, wenn der Ressourcenname nicht eindeutig ist. Das Format ist: */subscriptions/<Abonnement-ID>/resourcegroups/<Ressourcengruppe>/providers/microsoft.OperationalInsights/components/<Komponentenname>*.  

    Beispiel: 
    ```
    app("/subscriptions/7293b69-db12-44fc-9a66-9c2005c3051d/resourcegroups/Fabrikam/providers/microsoft.insights/components/fabrikamapp").requests | count
    ```

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich in der [Referenz zur Log Analytics-Suche](https://docs.loganalytics.io/docs/Language-Reference) über alle Optionen der Abfragesyntax, die in Log Analytics zur Verfügung stehen.    