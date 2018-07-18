---
title: Parametrisieren von Konfigurationsdateien in Microsoft Azure Service Fabric | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Konfigurationsdatei in Service Fabric parametrisieren.
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/06/2017
ms.author: mikhegn
ms.openlocfilehash: e5bb2f270cc5a6f288e1e995f4bfa74f4e3551b7
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
ms.locfileid: "34207817"
---
# <a name="how-to-parameterize-configuration-files-in-service-fabric"></a>Parametrisieren von Konfigurationsdateien in Service Fabric

In diesem Artikel erfahren Sie, wie Sie eine Konfigurationsdatei in Service Fabric parametrisieren.

## <a name="procedure-for-parameterizing-configuration-files"></a>Verfahren für das Parametrisieren von Konfigurationsdateien

In diesem Beispiel überschreiben Sie einen Konfigurationswert, indem Sie Parameter in der Anwendungsbereitstellung verwenden.

1. Öffnen Sie die Datei „Config\Settings.xml“.
1. Legen Sie einen Konfigurationsparameter fest, indem Sie den folgenden XML-Code hinzufügen:

    ```xml
      <Section Name="MyConfigSection">
        <Parameter Name="CacheSize" Value="25" />
      </Section>
    ```

1. Speichern und schließen Sie die Datei.
1. Öffnen Sie die Datei `ApplicationManifest.xml` .
1. Fügen Sie ein `ConfigOverride`-Element hinzu, in dem auf das Konfigurationspaket, den Abschnitt und den Parameter verwiesen wird.

      ```xml
        <ConfigOverrides>
          <ConfigOverride Name="Config">
              <Settings>
                <Section Name="MyConfigSection">
                    <Parameter Name="CacheSize" Value="[Stateless1_CacheSize]" />
                </Section>
              </Settings>
          </ConfigOverride>
        </ConfigOverrides>
      ```

1. Geben Sie dann, weiterhin in der Datei „ApplicationManifest.xml“, den Parameter im `Parameters`-Element an.

    ```xml
      <Parameters>
        <Parameter Name="Stateless1_CacheSize" />
      </Parameters>
    ```

1. Definieren Sie nun einen Standardwert (`DefaultValue`).

    ```xml
      <Parameters>
        <Parameter Name="Stateless1_CacheSize" DefaultValue="80" />
      </Parameters>
    ```

> [!NOTE]
> Wenn Sie ein ConfigOverride-Element hinzugefügt haben, wählt Service Fabric immer die Anwendungsparameter oder den Standardwert aus, der im Anwendungsmanifest angegeben ist.
>
>

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu einigen der grundlegenden Konzepte, die in diesem Artikel behandelt werden, finden Sie im Artikel [Verwalten von Anwendungen für mehrere Umgebungen](service-fabric-manage-multiple-environment-app-configuration.md).

Informationen zu anderen App-Verwaltungsfunktionen in Visual Studio finden Sie unter [Verwalten Ihrer Service Fabric-Anwendungen in Visual Studio](service-fabric-manage-application-in-visual-studio.md).