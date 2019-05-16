---
title: Tutorial zur Verwaltung von Featureflags mithilfe von Azure App Configuration | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie Featureflags mithilfe von Azure App Configuration getrennt von Ihrer Anwendung verwalten.
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.devlang: csharp
ms.topic: tutorial
ms.date: 04/19/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: d995a2e9f0d05dc3b0853036e8fb3c04ccdfab96
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2019
ms.locfileid: "65412289"
---
# <a name="tutorial-manage-feature-flags-in-azure-app-configuration"></a>Tutorial: Verwalten von Featureflags in Azure App Configuration

Sie können alle Featureflags in Azure App Configuration speichern und von einem zentralen Ort aus verwalten. Die verfügbare Portal-Benutzeroberfläche namens **Feature-Manager** ist speziell für Featureflags konzipiert. Darüber hinaus wird das Datenschema für .NET Core-Featureflags von App Configuration nativ unterstützt.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Definieren und Verwalten von Featureflags in App Configuration
> * Zugreifen auf Featureflags über Ihre Anwendung

## <a name="create-feature-flags"></a>Erstellen von Featureflags

Der **Feature-Manager** im Azure-Portal für App Configuration bietet eine Benutzeroberfläche zum Erstellen und Verwalten von Featureflags, die Sie in Ihrer Anwendung verwenden. Führen Sie die folgenden Schritte aus, um ein neues Featureflag hinzufügen:

1. Wählen Sie **Feature-Manager** > **+ Erstellen** aus, um ein Featureflag hinzuzufügen.

    ![Liste mit Featureflags](./media/azure-app-configuration-feature-flags.png)

2. Geben Sie einen eindeutigen Schlüsselnamen für das Featureflag ein. Dieser Name wird benötigt, um im Code auf das Flag zu verweisen.

3. Optional: Geben Sie eine benutzerfreundlichere Beschreibung für das Featureflag ein.

4. Legen Sie den Anfangszustand des Featureflags fest. In der Regel wird einfach *Ein* oder *Aus* verwendet.

    ![Erstellung des Featureflags](./media/azure-app-configuration-feature-flag-create.png)

5. Wenn der Zustand *Ein* lautet, können Sie optional über **Filter hinzufügen** zusätzliche Qualifizierungsbedingungen angeben. Geben Sie einen integrierten oder benutzerdefinierten Filterschlüssel ein, und ordnen Sie Parameter zu. Zu den integrierten Filtern zählen folgende:

    | Schlüssel | JSON-Parameter |
    |---|---|
    | Microsoft.Percentage | {"Value": 0–100 Prozent} |
    | Microsoft.TimeWindow | {"Start": UTC-Zeit, "End": UTC-Zeit} |

    ![Featureflagfilter](./media/azure-app-configuration-feature-flag-filter.png)

## <a name="update-feature-flag-states"></a>Aktualisieren von Featureflagzuständen

Gehen Sie wie folgt vor, um den Zustandswert eines Featureflags zu ändern:

1. Wählen Sie **Feature-Manager** aus.

2. Klicken Sie rechts neben einem Featureflag, das Sie ändern möchten, auf **...** > **Bearbeiten**.

3. Legen Sie einen neuen Zustand für das Featureflag fest.

## <a name="access-feature-flags"></a>Zugreifen auf Featureflags

Vom **Feature-Manager** erstellte Featureflags werden als reguläre Schlüssel-Wert-Paare gespeichert und abgerufen. Sie befinden sich unter einem speziellen Namespacepräfix: *.appconfig.featureflag*. Die zugrunde liegenden Schlüssel-Wert-Paare können im **Konfigurations-Explorer** angezeigt werden. Ihre Anwendung kann zum Abrufen die App Configuration-Konfigurationsanbieter, SDKs, Befehlszeilenerweiterungen und REST-APIs verwenden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie Featureflags und deren Zustand mithilfe von App Configuration verwalten. Die folgenden Ressourcen enthalten weitere Informationen zur Unterstützung der Featureverwaltung in App Configuration und ASP.NET Core:

* [Tutorial: Use feature flags in a .NET Core app](./use-feature-flags-dotnet-core.md) (Tutorial: Verwenden von Featureflags in einer .NET Core-App)
