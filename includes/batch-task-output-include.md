---
title: Includedatei
description: Includedatei
services: batch
author: dlepow
ms.service: batch
ms.topic: include
ms.date: 04/06/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 0dcb608553d4455403c073e34e83bccb81cc64db
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2018
---
Eine in Azure Batch ausgeführte Aufgabe kann während der Ausführung Ausgabedaten erzeugen. Oft müssen Aufgabenausgabedaten gespeichert werden, damit sie von anderen Aufgaben des Auftrags, von der Clientanwendung, die den Auftrag ausgeführt hat, oder von beiden abgerufen werden können. Aufgaben schreiben Ausgabedaten in das Dateisystem eines Batch-Computeknotens, doch alle Daten im Knoten gehen verloren, wenn ein Reimaging ausgeführt wird oder der Knoten den Pool verlässt. Außerdem können Aufgaben eine Vermerkdauer für Dateien aufweisen, nach der die von der Aufgabe erstellten Dateien gelöscht werden. Aus diesen Gründen es ist wichtig, Aufgabenausgaben, die Sie später benötigen, in einem Datenspeicher wie [Azure Storage](https://docs.microsoft.com/azure/storage/) beizubehalten.

Informationen zu den Optionen für Speicherkonten in Batch finden Sie unter [Entwickeln von parallelen Computelösungen in größerem Umfang mit Batch](../articles/batch/batch-api-basics.md#azure-storage-account).
