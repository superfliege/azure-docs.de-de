---
title: Azure Security Center und virtuellen Azure-Computern mit Linux | Microsoft Docs
description: "Dieses Dokument hilft Ihnen beim Verständnis, wie Azure Security Center Sie virtuelle Computer in Azure schützen können."
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 5fe5a12c-5d25-430c-9d47-df9438b1d7c5
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/03/2017
ms.author: yurid
ms.openlocfilehash: 0df4fca59575bd8e18e91fea2066a9e694ed320d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-security-center-and-azure-virtual-machines-with-linux"></a>Azure Security Center und virtuellen Azure-Computern mit Linux
[Azure Security Center](https://azure.microsoft.com/services/security-center/) können Sie verhindern, erkennen und auf Bedrohungen reagiert. Sie stellt integrierte Sicherheit von Überwachung und Richtlinien-Management über Ihr Azure-Abonnements, hilft bei der Ermittlung Bedrohungen, die andernfalls unbemerkt möglicherweise und arbeitet mit einer umfassenden sicherheitslösungen-Ökosystem.

Dieser Artikel zeigt, wie Sicherheitscenter Sichern Ihrer Azure virtuellen Computern (VMs) mit Linux-Betriebssystem helfen können.

## <a name="why-use-security-center"></a>Gründe für die Verwendung von Sicherheitscenter
Security Center können Sie die Daten des virtuellen Computers in Azure zu schützen, indem Sie Einblick in die Sicherheitseinstellungen Ihres virtuellen Computers bereitstellen und Überwachen der Gefahren enthalten. Sicherheitscenter können Ihre virtuellen Computer für überwachen: 

* Betriebssystem (BS) Sicherheitseinstellungen für den Regeln für die empfohlene Konfiguration
* Sicherheit und wichtige Updates, die fehlen
* EndPoint Protection-Empfehlungen
* Überprüfung der Datenträger-Verschlüsselung
* Browserbasierte Angriffe Netzwerk (nur verfügbar in [Standardversion](https://azure.microsoft.com/en-us/pricing/details/security-center/))

Zusätzlich zum Schutz Ihrer Azure-VMs bietet Sicherheitscenter auch sicherheitsüberwachung und Verwaltung für Cloud-Dienste, App-Dienste und virtuelle Netzwerke. 

> [!NOTE]
> Finden Sie unter [Einführung in Azure Security Center](security-center-intro.md) Weitere Informationen zu Azure Security Center.
> 
> 

## <a name="prerequisites"></a>Voraussetzungen
Um die ersten Schritte mit Azure Security Center müssen Sie kennen und berücksichtigen Folgendes:

* Sie müssen ein Microsoft Azure-Abonnement verfügen. Finden Sie unter [Security Center Preise](https://azure.microsoft.com/pricing/details/security-center/) für Weitere Informationen zum Sicherheitscenter der freien und standard-Ebene.
* Planen Sie Ihre Security Center Akzeptanz, finden Sie unter [Azure Security Center-Planung und Operations Guide](security-center-planning-and-operations-guide.md) Weitere Informationen zu Überlegungen zur Planung und Vorgänge.
* Informationen zum Betriebssystem unterstützbarkeit, finden Sie unter [Azure Security Center-häufig gestellte Fragen (FAQ)](security-center-faq.md). 

## <a name="set-security-policy"></a>Set-Sicherheitsrichtlinie
Die Datensammlung muss aktiviert sein, damit diese Azure Security Center, die Informationen, die sie bereitstellen sammeln können, Empfehlungen und Warnungen, die generiert werden muss anhand der Sicherheitsrichtlinie, die Sie konfigurieren. In der folgenden Abbildung können Sie sehen, die **Datensammlung** wurde **auf**.

Eine Sicherheitsrichtlinie definiert den Satz von Steuerelementen, die für Ressourcen innerhalb der angegebenen Abonnement oder Ressourcengruppe empfohlen werden. Vor der Aktivierung der Sicherheitsrichtlinie, benötigen Sie die Datensammlung aktiviert, Security Center erfasst Daten über Ihre virtuellen Computer, um ihren Sicherheitszustand bewerten, Empfehlungen zur Sicherheit bieten und Risiken benachrichtigt. Im Sicherheitscenter definieren Sie die Richtlinien für Ihr Azure-Abonnements oder Ressourcengruppen entsprechend den sicherheitsanforderungen Ihres Unternehmens und Anwendungstyp oder Vertraulichkeit der Daten in jedem Abonnement. 

![Sicherheitsrichtlinie](./media/security-center-linux-virtual-machine/security-center-linux-virtual-machine-fig1.png)

> [!NOTE]
> Weitere Informationen zu jeder **Richtlinie zum Verhindern von Datenverlusten** verfügbar ist, finden Sie unter [Sicherheitsrichtlinien festlegen](security-center-policies.md) Artikel.
> 

## <a name="manage-security-recommendations"></a>Verwalten von Empfehlungen zur Sicherheit
Sicherheitscenter analysiert den Sicherheitszustand des Azure-Ressourcen. Wenn Sicherheitscenter potenziellen Sicherheitslücken identifiziert wird, werden Empfehlungen erstellt. Die Empfehlungen, die Sie durch den Konfigurationsprozess für die erforderlichen Steuerelemente geführt.

Nach dem Festlegen einer Sicherheitsrichtlinie, analysiert Sicherheitscenter den Sicherheitszustand Ihrer Ressourcen, um potenzielle Sicherheitslücken zu identifizieren. Die Empfehlungen werden in einem Tabellenformat angezeigt, wobei jede Zeile eine bestimmte Empfehlung darstellt. In der folgenden Tabelle enthält einige Beispiele für Empfehlungen für virtuelle Azure-Computer mit Linux-Betriebssystem und was wird jeweils tun, wenn Sie sie anwenden. Wenn Sie eine Empfehlung auswählen, werden Informationen bereitgestellt, die zur Umsetzung der Empfehlung im Sicherheitscenter zeigt.

| Empfehlung | Beschreibung |
| --- | --- |
| [Aktivieren Sie die Datensammlung für Abonnements](security-center-enable-data-collection.md) |Es wird empfohlen, dass Sie die Datensammlung in der Sicherheitsrichtlinie für jede Ihrer Abonnements und alle virtuellen Computer (VMs) in Ihre Abonnements zu aktivieren. |
| [Beheben von Sicherheitsrisiken OS](security-center-remediate-os-vulnerabilities.md) |Empfiehlt, dass Sie Ihr Betriebssystem Konfigurationen mit den empfohlenen Konfigurationsregeln ausrichten, z. B. zulassen nicht Kennwörter gespeichert werden sollen. |
| [Systemupdates anwenden](security-center-apply-system-updates.md) |Empfiehlt, dass Sie fehlenden Sicherheit des Systems und wichtige Updates für virtuelle Computer bereitstellen. |
| [Neu starten, nachdem Systemupdates](security-center-apply-system-updates.md#reboot-after-system-updates) |Empfiehlt, dass Sie einen virtuellen Computer zum Abschließen des Vorgangs des Anwendens von Systemupdates neu starten. |
| [Aktivieren Sie die VM-Agent](security-center-enable-vm-agent.md) |Ermöglicht es Ihnen, finden Sie unter dem virtuelle Computer erfordern den VM-Agent. Der VM-Agent muss auf virtuellen Computern installiert werden, um Patches Scannen von Scannen Baseline und Antischadsoftware bereitstellen. Der VM-Agent wird für virtuelle Computer, die über Azure Marketplace bereitgestellt werden, standardmäßig installiert. Der Artikel [VM-Agent und Erweiterungen – Teil 2](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) enthält Informationen zum den VM-Agent zu installieren. |
| [Datenträgerverschlüsselung anwenden](security-center-apply-disk-encryption.md) |Empfiehlt, dass Sie die VM-Datenträgern, die mit Azure-Datenträger-Verschlüsselung (Windows- und Linux-VMs) verschlüsseln. Verschlüsselung wird für das Betriebssystem und die Daten-Volumes auf Ihrem virtuellen Computer empfohlen. |


> [!NOTE]
> Weitere Informationen zu Empfehlungen finden Sie unter [verwalten sicherheitsempfehlungen](security-center-recommendations.md) Artikel.
> 

## <a name="monitor-security-health"></a>Monitor-Sicherheitsintegrität
Nach der Aktivierung [Sicherheitsrichtlinien](security-center-policies.md) für ein Abonnement Ressourcen Sicherheitscenter analysiert die Sicherheit Ihrer Ressourcen, um potenzielle Sicherheitslücken zu identifizieren.  Sehen Sie den Sicherheitszustand Ihrer Ressourcen, sowie alle Probleme in der **Sicherheit ressourcenintegrität** Blatt. Beim Klicken auf **virtuelle Maschinen** in der **ressourcensicherheit** Kachel "Integrität" die **virtuelle Maschinen** Blatt mit den Empfehlungen für Ihre virtuellen Computer geöffnet wird. 

![Sicherheitsstatus](./media/security-center-virtual-machine/security-center-virtual-machine-fig2.png)

## <a name="manage-and-respond-to-security-alerts"></a>Verwalten von und reagieren auf Sicherheitswarnungen
Sicherheitscenter automatisch erfasst, analysiert und Integration von Protokolldaten aus Ihrer Azure-Ressourcen, die Netzwerk- und verbundene partnerlösungen (wie z. B. Firewall und Endpoint Protection Lösungen), um echte Bedrohungen erkennen und zu reduzieren falsch positive Ergebnisse. Durch die Nutzung verschiedenen Aggregations [Erkennungsfunktionalität](security-center-detection-capabilities.md), Security Center kann generieren priorisierte Sicherheitswarnungen, damit Sie schnell das Problem zu untersuchen und Empfehlungen zum Beheben von möglicher Angriffen bieten.

![Sicherheitswarnungen](./media/security-center-virtual-machine/security-center-virtual-machine-fig3.png)

Wählen Sie eine sicherheitswarnung an, um weitere Informationen zum die Ereignisse, die die Warnung und welche ausgelöst. wenn alle Schritte Sie einen Angriff nicht wiederhergestellt werden müssen. Sicherheitswarnungen werden gruppiert nach [Typ](security-center-alerts-type.md) und Datum.

## <a name="monitor-security-health"></a>Monitor-Sicherheitsintegrität
Nach der Aktivierung [Sicherheitsrichtlinien](security-center-policies.md) für ein Abonnement Ressourcen Sicherheitscenter analysiert die Sicherheit Ihrer Ressourcen, um potenzielle Sicherheitslücken zu identifizieren.  Sehen Sie den Sicherheitszustand Ihrer Ressourcen, sowie alle Probleme in der **Sicherheit ressourcenintegrität** Blatt. Beim Klicken auf **virtuelle Maschinen** in der **ressourcensicherheit** Kachel "Integrität" die **virtuelle Maschinen** Blatt mit den Empfehlungen für Ihre virtuellen Computer geöffnet wird. 

![Sicherheitsstatus](./media/security-center-linux-virtual-machine/security-center-linux-virtual-machine-fig4.png)

Wenn Sie auf diese Empfehlung klicken, sehen Sie weitere Informationen zu bestimmten Aktionen, die ausgeführt werden soll, um diese Probleme zu beheben. Die Details werden im unteren Rand der Blatt unter **Empfehlungen**. 

![Sicherheitsstatus 2](./media/security-center-linux-virtual-machine/security-center-linux-virtual-machine-fig5.png)


## <a name="see-also"></a>Weitere Informationen:
Weitere Informationen zum Security Center finden Sie in der folgenden:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center](security-center-policies.md) – Informationen zum Konfigurieren der Sicherheitsrichtlinien für Ihre Azure-Abonnements und Ressourcengruppen.
* [Verwalten von und reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md) – Informationen zum Verwalten von und reagieren auf Sicherheitswarnungen.
* [Azure Security Center – häufig gestellte Fragen](security-center-faq.md) – häufig gestellte Fragen zur Verwendung des Diensts suchen.

