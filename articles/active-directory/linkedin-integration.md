---
title: Konfigurieren der LinkedIn-Integration in Azure AD | Microsoft-Dokumentation
description: "Erklärt, wie Sie die LinkedIn-Integration für Microsoft-Apps in Azure Active Directory aktivieren oder deaktivieren."
services: active-directory
author: jeffgilb
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: jeffgilb
ms.reviewer: jsnow
ms.custom: it-pro
ms.openlocfilehash: 48b26bfcce67ce915c404a0ab2ac0f399c3b821d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="enabling-linkedin-integration-in-azure-active-directory"></a>Aktivieren der LinkedIn-Integration in Azure Active Directory
Durch die Aktivierung der LinkedIn-Integration können Ihre Benutzer sowohl auf die öffentlichen LinkedIn-Daten als auch, wenn sie möchten, über Microsoft-Apps auf ihr persönliches LinkedIn-Netzwerk zugreifen. Jeder Benutzer kann sich individuell dafür entscheiden, sein Arbeitskonto mit seinem LinkedIn-Konto zu verbinden.

### <a name="linkedin-integration-from-your-end-users-perspective"></a>LinkedIn-Integration aus der Perspektive Ihres Benutzers
Wenn Benutzer in Ihrer Organisation ihre LinkedIn-Konten mit ihren Arbeitskonten verbinden, können sie die Personen, mit denen sie innerhalb und außerhalb der Organisation zusammenarbeiten, besser identifizieren. Durch die Verbindung der beiden Konten können die LinkedIn-Verbindungen und -Profildaten in den Microsoft-Apps Ihrer Organisation verwendet werden. Sie können dies jedoch jederzeit rückgängig machen, indem sie LinkedIn die Berechtigung zum Freigeben dieser Daten entziehen. Die Integration verwendet zudem öffentlich verfügbare Profilinformationen, und Benutzer können über die Datenschutzeinstellungen auswählen, ob sie die Informationen ihres öffentlichen Profils und Netzwerks für Microsoft-Anwendungen freigeben möchten.

>[!NOTE]
> Durch die Aktivierung der LinkedIn-Integration in Azure AD können Apps und Dienste auf einige der Informationen Ihrer Benutzer zugreifen. Weitere Informationen zu den Überlegungen zum Datenschutz bei der LinkedIn-Integration in Azure AD finden Sie in der [Datenschutzerklärung von Microsoft](https://privacy.microsoft.com/privacystatement/). 

## <a name="enable-linkedin-integration"></a>Aktivieren der LinkedIn-Integration
Die LinkedIn-Integration für Unternehmen ist in Azure AD standardmäßig aktiviert. Wenn diese Funktion also Ihrem Mandanten zur Verfügung gestellt wird, werden allen Benutzern in Ihrer Organisation LinkedIn-Funktionen und -Profile angezeigt. Durch die Aktivierung der LinkedIn-Integration können Benutzer in Ihrer Organisation LinkedIn-Funktionen innerhalb von Microsoft-Diensten verwenden, z.B. das Anzeigen von Profilen beim Empfang einer E-Mail in Outlook. Durch die Deaktivierung dieser Funktion wird der Zugriff auf LinkedIn-Funktionen verhindert sowie die Verbindung von Benutzerkonten und das Teilen von Daten zwischen LinkedIn und Ihrer Organisation über Microsoft-Dienste gestoppt.

> [!IMPORTANT]
> Diese Funktion steht für „Go Local!“-Mandanten, unabhängige Mandanten und Mandanten von Behörden nicht zur Verfügung. Darüber hinaus werden Azure AD-Dienstupdates wie die Funktionen der LinkedIn-Integration nicht für alle Azure-Mandanten gleichzeitig bereitgestellt. Die können die LinkedIn-Integration mit Azure AD erst aktivieren, wenn diese Funktion für Ihren Azure-Mandanten eingeführt wurde.

1. Melden Sie sich beim [Azure Active Directory Admin Center](https://aad.portal.azure.com/) über ein Konto an, das als globaler Administrator für das Verzeichnis konfiguriert ist.
2. Wählen Sie **Benutzer und Gruppen**.
3. Wählen Sie auf dem Blatt **Benutzer und Gruppen** die Option **Benutzereinstellungen** aus.
4. Wählen Sie unter **LinkedIn-Integration** „Ja“ oder „Nein“ aus, um die LinkedIn-Integration zu aktivieren oder zu deaktivieren.
   ![Aktivieren der LinkedIn-Integration](./media/linkedin-integration/LinkedIn-integration.PNG)

### <a name="learn-more"></a>Weitere Informationen 
* [LinkedIn information and features in your Microsoft apps (LinkedIn-Informationen und -funktionen in Ihren Microsoft-Apps)](https://go.microsoft.com/fwlink/?linkid=850740)

* [LinkedIn-Hilfe](https://www.linkedin.com/help/linkedin)

## <a name="next-steps"></a>Nächste Schritte
Sie können den folgenden Link verwenden, um die LinkedIn-Integration in Azure AD über das Azure-Portal zu aktivieren oder zu deaktivieren:

[Konfigurieren der LinkedIn-Integration](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/UserSettings) 