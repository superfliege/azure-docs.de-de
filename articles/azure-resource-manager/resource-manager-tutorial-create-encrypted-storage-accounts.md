---
title: Erstellen einer Azure Resource Manager-Vorlage für die Bereitstellung eines verschlüsselten Speicherkontos | Microsoft-Dokumentation
description: Verwenden Sie Visual Studio Code, um eine Vorlage für die Bereitstellung eines verschlüsselten Speicherkontos zu erstellen.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 09/07/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: d48374d7919be3d141ea199e8238a220dbfe0332
ms.sourcegitcommit: 3150596c9d4a53d3650cc9254c107871ae0aab88
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/28/2018
ms.locfileid: "47419524"
---
# <a name="tutorial-create-an-azure-resource-manager-template-for-deploying-an-encrypted-storage-account"></a>Tutorial: Erstellen einer Azure Resource Manager-Vorlage für die Bereitstellung eines verschlüsselten Speicherkontos

Hier erfahren Sie, wie Sie Informationen zum Ausführen einer Azure Resource Manager-Vorlage finden.

In diesem Tutorial verwenden Sie eine Basisvorlage aus Azure-Schnellstartvorlagen, um ein Azure Storage-Konto zu erstellen.  Anhand der Vorlagenreferenzdokumentation passen Sie die Basisvorlage an, um ein verschlüsseltes Speicherkonto zu erstellen.

Dieses Tutorial enthält die folgenden Aufgaben:

> [!div class="checklist"]
> * Öffnen einer Schnellstartvorlage
> * Kennenlernen der Vorlage
> * Bearbeiten der Vorlage
> * Bereitstellen der Vorlage

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie die Anweisungen in diesem Artikel ausführen können, benötigen Sie Folgendes:

