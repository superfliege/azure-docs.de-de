---
title: Includedatei
description: Includedatei
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 01/22/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 3d100ec08b7b6d70366e605daf9c67edc6ab3bf3
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/16/2019
ms.locfileid: "56331067"
---
Storage Premium-Konten weisen folgende Skalierbarkeitsziele auf:

| Gesamtkapazität des Kontos | Gesamtbandbreite für ein lokal redundantes Speicherkonto |
| --- | --- | 
| Datenträgerkapazität: 35 TB <br>Kapazität für Momentaufnahmen: 10 TB | Bis zu 50 Gigabit pro Sekunde für eingehenden<sup>1</sup> und ausgehenden<sup>2</sup> Datenverkehr |

<sup>1</sup> Alle Daten (Anforderungen), die an ein Speicherkonto gesendet werden

<sup>2</sup> Alle Daten (Antworten), die von einem Speicherkonto empfangen werden

Wenn Sie Storage Premium-Konten für nicht verwaltete Datenträger verwenden und Ihre Anwendung die Skalierbarkeitsziele eines einzelnen Speicherkontos überschreitet, sollten Sie die Migration zu verwalteten Datenträgern erwägen. Falls die Migration zu verwalteten Datenträgern für Sie keine Option ist, können Sie Ihre Anwendung für die Nutzung mehrerer Speicherkonten erstellen. Partitionieren Sie Ihre Daten dann basierend auf diesen Speicherkonten. Wenn Sie beispielsweise 51-TB-Datenträger an mehrere VMs anfügen möchten, können Sie sie auf zwei Speicherkonten verteilen. 35 TB ist der Grenzwert pro Storage Premium-Konto. Stellen Sie sicher, dass ein Storage Premium-Konto stets Datenträger mit einer Größe von maximal 35 TB enthält.