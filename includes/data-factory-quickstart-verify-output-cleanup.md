---
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 11/09/2018
ms.author: jingwang
ms.openlocfilehash: 831a72fff0931d116a669060b160f51fde6e1d3e
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2018
ms.locfileid: "51572188"
---
## <a name="verify-the-output"></a>Überprüfen der Ausgabe
Die Pipeline erstellt den Ausgabeordner automatisch im Blobcontainer „adftutorial“. Anschließend wird die Datei „emp.txt“ aus dem Eingabe- in den Ausgabeordner kopiert. 

1. Klicken Sie im Azure-Portal auf der Seite des Containers **adftutorial** auf **Aktualisieren**, um den Ausgabeordner anzuzeigen. 
    
    ![Aktualisieren](media/data-factory-quickstart-verify-output-cleanup/output-refresh.png)
2. Klicken Sie in der Ordnerliste auf **Ausgabe**. 
2. Überprüfen Sie, ob die Datei **emp.txt** in den Ausgabeordner kopiert wurde. 

    ![Aktualisieren](media/data-factory-quickstart-verify-output-cleanup/output-file.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Die im Rahmen dieser Schnellstartanleitung erstellten Ressourcen können auf zwei Arten bereinigt werden. Sie können die [Azure-Ressourcengruppe](../articles/azure-resource-manager/resource-group-overview.md) einschließlich aller darin enthaltenen Ressourcen löschen. Falls die anderen Ressourcen erhalten bleiben sollen, löschen Sie nur die Data Factory, die Sie in diesem Tutorial erstellt haben.

Wenn Sie eine Ressourcengruppe löschen, werden alle Ressourcen einschließlich enthaltener Data Factorys gelöscht. Führen Sie den folgenden Befehl aus, um die gesamte Ressourcengruppe zu löschen: 
```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

Hinweis: Das Löschen einer Ressourcengruppe kann einige Zeit in Anspruch nehmen. Bitte haben Sie etwas Geduld.

Wenn Sie nur die Data Factory und nicht die gesamte Ressourcengruppe löschen möchten, führen Sie den folgenden Befehl aus: 

```powershell
Remove-AzureRmDataFactoryV2 -Name $dataFactoryName -ResourceGroupName $resourceGroupName
```