---
title: Live-Schaltung Ihres Angebots zu Azure-Anwendungen im Azure Marketplace | Microsoft-Dokumentation
description: .
services: Azure, Marketplace, Cloud Partner Portal,
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
ms.date: 11/15/2018
ms.author: pbutlerm
ms.openlocfilehash: 18a8e6ae8ab3bd4299c6a014f938e73a2a021492
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2018
ms.locfileid: "52263587"
---
<a name="make-your-azure-application-offer-live-on-azure-marketplace"></a>Live-Schaltung Ihres Angebots zu Azure-Anwendungen im Azure Marketplace 
===========================================================

Nachdem Sie alle Angebotsdetails ausgefüllt haben, können Sie nun Ihr Angebot veröffentlichen und im Azure Marketplace live schalten. Es gibt verschiedene Schritte, um sicherzustellen, dass Ihre Marketinginhalte und Ihre Anwendung die Qualitätsanforderungen gemäß Azure Certified erfüllen, bevor sie auf der Website live geschaltet werden.

![Veröffentlichungsfluss](./media/cloud-partner-portal-publish-managed-app/publish_flow.png)

Gehen wir nun einmal näher auf diesen Vorgang ein, um zu verstehen, was während der einzelnen Schritte passiert, und die Aktionselemente in diesem Prozess, um sicherzustellen, dass Ihr Angebot weiterverarbeitet wird.

<a name="publishing-process"></a>Veröffentlichungsvorgang 
------------------

Um den Veröffentlichungsvorgang zu starten, klicken Sie auf der Registerkarte **Editor** auf \"Veröffentlichen\".

![Angebotsaktivierungssequenz 1 – Veröffentlichen](./media/cloud-partner-portal-publish-managed-app/newOffer_managedApp_publish.png)

Auf der Registerkarte **Status** sehen Sie die Veröffentlichungsschritte und in welchem Schritt sich das Angebot befindet.

![Angebotsaktivierungssequenz 2 – Workflow](./media/cloud-partner-portal-publish-managed-app/newOffer_managedApp_workflow.png)

Während des Veröffentlichungsprozesses können Sie sich auch jederzeit anmelden und auf die Registerkarte **Alle Angebote** klicken, um den neuesten Status für Ihre Angebote einzusehen. Sie können direkt auf den Status Ihres Angebots klicken, um ausführliche Informationen darüber zu erhalten, wo sich Ihr Angebot im Veröffentlichungsvorgang befindet.

> [!WARNING]
> Wenn die Zuordnung der Nutzung durch Kunden nicht aktiviert ist, erhalten Sie während der Veröffentlichung eine Fehlermeldung, die in etwa wie folgt lautet: „Die Zuordnung der Nutzung durch Kunden von Azure-Partnern fehlt in mindestens einer Azure Resource Manager-Vorlage. Fügen Sie zur Problembehebung der Azure Resource Manager-Vorlage in den „maintemplate.JSON“-Paketen eine Nachverfolgungs-GUID für den folgenden Plan hinzu: Servicenow. Weitere Informationen finden Sie unter http://aka.ms/customerusageattribution.” 

Gehen wir nun die einzelnen Veröffentlichungsschritte durch, um zu besprechen, was während der einzelnen Schritte passiert und wie viel Zeit Sie für jeden Schritt einplanen sollten.

### <a name="validate-prerequisites-1-day"></a>Überprüfen der Voraussetzungen (\<1 Tag) 

Wenn Sie auf \"Veröffentlichen\" klicken, wird eine automatisierte Prüfung durchgeführt, um sicherzustellen, dass Sie alle erforderlichen Felder in Ihrem Angebot ausgefüllt haben. Wenn eines der Felder nicht ausgefüllt sein sollte, wird neben dem jeweiligen Feld eine Warnung angezeigt. Sie müssen es dann korrekt ausfüllen und erneut auf \'Veröffentlichen\' klicken.

Wenn Sie diesen Schritt korrekt ausgeführt haben, wird eine Popupmeldung angezeigt, in der Sie gebeten werden, eine E-Mail-Adresse anzugeben. Über diese E-Mail-Adresse erhalten Sie Benachrichtigungen zum Veröffentlichungsstatus für die verbleibenden Phasen des Veröffentlichungsvorgangs. Wenn Sie Ihre E-Mail-Adresse abgesendet haben, ist dieser Schritt abgeschlossen.

![Angebotsaktivierungssequenz 1 – Veröffentlichen](./media/cloud-partner-portal-publish-managed-app/newOffer_managedApp_publish1.png)

### <a name="certification-5-days"></a>Zertifizierung (\<5 Tage) 

In diesem Schritt werden verschiedene Tests durchgeführt, um sicherzustellen, dass Ihr Azure-Anwendungspaket die Anforderungen für Azure Certified erfüllt.

Da dieser Schritt mehrere Tage in Anspruch nehmen kann, können Sie sich vom Cloud Partner-Portal abmelden. Wir senden Ihnen eine E-Mail-Benachrichtigung, falls Fehler aufgetreten sind. Wenn alle Anforderungen erfolgreich bestanden wurden, wird der Vorgang automatisch mit dem Bereitstellungsschritt fortgesetzt.

