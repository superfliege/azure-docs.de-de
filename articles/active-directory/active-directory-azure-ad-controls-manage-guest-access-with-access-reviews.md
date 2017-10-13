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
ms.openlocfilehash: b2a76945482e0fe51dc7cc46740cbb8f3f2ff643
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="manage-guest-access-with-azure-ad-access-reviews"></a>Verwalten des Gastzugriffs mit Azure AD-Zugriffsüberprüfungen


Mit Azure Active Directory können Sie problemlos die Zusammenarbeit über Organisationsgrenzen hinweg ermöglichen, indem Sie das Feature [Azure AD B2B](active-directory-b2b-what-is-azure-ad-b2b.md) verwenden. Gastbenutzer von anderen Mandanten können von [Administratoren](active-directory-b2b-admin-add-users.md) oder von [Endbenutzern eingeladen](active-directory-b2b-how-it-works.md) werden.  Dies gilt auch für soziale Identitäten wie z. B. Microsoft-Konten.

Sie können auch auf einfache Weise sicherstellen, dass Gastbenutzer über entsprechenden Zugriff verfügen.  Hierzu können Sie die Gäste selbst oder einen Entscheidungsträger bitten, an einer Zugriffsüberprüfung teilzunehmen und den Zugriff des Gasts erneut zu zertifizieren (oder zu „attestieren“). Basierend auf Vorschlägen von Azure AD können die Prüfer die Notwendigkeit des weiteren Zugriffs der einzelnen Benutzer abwägen. Nach Abschluss einer Zugriffsüberprüfung können Sie dann Änderungen vornehmen und Zugriffsrechte für Gäste entfernen, die diese nicht mehr benötigen.

> [!NOTE]
> Dieses Dokument beschäftigt sich mit der Überprüfung des Zugriffs von Gastbenutzern. Wenn Sie den Zugriff aller Benutzer (nicht nur der Gäste) überprüfen möchten, lesen Sie stattdessen den Leitfaden zum [Verwalten des Benutzerzugriffs mit Zugriffsüberprüfungen](active-directory-azure-ad-controls-manage-user-access-with-access-reviews.md).  Wenn Sie die Benutzermitgliedschaft in Administratorrollen wie „Globaler Administrator“ überprüfen möchten, finden Sie entsprechende Informationen unter [Starten einer Zugriffsüberprüfung in Azure AD PIM](active-directory-privileged-identity-management-how-to-start-security-review.md). 
>
>

## <a name="prerequisites"></a>Voraussetzungen 

Zugriffsüberprüfungen sind mit der Premium P2-Edition von Azure Active Directory (in EMS E5 enthalten) verfügbar. Weitere Informationen finden Sie unter [Azure Active Directory-Editionen](active-directory-editions.md).  Jeder Benutzer, der dieses Feature zum Erstellen, Durchführen oder Anwenden einer Zugriffsüberprüfung verwendet, benötigt eine Lizenz.  

Wenn Sie Gastbenutzer bitten, ihren eigenen Zugriff zu überprüfen, erhalten Sie weitere Informationen zur Gastbenutzerlizenzierung unter [Azure AD B2B-Lizenzierung für die Zusammenarbeit](active-directory-b2b-licensing.md).

## <a name="creating-and-performing-an-access-review-for-guests"></a>Erstellen und Ausführen einer Zugriffsüberprüfung für Gäste

Aktivieren Sie zunächst Zugriffsüberprüfungen, damit diese in den Zugriffsbereichen der Prüfer angezeigt werden.  Navigieren Sie als globaler Administrator zur Seite [Zugriffsüberprüfungen](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/). 

Azure AD ermöglicht mehrere Szenarien für die Überprüfung von Gastbenutzern.  

Wählen Sie eines der folgenden Szenarien aus:
 - eine Gruppe in Azure Active Directory, die mindestens einen Gast als Mitglieder aufweist 
 - oder eine Anwendung, die mit Azure Active Directory verbunden ist, der mindestens ein Gastbenutzer zugewiesen ist. Entscheiden Sie dann, ob jeder Gast seinen eigenen Zugriff überprüft oder mindestens ein Benutzer den Zugriff aller Gäste überprüft.

 Jedes dieser Szenarien wird in einem der folgenden Abschnitte behandelt.

