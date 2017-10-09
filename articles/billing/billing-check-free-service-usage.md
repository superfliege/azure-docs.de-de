---
title: "Überwachen und Nachverfolgen der Verwendung kostenloser Dienste – Azure | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie die Nutzung von kostenlosen Diensten überprüfen. Verwenden Sie das Azure-Portal und die Nutzungs-CSV-Datei."
services: 
documentationcenter: 
author: amberbhargava
manager: amberb
editor: 
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/25/2017
ms.author: amberb
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: e70a2378d1eda6aac01d5f5e5e449bb88f359dc6
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
# <a name="check-usage-of-free-services-included-with-your-azure-free-account"></a>Überprüfen der Verwendung der kostenlosen Dienste, die in Ihrem kostenlosen Azure-Konto enthalten sind 

Ihnen werden keine Gebühren für kostenlose Dienste, die in Ihrem kostenlosen Azure-Konto enthalten sind, in Rechnung gestellt, solange Sie die Grenzwerte dieser Dienste nicht überschreiten. Um die Grenzwerte einzuhalten, können Sie entweder das Azure-Portal oder die Nutzungsdatendatei zum Überwachen und Nachverfolgen der Verwendung kostenloser Dienste nutzen. 

## <a name="check-usage-on-the-azure-portal"></a>Überprüfen der Nutzung im Azure-Portal

1.  Melden Sie sich beim [Azure-Portal]( http://portal.azure.com)an.

2.  Wählen Sie unten im linken Navigationsbereich **Weitere Dienste**.

3.  Wählen Sie **Abonnements**.

4.  Wählen Sie das Abonnement aus, das Sie erstellt haben, als Sie sich für das kostenlose Konto registriert haben.

    ![Screenshot mit allen Abonnements](./media/billing-check-usage-of-free-services/select-free-account-subscription.png)

5.  Der Übersichtsabschnitt zeigt grundlegende Informationen zu Ihrem Abonnement an, z.B. Abonnement-ID, Angebotstyp und Abonnementnamen. Sie erhalten auch Informationen zum Ablauf des Guthabens auf Ihrem kostenlosen Konto.

    ![Screenshot mit grundlegenden Abonnementinformationen](./media/billing-check-usage-of-free-services/subscription-essential-information.png)

6.  Scrollen Sie nach unten, um Informationen zu Ihren aktuellen und prognostizierten Kosten anzuzeigen. Die Kosten beinhalten die Nutzung der Dienste, die nicht in Ihrem kostenlosen Konto enthalten sind, und Nutzung, die die Grenzwerte der kostenlosen Dienste überschreitet. 

    ![Screenshot mit Abonnementkosteninformationen](./media/billing-check-usage-of-free-services/subscription-cost-information.png)

7.  Der letzte Teil des Übersichtsabschnitts enthält eine Tabelle zur Verwendung der kostenlosen Dienste. 

    ![Screenshot mit Verwendung der kostenlosen Dienste](./media/billing-check-usage-of-free-services/subscription-usage-free-services.png)

    Die Tabelle umfasst die folgenden Spalten:

* **Name der Verbrauchseinheit:** Gibt die Maßeinheit für die genutzte Verbrauchseinheit an. Informationen zur Zuordnung von Diensten zu Verbrauchseinheiten finden Sie unter [Understand free service to meter mapping](billing-understand-free-service-meter-mapping.md) (Grundlegendes zur Zuordnung kostenloser Dienste zu Verbrauchseinheiten). 
* **Nutzung/Limit:** Nutzung und Grenzwert für die Verbrauchseinheit für den aktuellen Monat. Diese Informationen finden Sie auch in der Statusleiste.
* **Status:** Verwendungsstatus der Verbrauchseinheit. Basierend auf Ihrem Nutzungsmuster können Sie einen dieser Status besitzen.
  * **Nicht in Verwendung:** Sie haben diese Verbrauchseinheit nicht verwendet, oder die Verwendung der Verbrauchseinheit hat nicht das Abrechnungssystem erreicht.
  * **Überschritten am \<Datum>:** Sie haben den Grenzwert für die Verbrauchseinheit am \<Datum> überschritten.
  * **Überschreitung unwahrscheinlich:** Es ist unwahrscheinlich, dass Sie das Limit für diese Verbrauchseinheit überschreiten.
  * **Überschreitung am \<Datum>:** Sie werden den Grenzwert für die Verbrauchseinheit voraussichtlich am \<Datum> überschreiten.


## <a name="check-usage-through-the-usage-file"></a>Überprüfen der Verwendung mit der Nutzungsdatendatei

Ihre Nutzungsdatendatei enthält detaillierte Informationen über Ihr Azure-Abonnement. Sie können Ihre monatliche und tägliche Nutzungsdatendatei vom Azure-Kontocenter herunterladen. Informationen zum Herunterladen der Nutzungsdatendatei und erforderlichen Zugriffsrechten finden Sie unter [Herunterladen oder Anzeigen Ihrer Azure-Rechnungen und täglichen Nutzungsdaten](billing-download-azure-invoice-daily-usage-date.md). Weitere Informationen zu Spalten in der Nutzungsdatendatei finden Sie unter [Grundlegendes über Benennungen zu den Gebühren in der Datei mit ausführlichen Nutzungsdaten zu Microsoft Azure](billing-understand-your-usage.md). 

Die Nutzungsdatendatei enthält Nutzungsinformationen zu kostenlosen und kostenpflichtigen Diensten. An das Ende des Namens kostenloser Dienstverbrauchseinheiten ist **Kostenlos** angefügt. Um kostenlose Verbrauchseinheiten zu suchen, öffnen Sie die Datei in Excel, und filtern Sie die **Spalte „Kategorie der Verbrauchseinheit“** nach Zellen, die den Text **– Kostenlos** enthalten (Textfilter &rarr; „Enthält“-Filter verwenden)&nbsp;.

![Screenshot mit Verwendung der kostenlosen Dienste](./media/billing-check-usage-of-free-services/free-services-usage-csv.png)


## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Support kontaktieren

[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls Sie weitere Hilfe benötigen, um das Problem schnell beheben zu lassen.

