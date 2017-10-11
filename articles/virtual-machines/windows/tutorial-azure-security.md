---
title: Azure Security Center und Windows-VMs in Azure | Microsoft Docs
description: "Informationen Sie zur Sicherheit für den Windows Azure-virtuellen Computer mit Azure Security Center."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/01/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: adb00e28b0b204858a763f83836ee2ac96f8f9e4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-virtual-machine-security-by-using-azure-security-center"></a>Überwachen der virtuellen Maschine Sicherheit mithilfe von Azure Security Center

Azure Security Center können Sie die Einsicht in Ihre Azure-Ressource Sicherheitsmaßnahmen zu erhalten. Security Center bietet integrierte Sicherheit zu überwachen. Sie können Bedrohungen erkennen, die andernfalls unbemerkt möglicherweise. In diesem Lernprogramm lernen Sie Azure Security Center, und wie Sie:
 
> [!div class="checklist"]
> * Datensammlung einrichten
> * Einrichten von Sicherheitsrichtlinien
> * Anzuzeigen Sie und zu beheben Sie Configuration-Integritätsproblemen
> * Überprüfen Sie erkannte Bedrohungen  

## <a name="security-center-overview"></a>Sicherheitscenter (Übersicht)

Sicherheitscenter potenzieller Konfigurationsprobleme in virtuellen Computern (VM) identifiziert und Sicherheitsrisiken ausgerichtet. Diese können virtuelle Computer enthalten, in denen netzwerksicherheitsgruppen, unverschlüsselte Datenträger und Brute-Force-Remote Desktop Protocol (RDP)-Angriffe fehlen. Die Informationen werden auf dem Dashboard Sicherheitscenter in einfach zu lesende Diagramme angezeigt.

Wählen Sie zum Zugriff auf das Dashboard Sicherheitscenter in Azure-Portal auf der Menüleiste **Sicherheitscenter**. Auf dem Dashboard können Sie finden Sie unter der Sicherheitsintegrität der Azure-Umgebung, suchen die Anzahl der aktuellen Empfehlungen und zeigt den aktuellen Status der Bedrohung Warnungen. Sie können jedes Diagramm auf hoher Ebene, um weitere Details finden Sie unter erweitern.

![Security Center-dashboard](./media/tutorial-azure-security/asc-dash.png)

Sicherheitscenter hinausgeht Datenermittlung, Empfehlungen für Probleme bereitzustellen, die er erkennt. Wenn ein virtueller Computer ohne eine angefügte Netzwerksicherheitsgruppe bereitgestellt wurde, zeigt Sicherheitscenter z. B. eine Empfehlung, mit Maßnahmen, die Sie ergreifen können. Sie erhalten automatisierte Wartungsvorgänge ohne Sicherheitscenter Kontext verlassen zu müssen.  

![Empfehlungen](./media/tutorial-azure-security/recommendations.png)

## <a name="set-up-data-collection"></a>Datensammlung einrichten

Bevor Sie die Sichtbarkeit in Sicherheitskonfigurationen VM abrufen können, müssen Sie Security Center-Datensammlung einrichten. Dies umfasst das Aktivieren der Datensammlung, und erstellen ein Azure-Speicherkonto zum Speichern von gesammelten Daten. 

1. Klicken Sie auf das Dashboard Sicherheitscenter auf **Sicherheitsrichtlinie**, und wählen Sie Ihr Abonnement. 
2. Für **Datensammlung**Option **auf**.
3. Wählen Sie zum Erstellen eines Speicherkontos **wählen Sie ein Speicherkonto**. Aktivieren Sie das Kontrollkästchen **OK**.
4. Auf der **Sicherheitsrichtlinie** Blatt select **speichern**. 

Der Sicherheitscenter Auflistung-Agent auf allen virtuellen Computern installiert, und die Datensammlung beginnt. 

## <a name="set-up-a-security-policy"></a>Eine Sicherheitsrichtlinie einrichten

Sicherheitsrichtlinien werden verwendet, um die Elemente zu definieren, für die Sicherheitscenter sammelt Daten und gibt Empfehlungen. Sie können verschiedene Richtlinien auf verschiedene Azure-Ressourcen anwenden. Zwar wird standardmäßig Azure-Ressourcen für alle Richtlinienelemente ausgewertet werden, können Sie einzelne Richtlinienelemente für alle Azure-Ressourcen oder für eine Ressourcengruppe deaktivieren. Ausführliche Informationen über die Security Center-Sicherheitsrichtlinien finden Sie unter [Sicherheitsrichtlinien festlegen, in Azure Security Center](../../security-center/security-center-policies.md). 

