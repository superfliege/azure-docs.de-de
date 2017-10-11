---
title: Behandeln von Sicherheitswarnungen in Azure Security Center | Microsoft Docs
description: "Dieses Dokument unterstützt Sie beim Azure Security Center-Funktionen verwenden, um Sicherheitsvorfälle zu behandeln."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: e8feb669-8f30-49eb-ba38-046edf3f9656
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2017
ms.author: yurid
ms.openlocfilehash: a302f8cb2555eef469a24da2523fdd9b97cc5730
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="handling-security-incidents-in-azure-security-center"></a>Behandlung von Sicherheitsvorfälle in Azure Security Center
Selektierung und Untersuchen von Sicherheitswarnungen für die können für auch die am häufigsten qualifizierten Sicherheitsanalysten sehr zeitaufwändig sein, und für viele schwer feststellbar ist sogar wo begonnen wird. Mithilfe von [Analytics](security-center-detection-capabilities.md) Verbindung Informationen zwischen unterschiedlichen [Sicherheitswarnungen](security-center-managing-and-responding-alerts.md), Security Center können Sie mit einer einzelnen Ansicht eine Kampagne Angriff und alle dazugehörigen Warnungen bereitstellen – Sie können schnell verstehen, welche Aktionen der Angreifer hat und welche Ressourcen betroffen sind.

Dieses Dokument erläutert, wie Sicherheit alert-Funktion im Sicherheitscenter zur Unterstützung bei der Behandlung von Sicherheitsvorfälle.

## <a name="what-is-a-security-incident"></a>Was ist ein Sicherheitsvorfall?
Im Sicherheitscenter, ein Sicherheitsvorfall ist eine Ansammlung aller Warnungen für eine Ressource, die mit ausrichten [kill Chain](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/) Muster. Vorfälle werden in der [Sicherheitswarnungen](security-center-managing-and-responding-alerts.md) Kachel- und Blatt. Ein Vorfall wird die Liste der dazugehörigen Warnungen offenlegen, dadurch können Sie weitere Informationen zu jedem Vorkommen abzurufen.

## <a name="managing-security-incidents"></a>Verwalten von Sicherheitsvorfälle
Sie können Ihre aktuellen Sicherheitsvorfälle durch einen Blick auf die Sicherheit Warnungen Kachel überprüfen. Zugriff auf das Azure-Portal, und führen Sie die Schritte unten, um weitere Details zu jedem Sicherheitsvorfall finden Sie unter:

1. Auf dem Dashboard Sicherheitscenter sehen Sie die **Sicherheitswarnungen** Kachel.

    ![Kachel "Sicherheitswarnungen" im Sicherheitscenter](./media/security-center-incident/security-center-incident-fig1.png)

2. Klicken Sie auf diese Kachel zu erweitern und ein Sicherheitsvorfall erkannt wird, wird er unter dem Diagramm für Warnungen wie unten dargestellt angezeigt wird:

    ![Sicherheitsvorfall](./media/security-center-incident/security-center-incident-fig2.png)

3. Beachten Sie, dass die Beschreibung des Vorfalls Sicherheit ein anderes Symbol im Vergleich zu anderen Warnungen verfügt. Klicken Sie darauf, um weitere Informationen zu diesem Incident anzeigen.

    ![Sicherheitsvorfall](./media/security-center-incident/security-center-incident-fig3.png)

4. Auf der **Incident** Blatt Sie weitere erhalten details zu dieser Sicherheitsvorfall, das die vollständige Beschreibung, ihren Schweregrad enthält (die in diesem Fall hoch ist), seinem aktuellen Zustand (in diesem Fall ist es immer noch *active*, was bedeutet, dass den Benutzer eine Aktion, die er entnommen wurde nicht – Dies kann geschehen, indem Sie mit der rechten Maustaste auf den Incident in der **Sicherheitswarnungen** Blatt ") , die angegriffenen Ressource (in diesem Fall *VM1*), die Wiederherstellung nach dem Vorfall Schritte aus, und im unteren Bereich haben Sie die Warnungen, die in diesem Incident enthalten waren. Wenn Sie weitere Informationen zu einzelnen Warnungen erhalten möchten, wird Klicken Sie einfach darauf, und ein weiteres Blatt geöffnet, wie unten dargestellt:

    ![Sicherheitsvorfall](./media/security-center-incident/security-center-incident-fig4.png)

Die Informationen auf diesem Blatt variieren abhängig von der Warnung. Lesen [Verwalten von und reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md) für Weitere Informationen zum Verwalten dieser Warnungen. Einige wichtige Aspekte im Zusammenhang dieser Funktion werden soll:

* Ein neuer Filter können Sie Ihre anzeigen, Incident nur oder nur Warnungen anpassen.
* Dieselbe Warnung kann im Rahmen eines Vorfalls (falls zutreffend) als auch für sichtbar sein wie eine eigenständige Warnung vorhanden sein.

## <a name="see-also"></a>Weitere Informationen:
In diesem Dokument haben Sie gelernt, wie die Sicherheit Incident-Funktion im Sicherheitscenter zu verwenden. Weitere Informationen zum Security Center finden Sie in der folgenden:

* [Verwalten von und reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md)
* [Azure Security Center Erkennungsfunktionalität](security-center-detection-capabilities.md)
* [Azure Security Center Planung und Betriebshandbuch](security-center-planning-and-operations-guide.md)
* [Verwalten von und reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md)
* [Azure Security Center – häufig gestellte Fragen](security-center-faq.md)– häufig gestellte Fragen zur Verwendung des Diensts suchen.
* [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/)--Blogbeiträge zu Azure-Sicherheit und Kompatibilität zu finden.
