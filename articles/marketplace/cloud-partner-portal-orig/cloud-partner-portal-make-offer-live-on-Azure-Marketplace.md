---
title: Live-Schaltung Ihres Angebots zu virtuellen Maschinen im Azure Marketplace
description: Live-Schaltung Ihres Angebots zu virtuellen Maschinen im Azure Marketplace
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: qianw211
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: ff3ca665382efc9133a4c4ce3f3fde25c03c5e52
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48806740"
---
<a name="make-your-virtual-machine-offer-live-on-azure-marketplace"></a>Live-Schaltung Ihres Angebots zu virtuellen Maschinen im Azure Marketplace
=========================================================

Nachdem Sie alle Angebotsdetails ausgefüllt haben, können Sie nun Ihr Angebot veröffentlichen und im Azure Marketplace freischalten. Es gibt verschiedene Phasen, die das Angebot durchläuft. Stellen Sie sicher, dass Ihre Marketinginhalte und Ihr VM-Image die Qualitätsanforderungen erfüllen, damit sie als „Azure Certified“ eingestuft und auf der Website freigeschaltet werden können.

![Angebotsaktivierungssequenz 0](./media/cloud-partner-portal-offer-go-live-azure-marketplace/makeanofferlive.png)

Dieser Prozess wird nun im Detail vorgestellt, um besser zu verstehen, was bei jedem Schritt passiert. Im Verlauf dieses Prozesses müssen Sie reagieren, um sicherzustellen, dass es entsprechende Fortschritte für Ihr Angebot gibt.

<a name="publishing-process"></a>Veröffentlichungsvorgang
------------------

Klicken Sie auf der Registerkarte „Editor“ auf „Veröffentlichen“, um den Veröffentlichungsprozess zu starten.

![Angebotsaktivierungssequenz 1 – Veröffentlichen](./media/cloud-partner-portal-offer-go-live-azure-marketplace/publish.png)

Auf der Registerkarte „Status“ sehen Sie die Veröffentlichungsschritte und in welcher Vorgangsphase sich das Angebot befindet.

![Angebotsaktivierungssequenz 2 – Status](./media/cloud-partner-portal-offer-go-live-azure-marketplace/status.png)

Während des Veröffentlichungsprozesses können Sie sich auch jederzeit anmelden und auf die Registerkarte „Alle Angebote“ klicken, um den neuesten Status für irgendeines Ihrer Angebot einzusehen. Sie können direkt auf den Status Ihres Angebots klicken, um ausführliche Informationen darüber zu erhalten, wo sich Ihr Angebot im Veröffentlichungsvorgang befindet.

![Angebotsaktivierungssequenz 3 – Status aller Angebote](./media/cloud-partner-portal-offer-go-live-azure-marketplace/alloffersstatus.png)

Gehen wir nun die einzelnen Veröffentlichungsschritte durch, um zu besprechen, was während der einzelnen Schritte passiert und wie viel Zeit Sie für jeden Schritt einplanen sollten.

**Überprüfen der Voraussetzungen (\<1 Tag)**

Wenn Sie auf „Veröffentlichen“ klicken, wird eine automatisierte Prüfung durchgeführt, um sicherzustellen, dass Sie alle erforderlichen Felder in Ihrem Angebot ausgefüllt haben. Wenn eines der Felder nicht ausgefüllt sein sollte, wird neben dem jeweiligen Feld eine Warnung angezeigt. Sie müssen es dann korrekt ausfüllen und erneut auf „Veröffentlichen“ klicken.

Wenn Sie diesen Schritt korrekt ausgeführt haben, wird eine Popupmeldung angezeigt, in der Sie aufgefordert werden, eine E-Mail-Adresse anzugeben. Über diese E-Mail-Adresse erhalten Sie Benachrichtigungen zum Veröffentlichungsstatus für die verbleibenden Phasen des Veröffentlichungsvorgangs. Wenn Sie Ihre E-Mail-Adresse abgesendet haben, ist dieser Schritt abgeschlossen.

![Angebotsaktivierungssequenz 4 – Veröffentlichen Ihres Angebots](./media/cloud-partner-portal-offer-go-live-azure-marketplace/publishyouroffer.png)

**Zertifizierung (\<5 Tage)**

In diesem Schritt werden verschiedene Tests durchgeführt, um sicherzustellen, dass Ihr VM-Image die Anforderungen für Azure Certified erfüllt. Sämtliche Informationen, die Sie benötigen, um die Zertifizierungsanforderungen zu bestehen, finden Sie [hier](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation-prerequisites).

Da dieser Schritt mehrere Tage in Anspruch nehmen kann, können Sie sich vom Cloud Partner-Portal abmelden. Wir senden Ihnen eine E-Mail-Benachrichtigung, falls Fehler aufgetreten sind, die von Ihnen behoben werden müssen. Wenn alle Anforderungen erfolgreich bestanden wurden, wird der Vorgang automatisch mit dem Bereitstellungsschritt fortgesetzt.

**Bereitstellung (\<1 Tag)**

In dieser Phase werden Ihre Images in allen Azure-Rechenzentren weltweit repliziert. Dies kann bis zu einen Tag dauern.

**Paketerstellung und Registrierung von generierten Leads (\<1 Stunde)**

