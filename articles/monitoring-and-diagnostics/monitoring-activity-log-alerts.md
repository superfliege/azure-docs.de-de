---
title: Aktivitätsprotokollwarnungen in Azure Monitor
description: Lassen Sie sich per SMS, Webhook und E-Mail usw. benachrichtigen, wenn bestimmte Ereignisse im Aktivitätsprotokoll auftreten.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: 5ddf510d50f38ed9aaf742bd06c330e53ffe1391
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46992795"
---
# <a name="alerts-on-activity-log"></a>Warnungen zum Aktivitätsprotokoll 

## <a name="overview"></a>Übersicht
Aktivitätsprotokollwarnungen sind Warnungen, die aktiviert werden, wenn ein neues Aktivitätsprotokollereignis auftritt, das die in der Warnung angegebenen Bedingungen erfüllt. Als Azure-Ressourcen können sie mit einer Azure Resource Manager-Vorlage erstellt werden. Sie können auch im Azure-Portal erstellt, aktualisiert oder gelöscht werden. In diesem Artikel werden die Konzepte erläutert, auf denen Aktivitätsprotokollwarnungen basieren. Dann erfahren Sie, wie Sie im Azure-Portal Warnungen zu Aktivitätsprotokollereignissen einrichten können. Weitere Informationen zu Verwendung finden Sie unter [Erstellen und Verwalten von Aktivitätsprotokollwarnungen](alert-activity-log.md).

> [!NOTE]
> Warnungen können **nicht** für Ereignisse in der Kategorie „Warnung“ des Aktivitätsprotokolls erstellt werden.

Normalerweise erstellen Sie Aktivitätsprotokollwarnungen, um Benachrichtigungen zu empfangen, wenn Folgendes geschieht:

* Spezifische Vorgänge treten bei Ressourcen in Ihrem Azure-Abonnement auf, die häufig für bestimmte Ressourcengruppen oder Ressourcen gelten. Es kann beispielsweise sein, dass Sie benachrichtigt werden möchten, wenn ein beliebiger virtueller Computer in myProductionResourceGroup gelöscht wird. Unter Umständen möchten Sie auch benachrichtigt werden, wenn einem Benutzer unter Ihrem Abonnement neue Rollen zugewiesen werden.
* Ein Dienstintegritätsereignis tritt auf. Dienstintegritätsereignisse umfassen eine Benachrichtigung bei Vorfällen und Wartungsereignissen, die für Ressourcen Ihres Abonnements gelten.

Eine einfache Analogie zum Verständnis der Bedingungen, unter denen Warnungsregeln für das Aktivitätsprotokoll erstellt werden können, ist die Untersuchung oder Filterung von Ereignissen über das [Aktivitätsprotokoll im Azure-Portal](monitoring-overview-activity-logs.md#query-the-activity-log-in-the-azure-portal). In „Azure Monitor – Aktivitätsprotokoll“ können Sie erforderliche Ereignisse filtern oder finden und dann eine Warnung mit der Schaltfläche **Aktivitätsprotokollwarnung hinzufügen** erstellen.

In beiden Fällen wird mit einer Aktivitätsprotokollwarnung nur eine Überwachung auf Ereignisse in dem Abonnement durchgeführt, unter dem die Warnung erstellt wurde.

Sie können eine Aktivitätsprotokollwarnung basierend auf einer beliebigen Eigenschaft der obersten Ebene im JSON-Objekt für ein Aktivitätsprotokollereignis konfigurieren. Weitere Informationen finden Sie unter [Kategorien im Aktivitätsprotokoll](./monitoring-overview-activity-logs.md#categories-in-the-activity-log). Weitere Informationen zu Dienstintegritätsereignissen finden Sie unter [Erstellen von Aktivitätsprotokollwarnungen zu Dienstbenachrichtigungen](./monitoring-activity-log-alerts-on-service-notifications.md). 

Aktivitätsprotokollwarnungen verfügen über einige allgemeine Optionen:

- **Kategorie**: Verwaltung, Dienstintegrität, Autoskalierung, Sicherheit, Richtlinie und Empfehlung. 
- **Bereich**: Die einzelne Ressource oder Ressourcengruppe, für die die Warnung für das Aktivitätsprotokoll definiert ist. Der Bereich für eine Aktivitätsprotokollwarnung kann auf verschiedenen Ebenen definiert werden:
    - Ressourcenebene: Z. B. für einen bestimmten virtuellen Computer.
    - Ressourcengruppenebene: Z. B. alle virtuellen Computer in einer bestimmten Ressourcengruppe.
    - Abonnementebene: Z. B. alle virtuellen Computer in einem Abonnement (oder) alle Ressourcen in einem Abonnement.
- **Ressourcengruppe**: Standardmäßig wird die Warnungsregel in derselben Ressourcengruppe gespeichert wie die des im Bereich definierten Ziels. Der Benutzer kann auch die Ressourcengruppe definieren, in der die Warnungsregel gespeichert werden soll.
- **Ressourcentyp**: Der von Resource Manager definierte Namespace für das Ziel der Warnung.

- **Vorgangsname**: Der Name des Resource Manager-Vorgangs mit rollenbasierter Zugriffssteuerung.
- **Grad**: Der Schweregrad des Ereignisses („Ausführlich“, „Information“, „Warnung“, „Fehler“ oder „Kritisch“).
- **Status**: Der Status des Ereignisses, z.B. „Gestartet“, „Fehler“ oder „Erfolgreich“.
- **Ereignis initiiert von**: Wird auch als „Aufrufer“ bezeichnet. Die E-Mail-Adresse oder der Azure Active Directory-Bezeichner des Benutzers, der den Vorgang durchgeführt hat.

> [!NOTE]
> In einem Abonnement bis zu 100 Warnungsregeln für die Aktivität des Bereichs bei entweder: einer einzelnen Ressource, allen Ressourcen in der Ressourcengruppe (oder) der gesamten Abonnementebene.

Bei Aktivierung einer Aktivitätsprotokollwarnung wird eine Aktionsgruppe verwendet, um Aktionen oder Benachrichtigungen zu generieren. Eine Aktionsgruppe ist ein wiederverwendbarer Satz von Benachrichtigungsempfängern, z.B. E-Mail-Adressen, Webhook-URLs oder SMS-Telefonnummern. Mehrere Warnungen können auf die Empfänger verweisen, um Ihre Benachrichtigungskanäle zu zentralisieren und gruppieren. Wenn Sie Ihre Aktivitätsprotokollwarnung definieren, stehen Ihnen zwei Optionen zur Verfügung. Ihre Möglichkeiten:

* Verwenden Sie eine vorhandene Aktionsgruppe in Ihrer Aktivitätsprotokollwarnung.
* Erstellen Sie eine neue Aktionsgruppe.

Weitere Informationen zu Aktionsgruppen finden Sie unter [Erstellen und Verwalten von Aktionsgruppen im Azure-Portal](monitoring-action-groups.md).


## <a name="next-steps"></a>Nächste Schritte
- Lesen Sie eine [Übersicht über Warnungen](monitoring-overview-alerts.md).
- Erfahren Sie mehr über das [Erstellen und Ändern von Aktivitätsprotokollwarnungen](alert-activity-log.md).
- Weitere Informationen zum [Webhookschema für Aktivitätsprotokollwarnungen](monitoring-activity-log-alerts-webhook.md).
- Weitere Informationen zu [Dienstintegritätsbenachrichtigungen](monitoring-service-notifications.md).

