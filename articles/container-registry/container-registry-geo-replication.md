---
title: Georeplikation einer Azure-Containerregistrierung
description: Erste Schritte beim Erstellen und Verwalten von Azure-Containerregistrierungen mit Georeplikation.
services: container-registry
documentationcenter: 
author: SteveLas
manager: balans
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-registry
ms.devlang: 
ms.topic: overview-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/24/2017
ms.author: stevelas
ms.custom: 
ms.openlocfilehash: 7a05f12e7873b8280dd737b008e186626017125e
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2017
---
# <a name="geo-replication-in-azure-container-registry"></a>Georeplikation in Azure Container Registry

Unternehmen, die eine lokale Umgebung oder ein Hotbackup wünschen, entscheiden sich für die Ausführung von Diensten in mehreren Azure-Regionen. Als bewährte Methode ermöglicht die Platzierung einer Containerregistrierung in jeder Region, in der Images ausgeführt werden, netzwerknahe Vorgänge, die schnelle und zuverlässige Übertragungen auf Image-Ebene gestatten.

Die Georeplikation ermöglicht einer Azure-Containerregistrierung, als zentrale Registrierung zu fungieren, die mehreren Regionen regionale Multimasterregistrierungen zur Verfügung stellt.

> [!IMPORTANT]
> Die Georeplikationsfunktion von Azure Container Registry befindet sich derzeit in der **Vorschauphase**. Vorschauversionen werden Ihnen zur Verfügung gestellt, wenn Sie die [zusätzlichen Nutzungsbedingungen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) akzeptieren. Einige Aspekte dieses Features werden bis zur allgemeinen Verfügbarkeit unter Umständen noch geändert.
>

Eine Registrierung mit Georeplikation bietet folgende Vorteile:

* Einzelner Registrierungs-/Image-/Tagname in mehreren Regionen verwendbar
* Netzwerknaher Registrierungszugriff in regionalen Bereitstellungen
* Keine zusätzlichen Ausgangsgebühren, da Images aus einer lokalen, replizierten Registrierung in der gleichen Region wie Ihr Containerhost abgerufen werden
* Zentrale Verwaltung einer Registrierung über mehrere Regionen hinweg

## <a name="example-use-case"></a>Beispiel eines Anwendungsfalls
Contoso betreibt eine öffentlich zugängliche Website in den USA, Kanada und Europa. Um diese Märkte mit lokalen und netzwerknahen Inhalten zu bedienen, betreibt Contoso auf [Azure Container Service](/azure/container-service/kubernetes/) (ACS) basierende Kubernetes-Cluster in den Regionen „USA, Westen“, „USA, Osten“, „Kanada, Mitte“ und „Europa, Westen“. Die Websiteanwendung, die als Docker-Image bereitgestellt ist, verwendet in allen Regionen den gleichen Code und das gleiche Image. Der für die jeweilige Region lokale Inhalt wird aus einer Datenbank abgerufen, die in jeder Region individuell bereitgestellt wird. Jede regionale Bereitstellung hat eine eigene Konfiguration für Ressourcen wie die lokale Datenbank.

Das Bereitstellungsteam befindet sich in Seattle im US-Bundesstaat Washington und nutzt das Rechenzentrum USA, Westen.

![Übertragung per Push in mehrere Registrierungen](media/container-registry-geo-replication/before-geo-replicate.png)<br />*Übertragung per Push in mehrere Registrierungen*

Vor der Nutzung der Georeplikationsfunktionen verfügte Contoso über eine Registrierung in der Region „USA, Westen“ und eine zusätzliche Registrierung in der Region „Europa, Westen“. Um diese verschiedenen Regionen zu versorgen, musste das Entwicklungsteam Images per Push in zwei verschiedene Registrierungen übertragen.

```bash
docker push contoso.azurecr.io/pubic/products/web:1.2
docker push contosowesteu.azurecr.io/pubic/products/web:1.2
```
![Abruf per Pull aus mehreren Registrierungen](media/container-registry-geo-replication/before-geo-replicate-pull.png)<br />*Abruf per Pull aus mehreren Registrierungen*

Es folgen typische Herausforderungen bei mehreren Registrierungen:

* Die Cluster „USA, Osten“, „USA, Westen“ und „Kanada, Mitte“ rufen Daten per Pull aus der Registrierung „USA, Westen“ ab. Dabei fallen Ausgangsgebühren an, da jeder dieser Remotecontainerhosts Images per Pull aus den Rechenzentren von „USA, Westen“ abruft.
* Das Entwicklungsteam muss Images in die Registrierungen „USA, Westen“ und „Europa, Westen“ per Push übertragen.
* Das Entwicklungsteam muss jede regionale Bereitstellung mit Imagenamen konfigurieren und verwalten, die auf die lokale Registrierung verweisen.
* Der Zugriff auf die Registrierung muss für jede Region konfiguriert werden.

