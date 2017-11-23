---
title: "Verwalten von App Service-Plänen in Azure | Microsoft-Dokumentation"
description: "Erfahren Sie, wie App Service-Pläne verschiedene Aufgaben zum Verwalten eines App Service-Plans ausführen."
keywords: "App Service, Azure App Service, Skalierung, App Services-Plan, ändern, verwalten, Verwaltung"
services: app-service
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.assetid: 4859d0d5-3e3c-40cc-96eb-f318b2c51a3d
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: cephalin
ms.openlocfilehash: c1b832895476e2f64bbae638db76f89890e5c804
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2017
---
# <a name="manage-an-app-service-plan-in-azure"></a>Verwalten eines App Service-Plans in Azure

Ein [App Service-Plan](azure-web-sites-web-hosting-plans-in-depth-overview.md) stellt Ressourcen bereit, die zum Ausführen einer App Service-App erforderlich sind. Diese Anleitung veranschaulicht das Verwalten eines App Service-Plans. 

## <a name="create-an-app-service-plan"></a>Wie erstelle ich einen Plan?

> [!TIP]
> Falls Sie eine App Service-Umgebung haben, finden Sie Informationen dazu im Abschnitt [Create an App Service plan (Erstellen eines App Service-Plans)](environment/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan).

Sie können einen App Service-Plan eigenständig oder zusammen mit einer Web-App erstellen.

Klicken Sie im [Azure-Portal](https://portal.azure.com) auf **Neu** > **Web und mobil**, und wählen Sie dann die Option **Web-App** oder eine andere Art von App Service-App aus.

![Erstellen Sie eine App im Azure-Portal.][createWebApp]

Sie können dann einen vorhandenen App Service-Plan auswählen oder einen Plan für die neue Anwendung erstellen.

 ![Erstellen eines App Service-Plans][createASP]

Klicken Sie zum Erstellen eines App Service-Plans auf **[+] Neu erstellen**, geben Sie den Namen für den **App Service-Plan** ein, und wählen Sie einen geeigneten **Speicherort** aus. Klicken Sie auf **Tarif**, und wählen Sie einen geeigneten Tarif für den Dienst aus. Wählen Sie **Alle anzeigen** aus, um mehr Tarifoptionen anzuzeigen, z. B. **Free** und **Shared**. 

Klicken Sie nach dem Auswählen des Tarifs auf die Schaltfläche **Auswählen**.

<a name="move"></a>

## <a name="move-an-app-to-another-app-service-plan"></a>Verschieben einer App in einen anderen App Service-Plan

Sie können eine App in einen anderen App Service-Plan verschieben, solange sich der Quellplan und der Zielplan in der _gleichen Ressourcengruppe und geografischen Region_ befinden.

Um eine App in einen anderen Plan zu verschieben, navigieren Sie im [Azure-Portal](https://portal.azure.com) zu der zu verschiebenden App.

Wechseln Sie im **Menü** zum Abschnitt **App Service-Plan**.

Wählen Sie **App Service-Plan ändern**, um den Vorgang zu starten.

Mit **App Service-Plan ändern** wird die Auswahlfunktion für den **App Service-Plan** geöffnet. Wählen Sie einen vorhandenen Plan aus, in den diese App verschoben werden soll. 

> [!IMPORTANT] 
> Die Seite **App Service-Plan auswählen** wird anhand der folgenden Kriterien gefiltert: 
> - In derselben Ressourcengruppe vorhanden 
> - In derselben geografischen Region vorhanden 
> - Im selben Webspace vorhanden  
> 
> Ein _Webspace_ ist ein logisches Konstrukt in App Service, das eine Gruppierung von Serverressourcen definiert. Eine geografische Region (z. B. „USA, Westen“) enthält viele Webspaces, um Kunden mithilfe von App Service zuzuordnen. Derzeit können App Service-Ressourcen nicht zwischen Webspaces verschoben werden. 
> 

![Auswahlelement für App Service-Pläne][change]

Jeder Plan hat einen eigenen Tarif. Wenn Sie beispielsweise eine Website aus dem **Free**-Tarif in den **Standard**-Tarif verschieben, können alle zugewiesenen Apps die Features und Ressourcen des **Standard**-Tarifs nutzen. Allerdings bedeutet das Verschieben einer App von einem Plan mit einem höheren Tarif zu einem Plan mit einem niedrigeren Tarif, dass Sie keinen Zugriff mehr auf bestimmte Features haben. Wenn Ihre Anwendung ein Feature verwendet, das im Zielplan nicht verfügbar ist, erhalten Sie eine Fehlermeldung, die anzeigt, welches Feature verwendet wird, das nicht verfügbar ist. Wenn z. B. eine Ihrer Anwendungen SSL-Zertifikate verwendet, wird möglicherweise die folgende Fehlermeldung angezeigt: `Cannot update the site with hostname '<app_name>' because its current SSL configuration 'SNI based SSL enabled' is not allowed in the target compute mode. Allowed SSL configuration is 'Disabled'.`In diesem Fall müssen Sie den Tarif des Zielplans zentral auf **Basic** oder höher hochskalieren, oder Sie müssen alle SSL-Verbindungen mit Ihrer App entfernen, bevor Sie die App in den Zielplan verschieben können.

## <a name="move-an-app-to-a-different-region"></a>Verschieben einer App in eine andere Region

Die Region, in der Ihre App ausgeführt wird, ist die Region des App Service-Plans, in dem sie sich befindet. Sie können die Region eines App Service-Plans jedoch nicht ändern. Wenn Sie die App in einer anderen Region ausführen möchten, ist das Klonen der App eine Möglichkeit. Beim Klonen wird eine Kopie Ihrer App in einem neuen oder vorhandenen App Service-Plan in einer beliebigen Region erstellt.

Sie finden **App klonen** im Abschnitt **Entwicklungstools** des Menüs.

> [!IMPORTANT]
> Beim Klonen gelten einige Einschränkungen, über die Sie sich unter [Klonen der Azure App Service-App](app-service-web-app-cloning.md) informieren können.

## <a name="scale-an-app-service-plan"></a>Skalieren eines App Service-Plans

Informationen zum zentralen Hochskalieren des Tarifs eines App Service-Plans finden Sie unter [Zentrales Hochskalieren einer App in Azure](web-sites-scale.md).

Informationen zum horizontalen Hochskalieren einer App-Instanz finden Sie unter [Manuelles oder automatisches Skalieren der Instanzenzahl](../monitoring-and-diagnostics/insights-how-to-scale.md).

<a name="delete"></a>

## <a name="delete-an-app-service-plan"></a>Löschen eines App Service-Plans

Um unerwartete Kosten zu vermeiden, löscht App Service den Plan standardmäßig, wenn Sie die letzte App in einem App Service-Plan löschen. Wenn Sie sich dafür entscheiden, den Plan stattdessen beizubehalten, sollten Sie den Plan in den **Free**-Tarif ändern, damit Sie nicht belastet werden.

> [!IMPORTANT]
> Für **App Service-Pläne**, denen keine Apps zugeordnet sind, fallen trotzdem Gebühren an, da die konfigurierten VM-Instanzen weiterhin reserviert werden.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Zentrales Hochskalieren einer App in Azure](web-sites-scale.md)

[change]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/change-appserviceplan.png
[createASP]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-appserviceplan.png
[createWebApp]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-web-app.png
