---
title: Azure Container Registry-SKUs
description: "Vergleich: die verschiedenen in Azure Container Registry verfügbaren Dienstebenen"
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: mmacy
ms.service: container-registry
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/16/2017
ms.author: stevelas
ms.openlocfilehash: dae97084bdaab77efd38169cdf7e70c827b0b5ab
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2017
---
# <a name="azure-container-registry-skus"></a>Azure Container Registry-SKUs

Azure Container Registry (ACR) steht in verschiedenen Dienstebenen, sogenannten SKUs, zur Verfügung. Durch diese SKUs können Preise im Voraus eingeschätzt werden. Darüber hinaus bieten sie mehrere Optionen zum Verwenden Ihrer privaten Docker-Registrierung in Azure. Wenn Sie sich für eine SKU auf höherer Ebene entscheiden, können Sie von einer höheren Leistung und Skalierung profitieren. Allerdings bieten alle SKUs die gleichen Programmfunktionen, sodass ein Entwickler mit Basic beginnen und dann mit steigender Nutzung zu Standard und Premium wechseln kann.

## <a name="basic"></a>Basic
Ein kostenoptimierter Einstiegspunkt für Entwickler, die sich mit Azure Container Registry vertraut machen. Basic-Registrierungen verfügen über die gleichen Programmfunktionen wie Standard- und Premium-Registrierungen (Azure Active Directory-Authentifizierungsintegration, Löschen von Images sowie Webhooks). Allerdings bestehen Einschränkungen bei Größe und Nutzung.

## <a name="standard"></a>Standard
Standard-Registrierungen bieten die gleichen Funktionen wie Basic. In Standard gibt es jedoch erweiterte Speicherlimits und einen erhöhten Imagedurchsatz. Standard-Registrierungen erfüllen üblicherweise die Bedürfnisse der meisten Produktionsszenarios.

## <a name="premium"></a>Premium
Premium-Registrierungen sind die Grenzwerte für den Speicher und gleichzeitige Vorgänge noch höher, sodass Szenarios mit großen Volumen möglich sind. Zusätzlich zu Funktionen für einen erhöhten Imagedurchsatz enthält Premium Features wie die [Georeplikation](container-registry-geo-replication.md) zum regionsübergreifenden Verwalten einer Registrierung, sodass die Registrierung jeder Bereitstellung netzwerknah ist.

## <a name="classic"></a>Klassisch
Durch die klassische Registrierungs-SKU war das erstmalige Release des Azure Container Registry-Diensts in Azure möglich. Klassische Registrierungen werden von einem Speicherkonto unterstützt, das von Azure in Ihrem Abonnement erstellt wird, das dazu führt, dass ACR nur eingeschränkt allgemeine Funktionen wie einen erhöhten Durchsatz oder die Georeplikation bieten kann. Durch diese eingeschränkten Funktionen wird die Unterstützung für die klassische SKU in naher Zukunft eingestellt.

