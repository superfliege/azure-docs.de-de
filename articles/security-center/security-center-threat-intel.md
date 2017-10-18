---
title: Informationen zu Bedrohungen in Azure Security Center | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie anhand von Informationen zu Bedrohungen in Azure Security Center potenzielle Bedrohungen auf Ihren virtuellen und physischen Computern identifizieren.
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: a771a3a1-2925-46ca-8e27-6f6a0746f58b
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/12/2017
ms.author: yurid
ms.openlocfilehash: b5bdeb1c70893c2477413df02c7b1a4a07999e93
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="threat-intelligence-in-azure-security-center"></a>Informationen zu Bedrohungen in Azure Security Center
In diesem Artikel erfahren Sie, wie Sie anhand von Informationen zu Bedrohungen in Azure Security Center sicherheitsbezogene Probleme lösen.

## <a name="what-is-threat-intelligence"></a>Was sind Informationen zu Bedrohungen?
Mithilfe der Option „Informationen zu Bedrohungen“ in Security Center können IT-Administratoren Sicherheitsrisiken für die Umgebung identifizieren. So können sie beispielsweise ermitteln, ob ein bestimmter Computer Teil eines Botnets ist. Computer können zu Knoten in einem Botnet werden, wenn Angreifer illegal Schadsoftware installieren, die diesen Computer heimlich mit einem Steuerknoten verbindet. Die Informationen zu Bedrohungen ermöglichen auch die Erkennung potenzieller Bedrohungen aus Kommunikationskanälen wie dem Darknet.

Für den Aufbau dieser Informationen zu Bedrohungen werden in Security Center Daten aus mehreren Quellen von Microsoft herangezogen. Security Center nutzt diese Daten zur Erkennung potenzieller Bedrohungen für Ihre Umgebung. Der Bereich **Informationen zu Bedrohungen** umfasst drei wichtige Optionen:

- Arten der erkannten Bedrohungen
- Ursprung der Bedrohung
- Threat Intelligence-Karte


