---
title: "Verwalten von Partnerlösungen in Azure Security Center | Microsoft Docs"
description: "Dieses Dokument beschreibt, wie Sie mithilfe von Azure Security Center auf einen Blick den Integritätsstatus Ihrer Partnerlösungen prüfen können, die in Ihr Azure-Abonnement integriert sind."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/14/2017
ms.author: terrylan
ms.openlocfilehash: 3ff38892f198ab5dfb9e08c1c01b942810a40260
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="monitoring-partner-solutions-with-azure-security-center"></a>Überwachen von Partnerlösungen mit Azure Security Center
In diesem Dokument erfahren Sie, wie der Integritätsstatus Ihrer Partnerlösungen in Azure Security Center überwacht wird.

> [!NOTE]
> Der Dienst wird anhand einer Beispielbereitstellung vorgestellt. Dieses Dokument ist keine Schritt-für-Schritt-Anleitung.
>
>

## <a name="monitoring-partner-solutions"></a>Überwachen von Partnerlösungen
Auf der Kachel **Sicherheitslösungen** können Sie unter **Übersicht** auf einen Blick den Integritätsstatus Ihrer Partnerlösungen prüfen, die in Ihr Azure-Abonnement integriert sind.

![Kachel „Partnerlösungen“][1]

So zeigen der Integrität Ihrer Partnerlösungen an:

1. Wählen Sie die Kachel **Sicherheitslösungen** aus. **Sicherheitslösungen** wird mit einer Liste der Partnerlösungen geöffnet, die mit Security Center verbunden sind.

   ![Partnerlösungen][3]

   Eine Partnerlösung kann den folgenden Status aufweisen:

   * Geschützt (grün): Es liegt kein Integritätsproblem vor.
   * Fehlerhaft (rot): Es liegt ein Integritätsproblem vor, das sofort untersucht werden muss.
   * Melden beendet (orange): Die Lösung hat das Melden der Integrität beendet.
   * Unbekannter Schutzstatus (orange): Der Integritätsstatus der Lösung ist derzeit unbekannt, da beim Hinzufügen einer neuen Ressource zur vorhandenen Lösung ein Fehler aufgetreten ist.
   * Nicht gemeldet (grau): Die Lösung hat noch keine Meldung berichtet. Der Status einer Lösung kann unter Umständen noch nicht gemeldet worden sein, wenn die Verbindung kürzlich hergestellt wurde und die Bereitstellung noch nicht abgeschlossen ist.

2. Wählen Sie eine Partnerlösung aus. Auf einem Blatt werden der Status der Partnerlösung und die zugeordneten Ressourcen der Lösung angezeigt. Wählen Sie die Option **Lösungskonsole** , um die Verwaltungsoberfläche des Partners für diese Lösung zu öffnen.

   ![Details der Partnerlösung][4]
3. Wechseln Sie zurück zum Blatt **Qualys**, und wählen Sie **Virtuellen Computer verknüpfen** aus. Das Blatt **Anwendungen verknüpfen** wird geöffnet. Hier können Sie Ressourcen mit der Partnerlösung verknüpfen.

   ![Verknüpfen Sie Ressourcen mit Partnerlösungen][5]

## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument wurde die Kachel **Sicherheitslösungen** in Security Center vorgestellt. Unter [Partner- und Lösungsintegration](security-center-partner-integration.md) erfahren Sie mehr zur Integration mit Partnern zum Verbessern der allgemeinen Sicherheit.

Weitere Informationen zu Security Center finden Sie in den folgenden Artikeln:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](security-center-policies.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
* [Verwalten von Sicherheitsempfehlungen in Azure Security Center:](security-center-recommendations.md) Hier erfahren Sie, wie Empfehlungen Ihnen beim Schutz der Azure-Ressourcen helfen.
* [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md) : Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md) : Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Azure Security Center – Häufig gestellte Fragen](security-center-faq.md) : Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Neuigkeiten und Informationen zur Azure-Sicherheit.

<!--Image references-->
[1]: ./media/security-center-partner-solutions/partner-solutions-tile.png
[3]: ./media/security-center-partner-solutions/partner-solutions.png
[4]: ./media/security-center-partner-solutions/partner-solutions-detail.png
[5]: ./media/security-center-partner-solutions/link-applications.png
