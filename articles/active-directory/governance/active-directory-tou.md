---
title: Azure Active Directory-Nutzungsbedingungen | Microsoft-Dokumentation
description: Mit den Azure AD-Nutzungsbedingungen können Sie und Ihr Unternehmen Nutzungsbedingungen für Benutzer von Azure AD-Diensten angeben.
services: active-directory
author: rolyon
manager: mtillman
editor: ''
ms.assetid: d55872ef-7e45-4de5-a9a0-3298e3de3565
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: compliance
ms.date: 09/04/2018
ms.author: rolyon
ms.openlocfilehash: 9fa966999e220ea4357d5b5c37f0038c75fe2339
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/14/2018
ms.locfileid: "45607110"
---
# <a name="azure-active-directory-terms-of-use-feature"></a>Nutzungsbedingungsfeature für Azure Active Directory
Mit den Azure AD-Nutzungsbedingungen können Organisationen komfortabel Informationen für Endbenutzer anzeigen. Dadurch wird sichergestellt, dass Benutzern relevante Haftungsausschlüsse angezeigt werden, um rechtliche oder compliancebezogene Anforderungen zu erfüllen. Dieser Artikel enthält eine Einführung in die Verwendung der Azure AD-Nutzungsbedingungen.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="what-can-i-do-with-terms-of-use"></a>Welche Möglichkeiten bieten die Nutzungsbedingungen?
Die Azure AD-Nutzungsbedingungen ermöglichen Folgendes:
- Voraussetzen, dass Mitarbeiter oder Gäste Ihren Nutzungsbedingungen zustimmen, bevor ihnen Zugriff gewährt wird
- Anzeigen allgemeiner Nutzungsbedingungen für alle Benutzer in Ihrer Organisation
- Anzeigen spezifischer Nutzungsbedingungen auf der Grundlage von Benutzerattributen (beispielsweise Ärzte/Pflegepersonal oder Mitarbeiter im Inland/Ausland unter Verwendung [dynamischer Gruppen](../users-groups-roles/groups-dynamic-membership.md))
- Anzeigen spezifischer Nutzungsbedingungen beim Zugriff auf besonders geschäftsrelevante Anwendungen (beispielsweise Salesforce)
- Anzeigen von Nutzungsbedingungen in verschiedenen Sprachen
- Auflisten, wer den Nutzungsbedingungen bereits (oder noch nicht) zugestimmt hat
- Anzeigen eines Überwachungsprotokolls mit Aktivitäten für die Nutzungsbedingungen

## <a name="prerequisites"></a>Voraussetzungen
Um die Azure AD-Nutzungsbedingungen verwenden und konfigurieren zu können, benötigen Sie Folgendes:

