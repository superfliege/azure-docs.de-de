---
title: "Azure-Schnellstart – Erstellen eines Azure Automation-Kontos | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie ein Azure Automation-Konto erstellen und ein Runbook ausführen."
services: automation
author: csand-msft
ms.author: csand
ms.date: 12/13/2017
ms.topic: quickstart
ms.service: automation
ms.custom: mvc
ms.openlocfilehash: 5522c56749f9c26f376661636dd62f88017f805e
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/10/2018
---
# <a name="create-an-azure-automation-account"></a>Erstellen eines Azure Automation-Kontos

Azure Automation-Konten können über Azure erstellt werden. Diese Methode bietet eine browserbasierte Benutzeroberfläche zum Erstellen und Konfigurieren von Automation-Konten und den dazugehörigen Ressourcen. In dieser Schnellstartanleitung werden die Erstellung eines Automation-Kontos und die Ausführung eines Runbooks im Konto Schritt für Schritt beschrieben.

Wenn Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich unter „https://portal.azure.com“ an Azure an.

## <a name="create-automation-account"></a>Erstellen eines Automation-Kontos

1. Klicken Sie in Azure in der linken oberen Ecke auf die Schaltfläche **Neu**.

1. Wählen Sie **Überwachung + Verwaltung** und dann **Automation**.

1. Geben Sie die Kontoinformationen ein. Wählen Sie für **Ausführendes Azure-Konto erstellen** die Option **Ja**, damit die Artefakte zum Vereinfachen der Authentifizierung für Azure automatisch aktiviert werden. Klicken Sie nach Abschluss des Vorgangs auf **Erstellen**, um die Bereitstellung des Automation-Kontos zu starten.

    ![Eingeben von Informationen zu Ihrem Automation-Konto auf der Seite](./media/automation-quickstart-create-account/create-automation-account-portal-blade.png)  

1. Das Automation-Konto wird im Azure-Dashboard angeheftet. Nachdem die Bereitstellung abgeschlossen ist, wird die Übersicht über das Automation-Konto automatisch geöffnet.

    ![Automation-Konto – Übersicht](./media/automation-quickstart-create-account/automation-account-overview.png)

## <a name="run-a-runbook"></a>Ausführen eines Runbooks

Führen Sie ein Runbook des Tutorials aus.

1. Klicken Sie unter **PROZESSAUTOMATISIERUNG** auf **Runbooks**. Die Liste mit den Runbooks wird angezeigt. Standardmäßig sind mehrere Tutorial-Runbooks im Konto aktiviert.

    ![Liste mit Runbooks des Automation-Kontos](./media/automation-quickstart-create-account/automation-runbooks-overview.png)

1. Wählen Sie das Runbook **AzureAutomationTutorialScript** aus. Die Seite mit der Runbookübersicht wird geöffnet.

    ![Runbookübersicht](./media/automation-quickstart-create-account/automation-tutorial-script-runbook-overview.png)

1. Klicken Sie auf **Starten** und auf der Seite **Runbook starten** dann auf **OK**, um das Runbook zu starten.

    ![Seite für Runbookauftrag](./media/automation-quickstart-create-account/automation-tutorial-script-job.png)

1. Nachdem sich der **Auftragsstatus** in **Wird ausgeführt** geändert hat, können Sie auf **Ausgabe** oder **Alle Protokolle** klicken, um die Ausgabe des Runbookauftrags anzuzeigen. Für dieses Tutorial-Runbook ist die Ausgabe eine Liste mit Ihren Azure-Ressourcen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe, das Automation-Konto und alle dazugehörigen Ressourcen, wenn Sie sie nicht mehr benötigen. Wählen Sie hierzu die Ressourcengruppe für das Automation-Konto aus, und klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie ein Automation-Konto bereitgestellt, einen Runbookauftrag gestartet und die Auftragsergebnisse angezeigt. Weitere Informationen zu Azure Automation finden Sie in der Schnellstartanleitung zum Erstellen Ihres ersten Runbooks.

> [!div class="nextstepaction"]
> [Automation-Schnellstart – Erstellen eines Runbooks](./automation-quickstart-create-runbook.md)
