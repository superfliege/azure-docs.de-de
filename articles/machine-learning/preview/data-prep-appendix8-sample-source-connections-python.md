---
title: "Beispiele für zusätzliche mögliche Quelldatenverbindungen für die Azure Machine Learning-Datenvorbereitung | Microsoft-Dokumentation"
description: "Dieses Dokument enthält eine Reihe von Beispielen für mögliche Quelldatenverbindungen für die Azure ML-Datenvorbereitung."
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: 
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 09/11/2017
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 550cca100314009f63eec2136e8c65426d8bf07f
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
# <a name="sample-of-custom-source-connections-python"></a>Beispiele für benutzerdefinierte Quellverbindungen (Python) 
Lesen Sie vor diesem Anhang den Artikel [Python-Erweiterbarkeit – Übersicht](data-prep-python-extensibility-overview.md).

## <a name="loading-data-from-dataworld"></a>Laden von Daten aus data.world

### <a name="prerequisites"></a>Voraussetzungen

#### <a name="register-yourself-at-dataworld"></a>Registrieren bei data.world
Ein API-Token ist erforderlich, das von der data.world-Website bezogen werden kann.

#### <a name="install-dataworld-library"></a>Installieren der data.world-Bibliothek

Öffnen Sie über _Datei > Befehlszeilenschnittstelle öffnen_ die Befehlszeilenschnittstelle von Azure Machine Learning Workbench.

```console
pip install git+git://github.com/datadotworld/data.world-py.git
```

Führen Sie in der Befehlszeile anschließend `dw configure` aus, woraufhin Sie zur Eingabe Ihres Tokens aufgefordert werden. Bei der Eingabe des Tokens wird in Ihrem Basisverzeichnis ein .dw/-Verzeichnis erstellt, und Ihr Token wird dort gespeichert.

```
API token (obtained at: https://data.world/settings/advanced): <enter API token here>
```
Nun sollten Sie data.world-Bibliotheken importieren können.

#### <a name="load-data-into-data-preparation"></a>Laden von Daten bei der Datenvorbereitung

Erstellen Sie einen neuen skriptbasierten Datenfluss, und laden Sie mithilfe des folgenden Skripts die Daten aus data.world.

```python
#paths = df['Path'].tolist()

import datadotworld as dw

#load the dataset
lds = dw.load_dataset('data-society/the-simpsons-by-the-data')

#Load specific data frame from the dataset
df = lds.dataframes['simpsons_episodes']

```

## <a name="load-cosmosdb-data-into-data-preparation"></a>Laden von CosmosDB-Daten bei der Datenvorbereitung

Erstellen Sie einen neuen skriptbasierten Datenfluss, und laden Sie mithilfe des folgenden Skripts die Daten aus CosmosDB. (Zuerst müssen die Bibliotheken installiert werden. Weitere Informationen hierzu finden Sie im oben verknüpften Referenzdokument.)

```python
import pydocumentdb
import pydocumentdb.document_client as document_client

import pandas as pd

config = { 
    'ENDPOINT': '<Endpoint>',
    'MASTERKEY': '<Key>',
    'DOCUMENTDB_DATABASE': '<DBName>',
    'DOCUMENTDB_COLLECTION': '<collectionname>'
};

# Initialize the Python DocumentDB client
client = document_client.DocumentClient(config['ENDPOINT'], {'masterKey': config['MASTERKEY']})

# Read databases and take first since id should not be duplicated.
db = next((data for data in client.ReadDatabases() if data['id'] == config['DOCUMENTDB_DATABASE']))

# Read collections and take first since id should not be duplicated.
coll = next((coll for coll in client.ReadCollections(db['_self']) if coll['id'] == config['DOCUMENTDB_COLLECTION']))

docs = client.ReadDocuments(coll['_self'])

df = pd.DataFrame(list(docs))
```

