---
title: "Ablauf für Office 365-Gruppen in Azure Active Directory | Microsoft-Dokumentation"
description: "Einrichten des Ablaufs für Office 365-Gruppen in Azure Active Directory (Vorschau)"
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2018
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: it-pro
ms.openlocfilehash: 6b454ed7257e8d3f91e585cee2b559c54371fb15
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/05/2018
---
# <a name="configure-expiration-for-office-365-groups-preview"></a>Konfigurieren des Ablaufs für Office 365-Gruppen (Vorschauversion)

Der Lebenszyklus von Office 365-Gruppen kann jetzt durch Festlegen von Ablauffeatures verwaltet werden. Sie können einen Ablauf rein für Office 365-Gruppen in Azure Active Directory (Azure AD) festlegen. Das Festlegen eines Ablaufs für eine Gruppe bewirkt Folgendes:
-   Besitzer der Gruppe werden vor Erreichen des Ablaufs aufgefordert, die Gruppe zu erneuern.
-   Nicht erneuerte Gruppen werden gelöscht.
-   Gelöschte Office 365-Gruppen können innerhalb von 30 Tagen von den Gruppenbesitzern oder vom Administrator wiederhergestellt werden.

> [!NOTE]
> Zum Festlegen des Ablaufs für Office 365-Gruppen ist eine Azure Active Directory Premium-Lizenz oder eine für alle Mitglieder der Gruppen erforderlich, auf die Ablaufeinstellungen angewendet werden.

Informationen zum Herunterladen und Installieren der Azure AD-PowerShell-Cmdlets finden Sie unter [Azure Active Directory PowerShell for Graph - Public Preview Release 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137) (Azure Active Directory: PowerShell für Graph – öffentliche Vorschauversion 2.0.0.137).

## <a name="set-group-expiration"></a>Festlegen des Gruppenablaufs

1. Öffnen Sie das [Azure AD Admin Center](https://aad.portal.azure.com) mit dem Konto eines globalen Administrators in Ihrem Azure AD-Mandanten.

2. Öffnen Sie Azure AD, und wählen Sie **Benutzer und Gruppen**.

3. Wählen Sie **Gruppeneinstellungen**, und wählen Sie dann **Ablauf**, um die Ablaufeinstellungen zu öffnen.
  
  ![Blatt „Ablauf“](./media/active-directory-groups-lifecycle-azure-portal/expiration-settings.png)

4. Auf dem Blatt **Ablauf** können Sie:

  * Die Gruppenlebensdauer in Tagen festlegen. Sie könnten einen der vordefinierten Werte oder einen benutzerdefinierten Wert wählen (mindestens 31 Tage). 
  * Geben Sie eine E-Mail-Adresse an, an die die Verlängerungs- und Ablaufbenachrichtigungen gesendet werden sollten, wenn eine Gruppe keinen Besitzer hat. 
  * Wählen Sie, welche Office 365-Gruppen ablaufen. Sie können den Ablauf für **Alle** Office 365-Gruppen aktivieren, Sie können eine Auswahl aus den Office 365-Gruppen treffen, oder Sie wählen **Keine**, um den Ablauf für alle Gruppen zu deaktivieren.
  * Speichern Sie die Einstellungen durch Auswahl von **Speichern**.


E-Mail-Benachrichtigungen, wie diese werden an die Office 365-Gruppenbesitzer 30 Tage, 15 Tage und 1 Tag vor Ablauf der Gruppe gesendet.

![E-Mail-Benachrichtigung zum Ablauf](./media/active-directory-groups-lifecycle-azure-portal/expiration-notification.png)

Der Besitzer der Gruppe kann dann **Gruppe verlängern** auswählen, um die Office 365-Gruppe zu verlängern, oder **Zur Gruppe wechseln**, um die Mitglieder und andere Details der Gruppe anzuzeigen.

Wenn eine Gruppe abläuft, wird die Gruppe einen Tag nach dem Ablaufdatum gelöscht. Eine E-Mail-Benachrichtigung wie diese wird an die Office 365-Gruppenbesitzer gesendet, um sie über den Ablauf und die nachfolgende Löschung ihrer Office 365-Gruppe zu informieren.

![E-Mail-Benachrichtigung zur Gruppenlöschung](./media/active-directory-groups-lifecycle-azure-portal/deletion-notification.png)

Die Gruppe kann durch Auswahl von **Gruppe wiederherstellen** oder mithilfe von PowerShell-Cmdlets wiederhergestellt werden, wie beschrieben in [Wiederherstellen einer gelöschten Office 365-Gruppe in der Azure Active Directory-Vorschau] (https://docs.microsoft.com/azure/active-directory/active-directory-groups-restore-azure-portal).
    
Wenn die Gruppe, die Sie wiederherstellen, Dokumente, SharePoint-Websites oder andere beständige Objekte enthält, kann es bis zu 24 Stunden dauern, bis die Gruppe und deren Inhalte vollständig wiederhergestellt werden.

> [!NOTE]
> * Beim erstmaligen Einrichten des Ablaufs wird für alle Gruppen, deren Alter das Ablaufintervall übersteigt, ein Ablaufzeitraum von 30 Tagen festgelegt. Die erste Verlängerungsbenachrichtigung wird innerhalb eines Tages gesendet. 
>   Beispielsweise wurde Gruppe A vor 400 Tagen erstellt, und das Ablaufintervall ist auf 180 Tage festgelegt. Wenn Sie Ablaufeinstellungen anwenden, hat Gruppe A 30 Tage, bevor sie gelöscht wird, es sei denn, der Besitzer verlängert sie.
> * Wenn eine dynamische Gruppe gelöscht und wiederhergestellt wird, gilt sie als neue Gruppe und wird der Regel entsprechend erneut aufgefüllt. Dieser Vorgang kann bis zu 24 Stunden dauern.

## <a name="next-steps"></a>Nächste Schritte
Diese Artikel enthalten zusätzliche Informationen zu Azure AD-Gruppen.

* [Anzeigen vorhandener Gruppen](active-directory-groups-view-azure-portal.md)
* [Verwalten der Einstellungen einer Gruppe](active-directory-groups-settings-azure-portal.md)
* [Verwalten der Mitglieder einer Gruppe](active-directory-groups-members-azure-portal.md)
* [Verwalten der Mitgliedschaften einer Gruppe](active-directory-groups-membership-azure-portal.md)
* [Verwalten dynamischer Regeln für Benutzer in einer Gruppe](active-directory-groups-dynamic-membership-azure-portal.md)
