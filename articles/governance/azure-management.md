---
title: Azure-Verwaltung
description: Dieser Artikel enthält eine Übersicht über die Verwaltungsbereiche für Azure-Anwendungen und -Ressourcen sowie Links zu Inhalten zu Azure-Verwaltungstools.
author: DCtheGeek
manager: carmonm
ms.service: governance
ms.topic: article
ms.date: 09/18/2018
ms.author: dacoulte
ms.openlocfilehash: 268526277dc935ccffc30120f2bb170ad3ff3ef1
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46978221"
---
# <a name="management-in-azure"></a>Verwaltung in Azure

Governance in Azure ist ein Aspekt der Azure-Verwaltung. Dieser Artikel beschreibt kurz die verschiedenen Bereiche der Verwaltung, die zum Bereitstellen und Warten Ihrer Anwendungen und Ressourcen in Azure erforderlich sind. Zudem werden Links zur Dokumentation bereitgestellt, um Ihnen den Einstieg zu erleichtern.

„Verwaltung“ bezieht sich auf die Aufgaben und Prozesse, die zur Wartung Ihrer Geschäftsanwendungen erforderlich sind, sowie die Ressourcen, die sie unterstützen. Azure umfasst mehrere Dienste und Tools, die zusammen verwendet eine umfassende Verwaltung nicht nur für Ihre in Azure ausgeführten Anwendungen ermöglichen, sondern auch Anwendungen, die in anderen Clouds und lokal ausgeführt werden. Das Verstehen der verschiedenen verfügbaren Tools und wie sie zusammen für eine Vielzahl von Szenarien verwendet werden können ist der erste Schritt beim Entwerfen einer umfassenden Verwaltungsumgebung.

Das folgende Diagramm veranschaulicht die verschiedenen Bereiche der Verwaltung, die zum Warten einer Anwendung oder Ressource erforderlich sind. Diese unterschiedlichen Bereiche können als Bestandteile eines Lebenszyklus betrachtet werden, wobei alle Bereich fortlaufend nacheinander während der Lebensdauer einer Ressource erforderlich sind. Dies beginnt mit der anfänglichen Bereitstellung, anschließend erfolgt der fortgesetzte Betrieb und zuletzt die Außerkraftsetzung.

![Verwaltungsfunktionen](../monitoring/media/management-overview/management-capabilities.png)

Kein einzelner Azure-Dienst erfüllt die Anforderungen eines bestimmten Verwaltungsbereichs vollständig. Die Bereiche werden jeweils durch die Zusammenarbeit mehrerer Dienste abgedeckt. Einige Dienste bieten zielgerichtete Funktionen, z.B. stellt Application Insights die Überwachung für Webanwendungen bereit. Andere Dienste stellen allgemeine Funktionen bereit. Log Analytics speichert beispielsweise Verwaltungsdaten für andere Dienste, sodass Sie Daten verschiedener Typen analysieren können, die von verschiedenen Diensten gesammelt werden.

In den folgenden Abschnitten werden verschiedenen Verwaltungsbereiche kurz beschrieben und Links zu ausführlichen Inhalten zu den Azure-Hauptdiensten bereitgestellt, die für die jeweiligen Bereiche vorgesehen sind.

## <a name="monitor"></a>Überwachen

Überwachung ist das Erfassen und Analysieren von Daten, um die Leistung, Integrität und Verfügbarkeit Ihrer Geschäftsanwendung und der Ressourcen, von denen sie abhängt, zu bestimmen. Eine effektive Überwachungsstrategie erleichtert Ihnen, den detaillierten Einsatz der verschiedenen Komponenten Ihrer Anwendung zu verstehen und die Betriebszeit zu erhöhen, indem Sie proaktiv über kritische Punkte informiert werden, bevor Probleme auftreten. Unter [Überwachen von Azure-Anwendungen und -Ressourcen](../monitoring/monitoring-overview.md) finden Sie eine Übersicht über die Überwachung in Azure, die die verschiedenen Dienste identifiziert, die für eine Überwachungsstrategie verwendet werden.

## <a name="configure"></a>Konfigurieren