In dieser Phase werden technische und Marketinginhalte für die Zusammenstellung der Produktseite auf der Website kombiniert.

Wenn Sie die Leadgenerierungsfunktion konfiguriert haben, wird durch Senden eines Testleads an Ihr CRM zudem überprüft, ob Ihre CRM-Integration funktioniert. Nach Abschluss dieses Schritts wird Ihre CRM- oder Azure-Tabelle mit einem Satz falscher Daten ausgefüllt. Sämtliche Dokumentation zur Leadgenerierung finden Sie [hier](./cloud-partner-portal-get-customer-leads.md).

**Verfügbarkeit des Angebots in der Vorschau**

Sie erhalten eine E-Mail-Benachrichtigung, die darauf hinweist, dass Ihr Angebot die Schritte, die für den Zugriff auf das Angebot in der Vorschau erforderlich sind, erfolgreich durchlaufen hat. Bei diesem Schritt sollten Sie eine Vorschau von Ihrem Angebot anzeigen. Hierbei sollten Sie es auf seine Richtigkeit überprüfen und sicherstellen, dass Ihre VM korrekt in der Stagingumgebung bereitgestellt wurde.

**Nur Abonnements in der Whitelist können überprüft werden.**\*

**Einholen der Genehmigung des Herausgebers**

Nachdem Sie überprüft haben, ob alles seine Richtigkeit hat und ordnungsgemäß in der Vorschau funktioniert, können Sie das Angebot nun live schalten. Klicken Sie hierzu auf der Registerkarte „Status“ auf „Live schalten“, um mit den Schritten zum Aktivieren Ihres Angebot für die Produktion und auf der Website zu beginnen. Nachdem Sie auf „Live schalten“ geklickt haben, dauert es in der Regel mehrere Stunden, bis das Angebot auf der Website live geschaltet wird. Sobald Ihr Angebot offiziell auf der Website live geschaltet ist, senden wir Ihnen eine E-Mail-Benachrichtigung.

![Angebotsaktivierungssequenz 5 – Freischalten](./media/cloud-partner-portal-offer-go-live-azure-marketplace/golive.png)

**Live**

Ihr Angebot ist nun im Azure Marketplace und Azure-Portal live geschaltet. Kunden können dann Ihre virtuelle Maschine in ihren Azure-Abonnements anzeigen und bereitstellen. Sie können zu einem beliebigen Zeitpunkt auf die Registerkarte „Alle Angebote“ klicken und den in der rechten Spalte aufgeführten Status für all Ihre Angebote einsehen. Um ausführliche Informationen zum Status des Veröffentlichungsvorgangs für Ihr Angebot zu sehen, können Sie auf den Status klicken.

<a name="error-handling"></a>Fehlerbehandlung
--------------

Während des Veröffentlichungsvorgangs können eventuell Fehler auftreten. Wenn ein Fehler festgestellt wird, erhalten Sie eine E-Mail-Benachrichtigung, die Sie darüber in Kenntnis setzt, dass bei den Anweisungen zu den nächsten Schritten ein Fehler aufgetreten ist. Fehler können auch durch Klicken auf die Registerkarte „Status“ jederzeit während dieses Vorgangs angezeigt werden. Es wird angezeigt, an welcher Stelle im Vorgang der Fehler aufgetreten ist, sowie die Fehlermeldung, die angibt, welche Fehler zu beheben sind.

![Angebotsaktivierungssequenz 6 – Fehlermeldungen](./media/cloud-partner-portal-offer-go-live-azure-marketplace/errormessage.png)

Wenn während des Veröffentlichungsvorgangs Fehler auftreten, müssen Sie diese beheben und anschließend auf „Veröffentlichen“ klicken, um den Vorgang zu wiederholen. Nach jeder Fehlerbehebungsmaßnahme müssen Sie bei der erneuten Veröffentlichung am Anfang der Veröffentlichungsschritte bei der Überprüfung der Voraussetzungen beginnen.

Wenn Sie Probleme bei der Behebung eines Fehlers haben, sollten Sie eine Supportanfrage öffnen, um Unterstützung von unseren Supportmitarbeitern zu erhalten.

![Angebotsaktivierungssequenz 7 – Support anfordern](./media/cloud-partner-portal-offer-go-live-azure-marketplace/getsupport.png)

<a name="canceling-the-publishing-request"></a>Abbrechen von Veröffentlichungsanforderungen
--------------------------------

Es kann vorkommen, dass Sie einen Veröffentlichungsvorgang in die Wege leiten und Ihre Anforderung abbrechen müssen. Sie können eine Veröffentlichungsanforderung erst dann abbrechen, wenn die Veröffentlichungsanforderung den Schritt zum Einholen der Genehmigung des Herausgebers erreicht hat. Klicken Sie zum Abbrechen auf „Veröffentlichen abbrechen“. Der Veröffentlichungsstatus wird auf Schritt 1 zurückgesetzt. Um die Veröffentlichung zu wiederholen, sollten Sie auf „Veröffentlichen“ klicken und die Schritte im Status befolgen.

![Angebotsaktivierungssequenz 8 – Status](./media/cloud-partner-portal-offer-go-live-azure-marketplace/status5.png)

