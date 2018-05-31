---
title: Azure Active Directory-Nutzungsbedingungen | Microsoft-Dokumentation
description: Mit den Azure AD-Nutzungsbedingungen können Sie und Ihr Unternehmen Nutzungsbedingungen für Benutzer von Azure AD-Diensten angeben.
services: active-directory
author: billmath
manager: mtillman
editor: ''
ms.assetid: d55872ef-7e45-4de5-a9a0-3298e3de3565
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/15/2018
ms.author: billmath
ms.openlocfilehash: 428d70474ba928a9e0c774aeb16395ef6a4cea2e
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
ms.locfileid: "34192954"
---
# <a name="azure-active-directory-terms-of-use-feature"></a>Nutzungsbedingungsfeature für Azure Active Directory
Mit den Azure AD-Nutzungsbedingungen können Organisationen komfortabel Informationen für Endbenutzer anzeigen.  Dadurch wird sichergestellt, dass Benutzern relevante Haftungsausschlüsse angezeigt werden, um rechtliche oder compliancebezogene Anforderungen zu erfüllen.

Der Inhalt von Azure AD Nutzungsbedingungen wird im PDF-Format bereitgestellt.   Das PDF-Format kann für beliebige Inhalte verwendet werden – etwa für vorhandene Vertragsdokumente, sodass Sie bei der Benutzeranmeldung die Zustimmung von Endbenutzern einholen können.  Die Nutzungsbedingungen können für Anwendungen oder Benutzergruppen verwendet werden (oder für unterschiedliche Zwecke, falls Sie über mehrere Nutzungsbedingungen verfügen).

Im weiteren Verlauf dieses Dokuments erfahren Sie, wie Sie Azure AD-Nutzungsbedingungen verwenden.  

## <a name="why-use-azure-ad-terms-of-use"></a>Argumente für die Verwendung von Azure AD-Nutzungsbedingungen
Sie haben Schwierigkeiten damit, Mitarbeiter oder Gäste vor der Zugriffsgewährung dazu zu bringen, Ihren Nutzungsbedingungen zuzustimmen? Sie benötigen Hilfe bei der Frage, wer den Nutzungsbedingungen Ihres Unternehmens zugestimmt hat und wer nicht?  Mit den Azure AD-Nutzungsbedingungen können Organisationen komfortabel Informationen für Endbenutzer anzeigen.  Dadurch wird sichergestellt, dass ihnen relevante Haftungsausschlüsse angezeigt werden, um rechtliche oder compliancebezogene Anforderungen zu erfüllen.

