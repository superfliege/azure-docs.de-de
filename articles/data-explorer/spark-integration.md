---
title: Azure Data Explorer-Connector für Apache Spark
description: In diesem Artikel wird die Verwendung des Azure Data Explorer-Connectors für Apache Spark erörtert.
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 02/05/2018
ms.openlocfilehash: 6f115df97d0b790c94d6dc07f3f40feeceb1a7cc
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55824971"
---
# <a name="azure-data-explorer-connector-for-apache-spark"></a>Azure Data Explorer-Connector für Apache Spark

Mit Azure Data Explorer und Apache Spark können Sie schnelle und skalierbare Anwendungen für datengesteuerte Szenarien erstellen. Beispiele dafür sind maschinelles Lernen (Machine Learning, ML), Extrahieren, Transformieren und Laden (Extract-Transform-Load, ETL) und Log Analytics.

## <a name="prerequisites"></a>Voraussetzungen

Um den Connector verwenden zu können, muss Ihre Anwendung über Folgendes verfügen:

* Java 1.8 SDK
* Maven 3.x
* Spark, Version 2.3.2 oder höher

## <a name="link-to-data-explorer"></a>Verbindung mit Data Explorer

Wenn Sie Scala- und Java-Anwendungen mit Maven-Projektdefinitionen verwenden, verknüpfen Sie Ihre Anwendung mit dem folgenden Artefakt:

```java
groupId = com.microsoft.azure
artifactId = spark-kusto-connector
version = 1.0.0-Beta-01 
```

Erstellen Sie in Maven die folgende Verknüpfung:

```Maven
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>spark-kusto-connector</artifactId>
    <version>1.0.0-Beta-01</version>
  </dependency>
```

## <a name="build-commands"></a>Erstellen von Befehlen

Führen Sie den folgenden Befehl aus, um die JAR-Datei zu erstellen und alle Tests auszuführen:

```
mvn clean package
```

Führen Sie den folgenden Befehl aus, um die JAR-Datei zu erstellen, alle Tests auszuführen und die JAR-Datei in Ihrem lokalen Maven-Repository zu installieren:

```
mvn clean install
```