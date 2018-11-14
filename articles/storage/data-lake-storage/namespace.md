---
title: Hierarchischer Namespace für Azure Data Lake Storage Gen2 (Vorschauversion)
description: Beschreibt das Konzept des hierarchischen Namespace für Azure Data Lake Storage Gen2 (Vorschauversion).
services: storage
author: jamesbak
ms.service: storage
ms.topic: article
ms.date: 06/27/2018
ms.author: jamesbak
ms.component: data-lake-storage-gen2
ms.openlocfilehash: b5d3a735bd490468e989ac29c9f082475cc7eab3
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2018
ms.locfileid: "51283365"
---
# <a name="azure-data-lake-storage-gen2-preview-hierarchical-namespace"></a>Hierarchischer Namespace für Azure Data Lake Storage Gen2 (Vorschauversion)

Ein Schlüsselmechanismus, der es Azure Data Lake Storage Gen2 (Vorschauversion) ermöglicht, Dateisystemleistung im Umfang und zu Preisen eines Objektspeichers bereitzustellen, ist das Hinzufügen eines **hierarchischen Namespace**. Dadurch kann die Sammlung von Objekten/Dateien innerhalb eines Kontos in einer Hierarchie von Verzeichnissen und geschachtelten Unterverzeichnissen auf gleiche Weise wie beim Dateisystem auf Ihrem Computer organisiert werden. Bei aktiviertem hierarchischem Namespace kann ein Speicherkonto die Skalierbarkeit und Kosteneffizienz eines Objektspeichers mit einer Dateisystemsemantik bereitstellen, die Analyse-Engines und -Frameworks vertraut ist.

## <a name="the-benefits-of-the-hierarchical-namespace"></a>Vorteile des hierarchischen Namespace

> [!NOTE]
> Während der Public Preview-Phase von Azure Data Lake Storage Gen2 kann die Verfügbarkeit einiger der unten aufgeführten Features variieren. Sobald neue Features und Regionen in der Preview-Phase freigegeben werden, erfolgt eine entsprechende Mitteilung über unsere dedizierte Yammer-Gruppe.  

Dateisysteme, die einen hierarchischen Namespace über Blob-Daten implementieren, bieten folgende Vorteile:

- **Atomarische Verzeichnisbearbeitung:** Objektspeicher nähern sich einer Verzeichnishierarchie, indem eine Konvention mit Einbettung von Schrägstrichen (/) in den Objektnamen zur Angabe von Pfadsegmenten übernommen wird. Diese Konvention funktioniert zwar beim Organisieren von Objekten, stellt aber keine Hilfe bei Aktionen wie dem Verschieben, Umbenennen oder Löschen von Verzeichnissen dar. Ohne wirkliche Verzeichnisse müssen Anwendungen möglicherweise Millionen einzelner Blobs zur Ausführung von Aufgaben auf Verzeichnisebene verarbeiten. Der hierarchische Namespace hingegen verarbeitet diese Aufgaben durch Aktualisieren eines einzelnen Eintrags (des übergeordneten Verzeichnisses).

    Diese wesentliche Verbesserung ist für viele Big Data-Analyseframeworks von besonderer Bedeutung. Tools wie Hive, Spark usw. schreiben häufig die Ausgabe an temporäre Speicherorte und benennen dann bei Abschluss des Auftrags den Speicherort um. Ohne hierarchischen Namespace kann dieses Umbenennen häufig länger dauern als der eigentliche Analyseprozess. Eine niedrigere Auftragslatenz bedeutet auch niedrigere Gesamtkosten für Analyseworkloads.

- **Vertrauter Schnittstellenstil:** Dateisysteme werden sowohl von Entwicklern als auch Benutzern gut verstanden. Es ist nicht erforderlich, beim Wechsel zur Cloud ein neues Speicherparadigma zu erlernen, da die von Data Lake Storage Gen2 angezeigte Dateisystemschnittstelle dem sowohl von kleinen als auch großen Computern verwendeten Paradigma entspricht.

Einer der Gründe, warum Objektspeicher bisher keine hierarchischen Namespaces unterstützt haben, ist, dass durch hierarchische Namespaces die Skalierbarkeit begrenzt war. Der hierarchische Namespace für Data Lake Storage Gen2 kann jedoch linear skaliert werden und beeinträchtigt weder Datenkapazität noch Leistung.

## <a name="when-to-enable-the-hierarchical-namespace"></a>Aktivierungsempfehlung für den hierarchischen Namespace

Das Aktivieren des hierarchischen Namespace wird für Speicherworkloads empfohlen, die für Dateisysteme entwickelt wurden, die Verzeichnisse bearbeiten. Dazu gehören alle Workloads, die primär der Analyseverarbeitung dienen. Datasets, die ein hohes Maß an Organisation erfordern, profitieren ebenfalls vom Aktivieren des hierarchischen Namespace.

Die Gründe für das Aktivieren des hierarchischen Namespace werden durch eine Gesamtkostenanalyse ermittelt. Im Allgemeinen werden durch Verbesserungen der Workloadlatenz aufgrund beschleunigter Speicherung Computeressourcen über einen kürzeren Zeitraum benötigt. Die Latenz für viele Workloads kann aufgrund der atomarischen Verzeichnisbearbeitung verbessert werden, die der hierarchische Namespace ermöglicht. Bei vielen Workloads stellt die Computeressource mehr als 85 % der Gesamtkosten dar, sodass auch nur eine geringfügige Verringerung der Workloadlatenz zu wesentlichen Einsparungen bei den Gesamtkosten führen kann. Selbst in Fällen, in denen durch das Aktivieren des hierarchischen Namespace die Speicherkosten steigen, werden dennoch die Gesamtkosten aufgrund der niedrigeren Computekosten reduziert.

## <a name="when-to-disable-the-hierarchical-namespace"></a>Deaktivierungsempfehlung für den hierarchischen Namespace

Für einige Workloads des Objektspeichers bietet das Aktivieren des hierarchischen Namespace möglicherweise keine Vorteile. Beispiele dafür sind Backups, Imagespeicher und andere Anwendungen, bei denen die Objektorganisation getrennt von den eigentlichen Objekten gespeichert wird (*z.B.* in einer separaten Datenbank).

> [!NOTE]
> Wenn Sie in der Vorschauversion den hierarchischen Namespace aktivieren, besteht keine Interoperabilität von Daten oder Vorgängen zwischen Blob- und Data Lake Storage Gen2-REST-APIs. Diese Funktionalität wird während der Preview-Phase hinzugefügt.

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen eines Speicherkontos](./quickstart-create-account.md)