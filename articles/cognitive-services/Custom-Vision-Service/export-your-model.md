---
title: Exportieren von Custom Vision Service-Modellen auf Mobilgeräte – Custom Vision Service – Azure Cognitive Services | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Ihr Modell für die Erstellung von Mobilanwendungen exportieren.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/03/2018
ms.author: anroth
ms.openlocfilehash: ce8f42d6239867dd217cddfc61a27d7835dc9c9b
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "35378681"
---
# <a name="export-your-model-for-use-with-mobile-devices"></a>Exportieren Ihres Modells für die Verwendung mit Mobilgeräten

Custom Vision Service ermöglicht das Exportieren von Klassifizierern für die Offlineausführung. Sie können den exportierten Klassifizierer in eine Anwendung einbetten und lokal auf einem Gerät ausführen, um eine Klassifizierung in Echtzeit zu erhalten. 

Custom Vision Service unterstützt die folgenden Exporte:

* __Tensorflow__ für __Android__
* __Core ML__ für __iOS 11__
* __ONNX__ für __Windows ML__
* Einen Windows- oder Linux-__Container__. Der Container enthält ein Tensorflow-Modell und Dienstcode zur Verwendung der Custom Vision Service-API. 

> [!IMPORTANT]
> Custom Vision Service exportiert nur __kompakte__ Domänen. Die durch kompakte Domänen generierten Modelle sind für die Einschränkungen der Klassifizierung in Echtzeit auf Mobilgeräten optimiert. Mit einer kompakten Domäne erstellte Klassifizierer sind möglicherweise etwas weniger genau als eine Standarddomäne mit der gleichen Menge an Trainingsdaten.
>
> Informationen zur Verbesserung der Klassifizierer finden Sie im Dokument [Verbessern Ihrer Klassifizierung](getting-started-improving-your-classifier.md).

## <a name="convert-to-a-compact-domain"></a>Konvertieren in eine kompakte Domäne

> [!NOTE]
> Die Schritte in diesem Abschnitt gelten nur, wenn Sie einen vorhandenen Klassifizierer haben, der nicht als kompakte Domäne festgelegt ist.
 
Gehen Sie zum Konvertieren der Domäne einer vorhandenen Klassifizierung folgendermaßen vor:

1. Wählen Sie auf der [Custom Vision-Seite](https://customvision.ai) das Symbol __Home__ aus, um eine Liste Ihrer Projekte anzuzeigen. Sie können Ihre Projekte auch unter [https://customvision.ai/projects](https://customvision.ai/projects) einsehen.

    ![Abbildung des Symbols „Home“ und der Projektliste](./media/export-your-model/projects-list.png)

2. Wählen Sie ein Projekt und dann das __Zahnradsymbol__ in der rechten oberen Ecke der Seite aus.

    ![Abbildung des Zahnradsymbols](./media/export-your-model/gear-icon.png)

3. Wählen Sie im Abschnitt __Domänen__ eine __kompakte__ Domäne aus. Wählen Sie zum Speichern der Änderungen __Änderungen speichern__ aus.

    ![Abbildung der Domänenauswahl](./media/export-your-model/domains.png)

4. Wählen Sie im oberen Bereich der Seite __Trainieren__ aus, um das Training mit der neuen Domäne zu wiederholen.

## <a name="export-your-model"></a>Exportieren Ihres Modells

Um das Modell nach dem erneuten Training zu exportieren, gehen Sie folgendermaßen vor:

1. Wechseln Sie zur Registerkarte **Leistung**, und wählen Sie __Exportieren__ aus. 

    ![Abbildung des Symbols „Exportieren“](./media/export-your-model/export.png)

    > [!TIP]
    > Wenn der Eintrag __Exportieren__ nicht verfügbar ist, verwendet die ausgewählte Iteration keine kompakte Domäne. Wählen Sie im Abschnitt __Iterationen__ dieser Seite eine Iteration aus, die eine kompakte Domäne verwendet, und wählen Sie dann __Exportieren__ aus.

2. Wählen Sie das Exportformat aus, und wählen Sie dann __Exportieren__ aus, um das Modell herunterzuladen.

## <a name="next-steps"></a>Nächste Schritte

Integrieren Sie das exportierte Modell in eine Anwendung. Es sind mehrere Beispielanwendungen verfügbar:

* Beispiel für das [Verwenden des exportierten Core ML-Modells in einer iOS-Anwendung](https://go.microsoft.com/fwlink/?linkid=857726) für die Bildklassifizierung in Echtzeit mit Swift
* iOS-Beispielanwendung für das [Verwenden des exportierten Core ML-Modells mit Xamarin](https://github.com/xamarin/ios-samples/tree/master/ios11/CoreMLAzureModel) für die Bildklassifizierung in Echtzeit 
* Beispiel für das [Verwenden des exportierten Tensorflow-Modells in einer Android-Anwendung](https://github.com/Azure-Samples/cognitive-services-android-customvision-sample) für die Bildklassifizierung in Echtzeit 
* [Verwenden Ihres Tensorflow-Modells mit Windows](https://docs.microsoft.com/en-us/azure/cognitive-services/custom-vision-service/export-model-python)
* Beispiel für das [Verwenden des exportierten ONNX-Modells mit Windows Machine Learning](https://azure.microsoft.com/en-us/resources/samples/cognitive-services-onnx-customvision-sample/)
