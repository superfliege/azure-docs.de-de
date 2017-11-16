---
title: Erste Schritte mit Azure Multi-Factor Authentication-Anbietern | Microsoft Docs
description: Lernen Sie, wie Sie einen Azure Multi-Factor Authentication-Anbieter erstellen.
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: a7dd5030-7d40-4654-8fbd-88e53ddc1ef5
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/02/2017
ms.author: joflore
ms.reviewer: richagi
ms.custom: it-pro
ms.openlocfilehash: 71038dd1957f5322acc3d54600481e663d855151
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="getting-started-with-an-azure-multi-factor-authentication-provider"></a>Erste Schritte mit einem Azure Multi-Factor Authentication-Anbieter
Die Überprüfung in zwei Schritten ist standardmäßig für globale Administratoren, die mit Azure Active Directory arbeiten, und für Office 365-Benutzer verfügbar. Falls Sie die [erweiterten Funktionen](multi-factor-authentication-whats-next.md) nutzen möchten, müssen Sie die Vollversion von Azure Multi-Factor Authentication (MFA) erwerben.

Ein Azure Multi-Factor Authentication-Anbieter wird verwendet, um die Features zu nutzen, die mit der Vollversion von Azure MFA bereitgestellt werden. Er wird für Benutzer verwendet, die **keine Lizenzen über Azure MFA, Azure AD Premium oder EMS (Enterprise Mobility + Security) besitzen**.  Azure MFA, Azure AD Premium und EMS enthalten standardmäßig die Vollversion von Azure MFA. Wenn Sie im Besitz von Lizenzen sind, benötigen Sie keinen Azure Multi-Factor Authentication-Anbieter.

Ein Azure Multi-Factor Authentication-Anbieter ist erforderlich, um das SDK herunterzuladen.

> [!IMPORTANT]
> Um das SDK herunterzuladen, müssen Sie einen Azure Multi-Factor Authentication-Anbieter erstellen, auch wenn Sie über Azure MFA-, AAD Premium- oder EMS-Lizenzen verfügen.  Wenn Sie zu diesem Zweck einen Azure Multi-Factor Authentication-Anbieter erstellen und bereits über Lizenzen verfügen, sollten Sie den Anbieter mit dem Modell **Pro aktiviertem Benutzer** erstellen. Verknüpfen Sie den Anbieter anschließend mit dem Verzeichnis, in dem die Lizenzen für Azure MFA, Azure AD Premium oder EMS enthalten sind. Diese Konfiguration stellt sicher, dass nur Kosten berechnet werden, wenn die Anzahl eindeutiger Benutzer, die die Überprüfung in zwei Schritten verwenden, die Anzahl Ihrer Lizenzen übersteigt.

## <a name="what-is-an-mfa-provider"></a>Was ist ein MFA-Anbieter?

Wenn Sie keine Lizenzen für Azure Multi-Factor Authentication besitzen, können Sie einen Authentifizierungsanbieter erstellen, um die Überprüfung in zwei Schritten für Ihre Benutzer erforderlich zu machen. Wenn Sie eine benutzerdefinierte App entwickeln und Azure MFA aktivieren möchten, erstellen Sie einen Authentifizierungsanbieter und [laden Sie das SDK herunter](multi-factor-authentication-sdk.md).

Es gibt zwei Arten von Authentifizierungsanbietern. Der Unterschied besteht darin, wie Ihr Azure-Abonnement in Rechnung gestellt wird. Bei der Option pro Authentifizierung wird die Anzahl der Authentifizierungen berechnet, die für Ihren Mandanten in einem Monat ausgeführt werden. Diese Option ist am besten geeignet, wenn sich mehrere Benutzer nur gelegentlich authentifizieren, beispielsweise wenn Sie MFA für eine benutzerdefinierte Anwendung benötigen. Bei der Option pro Benutzer wird die Anzahl der Personen in Ihrem Mandanten berechnet, die in einem Monat die Überprüfung in zwei Schritten ausführen. Diese Option empfiehlt sich, wenn Sie über einige Benutzer mit Lizenzen verfügen, MFA aber auf weitere Benutzer außerhalb Ihrer Lizenzierungsgrenzwerte erweitern müssen.

