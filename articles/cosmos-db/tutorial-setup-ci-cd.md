---
title: Einrichten einer CI/CD-Pipeline mit dem Buildtask des Azure Cosmos DB-Emulators
description: Tutorial zum Einrichten des Build- und Releaseworkflows in Visual Studio Team Services (VSTS) mithilfe des Buildtasks des Cosmos DB-Emulators
services: cosmos-db
keywords: Azure Cosmos DB-Emulator
author: deborahc
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: tutorial
ms.date: 8/28/2018
ms.author: dech
ms.openlocfilehash: 37bb43435c34f14145b3642aa12c5cb0f16d780c
ms.sourcegitcommit: e2348a7a40dc352677ae0d7e4096540b47704374
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/05/2018
ms.locfileid: "43783772"
---
# <a name="set-up-a-cicd-pipeline-with-the-azure-cosmos-db-emulator-build-task-in-visual-studio-team-services"></a>Einrichten einer CI/CD-Pipeline mit dem Buildtask des Azure Cosmos DB-Emulators in Visual Studio Team Services

Der Azure Cosmos DB-Emulator stellt eine lokale Umgebung bereit, die zu Entwicklungszwecken den Azure Cosmos DB-Dienst emuliert. Mithilfe des Emulators können Sie Ihre Anwendung lokal entwickeln und testen, ohne ein Azure-Abonnement zu erstellen oder sonstige Kosten zu verursachen. 

Der Buildtask des Azure Cosmos DB-Emulators für Visual Studio Team Services (VSTS) ermöglicht Ihnen die gleichen Schritte in einer CI-Umgebung. Mit dem Buildtask können Sie im Rahmen der Build- und Releaseworkflows Tests für den Emulator ausführen. Der Task startet einen Docker-Container, in dem der Emulator bereits ausgeführt wird, und stellt einen Endpunkt bereit, der von der übrigen Builddefinition verwendet werden kann. Sie können beliebig viele Emulatorinstanzen erstellen, die jeweils in einem separaten Container ausgeführt werden. 

In diesem Artikel wird gezeigt, wie Sie für eine ASP.NET-Anwendung eine CI-Pipeline in VSTS einrichten, die den Buildtask des Cosmos DB-Emulators zum Ausführen von Tests nutzt. 

## <a name="install-the-emulator-build-task"></a>Installieren des Emulator-Buildtasks

