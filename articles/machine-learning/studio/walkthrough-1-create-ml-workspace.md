---
title: 'Schritt 1: Erstellen eines Machine Learning Studio-Arbeitsbereichs | Microsoft-Dokumentation'
description: 'Exemplarische Vorgehensweise zum Entwickeln einer Vorhersagelösung – Schritt 1: Erfahren Sie, wie Sie einen neuen Azure Machine Learning Studio-Arbeitsbereich einrichten.'
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: (previous ms.author=hshapiro, author=heatherbshapiro)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: b3c97e3d-16ba-4e42-9657-2562854a1e04
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.openlocfilehash: 0990fb6f8166be516b32e1f457f868be16dedcb2
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2018
ms.locfileid: "52316959"
---
# <a name="walkthrough-step-1-create-a-machine-learning-studio-workspace"></a>Exemplarische Vorgehensweise, Schritt 1: Erstellen eines Machine Learning Studio-Arbeitsbereichs
Dies ist der erste Schritt der exemplarischen Vorgehensweise zum [Entwickeln einer Predictive Analytics-Lösung mit Azure Machine Learning](walkthrough-develop-predictive-solution.md).

1. **Erstellen eines Machine Learning-Arbeitsbereichs**
2. [Hochladen vorhandener Daten](walkthrough-2-upload-data.md)
3. [Erstellen eines neuen Experiments](walkthrough-3-create-new-experiment.md)
4. [Trainieren und Bewerten der Modelle](walkthrough-4-train-and-evaluate-models.md)
5. [Bereitstellen des Webdiensts](walkthrough-5-publish-web-service.md)
6. [Zugreifen auf den Webdienst](walkthrough-6-access-web-service.md)

- - -
<!-- This needs to be updated to refer to the new way of creating workspaces in the Ibiza portal -->

Um Machine Learning Studio verwenden zu können, benötigen Sie einen Microsoft Azure Machine Learning-Arbeitsbereich. Dieser Arbeitsbereich enthält die Tools, die zum Erstellen, Verwalten und Veröffentlichen von Experimenten erforderlich sind.  

Der Administrator für Ihr Azure-Abonnement muss den Arbeitsbereich erstellen und Sie dann als Besitzer oder Mitwirkenden hinzufügen. Details finden Sie unter [Erstellen und Freigeben eines Azure Machine Learning-Arbeitsbereichs](create-workspace.md).

Nachdem Ihr Arbeitsbereich erstellt wurde, öffnen Sie Machine Learning Studio ([https://studio.azureml.net/Home](https://studio.azureml.net/Home)). Wenn Sie über mehrere Arbeitsbereiche verfügen, können Sie den Arbeitsbereich im Fenster oben rechts auf der Symbolleiste auswählen.

![Auswählen eines Arbeitsbereichs in Studio][2]

> [!TIP]
> Wenn Sie als Besitzer des Arbeitsbereichs hinzugefügt wurden, können Sie die Experimente, an denen Sie arbeiten, mit anderen Personen teilen, indem Sie sie zum Arbeitsbereich einladen. Dies können Sie in Machine Learning Studio auf der Seite **EINSTELLUNGEN** vornehmen. Sie benötigen nur das Microsoft-Konto oder Organisationskonto des betreffenden Benutzers.
> 
> Klicken Sie auf der Seite **EINSTELLUNGEN** auf **BENUTZER** und dann unten in Fenster auf **INVITE MORE USERS** (WEITERE BENUTZER EINLADEN).
> 
> 

- - -
**Nächster Schritt: [Hochladen vorhandener Daten](walkthrough-2-upload-data.md)**

[1]: ./media/walkthrough-1-create-ml-workspace/create1.png
[2]: ./media/walkthrough-1-create-ml-workspace/open-workspace.png
