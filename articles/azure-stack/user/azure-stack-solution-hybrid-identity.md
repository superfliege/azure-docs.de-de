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
ms.date: 05/07/2018
ms.author: mabrigg
ms.reviewer: Anjay.Ajodha
ms.openlocfilehash: 5224fe4682509f92e1f18d865e5cc5afb1218ad1
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
---
# <a name="tutorial-configure-hybrid-cloud-identity-with-azure-and-azure-stack-applications"></a>Tutorial: Konfigurieren einer Hybrid Cloud-Identität mit Azure- und Azure Stack-Anwendungen

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Der Zugriff auf Ihre Anwendungen in globalen Azure- und Azure Stack-Instanzen kann auf zwei Arten gewährt werden. Wenn Azure Stack über eine ständige Internetverbindung verfügt, können Sie Azure Active Directory (Azure AD) verwenden. Wenn Azure Stack über keine Internetverbindung verfügt, können Sie Azure Directory-Verbunddienste (AD FS) verwenden. Dienstprinzipale werden verwendet, um den Zugriff auf Ihre Anwendungen in Azure Stack zu gewähren und die Bereitstellung oder Konfiguration über Azure Resource Manager in Azure Stack zu ermöglichen. 

In diesem Tutorial erstellen Sie eine Beispielumgebung, die Folgendes ermöglicht:

> [!div class="checklist"]
> * Einrichten einer Hybrididentität in globalen Azure- und Azure Stack-Instanzen
> * Abrufen eines Tokens für den Zugriff auf die Azure Stack-API

Für diese Schritte benötigen Sie Azure Stack-Bedienerberechtigungen.

## <a name="create-a-service-principal-for-azure-ad-in-the-portal"></a>Erstellen eines Dienstprinzipals für Azure AD über das Portal

Wenn Sie Azure Stack mit Azure AD als Identitätsspeicher bereitgestellt haben, können Sie Dienstprinzipale genauso wie für Azure erstellen. In [diesem Dokument](https://docs.microsoft.com/en-us/azure/azure-stack/user/azure-stack-create-service-principals#create-service-principal-for-azure-ad) erfahren Sie, wie die erforderlichen Schritte über das Portal ausgeführt werden. Vergewissern Sie sich vorher, ob Sie über die [erforderlichen Azure AD-Berechtigungen](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-create-service-principal-portal#required-permissions) verfügen.

## <a name="create-a-service-principal-for-ad-fs-using-powershell"></a>Erstellen eines Dienstprinzipals für AD FS mithilfe von PowerShell

Wenn Sie Azure Stack mit AD FS bereitgestellt haben, können Sie PowerShell verwenden, um einen Dienstprinzipal zu erstellen, eine Rolle für den Zugriff zuzuweisen und die Anmeldung über PowerShell mit dieser Identität durchzuführen. In [diesem Dokument](https://docs.microsoft.com/en-us/azure/azure-stack/user/azure-stack-create-service-principals#create-service-principal-for-ad-fs) erfahren Sie, wie die erforderlichen Schritte mithilfe von PowerShell ausgeführt werden.

## <a name="using-the-azure-stack-api"></a>Verwenden der Azure Stack-API

In [diesem Tutorial](https://docs.microsoft.com/en-us/azure/azure-stack/user/azure-stack-rest-api-use) erfahren Sie Schritt für Schritt, wie Sie ein Token für den Zugriff auf die Azure Stack-API abrufen.

## <a name="connect-to-azure-stack-using-powershell"></a>Herstellen einer Verbindung mit Azure Stack mithilfe von PowerShell

Hier finden Sie ein Beispielskript, in dem die in diesem Dokument behandelten Konzepte verwendet werden, um eine Verbindung mit Ihrer Azure Stack-Instanz herzustellen.

### <a name="prerequisites"></a>Voraussetzungen

Eine mit Azure Active Directory verbundene Azure Stack-Installation mit einem Abonnement, auf das Sie zugreifen können. Sollten Sie über keine Azure Stack-Installation verfügen, können Sie anhand [dieser Anleitung](https://docs.microsoft.com/en-us/azure/azure-stack/asdk/asdk-deploy) ein Azure Stack Development Kit einrichten.

In [diesem Tutorial](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-powershell-configure-quickstart) erfahren Sie Schritt für Schritt, wie Sie Azure PowerShell installieren und eine Verbindung mit Ihrer Azure Stack-Installation herstellen.

#### <a name="connect-to-azure-stack-using-code"></a>Herstellen einer Verbindung mit Azure Stack mithilfe von Code

Wenn Sie mithilfe von Code eine Verbindung mit Azure Stack herstellen möchten, verwenden Sie die Endpunkte-API von Azure Resource Manager, um die Authentifizierung und die Graph-Endpunkte für Ihre Azure Stack-Installation abzurufen, und führen Sie dann die Authentifizierung mithilfe von REST-Anforderungen durch. Eine Beispielanwendung finden Sie [hier](https://github.com/shriramnat/HybridARMApplication).

> [!Note]  
Wenn Sie das Azure SDK für Azure Stack verwenden möchten, muss es Azure-API-Profile für die gewählte Sprache unterstützen. Weitere Informationen zu Azure-API-Profilen finden Sie [hier](https://docs.microsoft.com/da-dk/azure/azure-stack/user/azure-stack-version-profiles).

## <a name="next-steps"></a>Nächste Schritte

 - Weitere Informationen zur Behandlung der Identität in Azure Stack finden Sie unter [Identitätsarchitektur für Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-identity-architecture).  
 - Weitere Informationen zu Azure-Cloudmustern finden Sie unter [Cloudentwurfsmuster](https://docs.microsoft.com/azure/architecture/patterns).
