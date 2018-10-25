---
title: Hinzufügen der Datenschutzinformationen Ihrer Organisation in Azure Active Directory | Microsoft-Dokumentation
description: Hier wird erläutert, wie Sie die Datenschutzinformationen Ihrer Organisation im Bereich „Eigenschaften“ von Azure Active Directory (Azure AD) hinzufügen.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: lizross
ms.reviewer: bpham
ms.custom: it-pro
ms.openlocfilehash: f32eeee8f02cee32eb05e14aa98387a20cb2e626
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "48903825"
---
# <a name="how-to-add-your-privacy-info-using-azure-active-directory"></a>Vorgehensweise: Hinzufügen der Datenschutzinformationen mit Azure Active Directory
In diesem Artikel wird erläutert, wie ein Mandantenadministrator über das Azure-Portal Informationen in Bezug auf den Datenschutz zum Azure Active Directory-Mandanten (Azure AD) einer Organisation hinzufügen kann.

Es wird dringend empfohlen, sowohl Ihre globalen Kontaktdaten in Sachen Datenschutz und die Datenschutzerklärung Ihrer Organisation hinzufügen, damit Ihre internen Mitarbeiter und externen Gäste Ihre Richtlinien lesen können. Da Datenschutzerklärungen individuell erstellt werden und auf das jeweilige Unternehmen zugeschnitten sind, wird dringend empfohlen, zu diesem Sachverhalt einen Anwalt hinzuziehen.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="add-your-privacy-info-on-azure-ad"></a>Hinzufügen Ihrer Datenschutzinformationen in Azure AD
Sie fügen die Datenschutzinformationen Ihrer Organisation im Bereich **Eigenschaften** von Azure AD hinzu.

### <a name="to-access-the-properties-area-and-add-your-privacy-information"></a>So greifen Sie auf den Bereich „Eigenschaften“ zu und fügen die Datenschutzinformationen hinzu

1.  Melden Sie sich als Mandantenadministrator beim Azure-Portal an.

2.  Wählen Sie auf der linken Navigationsleiste **Azure Active Directory** und dann **Eigenschaften** aus.

    Der Bereich **Eigenschaften** wird angezeigt.

    ![Bereich „Eigenschaften“ in Azure AD mit hervorgehobenem Bereich „Datenschutzinformationen“](media/active-directory-properties-area/properties-area.png)

3.  Fügen Sie Ihre Datenschutzinformationen für Ihre Mitarbeiter hinzu:

    - **Technischer Kontakt**: Geben Sie die E-Mail-Adresse der Person ein, die bei Anfragen zum technischen Support innerhalb Ihrer Organisation zu kontaktieren ist.
    
    - **Globaler Datenschutzkontakt**: Geben Sie die E-Mail-Adresse der Person ein, die bei Anfragen zum Schutz persönlicher Daten zu kontaktieren ist. Diese Person wird von Microsoft auch kontaktiert, wenn ein Verstoß gegen die Datenschutzbestimmungen vorliegt. Wenn keine Person hier aufgeführt wird, kontaktiert Microsoft Ihre globalen Administratoren.

    - **URL zur Datenschutzerklärung**: Geben Sie den Link zum Dokument Ihrer Organisation an, in dem beschrieben wird, wie Ihre Organisation die Daten von internen sowie externen Gast schützt.

        >[!Important]
        >Wenn Sie Ihre Datenschutzerklärung oder Ihren Datenschutzkontakt nicht angeben, wird Ihren externen Gästen im Feld **Berechtigungen überprüfen** die Meldung angezeigt, dass **<_Name Ihrer Organisation_> keine Links zu deren Bedingungen zur Einsicht für den Gast angegeben hat**. Diese Meldung wird beispielsweise einem Gastbenutzer angezeigt, wenn er über die B2B-Zusammenarbeit eine Einladung für den Zugang zu einer Organisation erhält.

        ![Feld „Berechtigungen überprüfen“ in der B2B-Zusammenarbeit mit Meldung](media/active-directory-properties-area/active-directory-no-privacy-statement-or-contact.png)

4.  Wählen Sie **Speichern**aus.

## <a name="next-steps"></a>Nächste Schritte
- [Azure Active Directory B2B-Zusammenarbeit: Einlösen von Einladungen](https://aka.ms/b2bredemption)
- [Hinzufügen oder Ändern von Profilinformationen für einen Benutzer in Azure Active Directory](active-directory-users-profile-azure-portal.md)