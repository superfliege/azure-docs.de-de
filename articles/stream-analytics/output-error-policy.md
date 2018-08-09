---
title: Ausgabefehlerrichtlinien in Azure Stream Analytics
description: Hier erfahren Sie mehr über die Richtlinien zur Ausgabefehlerbehandlung, die in Azure Stream Analytics verfügbar sind.
services: stream-analytics
author: sidram
ms.author: sidram
manager: jeanb
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 07/27/2018
ms.openlocfilehash: f854e8ce35ac9d0a99b4a7dc1cdc66724114a5c4
ms.sourcegitcommit: e3d5de6d784eb6a8268bd6d51f10b265e0619e47
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2018
ms.locfileid: "39392567"
---
# <a name="output-error-policy"></a>Ausgabefehlerrichtlinie
In diesem Artikel werden die Richtlinien zur Ausgabefehlerbehandlung beschrieben, die in Azure Stream Analytics konfiguriert werden können.

Richtlinien zur Ausgabefehlerbehandlung gelten nur für Konvertierungsfehler, die auftreten, wenn das von einem Stream Analytics-Auftrag generierte Ausgabeereignis nicht dem Schema der Zielsenke entspricht. Sie können diese Richtlinie durch Auswählen von **Wiederholen** oder **Verwerfen** konfigurieren. Wählen Sie im Azure-Portal in einem Stream Analytics-Auftrag unter **Konfigurieren** die Option **Fehlerrichtlinie**, um Ihre Auswahl zu treffen.

![Fehlerrichtlinie: Position](./media/stream-analytics-error-policy/stream-analytics-error-policy-locate.PNG)


## <a name="retry"></a>Wiederholen
Wenn ein Fehler auftritt, wiederholt Azure Stream Analytics den Schreibvorgang für das Ereignis unbegrenzt, bis er erfolgreich ist. Für Wiederholungen gilt kein Timeout. Letztendlich wird die Verarbeitung aller nachfolgenden Ereignisse durch das Ereignis blockiert, für das Wiederholungen ausgeführt werden. Diese Option ist die Standardrichtlinie für die Ausgabefehlerbehandlung.

## <a name="drop"></a>Verwerfen
Azure Stream Analytics verwirft jedes Ausgabeereignis, das zu einem Konvertierungsfehler führt. Die verworfenen Ereignisse können nicht zur späteren erneuten Verarbeitung wiederhergestellt werden.


Alle vorübergehenden Fehler (z.B. Netzwerkfehler) werden unabhängig von der Konfiguration der Richtlinien für die Ausgabefehlerbehandlung wiederholt.


## <a name="next-steps"></a>Nächste Schritte
[Leitfaden zur Behandlung von Problemen bei Azure Stream Analytics](stream-analytics-troubleshooting-guide.md)
