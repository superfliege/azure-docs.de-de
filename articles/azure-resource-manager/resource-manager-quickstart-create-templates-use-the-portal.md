---
title: Erstellen und Bereitstellen einer Azure Resource Manager-Vorlage über das Azure-Portal | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie über das Azure-Portal Ihre erste Azure Resource Manager-Vorlage erstellen und bereitstellen.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 10/16/2018
ms.topic: quickstart
ms.author: jgao
ms.openlocfilehash: 8094105fd14ef13dd5f6b892425608806ca4dbd2
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49378032"
---
# <a name="quickstart-create-and-deploy-azure-resource-manager-templates-by-using-the-azure-portal"></a>Schnellstart: Erstellen und Bereitstellen von Azure Resource Manager-Vorlagen über das Azure-Portal

Hier erfahren Sie, wie Sie Ihre erste Azure Resource Manager-Vorlage erstellen, indem Sie über das Azure-Portal eine Vorlage erstellen diese dann über das Portal bearbeiten und bereitstellen. Resource Manager-Vorlagen sind JSON-Dateien, mit denen die Ressourcen definiert werden, die Sie für Ihre Lösung bereitstellen müssen. In diesem Tutorial wird ein Azure Storage-Konto erstellt. Der gleiche Prozess kann aber auch zur Erstellung anderer Azure-Ressourcen verwendet werden.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="generate-a-template-using-the-portal"></a>Generieren einer Vorlage über das Portal

In diesem Abschnitt erstellen Sie ein Speicherkonto über das Azure-Portal. Vor dem Bereitstellen des Speicherkontos haben Sie die Möglichkeit, sich die Vorlage anzusehen, die durch das Portal auf der Grundlage Ihrer Konfigurationen generiert wurde. Die Vorlage kann gespeichert und später wiederverwendet werden.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie auf **Ressource erstellen** > **Speicher** > **Speicherkonto – Blob, Datei, Tabelle, Warteschlange**.

    ![Erstellen eines Azure-Speicherkontos über das Azure-Portal](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account-portal.png)
3. Geben Sie Folgendes ein: 

    - **Ressourcengruppe**: Erstellen Sie eine neue Azure-Ressourcengruppe mit einem Namen Ihrer Wahl. Auf dem Screenshot lautet der Name der Ressourcengruppe *mystorage1016rg*.
    - **Name**: Geben Sie einen eindeutigen Namen für Ihr Speicherkonto ein. Auf dem Screenshot lautet der Name *mystorage1016*.

    Für die restlichen Eigenschaften können Sie die Standardwerte verwenden.

    ![Erstellen einer Azure-Speicherkontokonfiguration über das Azure-Portal](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account.png)

    > [!NOTE]
    > Bei manchen der exportierten Vorlagen sind vor der Bereitstellung noch einige Änderungen erforderlich.

4. Wählen Sie am unteren Bildschirmrand **Überprüfen + erstellen** aus. 
5. Wählen Sie am unteren Bildschirmrand **Download a template for automation** (Herunterladen einer Vorlage für die Automatisierung) aus. Das Portal zeigt die generierte Vorlage:

    ![Generieren einer Vorlage über das Portal](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account-template.png)

    Im Hauptbereich wird die Vorlage angezeigt. Dabei handelt es sich um eine JSON-Datei mit vier übergeordneten Elementen: `schema`, `contentVersion`, `parameters` und `resources`. Weitere Informationen finden Sie unter [Verstehen der Struktur und Syntax von Azure Resource Manager-Vorlagen](./resource-group-authoring-templates.md).

    Sechs Parameter wurden definiert. Einer davon heißt **storageAccountName**. Der zweite hervorgehobene Teil veranschaulicht, wie dieser Parameter in der Vorlage zu verwenden ist. Im nächsten Abschnitt bearbeiten Sie die Vorlage, um einen generierten Name für das Speicherkonto zu verwenden.

    In der Vorlage ist eine Azure-Ressource definiert. Der Typ ist [Microsoft.Storage/storageAccounts]. Sehen Sie sich die Definition der Ressource und die Ressourcenstruktur an.