## <a name="benefits-of-geo-replication"></a>Vorteile der Georeplikation

![Abruf per Pull aus Registrierung mit Georeplikation](media/container-registry-geo-replication/after-geo-replicate-pull.png)

Die Georeplikationsfunktion von Azure Container Registry bietet die folgenden Vorteile:

* Verwalten einer einzelnen Registrierung in allen Regionen: `contoso.azurecr.io`
* Verwalten einer einzelnen Konfiguration von Imagebereitstellungen, da alle Regionen die gleiche Image-URL verwenden: `contoso.azurecr.io/public/products/web:1.2`
* Übertragung per Push in eine einzelne Registrierung, während ACR die Georeplikation verwaltet, einschließlich regionaler Webhooks für lokale Benachrichtigungen

## <a name="configure-geo-replication"></a>Konfigurieren der Georeplikation
Die Konfiguration der Georeplikation ist so einfach wie das Klicken auf Regionen auf einer Karte.

Die Georeplikation ist nur bei [Premium-Registrierungen](container-registry-skus.md) möglich. Wenn Ihre Registrierung noch nicht im Premium-Tarif betrieben wird, können Sie im [Azure-Portal](https://portal.azure.com) von den Tarifen Basic und Standard zu Premium wechseln:

![Wechseln von SKUs im Azure-Portal](media/container-registry-skus/update-registry-sku.png)

Um die Georeplikation für Ihre Premium-Registrierung zu konfigurieren, melden Sie sich unter „http://portal.azure.com“ beim Azure-Portal an.

Navigieren Sie zu Ihrer Azure Container Registry-Instanz, und wählen Sie **Replikationen** aus:

![Replikationen auf der Benutzeroberfläche für Containerregistrierungen im Azure-Portal](media/container-registry-geo-replication/registry-services.png)

Eine Karte mit allen aktuellen Azure-Regionen wird angezeigt:

 ![Karte mit Regionen im Azure-Portal](media/container-registry-geo-replication/registry-geo-map.png)

* Blaue Sechsecke stellen aktuelle Replikate dar.
* Grüne Sechsecke stellen mögliche Replikatregionen dar.
* Graue Sechsecke stellen Azure-Regionen dar, die noch nicht für die Replikation verfügbar sind.

Um ein Replikat konfigurieren, wählen Sie ein grünes Sechseck aus und klicken dann auf **Erstellen**:

 ![Benutzeroberfläche zum Erstellen von Replikaten im Azure-Portal](media/container-registry-geo-replication/create-replication.png)

Um zusätzliche Replikate zu konfigurieren, wählen Sie die grünen Sechsecke für andere Regionen aus und klicken dann auf **Erstellen**.

ACR beginnt, Images in den konfigurierten Replikaten zu synchronisieren. Sobald der Vorgang abgeschlossen ist, gibt das Portal *Bereit* zurück. Der Status des Replikats im Portal wird nicht automatisch aktualisiert. Klicken Sie auf die Schaltfläche „Aktualisieren“, um den aktualisierten Status anzuzeigen.

## <a name="geo-replication-pricing"></a>Georeplikation – Preise

Die Georeplikation ist ein Funktionsmerkmal des [Premium-Tarifs](container-registry-skus.md#premium) von Azure Container Registry. Wenn Sie eine Registrierung in die gewünschten Regionen replizieren, fallen für jede Region Premium-Registrierungsgebühren an.

Im vorhergehenden Beispiel hat Contoso zwei Registrierungen zu einer konsolidiert und Replikate den Regionen „USA, Osten“, „Kanada, Mitte“ und „Europa, Westen“ hinzugefügt. Contoso zahlt nun viermal den Premium-Tarif pro Monat, ohne zusätzliche Konfiguration oder Verwaltung. Jede Region ruft nun ihre Images lokal per Pull ab und verbessert so die Leistung und Zuverlässigkeit ohne Netzwerkausgangsgebühren von USA, Westen nach Kanada und USA, Osten.

## <a name="summary"></a>Zusammenfassung

Mithilfe der Georeplikation können Sie Ihre regionalen Rechenzentren als eine globale Cloud verwalten. Da Images in vielen Azure-Diensten verwendet werden, können Sie von einer einzigen Verwaltungsebene profitieren und gleichzeitig lokale Images netzwerknah, schnell und zuverlässig per Pull abrufen.

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die dreiteilige Tutorialreihe [Georeplikation in Azure Container Registry](container-registry-tutorial-prepare-registry.md) an. Durchlaufen Sie das Erstellen einer georeplizierten Registrierung und eines Containers, und stellen Sie diesen anschließend mit einem einzigen `docker push`-Befehl in mehreren regionalen Web-Apps für Container-Instanzen bereit.

> [!div class="nextstepaction"]
> [Georeplikation in Azure Container Registry](container-registry-tutorial-prepare-registry.md)