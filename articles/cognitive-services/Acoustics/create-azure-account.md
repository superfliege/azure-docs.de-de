---
title: Einrichten von Azure-Konten für Acoustics – Cognitive Services
description: Befolgen Sie diese Anleitung zum Einrichten von Azure Batch- und Storage-Konten, die für die Verwendung mit Acoustics erforderlich sind.
services: cognitive-services
author: ashtat
manager: noelc
ms.service: cognitive-services
ms.component: acoustics
ms.topic: article
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: d5e78df2cb17e8275aef3694dda90a705ef4bdaa
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2018
ms.locfileid: "40181227"
---
# <a name="create-an-azure-batch-account"></a>Erstellen eines Azure Batch-Kontos
Befolgen Sie diese Anleitung zum Einrichten von Azure Batch- und Storage-Konten, die für die Verwendung mit Acoustics erforderlich sind. Informationen zum Unity-Plug-In, das im Rahmen von Project Acoustics entwickelt wurde, finden Sie unter [Was ist Acoustics](what-is-acoustics.md). Informationen dazu, wie Acoustics in das Unity-Projekt eingebunden wird, finden Sie unter [Erste Schritte](getting-started.md).  

## <a name="get-an-azure-subscription"></a>Erhalten eines Azure-Abonnements
Zum Einrichten von Batch- und Storage-Konten ist ein [Azure-Abonnement](https://azure.microsoft.com/free/) erforderlich. Wenn Sie sich zum ersten Mal anmelden, stellt Azure einige zeitlich begrenzte kostenlose Ressourcen und ein Guthaben in Höhe von 200 USD bereit.

## <a name="create-azure-batch-and-storage-accounts"></a>Erstellen von Azure Batch- und Azure Storage-Konten
Befolgen Sie als Nächstes [diese Anweisungen](https://docs.microsoft.com/azure/batch/batch-account-create-portal), um Ihre Azure Batch-Konten und die damit verbundenen Azure Storage-Konten einzurichten.

Wählen Sie für Batch- und Storage-Konten Standardoptionen aus:
  
  ![Neues Batch-Konto](media/NewBatchAccountCreate.png)

  ![Neues Storage-Konto](media/BatchStorageAccountCreate.png)

Das Bereitstellen der Konten durch Azure dauert einige Minuten. Halten Sie nach einer Benachrichtigung zum Abschluss in der oberen rechten Ecke des Portals Ausschau.
  
  ![Bereitgestellte Konten](media/BatchAccountsDeployNotification.png)

Ihre Konten sollten nun in Ihrem Dashboard angezeigt werden.
  
  ![Dashboard des Portals](media/AzurePortalDashboard.png)

## <a name="set-up-acoustics-bake-ui-with-azure-credentials"></a>Einrichten der Benutzeroberfläche für den Acoustics-Bake-Vorgang mit Azure-Anmeldeinformationen
Klicken Sie im Dashboard auf den Link für das Batch-Konto und anschließend auf der Seite „Batch-Konto“ auf den Link **Keys** (Schlüssel), um auf Ihre Anmeldeinformationen zuzugreifen.
  
  ![Link für Batch-Schlüssel](media/BatchAccessKeys.png)

  ![Anmeldeinformationen für Batch-Konto](media/BatchKeysInfo.png)

Klicken Sie auf der Seite auf den Link **Storage-Konto**, um auf die Anmeldeinformationen Ihres Azure Storage-Kontos zuzugreifen.
  
  ![Anmeldeinformationen des Speicherkontos](media/StorageKeysInfo.png)

Geben Sie diese Anmeldeinformationen auf der Registerkarte „Bake“ (Bake-Vorgang) ein wie in der [exemplarischen Vorgehensweise für Baking-UI](bake-ui-walkthrough.md) beschrieben.

## <a name="node-types-and-region-support"></a>Knotentypen und Unterstützung für Regionen
Für Project Acoustics sind Compute-optimierte Azure VM-Knoten der F- und H-Serie erforderlich, die möglicherweise nicht in allen Azure-Regionen unterstützt werden. Überprüfen Sie [diese Tabelle](https://azure.microsoft.com/global-infrastructure/services), um sicherzustellen, dass Sie den richtigen Ort für Ihr Batch-Konto auswählen. Derzeit werden VMs der H-Serie in den Regionen „USA, Osten“, „USA, Norden-Mitte“, „USA, Süden-Mitte“, „USA, Westen“, „USA, Westen 2“, „Europa, Norden“, „Europa, Westen“ und „Japan, Westen“ unterstützt.

## <a name="upgrading-your-quota"></a>Upgraden des Kontingents
Azure Batch-Konten werden beim Erstellen des Kontos mit einem Limit von 20 Compute-Kernen bereitgestellt. Sie sollten dieses Limit erhöhen. Dadurch werden die Zeiten für den Bake-Vorgang verkürzt, da Sie die Acoustics-Workload auf mehreren Knoten (bis zur Anzahl von Testpunkten in Ihrer Szene) gleichzeitig verarbeiten können. Sie können eine Kontingenterhöhung anfordern, indem Sie auf Ihrer Azure Batch-Portalseite auf den Link **Kontingent** und anschließend auf **Kontingenterhöhung anfordern** klicken:

![Erhöhen des Azure-Kontingente](media/azurequotas.png)

## <a name="next-steps"></a>Nächste Schritte
* Erste Schritte mit der [Integration von Akustik in das Unity-Projekt](getting-started.md)
* Untersuchen der [Beispielszene](sample-walkthrough.md)

