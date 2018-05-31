---
title: Verwalten von Geheimnissen beim Arbeiten mit einem Azure Dev Space | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: article
ms.technology: azds-kubernetes
description: Schnelle Kubernetes-Entwicklung mit Containern und Microservices in Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Container Service, Container
manager: douge
ms.openlocfilehash: b77d862f578ddc374dbb58117b4ea58eb973e5fe
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
ms.locfileid: "34198312"
---
# <a name="how-to-manage-secrets-when-working-with-an-azure-dev-space"></a>Verwalten von Geheimnissen beim Arbeiten mit einem Azure Dev Space

Für Ihre Dienste sind möglicherweise bestimmte Kennwörter, Verbindungszeichenfolgen und andere Geheimnisse erforderlich, z. B. für Datenbanken oder andere sichere Azure-Dienste. Wenn Sie die Werte dieser Geheimnisse in Konfigurationsdateien festlegen, können Sie sie in Ihrem Code als Umgebungsvariablen zur Verfügung stellen.  Sie müssen umsichtig damit umgehen, um die Sicherheit der Geheimnisse nicht zu gefährden.

Azure Dev Spaces bietet zwei empfohlene Optionen zum Speichern von Geheimnissen: in der Datei „values.dev.yaml“ und durch direktes Einbeziehen in „azds.yaml“. Es wird davon abgeraten, Geheimnisse in der Datei „values.yaml“ zu speichern.
 
## <a name="method-1-valuesdevyaml"></a>Methode 1: „values.dev.yaml“
1. Öffnen Sie Visual Studio Code (VS Code) mit Ihrem für Azure Dev Spaces aktivierten Projekt.
2. Fügen Sie eine Datei mit dem Namen _values.dev.yaml_ im gleichen Ordner wie die vorhandene _values.yaml_ hinzu, und definieren Sie Ihren geheimen Schlüssel und die Werte, wie im folgenden Beispiel:

    ```yaml
    secrets:
      redis:
        port: "6380"
        host: "contosodevredis.redis.cache.windows.net"
        key: "secretkeyhere"
    ```
     
3. Aktualisieren Sie _azds.yaml_, um Azure Dev Spaces anzuweisen, Ihre neue Datei _values.dev.yaml_ zu verwenden. Fügen Sie zu diesem Zweck die folgende Konfiguration unter dem Abschnitt „configurations.develop.container“ hinzu:

    ```yaml
           container:
             values:
             - "charts/webfrontend/values.dev.yaml"
    ```
 
4. Ändern Sie den Dienstcode so, dass er auf diese Geheimnisse als Umgebungsvariablen verweist, wie im folgenden Beispiel:

    ```
    var redisPort = process.env.REDIS_PORT
    var host = process.env.REDIS_HOST
    var theKey = process.env.REDIS_KEY
    ```
    
5. Aktualisieren Sie die in Ihrem Cluster ausgeführten Dienste mit diesen Änderungen. Führen Sie in der Befehlszeile den folgenden Befehl aus:

    ```
    azds up
    ```
 
6. (Optional) Überprüfen Sie über die Befehlszeile, ob diese Geheimnisse erstellt wurden:

      ```
      kubectl get secret --namespace default -o yaml 
      ```

7. Vergewissern Sie sich, dass Sie _values.dev.yaml_ zur _GITIGNORE-Datei_ hinzugefügt haben, um das Ausführen eines Commits für Geheimnisse in der Quellcodeverwaltung zu vermeiden.
 
 
## <a name="method-2-inline-directly-in-azdsyaml"></a>Methode 2: Direktes Einbeziehen in „azds.yaml“
1.  Legen Sie in der Datei _azds.yaml_ die Geheimnisse unter dem YAML-Abschnitt „configurations/develop/install“ fest. Geheimniswerte können zwar direkt dort eingegeben werden, dies ist jedoch nicht empfehlenswert, weil _azds.yaml_ in der Quellcodeverwaltung eingecheckt ist. Fügen Sie stattdessen Platzhalter mit der „$PLACEHOLDER“-Syntax hinzu.

    ```yaml
    configurations:
      develop:
        ...
        install:
          set:
            secrets:
              redis:
                port: "$REDIS_PORT_DEV"
                host: "$REDIS_HOST_DEV"
                key: "$REDIS_KEY_DEV"
    ```
     
2.  Erstellen Sie eine_ENV-Datei_ im gleichen Ordner wie _azds.yaml_. Geben Sie Geheimnisse in der Notation Standardschlüssel=Wert ein. Übergeben Sie die_ENV-Datei_ nicht an die Quellcodeverwaltung. (Um sie in git-basierten Versionskontrollsystemen auszulassen, fügen Sie die Datei zur__  GITIGNORE-Datei hinzu.) Im folgenden Beispiel ist eine_ENV-Datei_ dargestellt:

    ```
    REDIS_PORT_DEV=3333
    REDIS_HOST_DEV=myredishost
    REDIS_KEY_DEV=myrediskey
    ```
2.  Ändern Sie den Dienstquellcode so, dass er auf diese Geheimnisse im Code verweist, wie im folgenden Beispiel:

    ```
    var redisPort = process.env.REDIS_PORT
    var host = process.env.REDIS_HOST
    var theKey = process.env.REDIS_KEY
    ```
 
3.  Aktualisieren Sie die in Ihrem Cluster ausgeführten Dienste mit diesen Änderungen. Führen Sie in der Befehlszeile den folgenden Befehl aus:

    ```
    azds up
    ```

4.  (Optional) Zeigen Sie Geheimnisse aus kubectl an:

    ```
    kubectl get secret --namespace default -o yaml
    ```

## <a name="next-steps"></a>Nächste Schritte

Mithilfe dieser Methoden können Sie jetzt eine sichere Verbindung mit einer Datenbank oder einem Redis Cache herstellen oder auf sichere Azure-Dienste zugreifen.
 