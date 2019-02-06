---
title: Includedatei
description: Includedatei
services: virtual-machines-windows, virtual-machines-linux
author: dlepow
ms.service: multiple
ms.topic: include
ms.date: 10/09/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 19a78b772d2813c263017515f18da06fdb20aa70
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2019
ms.locfileid: "55082264"
---
## <a name="terminology"></a>Begriff

Ein Marketplace-Image in Azure hat die folgenden Attribute:

* **Herausgeber**: Die Organisation, die das Image erstellt hat. Beispiele: Canonical, MicrosoftWindowsServer
* **Angebot**: Name einer Gruppe verwandter Images, die von einem Herausgeber erstellt wurden. Beispiele: Ubuntu Server, Windows Server
* **SKU**: Eine Instanz eines Angebots, z.B. eine Hauptversion einer Distribution. Beispiele: 18.04-LTS, 2019-Datacenter
* **Version**: Die Versionsnummer einer Image-SKU. 

Um ein Marketplace-Image zu identifizieren, wenn Sie einen virtuellen Computer programmgesteuert bereitstellen, geben Sie diese Werte einzeln als Parameter an. Einige Tools akzeptieren einen Image-*URN*. Der URN kombiniert diese Werte und trennt sie durch einen Doppelpunkt (:): *Herausgeber*:*Angebot*:*SKU*:*Version*. In einem URN können Sie die Versionsnummer durch „latest“ ersetzen, um die neueste Version des Images auszuwählen. 

Wenn der Imageherausgeber zusätzliche Lizenz- und Kaufbedingungen angibt, müssen Sie diese Bedingungen akzeptieren und die programmgesteuerte Bereitstellung aktivieren. Sie müssen auch die Parameter des *Kaufplans* angeben, wenn Sie einen virtuellen Computer programmgesteuert bereitstellen. Siehe [Bereitstellen eines Images mit Marketplace-Nutzungsbedingungen](#deploy-an-image-with-marketplace-terms).