---
title: "Verwalten des Gastzugriffs mit Azure AD-Zugriffsüberprüfungen | Microsoft Docs"
description: "Verwalten von Gastbenutzern als Mitglieder einer Gruppe oder einer Anwendung zugewiesene Benutzer mit Azure Active Directory-Zugriffsüberprüfungen"
services: active-directory
documentationcenter: 
author: markwahl-msft
manager: femila
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: billmath
ms.openlocfilehash: d67b12eaac17e278724ddf8670e65afba235f099
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2017
---
# <a name="manage-guest-access-with-azure-ad-access-reviews"></a>Verwalten des Gastzugriffs mit Azure AD-Zugriffsüberprüfungen


Mit Azure Active Directory (Azure AD) können Sie problemlos die Zusammenarbeit über Organisationsgrenzen hinweg ermöglichen, indem Sie das Feature [Azure AD B2B](active-directory-b2b-what-is-azure-ad-b2b.md) verwenden. Gastbenutzer von anderen Mandanten können von [Administratoren](active-directory-b2b-admin-add-users.md) oder [anderen Benutzern eingeladen](active-directory-b2b-how-it-works.md) werden. Dies gilt auch für soziale Identitäten wie z.B. Microsoft-Konten.

Sie können auch auf einfache Weise sicherstellen, dass Gastbenutzer über entsprechenden Zugriff verfügen. Hierzu können Sie die Gäste selbst oder einen Entscheidungsträger bitten, an einer Zugriffsüberprüfung teilzunehmen und den Zugriff des Gasts erneut zu zertifizieren (oder zu „bescheinigen“). Basierend auf Vorschlägen von Azure AD können die Prüfer die Notwendigkeit des weiteren Zugriffs der einzelnen Benutzer abwägen. Nach Abschluss einer Zugriffsüberprüfung können Sie dann Änderungen vornehmen und Zugriffsrechte für Gäste entfernen, die diese nicht mehr benötigen.

> [!NOTE]
> Dieses Dokument beschäftigt sich mit der Überprüfung des Zugriffs von Gastbenutzern. Wenn Sie den Zugriff aller Benutzer überprüfen möchten (und nicht nur den von Gästen), lesen Sie [Verwalten des Benutzerzugriffs mit Azure AD-Zugriffsüberprüfungen](active-directory-azure-ad-controls-manage-user-access-with-access-reviews.md). Wenn Sie die Benutzermitgliedschaft in Administratorrollen wie „Globaler Administrator“ überprüfen möchten, finden Sie entsprechende Informationen unter [Starten einer Zugriffsüberprüfung in Azure AD Privileged Identity Management](active-directory-privileged-identity-management-how-to-start-security-review.md). 
>
>

## <a name="prerequisites"></a>Voraussetzungen 

Zugriffsüberprüfungen sind mit der Premium P2-Edition von Azure AD (in Microsoft Enterprise Mobility + Security E5 enthalten) verfügbar. Weitere Informationen finden Sie unter [Azure Active Directory-Editionen](active-directory-editions.md). Jeder Benutzer, der dieses Feature zum Erstellen oder Anwenden einer Zugriffsüberprüfung bzw. Zugreifen darauf verwendet, benötigt eine Lizenz.

Wenn Sie Gastbenutzer bitten möchten, ihren eigenen Zugriff zu überprüfen, lesen Sie die Informationen zur Lizenzierung von Gastbenutzern. Weitere Informationen finden Sie unter [Lizenzierung für die Azure AD B2B-Zusammenarbeit](active-directory-b2b-licensing.md).

## <a name="create-and-perform-an-access-review-for-guests"></a>Erstellen und Durchführen einer Zugriffsüberprüfung für Gäste

Aktivieren Sie zunächst Zugriffsüberprüfungen, damit diese in den Zugriffsbereichen eines Prüfers angezeigt werden. Navigieren Sie als globaler Administrator zur Seite [Zugriffsüberprüfungen](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/). 

Azure AD ermöglicht mehrere Szenarien für die Überprüfung von Gastbenutzern.