## <a name="create-an-mfa-provider---public-preview"></a>Erstellen eines MFA-Anbieters – öffentliche Vorschauversion

Führen Sie die folgenden Schritte aus, um einen Azure Multi-Factor Authentication-Anbieter über das Azure-Portal zu erstellen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als Administrator an. 
2. Klicken Sie auf **Azure Active Directory** > **MFA-Server**.
3. Wählen Sie **Anbieter** aus.
4. Wählen Sie **Hinzufügen**.
5. Füllen Sie die folgenden Felder aus, und klicken Sie anschließend auf **Hinzufügen**:
   - **Name:** Der Name des Anbieters.
   - **Nutzungsmodell:** Sie können eine dieser beiden Optionen wählen:
      * Pro Authentifizierung: Ein Kaufmodell, bei dem die Kosten pro Authentifizierung berechnet werden. Wird in der Regel für Szenarien verwendet, in denen Azure Multi-Factor Authentication in einer kundenorientierten Anwendung verwendet wird.
      * Pro aktiviertem Benutzer: Ein Kaufmodell, bei dem die Kosten pro aktiviertem Benutzer berechnet werden. Wird in der Regel für den Mitarbeiterzugriff auf Anwendungen wie Office 365 verwendet. Wählen Sie diese Option, wenn Sie über einige Benutzer verfügen, die bereits für Azure MFA lizenziert sind.
   - **Abonnement:** Das Azure-Abonnement, über das Aktivitäten für die zweistufige Überprüfung durch den Anbieter abgerechnet werden. 
   - **Verzeichnis:** Der Azure Active Directory-Mandant, dem der Anbieter zugeordnet ist. Bedenken Sie dabei Folgendes:
      * Zum Erstellen eines Anbieters benötigen Sie kein Azure AD-Verzeichnis. Lassen Sie dieses Feld leer, wenn Sie nur den Azure Multi-Factor Authentication-Server oder das Azure Multi-Factor Authentication-SDK herunterladen möchten.
      * Der Anbieter muss einem Azure AD-Verzeichnis zugeordnet werden, um die erweiterten Features nutzen zu können.
      * Einem Azure AD-Verzeichnis kann immer nur ein einzelner Anbieter zugeordnet werden.

## <a name="create-an-mfa-provider"></a>Erstellen eines MFA-Anbieters
Führen Sie die folgenden Schritte aus, um einen Azure Multi-Factor Authentication-Anbieter über das klassische Portal zu erstellen:

1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com) als Administrator an.
2. Wählen Sie im linken Bereich **Active Directory**aus.
3. Wählen Sie oben auf der Seite „Active Directory“ die Option **Anbieter für mehrstufige Authentifizierung**.
   
   ![MFA-Anbieter erstellen](./media/multi-factor-authentication-get-started-auth-provider/authprovider1.png)

4. Klicken Sie dann auf **Neu**.
   
   ![MFA-Anbieter erstellen](./media/multi-factor-authentication-get-started-auth-provider/authprovider2.png)

5. Wählen Sie unter „App Services“ die Option **Anbieter für mehrstufige Authentifizierung**.
   
   ![MFA-Anbieter erstellen](./media/multi-factor-authentication-get-started-auth-provider/authprovider3.png)

6. Wählen Sie **Schnellerfassung**.
   
   ![MFA-Anbieter erstellen](./media/multi-factor-authentication-get-started-auth-provider/authprovider4.png)

