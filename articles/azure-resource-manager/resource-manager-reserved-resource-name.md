---
title: Fehler vom Typ „Reservierter Azure-Ressourcenname“ | Microsoft-Dokumentation
description: Hier wird beschrieben, wie Sie Fehler beheben, die auftreten, wenn der angegebene Ressourcenname ein reserviertes Wort enthält.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/08/2017
ms.author: tomfitz
ms.openlocfilehash: b91a53d17d64afb0a56f745505f10e8cabbc22cc
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
ms.locfileid: "34357060"
---
# <a name="resolve-reserved-resource-name-errors"></a>Fehler vom Typ „Reservierter Ressourcenname“

In diesem Artikel wird der Fehler beschrieben, der auftritt, wenn Sie eine Ressource bereitstellen, deren Name ein reserviertes Wort enthält.

## <a name="symptom"></a>Symptom

Wenn Sie eine Ressource bereitstellen, die über einen öffentlichen Endpunkt verfügbar ist, kann der folgende Fehler auftreten:

```
Code=ReservedResourceName;
Message=The resource name <resource-name> or a part of the name is a trademarked or reserved word.
```

## <a name="cause"></a>Ursache

Die Namen von Ressourcen mit einem öffentlichen Endpunkt dürfen keine reservierten Wörter oder Marken enthalten.

Die folgenden Wörter sind reserviert:

* ACCESS
* AZURE
* BING
* BIZSPARK
* BIZTALK
* CORTANA
* DIRECTX
* DOTNET
* DYNAMICS
* EXCEL
* EXCHANGE
* FOREFRONT
* GROOVE
* HOLOLENS
* HYPERV
* KINECT
* LYNC
* MSDN
* O365
* OFFICE
* OFFICE365
* ONEDRIVE
* ONENOTE
* OUTLOOK
* POWERPOINT
* SHAREPOINT
* SKYPE
* VISIO
* VISUALSTUDIO

Die folgenden Wörter dürfen weder als ganzes Wort noch als Teilzeichenfolge im Namen verwendet werden:

* LOGIN
* MICROSOFT
* WINDOWS
* XBOX

## <a name="solution"></a>Lösung

Geben Sie einen Namen an, der keines der reservierten Wörter enthält.