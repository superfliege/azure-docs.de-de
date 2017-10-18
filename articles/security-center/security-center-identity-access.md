---
title: "Überwachen von Identität und Zugriff in Azure Security Center | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie mithilfe der Identitäts- und Zugriffsfunktion in Azure Security Center die Zugriffsaktivitäten der Benutzer sowie identitätsbezogene Probleme überwachen können."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 9f04e730-4cfa-4078-8eec-905a443133da
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/12/2017
ms.author: yurid
ms.openlocfilehash: 842045fbcb5b4f661cc006a207f4087370a7b4ab
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-identity-and-access-in-azure-security-center"></a>Überwachen von Identität und Zugriff in Azure Security Center
In diesem Artikel erfahren Sie, wie Sie mithilfe von Azure Security Center die Identität und Zugriffsaktivitäten von Benutzern überwachen.

## <a name="why-monitor-identity-and-access"></a>Gründe für das Überwachen der Identität und des Zugriffs
Die Identität sollte die Kontrollebene für Ihr Unternehmen darstellen, und der Schutz der Identität sollte höchste Priorität haben. In der Vergangenheit waren Unternehmen von Grenzen umgeben, die als eine der Hauptverteidigungsmaßnahmen dienten. Da heutzutage jedoch immer mehr Daten und Apps in die Cloud verlagert werden, ist die Identität zur neuen Grenze geworden.

Durch die Überwachung von Identitätsaktivitäten können Sie proaktive Maßnahmen ergreifen, bevor es zu einem Vorfall kommt, oder einen Angriffsversuch abwehren. Auf dem Dashboard „Identity & Access“ (Identität und Zugriff) können Sie sich anhand folgender Informationen einen Überblick über den Identitätszustand verschaffen:

* Anzahl erfolgloser Anmeldeversuche 
* Bei diesen Versuchen verwendete Benutzerkonten
* Gesperrte Konten
* Konten mit geändertem oder zurückgesetztem Kennwort 
* Anzahl angemeldeter Konten

## <a name="monitor-identity-and-access-activities"></a>Überwachen von Identitäts- und Zugriffsaktivitäten
Öffnen Sie zum Anzeigen der aktuellen Identitäts- und Zugriffsaktivitäten das Dashboard **Identity & Access** (Identität und Zugriff):

1. Öffnen Sie das Dashboard **Security Center**.

2. Klicken Sie im linken Bereich unter **Prävention** auf **Identity & Access** (Identität und Zugriff). Wenn Sie mehrere Arbeitsbereiche verwenden, wird die Arbeitsbereichsauswahl angezeigt.

    ![Arbeitsbereichsauswahl](./media/security-center-identity-access\security-center-identity-access-fig1.png)

    > [!NOTE]
    > Wenn in der Spalte ganz rechts **UPGRADE PLAN** (PLAN UPGRADEN) angezeigt wird, wird für diesen Arbeitsbereich das kostenlose Abonnement verwendet. Führen Sie ein Upgrade auf das Standard-Abonnement durch, um das Feature verwenden zu können. Wenn in der Spalte ganz rechts **REQUIRES UPDATE** (UPDATE ERFORDERLICH) angezeigt wird, aktualisieren Sie [Azure Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview), um das Feature verwenden zu können. Weitere Informationen zum Tarif finden Sie unter „Security Center – Preise“. 
    > 
3. Falls Sie mehrere Arbeitsbereiche untersuchen müssen, können Sie die Untersuchung anhand der Spalte **FAILED LOGONS** (FEHLGESCHLAGENE ANMELDUNGEN) priorisieren. In dieser Spalte wird die aktuelle Anzahl nicht erfolgreicher Anmeldeversuche für den Arbeitsbereich angezeigt. Wählen Sie den gewünschten Arbeitsbereich aus. Das Dashboard **Identity & Access** (Identität und Zugriff) wird angezeigt.

    ![„Identity & Access“ (Identität und Zugriff)](./media/security-center-identity-access\security-center-identity-access-fig2.png)

4. Die in diesem Dashboard verfügbaren Informationen helfen Ihnen beim Identifizieren einer potenziell verdächtigen Aktivität. Dieses Dashboard ist in drei Hauptbereiche unterteilt:

    a. **Identitätsstatus**. Enthält eine Zusammenfassung der identitätsbezogenen Aktivitäten in diesem Arbeitsbereich.

    b. **Failed logons** (Fehlgeschlagene Anmeldungen). Ermöglicht die schnelle Ermittlung der Hauptursache für nicht erfolgreiche Anmeldeversuche. In einer werden die zehn Konten mit den meisten erfolglosen Anmeldeversuchen angezeigt.

    c. **Anmeldungen im zeitlichen Verlauf**. Ermöglicht die schnelle Ermittlung der Anzahl von Anmeldungen im Zeitverlauf. Dazu wird eine Liste mit den meisten Computerkontoanmeldungen angezeigt.
    
Das angezeigte Dashboard basiert unabhängig von der ausgewählten Kachel auf der Protokollsuchabfrage. Der einzige Unterschied sind die Art der Abfrage und das Ergebnis. Sie können weiterhin ein Element (beispielsweise einen Computer) auswählen und relevante Daten anzeigen. 

## <a name="see-also"></a>Weitere Informationen
In diesem Artikel wurde beschrieben, wie Sie die Identität und den Zugriff in Security Center überwachen. Weitere Informationen zu Security Center finden Sie in den folgenden Artikeln:

* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Hier erfahren Sie, wie Sie Warnungen verwalten und auf Sicherheitsvorfälle in Security Center reagieren.
* [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md). Hier erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verstehen der Sicherheitswarnungen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Hier finden Sie Informationen zu den unterschiedlichen Arten von Sicherheitswarnungen.
* [Azure Security Center – Handbuch zur Problembehandlung](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Hier erfahren Sie, wie Sie allgemeine Probleme in Security Center behandeln. 
* [Azure Security Center – häufig gestellte Fragen](security-center-faq.md)festgelegt ist. Hier finden Sie häufig gestellte Fragen zur Verwendung von Security Center.
* [Azure Security-Blog](http://blogs.msdn.com/b/azuresecurity/). Hier finden Sie Blogbeiträge zur Sicherheit und Compliance von Azure.

