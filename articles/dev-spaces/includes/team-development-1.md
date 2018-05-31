---
title: Includedatei
description: Includedatei
ms.custom: include file
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: include
manager: douge
ms.openlocfilehash: 404e238e51b7ac8b799f413965560a8d42ccc5df
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
ms.locfileid: "34371115"
---
Bisher haben Sie den Code Ihrer Anwendung so ausgeführt, als wären Sie der einzige Entwickler, der an der App arbeitet. In diesem Abschnitt erfahren Sie, wie Azure Dev Spaces die Teamentwicklung optimiert:
* Ermöglichen Sie einem Team von Entwicklern, in der gleichen Umgebungsentwicklung zu arbeiten.
* Jeder Entwickler kann seinen Code isoliert und ohne Beeinträchtigung der anderen Entwickler durchlaufen.
* Testen Sie Ihren Code vollständig, bevor Sie ihn committen, ohne Modelle erstellen oder Abhängigkeiten simulieren zu müssen.

## <a name="challenges-with-developing-microservices"></a>Herausforderungen bei der Entwicklung von Microservices
Ihre Beispielanwendung ist im Moment nicht sehr komplex. Bei der realen Entwicklung stoßen Sie jedoch bald auf Herausforderungen, wenn Sie mehr Dienste hinzufügen und das Entwicklungsteam wächst.

Stellen Sie sich vor, Sie arbeiten an einem Dienst, der mit Dutzenden anderen Diensten interagiert.

- Die lokale Ausführung aller Komponenten für die Entwicklung kann daher unrealistisch werden. Ihr Entwicklungscomputer verfügt unter Umständen nicht über ausreichend Ressourcen zum Ausführen der gesamten App. Oder Ihre App verwendet vielleicht Endpunkte, die öffentlich zugänglich sein müssen (beispielsweise antwortet Ihre App auf einen Webhook von einer SaaS-App).

- Sie können versuchen, nur die Dienste auszuführen, auf die Sie angewiesen sind. Das bedeutet jedoch, dass Sie alle Abhängigkeiten kennen müssen (beispielsweise Abhängigkeiten von Abhängigkeiten). Oder es ist nicht ganz einfach, Abhängigkeiten zu erstellen und auszuführen, da Sie an diesen nicht selbst gearbeitet haben.
- Einige Entwickler verlegen sich darauf, viele ihrer Dienstabhängigkeiten zu simulieren. Diese Vorgehensweise kann manchmal hilfreich sein. Für die Verwaltung dieser Simulationen kann jedoch bald ein gesonderter Entwicklungsaufwand entstehen. Darüber hinaus führt dieser Ansatz dazu, dass Ihre Entwicklungsumgebung sich wesentlich von der Produktionsumgebung unterscheidet, und es können sich kleine Fehler einschleichen.
- End-to-End-Tests können daher schwierig werden. Integrationstests sind nur nach einem Commit realistisch. Das bedeutet, dass Ihnen Probleme erst später im Entwicklungszyklus auffallen.

![](../media/common/microservices-challenges.png)


## <a name="work-in-a-shared-development-environment"></a>Arbeiten in einer gemeinsamen Entwicklungsumgebung
Mit Azure Dev Spaces können Sie eine *gemeinsame* Entwicklungsumgebung in Azure einrichten. Jeder Entwickler kann sich auf seinen Teil der Anwendung konzentrieren und *Code vor dem Commit* iterativ in einer Umgebung entwickeln, die bereits alle anderen Dienste und Cloudressourcen enthält, die der Entwickler für seine Szenarien benötigt. Abhängigkeiten sind immer aktuell, und Entwickler arbeiten wie in einer Produktionsumgebung.

## <a name="work-in-your-own-space"></a>Arbeiten im eigenen Bereich
Während der Entwicklung von Code für Ihren Dienst und vor dem Einchecken ist Code häufig nicht fehlerfrei. Sie strukturieren und testen den Code weiterhin iterativ und experimentieren mit Lösungen. In einem **Bereich** – einem Konzept von in Azure Dev Spaces – können Sie isoliert arbeiten, ohne Ihre Teammitglieder zu beeinträchtigen.

> [!Note]
> Bevor Sie fortfahren, schließen Sie alle VS Code-Fenster für beide Dienste, und führen Sie `azds up -d` im jeweiligen Stammordner der Dienste aus. (Dies ist eine Einschränkung der Vorschauversion.)

Sehen wir uns jetzt einmal an, wo die Dienste aktuell ausgeführt werden. Führen Sie den Befehl `azds list` aus, um eine Ausgabe wie die folgende zu erhalten:

```
Name         Space     Chart              Ports   Updated     Access Points
-----------  --------  -----------------  ------  ----------  -------------------------
mywebapi     default  mywebapi-0.1.0     80/TCP  2m ago     <not attached>
webfrontend  default  webfrontend-0.1.0  80/TCP  1m ago     http://webfrontend-contosodev.1234abcdef.eastus.aksapp.io
```

Die Spalte „Space“ zeigt, dass beide Dienste in einem Bereich namens `default` ausgeführt werden. Jeder Benutzer, der die öffentliche URL öffnet und zur Web-App navigiert, ruft den von Ihnen geschriebenen Codepfad auf, der beide Dienste durchläuft. Nehmen Sie an, Sie möchten mit der Entwicklung von `mywebapi` fortfahren. Wie nehmen Sie Änderungen am Code vor und testen diese, ohne andere Entwickler zu unterbrechen, die die Entwicklungsumgebung ebenfalls verwenden? Die Lösung: Sie richten Ihren eigenen Bereich ein.

## <a name="create-a-space"></a>Erstellen eines Bereichs
Um Ihre Version von `mywebapi` in einem anderen Bereich als `default` auszuführen, erstellen Sie mithilfe des folgenden Befehls einen eigenen Bereich:

``` 
azds space create --name scott
```

Im obigen Beispiel habe ich meinen Namen für den neuen Bereich verwendet, damit meine Kollegen erkennen können, dass es sich um den Bereich handelt, in dem ich arbeite. Sie können jedoch einen beliebigen Namen verwenden, z.B. „Demo“ oder „Sprint4“.

Führen Sie den Befehl `azds space list` aus, um eine Liste aller Bereiche in der Entwicklungsumgebung anzuzeigen. Neben dem aktuell ausgewählten Bereich wird ein Sternchen (*) angezeigt. In unserem Fall wurde der Bereich „scott“ automatisch ausgewählt, als er erstellt wurde. Sie können jederzeit mit dem Befehl `azds space select` einen anderen Bereich auswählen.