### <a name="packaging-and-lead-generation-registration-1-hour"></a>Paketerstellung und Registrierung von generierten Leads (\<1 Stunde) 

In dieser Phase werden technische und Marketinginhalte für die Zusammenstellung der Produktseite auf der Website kombiniert.

### <a name="offer-available-in-preview"></a>Verfügbarkeit des Angebots in der Vorschau 

Sie erhalten eine E-Mail-Benachrichtigung, die darauf hinweist, dass Ihr Angebot die Schritte, die für den Zugriff auf das Angebot in der Vorschau erforderlich sind, erfolgreich durchlaufen hat. In diesem Schritt sollten Sie eine Vorschau Ihres Angebots erstellen und sicherstellen, dass alles korrekt ist. Überprüfen Sie, ob Ihre VM ordnungsgemäß in der Stagingumgebung bereitgestellt ist.

Nur Abonnements in der Whitelist können diese Überprüfung ausführen.\*

### <a name="publisher-sign-out"></a>Bestätigung durch den Herausgeber 

Nachdem Sie überprüft haben, ob alles seine Richtigkeit hat und ordnungsgemäß in der Vorschau funktioniert, können Sie das Angebot nun live schalten. Klicken Sie auf der Registerkarte „Status“ auf „Live schalten“, um mit den Schritten zum Aktivieren Ihres Angebot für die Produktion und auf der Website zu beginnen. Nachdem Sie auf „Live schalten“ geklickt haben, dauert es in der Regel mehrere Stunden, bis das Angebot auf der Website live geschaltet wird. Sobald Ihr Angebot offiziell auf der Website live geschaltet ist, senden wir Ihnen eine E-Mail-Benachrichtigung.

![Angebotsaktivierungssequenz 6 – Live schalten](./media/cloud-partner-portal-publish-managed-app/newOffer_managedApp_goLive.png)

### <a name="microsoft-review"></a>Prüfung durch Microsoft 

Wenn Sie fertig sind und auf \"Live schalten\" klicken, wird eine Website in Visual Studio Team Services erstellt und anschließend ein Code Review des Paketinhalts seitens Microsoft durchgeführt. Beim Code Review werden bewährte Muster und Methoden für die Vorlagenerstellung sowie Tipps und Tricks behandelt, um die Ressourcenerstellung zu optimieren. Wenn der Herausgeber im Zuge des Feedbacks Änderungen an den Dateien vornehmen muss, muss der Veröffentlichungsprozess wiederholt werden. Die aktuelle Veröffentlichung wird abgebrochen. Sie müssen die Feedbackkommentare dann beheben und den Veröffentlichungsvorgang wiederholen.

### <a name="live"></a>Live

Ihr Angebot ist nun im Azure Marketplace und im Azure-Portal live geschaltet. Ihre Kunden können Ihre verwaltete Azure-Anwendung in ihren Azure-Abonnements anzeigen und bereitstellen. Sie können auf die Registerkarte „Alle Angebote“ klicken und den in der rechten Spalte aufgeführten Status für all Ihre Angebote einsehen. Um ausführliche Informationen zum Status des Veröffentlichungsvorgangs für Ihr Angebot zu sehen, können Sie auf den Status klicken.

### <a name="error-handling"></a>Fehlerbehandlung 

Wenn ein Fehler festgestellt wird, erhalten Sie eine E-Mail-Benachrichtigung, die Sie darüber in Kenntnis setzt, dass bei den Anweisungen zu den nächsten Schritten ein Fehler aufgetreten ist. Fehler können auch durch Klicken auf die Registerkarte „Status“ jederzeit während dieses Vorgangs angezeigt werden. Es wird angezeigt, an welcher Stelle im Vorgang der Fehler aufgetreten ist, sowie die Fehlermeldung, die angibt, welche Fehler zu beheben sind.

Wenn während des Veröffentlichungsvorgangs Fehler auftreten, müssen Sie diese beheben und anschließend auf \'Veröffentlichen\' klicken, um den Vorgang zu wiederholen. Nach jeder Fehlerbehebungsmaßnahme müssen Sie bei der erneuten Veröffentlichung am Anfang der Veröffentlichungsschritte bei der Überprüfung der Voraussetzungen beginnen.

Wenn Sie Probleme bei der Behebung eines Fehlers haben, sollten Sie eine Supportanfrage öffnen, um Unterstützung von unseren Supportmitarbeitern zu erhalten.

### <a name="canceling-the-publishing-request"></a>Abbrechen von Veröffentlichungsanforderungen

Es kann vorkommen, dass Sie einen Veröffentlichungsvorgang in die Wege leiten und Ihre Anforderung abbrechen müssen. Sie können eine Veröffentlichungsanforderung erst dann abbrechen, wenn die Veröffentlichungsanforderung den Schritt zum Einholen der Genehmigung des Herausgebers erreicht hat. Klicken Sie zum Abbrechen auf \'Veröffentlichen abbrechen\'. Der Veröffentlichungsstatus wird auf Schritt 1 zurückgesetzt. Um die Veröffentlichung zu wiederholen, sollten Sie auf „Veröffentlichen“ klicken und die Schritte im Status befolgen.
