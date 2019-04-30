---
title: Verwenden von Azure Monitor-Arbeitsmappen für Azure Active Directory-Berichte | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Azure Monitor-Arbeitsmappen für Azure Active Directory-Berichte verwenden.
services: active-directory
author: MarkusVi
manager: daveba
ms.assetid: 4066725c-c430-42b8-a75b-fe2360699b82
ms.service: active-directory
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: ''
ms.workload: identity
ms.subservice: report-monitor
ms.date: 04/18/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 2c9b3d0ef110fea0629af345a71d0d7b7cce7313
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2019
ms.locfileid: "60012911"
---
# <a name="how-to-use-azure-monitor-workbooks-for-azure-active-directory-reports"></a>Gewusst wie: Verwenden von Azure Monitor-Arbeitsmappen für Azure Active Directory-Berichte

Möchten Sie:

- Die Auswirkungen Ihrer [Richtlinien für bedingten Zugriff](../conditional-access/overview.md) auf die Anmeldung Ihrer Benutzer verstehen?

- Anmeldefehler beheben, um eine bessere Übersicht über die Anmeldeintegrität in Ihrer Organisation zu erhalten, sowie Probleme schnell beheben?

- Wissen, welche Benutzer sich über Legacyauthentifizierungen bei Ihrer Umgebung anmelden? Durch [Blockieren der Legacyauthentifizierung](../conditional-access/block-legacy-authentication.md) können Sie den Schutz Ihres Mandanten verbessern.


