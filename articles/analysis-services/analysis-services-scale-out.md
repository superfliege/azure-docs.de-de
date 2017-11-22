---
title: Horizontales Hochskalieren von Azure Analysis Services | Microsoft-Dokumentation
description: Replizieren von Azure Analysis Services-Servern mittels horizontalem Hochskalieren
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/06/2017
ms.author: owend
ms.openlocfilehash: a97f9648efef7f07659110d720c200dcd0a241a9
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2017
---
# <a name="azure-analysis-services-scale-out"></a>Horizontales Hochskalieren von Azure Analysis Services

Durch horizontales Hochskalieren können Clientabfragen auf mehrere *Abfragereplikate* in einem Abfragepool verteilt werden, was bei einem hohen Aufkommen von Abfrageworkloads zu kürzeren Antwortzeiten führt. Darüber hinaus kann die Verarbeitung vom Abfragepool getrennt werden, um sicherzustellen, dass Clientabfragen nicht durch Verarbeitungsvorgänge beeinträchtigt werden. Horizontales Hochskalieren kann im Azure-Portal oder mithilfe der Analysis Services-REST-API konfiguriert werden.

## <a name="how-it-works"></a>So funktioniert's

In einer typischen Serverbereitstellung fungiert ein einzelner Server sowohl als Verarbeitungs- als auch als Abfrageserver. Wenn die Anzahl von Clientabfragen für Modelle auf dem Server die QPUs (Query Processing Units) für den Tarif Ihres Servers übersteigt oder die Modellverarbeitung mit einem hohen Aufkommen von Abfrageworkloads zusammenfällt, kann sich dies negativ auf die Leistung auswirken. 

Mit der horizontalen Hochskalierung können Sie einen Abfragepool mit bis zu sieben zusätzlichen Replikaten erstellen, sodass Sie zusammen mit Ihrem Server über insgesamt acht verfügen. Sie können die Anzahl von Abfragereplikaten jederzeit skalieren, um die QPU-Anforderungen in kritischen Zeiten zu erfüllen, und Sie können einen Verarbeitungsserver aus dem Abfragepool herauslösen. 

Verarbeitungsworkloads werden unabhängig von der Anzahl von Abfragereplikaten in einem Abfragepool nicht auf Abfragereplikate verteilt. Ein einzelner Server fungiert als Verarbeitungsserver. Abfragereplikate bedienen ausschließlich Abfragen für die Modelle, die zwischen den einzelnen Replikaten im Abfragepool synchronisiert werden. 

Nach Abschluss von Verarbeitungsvorgängen ist eine Synchronisierung zwischen dem Verarbeitungsserver und den Abfragereplikatservern erforderlich. Wenn Verarbeitungsvorgänge automatisiert werden, muss auch ein Synchronisierungsvorgang konfiguriert werden, der nach erfolgreicher Durchführung von Verarbeitungsvorgängen ausgeführt wird. Die Synchronisierung kann manuell im Portal oder mithilfe von PowerShell oder der REST-API ausgeführt werden.

> [!NOTE]
> Horizontales Hochskalieren ist für Server im Standardtarif verfügbar. Jedes Abfragereplikat wird mit dem gleichen Tarif abgerechnet wie Ihr Server.

> [!NOTE]
> Durch horizontales Hochskalieren erhöht sich nicht die Menge an verfügbarem Arbeitsspeicher für Ihren Server. Zur Erhöhung des Arbeitsspeichers müssen Sie Ihren Plan upgraden.

## <a name="monitor-qpu-usage"></a>Überwachen der QPU-Nutzung

 Überwachen Sie Ihren Server im Azure-Portal mithilfe von Metriken, um zu ermitteln, ob horizontales Hochskalieren für Ihren Server erforderlich ist. Wenn regelmäßig die QPU-Obergrenze erreicht wird, übersteigt die Anzahl von Abfragen für Ihre Modelle das QPU-Limit für Ihren Tarif. Die Metrik „Warteschlangenlänge für Abfragepoolaufträge“ erhöht sich auch, wenn die Anzahl von Abfragen in der Warteschlange des Abfragethreadpools die verfügbaren QPUs übersteigt. Weitere Informationen finden Sie unter [Überwachen von Servermetriken](analysis-services-monitor.md).

