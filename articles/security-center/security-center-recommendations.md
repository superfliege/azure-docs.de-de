---
title: Verwalten von Sicherheitsempfehlungen in Azure Security Center | Microsoft Docs
description: In diesem Dokument erfahren Sie, wie Sicherheitsempfehlungen in Azure Security Center Ihnen helfen, Ihre Azure-Ressourcen zu schützen und Ihre Sicherheitsrichtlinien einzuhalten.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 86c50c9f-eb6b-4d97-acb3-6d599c06133e
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/13/2018
ms.author: rkarlin
ms.openlocfilehash: a5ca8efd43244825eef412cf3c05c8df20094112
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2019
ms.locfileid: "56109538"
---
# <a name="managing-security-recommendations-in-azure-security-center"></a>Verwalten von Sicherheitsempfehlungen in Azure Security Center
In diesem Dokument erfahren Sie, wie Sie Sicherheitsempfehlungen in Azure Security Center verwenden, um Ihre Azure-Ressourcen zu schützen.

> [!NOTE]
> Der Dienst wird anhand einer Beispielbereitstellung vorgestellt.  Dieses Dokument ist keine Schritt-für-Schritt-Anleitung.
>
>

## <a name="what-are-security-recommendations"></a>Was sind Sicherheitsempfehlungen?
Security Center analysiert in regelmäßigen Abständen den Sicherheitsstatus der Azure-Ressourcen. Wenn Security Center potenzielle Sicherheitsrisiken identifiziert, werden Empfehlungen erstellt. Entsprechend den Empfehlungen werden Sie durch den Prozess der Konfiguration des erforderlichen Sicherheitsmechanismus geführt.

## <a name="implementing-security-recommendations"></a>Implementieren von Sicherheitsempfehlungen
### <a name="set-recommendations"></a>Festlegen von Empfehlungen
Unter [Festlegen von Sicherheitsrichtlinien in Azure Security Center](tutorial-security-policy.md)wird Folgendes beschrieben:

* Konfigurieren von Sicherheitsrichtlinien
* Aktivieren der Datensammlung
* Auswählen der Empfehlungen, die Sie als Teil Ihrer Sicherheitsrichtlinie verwenden möchten

Aktuelle Richtlinienempfehlungen beziehen sich auf Systemupdates, Grundregeln, Antischadsoftware, [Netzwerksicherheitsgruppen](../virtual-network/security-overview.md) für Subnetze und Netzwerkschnittstellen, Überwachung der SQL-Datenbank, Transparent Data Encryption für die SQL-Datenbank und Web Application Firewalls.  [Einrichten von Sicherheitsrichtlinien](tutorial-security-policy.md) enthält eine Beschreibung der einzelnen Empfehlungsoptionen.

### <a name="monitor-recommendations"></a>Überwachen von Empfehlungen
Nach Einstellung einer Sicherheitsrichtlinie analysiert Security Center den Sicherheitsstatus Ihrer Ressourcen, um potenzielle Sicherheitsrisiken zu erkennen. Auf der Kachel **Empfehlungen** unter **Übersicht** können Sie die Gesamtzahl der von Security Center identifizierten Empfehlungen sehen.

![Kachel „Empfehlungen“][1]

Um die Details zu jeder Empfehlung anzuzeigen, wählen Sie unter **Übersicht** die Kachel **Empfehlungen**. Das Blatt **Empfehlungen** wird geöffnet.

![Empfehlungen filtern][2]

Sie können Empfehlungen filtern. Wählen Sie auf dem Blatt **Empfehlungen** die Option **Filter**, um die Empfehlungen zu filtern. Das Blatt **Filter** wird geöffnet. Sie können Werte für Schweregrad und Status auswählen, die Sie anzeigen möchten.

Die Empfehlungen werden in einem Tabellenformat angezeigt, wobei jede Zeile eine bestimmte Empfehlung darstellt. Die Spalten dieser Tabelle sind:

* **BESCHREIBUNG**: Eine Erläuterung der Empfehlung und der erforderlichen Schritte.
* **RESSOURCE**: Eine Liste mit den Ressourcen, für die diese Empfehlung gilt.
* **STATUS**: Beschreibt den aktuellen Status der Empfehlung:
  * **Offen**: Die Empfehlung wurde noch nicht bearbeitet.
  * **In Bearbeitung**: Die Empfehlung wird derzeit auf die Ressourcen angewendet, und es ist keine Aktion Ihrerseits erforderlich.
  * **Behoben**: Die Empfehlung wurde bereits abgeschlossen (in diesem Fall ist die Zeile grau hinterlegt).
* **SCHWEREGRAD**: Beschreibt den Schweregrad der jeweiligen Empfehlung:
  * **Hoch:** Ein Sicherheitsrisiko betrifft eine wichtige Ressource (z. B. eine Anwendung, eine VM oder eine Netzwerksicherheitsgruppe) und erfordert einen Eingriff.
  * **Mittel:** Es besteht ein Sicherheitsrisiko, und es sind nicht kritische oder zusätzliche Schritte erforderlich, um es zu beseitigen oder einen Prozess abzuschließen.
  * **Niedrig**: Es besteht ein Sicherheitsrisiko, das behandelt werden sollte, aber keinen unmittelbaren Eingriff erfordert. (Standardmäßig werden Empfehlungen mit dem Status „Niedrig“ nicht angezeigt, aber Sie können bei Bedarf nach diesen Empfehlungen filtern.)

Der folgenden Tabelle können Sie entnehmen, welche Empfehlungen verfügbar sind und welche Aktionen bei ihrer Anwendung jeweils ausgeführt werden.

> [!NOTE]
> Es empfiehlt sich, sich mit dem [klassischen Bereitstellungsmodell und dem Resource Manager-Bereitstellungsmodell](../azure-classic-rm.md) für Azure-Ressourcen vertraut zu machen.
>
>
### <a name="apply-recommendations"></a>Anwenden von Empfehlungen
Nach Auswertung aller Empfehlungen entscheiden Sie, welche zuerst angewendet werden soll. Es ist ratsam, den Schweregrad als wichtigsten Parameter bei der Entscheidung heranzuziehen, welche Empfehlungen zuerst angewendet werden sollen.



## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument wurden Ihnen die Sicherheitsempfehlungen in Security Center vorgestellt. Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](tutorial-security-policy.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
* [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md) : Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center:](security-center-managing-and-responding-alerts.md) Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Überwachen von Partnerlösungen mit Azure Security Center:](security-center-partner-solutions.md) Erfahren Sie, wie der Integritätsstatus Ihrer Partnerlösungen überwacht wird.
* [Azure Security Center – Häufig gestellte Fragen:](security-center-faq.md) Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](https://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Blogbeiträge zur Azure-Sicherheit und -Compliance.

<!--Image references-->
[1]: ./media/security-center-recommendations/recommendations-tile.png
[2]: ./media/security-center-recommendations/filter-recommendations.png
