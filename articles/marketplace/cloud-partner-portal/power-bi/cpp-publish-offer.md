---
title: Veröffentlichen eines Power BI-App-Angebots – Azure Marketplace | Microsoft-Dokumentation
description: Veröffentlichen eines Power BI-App-Angebots im Microsoft AppSource-Marketplace.
services: Azure, AppSource, Marketplace, Cloud Partner Portal, Power BI
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 01/31/2019
ms.author: pbutlerm
ms.openlocfilehash: 2b3783060cf5502076ce3bc98cf07f005366a9e2
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/02/2019
ms.locfileid: "55665711"
---
# <a name="publish-power-bi-app-offer"></a>Veröffentlichen eines Power BI-App-Angebots

Nachdem Sie das Angebot im Portal definiert und die zugeordneten technischen Ressourcen erstellt haben, wird das Angebot im letzten Schritt zur Veröffentlichung übermittelt.  Klicken Sie zum Starten dieses Prozesses im vertikalen Menü des Fensters **Neues Angebot** auf die Schaltfläche **Veröffentlichen**.  Weitere Informationen finden Sie unter [Veröffentlichen von Azure Marketplace- und AppSource-Angeboten](../manage-offers/cpp-publish-offer.md).


## <a name="publishing-steps"></a>Veröffentlichungsschritte

Im folgenden Diagramm sind die wichtigsten Schritte des Veröffentlichungsprozesses dargestellt, die ausgeführt werden müssen, um ein Angebot live zu schalten.

![Schritte des Veröffentlichungsprozesses für eine Power BI-App](./media/publishing-process-steps.png)

Die folgende Tabelle enthält die Beschreibung dieser Schritte sowie die voraussichtliche maximale Dauer der einzelnen Schritte:

|   Veröffentlichungsschritt            |   Zeit     |   BESCHREIBUNG                                                                  |
| --------------------         |------------| ----------------                                                               |
| Überprüfen der Voraussetzungen       | 15 Min.     | Die Angebotsinformationen und -einstellungen werden überprüft.                            |
| Zertifizierung                | 1–7 Tage   | Das Power BI-Zertifizierungsteam analysiert Ihr Angebot. Wir führen Ihre Power BI-App über einen manuellen Überprüfungstest aus und installieren die App dazu über die angegebene Installations-URL. Die wesentlichen Überprüfungen werden als Teil des App-Zertifizierungsprozesses durchgeführt (siehe weiter unten).         |
| Verpackung                    | \< 1 Stunde  | Die technischen Ressourcen des Angebots werden für die Verwendung durch die Kunden gepackt.                        |
| Registrierung von generierten Leads | \< 1 Stunde  | Die Leadsysteme werden konfiguriert und bereitgestellt.                                      |
| Einholen der Genehmigung des Herausgebers            | \-         | Letzte Überprüfung und Bestätigung durch den Herausgeber, bevor das Angebot live geschaltet wird. Nun wird auch ein Link angezeigt, um eine Vorschau von Ihrem Angebot zu erstellen. Wenn Sie mit der Darstellung der Vorschau zufrieden sind, klicken Sie auf der Registerkarte **Status** auf die Schaltfläche **Live schalten**. Mit dieser Aktion wird eine Anforderung an das Onboardingteam gesendet, um Ihre App in AppSource aufzulisten.    |
| Live                         | \< 3 Stunden | Ihr Angebot wird jetzt in AppSource öffentlich aufgelistet (live geschaltet), und Kunden können Ihre App in ihren Power BI-Abonnements einsehen und bereitstellen. Außerdem erhalten Sie eine Bestätigungs-E-Mail. Sie können jederzeit auf die Registerkarte **Alle Angebote** klicken und den in der rechten Spalte aufgeführten Status für alle Ihre Angebote einsehen. Um ausführliche Informationen zum Status des Veröffentlichungsvorgangs für Ihr Angebot anzuzeigen können Sie auf die Registerkarte **Status** klicken. |
|   |   |

Es kann bis zu acht Tage dauern, bis dieser Prozess abgeschlossen ist. Nach dem Ausführen dieser Veröffentlichungsschritte wird Ihr Power BI-App-Angebot in [AppSource](https://appsource.microsoft.com/marketplace/apps?product=power-bi%20) im Abschnitt zu den Power BI-Apps aufgeführt.


### <a name="app-certification-process"></a>App-Zertifizierungsprozess

Das Microsoft-Onboardingteam überprüft die Übermittlung Ihres Power BI-Angebots mit folgendem Verfahren:

1. Die rechtlich relevanten Dokumente und Hilfelinks werden überprüft.
2. Die Kontaktinformationen des Supports werden überprüft.
3. Die ordnungsgemäße Installation wird über die Installations-URL überprüft. 
4. Die App wird auf Schadsoftware und andere schädliche Inhalte überprüft. 
5. Es wird überprüft, ob die angezeigten Inhalte der Beschreibung der App entsprechen.
6. Es wird überprüft, ob App-bezogene Vorgänge wie erwartet in Power BI durchgeführt werden: Öffnen von Berichten und Dashboards mit Beispieldaten, Herstellen einer Verbindung mit benutzerdefinierten Datenquellen, Aktualisieren usw.

Das Zertifizierungsteam gibt Feedback, wenn Probleme gefunden werden.  Weitere Informationen zu den Anforderungen für Power BI-Apps finden Sie in der [Dokumentation zu Power BI-Apps](https://go.microsoft.com/fwlink/?linkid=2028636).


## <a name="next-steps"></a>Nächste Schritte

Es wird empfohlen, Ihre App im [AppSource-Marketplace](https://appsource.microsoft.com) regelmäßig zu überprüfen.  Zudem sollten Sie die Funktion [Seller Insights](../../cloud-partner-portal-orig/si-getting-started.md) des [Cloud-Partnerportals](https://cloudpartner.azure.com/#insights) verwenden, um Einblicke in Ihre Marketplace-Kunden und die Verwendung der App zu erhalten.  Sie können auch bestimmte [Aktualisierungen an Ihrem Angebot vornehmen](./cpp-update-existing-offer.md).
