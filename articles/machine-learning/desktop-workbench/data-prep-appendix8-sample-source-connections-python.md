---
title: Beispiele für zusätzliche mögliche Quelldatenverbindungen für die Azure Machine Learning-Datenvorbereitung | Microsoft-Dokumentation
description: Dieses Dokument enthält eine Reihe von Beispielen für Quelldatenverbindungen, die mit der Azure Machine Learning-Datenvorbereitung möglich sind.
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: desktop-workbench
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/01/2018
ms.openlocfilehash: b87d88a5bb7846894c425e701a073707ddd1f3be
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2018
ms.locfileid: "34831184"
---
# <a name="sample-of-custom-source-connections-python"></a>Beispiele für benutzerdefinierte Quellverbindungen (Python) 
Lesen Sie vor diesem Anhang den Artikel [Python-Erweiterungen für die Datenvorbereitung](data-prep-python-extensibility-overview.md).

## <a name="load-data-from-dataworld"></a>Laden von Daten aus „data.world“

### <a name="prerequisites"></a>Voraussetzungen

#### <a name="register-yourself-at-dataworld"></a>Registrieren bei data.world
Sie benötigen ein API-Token von der data.world-Website.

#### <a name="install-dataworld-library"></a>Installieren der data.world-Bibliothek

Öffnen Sie über **File** > **Open command-line interface** die Befehlszeilenschnittstelle von Azure Machine Learning Workbench.

```console
pip install git+git://github.com/datadotworld/data.world-py.git
```

Führen Sie in der Befehlszeile anschließend `dw configure` aus, woraufhin Sie zur Eingabe Ihres Tokens aufgefordert werden. Bei der Eingabe des Tokens wird in Ihrem Basisverzeichnis ein .dw/-Verzeichnis erstellt, und Ihr Token wird dort gespeichert.

```
API token (obtained at: https://data.world/settings/advanced): <enter API token here>
```
Nun sollten Sie data.world-Bibliotheken importieren können.

#### <a name="load-data-into-data-preparation"></a>Laden von Daten bei der Datenvorbereitung

Erstellen Sie eine Transformation „Datenfluss transformieren (Skript)“. Laden Sie dann mithilfe des folgenden Skripts Daten aus „data.world“.

```python
#paths = df['Path'].tolist()

import datadotworld as dw

#Load the dataset.
lds = dw.load_dataset('data-society/the-simpsons-by-the-data')

#Load specific data frame from the dataset.
df = lds.dataframes['simpsons_episodes']

```
## <a name="azure-cosmos-db-as-a-data-source-connection"></a>Azure Cosmos DB als Datenquellenverbindung
Ein Beispiel für Azure Cosmos DB als Datenverbindung finden Sie unter [Laden von Azure Cosmos DB als Quelldatenverbindung](data-prep-load-azure-cosmos-db.md).