Wählen Sie eines der folgenden Szenarien aus:

 - Eine Gruppe in Azure AD mit mindestens einem Gast als Mitglied
 - Eine mit Azure AD verbundene Anwendung, der mindestens ein Gastbenutzer zugewiesen ist 

Sie können dann entscheiden, ob Sie jeden Gast bitten, seinen eigenen Zugriff zu überprüfen, oder ob Sie einen oder mehrere Benutzer bitten, den Zugriff von jedem Gast zu überprüfen.

 Diese Szenarien werden in den folgenden Abschnitten behandelt.

### <a name="ask-guests-to-review-their-own-membership-in-a-group"></a>Auffordern von Gästen zur Überprüfung ihrer eigenen Mitgliedschaft in einer Gruppe

Mithilfe von Zugriffsüberprüfungen können Sie sicherstellen, dass Benutzer, die eingeladen und einer Gruppe hinzugefügt wurden, weiterhin Zugriff benötigen. Sie können Gäste ganz einfach darum bitten, ihre eigene Mitgliedschaft in dieser Gruppe zu überprüfen.

1. Um eine Zugriffsüberprüfung für die Gruppe zu starten, geben Sie an, dass die Überprüfung nur Gastbenutzermitglieder umfasst und die Mitglieder sich selbst überprüfen. Weitere Informationen finden Sie unter [Erstellen einer Zugriffsüberprüfung von Gruppenmitgliedern oder dem Anwendungszugriff mit Azure AD](active-directory-azure-ad-controls-create-access-review.md).

2. Bitten Sie jeden Gast, seine eigene Mitgliedschaft zu überprüfen. Standardmäßig erhält jeder Gast, der eine Einladung angenommen hat, eine E-Mail von Azure AD mit einem Link zu einer Zugriffsüberprüfung. Azure AD stellt Anweisungen für Gäste bereit, [wie sie ihren Zugriff überprüfen können](active-directory-azure-ad-controls-perform-access-review.md).

3. Nachdem die Prüfer ihre Einschätzung abgegeben haben, können Sie die Zugriffsüberprüfung beenden und die Änderungen anwenden. Weitere Informationen finden Sie unter [Durchführen einer Zugriffsüberprüfung von Mitgliedern einer Gruppe oder des Benutzerzugriffs auf eine Anwendung mit Azure AD](active-directory-azure-ad-controls-complete-access-review.md).

4. Zusätzlich zu den Benutzern, die angegeben haben, dass sie keinen weiteren Zugriff benötigen, können Sie auch Benutzer entfernen, die nicht reagiert haben. Benutzer, die nicht reagieren, erhalten möglicherweise keine E-Mails mehr.

5. Wenn die Gruppe nicht für die Zugriffsverwaltung verwendet wird, können Sie auch Benutzer entfernen, die nicht ausgewählt wurden, an der Überprüfung teilzunehmen, weil sie ihre Einladung nicht angenommen haben. Ein solches Versäumnis kann darauf hindeuten, dass die E-Mail-Adresse des eingeladenen Benutzers einen Tippfehler enthielt. Wenn eine Gruppe als Verteilerliste verwendet wird, wurden einige Gastbenutzer vermutlich nicht für eine Teilnahme ausgewählt, weil es sich bei ihnen um Kontaktobjekte handelt.

### <a name="ask-a-sponsor-to-review-a-guests-membership-in-a-group"></a>Auffordern eines Sponsors zum Überprüfen der Mitgliedschaft eines Gasts in einer Gruppe

Sie können einen Sponsor (z.B. den Besitzer einer Gruppe) bitten, die Notwendigkeit einer weitergehenden Mitgliedschaft eines Gasts in einer Gruppe zu überprüfen.

1. Um eine Zugriffsüberprüfung für die Gruppe zu starten, geben Sie an, dass die Überprüfung nur Gastbenutzermitglieder umfasst. Geben Sie dann mindestens einen Prüfer an. Weitere Informationen finden Sie unter [Erstellen einer Zugriffsüberprüfung von Gruppenmitgliedern oder dem Anwendungszugriff mit Azure AD](active-directory-azure-ad-controls-create-access-review.md).

