---
title: 'Schnellstart: Erkennen von Gesichtern in einem Bild mit der Azure-REST-API und Python'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung verwenden Sie die Azure-Gesichtserkennungs-REST-API mit Python, um Gesichter in einem Bild zu erkennen.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 11/09/2018
ms.author: pafarley
ms.openlocfilehash: 606f01059620443c8c42a24f416128d0856eff50
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55213785"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-face-rest-api-and-python"></a>Schnellstart: Erkennen von Gesichtern in einem Bild mit der Gesichtserkennungs-REST-API und Python

In dieser Schnellstartanleitung verwenden Sie die Azure-Gesichtserkennungs-REST-API mit Python, um menschliche Gesichter in einem Bild zu erkennen. Das Skript zeichnet Rahmen um die Gesichter und blendet Informationen zu Geschlecht und Alter im Bild ein.

![Ein Mann und eine Frau mit Rechtecken um die Gesichter und Anzeige von Alter und Geschlecht im Bild](../images/labelled-faces-python.png)

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen. 


## <a name="prerequisites"></a>Voraussetzungen

- Ein Abonnementschlüssel für die Gesichtserkennungs-API. Über die Seite [Cognitive Services ausprobieren](https://azure.microsoft.com/try/cognitive-services/?api=face-api) können Sie einen Abonnementschlüssel für eine kostenlose Testversion abrufen. Gehen Sie andernfalls wie unter [Schnellstart: Erstellen eines Cognitive Services-Kontos im Azure-Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) beschrieben vor, um den Gesichtserkennungs-API-Dienst zu abonnieren und Ihren Schlüssel zu erhalten.

## <a name="run-the-jupyter-notebook"></a>Ausführen von Jupyter Notebook

Sie können diesen Schnellstart als Jupyter Notebook auf [MyBinder](https://mybinder.org) ausführen. Klicken Sie zum Starten von Binder unten auf die Schaltfläche. Befolgen Sie dann die Anweisungen im Notebook.

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=FaceAPI.ipynb)

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich als Nächstes die Referenzdokumentation zur Gesichtserkennungs-API an, um mehr über die unterstützten Szenarien zu erfahren.

> [!div class="nextstepaction"]
> [Gesichtserkennungs-API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