- Eines der folgenden Abonnements: Azure AD Premium P1, P2, EMS E3 oder EMS E5.
    - Sollten Sie über keines dieser Abonnements verfügen, können Sie [Azure AD Premium beziehen](../fundamentals/active-directory-get-started-premium.md) oder [die Azure AD Premium-Testversion aktivieren](https://azure.microsoft.com/trial/get-started-active-directory/).
- Eines der folgenden Administratorkonten für das Verzeichnis, das Sie konfigurieren möchten:
    - Globaler Administrator
    - Sicherheitsadministrator
    - Administrator für bedingten Zugriff

## <a name="terms-of-use-document"></a>Dokument mit Nutzungsbedingungen

Der Inhalt von Azure AD Nutzungsbedingungen wird im PDF-Format bereitgestellt. Die PDF-Datei kann für beliebige Inhalte verwendet werden (etwa für vorhandene Vertragsdokumente) und ermöglicht es, bei der Benutzeranmeldung die Zustimmung von Endbenutzern einzuholen. Für PDF-Inhalte wird der Schriftgrad 24 empfohlen.

## <a name="add-terms-of-use"></a>Hinzufügen von Nutzungsbedingungen
Gehen Sie nach Fertigstellung Ihres Dokuments mit Nutzungsbedingungen wie folgt vor, um sie hinzuzufügen.

1. Melden Sie sich bei Azure als globaler Administrator, Sicherheitsadministrator oder Administrator für bedingten Zugriff an.

1. Navigieren Sie unter [https://aka.ms/catou](https://aka.ms/catou) zu **Nutzungsbedingungen**.

    ![Blatt für Nutzungsbedingungen](./media/active-directory-tou/tou-blade.png)

1. Klicken Sie auf **Neue Bedingungen**.

    ![Hinzufügen von Nutzungsbedingungen](./media/active-directory-tou/new-tou.png)

1. Geben Sie unter **Name** einen Namen für die Nutzungsbedingungen ein.

2. Geben Sie unter **Anzeigename** einen Anzeigenamen ein.  Hierbei handelt es sich um die Überschrift, die Benutzern bei der Anmeldung angezeigt wird.

3. **Navigieren** Sie zu Ihrer vorbereiteten PDF-Datei mit den Nutzungsbedingungen, und wählen Sie sie aus.

4. **Wählen** Sie eine Sprache für die Nutzungsbedingungen.  Die Sprachoption ermöglicht das Hochladen mehrerer Nutzungsbedingungen in verschiedenen Sprachen.  Die Version der Nutzungsbedingungen, die einem Endbenutzer angezeigt wird, basiert auf den Browsereinstellungen.

5. Aktivieren oder deaktivieren Sie die Option **Benutzer müssen die Nutzungsbedingungen erweitern**.  Wenn diese Einstellung aktiviert ist, müssen Endbenutzer die Nutzungsbedingungen anzeigen, bevor sie sie akzeptieren.

6. Unter **Bedingter Zugriff** können Sie die hochgeladenen Nutzungsbedingungen durch Auswahl einer Vorlage in der Dropdownliste oder mithilfe einer benutzerdefinierten Richtlinie für den bedingten Zugriff **erzwingen**.  Benutzerdefinierte Richtlinien für den bedingten Zugriff ermöglichen präzise Nutzungsbedingungen bis auf die Ebene einer spezifischen Cloudanwendung oder Benutzergruppe.  Weitere Informationen finden Sie unter [Best Practices für den bedingten Zugriff in Azure Active Directory](../../cognitive-services/qnamaker/concepts/best-practices.md).

    >[!IMPORTANT]
    >Die Richtliniensteuerungen für den bedingten Zugriff (einschließlich Nutzungsbedingungen) unterstützen die Erzwingung für Dienstkonten nicht.  Es wird empfohlen, alle Dienstkonten von der Richtlinie für den bedingten Zugriff auszuschließen.

7. Klicken Sie auf **Create**.

8. Nach dem Auswählen einer benutzerdefinierten Vorlage für bedingten Zugriff wird ein neuer Bildschirm angezeigt, auf dem Sie die Richtlinie für den bedingten Zugriff anpassen können.

    Ihre neuen Nutzungsbedingungen werden angezeigt.

    ![Hinzufügen von Nutzungsbedingungen](./media/active-directory-tou/create-tou.png)

## <a name="view-report-of-who-has-accepted-and-declined"></a>Anzeigen des Berichts über abgelehnte und akzeptierte Nutzungsbedingungen
Auf dem Blatt für die Nutzungsbedingungen wird angezeigt, wie viele Benutzer die Bedingungen akzeptiert bzw. abgelehnt haben. Diese Werte sowie die Angabe, wer die Bedingungen akzeptiert/abgelehnt hat, werden für die Lebensdauer der Nutzungsbedingungen gespeichert.

1. Melden Sie sich unter [https://aka.ms/catou](https://aka.ms/catou) bei Azure an, und navigieren Sie zu **Nutzungsbedingungen**.

    ![Überwachungsereignis](./media/active-directory-tou/view-tou.png)

1. Klicken Sie auf die Zahl unter **Akzeptiert** oder **Abgelehnt**, um den aktuellen Status für Benutzer anzuzeigen.

    ![Überwachungsereignis](./media/active-directory-tou/accepted-tou.png)

## <a name="view-azure-ad-audit-logs"></a>Anzeigen der Azure AD-Überwachungsprotokolle
Für den Fall, dass Sie zusätzliche Aktivitäten anzeigen möchten, enthalten die Azure AD-Nutzungsbedingungen auch Überwachungsprotokolle. Jede Benutzerzustimmung löst ein Ereignis in den Überwachungsprotokollen aus, die 30 Tage lang gespeichert werden. Diese Protokolle können im Portal angezeigt oder als CSV-Datei heruntergeladen werden.

Gehen Sie wie folgt vor, um mit der Verwendung von Azure AD-Überwachungsprotokollen zu beginnen:

1. Melden Sie sich unter [https://aka.ms/catou](https://aka.ms/catou) bei Azure an, und navigieren Sie zu **Nutzungsbedingungen**.

1. Klicken Sie auf **Überwachungsprotokolle anzeigen**.

    ![Überwachungsereignis](./media/active-directory-tou/audit-tou.png)

1. Die Informationen auf dem Bildschirm mit den Azure AD-Überwachungsprotokollen können mithilfe der bereitgestellten Dropdownlisten nach bestimmten Überwachungsprotokollinformationen gefiltert werden.

    ![Überwachungsereignis](./media/active-directory-tou/audit-logs-tou.png)

1. Sie können auch auf **Herunterladen** klicken, um die Informationen zur in einer CSV-Datei herunterzuladen und lokal zu verwenden.

## <a name="what-terms-of-use-looks-like-for-users"></a>Darstellung der Nutzungsbedingung für Benutzer
Wenn Nutzungsbedingungen erstellt wurden und erzwungen werden, wird den betroffenen Benutzern bei der Anmeldung folgender Bildschirm angezeigt.

![Überwachungsereignis](./media/active-directory-tou/user-tou.png)

Der folgende Bildschirm veranschaulicht die Darstellung der Nutzungsbedingungen auf mobilen Geräten:

![Überwachungsereignis](./media/active-directory-tou/mobile-tou.png)

Benutzer müssen die Nutzungsbedingungen nur einmal akzeptieren, und diese Bedingungen werden ihnen bei nachfolgenden Anmeldungen nicht erneut angezeigt.

### <a name="how-users-can-review-their-terms-of-use"></a>Möglichkeiten zum Lesen der Nutzungsbedingungen für die Benutzer
Benutzer können die Nutzungsbedingungen, die sie akzeptiert haben, wie folgt lesen und anzeigen:

1. Melden Sie sich bei [https://myapps.microsoft.com](https://myapps.microsoft.com) an.

1. Klicken Sie in der rechten oberen Ecke auf Ihren Namen, und wählen Sie in der Dropdownliste die Option **Profil** aus.

    ![Profil](./media/active-directory-tou/tou14.png)

1. Klicken Sie in Ihrem Profil auf **Nutzungsbedingungen lesen**.

    ![Überwachungsereignis](./media/active-directory-tou/tou13a.png)

1. Hier können Sie sich die akzeptierten Nutzungsbedingungen ansehen. 

## <a name="edit-terms-of-use-details"></a>Bearbeiten der Details der Nutzungsbedingungen
Sie können einige Details der Nutzungsbedingungen bearbeiten, aber kein vorhandenes Dokument ändern. Im Folgenden wird das Verfahren zum Bearbeiten von Details beschrieben.

1. Melden Sie sich unter [https://aka.ms/catou](https://aka.ms/catou) bei Azure an, und navigieren Sie zu **Nutzungsbedingungen**.

1. Wählen Sie die Nutzungsbedingungen aus, die Sie bearbeiten möchten.

1. Klicken Sie auf **Bedingungen bearbeiten**.

1. Ändern Sie im Bereich „Bedingungen bearbeiten“ den Namen und Anzeigenamen, oder fordern Sie von Benutzern, Werte zu erweitern.

    ![Hinzufügen von Nutzungsbedingungen](./media/active-directory-tou/edit-tou.png)

1. Klicken Sie zum Speichern der Änderungen auf **Speichern**.

    Nachdem Sie die Änderungen gespeichert haben, müssen die Benutzer die neuen Bedingungen erneut akzeptieren.

## <a name="add-a-terms-of-use-language"></a>Hinzufügen einer Sprache der Nutzungsbedingungen
Im Folgenden wird das Verfahren zum Hinzufügen einer Sprache der Nutzungsbedingungen beschrieben.

1. Melden Sie sich unter [https://aka.ms/catou](https://aka.ms/catou) bei Azure an, und navigieren Sie zu **Nutzungsbedingungen**.

1. Wählen Sie die Nutzungsbedingungen aus, die Sie bearbeiten möchten.

1. Klicken Sie im Detailbereich auf die Registerkarte **Sprachen**.

    ![Hinzufügen von Nutzungsbedingungen](./media/active-directory-tou/languages-tou.png)

1. Klicken Sie auf **Sprache hinzufügen**.

1. Laden Sie in den Bereich „Sprache der Nutzungsbedingungen hinzufügen“ Ihre lokalisierte PDF-Datei hoch, und wählen Sie die Sprache aus.

    ![Hinzufügen von Nutzungsbedingungen](./media/active-directory-tou/language-add-tou.png)

1. Klicken Sie auf **Hinzufügen**, um die Sprache hinzuzufügen.

## <a name="delete-terms-of-use"></a>Löschen der Nutzungsbedingungen
Veraltete Nutzungsbedingungen können wie folgt gelöscht werden:

1. Melden Sie sich unter [https://aka.ms/catou](https://aka.ms/catou) bei Azure an, und navigieren Sie zu **Nutzungsbedingungen**.

1. Wählen Sie die Nutzungsbedingungen aus, die Sie entfernen möchten.

1. Klicken Sie auf **Bedingungen löschen**.

1. Klicken Sie in der Bestätigungsmeldung auf **Ja**.

    ![Hinzufügen von Nutzungsbedingungen](./media/active-directory-tou/delete-tou.png)

    Ihre Nutzungsbedingungen werden nicht mehr angezeigt.

## <a name="deleted-users-and-active-terms-of-use"></a>Gelöschte Benutzer und aktive Nutzungsbedingungen
Ein gelöschter Benutzer befindet sich in Azure AD standardmäßig 30 Tage lang in einem gelöschten Zustand. In diesem Zeitraum kann er von einem Administrator wiederhergestellt werden, sofern erforderlich.  Nach 30 Tagen wird dieser Benutzer endgültig gelöscht.  Darüber hinaus kann ein globaler Administrator über das Azure Active Directory-Portal explizit [einen kürzlich gelöschten Benutzer endgültig löschen](../fundamentals/active-directory-users-restore.md), bevor dieser Zeitraum abgelaufen ist.  Wurde ein Benutzer endgültig gelöscht, werden nachfolgende Daten für diesen Benutzer aus den aktiven Nutzungsbedingungen entfernt.  Überwachungsinformationen zu gelöschten Benutzern verbleiben im Überwachungsprotokoll.

## <a name="policy-changes"></a>Richtlinienänderungen
Richtlinien für den bedingten Zugriff werden sofort wirksam. In diesem Fall werden dem Administrator Cloudprobleme oder Azure AD-Tokenprobleme angezeigt. Der Administrator muss sich ab- und dann erneut anmelden, um die neue Richtlinie zu erfüllen.

>[!IMPORTANT]
> Betroffene Benutzer müssen sich zur Erfüllung einer neuen Richtlinie ab- und wieder anmelden, wenn Folgendes zutrifft:
> - Für eine Nutzungsbedingung wird eine Richtlinie für den bedingten Zugriff aktiviert. Oder:
> - Eine zweite Nutzungsbedingung wird erstellt.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

**F: Wie sehe ich, wann/ob ein Benutzer Nutzungsbedingungen akzeptiert hat?**</br>
A: Klicken Sie auf dem Nutzungsbedingungenblatt auf die Zahl unter **Akzeptiert**. Sie können die Zustimmungsaktivität auch in den Azure AD-Überwachungsprotokollen anzeigen oder suchen. Weitere Informationen finden Sie unter [Anzeigen des Berichts über abgelehnte und akzeptierte Nutzungsbedingungen](#view-who-has-accepted-and-declined) und [Anzeigen der Azure AD-Überwachungsprotokolle](#view-azure-ad-audit-logs).
 
**F: Wie lange werden die Informationen gespeichert?**</br>
A: Die Benutzeranzahl im Bericht zu den Nutzungsbedingungen sowie die Angabe, wer die Bedingungen akzeptiert/abgelehnt hat, werden für die Lebensdauer der Nutzungsbedingungen gespeichert. Die Azure AD-Überwachungsprotokolle werden 30 Tage lang gespeichert.

**F: Warum sehe ich eine andere Anzahl von Zustimmungen im Bericht zu den Nutzungsbedingungen als in den Azure AD-Überwachungsprotokollen?**</br>
A: Der Bericht zu den Nutzungsbedingungen wird für die Lebensdauer dieser Nutzungsbedingungen gespeichert, während Azure AD-Überwachungsprotokolle nur für 30 Tage gespeichert werden. Darüber hinaus werden im Bericht zu den Nutzungsbedingungen nur der aktuelle Zustimmungsstatus der Benutzer angezeigt. Lehnt ein Benutzer z.B. die Nutzungsbedingungen ab und akzeptiert sie aber später, enthält der Bericht zu den Nutzungsbedingungen nur die Zustimmung. Wenn Sie den Verlauf anzeigen möchten, können Sie in den Azure AD-Überwachungsprotokollen nachsehen.

**F: Müssen Benutzer die Nutzungsbedingungen erneut akzeptieren, wenn ich Details der Nutzungsbedingungen bearbeite?**</br>
A: Ja. Wenn ein Administrator die Details der Nutzungsbedingungen bearbeitet, müssen sie von den Benutzern erneut akzeptiert werden.

**F: Kann ich ein vorhandenes Dokument mit Nutzungsbedingungen aktualisieren?**</br>
A: Derzeit können Sie ein vorhandenes Dokument mit Nutzungsbedingungen nicht aktualisieren. Um ein Dokument mit Nutzungsbedingungen zu ändern, müssen Sie eine neue Instanz der Nutzungsbedingungen erstellen.

**F: Wenn das PDF-Dokument mit den Nutzungsbedingungen Links enthält, können Benutzer darauf klicken?**</br>
A: Die PDF-Datei wird standardmäßig im JPEG-Format gerendert, damit Links nicht angeklickt werden können. Benutzer haben die Möglichkeit, auf die Option **Probleme mit der Anzeige? Klicken Sie hier** zu klicken. Dadurch wird die PDF-Datei nativ gerendert, und Links werden unterstützt.

**F: Können Nutzungsbedingungen mehrere Sprachen unterstützen?**</br>
A: Ja. Derzeit sind 108 verschiedene Sprachen verfügbar, die ein Administrator für eine einzelne Version der Nutzungsbedingungen konfigurieren kann.

**F: Wann werden die Nutzungsbedingungen ausgelöst?**</br>
A: Die Nutzungsbedingungen werden bei der Anmeldung ausgelöst.

**F: Für welche Anwendungen kann ich Nutzungsbedingungen verwenden?**</br>
A: Sie können eine Richtlinie für den bedingten Zugriff für Unternehmensanwendungen mit moderner Authentifizierung erstellen.  Weitere Informationen finden Sie unter [Anzeigen aller von Ihnen verwaltbaren Unternehmens-Apps in Azure Active Directory](./../manage-apps/view-applications-portal.md).

**F: Kann ich einem bestimmten Benutzer oder einer bestimmten App mehrere Nutzungsbedingungen hinzufügen?**</br>
A: Ja. Erstellen Sie hierzu mehrere Richtlinien für den bedingten Zugriff, und richten Sie sie auf die entsprechenden Gruppen oder Anwendungen aus. Wenn für einen Benutzer mehrere Nutzungsbedingungen gelten, muss er die Nutzungsbedingungen nacheinander akzeptieren.
 
**F: Was passiert, wenn ein Benutzer die Nutzungsbedingungen ablehnt?**</br>
A: Der Benutzer kann nicht auf die Anwendung zugreifen. Wenn er auf die Anwendung zugreifen möchte, muss er sich erneut anmelden und die Bedingungen akzeptieren.
 
**F: Ist es möglich, die Zustimmung zu Nutzungsbedingungen rückgängig zu machen, die bereits akzeptiert wurden?**</br>
A: Sie können [zuvor akzeptierte Nutzungsbedingungen überprüfen](#how-users-can-review-their-terms-of-use), aber zurzeit besteht keine Möglichkeit, die Zustimmung rückgängig zu machen.

## <a name="next-steps"></a>Nächste Schritte

- [Best Practices für den bedingten Zugriff in Azure Active Directory](../../cognitive-services/qnamaker/concepts/best-practices.md)