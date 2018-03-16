---
title: Namensfehler des Azure-Speicherkontos | Microsoft-Dokumentation
description: "Beschreibt die Fehler, die auftreten können, wenn Sie einen Speicherkontonamen angeben."
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: support-article
ms.date: 03/09/2018
ms.author: tomfitz
ms.openlocfilehash: c266e1073722733ec8b7353c6fdddc3ae341ab20
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="resolve-errors-for-storage-account-names"></a>Beheben von Fehlern bei Speicherkontonamen

Dieser Artikel beschreibt Benennungsfehler, die möglicherweise auftreten, wenn Sie ein Speicherkonto bereitstellen.

## <a name="symptom"></a>Symptom

Wenn der Name Ihres Speicherkontos unzulässige Zeichen enthält, erhalten Sie eine Fehlermeldung wie:

```
Code=AccountNameInvalid
Message=S!torageckrexph7isnoc is not a valid storage account name. Storage account name must be 
between 3 and 24 characters in length and use numbers and lower-case letters only.
```

Bei Speicherkonten müssen Sie einen Namen für die Ressource angeben, der in Azure eindeutig ist. Wenn Sie keinen eindeutigen Namen angeben, erhalten Sie einen Fehler wie diesen:

```
Code=StorageAccountAlreadyTaken
Message=The storage account named mystorage is already taken.
```

Wenn Sie ein Speicherkonto bereitstellen, das den gleichen Namen hat wie ein in Ihrem Abonnement vorhandenes Speicherkonto, aber einen anderen Speicherort angeben, erhalten Sie eine Fehlermeldung, dass das Speicherkonto bereits an einem anderen Speicherort existiert. Löschen Sie das vorhandene Speicherkonto, oder geben Sie den gleichen Speicherort wie für das vorhandene Speicherkonto an.

## <a name="cause"></a>Ursache

Speicherkontonamen müssen zwischen 3 und 24 Zeichen lang sein und dürfen nur Zahlen und Kleinbuchstaben enthalten. Der Name muss eindeutig sein.

## <a name="solution"></a>Lösung

Stellen Sie sicher, dass der Name des Speicherkontos eindeutig ist. Sie können einen eindeutigen Namen erstellen, indem Sie Ihre Benennungskonvention mit dem Ergebnis der [uniqueString](resource-group-template-functions-string.md#uniquestring) -Funktion verketten.

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]",
"type": "Microsoft.Storage/storageAccounts",
```

Stellen Sie sicher, dass der Name des Speicherkontos maximal 24 Zeichen lang ist. Die Funktion [uniqueString](resource-group-template-functions-string.md#uniquestring) gibt 13 Zeichen zurück. Wenn Sie ein Präfix oder Postfix mit dem Ergebnis von **uniqueString** (eindeutige Zeichenfolge) verknüpfen, sollte der Wert maximal 11 Zeichen lang sein.

```json
"parameters": {
    "storageNamePrefix": {
      "type": "string",
      "maxLength": 11,
      "defaultValue": "storage",
      "metadata": {
        "description": "The value to use for starting the storage account name."
      }
    }
}
```

Stellen Sie sicher, dass der Name des Speicherkontos keine Großbuchstaben oder Sonderzeichen enthält.