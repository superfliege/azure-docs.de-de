---
title: Azure Machine Learning und FPGAs
description: Hier erfahren Sie, wie Sie Modelle und Deep Neural Networks mit FPGAs beschleunigen.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: tedway
author: tedway
ms.date: 05/07/2018
ms.openlocfilehash: 493b3f8de617146af534349e18ed49b83c46dee8
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="what-is-a-field-programmable-gate-array-fpga"></a>Was ist ein Universalschaltkreis (Field Programmable Gate Array, FPGA)?

FPGAs sind integrierte Schaltkreise, die nach Bedarf neu konfiguriert werden können. Sie können einen FPGA gemäß Ihren Anforderungen anpassen, um neue Logik zu implementieren. Hardwarebeschleunigte Azure Machine Learning-Modelle ermöglichen die Bereitstellung trainierter Modelle in FPGAs in der Azure-Cloud.

Diese Funktion wird von „Project Brainwave“ unterstützt und verarbeitet die Übersetzung von Deep Neural Networks (DNN) in ein FPGA-Programm. 

## <a name="why-use-an-fpga"></a>Gründe für die Verwendung eines FPGA

FPGAs bieten äußerst geringe Wartezeiten und eignen sich insbesondere zum Bewerten von Daten kleinster Batchgrößen (kein Bedarf für große Batches).  Sie sind kostengünstig und in Azure verfügbar.  „Project Brainwave“ kann vortrainierte DNNs FPGA-übergreifend parallelisieren, um Ihren Dienst horizontal hochzuskalieren.

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen eines Modells als Webdienst in einem FPGA](how-to-deploy-fpga-web-service.md)

[Informationen zum Installieren des FPGA SDK](reference-fpga-package-overview.md)