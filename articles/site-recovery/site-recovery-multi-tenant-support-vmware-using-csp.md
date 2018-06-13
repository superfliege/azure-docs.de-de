---
title: Unterstützung mehrerer Mandanten für die VMware-VM-Replikation in Azure (CSP-Programm) | Microsoft-Dokumentation
description: Beschreibt, wie Azure Site Recovery in einer Umgebung mit mehreren Mandanten bereitgestellt wird, um Replikation, Failover und Wiederherstellung von lokalen VMware-VMs in Azure über das CSP-Programm im Azure-Portal zu orchestrieren.
services: site-recovery
documentationcenter: ''
author: mayanknayar
manager: rochakm
editor: ''
ms.assetid: ''
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2018
ms.author: manayar
ms.openlocfilehash: 532dd399d2d5fcbab616744dd02f4a95078cbb1b
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2018
ms.locfileid: "29767615"
---
# <a name="multi-tenant-support-in-azure-site-recovery-for-replicating-vmware-virtual-machines-to-azure-through-csp"></a>Unterstützung mehrerer Mandanten in Azure Site Recovery für die Replikation von VMware-VMs in Azure über das CSP-Programm

Azure Site Recovery unterstützt für Mandantenabonnements Umgebungen mit mehreren Mandanten. Unterstützt wird außerdem die Mehrmandantenfähigkeit für Mandantenabonnements, die über das Microsoft Cloud Solution Provider-Programm (CSP) erstellt und verwaltet werden. In diesem Artikel wird das Implementieren und Verwalten von VMware-zu-Azure-Szenarien mit mehreren Mandanten detailliert behandelt. Weitere Informationen zum Erstellen und Verwalten von Mandantenabonnements finden Sie unter [Verwalten der Mehrinstanzenfähigkeit mit dem CSP-Programm (Cloud Solution Provider)](site-recovery-manage-multi-tenancy-with-csp.md).

Diese Anleitung bezieht sich umfassend auf die vorhandene Dokumentation zum Replizieren von VMware-VMs in Azure. Weitere Informationen finden Sie unter [Replizieren von virtuellen VMware-Computern in Azure mithilfe von Site Recovery](site-recovery-vmware-to-azure.md).

## <a name="multi-tenant-environments"></a>Umgebungen mit mehreren Mandanten
Es gibt drei wesentliche Modelle mit mehreren Mandanten:

* **Shared Hosting Services Provider (SHSP, Anbieter von gemeinsam genutzten Hostingdiensten)**: Der Partner besitzt die physische Infrastruktur und arbeitet mit gemeinsam genutzten Ressourcen (vCenter, Datencentern, physischem Speicher usw.), um die virtuellen Computer mehrerer Mandanten in derselben Infrastruktur zu hosten. Der Partner kann die Verwaltung der Notfallwiederherstellung als verwalteten Dienst bereitstellen, oder der Mandant kann die Notfallwiederherstellung als Self-Service-Lösung betreiben.

* **Dedicated Hosting Services Provider (DHSP, Anbieter dedizierter Hostingdienste)**: Der Partner besitzt die physische Infrastruktur, verwendet aber dedizierte Ressourcen (mehrere vCenter-Server, physische Datenspeicher usw.), um die virtuellen Computer der einzelnen Mandanten jeweils in einer separaten Infrastruktur zu hosten. Der Partner kann die Verwaltung der Notfallwiederherstellung als verwalteten Dienst bereitstellen, oder der Mandant kann diese als Self-Service-Lösung betreiben.

* **Managed Services Provider (MSP, Anbieter verwalteter Dienste)**: Der Kunde besitzt die physische Infrastruktur, in der die VMs gehostet werden, während der Partner für die Aktivierung und Verwaltung der Notfallwiederherstellung zuständig ist.

## <a name="shared-hosting-multi-tenant-guidance"></a>Anleitung für SHSP mit mehreren Mandanten
In dieser Abschnitt wird das SHSP-Szenario ausführlich behandelt. Die anderen beiden Szenarien sind eine Teilmenge des SHSP-Szenarios und folgen denselben Prinzipien. Die Unterschiede werden am Ende der SHSP-Anleitung beschrieben.

