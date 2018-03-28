---
title: Grundlagen des Azure Stack Development Kits | Microsoft-Dokumentation
description: Dieser Artikel beschreibt die grundlegende Verwaltung des Azure Stack Development Kits.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: cb169c2d2a5aa918fb6d330ebc4677d6c16d308d
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/17/2018
---
# <a name="asdk-administration-basics"></a>Grundlagen zur Verwaltung des ASDK 
Wenn Sie mit der Verwaltung des Azure Stack Development Kits (ASDK) noch nicht vertraut sind, gibt es verschiedene Aspekte, die Sie kennen müssen. Dieser Leitfaden bietet eine Übersicht über Ihre Rolle als Azure Stack-Bediener in der Evaluierungsumgebung und erläutert, wie Sie dafür sorgen, dass Ihre Testbenutzer schnell produktiv arbeiten können.

Lesen Sie zunächst den Artikel [Was ist das Azure Stack Development Kit?](asdk-what-is.md), um sicherzustellen, dass Sie den Zweck des ASDK und die entsprechenden Einschränkungen verstanden haben. Sie sollten das Development Kit als „Sandbox“ verwenden, in dem Sie Azure Stack evaluieren und Ihre Apps in einer Nichtproduktionsumgebung entwickeln und testen können. 

Ebenso wie bei Azure gibt es bei Azure Stack häufig Innovationen – daher veröffentlichen wir regelmäßig neue ASDK-Builds. Sie können das ASDK jedoch nicht auf die gleiche Weise aktualisieren wie Bereitstellungen integrierter Azure Stack-Systeme. Wenn Sie eine Migration zum neuesten Build durchführen möchten, müssen Sie [das ASDK vollständig neu bereitstellen](asdk-redeploy.md). Es ist nicht möglich, Updatepakete anzuwenden. Dieser Vorgang dauert zwar eine Weile, doch danach können Sie die neuesten Funktionen ausprobieren, sobald diese verfügbar sind. 

## <a name="what-tools-do-i-use-to-manage"></a>Welche Tools verwende ich für die Verwaltung?
Für die Verwaltung von Azure Stack können Sie das [Azure Stack-Administratorportal](https://adminportal.local.azurestack.external) oder PowerShell verwenden. Die einfachste Möglichkeit, sich mit den grundlegenden Konzepten vertraut zu machen, bietet das Portal. Wenn Sie PowerShell verwenden möchten, müssen Sie [PowerShell für Azure Stack](asdk-post-deploy.md#install-azure-stack-powershell) installieren und [die Azure Stack-Tools aus GitHub herunterladen](asdk-post-deploy.md#download-the-azure-stack-tools).

Azure Stack verwendet den Azure Resource Manager als zugrunde liegenden Mechanismus für die Bereitstellung, Verwaltung und Organisation. Wenn Sie Azure Stack verwalten und Support für Benutzer bereitstellen werden, sollten Sie sich mit dem Azure Resource Manager vertraut machen. Weitere Informationen finden Sie im Whitepaper [Erste Schritte mit dem Azure Resource Manager](http://download.microsoft.com/download/E/A/4/EA4017B5-F2ED-449A-897E-BD92E42479CE/Getting_Started_With_Azure_Resource_Manager_white_paper_EN_US.pdf).

## <a name="your-typical-responsibilities"></a>Typische Aufgaben
Ihre Benutzer möchten Dienste nutzen. Aus ihrer Sicht besteht Ihre Hauptrolle darin, ihnen diese Dienste zur Verfügung zu stellen. Mit dem ASDK finden Sie heraus, welche Dienste Sie anbieten sollten und wie Sie diese Dienste durch die [Erstellung von Plänen, Angeboten und Kontingenten](asdk-offer-services.md) zur Verfügung stellen. Zudem müssen Sie Elemente wie Images für virtuelle Computer zum Marketplace hinzufügen. Die einfachste Möglichkeit besteht darin, [Marketplace-Elemente](asdk-marketplace-item.md) von Azure in Azure Stack herunterzuladen.

> [!NOTE]
> Wenn Sie Ihre Pläne, Angebote und Dienste testen möchten, sollten Sie das [Benutzerportal](https://portal.local.azurestack.external) verwenden, nicht das [Verwaltungsportal](https://adminportal.local.azurestack.external).

Neben der Bereitstellung von Diensten müssen Sie alle regulären Aufgaben eines Azure Stack-Bedieners durchführen, um den reibungslosen Betrieb des ASDK sicherzustellen. Zu diesen Aufgaben gehören folgende:
- Hinzufügen von Benutzerkonten zu Bereitstellungen von Azure Active Directory (Azure AD) oder Active Directory-Verbunddiensten (AD FS)
- Zuweisen von Rollen für die rollenbasierte Zugriffssteuerung (diese Aufgabe ist nicht auf Administratoren beschränkt)
- Überwachen der Infrastrukturintegrität
- Verwalten von Netzwerk- und Speicherressourcen
- Ersetzen fehlerhafter Hardware für den Development Kit-Hostcomputer 

## <a name="where-to-get-support"></a>Supportquellen
Die einzige Supportoption für das Development Kit besteht darin, Fragen im [Microsoft Azure Stack-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) zu stellen. Wenn Sie oben rechts im Administratorportal auf das Hilfe- und Supportsymbol (Fragezeichen) und dann auf **Neue Supportanfrage** klicken, wird die Forenwebsite direkt geöffnet. Diese Foren werden regelmäßig überprüft. 

> [!IMPORTANT]
> Da das ASDK eine Evaluierungsumgebung ist, wird über den Microsoft-Kundensupport kein offizieller Support angeboten.

## <a name="next-steps"></a>Nächste Schritte
[Bereitstellen des ASDK](asdk-deploy.md)

