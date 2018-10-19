---
title: Erstellen von serverlosen Apps mit Visual Studio | Microsoft-Dokumentation
description: Erstellen, Bereitstellen und Verwalten Ihrer ersten serverlosen App mit Azure Logic Apps und Azure Functions in Visual Studio
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.custom: vs-azure
ms.topic: article
ms.date: 08/01/2018
ms.openlocfilehash: f5555d9a60934529bf8fed6db6a18dd783f46075
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2018
ms.locfileid: "44297467"
---
# <a name="build-your-first-serverless-app-with-azure-logic-apps-and-azure-functions---visual-studio"></a>Erstellen Ihrer ersten serverlosen App mit Azure Logic Apps und Azure Functions – Visual Studio

Sie können Cloud-Apps mithilfe der serverlosen Tools und Funktionen in Azure, z. B. [Azure Logic Apps](../logic-apps/logic-apps-overview.md) und [Azure Functions](../azure-functions/functions-overview.md), schnell entwickeln und bereitstellen. In diesem Artikel wird gezeigt, wie Sie in Visual Studio mit dem Erstellen einer serverlosen App beginnen, die eine Logik-App zum Aufrufen einer Azure-Funktion verwendet. Weitere Informationen zu serverlosen Lösungen in Azure finden Sie unter [Serverloses Azure mit Functions und Logic Apps](../logic-apps/logic-apps-serverless-overview.md).

## <a name="prerequisites"></a>Voraussetzungen

Zum Erstellen einer serverlosen App in Visual Studio benötigen Sie folgende Elemente:

* Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/).