* [Visual Studio Code](https://code.visualstudio.com/).
* Die Erweiterung „Azure Resource Manager-Tools“. Informationen zur Installation finden unter [Schnellstart: Erstellen von Azure Resource Manager-Vorlagen mit Visual Studio Code](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).

## <a name="open-a-quickstart-template"></a>Öffnen einer Schnellstartvorlage

Die in dieser Schnellstartanleitung verwendete Vorlage heißt [Standardspeicherkonto erstellen](https://azure.microsoft.com/resources/templates/101-storage-account-create/). Die Vorlage definiert eine Azure Storage-Kontoressource.

1. Wählen Sie in Visual Studio Code **Datei**>**Datei öffnen** aus.
2. Fügen Sie in **Dateiname** die folgende URL ein:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```
3. Wählen Sie **Öffnen** aus, um die Datei zu öffnen.
4. Wählen Sie **Datei**>**Speichern unter** aus, um die Datei als **azuredeploy.json** auf dem lokalen Computer zu speichern.

## <a name="understand-the-format"></a>Verstehen des Formats

Reduzieren Sie in VS Code die Vorlage auf die Stammebene. Sie besitzen die einfachste Struktur mit den folgenden Elementen:

![Einfachste Struktur der Resource Manager-Vorlage](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-simplest-structure.png)

* **$schema**: Geben Sie den Speicherort der JSON-Schemadatei an, die die Version der Vorlagensprache beschreibt.
* **contentVersion**: Geben Sie einen beliebigen Wert für dieses Element an, um wichtige Änderungen an der Vorlage zu dokumentieren.
* **parameters**: Geben Sie die Werte an, die bei der Bereitstellung angegeben werden, um die Bereitstellung der Ressourcen anzupassen.
* **variables**: Geben Sie die Werte an, die als JSON-Fragmente in der Vorlage verwendet werden, um Vorlagensprachausdrücke zu vereinfachen.
* **resources**: Geben Sie die Ressourcentypen an, die in einer Ressourcengruppe bereitgestellt oder aktualisiert werden.
* **outputs**: Geben Sie die Werte an, die nach der Bereitstellung zurückgegeben werden.

## <a name="use-parameters-in-template"></a>Verwenden von Parametern in der Vorlage

Mit Parametern können Sie die Bereitstellung anpassen, indem Sie Werte angeben, die für eine bestimmte Umgebung maßgeschneidert sind. Sie verwenden die in der Vorlage definierten Parameter beim Festlegen von Werten für das Speicherkonto.

![Resource Manager-Vorlagenparameter](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-parameters.png)

In dieser Vorlage werden zwei Parameter definiert. Beachten Sie, dass in „location.defaultValue“ eine Vorlagenfunktion verwendet wird:

```json
"defaultValue": "[resourceGroup().location]",
```

Die Funktion „resourceGroup()“ gibt ein Objekt zurück, das die aktuelle Ressourcengruppe darstellt. Unter [Vorlagenfunktionen im Azure-Ressourcen-Manager](./resource-group-template-functions.md) finden Sie eine Liste der Vorlagenfunktionen.

So verwenden Sie die in der Vorlage definierten Parameter:

```json
"location": "[parameters('location')]",
"name": "[parameters('storageAccountType')]"
```

## <a name="use-variables-in-template"></a>Verwenden von Variablen in der Vorlage

Mit Variablen können Sie Werte erstellen, die in der ganzen Vorlage verwendet werden können. Variablen helfen Ihnen, die Komplexität der Vorlagen zu verringern.

![Resource Manager-Vorlagenvariablen](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-variables.png)

Diese Vorlage definiert eine Variable: *storageAccountName*. In der Definition werden zwei Vorlagenfunktionen verwendet:

- **concat()**: Verkettet Zeichenfolgen. Weitere Informationen finden Sie unter [concat](./resource-group-template-functions-string.md#concat).
- **uniqueString()**: Erstellt auf der Grundlage der als Parameter angegebenen Werte eine deterministische Hashzeichenfolge. Jedes Azure-Speicherkonto muss einen eindeutigen Namen in Azure besitzen. Diese Funktion stellt eine eindeutige Zeichenfolge bereit. Weitere Zeichenfolgenfunktionen finden Sie unter [Zeichenfolgenfunktionen für Azure Resource Manager-Vorlagen](./resource-group-template-functions-string.md).

So verwenden Sie die in der Vorlage definierte Variable:

```json
"name": "[variables('storageAccountName')]"
```

## <a name="edit-the-template"></a>Bearbeiten der Vorlage

In diesem Tutorial wird eine Vorlage zum Erstellen eines verschlüsselten Speicherkontos definiert.  Die Beispielvorlage erstellt nur ein einfaches unverschlüsseltes Speicherkonto. Sie können die Vorlagenreferenz des Azure Storage-Kontos verwenden, um die verschlüsselungsbezogene Konfiguration zu ermitteln.

1. Navigieren Sie zu [Azure-Vorlagen](https://docs.microsoft.com/azure/templates/).
2. Geben Sie in **Filtern nach Titel** **Speicherkonten** ein.
3. Wählen Sie **Referenz/Vorlagenreferenz/Speicher/Speicherkonten** wie im folgenden Screenshot gezeigt aus:

    ![Resource Manager: Vorlagenreferenz -> Speicherkonto](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-resources-reference-storage-accounts.png)

    resource-manager-template-resources-reference-storage-accounts
1. Untersuchen Sie die verschlüsselungsbezogenen Informationen.  
1. Fügen Sie im properties-Element der Ressourcendefinition für das Speicherkonto den folgenden JSON-Code ein:

    ```json
    "encryption": {
        "keySource": "Microsoft.Storage",
        "services": {
            "blob": {
                "enabled": true
            }
        }
    }
    ```
    Dieser Teil aktiviert die Verschlüsselungsfunktion des Blobspeicherdiensts.

Ändern Sie die Vorlage in Visual Studio Code, sodass das endgültige Ressourcenelement wie folgt aussieht:

![Verschlüsselte Speicherkontoressourcen der Resource Manager-Vorlage](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-encrypted-storage-resources.png)

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Informationen zum Bereitstellungsverfahren finden Sie in der Visual Studio Code-Schnellstartanleitung im Abschnitt [Bereitstellen der Vorlage](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#deploy-the-template).

Der folgende Screenshot zeigt den CLI-Befehl zum Auflisten des neu erstellten Speicherkontos, um zu sehen, dass die Verschlüsselung für den Blobspeicher aktiviert wurde.

![Verschlüsseltes Azure Resource Manager-Speicherkonto](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-encrypted-storage-account.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Azure-Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe, um die bereitgestellten Ressourcen zu bereinigen.

1. Wählen Sie im Azure-Portal im linken Menü die Option **Ressourcengruppe** aus.
2. Geben Sie den Namen der Ressourcengruppe in das Feld **Nach Name filtern** ein.
3. Wählen Sie den Namen der Ressourcengruppe aus.  Es werden insgesamt sechs Ressourcen in der Ressourcengruppe angezeigt.
4. Wählen Sie **Ressourcengruppe löschen** aus dem Menü ganz oben aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie die Vorlagenreferenz zum Anpassen einer vorhandenen Vorlage verwenden. Wie Sie mehrere Speicherkontoinstanzen erstellen, erfahren Sie unter:

> [!div class="nextstepaction"]
> [Erstellen mehrerer Instanzen](./resource-manager-tutorial-create-multiple-instances.md)
