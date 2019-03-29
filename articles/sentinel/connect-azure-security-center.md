---
title: Sammeln von Azure Security Center-Daten in der Vorschauversion von Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure Security Center-Daten in Azure Sentinel sammeln.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: d28c2264-2dce-42e1-b096-b5a234ff858a
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: f59c5f6a9f497a6420172996f9f327f16ffd26f9
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2019
ms.locfileid: "57242271"
---
# <a name="collect-data-from-azure-security-center"></a>Sammeln von Daten aus Azure Security Center

> [!IMPORTANT]
> Azure Sentinel ist derzeit als öffentliche Vorschauversion (Public Preview) verfügbar.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).



Mit Azure Sentinel können Sie Warnungen aus [Azure Security Center](../security-center/security-center-intro.md) sammeln und in Azure Sentinel streamen. 

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie Warnungen aus dem Azure Security Center exportieren möchten, müssen Sie ein Mitwirkender im Abonnement sein, dessen Protokolle Sie streamen.

- Sie müssen das Abonnement im [Azure Security Center-Standard-Tarif](../security-center/security-center-pricing.md) ausführen. Wenn dies nicht der Fall ist, [aktualisieren Sie Ihr Abonnement auf den Standard-Tarif](https://azure.microsoft.com/pricing/details/security-center/).

- Sie müssen sich als Benutzer anmelden, der über globale Administrator- oder Sicherheitsadministratorrechte für jedes Abonnement verfügt, das Sie verbinden möchten.


## <a name="connect-to-azure-security-center"></a>Herstellen einer Verbindung mit Azure Security Center

1. Wählen Sie in Azure Sentinel **Datenerfassung** aus, und klicken Sie dann auf die Kachel **Azure Security Center**.
1. Klicken Sie in der rechten Spalte neben jedem Abonnement, dessen Warnungen Sie in Azure Sentinel streamen möchten, auf **Verbinden**. Stellen Sie sicher, dass Sie jedes Abonnement auf den Azure Security Center-Standard-Tarif aktualisieren, um Warnungen an Azure Sentinel zu streamen.

3. Klicken Sie auf **Verbinden**.

4. Um das relevante Schema für die Azure Security Center-Warnungen in Log Analytics zu verwenden, suchen Sie nach **SecurityEvent**.

## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument wurde beschrieben, wie Sie Azure Security Center mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [ Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats.md).