2. Bitten Sie die Prüfer, ihre Einschätzung abzugeben. Standardmäßig erhalten alle Prüfer eine E-Mail von Azure AD mit einem Link zu dem Zugriffsbereich, in dem sie [die Zugriffsüberprüfung durchführen](active-directory-azure-ad-controls-perform-access-review.md).

3. Nachdem die Prüfer ihre Einschätzung abgegeben haben, können Sie die Zugriffsüberprüfung beenden und die Änderungen anwenden. Weitere Informationen finden Sie unter [Durchführen einer Zugriffsüberprüfung von Mitgliedern einer Gruppe oder des Benutzerzugriffs auf eine Anwendung mit Azure AD](active-directory-azure-ad-controls-complete-access-review.md).

### <a name="ask-guests-to-review-their-own-access-to-an-application"></a>Auffordern von Gästen zum Überprüfen ihres eigenen Zugriffs auf eine Anwendung

Mithilfe von Zugriffsüberprüfungen können Sie sicherstellen, dass Benutzer, die für eine bestimmte Anwendung eingeladen wurden, weiterhin Zugriff benötigen. Sie können die Gäste ganz einfach bitten, ihren eigenen Zugriffsbedarf selbst zu überprüfen.

1. Um eine Zugriffsüberprüfung für die Anwendung zu starten, geben Sie an, dass die Überprüfung nur Gäste umfasst und die Benutzer ihren eigenen Zugriff überprüfen. Weitere Informationen finden Sie unter [Erstellen einer Zugriffsüberprüfung von Gruppenmitgliedern oder dem Anwendungszugriff mit Azure AD](active-directory-azure-ad-controls-create-access-review.md).

2. Bitten Sie Gäste, ihren eigenen Zugriff auf die Anwendung zu überprüfen. Standardmäßig erhält jeder Gast, der eine Einladung angenommen hat, eine E-Mail von Azure AD mit einem Link zu der Zugriffsüberprüfung im Zugriffsbereich Ihrer Organisation. Azure AD stellt Anweisungen für Gäste bereit, [wie sie ihren Zugriff überprüfen können](active-directory-azure-ad-controls-perform-access-review.md).

3. Nachdem die Prüfer ihre Einschätzung abgegeben haben, können Sie die Zugriffsüberprüfung beenden und die Änderungen anwenden. Weitere Informationen finden Sie unter [Durchführen einer Zugriffsüberprüfung von Mitgliedern einer Gruppe oder des Benutzerzugriffs auf eine Anwendung mit Azure AD](active-directory-azure-ad-controls-complete-access-review.md).

4. Zusätzlich zu den Benutzern, die angegeben haben, dass sie keinen weiteren Zugriff benötigen, können Sie auch Gastbenutzer entfernen, die nicht reagiert haben. Benutzer, die nicht reagieren, erhalten möglicherweise keine E-Mails mehr. Sie können auch Gastbenutzer entfernen, die nicht für eine Teilnahme ausgewählt wurden, insbesondere, wenn diese in letzter Zeit nicht eingeladen wurden. Diese Benutzer haben ihre Einladung nicht angenommen und hatten daher keinen Zugriff auf die Anwendung. 

### <a name="ask-a-sponsor-to-review-a-guests-access-to-an-application"></a>Auffordern eines Sponsors zum Überprüfen des Zugriffs eines Gasts auf eine Anwendung

Sie können einen Sponsor (z.B. den Besitzer einer Anwendung) bitten, den weiteren Zugriffsbedarf eines Gasts auf die Anwendung zu überprüfen.

1. Um eine Zugriffsüberprüfung für die Anwendung zu starten, geben Sie an, dass die Überprüfung nur Gäste umfasst. Geben Sie dann mindestens einen Benutzer als Prüfer an. Weitere Informationen finden Sie unter [Erstellen einer Zugriffsüberprüfung von Gruppenmitgliedern oder dem Anwendungszugriff mit Azure AD](active-directory-azure-ad-controls-create-access-review.md).

2. Bitten Sie die Prüfer, ihre Einschätzung abzugeben. Standardmäßig erhalten alle Prüfer eine E-Mail von Azure AD mit einem Link zu dem Zugriffsbereich, in dem sie [die Zugriffsüberprüfung durchführen](active-directory-azure-ad-controls-perform-access-review.md).