* [Gäste überprüfen ihre eigene Mitgliedschaft in einer Gruppe](#asking-guests-to-review-their-own-membership-in-a-group)
* [Sponsoren überprüfen die Mitgliedschaft von Gästen in einer Gruppe](#asking-sponsors-to-review-guests-membership-in-a-group)
* [Gäste überprüfen ihren eigenen Zugriff auf eine Anwendung](#asking-guests-to-review-their-own-access-to-an-application)
* [Sponsoren überprüfen den Zugriff von Gästen auf eine Anwendung](#asking-sponsors-to-review-guests-access-to-an-application) 
* [Gäste überprüfen ihren Zugriff allgemein](#asking-guests-to-review-their-need-for-access-in-general)


### <a name="asking-guests-to-review-their-own-membership-in-a-group"></a>Gäste überprüfen ihre eigene Mitgliedschaft in einer Gruppe

Zugriffsüberprüfungen können verwendet werden, um sicherzustellen, dass Benutzer, die eingeladen und einer Gruppe hinzugefügt wurden, weiterhin Zugriff benötigen.  Eine einfache Möglichkeit besteht darin, die Gäste zu bitten, ihre eigene Mitgliedschaft in der betreffenden Gruppe selbst zu überprüfen.

1. Starten Sie eine Zugriffsüberprüfung für die Gruppe, und geben Sie an, dass die Überprüfung nur Gastbenutzermitglieder umfasst und die Mitglieder sich selbst überprüfen. Weitere Informationen finden Sie unter [Erstellen einer Zugriffsüberprüfung](active-directory-azure-ad-controls-create-access-review.md).
2. Bitten Sie jeden Gast, seine eigene Mitgliedschaft zu überprüfen.  Standardmäßig erhält jeder Gast, der eine Einladung akzeptiert hat, eine E-Mail von Azure AD mit einem Link zu einer Zugriffsüberprüfung.  Azure AD stellt Anleitungen für Gäste bereit, [wie sie ihren Zugriff überprüfen können](active-directory-azure-ad-controls-perform-access-review.md).
3. Nachdem die Prüfer ihre Einschätzung abgegeben haben, können Sie die Zugriffsüberprüfung beenden und die Änderungen anwenden. Weitere Informationen finden Sie unter [Abschließen einer Zugriffsüberprüfung](active-directory-azure-ad-controls-complete-access-review.md). 
4. Zusätzlich zu den Benutzern, die angegeben haben, dass sie keinen weiteren Zugriff benötigen, möchten Sie möglicherweise auch Benutzer entfernen, die nicht reagiert haben.  Bei Benutzern, die nicht reagieren, besteht die Möglichkeit, dass sie keine E-Mails mehr erhalten.
5. Wenn die Gruppe nicht für Zugriffsverwaltung verwendet wird, möchten Sie ggf. auch Benutzer entfernen, die nicht ausgewählt wurden, an der Überprüfung teilzunehmen, weil sie ihre Einladung nicht akzeptiert haben.  Dies kann darauf hindeuten, dass die E-Mail-Adresse des eingeladenen Benutzers falsch war.  Wenn eine Gruppe jedoch als Verteilerliste verwendet wird, wurden möglicherweise einige Gastbenutzer nicht für die Teilnahme ausgewählt, weil sie Kontaktobjekte sind.

### <a name="asking-sponsors-to-review-guests-membership-in-a-group"></a>Sponsoren überprüfen die Mitgliedschaft von Gästen in einer Gruppe

Sie können einen Sponsor (z. B. den Besitzer einer Gruppe) bitten, die weitere Mitgliedschaft von Gästen in einer Gruppe zu überprüfen.

1. Starten Sie eine Zugriffsüberprüfung für die Gruppe, und geben Sie an, dass die Überprüfung nur Gastbenutzermitglieder umfasst. Geben Sie außerdem mindestens einen Prüfer an. Weitere Informationen finden Sie unter [Erstellen einer Zugriffsüberprüfung](active-directory-azure-ad-controls-create-access-review.md).
2. Bitten Sie die Prüfer, ihre Einschätzung abzugeben. Standardmäßig erhalten alle Prüfer eine E-Mail von Azure AD mit einem Link zu dem Zugriffsbereich, in dem sie [die Zugriffsüberprüfung durchführen](active-directory-azure-ad-controls-perform-access-review.md).
3. Nachdem die Prüfer ihre Einschätzung abgegeben haben, können Sie die Zugriffsüberprüfung beenden und die Änderungen anwenden. Weitere Informationen finden Sie unter [Abschließen einer Zugriffsüberprüfung](active-directory-azure-ad-controls-complete-access-review.md).

### <a name="asking-guests-to-review-their-own-access-to-an-application"></a>Gäste überprüfen ihren eigenen Zugriff auf eine Anwendung

Zugriffsüberprüfungen können verwendet werden, um sicherzustellen, dass Benutzer, die für eine bestimmte Gruppe eingeladen wurden, weiterhin Zugriff benötigen.  Eine einfache Möglichkeit besteht darin, die Gäste zu bitten, ihren eigenen Zugriffsbedarf selbst zu überprüfen.

1. Starten Sie eine Zugriffsüberprüfung für die Anwendung, und geben Sie an, dass die Überprüfung nur Gäste umfasst und die Benutzer ihren eigenen Zugriff überprüfen. Weitere Informationen finden Sie unter [Erstellen einer Zugriffsüberprüfung](active-directory-azure-ad-controls-create-access-review.md).
2. Bitten Sie Gäste, ihren eigenen Zugriff auf die Anwendung zu überprüfen.  Standardmäßig erhält jeder Gast, der eine Einladung akzeptiert hat, eine E-Mail von Azure AD mit einem Link zu der Zugriffsüberprüfung im Zugriffsbereich Ihrer Organisation.  Azure AD stellt Anleitungen für Gäste bereit, [wie sie ihren Zugriff überprüfen können](active-directory-azure-ad-controls-perform-access-review.md).
3. Nachdem die Prüfer ihre Einschätzung abgegeben haben, können Sie die Zugriffsüberprüfung beenden und die Änderungen anwenden. Weitere Informationen finden Sie unter [Abschließen einer Zugriffsüberprüfung](active-directory-azure-ad-controls-complete-access-review.md).
4. Zusätzlich zu den Benutzern, die angegeben haben, dass kein weiterer Zugriff erforderlich ist, möchten Sie möglicherweise auch Gastbenutzer entfernen, die nicht reagiert haben, weil diese Benutzer möglicherweise keine E-Mail mehr empfangen.  Möglicherweise möchten Sie auch Gastbenutzer entfernen, die nicht für die Teilnahme ausgewählt wurden, insbesondere dann, wenn der Gast in der letzten Zeit nicht eingeladen wurde, weil diese Benutzer ihre Einladung nicht akzeptiert haben und daher keinen Zugriff auf die Anwendung besitzen. 

### <a name="asking-sponsors-to-review-guests-access-to-an-application"></a>Sponsoren überprüfen den Zugriff von Gästen auf eine Anwendung


Sie können einen Sponsor (z. B. den Besitzer einer Anwendung) bitten, den weiteren Zugriffsbedarf von Gästen auf die Anwendung zu überprüfen.

1. Starten Sie eine Zugriffsüberprüfung für die Anwendung, und geben Sie an, dass die Überprüfung nur Gäste umfasst. Geben Sie außerdem mindestens einen Benutzer als Prüfer an. Weitere Informationen finden Sie unter [Erstellen einer Zugriffsüberprüfung](active-directory-azure-ad-controls-create-access-review.md).
2. Bitten Sie die Prüfer, ihre Einschätzung abzugeben. Standardmäßig erhalten alle Prüfer eine E-Mail von Azure AD mit einem Link zu dem Zugriffsbereich, in dem sie [die Zugriffsüberprüfung durchführen](active-directory-azure-ad-controls-perform-access-review.md).
3. Nachdem die Prüfer ihre Einschätzung abgegeben haben, können Sie die Zugriffsüberprüfung beenden und die Änderungen anwenden. Weitere Informationen finden Sie unter [Abschließen einer Zugriffsüberprüfung](active-directory-azure-ad-controls-complete-access-review.md).

### <a name="asking-guests-to-review-their-need-for-access-in-general"></a>Gäste überprüfen ihren Zugriff allgemein

In einigen Organisationen sind Gästen ihre Gruppenmitgliedschaften möglicherweise nicht bewusst.

> [!NOTE]
> Frühere Versionen des Azure-Portals erlaubten keinen administrativen Zugriff durch Benutzer mit dem Benutzertyp „Gast“, und in einigen Fällen hat ein Administrator in Ihrem Verzeichnis möglicherweise den UserType-Wert eines Gasts mithilfe von PowerShell in „Mitglied“ geändert.  Wenn dieses Szenario zuvor im Verzeichnis aufgetreten ist, umfasst die oben beschriebene Abfrage ggf. nicht alle Gastbenutzer, die in der Vergangenheit über administrative Zugriffsrechte verfügt haben. Daher müssen Sie den Benutzertyp dieser Gäste ändern oder sie manuell in die Gruppenmitgliedschaft einschließen.

1. Erstellen Sie eine Sicherheitsgruppe mit den Gästen als Mitgliedern in Azure AD, wenn noch keine geeignete Gruppe vorhanden ist.  Sie können z. B. eine Gruppe mit einer manuell verwalteten Mitgliedschaft von Gästen erstellen.  Oder Sie können eine dynamische Gruppe mit einem Namen wie „Gäste von Contoso“ für Benutzer im Contoso-Mandanten erstellen, deren UserType-Attributwert „Gast“ lautet.
2. Starten Sie eine Überprüfung für diese Gruppe, und geben Sie an, dass die Prüfer selbst Mitglieder sind. Weitere Informationen finden Sie unter [Erstellen einer Zugriffsüberprüfung](active-directory-azure-ad-controls-create-access-review.md).
3. Bitten Sie jeden Gast, seine eigene Mitgliedschaft zu überprüfen.  Standardmäßig erhält jeder Gast, der eine Einladung akzeptiert hat, eine E-Mail von Azure AD mit einem Link zu der Zugriffsüberprüfung im Zugriffsbereich Ihrer Organisation.  Azure AD stellt Anleitungen für Gäste bereit, [wie sie ihren Zugriff überprüfen können](active-directory-azure-ad-controls-perform-access-review.md).
4. Nachdem die Prüfer ihre Einschätzung abgegeben haben, können Sie die Zugriffsüberprüfung beenden. Weitere Informationen finden Sie unter [Abschließen einer Zugriffsüberprüfung](active-directory-azure-ad-controls-complete-access-review.md).
5. Entfernen Sie den Gastzugriff für Gäste, die abgelehnt wurden, die Überprüfung nicht abgeschlossen haben oder ihre Einladung zuvor nicht akzeptiert haben.   Wenn einige der Gäste Kontakte sind, die für die Teilnahme an der Überprüfung ausgewählt wurden, weil sie eine Einladung zuvor nicht akzeptiert haben, können Sie die Anmeldung für deren Konten ggf. über das Azure-Portal oder mit PowerShell deaktivieren.  Wenn der Gast keinen Zugriff mehr benötigt und kein Kontakt ist, kann sein Benutzerobjekt aus dem Verzeichnis über das Azure-Portal oder mit PowerShell entfernt werden.

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen einer Zugriffsüberprüfung für Mitglieder einer Gruppe oder den Zugriff auf eine Anwendung](active-directory-azure-ad-controls-create-access-review.md)







