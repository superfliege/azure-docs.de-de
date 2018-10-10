---
title: Includedatei
description: Includedatei
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 07/27/2018
ms.openlocfilehash: 12f0f17a7c25696d2c8ff5b427f4b103617e5678
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47010729"
---
Erstellen und aktivieren Sie in einem Eingabeaufforderungs- oder Shellfenster die Conda-Paket-Manager-Umgebung mit NumPy und Cython. In diesem Beispiel wird Python 3.6 verwendet.

  + Unter Windows:
       ```sh 
       conda create -n myenv Python=3.6 cython numpy
       conda activate myenv
       ```

  + Unter Linux oder macOS:
       ```sh 
       conda create -n myenv Python=3.6 cython numpy
       source activate myenv
       ```

Installieren Sie das SDK.
   ```sh 
   pip install azureml-sdk
   ```