Die grundlegende Anforderung in einem Szenario mit mehreren Mandanten ist deren Isolation. Ein Mandant darf nicht wahrnehmen, was ein anderer Mandant hostet. In einer von einem Partner verwalteten Umgebung ist diese Anforderung nicht so wichtig wie für eine Self-Service-Umgebung, in der sie kritisch sein kann. In dieser Anleitung wird davon ausgegangen, dass die Mandantenisolation erforderlich ist.

Die Architektur ist im folgenden Diagramm dargestellt:

![SHSP mit einem vCenter](./media/site-recovery-multi-tenant-support-vmware-using-csp/shared-hosting-scenario.png)  
**SHSP-Szenario mit einem vCenter**

Wie Sie im vorigen Diagramm sehen können, hat jeder Kunde einen eigenen Verwaltungsserver. Diese Konfiguration beschränkt den Mandantenzugriff auf mandantenspezifische VMs und ermöglicht die Isolation von Mandanten. In einem VMware-VM-Replikationsszenario wird der Konfigurationsserver zum Verwalten von Konten verwendet, um VMs zu ermitteln und Agents zu installieren. Die gleichen Prinzipien gelten für Umgebungen mit mehreren Mandanten, wobei die VM-Ermittlung durch die vCenter-Zugriffssteuerung zusätzlich eingeschränkt wird.

Aufgrund der Datenisolationsanforderung dürfen gegenüber den Mandanten keine sensiblen Infrastrukturinformationen (wie etwa Anmeldeinformationen für den Zugriff) offengelegt werden. Aus diesem Grund wird empfohlen, dass alle Komponenten des Verwaltungsservers unter der alleinigen Kontrolle des Partners verbleiben. Der Verwaltungsserver hat die folgenden Komponenten:
* Konfigurationsserver
* Prozessserver
* Masterzielserver

Ein horizontal skalierter Prozessserver ist auch unter Kontrolle des Partners.

### <a name="every-cs-in-the-multi-tenant-scenario-uses-two-accounts"></a>Jeder Konfigurationsserver im Szenario mit mehreren Mandanten verwendet zwei Konten:

- **vCenter-Zugriffskonto**: Dieses Konto dient zum Ermitteln von Mandanten-VMs. Ihm sind vCenter-Zugriffsberechtigungen zugewiesen (wie im nächsten Abschnitt beschrieben). Um versehentliche Zugriffslecks zu vermeiden, wird empfohlen, dass Partner diese Anmeldeinformationen selbst in das Konfigurationstool eingeben.

- **VM-Zugriffskonto**: Dieses Konto dient zum Installieren des Mobilitäts-Agents auf den VMs des Mandanten per automatischem Push. Hierbei handelt es sich in der Regel um ein Domänenkonto, das ein Mandant ggf. für einen Partner bereitstellt oder das der Partner direkt verwaltet. Wenn ein Mandant die Details nicht direkt an den Partner weitergeben möchte, kann die Eingabe der Anmeldeinformationen im Rahmen eines zeitlich begrenzten Zugriffs auf den Konfigurationsserver ermöglicht werden. Alternativ kann der Mandant die Mobilitäts-Agents in Zusammenarbeit mit dem Partner manuell installieren.

### <a name="requirements-for-a-vcenter-access-account"></a>Anforderungen für das vCenter-Zugriffskonto

Wie im vorherigen Abschnitt erwähnt, müssen Sie den Konfigurationsserver mit einem Konto installieren, dem eine besondere Rolle zugewiesen ist. Diese Rollenzuweisung muss für das vCenter-Zugriffskonto für jedes vCenter-Objekt erfolgen und darf nicht an die untergeordneten Objekte weitergegeben werden. Diese Konfiguration stellt die Mandantenisolation sicher, da die Weitergabe von Zugangsdaten zu einem versehentlichen Zugriff auf andere Objekte führen kann.

