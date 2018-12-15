---
title: Verwalten von verbundenen Partnerlösungen in Azure Security Center | Microsoft-Dokumentation
description: Dieses Dokument beschreibt, wie Sie mithilfe von Azure Security Center auf einen Blick den Integritätsstatus Ihrer Partnerlösungen prüfen können, die in Ihr Azure-Abonnement integriert sind.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/20/2018
ms.author: rkarlin
ms.openlocfilehash: 27a8abe0008c0b9c3854ea663e1c0fb3b55cfc30
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2018
ms.locfileid: "52964542"
---
# <a name="managing-connected-partner-solutions-with-azure-security-center"></a>Verwalten von verbundenen Partnerlösungen in Azure Security Center
Dieser Artikel führt Sie durch die Schritte zum Verwalten und Überwachen von verbundenen Sicherheitslösungen in Azure Security Center.

## <a name="monitoring-partner-solutions"></a>Überwachen von Partnerlösungen
So überwachen Sie den Integritätsstatus von verbundenen Sicherheitslösungen und führen die grundlegende Verwaltung durch:

1. Wählen Sie unter **Security Center – Übersicht** die Option **Sicherheitslösungen**.

  ![Auswählen von „Sicherheitslösungen“][1]

  Der Abschnitt **Verbunde Lösungen** enthält Sicherheitslösungen, die mit Security Center verbunden sind, sowie Informationen zum Integritätsstatus der einzelnen Lösungen.

  ![Partnerlösungen][2]

   Eine Partnerlösung kann den folgenden Status aufweisen:

   * Fehlerfrei (grün): Es liegt kein Integritätsproblem vor.
   * Fehlerhaft (rot): Es liegt ein Integritätsproblem vor, das sofort untersucht werden muss.
   * Integritätsprobleme (orange): Die Lösung hat das Melden der Integrität beendet.
   * Nicht gemeldet (grau): Die Lösung hat noch nichts berichtet. Der Status einer Lösung kann unter Umständen noch nicht gemeldet worden sein, wenn die Verbindung kürzlich hergestellt wurde und die Bereitstellung noch nicht abgeschlossen ist, oder es sind keine Integritätsdaten verfügbar.

   > [!NOTE]
   > Wenn keine Integritätsstatusdaten verfügbar sind, zeigt Security Center das Datum und die Uhrzeit des letzten empfangenen Ereignisses an, um anzugeben, ob die Lösung Berichte übermittelt. Wenn keine Integritätsdaten verfügbar sind und innerhalb der letzten 14 Tage keine Warnungen empfangen wurden, gibt Security Center an, dass die Lösung fehlerhaft ist oder nicht berichtet.
   >
   >

2. Wählen Sie **ANSICHT**, um zusätzliche Informationen und Optionen anzuzeigen, darunter:

  - **Lösungskonsole**. Öffnet die Verwaltungsoberfläche für diese Lösung.
  - **Virtuellen Computer verknüpfen**. Öffnet das Blatt „Anwendungen verknüpfen“. Hier können Sie Ressourcen mit der Partnerlösung verknüpfen.
  - **Lösung löschen**.
  - **Konfigurieren**.

   ![Details der Partnerlösung][3]

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie gelernt, verbundene Sicherheitslösungen in Security Center zu verwalten und zu überwachen. Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

* [Integrieren von Sicherheitslösungen in Azure Security Center](security-center-partner-integration.md) – erfahren Sie, wie Sie eine Verbindung mit Sicherheitslösungen herstellen und sie verwalten.
* [Verbinden von Microsoft Advanced Threat Analytics mit Azure Security Center](security-center-ata-integration.md) – erfahren Sie, wie Sie eine Verbindung mit Warnungen von ATA herstellen.
* [Verbinden von Azure Active Directory Identity Protection mit Azure Security Center](security-center-aadip-integration.md) – erfahren Sie, wie Sie eine Verbindung mit Warnungen von Azure AD Identity Protection herstellen.
* [Integrieren von Sicherheitslösungen in Azure Security Center](security-center-partner-integration.md) – verschaffen Sie sich einen Überblick über die Integration anderer Sicherheitslösungen.
* [Verwalten von und Reagieren auf Sicherheitswarnungen:](security-center-managing-and-responding-alerts.md) Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Azure Security Center – Häufig gestellte Fragen:](security-center-faq.md) Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](https://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Blogbeiträge zur Azure-Sicherheit und -Compliance.

<!--Image references-->
[1]: ./media/security-center-partner-solutions/partner-solutions-tile.png
[2]: ./media/security-center-partner-solutions/partner-solutions.png
[3]: ./media/security-center-partner-solutions/partner-solutions-detail.png