6. Wählen Sie **Herunterladen** aus. Speichern Sie **template.json** aus dem heruntergeladenen Paket auf Ihrem Computer. Im nächsten Abschnitt verwenden Sie ein Bereitstellungstool für Vorlagen, um die Vorlage zu bearbeiten.
7. Wählen Sie die Registerkarte **Parameter**, um die von Ihnen angegebenen Werte für die Parameter anzuzeigen. Notieren Sie diese Werte, da Sie diese im nächsten Abschnitt benötigen, wenn Sie die Vorlage bereitstellen.

    ![Generieren einer Vorlage über das Portal](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account-template-parameters.png)

    Mit der Kombination aus Vorlagen- und Parameterdatei können Sie ein Azure-Speicherkonto erstellen.

## <a name="edit-and-deploy-the-template"></a>Bearbeiten und Bereitstellen der Vorlage

Das Azure-Portal kann für einige grundlegende Bearbeitungen der Vorlage verwendet werden. In dieser Schnellstartanleitung verwenden Sie ein Portaltool namens *Vorlagenbereitstellung*. Komplexere Vorlagen sollten ggf. in [Visual Studio Code](./resource-manager-quickstart-create-templates-use-visual-studio-code.md) bearbeitet werden, da dort umfangreichere Bearbeitungsfunktionen zur Verfügung stehen.

Azure erfordert, dass jeder Azure-Dienst einen eindeutigen Namen aufweist. Die Bereitstellung schlägt fehl, wenn Sie einen bereits vorhandenen Speicherkontonamen eingeben. Um dieses Problem zu vermeiden, können Sie durch Aufrufen der Vorlagenfunktion `uniquestring()` einen eindeutigen Speicherkontonamen generieren.

1. Klicken Sie im Azure-Portal auf **Ressource erstellen**.
2. Geben Sie in **Marketplace durchsuchen** den Begriff **Vorlagenbereitstellung** ein, und drücken Sie dann die **EINGABETASTE**.
3. Wählen Sie **Vorlagenbereitstellung** aus.

    ![Azure Resource Manager-Vorlagenbibliothek](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-library.png)
4. Klicken Sie auf **Erstellen**.
5. Wählen Sie **Eigene Vorlage im Editor erstellen**.
6. Wählen Sie **Datei laden**, und befolgen Sie dann die Anweisungen zum Laden der Datei „template.json“, die Sie im vorherigen Abschnitt heruntergeladen haben.
7. Fügen Sie wie im folgenden Screenshot dargestellt eine Variable hinzu:

    ```json
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
    ```
    ![Azure-Ressourcen-Manager-Vorlagen](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-edit-storage-account-template-revised.png)

    Zwei Funktionen werden hier verwendet: `concat()` und `uniqueString()`.

8. Entfernen Sie den im vorherigen Screenshot hervorgehobenen Parameter **storageAccountName**.
9. Aktualisieren Sie das Element „name“ der Ressource **Microsoft.Storage/storageAccounts**, um anstelle des Parameters die neu definierte Variable zu verwenden:

    ```json
    "name": "[variables('storageAccountName')]",
    ```

    Die endgültige Vorlage sollte wie folgt aussehen:

    ```json
    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "location": {
                "type": "string"
            },
            "accountType": {
                "type": "string"
            },
            "kind": {
                "type": "string"
            },
            "accessTier": {
                "type": "string"
            },
            "supportsHttpsTrafficOnly": {
                "type": "bool"
            }
        },
        "variables": {
            "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
        },
        "resources": [
            {
                "name": "[variables('storageAccountName')]",
                "type": "Microsoft.Storage/storageAccounts",
                "apiVersion": "2018-07-01",
                "location": "[parameters('location')]",
                "properties": {
                    "accessTier": "[parameters('accessTier')]",
                    "supportsHttpsTrafficOnly": "[parameters('supportsHttpsTrafficOnly')]"
                },
                "dependsOn": [],
                "sku": {
                    "name": "[parameters('accountType')]"
                },
                "kind": "[parameters('kind')]"
            }
        ],
        "outputs": {}
    }
    ```
