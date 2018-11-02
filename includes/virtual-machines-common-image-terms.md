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
ms.openlocfilehash: 6c7a9857f6d6d57dc9e314bcb1ef848a326b7ed2
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2018
ms.locfileid: "49437095"
---
## <a name="terminology"></a>Begriff

Ein Marketplace-Image in Azure hat die folgenden Attribute:

* **Publisher** (Herausgeber): Die Organisation, die das Image erstellt hat. Beispiele: Canonical, MicrosoftWindowsServer
* **Offer** (Angebot): Der Name einer Gruppe verwandter Images, die von einem Herausgeber erstellt wurden. Beispiele: Ubuntu Server, Windows Server
* **SKU**: Eine Instanz eines Angebots, etwa eine Hauptversion einer Distribution. Beispiele: 16.04-LTS, 2016-Datacenter
* **Version**: Die Versionsnummer einer Image-SKU. 

Um ein Marketplace-Image zu identifizieren, wenn Sie einen virtuellen Computer programmgesteuert bereitstellen, geben Sie diese Werte einzeln als Parameter an. Einige Tools akzeptieren einen Image-*URN*. Der URN kombiniert diese Werte und trennt sie durch einen Doppelpunkt (:): *Herausgeber*:*Angebot*:*SKU*:*Version*. In einem URN können Sie die Versionsnummer durch „latest“ ersetzen, um die neueste Version des Images auszuwählen. 

Wenn der Imageherausgeber zusätzliche Lizenz- und Kaufbedingungen angibt, müssen Sie diese Bedingungen akzeptieren und die programmgesteuerte Bereitstellung aktivieren. Sie müssen auch die Parameter des *Kaufplans* angeben, wenn Sie einen virtuellen Computer programmgesteuert bereitstellen. Siehe [Bereitstellen eines Images mit Marketplace-Nutzungsbedingungen](#deploy-an-image-with-marketplace-terms).