3. Nachdem die Prüfer ihre Einschätzung abgegeben haben, können Sie die Zugriffsüberprüfung beenden und die Änderungen anwenden. Weitere Informationen finden Sie unter [Durchführen einer Zugriffsüberprüfung von Mitgliedern einer Gruppe oder des Benutzerzugriffs auf eine Anwendung mit Azure AD](active-directory-azure-ad-controls-complete-access-review.md).

### <a name="ask-guests-to-review-their-need-for-access-in-general"></a>Auffordern von Gästen zur Überprüfung ihres allgemeinen Zugriffsbedarfs

In einigen Organisationen sind Gästen ihre Gruppenmitgliedschaften möglicherweise nicht bekannt.

> [!NOTE]
> In früheren Versionen des Azure-Portals war ein Administratorzugriff durch Benutzer mit dem UserType-Wert „Guest“ nicht zulässig. In einigen Fällen hat ein Administrator in Ihrem Verzeichnis möglicherweise den UserType-Wert eines Gasts mithilfe von PowerShell in „Member“ geändert. Wenn diese Änderung zuvor in Ihrem Verzeichnis vorgenommen wurde, enthält die vorherige Abfrage möglicherweise nicht alle Gastbenutzer, die in der Vergangenheit über Administratorrechte verfügten. In diesem Fall müssen Sie entweder den UserType-Wert des Gasts ändern oder den Gast manuell in die Gruppenmitgliedschaft aufnehmen.

1. Erstellen Sie eine Sicherheitsgruppe mit den Gästen als Mitgliedern in Azure AD, wenn noch keine geeignete Gruppe vorhanden ist. Sie können z.B. eine Gruppe mit einer manuell verwalteten Mitgliedschaft von Gästen erstellen. Oder Sie können eine dynamische Gruppe mit einem Namen wie „Gäste von Contoso“ für Benutzer im Contoso-Mandanten erstellen, deren UserType-Attributwert „Guest“ lautet.

2. Um eine Zugriffsüberprüfung für diese Gruppe zu starten, geben Sie an, dass die Prüfer selbst Mitglieder sind. Weitere Informationen finden Sie unter [Erstellen einer Zugriffsüberprüfung von Gruppenmitgliedern oder dem Anwendungszugriff mit Azure AD](active-directory-azure-ad-controls-create-access-review.md).

3. Bitten Sie jeden Gast, seine eigene Mitgliedschaft zu überprüfen. Standardmäßig erhält jeder Gast, der eine Einladung angenommen hat, eine E-Mail von Azure AD mit einem Link zu der Zugriffsüberprüfung im Zugriffsbereich Ihrer Organisation. Azure AD stellt Anweisungen für Gäste bereit, [wie sie ihren Zugriff überprüfen können](active-directory-azure-ad-controls-perform-access-review.md).

4. Nachdem die Prüfer ihre Einschätzung abgegeben haben, können Sie die Zugriffsüberprüfung beenden. Weitere Informationen finden Sie unter [Durchführen einer Zugriffsüberprüfung von Mitgliedern einer Gruppe oder des Benutzerzugriffs auf eine Anwendung mit Azure AD](active-directory-azure-ad-controls-complete-access-review.md).

5. Entfernen Sie den Gastzugriff für Gäste, die abgelehnt wurden, die Überprüfung nicht abgeschlossen haben oder ihre Einladung zuvor nicht angenommen haben. Wenn einige der Gäste Kontakte sind, die für die Teilnahme an der Überprüfung ausgewählt wurden, weil sie eine Einladung zuvor nicht angenommen haben, können Sie deren Konten über das Azure-Portal oder mit PowerShell deaktivieren. Wenn der Gast keinen Zugriff mehr benötigt und kein Kontakt ist, können Sie sein Benutzerobjekt über das Azure-Portal oder mit PowerShell aus dem Verzeichnis entfernen.

## <a name="next-steps"></a>Nächste Schritte

[Erstellen einer Zugriffsüberprüfung für Mitglieder einer Gruppe oder den Zugriff auf eine Anwendung](active-directory-azure-ad-controls-create-access-review.md)







