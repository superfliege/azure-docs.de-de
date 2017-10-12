---
title: "Entwickeln von Apps für Azure Stack | Microsoft-Dokumentation"
description: "Hier finden Sie Überlegungen zur Entwicklung bei der Prototyperstellung von Anwendungen in Azure Stack."
services: azure-stack
documentationcenter: 
author: HeathL17
manager: byronr
editor: 
ms.assetid: d3ebc6b1-0ffe-4d3e-ba4a-388239d6cdc3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: helaw
ms.openlocfilehash: 062c17173f87eec8e0eaa3f74323cbf8a8f48571
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="develop-for-azure-stack"></a>Entwickeln für Azure Stack
Sie können noch heute mit der Entwicklung von Anwendungen loslegen, auch wenn Sie keinen Zugriff auf eine Azure Stack-Umgebung haben. Da Azure Stack Microsoft Azure-Dienste bereitstellt, die in Ihrem Datencenter ausgeführt werden, können Sie ähnliche Tools und Prozesse zum Entwickeln für Azure Stack verwenden wie für Azure.  Mit etwas Vorbereitung sowie den Informationen aus den folgenden Themen können Sie Azure zum Emulieren einer Azure Stack-Umgebung nutzen:

* In Azure haben Sie die Möglichkeit, Azure Resource Manager-Vorlagen zu erstellen, die auch in Azure Stack bereitgestellt werden können.  Sehen Sie sich unter [Azure Resource Manager template considerations](azure-stack-develop-templates.md) (Überlegungen zu Azure Resource Manager-Vorlagen) die Anleitungen zum Entwickeln von Vorlagen an, um die Portabilität zu gewährleisten.
* Zwischen Azure und Azure Stack gibt es eine Abweichung in Bezug auf Dienstverfügbarkeit und Dienstversionsverwaltung. Mit dem [Azure Stack-Richtlinienmodul](azure-stack-policy-module.md) können Sie die Azure-Dienstverfügbarkeit und Azure-Ressourcentypen auf das Angebot von Azure Stack beschränken. Durch die Einschränkung verfügbarer Dienste nutzt Ihre Anwendung Dienste, die für Azure Stack verfügbar sind.
* Bei den [Azure Stack-Schnellstartvorlagen](https://github.com/Azure/AzureStack-QuickStart-Templates) handelt es sich um allgemeine Szenariobeispiele dazu, wie Sie Ihre Vorlagen entwickeln, sodass sie sowohl in Azure als auch in Azure Stack bereitgestellt werden können.


