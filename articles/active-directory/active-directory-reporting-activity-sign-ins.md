---
title: Berichte zu Anmeldeaktivitäten im Azure Active Directory-Portal | Microsoft-Dokumentation
description: Enthält eine Einführung in die Berichte zu Anmeldeaktivitäten im Azure Active Directory-Portal.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 06/21/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 0d651f9d4fa48cec3a61f1f307f4447fe2cba63b
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2018
ms.locfileid: "39248950"
---
# <a name="sign-in-activity-reports-in-the-azure-active-directory-portal"></a>Berichte zu Anmeldeaktivitäten im Azure Active Directory-Portal

Mit der Azure Active Directory-Berichterstellungsfunktion (Azure AD) im [Azure-Portal](https://portal.azure.com) können Sie alle Informationen abrufen, die Sie zum Ermitteln des Zustands Ihrer Umgebung benötigen.

Die Architektur für die Berichterstellung in Azure Active Directory umfasst die folgenden Komponenten:

- **Aktivität** 
    - **Anmeldeaktivitäten** : Informationen zur Nutzung von verwalteten Anwendungen und Aktivitäten der Benutzeranmeldung
    - **Überwachungsprotokolle:** Systemaktivitätsinformationen zu Benutzern und zur Gruppenverwaltung sowie zu verwalteten Anwendungen und Verzeichnisaktivitäten.
- **Sicherheit** 
    - **Riskante Anmeldungen:** Eine riskante Anmeldung ist ein Indikator für einen Anmeldeversuch von einem Benutzer, der nicht der rechtmäßige Besitzer eines Benutzerkontos ist. Weitere Informationen finden Sie unter „Riskante Anmeldungen“.
    - **Benutzer mit Risikomarkierung:** Ein Benutzer mit Risikomarkierung ist ein Indikator für ein möglicherweise kompromittiertes Benutzerkonto. Weitere Informationen finden Sie unter „Benutzer mit Risikomarkierung“.

In diesem Thema erhalten Sie einen Überblick über die Anmeldeaktivitäten.

## <a name="prerequisites"></a>Voraussetzungen

### <a name="who-can-access-the-data"></a>Wer kann auf die Daten zugreifen?
* Benutzer mit den Rollen „Sicherheitsadministrator“, „Benutzer mit Leseberechtigung für Sicherheitsfunktionen“ oder „Berichtsleser“
* Globale Administratoren
* Jeder Benutzer (Nicht-Administratoren) kann auf seine eigenen Anmeldungen zugreifen. 

### <a name="what-azure-ad-license-do-you-need-to-access-sign-in-activity"></a>Welche Azure AD-Lizenz benötigen Sie für den Zugriff auf die Anmeldeaktivität?
* Ihrem Mandanten muss eine Azure AD Premium-Lizenz zugewiesen sein, damit der Gesamtbericht für Anmeldeaktivitäten angezeigt werden kann.


## <a name="sign-in-activities"></a>Anmeldeaktivitäten

Mit den Informationen, die vom Bericht zur Benutzeranmeldung geliefert werden, können Sie beispielsweise Antworten auf folgende Fragen ermitteln:

* Wie sieht das Anmeldemuster eines Benutzers aus?
* Wie viele Benutzer haben sich im Laufe einer Woche angemeldet?
* Wie lautet der Status dieser Anmeldungen?

Ihr erster Einstiegspunkt für Daten zu allen Anmeldeaktivitäten ist **Anmeldungen** im Abschnitt „Aktivität“ von **Azure Active Directory**.


![Anmeldeaktivität](./media/active-directory-reporting-activity-sign-ins/61.png "Anmeldeaktivität")


Ein Anmeldungsprotokoll enthält eine Standardlistenansicht mit folgenden Informationen:

- Anmeldedatum
- Zugehöriger Benutzer
- Anwendung, bei der sich der Benutzer angemeldet hat
- Anmeldestatus
- Status der Risikoerkennung
- Status der MFA-Anforderung (Multi-Factor Authentication)

![Anmeldeaktivität](./media/active-directory-reporting-activity-sign-ins/01.png "Anmeldeaktivität")

Sie können die Listenansicht anpassen, indem Sie in der Symbolleiste auf **Spalten** klicken.

![Anmeldeaktivität](./media/active-directory-reporting-activity-sign-ins/19.png "Anmeldeaktivität")

Sie können dann weitere Felder anzeigen oder Felder entfernen, die bereits angezeigt werden.

![Anmeldeaktivität](./media/active-directory-reporting-activity-sign-ins/02.png "Anmeldeaktivität")

Wenn Sie in der Listenansicht auf einen Eintrag klicken, werden alle dazu verfügbaren Details in einer horizontalen Ansicht angezeigt.

![Anmeldeaktivität](./media/active-directory-reporting-activity-sign-ins/03.png "Anmeldeaktivität")

> [!NOTE]
> Benutzer können nun in allen Anmeldeberichten Probleme mit Richtlinien für bedingten Zugriff beheben. Durch Klicken auf die Registerkarte **Bedingter Zugriff** eines Anmeldedatensatzes können Benutzer den Status des bedingten Zugriffs überprüfen und die Richtliniendetails für die Anmeldung sowie die jeweiligen Richtlinienergebnisse ansehen.
> Weitere Informationen finden Sie in den [häufig gestellten Fragen zum bedingten Zugriff in allen Anmeldungen](active-directory-reporting-faq.md#conditional-access).

![Anmeldeaktivität](./media/active-directory-reporting-activity-sign-ins/ConditionalAccess.png "Anmeldeaktivität")


## <a name="filter-sign-in-activities"></a>Filtern von Anmeldeaktivitäten

Sie können die Anmeldungsdaten mit den folgenden Standardfeldern filtern, um die gemeldeten Daten einzugrenzen:

- Benutzer
- Anwendung
- Anmeldestatus
- Status der Risikoerkennung
- Datum

![Anmeldeaktivität](./media/active-directory-reporting-activity-sign-ins/04.png "Anmeldeaktivität")

Mit dem Filter **Benutzer** können Sie den Namen oder den Dienstprinzipalnamen (UPN) des gewünschten Benutzers angeben.

Mit dem Filter **Anwendung** können Sie den Namen der gewünschten Anwendung angeben.

Für den Filter **Anmeldestatus** können Sie eine der folgenden Optionen auswählen:

- Alle
- Erfolgreich
- Fehler

Für den Filter **Risiko erkannt** können Sie eine der folgenden Optionen auswählen:

- Alle
- JA
- Nein 

Mit dem Filter **Datum** können Sie einen Zeitrahmen für die zurückgegebenen Daten festlegen.  
Mögliche Werte:

- 1 Monat
- 7 Tage
- 24 Stunden
- Benutzerdefiniertes Zeitintervall

Beim Auswählen eines benutzerdefinierten Zeitraums können Sie eine Startzeit und eine Endzeit konfigurieren.

Wenn Sie der Anmeldungsansicht zusätzliche Felder hinzufügen, werden diese automatisch in die Filterliste aufgenommen. Wenn Sie Ihrer Liste also beispielsweise das Feld **Client-App** hinzufügen, erhalten Sie auch eine weitere Filteroption, mit der Sie folgende Filter festlegen können:

- "Browser"      
- Exchange ActiveSync (unterstützt)               
- Exchange ActiveSync (nicht unterstützt)
- Andere Clients               
    - IMAP
    - MAPI
    - Ältere Office-Clients
    - POP
    - SMTP


![Anmeldeaktivität](./media/active-directory-reporting-activity-sign-ins/12.png "Anmeldeaktivität")


## <a name="download-sign-in-activities"></a>Herunterladen von Anmeldeaktivitäten

Sie können die Daten zu Anmeldeaktivitäten herunterladen, wenn Sie sie außerhalb des Azure-Portals verwenden möchten. Wenn Sie auf **Herunterladen** klicken, wird eine CSV-Datei mit den letzten 5.000 Datensätzen erstellt.  Neben einer Downloadschaltfläche steht im Azure-Portal eine Option zum Generieren eines Skripts zum Herunterladen Ihrer Daten zur Verfügung.  

![Herunterladen](./media/active-directory-reporting-activity-sign-ins/71.png "Herunterladen")

Falls eine höhere Flexibilität erforderlich ist, können Sie die Skriptoption verwenden. Wenn Sie auf **Skript** klicken, wird ein PowerShell-Skript mit allen von Ihnen festgelegten Filtern erstellt. Laden Sie dieses Skript herunter, und führen Sie es im **Administratormodus** aus, um die CSV-Datei zu generieren. 

### <a name="running-the-script-on-a-windows-10-machine"></a>Ausführen des Skripts auf einem Windows 10-Computer

Wenn Sie das Skript auf einem **Windows 10**-Computer ausführen möchten, müssen Sie zuerst einige zusätzliche Schritte ausführen. 

1. Installieren Sie das [AzureRM-Modul](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-6.4.0l).
2. Importieren Sie das Modul, indem Sie eine PowerShell-Eingabeaufforderung öffnen und den Befehl **Import-Module AzureRM** ausführen.
3. Führen Sie **Set-ExecutionPolicy unrestricted**aus, und wählen Sie **Ja, alle** aus. 
4. Nun können Sie das heruntergeladene PowerShell-Skript im Administratormodus ausführen, um die CSV-Datei zu generieren.

Neben der technischen Implementierung ist die Anzahl der herunterladbaren Datensätze durch die [Aufbewahrungsrichtlinien für Azure Active Directory-Berichte](active-directory-reporting-retention.md) eingeschränkt.  


## <a name="sign-in-activities-shortcuts"></a>Optionen für Anmeldeaktivitäten

Zusätzlich zu Azure Active Directory enthält das Azure-Portal weitere Einstiegspunkte für Daten zu Anmeldeaktivitäten:

- Übersicht über Identitätssicherheit und Schutz
- Benutzer
- Gruppen
- Unternehmensanwendungen


### <a name="users-sign-ins-activities"></a>Anmeldeaktivitäten von Benutzern

Mit den Informationen, die vom Bericht zur Benutzeranmeldung geliefert werden, können Sie beispielsweise Antworten auf folgende Fragen ermitteln:

- Wie sieht das Anmeldemuster eines Benutzers aus?
- Wie viele Benutzer sind für Benutzer im Laufe einer Woche angemeldet?
- Wie lautet der Status dieser Anmeldungen?

Ihr Einstiegspunkt für diese Daten ist der Graph zu Benutzeranmeldungen auf der Übersichtsseite **Identitätssicherheit und Schutz**. Der Graph für die Benutzeranmeldung zeigt wöchentliche Aggregationen von Anmeldungen für alle Benutzer in einem bestimmten Zeitraum an. Die Standardeinstellung für den Zeitraum beträgt 30 Tage.

![Anmeldeaktivität](./media/active-directory-reporting-activity-sign-ins/06.png "Anmeldeaktivität")

Wenn Sie im Anmeldungsgraph auf einen Tag klicken, wird eine Übersicht über die Anmeldeaktivitäten für den entsprechenden Tag angezeigt.

Jede Zeile in der Liste mit den Anmeldeaktivitäten zeigt Folgendes:

* Wer hat sich angemeldet?
* Welche Anwendung war das Ziel der Anmeldung?
* Welchen Status hat die Anmeldung?
* Welchen MFA-Status hat die Anmeldung?

Durch Klicken auf ein Element können Sie ausführlichere Informationen zum entsprechenden Anmeldevorgang anzeigen:

- Benutzer-ID
- Benutzer
- Username
- Anwendungs-ID
- Anwendung
- Client
- Standort
- IP-Adresse
- Datum
- MFA erforderlich
- Anmeldestatus

 
Wenn Sie auf der Seite **Benutzer** im Abschnitt **Aktivität** auf **Anmeldevorgänge** klicken, wird eine umfassenden Übersicht über alle Benutzeranmeldungen angezeigt.

![Anmeldeaktivität](./media/active-directory-reporting-activity-sign-ins/08.png "Anmeldeaktivität")

## <a name="usage-of-managed-applications"></a>Nutzung von verwalteten Anwendungen

Mit einer anwendungsorientierten Ansicht Ihrer Anmeldedaten können Sie beispielsweise folgende Fragen beantworten:

* Wer verwendet meine Anwendungen?
* Welche drei Anwendungen sind im Unternehmen am beliebtesten?
* Ich habe vor Kurzem eine Anwendung eingeführt. Wie gut funktioniert sie?

Ihr Einstiegspunkt für diese Daten ist die Liste mit den beliebtesten drei Anwendungen in Ihrem Unternehmen im Bericht zu den letzten 30 Tagen (Abschnitt **Übersicht** unter **Unternehmensanwendungen**).

![Anmeldeaktivität](./media/active-directory-reporting-activity-sign-ins/10.png "Anmeldeaktivität")

Der Graph zur App-Nutzung gibt die wöchentlichen Aggregationen von Anmeldungen für Ihre beliebtesten drei Anwendungen in einem bestimmten Zeitraum an. Die Standardeinstellung für den Zeitraum beträgt 30 Tage.

![Anmeldeaktivität](./media/active-directory-reporting-activity-sign-ins/47.png "Anmeldeaktivität")

Wenn Sie möchten, können Sie den Fokus auf eine bestimmte Anwendung festlegen.

![Berichterstellung](./media/active-directory-reporting-activity-sign-ins/single_spp_usage_graph.png "Reporting")

Wenn Sie im Graph für die App-Nutzung auf einen Tag klicken, wird eine ausführliche Liste mit den Anmeldeaktivitäten angezeigt.

Mit der Option **Anmeldungen** können Sie eine vollständige Übersicht über alle Anmeldeereignisse für Ihre Anwendungen anzeigen.

![Anmeldeaktivität](./media/active-directory-reporting-activity-sign-ins/11.png "Anmeldeaktivität")



## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Fehlercodes von Anmeldeaktivitäten finden Sie unter [Fehlercodes des Berichts mit den Anmeldeaktivitäten im Azure Active Directory-Portal](active-directory-reporting-activity-sign-ins-errors.md).

