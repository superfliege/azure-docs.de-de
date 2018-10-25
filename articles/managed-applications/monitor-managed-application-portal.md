---
title: Verwenden des Azure-Portals zum Überwachung einer verwalteten Anwendung | Microsoft-Dokumentation
description: Zeigt, wie das Azure-Portal verwendet wird, um die Verfügbarkeit und Warnungen einer verwalteten Anwendung zu überwachen.
services: managed-applications
author: tfitzmac
ms.service: managed-applications
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 10/04/2018
ms.author: tomfitz
ms.openlocfilehash: ea9f55d7c6002aaba2fd4fdc2a76b93f98e1d6b0
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48806284"
---
# <a name="monitor-a-deployed-instance-of-a-managed-application"></a>Überwachen einer bereitgestellten Instanz einer verwalteten Anwendung

Nachdem Sie eine verwaltete Anwendung in Ihrem Azure-Abonnement bereitgestellt haben, können Sie den Status der Anwendung überprüfen. Dieser Artikel beschreibt die Optionen im Azure-Portal zum Überprüfen des Status. Sie können die Verfügbarkeit der Ressourcen in einer verwalteten Anwendung überwachen. Sie können auch Warnungen einrichten und anzeigen.

## <a name="view-resource-health"></a>Anzeigen der Ressourcenintegrität

1. Wählen Sie Ihre verwaltete Anwendungsinstanz aus.

   ![Auswählen der verwalteten Anwendung](./media/monitor-managed-application-portal/select-managed-application.png)

1. Wählen Sie **Ressourcenintegrität** aus.

   ![Auswählen der Ressourcenintegrität](./media/monitor-managed-application-portal/select-resource-health.png)

1. Zeigen Sie die Verfügbarkeit der Ressourcen in einer verwalteten Anwendung an.

   ![Anzeigen der Ressourcenintegrität](./media/monitor-managed-application-portal/view-health.png)

## <a name="view-alerts"></a>Anzeigen von Warnungen

1. Wählen Sie **Warnungen**.

   ![Auswählen von Warnungen](./media/monitor-managed-application-portal/select-alerts.png)

1. Wenn Sie die Warnungsregeln konfiguriert haben, werden Informationen zu den ausgelösten Warnungen angezeigt.

   ![Anzeigen von Warnungen](./media/monitor-managed-application-portal/view-alerts.png)

1. Wählen Sie zum Hinzufügen von Warnungsregeln **+ Neue Warnungsregel** aus.

   ![Erstellen einer Warnung](./media/monitor-managed-application-portal/create-new-alert.png)

Sie können Warnungen für Ihre verwaltete Anwendungsinstanz oder die Ressourcen in der verwalteten Anwendung erstellen. Weitere Informationen zum Erstellen von Warnungen finden Sie unter [Überblick über Warnungen in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-alerts.md).

## <a name="next-steps"></a>Nächste Schritte

* Beispiele zu verwalteten Anwendungen finden Sie unter [Sample projects for Azure managed applications](sample-projects.md) (Beispielprojekte für verwaltete Azure-Anwendungen).
* Informationen zum Bereitstellen einer verwalteten Anwendung finden Sie unter [Bereitstellen einer Dienstkatalog-App über das Azure-Portal](deploy-service-catalog-quickstart.md).