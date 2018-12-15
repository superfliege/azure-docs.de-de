---
title: Zielgruppenadressierung von Verwaltungslösungen in Azure | Microsoft-Dokumentation
description: Die Zielgruppenadressierung von Verwaltungslösungen bietet Ihnen die Möglichkeit, Verwaltungslösungen auf einen bestimmten Satz von Agents zu beschränken.  Dieser Artikel beschreibt, wie Sie eine Bereichskonfiguration erstellen und auf eine Lösung anwenden.
services: monitoring
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 1f054a4e-6243-4a66-a62a-0031adb750d8
ms.service: monitoring
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2017
ms.author: bwren
ms.openlocfilehash: d82c42fa734932655f536d4fc04a50b4d6904ac5
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2018
ms.locfileid: "53192752"
---
# <a name="targeting-management-solutions-in-azure-preview"></a>Zielgruppenadressierung von Verwaltungslösungen in Azure (Vorschau)
Wenn Sie eine Verwaltungslösung zu Ihrem Abonnement hinzufügen, wird sie automatisch standardmäßig für alle mit Ihrem Log Analytics-Arbeitsbereich verbundenen Windows- und Linux-Agents bereitgestellt.  Möglicherweise möchten Sie Ihre Kosten verwalten und den Umfang der für eine Lösung gesammelten Daten begrenzen, indem Sie sie auf einen bestimmten Satz von Agents beschränken.  Dieser Artikel beschreibt, wie Sie das Feature der **Zielgruppenadressierung für eine Lösung** verwenden, um einen Geltungsbereich auf Ihre Lösungen anzuwenden.

## <a name="how-to-target-a-solution"></a>Durchführen der Zielgruppenadressierung für eine Lösung
Die Zielgruppenadressierung für eine Lösung erfolgt in drei Schritten, wie in den folgenden Abschnitten beschrieben. 


### <a name="1-create-a-computer-group"></a>1. Erstellen einer Computergruppe
Sie geben durch das Erstellen einer [Computergruppe](../../azure-monitor/platform/computer-groups.md) in Log Analytics die Computer an, die Sie in einem Bereich einbeziehen möchten.  Die Computergruppe kann auf einer Protokollsuche basieren oder aus anderen Quellen importiert werden, wie z.B. Active Directory oder WSUS-Gruppen. Wie [unten beschrieben](#solutions-and-agents-that-cant-be-targeted) werden nur Computer in den Bereich einbezogen, die direkt mit Log Analytics verbunden sind.

Sobald Sie die Computergruppe in Ihrem Arbeitsbereich erstellt haben, beziehen Sie sie in eine Bereichskonfiguration ein, die auf eine oder mehrere Lösungen angewendet werden kann.
 
 
 ### <a name="2-create-a-scope-configuration"></a>2. Erstellen einer Bereichskonfiguration
 Eine **Bereichskonfiguration** enthält eine oder mehrere Computergruppen und kann auf eine oder mehrere Lösungen angewendet werden. 
 
 Erstellen Sie eine Bereichskonfiguration mithilfe des folgenden Vorgangs.  

 1. Navigieren Sie im Azure-Portal zu **Log Analytics**, und wählen Sie Ihren Arbeitsbereich aus.
 2. Wählen Sie unter **Arbeitsbereichsdatenquellen** in den Eigenschaften für den Arbeitsbereich **Bereichskonfigurationen**.
 3. Klicken Sie auf **Hinzufügen**, um eine neue Bereichskonfiguration zu erstellen.
 4. Geben Sie einen **Namen** für die Bereichskonfiguration an.
 5. Klicken Sie auf **Computergruppen auswählen**.
 6. Wählen Sie die Computergruppe, die Sie erstellt haben, und optional alle anderen Gruppen, die der Konfiguration hinzugefügt werden sollen.  Klicken Sie auf **Auswählen**.  
 6. Klicken Sie auf **OK**, um die Bereichskonfiguration zu erstellen. 


 ### <a name="3-apply-the-scope-configuration-to-a-solution"></a>3. Wenden Sie die Bereichskonfiguration auf eine Lösung an.
Sobald Sie eine Bereichskonfiguration haben, können Sie sie auf eine oder mehrere Lösungen anwenden.  Beachten Sie: Eine einzelne Bereichskonfiguration kann zwar mit mehreren Lösungen verwendet werden, doch jede Lösung kann nur eine Bereichskonfiguration verwenden.

Wenden Sie eine Bereichskonfiguration mithilfe des folgenden Vorgangs an.  

 1. Navigieren Sie im Azure-Portal zu **Log Analytics**, und wählen Sie Ihren Arbeitsbereich aus.
 2. Wählen Sie in den Eigenschaften für den Arbeitsbereich **Lösungen**.
 3. Klicken Sie auf die Lösung, für die Sie einen Bereich festlegen möchten.
 4. Wählen Sie unter **Arbeitsbereichsdatenquellen** in den Eigenschaften für die Lösung **Zielgruppenadressierung für Lösung**.  Wenn die Option nicht verfügbar ist, dann [ist für diese Lösung keine Zielgruppenadressierung möglich](#solutions-and-agents-that-cant-be-targeted).
 5. Klicken Sie auf **Bereichskonfiguration hinzufügen**.  Wenn Sie bereits eine Konfiguration auf diese Lösung angewendet haben, ist diese Option nicht verfügbar.  Sie müssen die vorhandene Konfiguration vor dem Hinzufügen einer anderen entfernen.
 6. Klicken Sie auf die Bereichskonfiguration, die Sie erstellt haben.
 7. Vergewissern Sie sich, dass die Konfiguration den **Status** **Erfolgreich** hat.  Wenn der Status einen Fehler meldet, klicken Sie auf die Auslassungspunkte rechts neben der Konfiguration, und wählen Sie **Bereichskonfiguration bearbeiten**, um Änderungen vorzunehmen.

## <a name="solutions-and-agents-that-cant-be-targeted"></a>Lösungen und Agents, für die keine Zielgruppenadressierung möglich ist
Im Folgenden werden die Kriterien für Agents und Lösungen erörtert, die nicht mit der Zielgruppenadressierung für Lösung verwendet werden können.

- Zielgruppenadressierung für Lösung kann nur auf Lösungen angewendet werden, die für Agents bereitgestellt werden.
- Zielgruppenadressierung für Lösung kann nur auf Lösungen angewendet werden, die von Microsoft bereitgestellt werden.  Sie kann nicht auf Lösungen angewendet werden, die [von Ihnen selbst oder Partnern erstellt](solutions-creating.md) worden sind.
- Sie können nur Agents herausfiltern, die direkt mit Log Analytics verbunden sind.  Lösungen werden automatisch für alle Agents bereitgestellt, die Teil einer verbundenen Operations Manager-Verwaltungsgruppe sind, und zwar unabhängig davon, ob sie in einer Bereichskonfiguration enthalten sind oder nicht.

### <a name="exceptions"></a>Ausnahmen
Zielgruppenadressierung für Lösung kann auch dann nicht mit den folgenden Lösungen verwendet werden, wenn diese den angegebenen Kriterien entsprechen.

- Bewertung der Agent-Integrität

## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zu Verwaltungslösungen, inklusive der Lösungen, die zur Installation in Ihrer Umgebung verfügbar sind, finden Sie unter [Hinzufügen von Azure Log Analytics-Verwaltungslösungen zu Ihrem Arbeitsbereich](solutions.md).
- Weitere Informationen zu Computergruppen finden Sie unter [Computergruppen in Log Analytics-Protokollsuchen](../../azure-monitor/platform/computer-groups.md).