7. Wählen Sie **Speichern**aus.
8. Geben Sie die folgenden Werte ein:

    - **Ressourcengruppe**: Versehen Sie Ihre Ressourcengruppe mit einem eindeutigen Namen.
    - **Standort**: Wählen Sie einen Standort für die Ressourcengruppe aus.
    - **Standort**: Wählen Sie einen Standort für das Speicherkonto aus.  Sie können den gleichen Standort verwenden wie für die Ressourcengruppe.
    - **Kontotyp**: Geben Sie für diese Schnellstartanleitung **Standard_LRS** ein.
    - **Variante**: Geben Sie für diese Schnellstartanleitung **StorageV2** ein.
    - **Zugriffsebene**: Geben Sie für diese Schnellstartanleitung **Heiße Zugriffsebene** ein.
    - **Https Traffic Only Enabled** (Nur HTTPS-Datenverkehr aktiviert):  Wählen Sie für diese Schnellstartanleitung die Option **true** aus.
    - **Ich stimme den oben genannten Geschäftsbedingungen zu**: Aktivieren Sie dieses Kontrollkästchen.

    Hier sehen Sie einen Screenshot einer Beispielbereitstellung:

    ![Azure Resource Manager-Vorlagenbereitstellung](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-deploy.png)

10. Wählen Sie die Option **Kaufen**.
11. Klicken Sie am oberen Bildschirmrand auf das Glockensymbol (Benachrichtigungen), um den Bereitstellungsstatus anzuzeigen. Warten Sie, bis die Bereitstellung abgeschlossen wurde.

    ![Bereitstellung von Azure Resource Manager-Vorlagen – Benachrichtigung](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-portal-notification.png)

12. Wählen Sie im Benachrichtigungsbereich die Option **Zu Ressourcengruppe wechseln** aus. Ein Bildschirm wird angezeigt, der etwa wie folgt aussieht:

    ![Bereitstellung von Azure Resource Manager-Vorlagen – Ressourcengruppe](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-portal-deployment-resource-group.png)

    Sie können sehen, dass der Bereitstellungsstatus erfolgreich war und nur ein Speicherkonto in der Ressourcengruppe vorhanden ist. Der Name des Speicherkontos ist eine eindeutige Zeichenfolge, die von der Vorlage generiert wird. Weitere Informationen zur Verwendung von Azure-Speicherkonten finden Sie unter [Schnellstart: Hochladen, Herunterladen und Auflisten von Blobs über das Azure-Portal](../storage/blobs/storage-quickstart-blobs-portal.md).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Azure-Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe, um die bereitgestellten Ressourcen zu bereinigen.

1. Klicken Sie im linken Menü des Azure-Portals auf **Ressourcengruppe**.
2. Geben Sie den Namen der Ressourcengruppe in das Feld **Nach Name filtern** ein.
3. Klicken Sie auf den Namen der Ressourcengruppe.  Das Speicherkonto in der Ressourcengruppe wird angezeigt.
4. Klicken Sie im oberen Menü auf **Ressourcengruppe löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie über das Azure-Portal eine Vorlage generieren und anschließend bereitstellen. In dieser Schnellstartanleitung wurde eine einfache Vorlage mit einer einzelnen Azure-Ressource verwendet. Bei komplexeren Vorlagen empfiehlt es sich, die Vorlage unter Verwendung von Visual Studio Code oder Visual Studio zu entwickeln. In der nächsten Schnellstartanleitung wird darüber hinaus erläutert, wie Sie Vorlagen mithilfe von Azure PowerShell und der Azure-Befehlszeilenschnittstelle bereitstellen.

> [!div class="nextstepaction"]
> [Schnellstart: Erstellen von Azure Resource Manager-Vorlagen mit Visual Studio Code](./resource-manager-quickstart-create-templates-use-visual-studio-code.md)
