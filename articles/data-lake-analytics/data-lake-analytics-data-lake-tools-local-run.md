---
title: Ausführen von Azure Data Lake-U-SQL-Skripts auf dem lokalen Computer | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie U-SQL-Aufträge mithilfe von Azure Data Lake Tools für Visual Studio auf dem lokalen Computer ausführen.
services: data-lake-analytics
documentationcenter: ''
author: yanancai
manager: ''
editor: ''
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/03/2018
ms.author: yanacai
ms.openlocfilehash: a7f43c7e17f36d9b4e0767744eee9604c2628ea8
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/06/2018
ms.locfileid: "37888965"
---
# <a name="run-u-sql-script-on-your-local-machine"></a>Ausführen von U-SQL-Skripts auf dem lokalen Computer

Beim Entwickeln von U-SQL-Skripts ist es üblich, die U-SQL-Skripts lokal auszuführen, da dies Kosten und Zeit spart. In Azure Data Lake Tools für Visual Studio ist es möglich, U-SQL-Skripts auf dem lokalen Computer auszuführen. 

## <a name="basic-concepts-for-local-run"></a>Grundlegende Konzepte für die lokale Ausführung

In der folgenden Aufstellung sind die Komponenten für die lokale Ausführung und ihre Entsprechung bei der Ausführung in der Cloud aufgeführt.

|Komponente|Lokale Ausführung|Ausführung in der Cloud|
|---------|---------|---------|
|Speicher|Lokaler Datenstammordner|Azure Data Lake Store-Standardkonto|
|Compute|Engine für lokale U-SQL-Ausführung|Azure Data Lake Analytics-Dienst|
|Ausführungsumgebung|Arbeitsverzeichnis auf dem lokalen Computer|Azure Data Lake Analytics-Cluster|

Weitere Erklärungen zu den Komponenten für die lokale Ausführung:

### <a name="local-data-root-folder"></a>Lokaler Datenstammordner

Der lokale Datenstammordner ist ein „lokaler Speicher“ für das lokale Computekonto. Jeder Ordner im lokalen Dateisystem auf dem lokalen Computer kann als lokaler Datenstammordner dienen. Er entspricht dem Azure Data Lake Store-Standardkonto eines Data Lake Analytics-Kontos. Der Wechsel zu einem anderen Datenstammordner entspricht dem Wechsel zu einem anderen Standardspeicherkonto. 

Der Datenstammordner wird für Folgendes verwendet:
- Speichern von Metadaten, z.B. Datenbanken, Tabellen, Tabellenwertfunktionen und Assemblys.
- Suchen nach Eingabe- und Ausgabepfaden, die als relative Pfade in U-SQL-Skripts definiert sind. Das Verwenden relativer Pfade erleichtert das Bereitstellen von U-SQL-Skripts in Azure.

### <a name="u-sql-local-run-engine"></a>Engine für lokale U-SQL-Ausführung

Die Engine für die lokale U-SQL-Ausführung ist ein „lokales Computekonto“ für U-SQL-Aufträge. Benutzer können U-SQL-Aufträge über Azure Data Lake Tools für Visual Studio lokal ausführen. Die lokale Ausführung wird auch über die Befehlszeilen- und Programmierschnittstellen von Azure Data Lake U-SQL SDK unterstützt. [Erfahren Sie mehr über Azure Data Lake U-SQL SDK](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/).

### <a name="working-directory"></a>Arbeitsverzeichnis

Beim Ausführen eines U-SQL-Skripts ist ein Arbeitsverzeichnis zum Zwischenspeichern von Kompilierungsergebnissen, Ausführungsprotokollen usw. erforderlich. In Azure Data Lake Tools für Visual Studio ist das Arbeitsverzeichnis das Arbeitsverzeichnis des U-SQL-Projekts (befindet sich normalerweise unter `<U-SQL project root path>/bin/debug>`). Das Arbeitsverzeichnis wird jedes Mal bereinigt, wenn eine neue Ausführung ausgelöst wird.

## <a name="local-run-in-visual-studio"></a>Lokale Ausführung in Visual Studio

Azure Data Lake Tools für Visual Studio umfasst eine integrierte Engine für die lokale Ausführung, die als lokales Computekonto angezeigt wird. Um ein U-SQL-Skript lokal auszuführen, wählen Sie links im Skript-Editor in der Dropdownliste das Konto „(Local-machine)“ oder „(Local-project)“ aus, und klicken Sie auf **Senden**.

![Data Lake Tools für Visual Studio, Senden des Skripts an lokales Konto](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-submit-script-to-local-account.png) 
 
## <a name="local-run-with-local-machine-account"></a>Lokale Ausführung mit dem Konto „(Local-machine)“

