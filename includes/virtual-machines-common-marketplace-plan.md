---
title: Includedatei
description: Includedatei
services: virtual-machines-windows, virtual-machines-linux
author: dlepow
ms.service: multiple
ms.topic: include
ms.date: 02/28/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: a3c5290eb0179fe5842c495c2e08f22580d02bda
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2018
ms.locfileid: "29743363"
---
## <a name="deploy-an-image-with-marketplace-terms"></a>Bereitstellen eines Images mit Marketplace-Nutzungsbedingungen

Für bestimmte VM-Images im Azure Marketplace gelten zusätzliche Lizenz- und Kaufbedingungen, die Sie akzeptieren müssen, bevor Sie sie programmgesteuert bereitstellen können.  

Um einen virtuellen Computer über ein solches Image bereitzustellen, müssen Sie die Bedingungen des Images akzeptieren und die programmgesteuerte Bereitstellung aktivieren. Dies müssen Sie in Ihrem Abonnement nur einmal durchführen. Dann müssen Sie jedes Mal, wenn Sie einen virtuellen Computer programmgesteuert über das Image bereitstellen, auch die Parameter des *Kaufplans* angeben.

In den folgenden Abschnitten werden diese Aktionen gezeigt:

* Herausfinden, ob für ein Marketplace-Image zusätzliche Lizenzbedingungen gelten 
* Programmgesteuertes Akzeptieren der Bedingungen
* Angeben von Parametern des Kaufplans, wenn Sie einen virtuellen Computer programmgesteuert bereitstellen