So richten eine Sicherheitsrichtlinie für alle Azure-Ressourcen

1. Wählen Sie auf dem Dashboard Sicherheitscenter **Sicherheitsrichtlinie**, und wählen Sie Ihr Abonnement.
2. Wählen Sie **Richtlinie zum Verhindern von Datenverlusten**.
3. Aktivieren Sie oder deaktivieren Sie der Richtlinienelemente, die Sie auf alle Azure-Ressourcen anwenden möchten.
4. Wenn Sie alle Ihre Einstellungen ausgewählt haben, wählen **OK**.
5. Auf der **Sicherheitsrichtlinie** Blatt select **speichern**. 

So richten eine Richtlinie für eine bestimmte Ressourcengruppe ein.

1. Wählen Sie auf dem Dashboard Sicherheitscenter **Sicherheitsrichtlinie**, und wählen Sie dann eine Ressourcengruppe.
2. Wählen Sie **Richtlinie zum Verhindern von Datenverlusten**.
3. Aktivieren Sie oder deaktivieren Sie der Richtlinienelemente, die Sie für die Ressourcengruppe anwenden möchten.
4. Klicken Sie unter **Vererbung**Option **Unique**.
5. Wenn Sie alle Ihre Einstellungen ausgewählt haben, wählen **OK**.
6. Auf der **Sicherheitsrichtlinie** Blatt select **speichern**.  

Sie können die Datensammlung für eine bestimmte Ressourcengruppe auf dieser Seite auch deaktivieren.

Im folgenden Beispiel wurde eine eindeutige Richtlinie für eine Ressourcengruppe mit dem Namen erstellt *MyResoureGroup*. In dieser Richtlinie werden die Datenträger-Verschlüsselung und Web Application Firewall Empfehlungen deaktiviert.

![Eindeutige Richtlinie](./media/tutorial-azure-security/unique-policy.png)

## <a name="view-vm-configuration-health"></a>Anzeigen von VM-Konfigurationsstatus

Nachdem Sie die Datensammlung aktiviert und eine Sicherheitsrichtlinie festgelegt haben, werden seine Aufgaben sofort Sicherheitscenter Warnungen und Empfehlungen. Wie VMs bereitgestellt werden, ist der Collection-Agent installiert. Sicherheitscenter ist für die neuen virtuellen Computer anschließend mit Daten aufgefüllt. Ausführliche Informationen über die Integrität der VM-Konfiguration finden Sie unter [schützen Sie Ihre virtuellen Computer im Sicherheitscenter](../../security-center/security-center-virtual-machine-recommendations.md). 

Wie Daten gesammelt werden, wird die ressourcenintegrität für jeden virtuellen Computer und die zugehörigen Azure-Ressource aggregiert. Die Informationen werden in einem einfach zu lesende angezeigt. 

So zeigen Sie die ressourcenintegrität an:

1.  Zentrieren Sie auf der Sicherheits-Dashboard unter **Sicherheit ressourcenintegrität**Option **berechnen**. 
2.  Auf der **berechnen** Blatt wählen **VMs**. Diese Ansicht bietet einen Überblick über den Konfigurationsstatus für Ihre virtuellen Computer.

![Berechnen der Integrität](./media/tutorial-azure-security/compute-health.png)

Wenn alle Empfehlungen für einen virtuellen Computer anzeigen zu können, wählen Sie den virtuellen Computer. Empfehlungen und Wiederherstellung werden im nächsten Abschnitt dieses Lernprogramms ausführlicher behandelt.

## <a name="remediate-configuration-issues"></a>Beheben von Konfigurationsproblemen

Nach dem Beginn der Security Center mit Konfigurationsdaten zu füllende Empfehlungen basierend auf der Sicherheitsrichtlinie erfolgen, die Sie haben eingerichtet. Wenn ein virtuellen Computer ohne eine zugeordnete Netzwerksicherheitsgruppe eingerichtet wurde, wird z. B. eine Empfehlung hergestellt, um eines zu erstellen. 