Das Konto „(Local-machine)“ ist ein freigegebenes lokales Computekonto mit einem einzelnen lokalen Datenstammordner als lokales Speicherkonto. Der Datenstammordner befindet sich standardmäßig unter „C:\Benutzer\<Benutzername>\AppData\Local\USQLDataRoot“ und kann über **Extras > Data Lake > Optionen und Einstellungen** konfiguriert werden.

![Data Lake Tools für Visual Studio, Konfigurieren des lokalen Datenstammordners](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-configure-local-data-root.png)
  
Für die lokale Ausführung ist ein U-SQL-Projekt erforderlich. Das Arbeitsverzeichnis des U-SQL-Projekts wird als Arbeitsverzeichnis für die lokale U-SQL-Ausführung verwendet. Kompilierungsergebnisse, Ausführungsprotokolle und andere auf die Ausführung von Aufträgen bezogene Dateien werden während der lokalen Ausführung im Arbeitsverzeichnis generiert und gespeichert. Jedes Mal, wenn Sie das Skript erneut ausführen, werden alle diese Dateien im Arbeitsverzeichnis bereinigt und neu generiert.

## <a name="local-run-with-local-project-account"></a>Lokale Ausführung mit dem Konto „(Local-project)“

Das Konto „(Local-project)“ ist ein projektisoliertes lokales Computekonto für die einzelnen Projekte mit einem isolierten lokalen Datenstammordner. Bei jedem Öffnen eines aktiven U-SQL-Projekts im Projektmappen-Explorer wird ein entsprechendes `(Local-project: <project name>)`-Konto im Server-Explorer und am Rand des U-SQL-Skript-Editors angezeigt. 

Das Konto „(Local-project)“ bietet eine saubere und isolierte Entwicklungsumgebung für Entwickler. Im Gegensatz zum Konto „(Local-machine)“, das einen freigegebenen lokalen Datenstammordner enthält, in dem Metadaten und Eingabe- und Ausgabedaten für alle lokalen Aufträge gespeichert werden, wird beim Konto „(Local-project)“ jedes Mal, wenn ein U-SQL-Skript ausgeführt wird, ein temporärer lokaler Datenstammordner unter dem Arbeitsverzeichnis des U-SQL-Projekts erstellt. Dieser temporäre Datenstammordner wird bereinigt, wenn eine Neuerstellung oder erneute Ausführung erfolgt. 

In einem U-SQL-Projekt lässt sich diese isolierte Umgebung zur lokalen Ausführung gut über Projektverweise und -eigenschaften verwalten. Sie können sowohl die Eingabedatenquellen für U-SQL-Skripts im Projekt als auch die referenzierten Datenbankumgebungen konfigurieren.

### <a name="manage-input-data-source-for-local-project-account"></a>Verwalten der Eingabedatenquelle für das Konto „(Local-project)“

Die Erstellung des lokalen Datenstammordners und die Einrichtung der Daten für das Konto „(Local-project)“ erfolgt durch das U-SQL-Projekt. Bei jeder Neuerstellung und jeder lokalen Ausführung wird der temporäre Datenstammordner bereinigt und unter dem Arbeitsverzeichnis des U-SQL-Projekts neu erstellt. Alle durch das U-SQL-Projekt konfigurierten Datenquellen werden vor der Ausführung eines lokalen Auftrags in diesen temporären lokalen Datenstammordner kopiert. 

Sie können den Stammordner der Datenquellen über **Rechtsklick auf U-SQL-Projekt > Eigenschaft > Testdatenquelle** konfigurieren. Beim Ausführen eines U-SQL-Skripts im Konto „(Local-project)“ werden alle Dateien und Unterordner (einschließlich der Dateien in den Unterordnern) im Ordner **Testdatenquelle** in den temporären lokalen Datenstammordner kopiert. Nach der Ausführung des lokalen Auftrags befinden sich auch die Ausgabeergebnisse unter dem temporären lokalen Datenstammordners im Arbeitsverzeichnis des Projekts. Beachten Sie, dass alle diese Ausgaben gelöscht und bereinigt werden, wenn das Projekt neu erstellt und bereinigt wird. 

![Data Lake Tools für Visual Studio, Konfigurieren der Testdatenquelle des Projekts](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-configure-project-test-data-source.png)

### <a name="manage-referenced-database-environment-for-local-project-account"></a>Verwalten der referenzierten Datenbankumgebung für das Konto „(Local-project)“ 

Wenn bei einer U-SQL-Abfrage U-SQL-Datenbankobjekte verwendet oder abgefragt werden, müssen Sie vor dem lokalen Ausführen dieses U-SQL-Skripts die Datenbankumgebungen lokal einrichten. Für das Konto „(Local-project)“ können U-SQL-Datenbankabhängigkeiten durch U-SQL-Projektverweise verwaltet werden. Sie können dem U-SQL-Projekt U-SQL-Datenbankprojektverweise hinzufügen. Vor dem Ausführen von U-SQL-Skripts im Konto „(Local-project)“ werden alle referenzierten Datenbanken im temporären lokalen Datenstammordner bereitgestellt. Bei jeder Ausführung wird der temporäre Datenstammordner außerdem als neue isolierte Umgebung bereinigt.

