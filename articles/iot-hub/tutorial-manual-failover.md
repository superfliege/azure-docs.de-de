---
title: Manuelles Failover einer Azure IoT Hub-Instanz | Microsoft-Dokumentation
description: Hier wird gezeigt, wie Sie ein manuelles Failover für eine Azure IoT Hub-Instanz ausführen.
author: robinsh
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: tutorial
ms.date: 07/11/2018
ms.author: robinsh
ms.custom: mvc
ms.openlocfilehash: f0e8bf922f142b795dd1a2ded4b3ec265c43481a
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2018
ms.locfileid: "39249993"
---
# <a name="tutorial-perform-manual-failover-for-an-iot-hub-public-preview"></a>Tutorial: Ausführen eines manuellen Failovers für eine IoT Hub-Instanz (öffentliche Vorschauversion)

Das manuelle Failover ist ein Feature des IoT Hub-Diensts, der Kunden die Ausführung eines [Failovers](https://en.wikipedia.org/wiki/Failover) für die Hubvorgänge einer primären Region in der entsprechenden geografisch gekoppelten Azure-Region ermöglicht. Das manuelle Failover kann im Falle eines regionalen Notfalls oder eines längeren Dienstausfalls ausgeführt werden. Sie können auch ein geplantes Failover zum Testen der Notfallwiederherstellungsfunktionen ausführen. Es wird jedoch empfohlen, einen IoT-Testhub anstelle einer in der Produktion ausgeführten Instanz zu verwenden. Das Feature für das manuelle Failover steht Kunden ohne zusätzliche Kosten zur Verfügung.

In diesem Tutorial führen Sie die folgenden Aufgaben aus:

> [!div class="checklist"]
> * Erstellen eines IoT-Hubs über das Azure-Portal 
> * Ausführen eines Failovers. 
> * Anzeigen der Hubausführung am sekundären Standort
> * Ausführen eines Failbacks, um die Vorgänge des IoT-Hubs am primären Standort wiederherzustellen 
> * Überprüfen, ob der Hub ordnungsgemäß am richtigen Standort ausgeführt wird

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 

2. Klicken Sie auf **+ Ressource erstellen**, und wählen Sie dann **Internet der Dinge** und anschließend **IoT Hub**.

   ![Screenshot, auf dem das Erstellen eines IoT-Hubs dargestellt ist](./media/tutorial-manual-failover/create-hub-01.png)

3. Klicken Sie auf die Registerkarte **Grundlagen**. Füllen Sie die folgenden Feldern aus:

    **Abonnement**: Wählen Sie das zu verwendende Azure-Abonnement aus.

    **Ressourcengruppe**: Klicken Sie auf **Neu erstellen**, und geben Sie als Ressourcengruppenname **ManlFailRG** an.

    **Region**: Wählen Sie eine Region in Ihrer Nähe aus, die Teil der Vorschau ist. In diesem Tutorial wird `westus2` verwendet. Ein Failover kann nur zwischen geografisch gekoppelten Azure-Regionen ausgeführt werden. Die mit „westus2“ geografisch gekoppelte Region ist „WestCentralUS“.
    
   > [!NOTE]
   > Das manuelle Failover befindet sich derzeit in der öffentlichen Vorschauphase und ist in den folgenden Azure-Regionen *nicht* verfügbar: USA, Osten; USA, Westen; Europa, Norden; Europa, Westen; Brasilien, Süden und USA, Süden-Mitte.

   **IoT Hub-Name**: Geben Sie einen Namen für Ihren IoT-Hub an. Der Hubname muss global eindeutig sein. 

   ![Screenshot, auf dem das Blatt „Grundlagen“ zum Erstellen eines IoT-Hubs dargestellt ist](./media/tutorial-manual-failover/create-hub-02-basics.png)

   Klicken Sie auf **Überprüfen + erstellen**. (Für Größe und Skalierung werden die Standardwerte verwendet.) 

4. Überprüfen Sie die Informationen, und klicken Sie dann auf **Erstellen**, um den IoT-Hub zu erstellen. 

   ![Screenshot, auf dem der letzte Schritt beim Erstellen eines IoT-Hubs dargestellt ist](./media/tutorial-manual-failover/create-hub-03-create.png)

## <a name="perform-a-manual-failover"></a>Ausführen eines manuellen Failovers

Beachten Sie, dass für einen IoT-Hub pro Tag maximal zwei Failover und zwei Failbacks ausgeführt werden können.

1. Klicken Sie auf **Ressourcengruppen**, und wählen Sie dann die Ressourcengruppe **ManlFailRG** aus. Klicken Sie in der Liste der Ressourcen auf Ihren Hub. 

2. Klicken Sie auf im IoT Hub-Bereich unter **Resilienz** auf **Manual failover (preview)** (Manuelles Failover (Vorschauversion)). Hinweis: Ihr Hub muss in einer gültigen Region eingerichtet werden, damit die Option für das manuelle Failover aktiviert ist.

   ![Screenshot mit dem Bereich „Eigenschaften“ der IoT Hub-Instanz](./media/tutorial-manual-failover/trigger-failover-01.png)

3. Im Bereich für das manuelle Failover werden die Optionen **IoT Hub Primary Location** (Primärer Standort der IoT Hub-Instanz) und **IoT Hub Secondary Location** (Sekundärer Standort der IoT Hub-Instanz) angezeigt. Der primäre Standort wird anfangs auf den Standort festgelegt, den Sie beim Erstellen des IoT-Hubs angegeben haben, und gibt immer den Standort an, an dem der Hub derzeit aktiv ist. Der sekundäre Standort ist die standardmäßige [geografisch gekoppelte Azure-Region](../best-practices-availability-paired-regions.md), die mit dem primären Standort gekoppelt ist. Die Standortwerte können nicht geändert werden. In diesem Tutorial ist der primäre Standort `westus2` und der sekundäre Standort `WestCentralUS`.

   ![Screenshot mit dem Bereich „Manuelles Failover“](./media/tutorial-manual-failover/trigger-failover-02.png)

3. Klicken Sie oben im Bereich „Manuelles Failover“ auf **Initiate failover** (Failover initiieren). Der Bereich **Confirm manual failover** (Manuelles Failover bestätigen) wird angezeigt. Geben Sie den Namen Ihres IoT-Hubs ein, um zu bestätigen, dass für ihn das Failover ausgeführt werden soll. Klicken Sie anschließend zum Initiieren des Failovers auf **OK**.

   Die Dauer für die Ausführung des manuellen Failovers ist proportional zur Anzahl von Geräten, die für Ihren Hub registriert sind. Wenn Sie beispielsweise 100.000 Geräte besitzen, dauert die Ausführung vielleicht 15 Minuten. Bei fünf Millionen Geräten kann sie jedoch eine Stunde oder länger dauern.

4. Geben Sie im Bereich **Confirm manual failover** (Manuelles Failover bestätigen) den Namen Ihres IoT-Hubs ein, um zu bestätigen, dass für ihn das Failover ausgeführt werden soll. Klicken Sie anschließend zum Initiieren des Failovers auf „OK“. 

   ![Screenshot mit dem Bereich „Manuelles Failover“](./media/tutorial-manual-failover/trigger-failover-03-confirm.png)

   Während der Ausführung des manuellen Failovers wird im Bereich „Manuelles Failover“ ein Banner mit dem Hinweis angezeigt, dass gerade ein manuelles Failover ausgeführt wird. 

   ![Screenshot mit dem Hinweis, dass gerade ein manuelles Failover ausgeführt wird](./media/tutorial-manual-failover/trigger-failover-04-in-progress.png)

   Wenn Sie den IoT Hub-Bereich schließen und wieder öffnen, indem Sie im Bereich „Ressourcengruppe“ auf den Hub klicken, wird ein Banner mit dem Hinweis angezeigt, dass der Hub nicht aktiv ist. 

   ![Screenshot mit dem inaktiven IoT-Hub](./media/tutorial-manual-failover/trigger-failover-05-hub-inactive.png)

   Nach Abschluss der Ausführung werden die primäre und die sekundäre Region auf der Seite „Manuelles Failover“ getauscht, und der Hub ist wieder aktiv. In diesem Beispiel ist jetzt der primäre Standort `WestCentralUS` und der sekundäre Standort `westus2`. 

   ![Screenshot mit abgeschlossenem Failover](./media/tutorial-manual-failover/trigger-failover-06-finished.png)

## <a name="perform-a-failback"></a>Ein Failback durchführen 

Nach der Ausführung eines manuellen Failovers können Sie die Vorgänge des Hubs wieder in der ursprünglichen primären Region auszuführen: Dieser Schritt wird Failback genannt. Falls Sie soeben ein Failover ausgeführt haben, müssen Sie bis zur Anforderung eines Failbacks eine Stunde warten. Wenn Sie vorher versuchen, das Failback auszuführen, wird eine Fehlermeldung angezeigt.

Ein Failback wird genau wie ein manuelles Failover ausgeführt. Hier sind die Schritte aufgeführt: 

1. Kehren Sie zum Ausführen eines Failbacks zum Iot Hub-Bereich für Ihren IoT-Hub zurück.

2. Klicken Sie auf im IoT Hub-Bereich unter **Resilienz** auf **Manual failover (preview)** (Manuelles Failover (Vorschauversion)). 

3. Klicken Sie oben im Bereich „Manuelles Failover“ auf **Initiate failover** (Failover initiieren). Der Bereich **Confirm manual failover** (Manuelles Failover bestätigen) wird angezeigt. 

4. Geben Sie im Bereich **Confirm manual failover** (Manuelles Failover bestätigen) den Namen Ihres IoT-Hubs ein, um zu bestätigen, dass für ihn das Failback ausgeführt werden soll. Klicken Sie dann auf „OK“, um das Failback zu initiieren. 

   ![Screenshot der Anforderung für ein manuelles Failback](./media/tutorial-manual-failover/trigger-failback-01-regions.png)

   Die Banner werden wie im Abschnitt [Ausführen eines Failovers](#perform-a-failover) angezeigt. Nach Abschluss des Failbacks wird wie ursprünglich festgelegt erneut `westus2` als primärer Standort und `WestCentralUS` als sekundärer Standort angezeigt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen 

Wenn Sie die für dieses Tutorial erstellten Ressourcen entfernen möchten, löschen Sie die Ressourcengruppe. Diese Aktion löscht alle in der Gruppe enthaltenen Ressourcen. In diesem Fall werden der IoT-Hub und die Ressourcengruppe selbst entfernt. 

1. Klicken Sie auf **Ressourcengruppen**. 

2. Suchen Sie nach der Ressourcengruppe **ManlFailRG**, und wählen Sie sie aus. Klicken Sie darauf, um es zu öffnen. 

3. Klicken Sie auf **Ressourcengruppe löschen**. Geben Sie bei entsprechender Aufforderung den Namen der Ressourcengruppe ein, und klicken Sie zum Bestätigen auf **Löschen**. 

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie ein manuelles Failover konfigurieren und ausführen und wie Sie durch Ausführen der folgenden Aufgaben ein Failback anfordern:

> [!div class="checklist"]
> * Erstellen eines IoT-Hubs über das Azure-Portal 
> * Ausführen eines Failovers. 
> * Anzeigen der Hubausführung am sekundären Standort
> * Ausführen eines Failbacks, um die Vorgänge des IoT-Hubs am primären Standort wiederherzustellen 
> * Überprüfen, ob der Hub ordnungsgemäß am richtigen Standort ausgeführt wird

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie den Status eines IoT-Geräts verwalten. 

> [!div class="nextstepaction"]
[Tutorial: Konfigurieren Ihrer Geräte über einen Back-End-Dienst](tutorial-device-twins.md)