7. Füllen Sie die folgenden Felder aus, und wählen Sie **Erstellen**.
   1. **Name:** Der Name des Anbieters.
   2. **Nutzungsmodell:** Sie können eine dieser beiden Optionen wählen:
      * Pro Authentifizierung: Ein Kaufmodell, bei dem die Kosten pro Authentifizierung berechnet werden. Wird in der Regel für Szenarien verwendet, in denen Azure Multi-Factor Authentication in einer kundenorientierten Anwendung verwendet wird.
      * Pro aktiviertem Benutzer: Ein Kaufmodell, bei dem die Kosten pro aktiviertem Benutzer berechnet werden. Wird in der Regel für den Mitarbeiterzugriff auf Anwendungen wie Office 365 verwendet. Wählen Sie diese Option, wenn Sie über einige Benutzer verfügen, die bereits für Azure MFA lizenziert sind.
   3. **Verzeichnis:** Der Azure Active Directory-Mandant, dem der Anbieter zugeordnet ist. Bedenken Sie dabei Folgendes:
      * Zum Erstellen eines Anbieters benötigen Sie kein Azure AD-Verzeichnis. Lassen Sie dieses Feld leer, wenn Sie nur den Azure Multi-Factor Authentication-Server oder das Azure Multi-Factor Authentication-SDK herunterladen möchten.
      * Der Anbieter muss einem Azure AD-Verzeichnis zugeordnet werden, um die erweiterten Features nutzen zu können.
      * Einem Azure AD-Verzeichnis kann immer nur ein einzelner Anbieter zugeordnet werden.  
      ![MFA-Anbieter erstellen](./media/multi-factor-authentication-get-started-auth-provider/authprovider5.png)

8. Nach dem Klicken auf „Erstellen“ wird der Multi-Factor Authentication-Anbieter erstellt und die folgende Meldung angezeigt: **Der Anbieter für Multi-Factor Authentication wurde erstellt**. Klicken Sie auf **OK**.  
   
   ![MFA-Anbieter erstellen](./media/multi-factor-authentication-get-started-auth-provider/authprovider6.png)  

## <a name="manage-your-mfa-provider"></a>Verwalten Ihres MFA-Anbieters

Das Nutzungsmodell (pro aktiviertem Benutzer oder pro Authentifizierung) kann nach der Erstellung eines MFA-Anbieters nicht mehr geändert werden. Sie können allerdings den MFA-Anbieter löschen und dann einen Anbieter mit einem anderen Nutzungsmodell erstellen.

Falls der aktuelle Multi-Factor Authentication-Anbieter mit einem Azure AD-Verzeichnis (auch Azure AD-Mandant genannt) verknüpft ist, können Sie den MFA-Anbieter problemlos löschen und einen neuen erstellen, der mit dem gleichen Azure AD-Mandanten verknüpft ist. Sofern die Anzahl der für MFA, Azure AD Premium oder Enterprise Mobility + Security (EMS) erworbenen Lizenzen für alle Benutzer ausreicht, für die MFA aktiviert ist, können Sie den MFA-Anbieter auch ganz löschen.

Falls Ihr MFA-Anbieter nicht mit einem Azure AD-Mandanten verknüpft ist oder Sie den neuen MFA-Anbieter mit einem anderen Azure AD-Mandanten verknüpfen, werden Benutzereinstellungen und Konfigurationsoptionen nicht übernommen. Darüber hinaus müssen vorhandene Azure MFA-Server unter Verwendung von Aktivierungsanmeldeinformationen des neuen MFA-Anbieters erneut aktiviert werden. Wenn Sie die MFA-Server erneut aktivieren, um sie mit dem neuen MFA-Anbieter zu verknüpfen, hat das keinerlei Auswirkungen auf die Authentifizierung per Telefonanruf oder SMS, aber Benachrichtigungen der mobilen App funktionieren erst wieder, wenn die Benutzer die mobile App erneut aktivieren.

## <a name="next-steps"></a>Nächste Schritte

[Herunterladen des Multi-Factor Authentication-SDKs](multi-factor-authentication-sdk.md)

[Konfigurieren von Azure Multi-Factor Authentication-Einstellungen](multi-factor-authentication-whats-next.md)
