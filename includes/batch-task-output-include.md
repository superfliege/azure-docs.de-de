---
title: Includedatei
description: Includedatei
services: batch
author: laurenhughes
ms.service: batch
ms.topic: include
ms.date: 04/06/2018
ms.author: lahugh
ms.custom: include file
ms.openlocfilehash: 7ba4c90811bd8051ed9c307d9f9fa33e08e69dc7
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/23/2019
ms.locfileid: "66127525"
---
Eine in Azure Batch ausgeführte Aufgabe kann während der Ausführung Ausgabedaten erzeugen. Oft müssen Aufgabenausgabedaten gespeichert werden, damit sie von anderen Aufgaben des Auftrags, von der Clientanwendung, die den Auftrag ausgeführt hat, oder von beiden abgerufen werden können. Aufgaben schreiben Ausgabedaten in das Dateisystem eines Batch-Computeknotens, doch alle Daten im Knoten gehen verloren, wenn ein Reimaging ausgeführt wird oder der Knoten den Pool verlässt. Außerdem können Aufgaben eine Vermerkdauer für Dateien aufweisen, nach der die von der Aufgabe erstellten Dateien gelöscht werden. Aus diesen Gründen es ist wichtig, Aufgabenausgaben, die Sie später benötigen, in einem Datenspeicher wie [Azure Storage](https://docs.microsoft.com/azure/storage/) beizubehalten.

Informationen zu den Optionen für Speicherkonten in Batch finden Sie unter [Entwickeln von parallelen Computelösungen in größerem Umfang mit Batch](../articles/batch/batch-api-basics.md#azure-storage-account).
