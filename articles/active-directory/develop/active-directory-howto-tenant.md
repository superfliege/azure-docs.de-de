---
title: Einrichten eines Azure AD-Mandanten | Microsoft Docs
description: Informationen zum Einrichten einen Azure Active Directory-Mandanten für das Registrieren und Erstellen von Anwendungen.
services: active-directory
documentationcenter: ''
author: mtillman
manager: mtillman
editor: ''
ms.assetid: 1f4b24eb-ab4d-4baa-a717-2a0e5b8d27cd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/23/2018
ms.author: mtillman
ms.custom: aaddev
ms.openlocfilehash: ab7db49fa07f260de6ebbe4b2cee943b64cab7fe
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2018
---
# <a name="how-to-get-an-azure-active-directory-tenant"></a>Einrichten eines Azure Active Directory-Mandanten
In Azure Active Directory (Azure AD) ist ein [Mandant](https://msdn.microsoft.com/library/azure/jj573650.aspx#Anchor_0) ein Stellvertreter einer Organisation.  Es handelt sich um eine dedizierte Instanz des Azure AD-Diensts, die eine Organisation erhält und besitzt, wenn sie eine Beziehung mit Microsoft eingeht, indem sie sich beispielsweise für einen Microsoft-Clouddienst wie Azure, Microsoft Intune oder Office 365 registriert.  Jeder Azure AD-Mandant ist eindeutig und von anderen Azure AD-Mandanten getrennt.  

Ein Mandant enthält die Benutzer in einem Unternehmen und die dazugehörigen Informationen, wie z. B. Kennwörter, Benutzerprofildaten, Berechtigungen usw.  Er enthält außerdem Gruppen, Anwendungen und andere Informationen, die eine Organisation und ihre Sicherheit betreffen.

Damit sich Azure AD-Benutzer bei Ihrer Anwendung anmelden können, müssen Sie Ihre Anwendung in einem eigenen Mandanten registrieren.  Die Erstellung eines Azure AD-Mandanten und die Veröffentlichung einer Anwendung darin ist **vollkommen kostenlos** (Sie können sich aber dafür entscheiden, für Premium-Features in Ihrem Mandanten zu bezahlen).  Viele Entwickler erstellen für Experimentier-, Entwicklungs-, Staging- und Testzwecke meist mehrere Mandanten und Anwendungen.

## <a name="use-an-existing-azure-ad-tenant"></a>Verwenden eines vorhandenen Azure AD-Mandanten

Viele Entwickler sind bereits über Dienste oder Abonnements, die mit Azure AD-Mandanten verknüpft sind (z.B. Office 365- oder Azure-Abonnements), im Besitz von Mandanten.  Wenn Sie überprüfen möchten, ob Sie bereits über einen Mandanten verfügen, können Sie sich am [Azure-Portal](https://portal.azure.com) mit dem Konto anmelden, das Sie zum Verwalten Ihrer Anwendung verwenden möchten. Achten Sie dann auf die obere rechte Ecke, in der Ihre Kontoinformationen angegeben sind.  Falls Sie einen Mandanten besitzen, werden Sie daran automatisch angemeldet, und der Mandantenname wird direkt unter dem Kontonamen angezeigt.  Wenn Ihr Konto mehreren Mandanten zugeordnet ist, können Sie auf Ihren Kontonamen klicken, um ein Menü zu öffnen, in dem Sie zwischen Mandanten wechseln können.

Sollte Ihrem Konto kein vorhandener Mandant zugeordnet sein, wird unter Ihrem Kontonamen eine GUID angezeigt. Sie können Aktionen wie das Registrieren von Apps dann erst durchführen, nachdem Sie [einen neuen Mandanten erstellt haben](#create-a-new-azure-ad-tenant).

## <a name="create-a-new-azure-ad-tenant"></a>Erstellen eines neuen Azure AD-Mandanten

Falls Sie nicht bereits über einen Azure AD-Mandanten verfügen oder einen neuen erstellen möchten, können Sie hierfür die [Benutzeroberfläche für die Verzeichniserstellung](https://portal.azure.com/#create/Microsoft.AzureActiveDirectory) im [Azure-Portal](https://portal.azure.com) verwenden.  Der Prozess dauert ungefähr eine Minute, und am Ende werden Sie aufgefordert, zu Ihrem neu erstellten Mandanten zu navigieren.