---
title: Konfigurieren einer Hybrid Cloud-Identität mit Azure- und Azure Stack-Anwendungen | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie eine Hybrid Cloud-Identität mit Azure- und Azure Stack-Anwendungen konfigurieren.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/14/2019
ms.author: mabrigg
ms.reviewer: anajod
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: d36b94ac9f130649e880e6bff1fa3a8b223828b4
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55244407"
---
# <a name="tutorial-configure-hybrid-cloud-identity-for-azure-and-azure-stack-applications"></a>Tutorial: Konfigurieren einer Hybrid Cloud-Identität für Azure- und Azure Stack-Anwendungen

*Anwendungsbereich: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Hier erfahren Sie, wie Sie eine Hybrid Cloud-Identität für Ihre Azure- und Azure Stack-Anwendungen konfigurieren.

Der Zugriff auf Ihre Anwendungen in globalen Azure- und Azure Stack-Instanzen kann auf zwei Arten gewährt werden.

 * Wenn Azure Stack über eine ständige Internetverbindung verfügt, können Sie Azure Active Directory (Azure AD) verwenden.
 * Wenn Azure Stack über keine Internetverbindung verfügt, können Sie Azure Directory-Verbunddienste (AD FS) verwenden.

Dienstprinzipale werden verwendet, um den Zugriff auf Ihre Azure Stack-Anwendungen zum Zweck der Bereitstellung oder Konfiguration über Azure Resource Manager in Azure Stack zu gewähren.

In diesem Tutorial erstellen Sie eine Beispielumgebung, die Folgendes ermöglicht:

> [!div class="checklist"]
> - Einrichten einer Hybrididentität in globalen Azure- und Azure Stack-Instanzen
> - Abrufen eines Tokens für den Zugriff auf die Azure Stack-API

Sie benötigen Azure Stack-Bedienerberechtigungen für die Schritte in diesem Tutorial.

> [!Tip]  
> ![hybrid-pillars.png](./media/azure-stack-solution-cloud-burst/hybrid-pillars.png)  
> Microsoft Azure Stack ist eine Erweiterung von Azure. Mit Azure Stack holen Sie sich die Agilität und Innovation von Cloud Computing in Ihre lokale Umgebung. Sie erhalten die einzige Hybrid Cloud, mit der Sie Hybrid-Apps überall entwickeln und bereitstellen können.  
> 
> Im Whitepaper [Design Considerations for Hybrid Applications](https://aka.ms/hybrid-cloud-applications-pillars) (Entwurfsüberlegungen für Hybridanwendungen) werden die wichtigen Aspekte in Bezug auf die Softwarequalität (Platzierung, Skalierbarkeit, Verfügbarkeit, Resilienz, Verwaltbarkeit und Sicherheit) beschrieben, die für das Entwerfen, Bereitstellen und Betreiben von Hybridanwendungen erforderlich sind. Die Überlegungen zum Entwurf dienen als Hilfe beim Optimieren des Designs von Hybridanwendungen, um für Produktionsumgebungen das Auftreten von Problemen zu minimieren.


## <a name="create-a-service-principal-for-azure-ad-in-the-portal"></a>Erstellen eines Dienstprinzipals für Azure AD über das Portal

Wenn Sie Azure Stack mit Azure AD als Identitätsspeicher bereitgestellt haben, können Sie Dienstprinzipale genauso wie für Azure erstellen. Im Artikel [Gewähren des Anwendungszugriffs auf Azure Stack-Ressourcen durch Erstellen von Dienstprinzipalen](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-create-service-principals#create-service-principal-for-azure-ad) erfahren Sie, wie die Schritte über das Portal ausgeführt werden. Vergewissern Sie sich vorher, ob Sie über die [erforderlichen Azure AD-Berechtigungen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#required-permissions) verfügen.

## <a name="create-a-service-principal-for-ad-fs-using-powershell"></a>Erstellen eines Dienstprinzipals für AD FS mithilfe von PowerShell

Wenn Sie Azure Stack mit AD FS bereitgestellt haben, können Sie PowerShell verwenden, um einen Dienstprinzipal zu erstellen, eine Rolle für den Zugriff zuzuweisen und die Anmeldung über PowerShell mit dieser Identität durchzuführen. Im Abschnitt [Erstellen eines Dienstprinzipals für AD FS](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-create-service-principals#create-service-principal-for-ad-fs) wird gezeigt, wie Sie die erforderlichen Schritte mithilfe von PowerShell ausführen.

## <a name="using-the-azure-stack-api"></a>Verwenden der Azure Stack-API

Im Tutorial zur [Azure Stack-API](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-rest-api-use) erfahren Sie Schritt für Schritt, wie Sie ein Token für den Zugriff auf die Azure Stack-API abrufen.

## <a name="connect-to-azure-stack-using-powershell"></a>Herstellen einer Verbindung mit Azure Stack mithilfe von PowerShell

In der Schnellstartanleitung zum [Einrichten von PowerShell in Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-configure-quickstart) sind die Schritte erläutert, die zum Installieren von Azure PowerShell und zum Herstellen einer Verbindung mit der Azure Stack-Installation ausgeführt werden müssen.

### <a name="prerequisites"></a>Voraussetzungen

Eine mit Azure Active Directory verbundene Azure Stack-Installation mit einem Abonnement, auf das Sie zugreifen können. Sollten Sie über keine Azure Stack-Installation verfügen, können Sie anhand [dieser Anleitung](https://docs.microsoft.com/azure/azure-stack/asdk/asdk-deploy) ein Azure Stack Development Kit einrichten.

#### <a name="connect-to-azure-stack-using-code"></a>Herstellen einer Verbindung mit Azure Stack mithilfe von Code

Wenn Sie mithilfe von Code eine Verbindung mit Azure Stack herstellen möchten, verwenden Sie die Endpunkte-API von Azure Resource Manager, um die Authentifizierung und die Graph-Endpunkte für Ihre Azure Stack-Installation abzurufen, und führen Sie dann die Authentifizierung mithilfe von REST-Anforderungen durch. Eine Beispielclientanwendung finden Sie auf [GitHub](https://github.com/shriramnat/HybridARMApplication).

>[!Note]
>Wenn Sie das Azure SDK für Azure Stack verwenden möchten, muss es Azure-API-Profile für die gewählte Sprache unterstützen. Weitere Informationen zu Azure-API-Profilen finden Sie im Artikel [Verwalten von API-Versionsprofilen in Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-version-profiles).

## <a name="next-steps"></a>Nächste Schritte

 - Weitere Informationen zur Behandlung der Identität in Azure Stack finden Sie unter [Identitätsarchitektur für Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-identity-architecture).
 - Weitere Informationen zu Azure-Cloudmustern finden Sie unter [Cloudentwurfsmuster](https://docs.microsoft.com/azure/architecture/patterns).