Verwandte Artikel:
* [Informationen zum Verwalten einer U-SQL-Datenbankdefinition über ein U-SQL-Datenbankprojekt](data-lake-analytics-data-lake-tools-develop-usql-database.md#reference-a-u-sql-database-project)
* [Informationen zum Verwalten eines U-SQL-Datenbankverweises in einem U-SQL-Projekt](data-lake-analytics-data-lake-tools-develop-usql-database.md)

## <a name="difference-between-local-machine-and-local-project-account"></a>Unterschied zwischen dem Konto „(Local-machine)“ und dem Konto „(Local-project)“

Das Konto „(Local-machine)“ simuliert ein Azure Data Lake Analytics-Konto auf dem lokalen Computer von Benutzern. Es hat die gleiche Oberfläche wie ein Azure Data Lake Analytics-Konto. Das Konto „(Local-project)“ bietet eine benutzerfreundliche lokale Entwicklungsumgebung, in der Benutzer vor dem lokalen Ausführen des Skripts Datenbankverweise und Eingabedaten bereitstellen können. Das Konto „(Local-machine)“ bietet eine freigegebene permanente Umgebung, auf die über alle Projekte zugegriffen werden kann. Das Konto „(Local-project)“ bietet eine isolierte Entwicklungsumgebung für jedes Projekt und wird bei jeder Ausführung aktualisiert. Im Vergleich bietet das Konto „(Local-project)“ eine schnellere Entwicklungsumgebung, da neue Änderungen rasch angewandt werden.

Weitere Unterschiede zwischen dem Konto „(Local-machine)“ und dem Konto „(Local-project)“ finden Sie in der folgenden Tabelle:

|Unterschied|(Local-machine)|(Local-project)|
|----------------|---------------|---------------|
|Lokaler Zugriff|Der Zugriff kann über alle Projekte erfolgen.|Nur das entsprechende Projekt kann auf dieses Konto zugreifen.|
|Lokaler Datenstammordner|Ein permanenter lokaler Ordner. Wird über **Extras > Data Lake > Optionen und Einstellungen** konfiguriert.|Ein temporärer Ordner, der für jede lokale Ausführung unter dem Arbeitsverzeichnis des U-SQL-Projekts erstellt wird. Der Ordner wird bereinigt, wenn eine Neuerstellung oder erneute Ausführung erfolgt.|
|Eingabedaten für das U-SQL-Skript|Relativer Pfad unter dem permanenten lokalen Datenstammordner|Wird über **U-SQL-Projekteigenschaft > Testdatenquelle** festgelegt. Alle Dateien und Unterordner werden vor der lokalen Ausführung in den temporären Datenstammordner kopiert.|
|Ausgabedaten für das U-SQL-Skript|Relativer Pfad unter dem permanenten lokalen Datenstammordner|Werden in den temporären Datenstammordner ausgegeben. Die Ergebnisse werden bereinigt, wenn eine Neuerstellung oder erneute Ausführung erfolgt.|
|Bereitstellung von referenzierten Datenbanken|Referenzierte Datenbanken werden beim Ausführen im Konto „(Local-machine)“ nicht automatisch bereitgestellt. Dies gilt auch beim Senden in das Azure Data Lake Analytics-Konto.|Referenzierte Datenbanken werden vor der lokalen Ausführung automatisch im Konto „(Local-project)“ bereitgestellt. Alle Datenbankumgebungen werden bereinigt und neu bereitgestellt, wenn eine Neuerstellung oder erneute Ausführung erfolgt.|

## <a name="local-run-with-u-sql-sdk"></a>Lokale Ausführung mit U-SQL SDK

Neben der lokalen Ausführung von U-SQL-Skripts in Visual Studio können Sie auch Azure Data Lake U-SQL SDK verwenden, um U-SQL-Skripts lokal über die Befehlszeilen- und Programmierschnittstellen auszuführen. Über diese Schnittstellen können Sie lokale U-SQL-Ausführungen und U-SQL-Tests automatisieren.

[Erfahren Sie mehr über Azure Data Lake U-SQL SDK](data-lake-analytics-u-sql-sdk.md).

## <a name="next-steps"></a>Nächste Schritte

- [Azure Data Lake U-SQL SDK](data-lake-analytics-u-sql-sdk.md)
- [Einrichten einer CI/CD-Pipeline für Azure Data Lake Analytics](data-lake-analytics-cicd-overview.md)
- [Entwickeln einer U-SQL-Datenbank mithilfe eines U-SQL-Datenbankprojekts](data-lake-analytics-data-lake-tools-develop-usql-database.md)
- [Testen des Azure Data Lake Analytics-Codes](data-lake-analytics-cicd-test.md)
