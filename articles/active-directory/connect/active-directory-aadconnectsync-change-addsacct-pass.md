---
title: "Azure AD Connect-Synchronisierung: Ändern des Azure AD DS-Kontokennworts | Microsoft-Dokumentation"
description: "In diesem Dokument wird die Aktualisierung von Azure AD Connect nach dem Ändern des Kennworts für das AD DS-Konto beschrieben."
services: active-directory
keywords: AD DS-Konto, Active Directory-Konto, Kennwort
documentationcenter: 
author: cychua
manager: femila
editor: 
ms.assetid: 76b19162-8b16-4960-9e22-bd64e6675ecc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 14e16a238e60ecfeeb3cbf88c3922a79349dcc75
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="changing-the-ad-ds-account-password"></a>Ändern des AD DS-Kontokennworts
Die AD DS-Konto bezieht sich auf das Benutzerkonto, das von Azure AD Connect zur Kommunikation mit dem lokalen Active Directory verwendet. Wenn Sie das Kennwort des AD DS-Kontos ändern, müssen Sie den Azure AD Connect-Synchronisierungsdienst mit dem neuen Kennwort aktualisieren. Andernfalls kann die Synchronisierung mit dem lokalen Active Directory nicht mehr ordnungsgemäß durchgeführt werden, und es treten die folgenden Fehler auf:

* Im Synchronization Service Manager tritt bei jedem Import- oder Exportvorgang mit einem lokalen AD der Fehler **no-start-credentials** auf.

* In der Windows-Ereignisanzeige enthält das Anwendungsereignisprotokoll einen Fehler mit der **Ereignis-ID 6000** und der Meldung **Der Verwaltungs-Agent „contoso.com“ konnte nicht ausgeführt werden, da die Anmeldeinformationen ungültig waren**.


## <a name="how-to-update-the-synchronization-service-with-new-password-for-ad-ds-account"></a>Aktualisieren des Synchronisierungsdiensts mit dem neuen Kennwort für das AD DS-Konto
So aktualisieren Sie den Synchronisierungsdienst mit dem neuen Kennwort

1. Starten Sie Synchronization Service Manager („START“ > „Synchronization Service“).
</br>![Synchronization Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)  

2. Wechseln Sie zur Registerkarte **Connectors**.

3. Wählen Sie den **AD-Connector** für das AD DS-Konto aus, dessen zugehöriges Kennwort geändert wurde.

4. Klicken Sie unter **Aktionen** auf **Eigenschaften**.

5. Wählen Sie im Popup-Dialogfeld **Mit Active Directory-Gesamtstruktur verbinden** aus:

6. Geben Sie das neue Kennwort des AD DS-Kontos im Textfeld **Kennwort** ein.

7. Klicken Sie auf **OK**, um das neue Kennwort zu speichern und das Popup-Dialogfeld zu schließen.

8. Starten Sie den Azure AD Connect-Synchronisierungsdienst im Dienststeuerungs-Manager von Windows neu. Dadurch wird sichergestellt, dass alle Verweise auf das alte Kennwort aus dem Arbeitsspeichercache entfernt werden.

## <a name="next-steps"></a>Nächste Schritte
**Übersichtsthemen**

* [Azure AD Connect-Synchronisierung: Grundlagen und Anpassung der Synchronisierung](active-directory-aadconnectsync-whatis.md)

* [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md)
