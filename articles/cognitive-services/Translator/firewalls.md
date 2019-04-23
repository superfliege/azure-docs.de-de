---
title: Übersetzung hinter Firewalls – Textübersetzungs-API
titlesuffix: Azure Cognitive Services
description: Übersetzen Sie mithilfe der Textübersetzungs-API hinter IP-Firewalls.
services: cognitive-services
author: v-pawal
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 02/21/2019
ms.author: v-jansko
ms.openlocfilehash: ebc8b7ce4ed0242f1bc62e3b97b6594cfe810374
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "58915746"
---
# <a name="how-to-translate-behind-ip-firewalls-with-the-translator-text-api"></a>Übersetzung hinter IP-Firewalls mithilfe der Textübersetzungs-API

Die Textübersetzungs-API kann hinter Firewalls entweder mithilfe der Domänennamen- oder der IP-Filterung Übersetzungen durchführen. Die Domänennamenfilterung ist hierbei die bevorzugte Methode. Es wird **nicht empfohlen**, Microsoft Translator hinter einer IP-gefilterten Firewall auszuführen. Die Unterstützung für dieses Setup wird in Zukunft möglicherweise ohne Vorankündigung eingestellt.

## <a name="translator-ip-addresses"></a>IP-Adressen für Translator
Die IP-Adressen für die Microsoft Textübersetzungs-API (api.cognitive.microsofttranslator.com) lauten ab dem 20. November 2018 wie folgt:

* **Asien-Pazifik:** 40.90.139.163, 104.44.89.44
* **Europa:** 40.90.138.4, 40.90.141.99
* **Nordamerika:** 40.90.139.36, 40.90.139.2


## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Übersetzung hinter IP-Firewalls in Ihrem Textübersetzungs-API-Aufruf](reference/v3-0-translate.md)
