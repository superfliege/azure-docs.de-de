---
title: Was ist Baselineschutz beim bedingten Zugriff in Azure Active Directory? – Vorschauversion | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie durch den Baselineschutz sicherstellen, dass in Ihrer Azure Active Directory-Umgebung mindestens eine Basisabsicherung aktiviert ist.
services: active-directory
keywords: bedingter Zugriff auf Apps, bedingter Zugriff mit Azure AD, sicherer Zugriff auf Unternehmensressourcen, Richtlinien für bedingten Zugriff
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.subservice: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/08/2018
ms.author: markvi
ms.reviewer: nigu
ms.collection: M365-identity-device-management
ms.openlocfilehash: 79cde42056d3943b8a3e15f46c57fdf6a0730de4
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56166163"
---
# <a name="what-is-baseline-protection-preview"></a>Was ist Baselineschutz (Vorschauversion)?  

Im vergangenen Jahr sind Angriffe auf Identitäten um 300 % gestiegen. Zum Schutz Ihrer Umgebung vor den ständig zunehmenden Angriffen wird in Azure Active Directory (Azure AD) ein neues Feature mit dem Namen Baselineschutz eingeführt. Baselineschutz umfasst mehrere vordefinierte [Richtlinien für den bedingten Zugriff](../active-directory-conditional-access-azure-portal.md). Ziel dieser Richtlinien ist, sicherzustellen, dass in allen Editionen von Azure AD mindestens der Baselineschutz aktiviert ist. 

Dieser Artikel gibt Ihnen einen Überblick über den Baselineschutz in Azure Active Directory.


 
## <a name="require-mfa-for-admins"></a>Anfordern von MFA für Administratoren

Benutzer mit Zugriff auf privilegierte Konten haben uneingeschränkten Zugriff auf Ihre Umgebung. Aufgrund der weitreichenden Befugnisse, die diese Konten haben, sollten Sie sie mit Bedacht verwalten. Eine gängige Methode, um den Schutz von privilegierten Konten zu verbessern, ist eine stärkere Form der Kontoüberprüfung bei der Anmeldung. In Azure Active Directory können Sie eine striktere Kontoüberprüfung erreichen, indem Sie eine mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) anfordern.  

Beim **Anfordern von MFA für Administratoren** handelt es sich um eine Basisrichtlinie, die MFA für die folgenden AD-Rollen anfordert: 

- Globaler Administrator  

- SharePoint-Administrator  

- Exchange-Administrator  

- Administrator für bedingten Zugriff  

- Sicherheitsadministrator  


![Azure Active Directory](./media/baseline-protection/01.png)

In dieser Basisrichtlinie haben Sie die Möglichkeit, Benutzer auszuschließen. Möglicherweise möchten Sie ein *[Administratorkonto für den Notfallzugriff](../users-groups-roles/directory-emergency-access.md)* ausschließen, um sicherzustellen, dass Sie für den Mandanten nicht gesperrt werden.


## <a name="enable-a-baseline-policy"></a>Aktivieren einer Basisrichtlinie 

Solange sich die Basisrichtlinien in der Vorschau befinden, sind sie standardmäßig nicht aktiviert. Sie müssen eine Richtlinie manuell aktivieren, wenn Sie sie aktivieren möchten. Wenn Sie die Basisrichtlinien in der Vorschauphase explizit aktivieren, bleiben sie aktiv, wenn dieses Feature den Status der allgemeinen Verfügbarkeit erreicht. Die geplante Verhaltensänderung ist der Grund, warum Sie neben der Aktivierung und Deaktivierung noch eine dritte Option zum Festlegen des Zustands einer Richtlinie haben: **Richtlinie zukünftig automatisch aktivieren**. Wenn Sie diese Option auswählen, können Sie die Deaktivierung der Richtlinien während der Vorschauphase beibehalten und sie dann von Microsoft automatisch aktivieren lassen, wenn dieses Feature den Status der allgemeinen Verfügbarkeit erreicht. Wenn Sie die Basisrichtlinien jetzt nicht explizit aktivieren und auch die Option **Richtlinie zukünftig automatisch aktivieren** nicht aktivieren, bleiben die Richtlinien deaktiviert, wenn dieses Feature die allgemeine Verfügbarkeit erreicht.


**So aktivieren Sie eine Basisrichtlinie:**  

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als globaler Administrator, Sicherheitsadministrator oder Administrator für bedingten Zugriff an.

2. Klicken Sie im **Azure-Portal** auf der linken Navigationsleiste auf **Azure Active Directory**.

    ![Azure Active Directory](./media/baseline-protection/02.png)

3. Klicken Sie auf der Seite **Azure Active Directory** im Abschnitt **Sicherheit** auf **Bedingter Zugriff**.

    ![Bedingter Zugriff](./media/baseline-protection/05.png)

4. Klicken Sie in der Liste der Richtlinien auf eine Richtlinie, die mit **Basisrichtlinie:** beginnt. 

5. Klicken Sie zum Aktivieren der Richtlinie auf **Richtlinie sofort verwenden**.

6. Klicken Sie auf **Speichern**. 
 
  
 

## <a name="what-you-should-know"></a>Wichtige Informationen 

Während für die Verwaltung benutzerdefinierter Richtlinien für den bedingten Zugriff eine Azure AD Premium-Lizenz erforderlich ist, sind die Basisrichtlinien in allen Editionen von Azure AD verfügbar.     

Bei den Verzeichnisrollen, die in der Basisrichtlinie enthalten sind, handelt es sich um Azure AD-Rollen mit den höchsten Berechtigungen. 

Wenn Sie über privilegierte Konten verfügen, die in Ihren Skripts verwendet werden, sollten Sie diese Konten durch [verwaltete Identitäten für Azure-Ressourcen](../managed-identities-azure-resources/overview.md) oder [Dienstprinzipale mit Zertifikaten](../develop/howto-authenticate-service-principal-powershell.md) ersetzen. Als vorläufige Problemumgehung können Sie bestimmte Benutzerkonten von der Basisrichtlinie ausschließen. 

Die Basisrichtlinien gelten für ältere Authentifizierungsabläufe, z.B. POP, IMAP oder ältere Office-Desktopclients. 




## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter

- [Fünf Schritte zum Sichern Ihrer Identitätsinfrastruktur](https://docs.microsoft.com/azure/security/azure-ad-secure-steps)

- [Was ist der bedingte Zugriff in Azure Active Directory?](overview.md) 

