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
ms.openlocfilehash: 47ef014f31c628fed9d7b2b005e67e8138d3e7e9
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38944351"
---
## <a name="terminology"></a>Begriff

Ein Marketplace-Image in Azure hat die folgenden Attribute:

* **Publisher** (Herausgeber) – Die Organisation, die das Image erstellt hat. Beispiele: Canonical, MicrosoftWindowsServer
* **Offer** (Angebot) – Name einer Gruppe verwandter Images, die von einem Herausgeber erstellt wurden. Beispiele: Ubuntu Server, Windows Server
* **SKU** – Eine Instanz eines Angebots, etwa eine Hauptversion einer Distribution. Beispiele: 16.04-LTS, 2016-Datacenter
* **Version** – Die Versionsnummer eines Image-SKU. 

Um ein Marketplace-Image zu identifizieren, wenn Sie einen virtuellen Computer programmgesteuert bereitstellen, geben Sie diese Werte einzeln als Parameter an; einige Tools akzeptieren auch den Image-*URN*. Der URN kombiniert diese Werte und trennt sie durch einen Doppelpunkt (:): *Herausgeber*:*Angebot*:*SKU*:*Version*. In einem URN können Sie die Versionsnummer durch „latest“ ersetzen, um die neueste Version des Images auszuwählen. 

Wenn der Imageherausgeber zusätzliche Lizenz- und Kaufbedingungen angibt, müssen Sie diese Bedingungen akzeptieren und die programmgesteuerte Bereitstellung aktivieren. Sie müssen auch die Parameter des *Kaufplans* angeben, wenn Sie einen virtuellen Computer programmgesteuert bereitstellen. Siehe [Bereitstellen eines Images mit Marketplace-Nutzungsbedingungen](#deploy-an-image-with-marketplace-terms).