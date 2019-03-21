---
title: Azure Security Center-Tutorial – Reagieren auf Sicherheitsincidents | Microsoft-Dokumentation
description: Azure Security Center-Tutorial – Reagieren auf Sicherheitsincidents
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 181e3695-cbb8-4b4e-96e9-c4396754862f
ms.service: security-center
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/30/2018
ms.author: rkarlin
ms.openlocfilehash: d726006d3ecce69f129b1576c7c6d12833582873
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58081989"
---
# <a name="tutorial-respond-to-security-incidents"></a>Tutorial: Reagieren auf Sicherheitsvorfälle
Security Center analysiert Ihre Hybrid Cloud-Workloads ständig mithilfe von Advanced Analytics- und Threat Intelligence-Funktionen, um Sie vor schädlichen Aktivitäten warnen zu können. Darüber hinaus können Sie Warnungen aus anderen Sicherheitsprodukten und Diensten in Security Center integrieren und basierend auf Ihren eigenen Indikatoren oder Intelligence-Quellen benutzerdefinierte Warnungen erstellen. Nachdem eine Warnung generiert wurde, sind schnelle Maßnahmen erforderlich, um das Problem zu untersuchen und zu beheben. In diesem Lernprogramm lernen Sie Folgendes:

> [!div class="checklist"]
> * Selektieren von Sicherheitswarnungen
> * Durchführen von weiteren Untersuchungen, um die Grundursache und den Umfang eines Sicherheitsincidents zu ermitteln
> * Durchsuchen von Sicherheitsdaten zur Unterstützung der Untersuchung

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen
Zum Durchlaufen der in diesem Tutorial behandelten Features müssen Sie den Tarif „Standard“ von Security Center verwenden. Sie können Security Center Standard kostenlos testen. Weitere Informationen finden Sie auf der [Preisseite](https://azure.microsoft.com/pricing/details/security-center/). Unter [Schnellstarthandbuch zu Azure Security Center](security-center-get-started.md) wird Schritt für Schritt beschrieben, wie Sie das Upgrade auf den Tarif „Standard“ durchführen.

## <a name="triage-security-alerts"></a>Selektieren von Sicherheitswarnungen
Mit Security Center erhalten Sie einen einheitlichen Überblick über alle Sicherheitswarnungen. Sicherheitswarnungen werden basierend auf dem Schweregrad und bei einer möglichen Kombination von verwandten Warnungen in einem Sicherheitsincident eingestuft. Beachten Sie beim Selektieren von Warnungen und Incidents Folgendes:

- Schließen Sie Warnungen, für die keine zusätzliche Aktion erforderlich ist, z.B. bei einer falsch positiven Warnung.
- Treffen Sie Vorkehrungen zur Begegnung von bekannten Angriffen, z.B. Blockierung von Netzwerkdatenverkehr von einer schädlichen IP-Adresse.
- Ermitteln Sie Warnungen, die näher untersucht werden müssen.


1. Wählen Sie im Hauptmenü von Security Center unter **ERKENNUNG** die Option **Sicherheitswarnungen**:

   ![Sicherheitswarnungen](./media/tutorial-security-incident/tutorial-security-incident-fig1.png)  

2. Klicken Sie in der Liste mit den Warnungen auf einen Sicherheitsincident, bei dem es sich um eine Sammlung von Warnungen handelt, um weitere Informationen zu diesem Incident zu erhalten. **Security incident detected** (Sicherheitsincident erkannt) wird geöffnet.

   ![Sicherheitsvorfall](./media/tutorial-security-incident/tutorial-security-incident-fig2.png)

3. Auf diesem Bildschirm ist oben die Beschreibung des Sicherheitsincidents und die Liste mit den Warnungen angegeben, die Teil des Incidents sind. Klicken Sie auf die Warnung, die Sie näher untersuchen möchten, um weitere Informationen zu erhalten.

   ![Sicherheitsvorfall](./media/tutorial-security-incident/tutorial-security-incident-fig3.png)

   Die Art der Warnung kann variieren. Weitere Informationen zur Art der Warnung und zu potenziellen Lösungsschritten finden Sie unter [Verstehen der Sicherheitswarnungen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Für Warnungen, die ohne Probleme geschlossen werden können, können Sie mit der rechten Maustaste auf die Warnung klicken und die Option **Schließen** wählen:

   ![Warnung](./media/tutorial-security-incident/tutorial-security-incident-fig4.png)

4. Wenn die Grundursache und der Umfang der schädlichen Aktivität unbekannt sind, können Sie mit dem nächsten Schritt fortfahren, um dies weiter zu untersuchen.

## <a name="investigate-an-alert-or-incident"></a>Untersuchen einer Warnung oder eines Incidents
1. Klicken Sie auf der Seite **Sicherheitswarnung** auf die Schaltfläche **Untersuchung starten**. (Falls Sie den Vorgang bereits gestartet haben, ändert sich der Name in **Untersuchung fortsetzen**.)

   ![Untersuchung](./media/tutorial-security-incident/tutorial-security-incident-fig5.png)

   Der Bereich für die Untersuchung ist eine grafische Darstellung der Entitäten (in Form einer Übersichtskarte), die mit dieser Sicherheitswarnung bzw. dem Sicherheitsincident verbunden sind. Wenn Sie in diesem Bereich auf eine Entität klicken, werden in den Informationen zur Entität die neuen Entitäten angezeigt, und der Bereich wird erweitert. Für die Entität, die im Bereich ausgewählt ist, werden die Eigenschaften rechts auf der Seite hervorgehoben. Die Informationen, die auf den einzelnen Registerkarten verfügbar sind, variieren je nach der ausgewählten Entität. Überprüfen Sie während des Untersuchungsvorgangs alle relevanten Informationen, um die Bewegungen des Angreifers besser zu verstehen.

2. Fahren Sie mit dem nächsten Schritt fort, wenn Sie mehr Beweise benötigen oder Entitäten, die während der Untersuchung gefunden wurden, näher untersuchen möchten.

## <a name="search-data-for-investigation"></a>Durchsuchen von Daten zu Untersuchungszwecken

Sie können Suchfunktionen in Security Center verwenden, um weitere Beweise für kompromittierte Systeme und mehr Details zu den Entitäten zu finden, die Teil der Untersuchung sind.

Öffnen Sie zum Durchführen einer Suche das Dashboard **Security Center**, klicken Sie im linken Navigationsbereich auf **Suchen**, wählen Sie den Arbeitsbereich aus, der die zu durchsuchenden Entitäten enthält, geben Sie die Suchabfrage ein, und klicken Sie auf die Suchschaltfläche.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Andere Schnellstartanleitungen und Tutorials in dieser Sammlung bauen auf dieser Schnellstartanleitung auf. Wenn Sie planen, mit den nachfolgenden Schnellstartanleitungen und Tutorials fortzufahren, sollten Sie weiter den Tarif „Standard“ nutzen und die automatische Bereitstellung aktiviert lassen. Gehen Sie wie folgt vor, falls Sie nicht fortfahren oder zum Free-Tarif zurückkehren möchten:

1. Kehren Sie zum Hauptmenü von Security Center zurück, und wählen Sie die Option **Sicherheitsrichtlinie**.
2. Wählen Sie das Abonnement oder die Richtlinie aus, für das bzw. die Sie zu „Free“ zurückwechseln möchten. Der Bereich **Sicherheitsrichtlinie** wird geöffnet.
3. Wählen Sie unter **RICHTLINIENKOMPONENTEN** die Option **Tarif**.
4. Wählen Sie **Free**, um für das Abonnement vom Tarif „Standard“ zu „Free“ zu wechseln.
5. Wählen Sie **Speichern** aus.

Gehen Sie wie folgt vor, um die automatische Bereitstellung zu deaktivieren:

1. Kehren Sie zum Hauptmenü von Security Center zurück, und wählen Sie die Option **Sicherheitsrichtlinie**.
2. Wählen Sie das Abonnement aus, für das Sie die automatische Bereitstellung deaktivieren möchten.
3. Wählen Sie im Bereich **Sicherheitsrichtlinie – Datensammlung** unter **Onboarding** die Option **Aus**, um die automatische Bereitstellung zu deaktivieren.
4. Wählen Sie **Speichern** aus.

>[!NOTE]
> Wenn Sie die automatische Bereitstellung deaktivieren, wird Microsoft Monitoring Agent nicht von virtuellen Azure-Computern entfernt, auf denen der Agent bereitgestellt wurde. Wenn Sie die automatische Bereitstellung deaktivieren, schränkt dies die Sicherheitsüberwachung für Ihre Ressourcen ein.
>

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie Informationen zu Security Center-Features erhalten, die zum Reagieren auf einen Sicherheitsincident verwendet werden, z.B.:

> [!div class="checklist"]
> * Sicherheitsincident, bei dem es sich um eine Aggregation von verwandten Warnungen für eine Ressource handelt
> * Bereich für die Untersuchung, bei dem es sich um eine grafische Darstellung der Entitäten handelt, die mit einer Sicherheitswarnung bzw. einem Sicherheitsincident verbunden sind
> * Suchfunktionen zur Ermittlung von weiteren Beweisen für kompromittierte Systeme

Weitere Informationen zum Untersuchungsfeature von Security Center finden Sie unter:

> [!div class="nextstepaction"]
> [Untersuchen von Vorfällen und Warnungen in Azure Security Center (Vorschauversion)](security-center-investigation.md)