* [Visual Studio 2017](https://www.visualstudio.com/vs/) oder Visual Studio 2015 - Community, Professional oder Enterprise

* [Microsoft Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 oder höher)

* [Azure PowerShell](https://github.com/Azure/azure-powershell#installation)

* [Azure Logic Apps-Tools für Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio-18551) oder [Visual Studio 2015](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio)

  Sie können die Azure Logic Apps-Tools entweder direkt vom Visual Studio Marketplace herunterladen und installieren oder sich über das [Installieren dieser Erweiterung aus Visual Studio](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions) informieren. 
  Achten Sie darauf, dass Sie Visual Studio nach Abschluss der Installation neu starten. 

* [Azure Functions Core Tools](https://www.npmjs.com/package/azure-functions-core-tools) zum lokalen Debuggen von Functions

* Internetzugriff bei Verwendung des in Visual Studio eingebetteten Logik-App-Designers

  Für den Designer ist eine Internetverbindung zum Erstellen von Ressourcen in Azure und zum Lesen der Eigenschaften und Daten von Connectors in Ihrer Logik-App erforderlich. 
  Wenn Sie beispielsweise den Dynamics CRM Online-Connector verwenden, prüft der Designer Ihre CRM-Instanz auf verfügbare standardmäßige und benutzerdefinierte Eigenschaften.

## <a name="create-resource-group-project"></a>Erstellen eines Ressourcengruppenprojekts

Erstellen Sie für Ihre serverlose App zunächst ein [Azure-Ressourcengruppenprojekt](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md). In Azure erstellen Sie Ressourcen innerhalb einer Ressourcengruppe. Dabei handelt es sich um eine logische Sammlung, die Sie zum Organisieren, Verwalten und Bereitstellen von Ressourcen für eine gesamte App als einzelnes Objekt verwenden. Für eine serverlose App in Azure umfasst Ihre Ressourcengruppe Ressourcen sowohl für Azure Logic Apps als auch für Azure Functions. Informieren Sie sich über [Azure-Ressourcengruppen und -Ressourcen](../azure-resource-manager/resource-group-overview.md).

1. Starten Sie Visual Studio, und melden Sie sich mit Ihrem Azure-Konto an. 

1. Wählen Sie im Menü **Datei** die Option **Neu** > **Projekt**. 

   ![Erstellen eines neuen Projekts in Visual Studio](./media/logic-apps-serverless-get-started-vs/create-new-project-visual-studio.png)

1. Wählen Sie unter **Installiert** die Option **Visual C#** oder **Visual Basic**. Wählen Sie **Cloud** > **Azure-Ressourcengruppe**.

   Wenn die Kategorie **Cloud** oder das Projekt **Azure-Ressourcengruppe** nicht vorhanden ist, vergewissern Sie sich, dass Sie das Azure SDK für Visual Studio installiert haben.

1. Geben Sie einen Namen und einen Speicherort für Ihr Projekt ein. Wählen Sie dann **OK** aus. 

   Visual Studio fordert sie auf, eine Vorlage auszuwählen. 
   Sie können mit einer leeren Vorlage, „Logik-App“, oder einer anderen Vorlage beginnen. In diesem Beispiel wird allerdings eine Azure-Schnellstartvorlage zum Erstellen einer serverlosen App verwendet, die eine Logik-App und einen Aufruf einer Azure-Funktion enthält.

   Wenn Sie in Visual Studio nur eine Logik-App erstellen möchten, wählen Sie die Vorlage **Logik-App** aus. Diese Vorlage erstellt eine leere Logik-App, die im Logik-App-Designer geöffnet wird, ohne dass Sie Ihre Projektmappe in einer Azure-Ressourcengruppe vorab bereitstellen müssen.

1. Wählen Sie unter **Vorlagen von diesem Speicherort anzeigen** die Option **Azure-Schnellstart (github/Azure/azure-quickstart-templates)** aus. 

1. Geben Sie im Suchfeld „Logik-App“ als Filter ein, und wählen Sie die folgende serverlose Schnellstartvorlage und dann **OK** aus: **101-logic-app-and-function-app**

   ![Auswählen der Azure-Schnellstartvorlage](./media/logic-apps-serverless-get-started-vs/select-template.png)

   Visual Studio erstellt und öffnet eine Projektmappe für Ihr Ressourcengruppenprojekt. 
   Die von Ihnen ausgewählte Schnellstartvorlage erstellt in Ihrem Ressourcengruppenprojekt die Bereitstellungsvorlage `azuredeploy.json`. 
   Diese Bereitstellungsvorlage enthält die Definition für eine einfache Logik-App, die bei einer HTTP-Anforderung ausgelöst wird, eine Azure-Funktion aufruft und das Ergebnis als HTTP-Antwort zurückgibt. 
   
   ![Neue serverlose Lösung](./media/logic-apps-serverless-get-started-vs/create-serverless-solution.png)

1. Als Nächstes müssen Sie Ihre Projektmappe in Azure bereitstellen, bevor Sie die Bereitstellungsvorlage öffnen und die Ressourcen für Ihre serverlose App überprüfen können. 

## <a name="deploy-your-solution"></a>Bereitstellen der Lösung

Bevor Sie Ihre Logik-App mit dem Logik-App-Designer in Visual Studio öffnen können, müssen Sie über eine Azure-Ressourcengruppe verfügen, die in Azure bereits bereitgestellt wurde. Der Designer kann dann Verbindungen zu Ressourcen und Diensten in Ihrer Logik-App aufbauen. Stellen Sie für diese Aufgabe Ihre Projektmappe aus Visual Studio im Azure-Portal bereit.

1. Öffnen Sie im Projektmappen-Explorer das Kontextmenü Ihres Ressourcenprojekts, und wählen Sie **Bereitstellen** > **Neu** aus.

   ![Erstellen einer neuen Bereitstellung für die Ressourcengruppe](./media/logic-apps-serverless-get-started-vs/deploy.png)

1. Falls dies noch nicht geschehen ist, wählen Sie Ihr Azure-Abonnement und die für die Bereitstellung vorgesehene Ressourcengruppe aus. Wählen Sie **Bereitstellen** aus.

   ![Bereitstellungseinstellungen](./media/logic-apps-serverless-get-started-vs/deploy-to-resource-group.png)

1. Geben Sie im Feld **Parameter bearbeiten** den Ressourcennamen ein, der für die Bereitstellung Ihrer Logik-App und Ihrer Azure-Funktions-App verwendet werden soll, und speichern Sie Ihre Einstellungen. Achten Sie darauf, einen global eindeutigen Namen für Ihre Funktions-App zu verwenden.

   ![Bereitstellen von Namen für Ihre Logik-App und Funktions-App](./media/logic-apps-serverless-get-started-vs/logic-function-app-name-parameters.png)

   Wenn Visual Studio mit der Bereitstellung in Ihrer angegebenen Ressourcengruppe beginnt, wird der Bereitstellungsstatus Ihrer Projektmappe im Visual Studio-Fenster **Ausgabe** angezeigt. 
   Nach Abschluss der Bereitstellung ist Ihre Logik-App im Azure-Portal live.

## <a name="edit-logic-app-in-visual-studio"></a>Bearbeiten der Logik-App in Visual Studio

Nachdem Ihre Projektmappe in Ihrer Ressourcengruppe bereitgestellt wurde, öffnen Sie Ihre Logik-App mit dem Logik-App-Designer, damit Sie sie bearbeiten und ändern können.

1. Öffnen Sie im Projektmappen-Explorer das Kontextmenü der Datei `azuredeploy.json`, und wählen Sie **Mit Logik-App-Designer öffnen** aus.

   ![Öffnen von "azuredeploy.json" im Logik-App-Designer](./media/logic-apps-serverless-get-started-vs/open-logic-app-designer.png)

1. Nachdem das Feld **Logik-App-Eigenschaften** eingeblendet wurde, wählen Sie unter **Abonnement** Ihr Azure-Abonnement aus, sofern es nicht bereits ausgewählt ist. Wählen Sie unter **Ressourcengruppe** die Ressourcengruppe und den Speicherort, in der bzw. dem Sie Ihre Projektmappe bereitgestellt haben, und dann **OK** aus.

   ![Logik-App-Eigenschaften](./media/logic-apps-serverless-get-started-vs/logic-app-properties.png)

   Nachdem der Logik-App-Designer geöffnet wurde, können Sie weiterhin Schritte hinzufügen oder den Workflow ändern und Ihre Änderungen speichern.

   ![Geöffnete Logik-App im Logik-App-Designer](./media/logic-apps-serverless-get-started-vs/opened-logic-app.png)

## <a name="create-azure-functions-project"></a>Erstellen eines Azure Functions-Projekts

Führen Sie zum Erstellen Ihres Functions-Projekts und Ihrer Funktion mit JavaScript, Python, F#, PowerShell, Batch oder Bash die Schritte im Artikel [Arbeiten mit Azure Functions Core Tools](../azure-functions/functions-run-local.md) aus. Zum Entwickeln Ihrer Azure-Funktion mit C# in Ihrer Projektmappe können Sie eine C#-Klassenbibliothek verwenden, indem Sie die Schritte im Artikel [Veröffentlichen einer .NET-Klassenbibliothek als Funktions-App](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/16/publishing-a-net-class-library-as-a-function-app/) ausführen.

## <a name="deploy-functions-from-visual-studio"></a>Bereitstellen von Funktionen aus Visual Studio

Ihre Bereitstellungsvorlage stellt alle in Ihrer Projektmappe aus dem Git-Repository (angegeben durch Variablen in der Datei `azuredeploy.json`) verfügbaren Azure-Funktionen bereit. Wenn Sie Ihr Functions-Projekt in Ihrer Projektmappe erstellen, können Sie es in die Git-Quellcodeverwaltung, z.B. GitHub oder Azure DevOps, einchecken und dann die Variable `repo` so ändern, dass die Vorlage Ihre Azure-Funktion bereitstellt.

## <a name="manage-logic-apps-and-view-run-history"></a>Verwalten von Logik-Apps und Anzeigen des Ausführungsverlaufs

Bei Logik-Apps, die in Azure bereits bereitgestellt wurden, können Sie weiterhin den Ausführungsverlauf bearbeiten, verwalten und anzeigen sowie diese Apps aus Visual Studio heraus deaktivieren. 

1. Öffnen Sie im Menü **Ansicht** in Visual Studio **Cloud-Explorer**. 

1. Wählen Sie unter **Alle Abonnements** das Azure-Abonnement aus, das mit den zu verwaltenden Logik-Apps verknüpft ist. Wählen Sie dann **Übernehmen** aus.

1. Wählen Sie unter **Logic Apps** Ihre Logik-App aus. Wählen Sie im Kontextmenü dieser App **Mit Logik-App-Editor öffnen** aus. 

Jetzt können Sie die bereits veröffentlichte Logik-App in Ihr Ressourcengruppenprojekt herunterladen. Obwohl Sie vielleicht eine Logik-App im Azure-Portal gestartet haben, können Sie diese App also weiterhin in Visual Studio importieren und verwalten. Weitere Informationen finden Sie unter [Verwalten von Logik-Apps mit Visual Studio](../logic-apps/manage-logic-apps-with-visual-studio.md).

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen eines serverlosen sozialen Dashboards](logic-apps-scenario-social-serverless.md)
* [Verwalten von Logik-Apps mit Visual Studio](manage-logic-apps-with-visual-studio.md)
* [Definitionssprache für Logik-App-Workflows](logic-apps-workflow-definition-language.md)