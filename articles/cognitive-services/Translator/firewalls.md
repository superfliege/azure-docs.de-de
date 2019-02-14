---
title: Übersetzung hinter Firewalls – Textübersetzungs-API
titlesuffix: Azure Cognitive Services
description: Übersetzen Sie mithilfe der Textübersetzungs-API hinter IP-Firewalls.
services: cognitive-services
author: Jann-Skotdal
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 11/20/2018
ms.author: v-jansko
ms.openlocfilehash: 1e598594f53bc94d3a740d2bd5cdd100456131fa
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55884731"
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
