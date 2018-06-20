---
title: Einrichten eines Azure AD-Mandanten | Microsoft Docs
description: Informationen zum Einrichten einen Azure Active Directory-Mandanten für das Registrieren und Erstellen von Anwendungen.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 1f4b24eb-ab4d-4baa-a717-2a0e5b8d27cd
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/23/2018
ms.author: celested
ms.custom: aaddev
ms.openlocfilehash: 26d336c9d896859d8012b1eaad1a16d0cbd9b628
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34596208"
---
# <a name="how-to-get-an-azure-active-directory-tenant"></a>Einrichten eines Azure Active Directory-Mandanten

In Azure Active Directory (Azure AD) ist ein [Mandant](https://msdn.microsoft.com/library/azure/jj573650.aspx#Anchor_0) ein Stellvertreter einer Organisation. Es handelt sich um eine dedizierte Instanz des Azure AD-Diensts, die eine Organisation erhält und besitzt, wenn sie eine Beziehung mit Microsoft eingeht, indem sie sich beispielsweise für einen Microsoft-Clouddienst wie Azure, Microsoft Intune oder Office 365 registriert. Jeder Azure AD-Mandant ist eindeutig und von anderen Azure AD-Mandanten getrennt. 

Ein Mandant enthält die Benutzer in einem Unternehmen und die dazugehörigen Informationen wie etwa Kennwörter, Benutzerprofildaten, Berechtigungen und Ähnliches. Er enthält außerdem Gruppen, Anwendungen und andere Informationen, die eine Organisation und ihre Sicherheit betreffen.

Damit sich Azure AD-Benutzer bei Ihrer Anwendung anmelden können, müssen Sie Ihre Anwendung in einem eigenen Mandanten registrieren. Die Erstellung eines Azure AD-Mandanten und die Veröffentlichung einer Anwendung darin sind **kostenlos**. Es stehen allerdings auch kostenpflichtige Premium-Features für Ihren Mandanten zur Verfügung. Viele Entwickler erstellen für Experimentier-, Entwicklungs-, Staging- und Testzwecke meist mehrere Mandanten und Anwendungen.

## <a name="use-an-existing-azure-ad-tenant"></a>Verwenden eines vorhandenen Azure AD-Mandanten

Viele Entwickler verfügen bereits aufgrund von Diensten oder Abonnements, die mit Azure AD-Mandanten verknüpft sind (beispielsweise Office 365- oder Azure-Abonnements), über Mandanten. Wenn Sie überprüfen möchten, ob Sie bereits über einen Mandanten verfügen, können Sie sich am [Azure-Portal](https://portal.azure.com) mit dem Konto anmelden, das Sie zum Verwalten Ihrer Anwendung verwenden möchten. Achten Sie dann auf die obere rechte Ecke, in der Ihre Kontoinformationen angegeben sind. Falls Sie einen Mandanten besitzen, werden Sie daran automatisch angemeldet, und der Mandantenname wird direkt unter dem Kontonamen angezeigt. Wenn Sie rechts oben im Azure-Portal auf Ihren Kontonamen zeigen, werden Ihr Name, Ihre E-Mail-Adresse, Ihr Verzeichnis und Ihre Mandanten-ID (GUID) sowie Ihre Domäne angezeigt. Falls Ihr Konto mehreren Mandanten zugeordnet ist, können Sie durch Klicken auf Ihren Kontonamen ein Menü öffnen, über das Sie zwischen Mandanten wechseln können. Jeder Mandant besitzt eine eigene Mandanten-ID.

> [!TIP]
> Die Mandanten-ID kann auf unterschiedliche Weise ermittelt werden. Sie können auf Ihren Kontonamen zeigen, um die Mandanten-ID zu erhalten, oder im Portal auf **Azure Active Directory > Eigenschaften > Verzeichnis-ID** klicken.

Sollte Ihrem Konto kein vorhandener Mandant zugeordnet sein, wird unter Ihrem Kontonamen eine GUID angezeigt. In diesem Fall sind Aktionen wie das Registrieren von Apps erst möglich, nachdem Sie [einen neuen Mandanten erstellt haben](#create-a-new-azure-ad-tenant).

## <a name="create-a-new-azure-ad-tenant"></a>Erstellen eines neuen Azure AD-Mandanten

Falls Sie nicht bereits über einen Azure AD-Mandanten verfügen oder einen neuen erstellen möchten, können Sie hierfür die [Benutzeroberfläche für die Verzeichniserstellung](https://portal.azure.com/#create/Microsoft.AzureActiveDirectory) im [Azure-Portal](https://portal.azure.com) verwenden. Der Prozess dauert ungefähr eine Minute, und am Ende werden Sie aufgefordert, zu Ihrem neu erstellten Mandanten zu navigieren.
