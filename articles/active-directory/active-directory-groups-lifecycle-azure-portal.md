---
title: Ablauf für Office 365-Gruppen in Azure Active Directory | Microsoft-Dokumentation
description: Einrichten des Ablaufs für Office 365-Gruppen in Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 03/09/2018
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: it-pro
ms.openlocfilehash: 1e500be80374edd4d2f53fd2f0604e4a63f640fc
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
ms.locfileid: "33761390"
---
# <a name="configure-the-expiration-policy-for-office-365-groups"></a>Konfigurieren der Ablaufrichtlinie für Office 365-Gruppen

Der Lebenszyklus von Office 365-Gruppen kann jetzt durch das Festlegen einer Ablaufrichtlinie verwaltet werden. Sie können eine Ablaufrichtlinie ausschließlich für Office 365-Gruppen in Azure Active Directory (Azure AD) festlegen. 

Das Festlegen eines Ablaufs für eine Gruppe bewirkt Folgendes:
-   Besitzer der Gruppe werden vor Erreichen des Ablaufs aufgefordert, die Gruppe zu erneuern.
-   Nicht erneuerte Gruppen werden gelöscht.
-   Gelöschte Office 365-Gruppen können innerhalb von 30 Tagen von den Gruppenbesitzern oder vom Administrator wiederhergestellt werden.

> [!NOTE]
> Beim Konfigurieren und Verwenden der Ablaufrichtlinie für Office 365-Gruppen müssen Sie über Azure AD Premium-Lizenzen für alle Mitglieder der Gruppen verfügen, auf die die Ablaufrichtlinie angewendet wird.

Informationen zum Herunterladen und Installieren der Azure AD-PowerShell-Cmdlets finden Sie unter [Azure Active Directory PowerShell for Graph – 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137) (Azure Active Directory: PowerShell für Graph – 2.0.0.137).

## <a name="roles-and-permissions"></a>Rollen und Berechtigungen
Nachfolgend sind Rollen aufgeführt, mit denen der Ablauf für Office 365-Gruppen in Azure AD konfiguriert und verwendet werden kann.

Rolle | Berechtigungen
-------- | --------
Globaler Administrator oder Benutzerkontoadministrator | Kann die Einstellungen der Ablaufrichtlinie für Office 365-Gruppen erstellen, lesen, aktualisieren und löschen.<br>Kann beliebige Office 365-Gruppen verlängern
Benutzer | Kann eine eigene Office 365-Gruppe verlängern<br>Kann eine eigene Office 365-Gruppe wiederherstellen<br>Kann die Einstellungen der Ablaufrichtlinie lesen

Weitere Informationen zu Berechtigungen zum Wiederherstellen von gelöschten Gruppen finden Sie unter [Wiederherstellen einer gelöschten Office 365-Gruppe in der Azure Active Directory-Vorschau](active-directory-groups-restore-azure-portal.md).

## <a name="set-group-expiration"></a>Festlegen des Gruppenablaufs