![Die Option „An untergeordnete Objekte weitergeben“](./media/site-recovery-multi-tenant-support-vmware-using-csp/assign-permissions-without-propagation.png)

Die Alternative besteht darin, das Benutzerkonto und die Rolle dem Rechenzentrumsobjekt zuzuweisen und diese an die untergeordneten Objekte weiterzugeben. Weisen Sie anschließend dem Konto die Rolle *Kein Zugriff* für alle Objekte zu (z.B. VMs anderer Mandanten), auf die ein bestimmter Mandant keinen Zugriff haben soll. Diese Konfiguration ist mühsam und ermöglicht die versehentliche Preisgabe von Zugangsdaten, da jedem neuen untergeordneten Objekt auch automatisch die vom übergeordneten Objekt vererbten Zugangsrechte erteilt werden. Aus diesem Grund wird empfohlen, die erste Methode zu verwenden.

Das Zugriffsverfahren des vCenter-Kontos ist wie folgt:

1. Erstellen Sie eine neue Rolle durch Klonen der vordefinierten Rolle *Schreibgeschützt*, und geben Sie ihr einen passenden Namen (z.B. „Azure_Site_Recovery“ wie in diesem Beispiel).

2. Weisen Sie dieser Rolle die folgenden Berechtigungen zu:

    * **Datenspeicher**: Speicherplatz zuordnen, Datenspeicher durchsuchen, Low-Level-Dateivorgänge, Datei entfernen, Dateien virtueller Computer aktualisieren

    * **Netzwerk**: Netzwerk zuweisen

    * **Ressource** Zuweisen der VM zu einem Ressourcenpool, ausgeschaltete VM migrieren, eingeschaltete VM migrieren

    * **Aufgaben**: Aufgabe erstellen, Aufgabe aktualisieren

    * **VM**:
        * Konfiguration > Alle
        * Interaktion > Frage beantworten, Geräteverbindung, CD-Medien konfigurieren, Diskettenmedien konfigurieren, Ausschalten, Einschalten, VMware-Tools installieren
        * Bestandsliste > Aus vorhandenen erstellen, Neu erstellen, Registrieren, Registrierung aufheben
        * Bereitstellung > Download virtueller Maschinen zulassen, Upload von Dateien virtueller Maschinen zulassen
        * Snapshot-Manager > Snapshots entfernen

    ![Das Dialogfeld „Rolle bearbeiten“](./media/site-recovery-multi-tenant-support-vmware-using-csp/edit-role-permissions.png)

3. Weisen Sie dem vCenter-Konto (das im Konfigurationsserver des Mandanten verwendet wird) für verschiedene Objekte Zugriffsebenen wie folgt zu:

>| Objekt | Rolle | Anmerkungen |
>| --- | --- | --- |
>| vCenter | Schreibgeschützt | Nur erforderlich, um vCenter den Zugriff zum Verwalten verschiedener Objekten zu gewähren. Sie können diese Berechtigung aufheben, wenn das Konto keinem Mandanten zur Verfügung gestellt wird oder nicht für Verwaltungsvorgänge in vCenter verwendet wird. |
>| Datacenter | Azure_Site_Recovery |  |
>| Host und Hostcluster | Azure_Site_Recovery | Stellt erneut sicher, dass der Zugriff auf Objektebene erfolgt, sodass es nur Zugriff auf Hosts gibt, die vor dem Failover und nach dem Failback über Mandanten-VMs verfügen. |
>| Datenspeicher und Datenspeichercluster | Azure_Site_Recovery | Siehe oben. |
>| Netzwerk | Azure_Site_Recovery |  |
>| Verwaltungsserver | Azure_Site_Recovery | Schließt den Zugriff auf alle Komponenten (Konfigurationsserver, Prozessserver und Masterzielserver) außerhalb des Konfigurationsservercomputers ein. |
>| Mandanten-VMs | Azure_Site_Recovery | Stellt sicher, dass alle neuen VMs eines bestimmten Mandanten auch diesen Zugriff erhalten, da sie andernfalls nicht im Azure-Portal ermittelt werden können. |