Um eine Liste aller Empfehlungen finden Sie unter: 

1. Wählen Sie auf dem Dashboard Sicherheitscenter **Empfehlungen**.
2. Wählen Sie eine bestimmte Empfehlung. Eine Liste aller Ressourcen für die der Empfehlung gilt wird angezeigt.
3. Um eine Empfehlung anwenden möchten, wählen Sie eine bestimmte Ressource. 
4. Befolgen Sie die Anweisungen für die Wiederherstellungsschritte aus. 

In vielen Fällen Anleitung Sicherheitscenter Handlungsbedarf, die Sie ergreifen können, um eine Empfehlung zu behandeln, ohne Sicherheitscenter verlassen zu müssen. Im folgenden Beispiel wird Sicherheitscenter eine Netzwerksicherheitsgruppe, die eine uneingeschränkte Eingangsregel wurde erkannt. Wählen Sie auf der Seite Empfehlung die **eingehende Regeln bearbeiten** Schaltfläche. Wird angezeigt, die Benutzeroberfläche, die benötigt werden, um die Regel zu ändern. 

![Empfehlungen](./media/tutorial-azure-security/remediation.png)

Wie Empfehlungen wiederhergestellt werden, werden sie als gelöst markiert. 

## <a name="view-detected-threats"></a>Anzeigen der erkannte Bedrohungen

Zusätzlich zu den Ressourcen-gatewaykonfigurationsempfehlungen zeigt Sicherheitscenter Bedrohung Erkennung Warnungen. Die Sicherheit Warnfunktion aggregiert Daten aus jeder VM Netzwerke Azure-Protokolle und verbundene partnerlösungen zum Erkennen von Sicherheitsrisiken für Azure-Ressourcen. Ausführliche Informationen zu Sicherheitscenter Bedrohung Erkennungsfunktionalität, finden Sie unter [Azure Security Center Erkennungsfunktionalität](../../security-center/security-center-detection-capabilities.md).

Die Sicherheit Warnfunktion erfordert Sicherheitscenter Tarif aus erhöht werden *frei* auf *Standard*. Eine 30-Tage- **kostenlose Testversion** ist verfügbar, wenn Sie auf diese höhere Preisebene verschieben. 

So ändern Sie die Preisstufe  

1. Klicken Sie auf das Dashboard Sicherheitscenter auf **Sicherheitsrichtlinie**, und wählen Sie Ihr Abonnement.
2. Wählen Sie **Tarif**.
3. Wählen Sie die neue Ebene, und wählen Sie dann **wählen**.
4. Auf der **Sicherheitsrichtlinie** Blatt select **speichern**. 

Nachdem Sie die Preisstufe geändert haben, startet das Diagramm für die Warnungen wie bei der Sicherheit aufgefüllt, die Bedrohungen erkannt werden.

![Sicherheitswarnungen](./media/tutorial-azure-security/security-alerts.png)

Wählen Sie eine Warnung aus, um Informationen anzuzeigen. Beispielsweise können Sie eine Beschreibung der Bedrohung, den Zeitpunkt der Erkennung, alle Versuche der Bedrohung und die empfohlenen Wartung finden Sie unter. Im folgenden Beispiel wurde ein RDP-Brute-Force-Angriff mit 294 RDP Fehlversuchen ermittelt. Eine empfohlene Lösung wird bereitgestellt.

![RDP-Angriff](./media/tutorial-azure-security/rdp-attack.png)

## <a name="next-steps"></a>Nächste Schritte
In diesem Lernprogramm Azure Security Center eingerichtet und überprüft dann VMs im Sicherheitscenter. Sie haben gelernt, wie auf:

> [!div class="checklist"]
> * Datensammlung einrichten
> * Einrichten von Sicherheitsrichtlinien
> * Anzuzeigen Sie und zu beheben Sie Configuration-Integritätsproblemen
> * Überprüfen Sie erkannte Bedrohungen

Wechseln Sie zum nächsten Lernprogramm erfahren, wie eine CI-CD-Pipeline mit Visual Studio Team Services und Windows-VM mit IIS zu erstellen.

> [!div class="nextstepaction"]
> [Visual Studio Team Services CI/CD-pipeline](./tutorial-vsts-iis-cicd.md)