[Azure Monitor-Arbeitsmappen](https://docs.microsoft.com/azure/azure-monitor/app/usage-workbooks) kombinieren Text, Analyseabfragen, Azure-Metriken und Parameter zu umfassenden interaktiven Berichten. Azure Active Directory umfasst Arbeitsmappen, mit deren Hilfe Sie Antworten auf die Fragen oben erhalten.

Dieser Artikel:

- Setzt voraus, dass Sie mit dem [Erstellen interaktiver Berichte mit Azure Monitor-Arbeitsmappen](https://docs.microsoft.com/azure/azure-monitor/app/usage-workbooks) vertraut sind.

- Erläutert, wie Sie mithilfe der Azure Monitor-Arbeitsmappen zur Überwachung Antworten auf die Fragen oben erhalten.
 


## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen Folgendes, um dieses Feature verwenden zu können:

- Einen Azure Active Directory-Mandanten mit einer Premium-Lizenz (P1/P2). Wie Sie eine Premium-Lizenz erhalten, erfahren Sie [hier](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-get-started-premium).

- Einen [Log Analytics-Arbeitsbereich](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace).

## <a name="access-workbooks"></a>Zugriff auf Arbeitsmappen 

So greifen Sie auf Arbeitsmappen zu

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Klicken Sie auf der linken Navigationsleiste auf **Azure Active Directory**.

3. Klicken Sie im Abschnitt **Überwachung** auf **Insights**. 

    ![Einblicke](./media/howto-use-azure-monitor-workbooks/41.png)

4. Klicken Sie auf einen Bericht oder eine Vorlage oder auf der Symbolleiste auf **Öffnen**. 

    ![Gallery](./media/howto-use-azure-monitor-workbooks/42.png)


## <a name="sign-in-analysis"></a>Analyse der Anmeldungen

Klicken Sie für den Zugriff auf die Arbeitsmappe zur Analyse der Anmeldungen im Abschnitt **Nutzung** auf **Anmeldungen**. 

Mit dieser Arbeitsmappe werden die folgenden Trends für Anmeldungen angezeigt:

- Alle Anmeldungen

- Erfolgreich

- Benutzeraktion ausstehend

- Fehler

Sie können die einzelnen Trends nach folgenden Komponenten filtern:

- Zeitbereich

- Apps

- Benutzer

![Gallery](./media/howto-use-azure-monitor-workbooks/43.png)


Für jeden Trend erhalten Sie eine Aufschlüsselung nach folgenden Bereichen:

- Standort

    ![Gallery](./media/howto-use-azure-monitor-workbooks/45.png)

- Gerät

    ![Gallery](./media/howto-use-azure-monitor-workbooks/46.png)


## <a name="sign-ins-using-legacy-authentication"></a>Anmeldungen mit Legacyauthentifizierung 


Klicken Sie für den Zugriff auf die Arbeitsmappe für Anmeldungen mit [Legacyauthentifizierung](../conditional-access/block-legacy-authentication.md) im Abschnitt **Nutzung** auf **Anmeldungen mit Legacyauthentifizierung**. 

Mit dieser Arbeitsmappe werden die folgenden Trends für Anmeldungen angezeigt:

- Alle Anmeldungen

- Erfolgreich


Sie können die einzelnen Trends nach folgenden Komponenten filtern:

- Zeitbereich

- Apps

- Benutzer

- Protokolle 

![Gallery](./media/howto-use-azure-monitor-workbooks/47.png)


Für jeden Trend erhalten Sie eine Aufschlüsselung nach App und Protokoll.

![Gallery](./media/howto-use-azure-monitor-workbooks/48.png)



## <a name="sign-ins-by-conditional-access"></a>Anmeldungen mithilfe des bedingten Zugriffs 


Klicken Sie für den Zugriff auf die Arbeitsmappe für Anmeldungen nach [Richtlinien für bedingten Zugriff](../conditional-access/overview.md) im Abschnitt **Bedingter Zugriff** auf **Anmeldungen mithilfe des bedingten Zugriffs**. 

Mit dieser Arbeitsmappe wird der Trend für deaktivierte Anmeldungen angezeigt.

Sie können die einzelnen Trends nach folgenden Komponenten filtern:

- Zeitbereich

- Apps

- Benutzer

![Gallery](./media/howto-use-azure-monitor-workbooks/49.png)


Für die deaktivierten Anmeldungen erhalten Sie eine Aufschlüsselung nach dem Status des bedingten Zugriffs.

![Status des bedingten Zugriffs](./media/howto-use-azure-monitor-workbooks/conditional-access-status.png)








## <a name="sign-ins-by-grant-controls"></a>Anmeldungen nach Steuerelementen zur Rechteerteilung

Klicken Sie für den Zugriff auf die Arbeitsmappe für Anmeldungen nach [Steuerelementen zur Rechteerteilung](../conditional-access/controls.md) im Abschnitt **Bedingter Zugriff** auf **Anmeldungen nach Steuerelementen zur Rechteerteilung**. 

Mit dieser Arbeitsmappe werden die folgenden Trends für deaktivierte Anmeldungen angezeigt:

- Anfordern von MFA
 
- Vorschreiben der Verwendung von Nutzungsbedingungen

- Vorschreiben von Datenschutzbestimmungen

- Andere


Sie können die einzelnen Trends nach folgenden Komponenten filtern:

- Zeitbereich

- Apps

- Benutzer

![Gallery](./media/howto-use-azure-monitor-workbooks/50.png)


Für jeden Trend erhalten Sie eine Aufschlüsselung nach App und Protokoll.

![Gallery](./media/howto-use-azure-monitor-workbooks/51.png)




## <a name="sign-ins-failure-analysis"></a>Analyse der Anmeldungsfehler

Verwenden Sie die Arbeitsmappe zur **Analyse der Anmeldungsfehler**, um Fehler in folgenden Bereichen zu beheben:

- Anmeldungen
- Richtlinien für bedingten Zugriff
- Legacyauthentifizierung 


Klicken Sie für den Zugriff auf die Arbeitsmappe für Anmeldungen nach Daten zum bedingten Zugriff im Abschnitt **Problembehandlung** auf **Anmeldungen mit Legacyauthentifizierung**. 

Mit dieser Arbeitsmappe werden die folgenden Trends für Anmeldungen angezeigt:

- Alle Anmeldungen

- Erfolgreich

- Ausstehende Aktion

- Fehler


Sie können die einzelnen Trends nach folgenden Komponenten filtern:

- Zeitbereich

- Apps

- Benutzer

![Gallery](./media/howto-use-azure-monitor-workbooks/52.png)


Für die Problembehandlung von Anmeldungen erhalten Sie eine Aufschlüsselung nach folgenden Bereichen:

- Häufigste Fehler

    ![Gallery](./media/howto-use-azure-monitor-workbooks/53.png)

- Anmeldungen mit Benutzeraktionen

    ![Gallery](./media/howto-use-azure-monitor-workbooks/54.png)






## <a name="next-steps"></a>Nächste Schritte

* [Erstellen interaktiver Berichte mit Azure Monitor-Arbeitsmappen](https://docs.microsoft.com/azure/azure-monitor/app/usage-workbooks)