Der vCenter-Kontozugriff ist nun eingerichtet. Mit diesem Schritt ist die Anforderung an die Mindestberechtigungen zum Durchführen von Failbackvorgängen erfüllt. Diese Zugriffsberechtigungen können auch mit Ihren vorhandenen Richtlinien verwendet werden. Ändern Sie Ihre vorhandenen Berechtigungen lediglich so, dass die unter Punkt 2 zuvor genannten Rollenberechtigungen hinzugefügt werden.

Zum Einschränken von Vorgängen für die Notfallwiederherstellung bis zum Failoverstatus, d.h. ohne Failbackfunktionen, befolgen Sie das vorherige Verfahren. Doch anstatt dem vCenter-Zugriffskonto die Rolle *Azure_Site_Recovery* zuzuweisen, weisen Sie ihm lediglich die Rolle *Schreibgeschützt* zu. Dieser Berechtigungssatz lässt die Replikation und ein Failover von VMs, aber kein Failback zu. Alles andere im vorhergehenden Prozess bleibt unverändert. Berechtigungen werden weiterhin nur auf Objektebene zugewiesen und nicht an untergeordnete Objekte weitergegeben, um die Mandantenisolation sicherzustellen und die VM-Ermittlung einzuschränken.

## <a name="other-multi-tenant-environments"></a>Andere Umgebungen mit mehreren Mandanten

Im vorherigen Abschnitt wurde das Einrichten einer Umgebung für mehrere Mandanten für eine SHPS-Lösung detailliert beschrieben. Die anderen beiden wichtigen Lösungen heißen DHPS und MSP. Die Architektur dieser Lösungen wird in den folgenden Abschnitten beschrieben.

### <a name="dedicated-hosting-solution"></a>Dedizierte Hostinglösung (DHPS)

Wie im folgenden Diagramm dargestellt, ist der architektonische Unterschied bei einer dedizierten Hostinglösung, dass die Infrastruktur jedes Mandanten allein für den jeweiligen Mandanten eingerichtet ist. Da Mandanten mithilfe getrennter vCenter isoliert werden, muss der Hostinganbieter weiterhin die CSP-Schritte ausführen, die für eine gemeinsam genutzte Hostinglösung beschrieben wurden. Er muss sich jedoch nicht um die Mandantenisolation kümmern. Das CSP-Setup bleibt unverändert.

![Architektur beim SHPS-Modell](./media/site-recovery-multi-tenant-support-vmware-using-csp/dedicated-hosting-scenario.png)  
**Dediziertes Hostingszenario mit mehreren vCentern**

### <a name="managed-service-solution"></a>MSP-Lösung (Managed Services Provider, Managed Services Provider)

Wie im folgenden Diagramm dargestellt, ist der architektonische Unterschiede bei einer MSP-Lösung, dass die Infrastruktur jedes Mandanten auch physisch von der Infrastruktur anderen Mandanten getrennt ist. Dieses Szenario liegt in der Regel vor, wenn der Mandant die Infrastruktur besitzt und lediglich möchte, dass sich ein Lösungsanbieter um die Notfallwiederherstellung kümmert. Da Mandanten wiederum mithilfe unterschiedlicher Infrastrukturen physisch isoliert sind, muss der Partner wiederum die CSP-Schritte ausführen, die für eine gemeinsam genutzte Hostinglösung beschrieben wurden. Er muss sich jedoch nicht um die Mandantenisolation kümmern. Die CSP-Bereitstellung bleibt unverändert.

![Architektur beim SHPS-Modell](./media/site-recovery-multi-tenant-support-vmware-using-csp/managed-service-scenario.png)  
**MSP-Szenario mit mehreren vCentern**

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Verwalten von Azure Site Recovery-Bereitstellungen mithilfe der rollenbasierten Zugriffssteuerung finden Sie [hier](site-recovery-role-based-linked-access-control.md).

[Verwalten der Mehrinstanzenfähigkeit mit dem CSP-Programm (Cloud Solution Provider)](site-recovery-manage-multi-tenancy-with-csp.md)