Azure AD-Nutzungsbedingungen können in folgenden Szenarien verwendet werden:
-   Allgemeine Nutzungsbedingungen für alle Benutzer in Ihrer Organisation
-   Spezifische Nutzungsbedingungen auf der Grundlage von Benutzerattributen (beispielsweise Ärzte/Pflegepersonal oder Mitarbeiter im Inland/Ausland – unter Verwendung [dynamischer Gruppen](https://azure.microsoft.com/updates/azure-active-directory-dynamic-membership-for-groups))
-   Spezifische Nutzungsbedingungen auf der Grundlage besonders geschäftsrelevanter Apps (beispielsweise Salesforce)


## <a name="prerequisites"></a>Voraussetzungen
Führen Sie zum Konfigurieren der Azure AD-Nutzungsbedingungen die folgenden Schritte aus:

1. Melden Sie sich als globaler Administrator, Sicherheitsadministrator oder Administrator für den bedingten Zugriff für das Verzeichnis an, für das Sie Azure AD-Nutzungsbedingungen konfigurieren möchten.
2. Vergewissern Sie sich, dass das Verzeichnis über einen der folgenden Azure AD-Abonnementtypen verfügt: Premium P1, P2, EMS E3 oder EMS E5.  Holen Sie sich andernfalls [Azure AD Premium](active-directory-get-started-premium.md), oder verwenden Sie eine [Testversion](https://azure.microsoft.com/trial/get-started-active-directory/).
3. Zeigen Sie unter [https://aka.ms/catou](https://aka.ms/catou) das Dashboard für Azure AD-Nutzungsbedingungen an.

>[!IMPORTANT]
>Die Richtlinie für den bedingten Zugriff (einschließlich Nutzungsbedingungen) unterstützt die Erzwingung für Dienstkonten nicht.  Es wird empfohlen, alle Dienstkonten von der Richtlinie für den bedingten Zugriff auszuschließen.

## <a name="add-company-terms-of-use"></a>Hinzufügen von Unternehmensnutzungsbedingungen
Gehen Sie nach Fertigstellung Ihrer Nutzungsbedingungen wie folgt vor, um sie hinzuzufügen.

### <a name="to-add-terms-of-use"></a>So fügen Sie Nutzungsbedingungen hinzu
1. Navigieren Sie zum Dashboard unter [https://aka.ms/catou](https://aka.ms/catou).
2. Klicken Sie auf "Hinzufügen".</br>
![Hinzufügen von Nutzungsbedingungen](media/active-directory-tou/tou12.png)
3. Geben Sie unter **Name** einen Namen für die Nutzungsbedingungen ein.
4. Geben Sie unter **Anzeigename** einen Anzeigenamen ein.  Die Überschrift wird Benutzern bei der Anmeldung angezeigt.
5. **Navigieren**Sie zur PDF-Datei mit den fertig gestellten Nutzungsbedingungen, und wählen Sie sie aus.  Wir empfehlen den Schriftgrad 24.
6. **Wählen** Sie eine Sprache für die Nutzungsbedingungen.  Die Sprachoption ermöglicht das Hochladen mehrerer Nutzungsbedingungen in verschiedenen Sprachen.  Die Version der Nutzungsbedingungen, die einem Endbenutzer angezeigt wird, basiert auf den Browsereinstellungen.
7. Aktivieren oder deaktivieren Sie **Benutzer müssen die Nutzungsbedingungen erweitern**.  Wenn diese Einstellung aktiviert ist, müssen Endbenutzer die Nutzungsbedingungen anzeigen, bevor sie sie akzeptieren.
8. Unter **Bedingter Zugriff** können Sie die hochgeladenen Nutzungsbedingungen durch Auswahl einer Vorlage in der Dropdownliste oder mithilfe einer benutzerdefinierten Richtlinie für den bedingten Zugriff **erzwingen**.  Benutzerdefinierte Richtlinien für den bedingten Zugriff ermöglichen präzise Nutzungsbedingungen bis hin zu einer spezifischen Cloudanwendung oder Benutzergruppe.  Weitere Informationen finden Sie unter [Best Practices für den bedingten Zugriff in Azure Active Directory](active-directory-conditional-access-best-practices.md).
9. Klicken Sie auf **Create**.
10. Nach dem Auswählen einer benutzerdefinierten Vorlage für bedingten Zugriff wird ein neuer Bildschirm angezeigt, auf dem Sie die Richtlinie für den bedingten Zugriff anpassen können.
11. Ihre neuen Nutzungsbedingungen werden angezeigt.</br>

![Hinzufügen von Nutzungsbedingungen](media/active-directory-tou/tou3.png)

## <a name="delete-terms-of-use"></a>Löschen von Nutzungsbedingungen
Veraltete Nutzungsbedingungen können wie folgt entfernt oder gelöscht werden:

### <a name="to-delete-terms-of-use"></a>So löschen Sie die Nutzungsbedingungen
1. Navigieren Sie zum Dashboard unter [https://aka.ms/catou](https://aka.ms/catou).
2. Wählen Sie die Nutzungsbedingungen aus, die Sie entfernen möchten.
3. Klicken Sie auf **Löschen**.
4. Ihre neuen Nutzungsbedingungen werden nicht mehr angezeigt.


## <a name="viewing-current-user-status"></a>Anzeigen des aktuellen Benutzerstatus
Sie sehen, dass für die Nutzungsbedingungen eine Zahl für die Benutzer angezeigt wird, die die Bedingungen akzeptiert und abgelehnt haben.

![Überwachungsereignis](media/active-directory-tou/tou15.png)

Sie können auf die Zahl unter **Akzeptiert** oder **Abgelehnt** klicken, um den aktuellen Status von Benutzern anzuzeigen.

![Überwachungsereignis](media/active-directory-tou/tou16.png)

## <a name="audit-terms-of-use"></a>Überwachen von Nutzungsbedingungen
Wenn Sie Verlaufsdaten zur Annahme bzw. Ablehnung der Nutzungsbedingungen und nicht nur den aktuellen Status anzeigen möchten, können Sie die benutzerfreundliche Überwachungsfunktion der Azure AD-Nutzungsbedingungen verwenden.  Mit dieser Überwachungsfunktion können Sie anzeigen, wer die Nutzungsbedingungen wann akzeptiert hat.  

Es gibt zwei Verwendungsmöglichkeiten für die Überwachung, abhängig davon, welche Aktion Sie gerade ausführen möchten.  


Gehen Sie wie folgt vor, um mit der Überwachung zu beginnen:

### <a name="to-audit-terms-of-use"></a>So überwachen Sie die Nutzungsbedingungen
1. Navigieren Sie zum Dashboard unter [https://aka.ms/catou](https://aka.ms/catou).
2. Klicken Sie auf „Überwachungsprotokolle anzeigen“.</br>
![Überwachungsereignis](media/active-directory-tou/tou8.png)
3.  Die Informationen auf dem Bildschirm mit den Azure AD-Überwachungsprotokollen können mithilfe der bereitgestellten Dropdownlisten nach bestimmten Überwachungsprotokollinformationen gefiltert werden.
[Überwachungsereignis](media/active-directory-tou/tou9.png)
4.  Die Informationen können auch in eine CSV-Datei heruntergeladen und lokal verwendet werden.

## 

## <a name="what-users-see"></a>Anzeige für Benutzer
Wenn Nutzungsbedingungen erstellt und erzwungen wurden, wird den betroffenen Benutzern Folgendes angezeigt.  Diese Bildschirme werden bei der Anmeldung angezeigt.
-   Der Schriftgrad in der PDF-Datei sollte 24 betragen.
![Überwachungsereignis](media/active-directory-tou/tou10.png)
-   Hier sehen Sie die Darstellung des Bildschirms auf Mobilgeräten.</br></br>
![Überwachungsereignis](media/active-directory-tou/tou11.png)

### <a name="review-terms-of-use"></a>Überprüfen der Nutzungsbedingungen
Benutzer können die akzeptierten Nutzungsbedingungen überprüfen und anzeigen.  Gehen Sie wie folgt vor, um die Nutzungsbedingungen zu überprüfen:

1. Navigieren Sie zu [https://myapps.microsoft.com](https://myapps.microsoft.com), und melden Sie sich an.
2. Klicken Sie in der rechten oberen Ecke auf Ihren Namen, und wählen Sie in der Dropdownliste die Option **Profil** aus.
![Profil](media/active-directory-tou/tou14.png)

3. Klicken Sie in Ihrem Profil auf **Nutzungsbedingungen lesen**.
![Überwachungsereignis](media/active-directory-tou/tou13a.png)

4.  Hier können Sie sich die akzeptierten Nutzungsbedingungen ansehen. 

## <a name="removing-users-from-an-active-terms-of-use"></a>Entfernen von Benutzern aus aktiven Nutzungsbedingungen

[!INCLUDE [Privacy](../../includes/gdpr-intro-sentence.md)]

Ein gelöschter Benutzer bleibt in Azure AD standardmäßig 30 Tage lang gelöscht. In diesem Zeitraum kann er von einem Administrator wiederhergestellt werden, sofern erforderlich.  Nach 30 Tagen wird dieser Benutzer endgültig gelöscht.  Darüber hinaus kann ein globaler Administrator über das Azure Active Directory-Portal explizit [einen kürzlich gelöschten Benutzer endgültig löschen](active-directory-users-restore.md), bevor dieser Zeitraum abgelaufen ist.  Wurde ein Benutzer endgültig gelöscht, werden nachfolgende Daten zu diesem Benutzer aus den aktiven Nutzungsbedingungen entfernt.  Überwachungsinformationen zu gelöschten Benutzern verbleiben im Überwachungsprotokoll.



## <a name="additional-information"></a>Zusätzliche Informationen
Folgende Informationen können bei der Verwendung von Nutzungsbedingungen hilfreich sein.

>[!IMPORTANT]
> Betroffene Benutzer müssen sich zur Erfüllung einer neuen Richtlinie ab- und wieder anmelden, wenn Folgendes zutrifft:
> - Für eine Nutzungsbedingung wird eine Richtlinie für den bedingten Zugriff aktiviert. Oder:
> - Eine zweite Nutzungsbedingung wird erstellt.
>
>Richtlinien für den bedingten Zugriff werden sofort wirksam. In diesem Fall werden dem Administrator Cloud- oder Azure AD-Tokenprobleme angezeigt. Der Administrator muss sich ab- und wieder anmelden, um die neue Richtlinie zu erfüllen.





## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

**F: Wie sehe ich, wann/ob ein Benutzer Nutzungsbedingungen akzeptiert hat?**</br>
A: Klicken Sie neben Ihren Nutzungsbedingungen auf die Zahl unter „Akzeptiert“.  Weitere Informationen finden Sie unter [Anzeigen des aktuellen Benutzerstatus](#viewing-current-user-status).  Ein Benutzer, der die Nutzungsbedingungen akzeptiert, wird außerdem in das Überwachungsprotokoll geschrieben. Sie können das Azure AD-Überwachungsprotokoll nach den Ergebnissen durchsuchen.  

**F: Müssen Benutzer die Nutzungsbedingungen erneut akzeptieren, wenn diese geändert werden?**</br>
A: Ja. Wenn ein Administrator die Nutzungsbedingungen ändert, müssen sie erneut akzeptiert werden.

**F: Können Nutzungsbedingungen mehrere Sprachen unterstützen?**</br>
A: Ja.  Derzeit sind 18 verschiedene Sprachen verfügbar, die ein Administrator für eine einzelne Version der Nutzungsbedingungen konfigurieren kann. 

**F: Wann werden die Nutzungsbedingungen ausgelöst?**</br>
A: Die Nutzungsbedingungen werden bei der Anmeldung ausgelöst.

**F: Für welche Anwendungen kann ich Nutzungsbedingungen verwenden?**</br>
A: Sie können eine Richtlinie für den bedingten Zugriff für Unternehmensanwendungen mit moderner Authentifizierung erstellen.  Weitere Informationen finden Sie unter [Anzeigen aller von Ihnen verwaltbaren Unternehmens-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-view-azure-portal).

**F: Kann ich einem bestimmten Benutzer oder einer bestimmten App mehrere Nutzungsbedingungen hinzufügen?**</br>
A: Ja. Erstellen Sie hierzu mehrere Richtlinien für den bedingten Zugriff, und richten Sie sie auf die entsprechenden Gruppen oder Apps aus. Wenn für einen Benutzer mehrere Nutzungsbedingungen gelten, muss er die Nutzungsbedingungen nacheinander akzeptieren.
 
**F: Was passiert, wenn ein Benutzer die Nutzungsbedingungen ablehnt?**</br>
A: Der Benutzer kann nicht auf die Anwendung zugreifen. Wenn er auf die Anwendung zugreifen möchte, muss er sich erneut anmelden und die Bedingungen akzeptieren.