## <a name="when-should-you-use-threat-intelligence"></a>Verwenden von Informationen zu Bedrohungen
Einer der Schritte bei der [Reaktion auf Sicherheitsvorfälle](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide#incident-response) ist die Ermittlung des Schweregrads für die betroffenen Systeme. In dieser Phase sollten Sie die folgenden Aufgaben durchführen:

- Bestimmen der Art des Angriffs.
- Bestimmen des Angriffsursprungs.
- Bestimmen der Absicht des Angriffs. Wurde Ihre Organisation gezielt angegriffen, um bestimmte Informationen zu erlangen, oder war es ein ungezielter Angriff?
- Ermitteln der betroffenen Systeme.
- Ermitteln der Dateien, auf die zugegriffen wurde, und Bestimmen der Vertraulichkeit dieser Dateien.

Bei diesen Aufgaben können Sie auf die Informationen zu Bedrohungen in Security Center zurückgreifen. 

## <a name="access-the-threat-intelligence"></a>Zugreifen auf die Informationen zu Bedrohungen
Wählen Sie zunächst den Arbeitsbereich aus, in dem sich Ihre Informationen befinden, um die aktuellen Informationen zu Bedrohungen für Ihre Umgebung zu visualisieren. Wenn Sie nicht über mehrere Arbeitsbereiche verfügen, wird die Arbeitsbereichsauswahl umgangen, und Sie gelangen direkt zum Dashboard **Informationen zu Bedrohungen**. So greifen Sie auf das Dashboard zu:

1. Öffnen Sie das Dashboard **Security Center**.

2. Klicken Sie im linken Bereich unter **Erkennung** auf **Informationen zu Bedrohungen**. Das Dashboard **Informationen zu Bedrohungen** wird angezeigt.

    ![Dashboard „Informationen zu Bedrohungen“](./media/security-center-threat-intel/security-center-threat-intel-fig1.png)

    > [!NOTE]
    > Wenn in der Spalte ganz rechts **UPGRADE PLAN** (PLAN UPGRADEN) angezeigt wird, wird für diesen Arbeitsbereich das kostenlose Abonnement verwendet. Führen Sie ein Upgrade auf „Standard“ durch, um das Feature verwenden zu können. Wenn in der Spalte ganz rechts **REQUIRES UPDATE** (UPDATE ERFORDERLICH) angezeigt wird, aktualisieren Sie [Azure Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview), um das Feature verwenden zu können. Weitere Informationen zum Tarif finden Sie unter „Azure Security Center-Preise“. 
    > 
3. Falls Sie mehrere Arbeitsbereiche untersuchen müssen, priorisieren Sie die Untersuchung anhand der Spalte **Schädliche IP**. Sie enthält die aktuelle Anzahl schädlicher IP-Adressen in diesem Arbeitsbereich. Wählen Sie den gewünschten Arbeitsbereich aus. Das Dashboard **Informationen zu Bedrohungen** wird angezeigt.

    ![Informationen zu Bedrohungen](./media/security-center-threat-intel/security-center-threat-intel-fig5.png)

4. Das Dashboard ist in vier Kacheln unterteilt:

    a.  **Bedrohungstypen**. Hier sind die Arten von Bedrohungen zusammengefasst, die im ausgewählten Arbeitsbereich erkannt wurden.

    b.  **Ursprungsland**. Aggregiert die Datenverkehrsmenge auf der Grundlage der Quelle.

    c.  **Bedrohungsposition**. Hilft bei der Identifizierung der aktuellen Orte auf der ganzen Welt, die mit Ihrer Umgebung kommunizieren. Die Richtung des Datenverkehrs wird auf der angezeigten Karte durch farbige Pfeile (orange für eingehenden, rot für ausgehenden Datenverkehr) dargestellt. Wenn Sie auf einen der Pfeile klicken, werden die Art der Bedrohung und die Richtung des Datenverkehrs angezeigt.

    d.  **Bedrohungsdetails**. Hier werden weitere Details zu der Bedrohung angezeigt, die Sie auf der Karte ausgewählt haben.

Das angezeigte Dashboard basiert unabhängig von der ausgewählten Optionskachel auf der Protokollsuchabfrage. Der einzige Unterschied sind die Art der Abfrage und das Ergebnis.

### <a name="threat-types"></a>Bedrohungstypen
Klicken Sie auf die Kachel **Bedrohungstypen**, um das Dashboard **Protokollsuche** zu öffnen. Filteroptionen werden auf der linken, Abfrageergebnisse auf der rechten Seite angezeigt.

![Protokollsuche](./media/security-center-threat-intel/security-center-threat-intel-fig3.png)

Das Abfrageergebnis zeigt die Bedrohungen nach Name an. Im linken Bereich können Sie das Attribut auswählen, nach dem Sie filtern möchten. Wenn Sie also beispielsweise nur Bedrohungen anzeigen möchten, die momentan mit den Computern verbunden sind, klicken Sie unter **SESSIONSTATE** auf **Verbunden** > **Übernehmen**.

![Sitzungszustand](./media/security-center-threat-intel/security-center-threat-intel-fig4.png)

Für virtuelle Azure-Computer werden im Dashboard **Informationen zu Bedrohungen** nur die Netzwerkdaten angezeigt, die den Agent durchlaufen. Für die Informationen zu Bedrohungen werden auch folgende Datentypen verwendet:

- CEF-Data (Type=CommonSecurityLog)
- WireData (Type=WireData)
- IIS-Protokolle (Type=W3CIISLog)
- Windows-Firewall (Type=WindowsFirewall)
- DNS-Ereignisse (Type=DnsEvents)


## <a name="see-also"></a>Weitere Informationen
In diesem Artikel wurde beschrieben, wie Sie Informationen zu Bedrohungen in Security Center beim Identifizieren von verdächtigen Aktivitäten nutzen. Weitere Informationen zu Security Center finden Sie in den folgenden Artikeln:

* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Hier erfahren Sie, wie Sie Warnungen verwalten und auf Sicherheitsvorfälle in Security Center reagieren.
* [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md). Hier erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verstehen der Sicherheitswarnungen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Hier finden Sie Informationen zu den unterschiedlichen Arten von Sicherheitswarnungen.
* [Azure Security Center – Handbuch zur Problembehandlung](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Hier erfahren Sie, wie Sie allgemeine Probleme in Security Center behandeln. 
* [Azure Security Center – häufig gestellte Fragen](security-center-faq.md)festgelegt ist. Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security-Blog](http://blogs.msdn.com/b/azuresecurity/). Hier finden Sie Blogbeiträge zur Sicherheit und Compliance von Azure.

