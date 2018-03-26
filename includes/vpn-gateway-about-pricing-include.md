---
title: Includedatei
description: Includedatei
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 205c67377e4bff66c02e3ee508c0f6f4e2823608
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
Sie bezahlen für zwei Dinge: die stündlichen Computekosten für das Gateway des virtuellen Netzwerks und die Übertragung der Ausgangsdaten vom Gateway des virtuellen Netzwerks. Informationen zu den Preisen finden Sie auf der Seite [Preise](https://azure.microsoft.com/pricing/details/vpn-gateway) .

**Computekosten für virtuelles Netzwerkgateway**<br>Für jedes Gateway des virtuellen Netzwerks fallen stündliche Computekosten an. Der Preis basiert auf der Gateway-SKU, die Sie beim Erstellen des Gateways des virtuellen Netzwerks angeben. Die Kosten für das eigentliche Gateway fallen zusätzlich zur Datenübertragung an, die über das Gateway durchgeführt wird.

**Datenübertragungskosten**<br>Datenübertragungskosten werden basierend auf ausgehendem Datenverkehr vom Gateway des virtuellen Quellnetzwerks berechnet.

* Wenn Sie Datenverkehr an Ihr lokales VPN-Gerät senden, wird dafür die Internetrate für die Übertragung von Ausgangsdaten berechnet.
* Wenn Sie Datenverkehr zwischen virtuellen Netzwerken in unterschiedlichen Regionen senden, richtet sich der Preis nach der Region.
* Wenn Sie Datenverkehr nur zwischen virtuellen Netzwerken senden, die sich in derselben Region befinden, fallen keine Kosten für die Datenübertragung an. Der Datenverkehr zwischen VNets in derselben Region ist kostenlos.