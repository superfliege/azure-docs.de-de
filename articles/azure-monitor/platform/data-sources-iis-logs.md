---
title: IIS-Protokolle in Log Analytics | Microsoft Docs
description: IIS (Internet Information Services, Internetinformationsdienste) speichern Benutzeraktivitäten in Protokolldateien, die von Log Analytics gesammelt werden können.  In diesem Artikel wird erläutert, wie Sie das Sammeln von IIS-Protokollen konfigurieren. Darüber hinaus enthält der Artikel Details zu den Datensätzen, die in Log Analytics erstellt werden.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: cec5ff0a-01f5-4262-b2e8-e3db7b7467d2
ms.service: log-analytics
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/28/2018
ms.author: bwren
ms.openlocfilehash: cd63c63344f322f7d761a2907f52e97f1009e3b8
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2019
ms.locfileid: "54101954"
---
# <a name="collect-iis-logs-in-log-analytics"></a>Sammeln von IIS-Protokollen in Log Analytics
IIS (Internet Information Services, Internetinformationsdienste) speichern Benutzeraktivitäten in Protokolldateien, die von Log Analytics gesammelt und als [Protokolldaten](data-collection.md) gespeichert werden können.

![IIS-Protokolle](media/data-sources-iis-logs/overview.png)

## <a name="configuring-iis-logs"></a>Konfigurieren von IIS-Protokollen
Log Analytics sammelt Einträge aus von IIS erstellten Protokolldateien. Daher müssen Sie [IIS für die Protokollierung konfigurieren](https://technet.microsoft.com/library/hh831775.aspx).

Log Analytics unterstützt nur IIS-Protokolldateien im W3C-Format, aber keine benutzerdefinierten Felder oder die erweiterte IIS-Protokollierung. Protokolle im NCSA- oder nativen IIS-Format werden nicht gesammelt.

IIS-Protokolle können in Log Analytics über das Menü [Erweiterte Einstellungen](agent-data-sources.md#configuring-data-sources) konfiguriert werden.  Abgesehen von der Auswahl der Option **IIS-Protokolldateien im W3C-Format sammeln**ist keine Konfiguration erforderlich.


## <a name="data-collection"></a>Datensammlung
Log Analytics sammelt jedes Mal IIS-Protokolleinträge aus jedem Agent, wenn das Protokoll geschlossen und ein neues Protokoll erstellt wird. Die Häufigkeit wird über die Einstellung für den **Rolloverzeitplan der Protokolldatei** für die IIS-Website gesteuert, wobei einmal täglich als Standardeinstellung festgelegt ist. Wenn die Einstellung beispielsweise auf **Stündlich** festgelegt ist, erfasst Log Analytics das Protokoll jede Stunde.  Wenn die Einstellung auf **Täglich** festgelegt ist, erfasst Log Analytics das Protokoll alle 24 Stunden.


## <a name="iis-log-record-properties"></a>Eigenschaften der IIS-Protokolldatensätze
IIS-Protokolldatensätze weisen den Typ **W3CIISLog** auf und besitzen die in der folgenden Tabelle aufgeführten Eigenschaften:

| Eigenschaft | BESCHREIBUNG |
|:--- |:--- |
| Computer |Name des Computers, auf dem das Ereignis gesammelt wurde. |
| cIP |IP-Adresse des Clients. |
| csMethod |Methode der Anforderung, beispielsweise GET oder POST. |
| csReferer |Website, von der der Benutzer über einen Link auf die aktuelle Website gelangt ist. |
| csUserAgent |Browsertyp des Clients. |
| csUserName |Name des authentifizierten Benutzers, der auf den Server zugegriffen hat. Anonyme Benutzer werden durch einen Bindestrich gekennzeichnet. |
| csUriStem |Ziel der Anforderung, beispielsweise eine Webseite. |
| csUriQuery |Abfrage, die der Client versucht hat auszuführen. |
| ManagementGroupName |Name der Verwaltungsgruppe für Operations Manager-Agents.  Bei anderen Agents lautet dieser „AOI-\<Arbeitsbereich-ID\>“. |
| RemoteIPCountry |Land der IP-Adresse des Clients. |
| RemoteIPLatitude |Breitengrad der Client-IP-Adresse. |
| RemoteIPLongitude |Längengrad der Client-IP-Adresse. |
| scStatus |HTTP-Statuscode. |
| scSubStatus |Unterstatus-Fehlercode. |
| scWin32Status |Windows-Statuscode. |
| sIP |IP-Adresse des Webservers. |
| SourceSystem |Operations Manager |
| sPort |Port auf dem Server, mit dem der Client verbunden ist. |
| sSiteName |Name der IIS-Website. |
| TimeGenerated |Datum und Uhrzeit, zu der der Eintrag protokolliert wurde. |
| TimeTaken |Verarbeitungsdauer der Anforderung in Millisekunden. |

## <a name="log-queries-with-iis-logs"></a>Protokollieren von Abfragen mit IIS-Protokollen
Die folgende Tabelle zeigt verschiedene Beispiele für Protokollabfragen, die IIS-Protokolldatensätze abrufen.

| Abfragen | BESCHREIBUNG |
|:--- |:--- |
| W3CIISLog |Alle IIS-Protokolldatensätze. |
| W3CIISLog &#124; where scStatus==500 |Alle IIS-Protokolleinträge mit dem Rückgabestatus 500 |
| W3CIISLog &#124; summarize count() by cIP |Anzahl der IIS-Protokolleinträge nach Client-IP-Adresse. |
| W3CIISLog &#124; where csHost=="www.contoso.com" &#124; summarize count() by csUriStem |Anzahl der IIS-Protokolleinträge nach URL für den Host www.contoso.com. |
| W3CIISLog &#124; summarize sum(csBytes) by Computer &#124; take 500000 |Gesamtzahl an Bytes, die von jedem IIS-Computer empfangen wurden. |

## <a name="next-steps"></a>Nächste Schritte
* Konfigurieren Sie Log Analytics für die Sammlung von Daten aus anderen [Datenquellen](agent-data-sources.md) zur Analyse.
* Erfahren Sie mehr über [Protokollabfragen](../../log-analytics/log-analytics-queries.md) zum Analysieren der aus Datenquellen und Lösungen gesammelten Daten.
