---
title: Azure Data Lake Analytics-Kontingentgrenzen
description: Erfahren Sie, wie Sie Kontingentgrenzen in ADLA-Konten (Azure Data Lake Analytics) anpassen und erhöhen.
services: data-lake-analytics
keywords: Azure Data Lake Analytics
documentationcenter: ''
author: omidm1
editor: omidm1
ms.assetid: 49416f38-fcc7-476f-a55e-d67f3f9c1d34
ms.service: data-lake-analytics
ms.topic: article
ms.workload: big-data
ms.date: 03/15/2018
ms.author: omidm
ms.openlocfilehash: 22774511720173915207da80a6ca33d5dbc83e19
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/17/2018
---
# <a name="azure-data-lake-analytics-quota-limits"></a>Azure Data Lake Analytics-Kontingentgrenzen

Erfahren Sie, wie Sie Kontingentgrenzen in ADLA-Konten (Azure Data Lake Analytics) anpassen und erhöhen. Diese Grenzen zu kennen, kann dabei helfen, das Verhalten von U-SQL-Aufträgen besser zu verstehen. Kontingentgrenzen sind nicht fix. Wenden Sie sich an den Azure-Support, um diese Höchstgrenzen zu erhöhen.

## <a name="azure-subscriptions-limits"></a>Azure-Abonnementgrenzen

**Maximale Anzahl von ADLA-Konten pro Abonnement:** 5.

Dies ist die maximale Anzahl von ADLA-Konten, die Sie pro Abonnement und Region erstellen können. Beim Versuch, ein sechstes ADLA-Konto zu erstellen, erhalten Sie folgenden Fehler : Sie haben die maximale Anzahl von Data Lake Analytics-Konten (5) in „Region“ für das Abonnement „Name“ erreicht. In diesem Fall können Sie bei Bedarf eine andere Region auswählen, oder unbenutzte ADLA-Konten in derselben Region löschen, oder den Azure-Support kontaktieren, indem Sie ein [Supportticket öffnen](#increase-maximum-quota-limits), um eine Kontingenterhöhung zu beantragen.

## <a name="adla-account-limits"></a>ADLA-Kontobegrenzungen

**Maximale Anzahl von Analytics-Einheiten (AUs) pro Konto:** 250.

Dies ist die maximale Anzahl von AUs, die in Ihrem Konto gleichzeitig ausgeführt werden können. Wenn die Gesamtzahl von in allen Aufträgen ausgeführten AUs diesen Grenzwert überschreitet, werden neuere Aufträge automatisch in die Warteschlange eingereiht. Beispiel: 

* Sie führen einen Auftrag mit 250 AUs aus. Wenn Sie einen zweiten Auftrag übermitteln, wartet dieser in der Auftragswarteschlange, bis der erste Auftrag abgeschlossen ist.
* Wenn bereits fünf Aufträge mit jeweils 50 AUs ausgeführt werden und Sie einen sechsten Auftrag übermitteln, der 20 AUs benötigt, wartet dieser Auftrag in der Auftragswarteschlange, bis 20 AUs verfügbar sind.

**Maximale Anzahl gleichzeitiger U-SQL-Aufträge pro Konto:** 20.

Dies ist die maximale Anzahl von Aufträgen, die in Ihrem Konto gleichzeitig ausgeführt werden können. Aufträge, die diesen Wert überschreiten, werden automatisch in die Warteschlange eingereiht.

## <a name="adjust-adla-quota-limits-per-account"></a>Anpassen von ADLA-Kontingentgrenzen pro Konto

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Wählen Sie ein vorhandenes ADLA-Konto aus.
3. Klicken Sie auf **Eigenschaften**.
4. Passen Sie **Parallelität** und **Gleichzeitige Aufträge** Ihren Anforderungen entsprechend an.

    ![Portalseite für Azure Data Lake Analytics](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-properties.png)

## <a name="increase-maximum-quota-limits"></a>Erhöhen der Kontingentobergrenzen

1. Öffnen Sie eine Supportanfrage im Azure-Portal.

    ![Portalseite für Azure Data Lake Analytics](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-help-support.png)

    ![Portalseite für Azure Data Lake Analytics](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request.png)
2. Wählen Sie als Problemtyp **Kontingent** aus.
3. Wählen Sie Ihr **Abonnement** aus (stellen Sie sicher, dass es kein Testabonnement ist).
4. Wählen Sie als Kontingenttyp **Data Lake Analytics** aus.

    ![Portalseite für Azure Data Lake Analytics](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request-basics.png)

5. Erläutern Sie auf der Problemseite Ihre Anforderung für die Erhöhung der Grenze, und geben Sie **Details** an, aus welchem Grund Sie diese zusätzliche Kapazität benötigen.

    ![Portalseite für Azure Data Lake Analytics](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request-details.png)

6. Überprüfen Sie Ihre Kontaktinformationen, und erstellen Sie die Supportanfrage.

Microsoft prüft Ihre Anforderung und versucht, Ihre geschäftlichen Anforderungen so bald wie möglich zu erfüllen.

## <a name="next-steps"></a>Nächste Schritte

* [Übersicht über Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Verwalten von Azure Data Lake Analytics mithilfe von Azure PowerShell](data-lake-analytics-manage-use-powershell.md)
* [Überwachung und Problembehandlung von Azure Data Lake Analytics-Aufträgen mithilfe des Azure-Portals](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
