---
title: "Erfahren Sie mehr über die Einschränkung in BizTalk Services | Microsoft Docs"
description: "Informationen Sie zur einschränkungsschwellenwerten und daraus resultierende Laufzeitverhalten für BizTalk-Dienste. Einschränkung basiert auf speicherauslastung und die Anzahl der Nachrichten. MABS WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: f6663cf2-cda4-4bac-855e-27d2ad5c4fa4
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 145e7470bbc01c676a1fb5856c0f9a8726e667fc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="biztalk-services-throttling"></a>BizTalk Services: Einschränkung

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Azure BizTalk Services implementiert dienstdrosselung basierend auf zwei Bedingungen: speicherauslastung und die Anzahl der Nachrichten gleichzeitig verarbeiten. Dieses Thema listet die einschränkungsschwellenwerte und beschreibt das Laufzeitverhalten aus, wenn eine Einschränkung angewendet wird.

## <a name="throttling-thresholds"></a>Einschränkungsschwellenwerte
Die folgende Tabelle enthält die Drosselung Quelle und die Schwellenwerte:

|  | Beschreibung | Niedriger Schwellenwert | Hoher Schwellenwert |
| --- | --- | --- | --- |
| Arbeitsspeicher |% des insgesamt System-Arbeitsspeicher-verfügbare/PageFileBytes. <p><p>Insgesamt verfügbaren PageFileBytes beträgt ungefähr 2 Mal der Arbeitsspeicher des Systems. |60% |70 % |
| Verarbeitung von Nachrichten |Anzahl der Nachrichten gleichzeitig verarbeiten |40 * Anzahl der Kerne |100 * Anzahl der Kerne |

Wenn ein hoher Schwellenwert erreicht wird, startet Azure BizTalk Services zur Drosselung. Einschränkung wird beendet, wenn der untere Schwellenwert erreicht wird. Der Dienst wird z. B. 65 % Systemspeicher verwendet. In diesem Fall wird der Dienst nicht einschränken. Der Dienst gestartet wird, mithilfe von 70 % Systemspeicher. In diesem Fall wird der Dienst drosselt und weiterhin einschränken, bis der Dienst % von 60 (niedriger Schwellenwert) Systemspeicher verwendet.

Azure BizTalk Services verfolgt die Drosselung Status (normaler Status im Vergleich zu gedrosselt Zustand) und die Dauer der Einschränkung.

## <a name="runtime-behavior"></a>Laufzeitverhalten
Wenn Azure BizTalk Services eine einschränkungsstatus eingibt, geschieht Folgendes:

* Einschränkung wird pro Rolleninstanz. Beispiel:<br/>
  RoleInstanceA ist die Drosselung. RoleInstanceB ist keine Einschränkung. In diesem Fall werden die Nachrichten in RoleInstanceB wie erwartet verarbeitet. Nachrichten in RoleInstanceA verworfen, und mit der folgenden Fehlermeldung fehlschlagen:<br/><br/>
  **Server ist ausgelastet. Bitte versuchen Sie es erneut.**<br/><br/>
* Alle Quellen Pull abrufen oder Herunterladen eine Nachricht nicht. Beispiel:<br/>
  Eine Pipeline abruft Nachrichten aus einer externen FTP-Quelle. In einem Status der nachrichtenveröffentlichungseinschränkung Ruft die Rolleninstanz, die auf diese Weise die Pull ab. In diesem Fall beendet die Pipeline zusätzliche Nachrichten herunterladen, bis die Rolleninstanz Einschränkung wird beendet.
* Eine Antwort wird an den Client gesendet werden, damit der Client die Nachricht erneut senden kann.
* Sie müssen warten, bis die Einschränkung aufgelöst wird. Insbesondere müssen Sie warten, bis der untere Schwellenwert erreicht ist.

## <a name="important-notes"></a>Wichtige Hinweise
* Einschränkung kann nicht deaktiviert werden.
* Einschränkungsschwellenwerten kann nicht geändert werden.
* Einschränkung wird die systemweite implementiert.
* Azure SQL-Datenbankserver verfügt auch über integrierte Einschränkung.

## <a name="additional-azure-biztalk-services-topics"></a>Weitere Azure BizTalk Services-Themen
* [Installieren des Azure BizTalk Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
* [Lernprogramme: Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
* [Gewusst wie: Starten Sie mithilfe der Azure BizTalk Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a>Siehe auch
* [BizTalk Services: Developer, Basic, Standard und Premium Edition-Diagramm](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
* [BizTalk Services: Bereitstellung Klassisches mit Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
* [BizTalk Services: Dienststatusdiagramm](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
* [BizTalk Services: Registerkarten "Dashboard, Monitor und Skala"](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
* [BizTalk Services: Sichern und Wiederherstellen](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
* [BizTalk Services: Ausstellername und Ausstellerschlüssel](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>

