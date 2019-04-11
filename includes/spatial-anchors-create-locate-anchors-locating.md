---
ms.openlocfilehash: 52dfbfca5f79a7f92848ea39eddc00aa10f05ff1
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2019
ms.locfileid: "58631640"
---
## <a name="locate-a-cloud-spatial-anchor"></a>Lokalisieren eines cloudbasierten Raumankers

Die Möglichkeit, einen zuvor hochgeladenen cloudbasierten Raumanker zu lokalisieren, ist einer der Hauptgründe für die Nutzung der Azure Spatial Anchors-Bibliothek. Um cloudbasierte Raumanker zu lokalisieren, müssen Sie ihre IDs kennen. Anker-IDs können im Back-End-Dienst Ihrer Anwendung gespeichert werden und sind für alle Geräte verfügbar, die sich ordnungsgemäß beim Dienst authentifizieren können. Ein Beispiel finden Sie unter [Tutorial: Freigeben von Raumankern für Geräte](/azure/spatial-anchors/tutorials/tutorial-share-anchors-across-devices/).

Instanziieren Sie ein `AnchorLocateCriteria`-Objekt, legen Sie die zu suchenden IDs fest, und rufen Sie die `CreateWatcher`-Methode für die Sitzung auf, indem Sie Ihr `AnchorLocateCriteria`-Objekt angeben.
