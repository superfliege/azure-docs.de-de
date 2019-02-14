---
title: Verlassen einer Organisation als Gastbenutzer – Azure Active Directory | Microsoft-Dokumentation
description: In diesem Artikel wird gezeigt, wie ein Azure AD B2B-Gastbenutzer eine Organisation über den Zugriffsbereich verlassen kann.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 05/11/2018
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: sasubram
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0d8183f7fa55a410b44d33295ad24f0f07adb145
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56185926"
---
# <a name="leave-an-organization-as-a-guest-user"></a>Verlassen einer Organisation als Gastbenutzer

Ein Azure Active Directory B2B-Gastbenutzer (Azure AD) kann eine Organisation jederzeit verlassen, wenn er die Apps dieser Organisation nicht mehr benötigt oder keine Verbindung mehr aufrechterhalten muss. Ein Benutzer kann eine Organisation selbst verlassen, ohne sich an einen Administrator wenden zu müssen.

## <a name="leave-an-organization"></a>Verlassen einer Organisation

Um eine Organisation als Benutzer zu verlassen, der am [Zugriffsbereich](https://myapps.microsoft.com) angemeldet ist, gehen Sie wie folgt vor:

1. Wenn Sie nicht bereits bei der Organisation angemeldet sind, die Sie verlassen möchten, wählen Sie Ihren Namen in der oberen rechten Ecke aus, und klicken Sie auf die Organisation, die Sie verlassen möchten.
2. Wählen Sie in der Ecke oben rechts Ihren Namen aus.
3. Wählen Sie neben **Organisationen** das Einstellungssymbol (Zahnrad) aus.
 
   ![Screenshot mit Benutzereinstellungen im Zugriffsbereich](media/leave-the-organization/UserSettings.png) 

3. Suchen Sie unter **Organisationen** nach der Organisation, die Sie verlassen möchten, und wählen Sie **Organisation verlassen** aus.

   ![Screenshot der Option „Organisation verlassen“ auf der Benutzeroberfläche](media/leave-the-organization/LeaveOrg.png)

4. Wenn Sie aufgefordert werden, Ihre Auswahl zu bestätigen, wählen Sie **Verlassen** aus. 

## <a name="account-removal"></a>Entfernen des Kontos

Wenn ein Benutzer eine Organisation verlässt, wird das Benutzerkonto im Verzeichnis vorläufig gelöscht. Das Benutzerobjekt wird in Azure AD standardmäßig in den Bereich **Gelöschte Benutzer** verschoben, aber erst nach 30 Tagen endgültig gelöscht. Bei einer solchen vorläufigen Löschung kann der Administrator das Benutzerkonto (einschließlich Gruppen und Berechtigungen) wiederherstellen, wenn der Benutzer innerhalb eines Zeitraums von 30 Tagen eine Anfrage zur Kontowiederherstellung sendet.

Falls gewünscht, kann ein Mandantenadministrator das Konto innerhalb des Zeitraums von 30 Tagen jederzeit endgültig löschen. Gehen Sie dazu folgendermaßen vor:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Option **Azure Active Directory** aus.
2. Wählen Sie unter **Verwalten** die Option **Benutzer** aus.
3. Wählen Sie **Gelöschte Benutzer** aus.
4. Aktivieren Sie das Kontrollkästchen neben einem gelöschten Benutzer, und wählen Sie dann **Endgültig löschen** aus.

Wenn Sie einen Benutzer endgültig löschen, ist diese Aktion unwiderruflich.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über Azure AD B2B finden Sie unter [Was ist die Azure AD B2B-Zusammenarbeit?](what-is-b2b.md)



