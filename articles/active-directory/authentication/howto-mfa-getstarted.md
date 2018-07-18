---
title: Erste Schritte mit Azure MFA in der Cloud | Microsoft Docs
description: Auf dieser Seite zu Microsoft Azure Multi-Factor Authentication werden die ersten Schritte mit Azure MFA in der Cloud beschrieben.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: get-started-article
ms.date: 06/24/2017
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.openlocfilehash: 0a822d55e8d7bd0d503eb7d77f96dc9e60e1a4ba
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2018
ms.locfileid: "33882867"
---
# <a name="getting-started-with-azure-multi-factor-authentication-in-the-cloud"></a>Erste Schritte mit Azure Multi-Factor Authentication in der Cloud
In diesem Artikel wird Schritt für Schritt beschrieben, wie Sie die ersten Schritte mit Azure Multi-Factor Authentication in der Cloud ausführen.

> [!NOTE]
> Die folgende Dokumentation enthält Informationen zum Aktivieren von Benutzern mithilfe des **Azure-Portals**. Weitere Informationen zum Einrichten von Azure Multi-Factor Authentication für Office 365-Benutzer finden Sie unter [Einrichten der mehrstufigen Authentifizierung für Office 365-Benutzer](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6?ui=en-US&rs=en-US&ad=US).

![MFA in der Cloud](./media/howto-mfa-getstarted/mfa_in_cloud.png)

## <a name="prerequisite"></a>Voraussetzung
[Registrieren Sie sich für ein Azure-Abonnement:](https://azure.microsoft.com/pricing/free-trial/) Falls Sie noch nicht über ein Azure-Abonnement verfügen, müssen Sie sich für ein Abonnement registrieren. Wenn Sie die Verwendung von Azure MFA testen möchten, können Sie ein Testabonnement verwenden.

## <a name="enable-azure-multi-factor-authentication"></a>Aktivieren von Azure Multi-Factor Authentication
Solange Ihre Benutzer über Lizenzen verfügen, die Azure Multi-Factor Authentication enthalten, müssen Sie nichts unternehmen, um Azure MFA zu aktivieren. Sie können die zweistufige Überprüfung auf Benutzerbasis obligatorisch machen. Azure MFA kann mit den folgenden Lizenzen aktiviert werden:
- Azure Multi-Factor Authentication
- Azure Active Directory Premium
- Enterprise Mobility + Security

Falls Sie nicht über eine dieser drei Lizenzen verfügen oder nicht genügend Lizenzen für alle Benutzer haben, ist dies auch kein Problem. Sie müssen lediglich einen zusätzlichen Schritt ausführen und in Ihrem Verzeichnis [einen Anbieter für die Multi-Factor Authentication erstellen](concept-mfa-authprovider.md).

## <a name="turn-on-two-step-verification-for-users"></a>Aktivieren der zweistufigen Überprüfung für Benutzer

Verwenden Sie eine der unter [Vorgehensweise zum Erzwingen einer zweistufigen Überprüfung für einen Benutzer oder eine Gruppe](howto-mfa-userstates.md) aufgeführten Verfahrensweisen, um mit der Verwendung von Azure MFA zu beginnen. Sie können die zweistufige Überprüfung auch für alle Anmeldungen erzwingen oder Richtlinien für bedingten Zugriff erstellen, um die zweistufige Überprüfung nur dann vorauszusetzen, wenn Sie es für wichtig erachten.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie die Azure Multi-Factor Authentication in der Cloud eingerichtet haben, können Sie die Bereitstellung konfigurieren und einrichten. Weitere Informationen finden Sie unter [Konfigurieren von Azure Multi-Factor Authentication](howto-mfa-mfasettings.md).

