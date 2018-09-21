---
title: Hinzufügen der Datenschutzinformationen Ihrer Organisation in Azure AD | Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, wie Sie die Datenschutzinformationen Ihrer Organisation im Bereich „Eigenschaften“ von Azure Active Directory (Azure AD) hinzufügen.
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: lizross
ms.reviewer: bpham
ms.custom: it-pro
ms.openlocfilehash: a34fa2b8c2d966af108664c219a222fb9a5b7abc
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2018
ms.locfileid: "35769197"
---
# <a name="how-to-add-your-organizations-privacy-info-in-azure-active-directory"></a>Gewusst wie: Hinzufügen der Datenschutzinformationen Ihrer Organisation in Azure Active Directory
In diesem Artikel wird erläutert, wie ein Mandantenadministrator über das Azure-Portal Informationen in Bezug auf den Datenschutz zum Azure Active Directory-Mandanten (Azure AD) einer Organisation hinzufügen kann.

Es wird dringend empfohlen, sowohl Ihre globalen Kontaktdaten in Sachen Datenschutz und die Datenschutzerklärung Ihrer Organisation hinzufügen, damit Ihre internen Mitarbeiter und externen Gäste Ihre Richtlinien lesen können. Da Datenschutzerklärungen individuell erstellt werden und auf das jeweilige Unternehmen zugeschnitten sind, wird dringend empfohlen, zu diesem Sachverhalt einen Anwalt hinzuziehen.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="access-the-properties-area-to-add-your-privacy-info"></a>Zugreifen auf den Bereich „Eigenschaften“ zum Hinzufügen von Datenschutzinformationen

1.  Melden Sie sich als Mandantenadministrator beim Azure-Portal an.

2.  Wählen Sie auf der linken Navigationsleiste **Azure Active Directory** und dann **Eigenschaften** aus.

    Der Bereich **Eigenschaften** wird angezeigt.

    ![Bereich „Eigenschaften“ in Azure AD mit hervorgehobenem Bereich „Datenschutzinformationen“](./media/active-directory-properties-area/properties-area.png)

3.  Fügen Sie Ihre Datenschutzinformationen für Ihre Mitarbeiter hinzu:

    - **Technischer Kontakt**: Geben Sie die E-Mail-Adresse der Person ein, die bei Anfragen zum technischen Support innerhalb Ihrer Organisation zu kontaktieren ist.
    
    - **Globaler Datenschutzkontakt**: Geben Sie die E-Mail-Adresse der Person ein, die bei Anfragen zum Schutz persönlicher Daten zu kontaktieren ist. Diese Person wird von Microsoft auch kontaktiert, wenn ein Verstoß gegen die Datenschutzbestimmungen vorliegt. Wenn keine Person hier aufgeführt wird, kontaktiert Microsoft Ihre globalen Administratoren.

    - **URL zur Datenschutzerklärung**: Geben Sie den Link zum Dokument Ihrer Organisation an, in dem beschrieben wird, wie Ihre Organisation die Daten von internen sowie externen Gast schützt.

        >[!Important]
        >Wenn Sie Ihre Datenschutzerklärung oder Ihren Datenschutzkontakt nicht angeben, wird Ihren externen Gästen im Feld **Berechtigungen überprüfen** die Meldung angezeigt, dass **<_Name Ihrer Organisation_> keine Links zu deren Bedingungen zur Einsicht für den Gast angegeben hat**. Diese Meldung wird beispielsweise einem Gastbenutzer angezeigt, wenn er über die B2B-Zusammenarbeit eine Einladung für den Zugang zu einer Organisation erhält.

        ![Feld „Berechtigungen überprüfen“ in der B2B-Zusammenarbeit mit Meldung](./media/active-directory-properties-area/active-directory-no-privacy-statement-or-contact.png)

4.  Wählen Sie **Speichern**aus.

## <a name="next-steps"></a>Nächste Schritte
- [Azure Active Directory B2B-Zusammenarbeit: Einlösen von Einladungen](https://aka.ms/b2bredemption)
- [Hinzufügen oder Ändern von Profilinformationen für einen Benutzer in Azure Active Directory](fundamentals/active-directory-users-profile-azure-portal.md)