1. Öffnen Sie das [Azure AD Admin Center](https://aad.portal.azure.com) mit dem Konto eines globalen Administrators in Ihrem Azure AD-Mandanten.

2. Wählen Sie **Gruppen**, und wählen Sie dann **Ablauf**, um die Ablaufeinstellungen zu öffnen.
  
  ![Blatt „Ablauf“](./media/active-directory-groups-lifecycle-azure-portal/expiration-settings.png)

4. Auf dem Blatt **Ablauf** können Sie:

  * Die Gruppenlebensdauer in Tagen festlegen. Sie könnten einen der vordefinierten Werte oder einen benutzerdefinierten Wert wählen (mindestens 31 Tage). 
  * Geben Sie eine E-Mail-Adresse an, an die die Verlängerungs- und Ablaufbenachrichtigungen gesendet werden sollten, wenn eine Gruppe keinen Besitzer hat. 
  * Wählen Sie, welche Office 365-Gruppen ablaufen. Sie können den Ablauf für **alle** Office 365-Gruppen aktivieren oder eine **Auswahl** aus den Office 365-Gruppen treffen, oder Sie wählen **Keine**, um den Ablauf für alle Gruppen zu deaktivieren.
  * Speichern Sie die Einstellungen durch Auswahl von **Speichern**.


E-Mail-Benachrichtigungen, wie diese werden an die Office 365-Gruppenbesitzer 30 Tage, 15 Tage und 1 Tag vor Ablauf der Gruppe gesendet.

![E-Mail-Benachrichtigung zum Ablauf](./media/active-directory-groups-lifecycle-azure-portal/expiration-notification.png)

Aus der Benachrichtigungs-E-Mail zu **Gruppe verlängern** können Gruppenbesitzer direkt auf die Seite mit den Gruppendetails im Zugriffsbereich zugreifen. Auf dieser Seite erhalten Benutzer weitere Informationen zur Gruppe, z.B. Beschreibung, Zeitpunkt der letzten Verlängerung, Ablaufzeitpunkt und Möglichkeit zur Verlängerung der Gruppe. Die Seite mit den Gruppendetails enthält jetzt auch Links zu den Office 365-Gruppenressourcen, sodass der Gruppenbesitzer den Inhalt und die Aktivitäten der Gruppe bequem anzeigen kann.

Wenn eine Gruppe abläuft, wird die Gruppe einen Tag nach dem Ablaufdatum gelöscht. Eine E-Mail-Benachrichtigung wie diese wird an die Office 365-Gruppenbesitzer gesendet, um sie über den Ablauf und die nachfolgende Löschung ihrer Office 365-Gruppe zu informieren.

![E-Mail-Benachrichtigung zur Gruppenlöschung](./media/active-directory-groups-lifecycle-azure-portal/deletion-notification.png)

Die Gruppe kann innerhalb von 30 Tagen nach ihrer Löschung wiederhergestellt werden, indem **Gruppe wiederherstellen** gewählt oder PowerShell-Cmdlets verwendet werden. Dies ist unter [Wiederherstellen einer gelöschten Office 365-Gruppe in der Azure Active Directory-Vorschau](active-directory-groups-restore-azure-portal.md) beschrieben.
    
Wenn die Gruppe, die Sie wiederherstellen, Dokumente, SharePoint-Websites oder andere beständige Objekte enthält, kann es bis zu 24 Stunden dauern, bis die Gruppe und deren Inhalte vollständig wiederhergestellt werden.

> [!NOTE]
> * Beim erstmaligen Einrichten des Ablaufs wird für alle Gruppen, deren Alter das Ablaufintervall übersteigt, ein Ablaufzeitraum von 30 Tagen festgelegt. Die erste Verlängerungsbenachrichtigung wird innerhalb eines Tages gesendet. 
>   Beispielsweise wurde Gruppe A vor 400 Tagen erstellt, und das Ablaufintervall ist auf 180 Tage festgelegt. Wenn Sie Ablaufeinstellungen anwenden, hat Gruppe A 30 Tage, bevor sie gelöscht wird, es sei denn, der Besitzer verlängert sie.
> * Derzeit kann für Office 365-Gruppen auf einem Mandanten nur eine Ablaufrichtlinie konfiguriert werden.
> * Wenn eine dynamische Gruppe gelöscht und wiederhergestellt wird, gilt sie als neue Gruppe und wird der Regel entsprechend erneut aufgefüllt. Dieser Vorgang kann bis zu 24 Stunden dauern.

## <a name="how-office-365-group-expiration-works-with-a-mailbox-on-legal-hold"></a>Funktionsweise des Office 365-Gruppenablaufs mit einem Postfach mit gesetzlicher Aufbewahrungspflicht
Wenn eine Gruppe abläuft und gelöscht wird, werden die Daten der Gruppe für Apps wie Planner, Sites oder Teams 30 Tage nach dem Löschvorgang endgültig gelöscht. Das Postfach der Gruppe, für das eine gesetzliche Aufbewahrungspflicht gilt, wird beibehalten und nicht endgültig gelöscht. Der Administrator kann Exchange-Cmdlets verwenden, um das Postfach zum Abrufen der Daten wiederherzustellen. 

## <a name="how-office-365-group-expiration-works-with-retention-policy"></a>Funktionsweise des Office 365-Gruppenablaufs mit Aufbewahrungsrichtlinie
Die Aufbewahrungsrichtlinie wird über das Security & Compliance Center konfiguriert. Wenn Sie eine Aufbewahrungsrichtlinie für Office 365-Gruppen eingerichtet haben, werden die Gruppenunterhaltungen im Postfach der Gruppe und die Dateien auf der Site der Gruppe nach dem Ablauf und der Löschung einer Gruppe im Aufbewahrungscontainer gemäß der Angabe in der Aufbewahrungsrichtlinie (in Tagen) beibehalten. Benutzer können die Gruppe oder ihren Inhalt nach dem Ablauf nicht mehr anzeigen, aber sie können die Site- und Postfachdaten per E-Discovery wiederherstellen.

## <a name="powershell-examples"></a>PowerShell-Beispiele
Hier sind Beispiele dafür angegeben, wie Sie PowerShell-Cmdlets zum Konfigurieren der Ablaufeinstellungen für Office 365-Gruppen in Ihrem Mandanten verwenden können:

1. Installieren Sie das PowerShell v2.0 Preview-Modul (2.0.0.137), und melden Sie sich an der PowerShell-Eingabeaufforderung an:
  ````
  Install-Module -Name AzureADPreview
  connect-azuread 
  ````
2. Konfigurieren Sie die Ablaufeinstellungen per „New-AzureADMSGroupLifecyclePolicy“: Mit diesem Cmdlet können Sie die Lebensdauer für alle Office 365-Gruppen im Mandanten auf 365 Tage festlegen. Verlängerungsbenachrichtigungen für Office 365-Gruppen ohne Besitzer werden an emailaddress@contoso.com gesendet.
  
  ````
  New-AzureADMSGroupLifecyclePolicy -GroupLifetimeInDays 365 -ManagedGroupTypes All -AlternateNotificationEmails emailaddress@contoso.com
  ````
3. Abrufen der vorhandenen Richtlinie per „Get-AzureADMSGroupLifecyclePolicy“: Mit diesem Cmdlet werden die konfigurierten aktuellen Einstellungen zum Office 365-Gruppenablauf abgerufen. In diesem Beispiel sehen Sie Folgendes:
  * Richtlinien-ID 
  * Festlegung der Lebensdauer für alle Office 365-Gruppen im Mandanten auf 365 Tage
  * Verlängerungsbenachrichtigungen für Office 365-Gruppen ohne Besitzer werden an emailaddress@contoso.com gesendet.
  
  ````
  Get-AzureADMSGroupLifecyclePolicy
  
  ID                                    GroupLifetimeInDays ManagedGroupTypes AlternateNotificationEmails
  --                                    ------------------- ----------------- ---------------------------
  26fcc232-d1c3-4375-b68d-15c296f1f077  365                 All               emailaddress@contoso.com
  ```` 
   
4. Aktualisieren der vorhandenen Richtlinie per „Set-AzureADMSGroupLifecyclePolicy“: Dieses Cmdlet wird zum Aktualisieren einer vorhandenen Richtlinie verwendet. Im folgenden Beispiel wird die Lebensdauer der Gruppe in der vorhandenen Richtlinie von 365 Tagen in 180 Tage geändert. 
  
  ````
  Set-AzureADMSGroupLifecyclePolicy -Id “26fcc232-d1c3-4375-b68d-15c296f1f077”   -GroupLifetimeInDays 180 -AlternateNotificationEmails "emailaddress@contoso.com"
  ````
  
5. Hinzufügen von spezifischen Gruppen zur Richtlinie per „Add-AzureADMSLifecyclePolicyGroup“: Mit diesem Cmdlet wird der Lebenszyklusrichtlinie eine Gruppe hinzugefügt. Beispiel: 
  
  ````
  Add-AzureADMSLifecyclePolicyGroup -Id “26fcc232-d1c3-4375-b68d-15c296f1f077” -groupId "cffd97bd-6b91-4c4e-b553-6918a320211c"
  ````
  
6. Entfernen der vorhandenen Richtlinie per „Policy Remove-AzureADMSGroupLifecyclePolicy“: Mit diesem Cmdlet werden die Einstellungen für den Office 365-Gruppenablauf gelöscht. Für diesen Vorgang ist aber die Richtlinien-ID erforderlich. Hierdurch wird der Ablauf für Office 365-Gruppen deaktiviert. 
  
  ````
  Remove-AzureADMSGroupLifecyclePolicy -Id “26fcc232-d1c3-4375-b68d-15c296f1f077”
  ````
  
Die folgenden Cmdlets können verwendet werden, um die Richtlinie ausführlicher zu konfigurieren. Weitere Informationen finden Sie in der [PowerShell-Dokumentation](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview&branch=master#groups).

* Get-AzureADMSGroupLifecyclePolicy
* New-AzureADMSGroupLifecyclePolicy
* Get-AzureADMSGroupLifecyclePolicy
* Set-AzureADMSGroupLifecyclePolicy
* Remove-AzureADMSGroupLifecyclePolicy
* Add-AzureADMSLifecyclePolicyGroup
* Remove-AzureADMSLifecyclePolicyGroup
* Reset-AzureADMSLifeCycleGroup   
* Get-AzureADMSLifecyclePolicyGroup

## <a name="next-steps"></a>Nächste Schritte
Diese Artikel enthalten zusätzliche Informationen zu Azure AD-Gruppen.

* [Anzeigen vorhandener Gruppen](active-directory-groups-view-azure-portal.md)
* [Verwalten der Einstellungen einer Gruppe](active-directory-groups-settings-azure-portal.md)
* [Verwalten der Mitglieder einer Gruppe](active-directory-groups-members-azure-portal.md)
* [Verwalten der Mitgliedschaften einer Gruppe](active-directory-groups-membership-azure-portal.md)
* [Verwalten dynamischer Regeln für Benutzer in einer Gruppe](active-directory-groups-dynamic-membership-azure-portal.md)
