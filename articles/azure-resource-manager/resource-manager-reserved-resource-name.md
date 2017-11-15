---
title: "Fehler vom Typ „Reservierter Azure-Ressourcenname“ | Microsoft-Dokumentation"
description: "Hier wird beschrieben, wie Sie Fehler beheben, die auftreten, wenn der angegebene Ressourcenname ein reserviertes Wort enthält."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: support-article
ms.date: 11/08/2017
ms.author: tomfitz
ms.openlocfilehash: 2f06b7bc65ea309bc5374b41f402f8e7bc4d2a38
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2017
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