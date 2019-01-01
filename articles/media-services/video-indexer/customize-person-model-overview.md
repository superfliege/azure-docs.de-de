---
title: 'Anpassen eines Personenmodells in Video Indexer: Azure'
titlesuffix: Azure Media Services
description: Dieser Artikel gibt einen Überblick darüber, was ein Personenmodell in Video Indexer ist und wie Sie es anpassen können.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.topic: article
ms.date: 12/05/2018
ms.author: anzaman
ms.openlocfilehash: 073cff22f17f496c2ff85cfbf716751dfea1e03e
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2018
ms.locfileid: "53283236"
---
# <a name="customize-a-person-model-in-video-indexer"></a>Anpassen eines Personenmodells in Video Indexer


Video Indexer unterstützt Gesichtserkennung und Erkennung bekannter Personen für Videoinhalte. Die Funktion zur Erkennung von Prominenten umfasst ungefähr 1.000.000 Gesichter, die auf häufig angeforderten Datenquellen wie IMDB, Wikipedia und den wichtigsten LinkedIn-Influencern basieren. Gesichter, die von der Funktion zur Erkennung von Prominenten nicht erkannt werden, werden erfasst, bleiben aber unbenannt. Nachdem Sie Ihr Video in Video Indexer hochgeladen und die Ergebnisse zurückerhalten haben, können Sie die Gesichter benennen, die zwar erfasst, aber nicht erkannt wurden. Sobald Sie ein Gesicht mit einem Namen versehen haben, werden Gesicht und Name dem Personenmodell Ihres Kontos hinzugefügt. Video Indexer erkennt dieses Gesicht dann in Ihren zukünftigen und früheren Videos.

Sie können die Video Indexer-Website oder -API verwenden, um Gesichter zu bearbeiten, die in einem Video in Ihrem Konto erkannt wurden. Dies wird in den folgenden Themen beschrieben:

- [Anpassen des Personenmodells mit APIs](customize-person-model-with-api.md)
- [Anpassen des Personenmodells mithilfe der Website](customize-person-model-with-website.md)
