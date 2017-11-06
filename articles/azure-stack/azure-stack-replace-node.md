---
title: "Austauschen eines Skalierungseinheitknotens für ein in Azure Stack integriertes System | Microsoft Docs"
description: Erfahren Sie, wie Sie einen physischen Skalierungseinheitknoten in einem in Azure Stack integrierten System austauschen.
services: azure-stack
documentationcenter: 
author: troettinger
manager: byronr
editor: 
ms.assetid: f9434689-ee66-493c-a237-5c81e528e5de
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2017
ms.author: twooley
ms.openlocfilehash: 63ba6cedd32bcf6ea3039bc80cc4a3f3407adfa7
ms.sourcegitcommit: cf4c0ad6a628dfcbf5b841896ab3c78b97d4eafd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2017
---
# <a name="replace-a-scale-unit-node-on-an-azure-stack-integrated-system"></a>Austauschen eines Skalierungseinheitknotens für ein in Azure Stack integriertes System

*Gilt für: Integrierte Azure Stack-Systeme*

In diesem Artikel wird der allgemeine Vorgang zum Austauschen eines physischen Computers (auch als *Skalierungseinheitknoten* bezeichnet) für ein in Azure Stack integriertes System beschrieben. Die tatsächlichen Schritte zum Austauschen eines Skalierungseinheitknotens sind abhängig von Ihrem OEM-Hardwareanbieter unterschiedlich. Die für Ihr System spezifischen ausführlichen Schritte finden Sie in der FRU-Dokumentation (Field Replaceable Unit) des Herstellers.

Das folgende Flussdiagramm zeigt den allgemeinen FRU-Vorgang zum Austauschen einer vollständigen Hardwarekomponente.

![Flussdiagramm für den Vorgang des Knotenaustauschs](media/azure-stack-replace-node/ReplaceNodeFlow.PNG)

*Diese Aktion ist abhängig vom physischen Zustand der Hardware möglicherweise nicht erforderlich.

## <a name="review-alert-information"></a>Überprüfen von Warnungsinformation

Wenn ein Skalierungseinheitknoten nicht verfügbar ist, erhalten Sie die folgenden kritischen Warnungen:

- Knoten ist nicht mit Netzwerkcontroller verbunden.
- Auf Knoten kann für Platzierung des virtuellen Computers nicht zugegriffen werden.
- Skalierungseinheitknoten ist offline.

![Liste der Warnungen für nicht verfügbare Skalierungseinheit](media/azure-stack-replace-node/NodeDownAlerts.PNG)

Wenn Sie die Warnung „Skalierungseinheitknoten ist offline“ öffnen, können Sie der Beschreibung der Warnung den Skalierungseinheitknoten entnehmen, auf den nicht zugegriffen werden kann. Möglicherweise erhalten Sie auch weitere Warnungen in der OEM-spezifischen Überwachungslösung, die auf dem Hardwarelebenszyklushost ausgeführt wird.

![Details der Warnung „Knoten offline“](media/azure-stack-replace-node/NodeOffline.PNG)

## <a name="scale-unit-node-replacement-process"></a>Austauschvorgang des Skalierungeinheitknotens

Die folgenden Schritte werden als allgemeine Übersicht zum Austauschvorgang des Skalierungseinheitknotens bereitgestellt. Die für Ihr System spezifischen ausführlichen Schritte finden Sie in der FRU-Dokumentation (Field Replaceable Unit) des OEM-Hardwareherstellers. Befolgen Sie diese Schritte nicht, ohne zuvor die vom OEM-Anbieter bereitgestellte Dokumentation zu Rate gezogen zu haben.

1. Verwenden Sie die Aktion [Ausgleichen](azure-stack-node-actions.md#scale-unit-node-actions), um den Skalierungseinheitknoten in den Wartungsmodus zu versetzen. Diese Aktion ist abhängig vom physischen Zustand der Hardware möglicherweise nicht erforderlich.
2. Wenn der Knoten noch an die Stromversorgung angeschlossen ist, verwenden Sie die Aktion [Ausschalten](azure-stack-node-actions.md#scale-unit-node-actions). Diese Aktion ist abhängig vom physischen Zustand der Hardware möglicherweise nicht erforderlich.
 
   > [!NOTE]
   > Verwenden Sie in dem unwahrscheinlichen Fall, dass die Ausschaltaktion nicht funktioniert, stattdessen die Webbenutzeroberfläche des Baseboard-Verwaltungscontrollers (BMC).

1. Tauschen Sie den physischen Computer aus. In der Regel erfolgt dies durch Ihren OEM-Hardwarehersteller.
2. Verwenden Sie die Aktion [Reparieren](azure-stack-node-actions.md#scale-unit-node-actions), um der Skalierungseinheit den neuen physischen Computer hinzuzufügen.
3. Verwenden Sie den privilegierten Endpunkt, um den [Status der Reparatur des virtuellen Datenträgers zu überprüfen](azure-stack-replace-disk.md#check-the-status-of-virtual-disk-repair). Bei neuen Datenlaufwerken kann ein vollständiger Speicherreparaturauftrag je nach Systemlast und genutztem Speicherplatz mehrere Stunden dauern.
4. Überprüfen Sie nach Abschluss der Reparaturaktion, ob alle aktiven Warnungen automatisch geschlossen wurden.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Ersetzen eines im laufenden Betrieb austauschbaren physischen Datenträgers finden Sie unter [Ersetzen eines Datenträgers](azure-stack-replace-disk.md). 
- Informationen zum Ersetzen einer Hardwarekomponente, die nicht im laufenden Betrieb ausgetauscht werden kann, finden Sie unter [Ersetzen einer Hardwarekomponente](azure-stack-replace-component.md). 