„Konfigurieren“ bezieht sich auf die ursprüngliche Bereitstellung und Konfiguration von Anwendungen und Ressourcen sowie deren laufende Wartung mit Patches und Updates. Die Automatisierung dieser Aufgaben mithilfe von Skripts und Richtlinien ermöglicht Ihnen das Beseitigen von Redundanz, das Minimieren des (Zeit-) Aufwands und die Steigerung der Genauigkeit und Effizienz. [Azure Automation](..\automation\automation-intro.md) stellt den Großteil der Dienste für die Automatisierung von Konfigurationsaufgaben bereit. Neben Runbooks für das Automatisieren von Prozessen stellt der Dienst auch Konfigurations- und Updateverwaltung bereit, um Sie bei der Verwaltung der Konfiguration durch Richtlinien und das Erkennen und Bereitstellen von Updates zu unterstützen.

## <a name="govern"></a>Steuern

Governance stellt Mechanismen und Prozesse zum Beibehalten der Kontrolle über Ihre Anwendungen und Ressourcen in Azure bereit. Sie umfasst die Planung von Initiativen und das Setzen von strategischen Prioritäten.
Governance in Azure wird in erster Linie mit zwei Diensten implementiert. [Azure Policy](../azure-policy/azure-policy-introduction.md) ermöglicht Ihnen das Erstellen, Zuweisen und Verwalten von Richtliniendefinition, die unterschiedliche Regeln und Aktionen für Ihre Ressourcen erzwingen, damit diese Ressourcen stets konform mit Ihren Unternehmensstandards und Vereinbarungen zum Service Level bleiben. [Azure Cost Management von Cloudyn](../cost-management/overview.md) ermöglicht Ihnen das Nachverfolgen der Cloudnutzung und der Ausgaben für Ihre Azure-Ressourcen und andere Cloudanbieter, einschließlich AWS und Google.

## <a name="secure"></a>Schützen

Das Verwalten der Sicherheit Ihrer Anwendungen, Ressourcen und Daten umfasst eine Kombination aus der Bewertung von Bedrohungen, dem Sammeln und Analysieren von Sicherheitsdaten und dem Sicherstellen, dass Ihre Anwendungen und Ressourcen auf sichere Weise entworfen und konfiguriert werden. Sicherheitsüberwachung und Bedrohungsanalyse werden vom [Azure Security Center](../security-center/security-center-intro.md) bereitgestellt. Dieses umfasst einheitliche Funktionen für die Sicherheitsverwaltung und den erweiterten Schutz vor Bedrohungen für Hybrid Cloud-Workloads. Lesen Sie auch die [Einführung in Azure Security](../security/azure-security.md), um umfassende Informationen zu Sicherheit in Azure und eine Anleitung zum sicheren Konfigurieren von Azure-Ressourcen zu erhalten.

## <a name="protect"></a>Schützen

„Schutz“ bezieht sich auf das Sicherstellen, dass Ihre Anwendungen und Daten immer verfügbar sind, auch bei Ausfällen, die sich Ihrer Kontrolle entziehen. Schutz in Azure wird durch zwei Dienste bereitgestellt. [Azure Backup](../backup/backup-introduction-to-azure-backup.md) bietet die Sicherung und Wiederherstellung Ihrer Daten entweder in der Cloud oder lokal. [Azure Site Recovery](../site-recovery/site-recovery-overview.md) gewährleistet hohe Verfügbarkeit Ihrer Anwendung durch die Bereitstellung von Geschäftskontinuität und sofortige Wiederherstellung im Notfall.

## <a name="migrate"></a>Migrieren

„Migration“ bezieht sich auf den Übergang von Workloads, die derzeit lokal ausgeführt werden, in die Azure-Cloud.
[Azure Migrate](../migrate/migrate-overview.md) ist ein Dienst für die Bewertung der Migrationseignung von lokalen virtuellen Computern zu Azure, einschließlich leistungsbasierter Größen- und Kostenschätzungen. Azure Site Recovery unterstützt Sie bei der Ausführung der tatsächlichen Migration von virtuellen Computern [aus einer lokalen Umgebung](../site-recovery/migrate-tutorial-on-premises-azure.md) oder [Amazon Web Services](../site-recovery/migrate-tutorial-aws-azure.md). [Azure Database Migration](../dms/dms-overview.md) hilft Ihnen bei der Migration mehrerer Datenbankenquellen zu Azure Data-Plattformen.