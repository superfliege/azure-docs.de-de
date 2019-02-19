---
title: JavaScript und Seitenvertragsversionen für Benutzerflows in Azure Active Directory B2C | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie JavaScript aktivieren und mit Seitenvertragsversionen einen Benutzerflow in Azure Active Directory B2C anpassen.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 997babef5860488232f0b530c90b44aba6608ac5
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2019
ms.locfileid: "56007468"
---
# <a name="about-using-javascript-and-page-contract-versions-in-a-user-flow"></a>Informationen zu JavaScript und Seitenvertragsversionen in einem Benutzerflow

Azure Active Directory B2C stellt eine Reihe von gepackten Inhalten bereit, die HTML, CSS und JavaScript für die Benutzeroberflächenelemente in Ihren Benutzerflows enthalten. Wenn Sie beabsichtigen, clientseitigen [JavaScript](javascript-samples.md)-Code in Ihren Benutzerflows zu aktivieren, möchten Sie sichergehen, dass die Elemente, auf denen der JavaScript-Code basiert, unveränderlich sind. Andernfalls könnten Änderungen zu einem unerwarteten Verhalten auf Ihren Benutzerflowseiten führen. Um diese Probleme zu vermeiden, können Sie die Verwendung eines Seitenvertrags für einen Benutzerflow erzwingen und eine Seitenvertragsversion angeben. Dadurch wird sichergestellt, dass alle Inhaltsdefinitionen, auf denen Ihr JavaScript-Code basiert, unveränderlich sind. Auch wenn Sie nicht beabsichtigen, JavaScript für einen Benutzerflow zu aktivieren, können Sie eine Seitenvertragsversion für Ihre Benutzerflowseiten angeben.

> [!NOTE]
> In diesem Artikel geht es um JavaScript für Benutzerflows. Sie können aber auch JavaScript verwenden und Seitenvertragsversionen auswählen, wenn Sie mit [benutzerdefinierten Richtlinien](page-contract.md) arbeiten.

## <a name="enable-javascript"></a>Aktivieren von JavaScript

In den Benutzerfloweigenschaften können Sie JavaScript aktivieren, wodurch auch die Verwendung eines Seitenvertrags erzwungen wird. Anschließend können Sie die Seitenvertragsversion festlegen, wie im nächsten Abschnitt beschrieben.

![Einstellung „JavaScript aktivieren“](media/user-flow-javascript-overview/javascript-settings.PNG)

## <a name="specify-a-page-contract-version"></a>Angeben einer Seitenvertragsversion

Sie können eine Seitenvertragsversion für Ihre Benutzerflowseiten angeben, und zwar unabhängig davon, ob Sie JavaScript in den Benutzerfloweigenschaften aktivieren oder nicht. Öffnen Sie den Benutzerflow, und wählen Sie **Seitenlayouts** aus. Wählen Sie unter **Layoutname** eine Benutzerflowseite und dann die Option **Seitenvertragsversion** aus.

![Einstellung „JavaScript aktivieren“](media/user-flow-javascript-overview/page-contract-version.PNG)

## <a name="next-steps"></a>Nächste Schritte
Lesen Sie [JavaScript-Beispiele für die Verwendung in Azure Active Directory B2C](javascript-samples.md).