Damit der Buildtask verwendet werden kann, muss er zunächst in der VSTS-Organisation installiert werden. Suchen Sie im [Marketplace](https://marketplace.visualstudio.com/items?itemName=azure-cosmosdb.emulator-public-preview) nach **Azure Cosmos DB-Emulator**, und klicken Sie auf **Kostenlos erhalten**.

![Suchen und Installieren des Buildtasks des Azure Cosmos DB-Emulators im VSTS-Marketplace](./media/tutorial-setup-ci-cd/addExtension_1.png)

Wählen Sie anschließend die Organisation aus, in der die Erweiterung installiert werden soll. 

> [!NOTE]
> Sie müssen Kontobesitzer oder Projektsammlungsadministrator sein, um eine Erweiterung in einer VSTS-Organisation installieren zu können. Falls Sie keine Berechtigungen haben, aber Kontomitglied sind, können Sie stattdessen Erweiterungen anfordern. [Weitere Informationen.](https://docs.microsoft.com/vsts/marketplace/faq-extensions?view=vsts#install-request-assign-and-access-extensions) 

![Auswählen einer VSTS-Organisation für die Installation einer Erweiterung](./media/tutorial-setup-ci-cd/addExtension_2.png)

## <a name="create-a-build-definition"></a>Erstellen einer Builddefinition

Die Erweiterung ist installiert und muss nun einer [Builddefinition](https://docs.microsoft.com/vsts/pipelines/get-started-designer?view=vsts&tabs=new-nav) hinzugefügt werden. Sie können eine vorhandene Builddefinition ändern oder eine neue erstellen. Wenn Sie bereits eine Builddefinition besitzen, können Sie mit [Hinzufügen des Tasks zu einer Builddefinition](#addEmulatorBuildTaskToBuildDefinition) fortfahren.

Navigieren Sie in VSTS zur Registerkarte **Build und Release**. Klicken Sie auf **+Neu.**

![Erstellen einer neuen Builddefinition](./media/tutorial-setup-ci-cd/CreateNewBuildDef_1.png) Wählen Sie das gewünschte Teamprojekt und Repository und den gewünschten Branch aus, um Builds zu ermöglichen. 

![Auswählen des Teamprojekts, des Repositorys und des Branch für die Builddefinition ](./media/tutorial-setup-ci-cd/CreateNewBuildDef_2.png)

Wählen Sie abschließend die gewünschte Vorlage für die Builddefinition aus. In diesem Tutorial wird die Vorlage **ASP.NET** ausgewählt. 

![Auswählen der gewünschten Builddefinitionsvorlage ](./media/tutorial-setup-ci-cd/CreateNewBuildDef_3.png)

Sie besitzen nun eine Builddefinition und können sie zur Verwendung des Azure Cosmos DB-Emulator-Buildtasks einrichten, der etwa wie folgt aussieht: 

![ASP.NET-Builddefinitionsvorlage](./media/tutorial-setup-ci-cd/CreateNewBuildDef_4.png)

## <a name="addEmulatorBuildTaskToBuildDefinition"></a>Hinzufügen des Tasks zu einer Builddefinition

Suchen Sie zum Hinzufügen des Emulator-Buildtasks mithilfe des Suchfelds nach **cosmos**, und klicken Sie auf **Hinzufügen**. Der Buildtask startet einen Container, in dem bereits eine Instanz des Cosmos DB-Emulators ausgeführt wird. Der Task muss daher vor allen anderen Tasks platziert werden, die darauf angewiesen sind, dass der Emulator ausgeführt wird.

![Hinzufügen des Emulator-Buildtasks zur Builddefinition](./media/tutorial-setup-ci-cd/addExtension_3.png) In diesem Tutorial fügen Sie den Task zu Beginn von Phase 1 hinzu, um sicherzustellen, dass der Emulator verfügbar ist, bevor die Tests ausgeführt werden.
Die abgeschlossene Builddefinition sieht nun wie folgt aus: 

![ASP.NET-Builddefinitionsvorlage](./media/tutorial-setup-ci-cd/CreateNewBuildDef_5.png)

## <a name="configure-tests-to-use-the-emulator"></a>Konfigurieren von Tests zur Verwendung des Emulators
Sie konfigurieren die Tests nun zur Verwendung des Emulators. Der Emulator-Buildtask exportiert die Umgebungsvariable „CosmosDbEmulator.Endpoint“. Diese kann von nachfolgenden Tasks in der Buildpipeline verwendet werden, um Anforderungen auszugeben. 

In diesem Tutorial führen Sie mithilfe des [Visual Studio-Testtasks](https://github.com/Microsoft/vsts-tasks/blob/master/Tasks/VsTestV2/README.md) Komponententests aus, die über eine Datei vom Typ **.runsettings** konfiguriert wurden. Weitere Informationen zur Komponententesteinrichtung finden Sie in der [Dokumentation](https://docs.microsoft.com/visualstudio/test/configure-unit-tests-by-using-a-dot-runsettings-file?view=vs-2017).

Nachfolgend sehen Sie ein Beispiel einer Datei vom Typ **.runsettings**, die Parameter definiert, die an die Komponententests einer Anwendung übergeben werden sollen. Beachten Sie, dass die Variable `authKey` der [bekannte Schlüssel](https://docs.microsoft.com/azure/cosmos-db/local-emulator#authenticating-requests) für den Emulator ist. Dieses `authKey`-Element ist der vom Emulator-Buildtask erwartete Schlüssel. Er muss in der Datei vom Typ **.runsettings** definiert sein.

```csharp
<RunSettings>
    <TestRunParameters>
    <Parameter name="endpoint" value="https://localhost:8081" />
    <Parameter name="authKey" value="C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==" />
    <Parameter name="database" value="ToDoListTest" />
    <Parameter name="collection" value="ItemsTest" />
  </TestRunParameters>
</RunSettings>
```
Auf diese `TestRunParameters`-Parameter wird im Testprojekt der Anwendung über eine `TestContext`-Eigenschaft verwiesen. Hier sehen Sie ein Beispiel für einen Test, der für Cosmos DB ausgeführt wird: 

```csharp
namespace todo.Tests
{
    [TestClass]
    public class TodoUnitTests
    {
        public TestContext TestContext { get; set; }

        [TestInitialize()]
        public void Initialize()
        {
            string endpoint = TestContext.Properties["endpoint"].ToString();
            string authKey = TestContext.Properties["authKey"].ToString();
            Console.WriteLine("Using endpoint: ", endpoint);
            DocumentDBRepository<Item>.Initialize(endpoint, authKey);
        }
        [TestMethod]
        public async Task TestCreateItemsAsync()
        {
            var item = new Item
            {
                Id = "1",
                Name = "testName",
                Description = "testDescription",
                Completed = false,
                Category = "testCategory"
            };

            // Create the item
            await DocumentDBRepository<Item>.CreateItemAsync(item);
            // Query for the item
            var returnedItem = await DocumentDBRepository<Item>.GetItemAsync(item.Id, item.Category);
            // Verify the item returned is correct.
            Assert.AreEqual(item.Id, returnedItem.Id);
            Assert.AreEqual(item.Category, returnedItem.Category);
        }

        [TestCleanup()]
        public void Cleanup()
        {
            DocumentDBRepository<Item>.Teardown();
        }
    }
}
```

Navigieren Sie zu den Ausführungsoptionen im Visual Studio-Testtask. Geben Sie unter **Einstellungsdatei** an, dass die Tests mit der Datei **.runsettings** konfiguriert sind. Fügen Sie unter **Testlaufparameter überschreiben** die Zeichenfolge ` -endpoint $(CosmosDbEmulator.Endpoint)` ein. Dadurch wird der Testtask so konfiguriert, dass er auf den Endpunkt des Emulator-Buildtasks und nicht auf den in der Datei **.runsettings** definierten Endpunkt verweist.  

![Überschreiben der Endpunktvariablen mit dem Endpunkt des Emulator-Buildtasks](./media/tutorial-setup-ci-cd/addExtension_5.png)

## <a name="run-the-build"></a>Führen Sie den Buildvorgang durch.
Speichern Sie nun den Build, und fügen Sie ihn der Warteschlange hinzu. 

![Speichern und Ausführen des Builds](./media/tutorial-setup-ci-cd/runBuild_1.png)

Nach dem Start des Builds sehen Sie, dass der Cosmos DB-Emulatortask damit begonnen hat, das Docker-Image mit dem installierten Emulator abzurufen. 

![Speichern und Ausführen des Builds](./media/tutorial-setup-ci-cd/runBuild_4.png)

Nach Abschluss des Builds sehen Sie, dass Ihre Tests, die für den Cosmos DB-Emulator aus dem Buildtask ausgeführt wurden, erfolgreich waren.

![Speichern und Ausführen des Builds](./media/tutorial-setup-ci-cd/buildComplete_1.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Verwenden des Emulators für lokale Entwicklungs- und Testvorgänge finden Sie unter [Verwenden des Azure Cosmos DB-Emulators für lokale Entwicklungs- und Testvorgänge](https://docs.microsoft.com/azure/cosmos-db/local-emulator).

Informationen zum Exportieren von SSL-Zertifikaten finden Sie unter [Exportieren der Azure Cosmos DB-Emulatorzertifikate für die Verwendung mit Java, Python und Node.js](https://docs.microsoft.com/azure/cosmos-db/local-emulator-export-ssl-certificates).