> [!NOTE]
> Wegen der geplanten Einstellung der Unterstützung für die klassische Registrierungs-SKU wird empfohlen, Basic, Standard oder Premium für alle neuen Registrierungen zu verwenden. Informationen zum Umstellen Ihrer vorhandenen klassischen Registrierung finden Sie unter [Wechseln von SKUs](#changing-skus).
>

## <a name="registry-sku-feature-matrix"></a>Featurematrix der Registrierungs-SKUs

In der folgenden Tabelle werden die Features und Grenzwerte der Dienstebenen Basic, Standard und Premium dargestellt.

[!INCLUDE [container-instances-limits](../../includes/container-registry-limits.md)]

## <a name="manage-registry-size"></a>Verwalten von Registrierungsgrößen
Die Speichereinschränkungen jeder SKU entsprechen einem der folgenden Szenarios: Basic für den Anfang, Standard für den Großteil der Produktions-Apps und Premium für Hyperskalierungsleistung und die [Georeplikation](container-registry-geo-replication.md). Während der gesamten Dauer Ihrer Registrierung sollten Sie die Größe durch regelmäßiges Löschen von ungenutztem Inhalt verwalten.

Sie können die aktuelle Nutzung einer Registrierung in der **Übersicht** der Containerregistrierung im Azure-Portal finden:

![Informationen zur Registrierungsnutzung im Azure-Portal](media/container-registry-skus/registry-overview-quotas.png)

Sie können die Größe Ihrer Registrierung verwalten, indem Sie Repositorys im Azure-Portal löschen.

Klicken Sie unter **Dienste** auf **Repositorys**, und klicken Sie dann mit der rechten Maustaste auf das Repository, das Sie löschen möchten. Klicken Sie anschließend auf **Löschen**.

![Löschen eines Repositorys aus einem Verzeichnis im Azure-Portal](media/container-registry-skus/delete-repository-portal.png)

## <a name="changing-skus"></a>Wechseln von SKUs

Sie können die SKU einer Registrierung im Azure-Portal wechseln.

Klicken Sie in der **Übersicht** der Registrierung im Azure-Portal auf **Aktualisieren**, und klicken Sie anschließend auf die neue **SKU** im Dropdownmenü.

![Aktualisieren der Containerregistrierung-SKU im Azure-Portal](media/container-registry-skus/update-registry-sku.png)

## <a name="changing-from-classic"></a>Wechseln von der klassischen Registrierung
Wenn Sie von einer klassischen Registrierung zu Basic, Standard oder Premium wechseln, kopiert Azure vorhandene Containerimages aus dem verknüpften Speicherkonto in Ihrem Abonnement in ein von Azure verwaltetes Speicherkonto. Dieser Vorgang kann eine Weile dauern.

Während des Wechsels funktioniert `docker pull` weiterhin. `docker push` wird allerdings blockiert, bis der Wechsel vollständig durchgeführt wurde.

Sobald er abgeschlossen wurde, wird das Speicherkonto des Abonnements nicht mehr von ACR verwendet.

### <a name="why-change-from-classic-to-basic-standard-or-premium"></a>Gründe für den Wechsel von Klassisch zu Basic, Standard oder Premium

Aufgrund der eingeschränkten Funktionen der klassischen Registrierung wird empfohlen, dass Sie von ihrer klassischen zu einer Basic-, Standard- oder Premium-Registrierung wechseln. Die SKUs auf höherer Ebene integrieren die Registrierung nahtloser mit den Funktionen von Azure. Diese Funktionen umfassen unter anderem:

* Azure Active Directory-Integration für die einzelne Authentifizierung (weitere Informationen unter [az acr login](/cli/azure/acr?view=azure-cli-latest#az_acr_login))
* Unterstützung für das Löschen von Images und Tags
* [Georeplikation](container-registry-geo-replication.md)
* [Webhooks](container-registry-webhook.md)

Die klassische Registrierung ist vor allem von einem Speicherkonto abhängig, das von Azure automatisch beim Erstellen der Registrierung bereitgestellt wurde. Basic-, Standard- und Premium-SKUs verwenden hingegen *verwalteten Speicher*. Dies bedeutet, dass Azure den Speicher für Ihre Images für Sie transparent verwaltet – und dafür wird kein separates Speicherkonto in Ihrem Abonnement erstellt.

Hier sind einige Vorteile von verwaltetem Speicher, die von Basic-, Standard- und Premium-Registrierungen bereitgestellt werden:

* Containerimages werden [im Ruhezustand verschlüsselt](../storage/common/storage-service-encryption.md).
* Images werden mit einem [georedundanten Speicher](../storage/common/storage-redundancy.md#geo-redundant-storage) gespeichert, sodass die Sicherung Ihrer Images durch die Replikation in mehreren Regionen gewährleistet ist.
* Die Möglichkeit, [zwischen SKUs zu wechseln](#changing-skus), sodass der Durchsatz mit einer höheren SKU steigt. Jede SKU von ACR erfüllt Ihre Bedürfnisse beim Durchsatz Ihrem Bedarf entsprechend. Die zugrunde liegende Implementierung des erwünschten Durchsatzes von ACR wird als *Absicht* (durch das Wählen einer höheren SKU) ausgedrückt, ohne dass Sie die genauen Angaben der Implementierung verwalten müssen.

## <a name="pricing"></a>Preise

Informationen zu Preisen für jede SKU von Azure Container Registry finden Sie unter [Preise von Azure Container Registry](https://azure.microsoft.com/pricing/details/container-registry/).

## <a name="next-steps"></a>Nächste Schritte

**Azure Container Registry – Roadmap**

Besuchen Sie [ACR Roadmap](https://aka.ms/acr/roadmap) auf GitHub, wo Sie Informationen zu zukünftigen Features des Diensts finden.

**Azure Container Registry – UserVoice**

Unter [ACR UserVoice](https://feedback.azure.com/forums/903958-azure-container-registry) können Sie neue Featurevorschläge einsenden oder für diese abstimmen.