---
ms.openlocfilehash: 430302d8b1b2a01febbbe2f11057bb331ec80c28
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57907743"
---
## <a name="locate-a-cloud-spatial-anchor"></a>Lokalisieren eines cloudbasierten Raumankers

Um cloudbasierte Raumanker zu lokalisieren, müssen Sie ihre IDs kennen. Anker-IDs können im Back-End-Dienst Ihrer Anwendung gespeichert werden und sind für alle Geräte verfügbar, die sich ordnungsgemäß beim Dienst authentifizieren können. Ein Beispiel finden Sie unter [Tutorial: Freigeben von Raumankern für Geräte](/azure/spatial-anchors/tutorials/tutorial-share-anchors-across-devices/).

Instanziieren Sie ein AnchorLocateCriteria-Objekt, legen Sie die zu suchenden IDs fest, und rufen Sie die CreateWatcher-Methode für die Sitzung auf, indem Sie Ihr AnchorLocateCriteria-Objekt angeben.
