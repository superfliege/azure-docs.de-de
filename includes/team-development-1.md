---
title: Includedatei
description: Includedatei
ms.custom: include file
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.subservice: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: include
manager: douge
ms.openlocfilehash: 7092d5059f36e6c8c046d14bb446d7ad1ac18451
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55291065"
---
Bisher haben Sie den Code Ihrer Anwendung so ausgeführt, als wären Sie der einzige Entwickler, der an der App arbeitet. In diesem Abschnitt erfahren Sie, wie Azure Dev Spaces die Teamentwicklung optimiert:
* Ein Team von Entwicklern kann in der gleichen Umgebung arbeiten und je nach Bedarf einen gemeinsamen oder einen getrennten Entwicklungsbereich nutzen.
* Jeder Entwickler kann seinen Code isoliert und ohne Beeinträchtigung der anderen Entwickler durchlaufen.
* Testen Sie Ihren Code vollständig, bevor Sie ihn committen, ohne Modelle erstellen oder Abhängigkeiten simulieren zu müssen.

### <a name="challenges-with-developing-microservices"></a>Herausforderungen bei der Entwicklung von Microservices
Ihre Beispielanwendung ist im Moment nicht sehr komplex. Bei der realen Entwicklung stoßen Sie jedoch bald auf Herausforderungen, wenn Sie mehr Dienste hinzufügen und das Entwicklungsteam wächst.

Stellen Sie sich vor, Sie arbeiten an einem Dienst, der mit Dutzenden anderen Diensten interagiert.

- Die lokale Ausführung aller Komponenten für die Entwicklung kann daher unrealistisch werden. Ihr Entwicklungscomputer verfügt unter Umständen nicht über ausreichend Ressourcen zum Ausführen der gesamten App. Oder Ihre App verwendet vielleicht Endpunkte, die öffentlich zugänglich sein müssen (beispielsweise antwortet Ihre App auf einen Webhook von einer SaaS-App).

- Sie können versuchen, nur die Dienste auszuführen, auf die Sie angewiesen sind. Das bedeutet jedoch, dass Sie alle Abhängigkeiten kennen müssen (beispielsweise Abhängigkeiten von Abhängigkeiten). Oder es ist nicht ganz einfach, Abhängigkeiten zu erstellen und auszuführen, da Sie an diesen nicht selbst gearbeitet haben.
- Einige Entwickler verlegen sich darauf, viele ihrer Dienstabhängigkeiten zu simulieren. Diese Vorgehensweise kann manchmal hilfreich sein. Für die Verwaltung dieser Simulationen kann jedoch bald ein gesonderter Entwicklungsaufwand entstehen. Darüber hinaus führt dieser Ansatz dazu, dass Ihr Entwicklungsbereich sich wesentlich von der Produktionsumgebung unterscheidet, und es können sich kleine Fehler einschleichen.
- End-to-End-Tests können daher schwierig werden. Integrationstests sind nur nach einem Commit realistisch. Das bedeutet, dass Ihnen Probleme erst später im Entwicklungszyklus auffallen.

![](../articles/dev-spaces/media/common/microservices-challenges.png)


### <a name="work-in-a-shared-dev-space"></a>Arbeiten in einem gemeinsamen Entwicklungsbereich
Mit Azure Dev Spaces können Sie einen *gemeinsamen* Entwicklungsbereich in Azure einrichten. Jeder Entwickler kann sich auf seinen Teil der Anwendung konzentrieren und *Code vor dem Commit* iterativ in einem Entwicklungsbereich entwickeln, der bereits alle anderen Dienste und Cloudressourcen enthält, die der Entwickler für seine Szenarien benötigt. Abhängigkeiten sind immer aktuell, und Entwickler arbeiten wie in einer Produktionsumgebung.

### <a name="work-in-your-own-space"></a>Arbeiten im eigenen Bereich
Während der Entwicklung von Code für Ihren Dienst und vor dem Einchecken ist Code häufig nicht fehlerfrei. Sie strukturieren und testen den Code weiterhin iterativ und experimentieren mit Lösungen. In einem **Bereich** – einem Konzept von in Azure Dev Spaces – können Sie isoliert arbeiten, ohne Ihre Teammitglieder zu beeinträchtigen.

> [!Note]
> Bevor Sie fortfahren, schließen Sie alle VS Code-Fenster für beide Dienste, und führen Sie `azds up -d` im jeweiligen Stammordner der Dienste aus. (Dies ist eine Einschränkung der Vorschauversion.)

Sehen wir uns jetzt einmal an, wo die Dienste aktuell ausgeführt werden. Führen Sie den Befehl `azds list-up` aus, um eine Ausgabe wie die folgende zu erhalten:

```
Name                          DevSpace  Type     Updated  Status
----------------------------  --------  -------  -------  -------
mywebapi                      default   Service  3m ago   Running
mywebapi-56c8f45d9-zs4mw      default   Pod      3m ago   Running
webfrontend                   default   Service  1m ago   Running
webfrontend-6b6ddbb98f-fgvnc  default   Pod      1m ago   Running
```

Die Spalte „DevSpace“ zeigt, dass beide Dienste in einem Bereich namens `default` ausgeführt werden. Jeder Benutzer, der die öffentliche URL öffnet und zur Web-App navigiert, ruft den von Ihnen geschriebenen Codepfad auf, der beide Dienste durchläuft. Nehmen Sie an, Sie möchten mit der Entwicklung von `mywebapi` fortfahren. Wie nehmen Sie Änderungen am Code vor und testen diese, ohne andere Entwickler zu unterbrechen, die die Entwicklungsumgebung ebenfalls verwenden? Die Lösung: Sie richten Ihren eigenen Bereich ein.

### <a name="create-a-dev-space"></a>Erstellen eines Entwicklungsbereichs (Dev Space)
Um Ihre Version von `mywebapi` in einem anderen Bereich als `default` auszuführen, erstellen Sie mithilfe des folgenden Befehls einen eigenen Bereich:

``` 
azds space select --name scott
```

Wählen Sie bei Aufforderung unter **parent dev space** die Option `default`. Dies bedeutet, dass der neue Bereich `default/scott` vom Bereich `default` abgeleitet wird. Wir werden gleich sehen, wie uns dies beim Testen behilflich ist. 

Im obigen Beispiel habe ich meinen Namen für den neuen Bereich verwendet, damit meine Kollegen erkennen können, dass es sich um den Bereich handelt, in dem ich arbeite. Sie können jedoch einen beliebigen Namen verwenden, z.B. „Demo“ oder „Sprint4“.

Führen Sie den Befehl `azds space list` aus, um eine Liste aller Bereiche in der Entwicklungsumgebung anzuzeigen. Neben dem aktuell ausgewählten Bereich wird ein Sternchen (*) angezeigt. In unserem Fall wurde der Bereich „default/scott“ automatisch ausgewählt, als er erstellt wurde. Sie können jederzeit mit dem Befehl `azds space select` einen anderen Bereich auswählen.