## <a name="configure-scale-out"></a>Konfigurieren des horizontalen Hochskalierens

### <a name="in-azure-portal"></a>Im Azure-Portal

1. Klicken Sie im Portal auf **Horizontale Skalierung**. Wählen Sie mithilfe des Schiebereglers die Anzahl von Abfragereplikatservern aus. Die gewählte Anzahl von Replikaten kommt zu Ihrem bereits vorhandenen Server hinzu.

2. Klicken Sie unter **Verarbeitungsserver vom Abfragepool trennen** auf „Ja“, um Ihren Verarbeitungsserver von Abfrageservern auszuschließen.

   ![Schieberegler für horizontales Hochskalieren](media/analysis-services-scale-out/aas-scale-out-slider.png)

3. Klicken Sie auf **Speichern**, um Ihre neuen Abfragereplikatserver bereitzustellen. 

Tabellarische Modelle auf Ihrem primären Server werden mit den Replikatservern synchronisiert. Nach Abschluss der Synchronisierung beginnt der Abfragepool mit der Verteilung eingehender Abfragen auf die Replikatserver. 


## <a name="synchronization"></a>Synchronisierung 

Wenn Sie neue Abfragereplikate bereitstellen, repliziert Azure Analysis Services Ihre Modelle automatisch in allen Replikaten. Sie können auch eine manuelle Synchronisierung mithilfe des Portals oder der REST-API ausführen. Wenn Sie Ihre Modelle verarbeiten, sollten Sie eine Synchronisierung durchführen, damit Aktualisierungen zwischen Ihren Abfragereplikaten synchronisiert werden.

### <a name="in-azure-portal"></a>Im Azure-Portal

Klicken Sie in der **Übersicht** für das Modell auf das Symbol **Modell synchronisieren**.

![Schieberegler für horizontales Hochskalieren](media/analysis-services-scale-out/aas-scale-out-sync.png)

### <a name="rest-api"></a>REST-API
Verwenden Sie die **sync**-Operation.

#### <a name="synchronize-a-model"></a>Synchronisieren eines Modells   
`POST https://<region>.asazure.windows.net/servers/<servername>:rw/models/<modelname>/sync`

#### <a name="get-sync-status"></a>Abrufen des Synchronisierungsstatus  
`GET https://<region>.asazure.windows.net/servers/<servername>:rw/models/<modelname>/sync`

### <a name="powershell"></a>PowerShell
Um die Synchronisierung über PowerShell auszuführen, nehmen Sie eine [Aktualisierung auf das neueste](https://github.com/Azure/azure-powershell/releases) AzureRM-Modul, Version 5.01 oder höher, vor. Verwendung Sie [Sync-AzureAnalysisServicesInstance](https://docs.microsoft.com/en-us/powershell/module/azurerm.analysisservices/sync-azureanalysisservicesinstance).

## <a name="connections"></a>Verbindungen

Auf der Übersichtsseite des Servers werden zwei Servernamen angezeigt. Wenn Sie noch keine horizontale Hochskalierung für einen Server konfiguriert haben, funktionieren beide Servernamen gleich. Nach dem Konfigurieren der horizontalen Hochskalierung für einen Server müssen Sie den passenden Servernamen für den jeweiligen Verbindungstyp angeben. 

Für Endbenutzer-Clientverbindungen wie Power BI Desktop, Excel und benutzerdefinierte Apps muss der **Servername** verwendet werden. 

Für SSMS, SSDT und Verbindungszeichenfolgen in PowerShell, Azure Functions-Apps und AMO muss der **Name des Verwaltungsservers** verwendet werden. Der Name des Verwaltungsservers enthält einen speziellen `:rw`-Qualifizierer (Lesen/Schreiben). Sämtliche Verarbeitungsvorgänge finden auf dem Verwaltungsserver statt.

![Servernamen](media/analysis-services-scale-out/aas-scale-out-name.png)

## <a name="related-information"></a>Verwandte Informationen

[Überwachen von Servermetriken](analysis-services-monitor.md)   
[Verwalten von Azure Analysis Services](analysis-services-manage.md) 

