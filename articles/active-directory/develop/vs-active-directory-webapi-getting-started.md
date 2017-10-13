---
title: Erste Schritte mit Azure AD in Visual Studio-WebApi-Projekten | Microsoft-Dokumentation
description: "Erfahren Sie etwas über die ersten Schritte mit Azure Active Directory in WebApi-Projekten nach dem Herstellen einer Verbindung oder dem Erstellen eines Azure AD mithilfe von verbundenen Visual Studio-Diensten."
services: active-directory
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: bf1eb32d-25cd-4abf-8679-2ead299fedaa
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 03/19/2017
ms.author: kraigb
ms.custom: aaddev
ms.openlocfilehash: a756316054dd3bb63f3b0a9f59621bb1345bc693
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-azure-active-directory-and-visual-studio-connected-services-webapi-projects"></a>Erste Schritte mit Azure Active Directory und verbundenen Visual Studio-Diensten (WebApi-Projekte)
> [!div class="op_single_selector"]
> * [Erste Schritte](vs-active-directory-webapi-getting-started.md)
> * [Was ist passiert?](vs-active-directory-webapi-what-happened.md)
> 
> 

## <a name="requiring-authentication-to-access-controllers"></a>Erfordern von Authentifizierung für den Zugriff auf Controller
Alle Controller in Ihrem Projekt wurden mit dem Attribut **Authorize** versehen. Dieses Attribut erfordert, dass der Benutzer vor dem Zugriff auf die durch diese Controller definierten APIs authentifiziert werden muss. Wenn Sie anonymen Zugriff auf diesen Controller erlauben möchten, entfernen Sie dieses Attribut vom Controller. Wenn Sie die Berechtigungen präziser festlegen möchten, wenden Sie das Attribut auf jede Methode an, die Autorisierung erfordert, anstatt es auf die Controllerklasse anzuwenden.

## <a name="next-steps"></a>Nächste Schritte
[Weitere Informationen zu Azure Active Directory](https://azure.microsoft.com/services/active-directory/)

