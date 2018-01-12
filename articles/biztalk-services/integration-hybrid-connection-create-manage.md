---
title: Erstellen und Verwalten von Hybridverbindungen | Microsoft Docs
description: Erfahren Sie, wie Sie eine Hybridverbindung herstellen, die Verbindung verwalten und den Hybridverbindungs-Manager installieren. MABS, WABS
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: erikre
editor: 
ms.assetid: aac0546b-3bae-41e0-b874-583491a395ea
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2016
ms.author: ccompy
ms.openlocfilehash: 1751d33b5f6f6a506654daedd15bbd75ae271483
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2017
---
# <a name="create-and-manage-hybrid-connections"></a>Erstellen und Verwalten von Hybridverbindungen

> [!IMPORTANT]
> BizTalk-Hybridverbindungen ist veraltet und wird durch App Service-Hybridverbindungen ersetzt. Weitere Informationen, z.B. darüber, wie Sie Ihre vorhandenen BizTalk-Hybridverbindungen verwalten, finden Sie unter [Azure App Service-Hybridverbindungen](../app-service/app-service-hybrid-connections.md).

>[!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

## <a name="overview-of-the-steps"></a>Übersicht über die Schritte
1. Erstellen Sie eine Hybridverbindung, indem Sie den **host name** oder **FQDN** of the on-premises resource in your private netwoderk.
2. Verknüpfen Sie Ihre Azure-Web-Apps bzw. mobile Azure-Apps mit der Hybridverbindung.
3. Installieren Sie den Hybrid Connection Manager auf der lokalen Ressource, und stellen Sie eine Verbindung zur betreffenden Hybridverbindung her. Das Azure-Portal stellt ein Ein-Klick-Verfahren für die Installation und Verbindung bereit.
4. Verwalten Sie Hybridverbindungen und deren Verbindungsschlüssel.

In diesem Thema werden diese Schritte aufgeführt. 

> [!IMPORTANT]
> Sie können einen Hybridverbindungsendpunkt auf eine IP-Adresse festlegen. Bei Verwendung einer IP-Adresse hängt es vom Client ab, ob die lokale Ressource erreichbar ist. Die Hybridverbindung ist darauf angewiesen, dass der Client eine DNS-Suche ausführt. In den meisten Fällen handelt es sich beim **Client** um den Anwendungscode. Falls der Client keine DNS-Suche ausführt (er versucht nicht, die IP-Adresse aufzulösen, als handele es sich um einen Domänennamen (x.x.x.x)), wird der Datenverkehr nicht über die Hybridverbindung gesendet.
> 
> Beispiel: Sie legen **10.4.5.6** als lokalen Host fest:
> 
> **Das folgende Szenario funktioniert:**  
> `Application code -> GetHostByName("10.4.5.6") -> Resolves to 127.0.0.3 -> Connect("127.0.0.3") -> Hybrid Connection -> on-prem host`
> 
> **Das folgende Szenario funktioniert nicht:**  
> `Application code -> Connect("10.4.5.6") -> ?? -> No route to host`
> 
> 

## <a name="CreateHybridConnection"></a>Erstellen einer Hybridverbindung
Eine Hybridverbindung kann in [Azure App Service Hybrid Connections](../app-service/app-service-hybrid-connections.md) **oder** mit [BizTalk Services REST-APIs](https://msdn.microsoft.com/library/azure/dn232347.aspx) erstellt werden. 

<!-- **To create Hybrid Connections using Web Apps**, see [Connect Azure Web Apps to an On-Premises Resource](../app-service-web/web-sites-hybrid-connection-get-started.md). You can also install the Hybrid Connection Manager (HCM) from your web app, which is the preferred method.  -->

#### <a name="additional"></a>Zusätzlich
* Es können mehrere Hybridverbindungen erstellt werden. Die Anzahl der zulässigen Verbindungen finden Sie unter [BizTalk Services: Editionsübersicht](biztalk-editions-feature-chart.md) für . 
* Jede Hybridverbindung wird mit einem Paar von Verbindungszeichenfolgen erstellt: Anwendungsschlüsseln, die SENDEN, und lokalen Schlüsseln, die LAUSCHEN. Jedes Paar hat einen primären und einen sekundären Schlüssel. 

## <a name="LinkWebSite"></a>Verknüpfen Ihrer Azure App Service-Web-App oder Mobile App
Um eine Web-App oder Mobile App mit einer vorhandenen Hybridverbindung zu verknüpfen, wählen Sie auf dem Blatt „Hybridverbindungen“ die Option **Vorhandene Hybridverbindung verwenden** aus. 
<!-- See [Access on-premises resources using hybrid connections in Azure App Service](../app-service-web/web-sites-hybrid-connection-get-started.md). -->

## <a name="InstallHCM"></a>Lokale Installation des Hybrid Connection Manager
Nachdem eine Hybridverbindung erstellt wurde, installieren Sie den Hybrid Connection Manager auf der lokalen Ressource. Er kann aus Azure-Web-Apps oder BizTalk Service heruntergeladen werden. 

[!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]
 
[Azure App Service Hybrid Connections](../app-service/app-service-hybrid-connections.md) ist ebenfalls eine geeignete Ressource.

<!--
You can also download the Hybrid Connection Manager MSI file and copy the file to your on-premises resource. Specific steps:

1. Copy the on-premises primary Connection String. See [Manage Hybrid Connections](#ManageHybridConnection) in this topic for the specific steps.
2. Download the Hybrid Connection Manager MSI file. 
3. On the on-premises resource, install the Hybrid Connection Manager from the MSI file. 
4. Using Windows PowerShell, type: 
> Add-HybridConnection -ConnectionString “*Your On-Premises Connection String that you copied*” 
--> 

#### <a name="additional"></a>Zusätzlich
* Hybrid Connection Manager kann unter den folgenden Betriebssystemen installiert werden:
  
  * Windows Server 2008 R2 (.NET Framework 4.5 und Windows Management Framework 4.0 mindestens erforderlich)
  * Windows Server 2012 (Windows Management Framework 4.0 mindestens erforderlich)
  * Windows Server 2012 R2
* Nachdem Sie den Hybrid Connection Manager installiert haben, geschieht Folgendes: 
  
  * Die auf Azure gehostete Hybridverbindung wird automatisch so konfiguriert, dass sie die primäre Anwendungsverbindungszeichenfolge verwendet. 
  * Die lokale Ressource wird automatisch so konfiguriert, dass sie die primäre lokale Verbindungszeichenfolge verwendet.
* Der Hybrid Connection Manager muss eine gültige lokale Verbindungszeichenfolge für die Autorisierung verwenden. Die Azure-Web-Apps bzw. die mobilen Apps müssen eine gültige Anwendungsverbindungszeichenfolge für die Autorisierung verwenden.
* Sie können Hybridverbindungen skalieren, indem Sie eine andere Instanz von Hybrid Connection Manager auf einem anderen Server installieren. Konfigurieren Sie den lokalen Listener so, dass er die gleiche Adresse wie der erste lokale Listener verwenden. In diesem Fall wird der Datenverkehr zufällig (Roundrobin) zwischen den aktiven lokalen Listenern verteilt. 

## <a name="ManageHybridConnection"></a>Verwalten von Hybridverbindungen

[!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)] 

[Azure App Service Hybrid Connections](../app-service/app-service-hybrid-connections.md) ist ebenfalls eine geeignete Ressource.

#### <a name="copyregenerate-the-hybrid-connection-strings"></a>Kopieren Sie die Hybridverbindungszeichenfolgen oder generieren Sie diese erneut.

[!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)] 

[Azure App Service Hybrid Connections](../app-service/app-service-hybrid-connections.md) ist ebenfalls eine geeignete Ressource.

#### <a name="use-group-policy-to-control-the-on-premises-resources-used-by-a-hybrid-connection"></a>Verwenden Sie Gruppenrichtlinien, um die von einer Hybridverbindung verwendeten lokalen Ressourcen zu steuern.
1. Laden Sie die [administrativen Vorlagen des Hybrid Connection Manager](http://www.microsoft.com/download/details.aspx?id=42963)herunter.
2. Extrahieren Sie die Dateien.
3. Gehen Sie auf dem Computer, der die Gruppenrichtlinie ändert, wie folgt vor:  
   
   * Kopieren Sie die ADMX-Dateien in den Ordner *%WINROOT%\PolicyDefinitions*.
   * Kopieren Sie die ADML-Dateien in den Ordner *%WINROOT%\PolicyDefinitions\en-us*.

Nach dem Kopieren können Sie den Gruppenrichtlinien-Editor verwenden, um die Richtlinie zu ändern.

## <a name="next"></a>Next (Weiter)
[Übersicht über Hybridverbindungen](integration-hybrid-connection-overview.md)

## <a name="see-also"></a>Weitere Informationen
[REST API für die Verwaltung von BizTalk Services unter Microsoft Azure](http://msdn.microsoft.com/library/azure/dn232347.aspx)  
[BizTalk Services: Editionsübersicht](biztalk-editions-feature-chart.md)  
[Erstellen eines BizTalk Service](biztalk-provision-services.md)  
[BizTalk Services: Registerkarten "Dashboard", "Überwachen" und "Skalieren"](biztalk-dashboard-monitor-scale-tabs.md)

[HybridConnectionTab]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionManageConn.png 
