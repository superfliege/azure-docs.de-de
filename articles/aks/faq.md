---
title: "Häufig gestellte Fragen zum Azure Container Service"
description: "Enthält Antworten auf einige der häufig gestellten Fragen zum Azure Container Service."
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 2/01/2018
ms.author: nepeters
ms.openlocfilehash: 2b78479c257930669729a7781b3893b3e2064bab
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/03/2018
---
# <a name="frequently-asked-questions-about-azure-container-service-aks"></a>Häufig gestellte Fragen zum Azure Container Service (AKS)

Dieser Artikel behandelt häufig gestellte Fragen zum Azure Container Service (AKS).

## <a name="which-azure-regions-will-have-azure-container-service-aks"></a>In welchen Azure-Regionen wird der Azure Container Service (AKS) verfügbar sein? 

- Kanada, Mitte 
- Kanada, Osten 
- USA (Mitte) 
- USA (Ost) 
- Südostasien 
- Europa, Westen 
- USA, Westen 2 

## <a name="when-will-additional-regions-be-added"></a>Wann werden weitere Regionen hinzugefügt? 

Zusätzliche Regionen werden bei steigendem Bedarf hinzugefügt.

## <a name="are-security-updates-applied-to-aks-nodes"></a>Werden Sicherheitsupdates auf AKS-Knoten angewendet? 

Betriebssystem-Sicherheitspatches werden planmäßig nachts auf die Knoten in Ihrem Cluster angewendet, jedoch wird kein Neustart ausgeführt. Bei Bedarf können die Knoten über das Portal oder die Azure CLI neu gestartet werden. Bei der Aktualisierung eines Clusters wird das neueste Ubuntu-Image verwendet, und alle Sicherheitspatches werden (mit einem Neustart) angewendet.

## <a name="do-you-recommend-customers-use-acs-or-akss"></a>Empfehlen Sie Kunden die Verwendung von ACS oder AKS? 

Davon ausgehend, dass der Azure Container Service (AKS) zu einem späteren Zeitpunkt allgemein verfügbar sein wird, sollten Sie Proofs of Concept, Entwicklungs- und Testcluster in AKS erstellen, Produktionscluster jedoch in ACS-Kubernetes.  

## <a name="when-will-acs-be-deprecated"></a>Wenn wird ACS veraltet sein? 

ACS wird etwa dann veraltet sein, wenn AKS allgemein verfügbar sein wird. Ab diesem Datum haben Sie 12 Monate Zeit, Cluster zu AKS zu migrieren. Während des Zeitraums von 12 Monaten können Sie alle ACS-Vorgänge ausführen.

## <a name="does-aks-support-node-autoscaling"></a>Unterstützt AKS die automatische Skalierung von Knoten? 

Automatische Knotenskalierung wird nicht unterstützt, ist aber geplant. Werfen Sie einen Blick auf diese [Implementierung der automatischen Skalierung ][auto-scaler] in Open Source.

## <a name="why-are-two-resource-groups-created-with-aks"></a>Warum werden zwei Ressourcengruppen mit AKS erstellt? 

Die zweite Ressourcengruppe wird zum mühelosen Löschen aller Ressourcen, die einer AKS-Bereitstellung zugeordnet sind, automatisch erstellt.

## <a name="is-azure-key-vault-integrated-with-aks"></a>Ist Azure Key Vault in AKS integriert? 

Nein, diese Integration ist aber geplant. In der Zwischenzeit können Sie die folgende Lösung von [Hexadite][hexadite] ausprobieren. 

<!-- LINKS - external -->
[auto-scaler]: https://github.com/kubernetes/autoscaler
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent  