---
title: Integrationslaufzeit in Azure Data Factory | Microsoft-Dokumentation
description: "Enthält Informationen zur Integrationslaufzeit in Azure Data Factory."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/15/2017
ms.author: shlo
ms.openlocfilehash: 82b1dc036b53fbc1f0159d77ce102dcd054ea722
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="integration-runtime-in-azure-data-factory"></a>Integrationslaufzeit in Azure Data Factory
Bei der Integrationslaufzeit (Integration Runtime, IR) handelt es sich um die Computeinfrastruktur, mit der Azure Data Factory die folgenden Datenintegrationsfunktionen für verschiedene Netzwerkumgebungen bereitstellt:

- **Datenverschiebung**: Dient zum Verschieben von Daten zwischen Datenspeichern im öffentlichen Netzwerk und Datenspeichern im privaten Netzwerk (lokales oder virtuelles privates Netzwerk). Es wird Unterstützung für integrierte Connectors, Formatkonvertierung, Spaltenzuordnung und eine leistungsstarke und skalierbare Datenübertragung bereitgestellt.
- **Aktivitätsverteilung**: Dient zum Verteilen und Überwachen von Transformationsaktivitäten, die in vielen verschiedenen Computediensten wie Azure HDInsight, Azure Machine Learning, Azure SQL-Datenbank, SQL Server und vielen weiteren ausgeführt werden.
- **SSIS-Paketausführung**: Dient zum nativen Ausführen von SSIS-Paketen (SQL Server Integration Services) in einer verwalteten Azure-Computeumgebung.


> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 der Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie Version 1 des Data Factory-Diensts verwenden, die allgemein verfügbar (GA) ist, helfen Ihnen die Informationen unter [Dokumentation zur Version 1 von Data Factory](v1/data-factory-introduction.md) weiter.

In Data Factory wird mit einer Aktivität eine durchzuführende Aktion definiert. Mit einem verknüpften Dienst wird ein Zieldatenspeicher oder ein Computedienst definiert. Eine Integrationslaufzeit stellt die Brücke zwischen der Aktivität und verknüpften Diensten dar.  Vom verknüpften Dienst wird darauf verwiesen, und sie stellt die Computeumgebung bereit, in der die Aktivität entweder ausgeführt wird oder aus der sie verteilt wird.  Auf diese Weise kann die Aktivität in der Region durchgeführt werden, die dem Zieldatenspeicher bzw. dem Computedienst am nächsten liegt, und es kann die höchste Leistung erzielt werden, während gleichzeitig die Anforderungen an die Sicherheit und Konformität erfüllt werden.

## <a name="integration-runtime-types"></a>Integrationslaufzeit-Typen
Data Factory verfügt über drei Integrationslaufzeit-Typen. Sie sollten den Typ wählen, der die Anforderungen an die gewünschten Datenintegrationsfunktionen und die Netzwerkumgebung am besten erfüllt.  Diese drei Typen lauten:

- Azure
- Selbstgehostet
- Azure-SSIS

In der folgenden Tabelle sind die Funktionen und die Netzwerkunterstützung für die einzelnen Integrationslaufzeit-Typen beschrieben:

IR-Typ | Öffentliches Netzwerk | Privates Netzwerk
------- | -------------- | ---------------
Azure | Datenverschiebung<br/>Aktivitätsverteilung | &nbsp;
Selbstgehostet | Datenverschiebung<br/>Aktivitätsverteilung | Datenverschiebung<br/>Aktivitätsverteilung
Azure-SSIS | SSIS-Paketausführung | SSIS-Paketausführung

Im folgenden Diagramm ist dargestellt, wie die unterschiedlichen Integrationslaufzeiten als Kombination verwendet werden können, um umfassende Datenintegrationsfunktionen und Netzwerkunterstützung zu erzielen:

![Verschiedene Integrationslaufzeit-Typen](media\concepts-integration-runtime\different-integration-runtimes.png)


## <a name="azure-integration-runtime"></a>Azure-Integrationslaufzeit
Die Azure-Integrationslaufzeit ermöglicht Folgendes:

- Ausführen von Kopieraktivitäten zwischen Clouddatenspeichern
- Im öffentlichen Netzwerk werden die folgenden Transformationsaktivitäten bereitgestellt: HDInsight Hive-Aktivität, HDInsight Pig-Aktivität, HDInsight MapReduce-Aktivität, HDInsight Spark-Aktivität, HDInsight-Streamingaktivität, Machine Learning-Batchausführungsaktivität, Machine Learning-Ressourcenaktualisierungsaktivitäten, Aktivität „Gespeicherte Prozedur“, U-SQL-Aktivität für Data Lake Analytics, benutzerdefinierte .NET-Aktivität, Webaktivität, Lookup-Aktivität und Aktivität „Metadaten abrufen“.

### <a name="network-environment"></a>Netzwerkumgebung
Azure-Integrationslaufzeit unterstützt die Verbindungsherstellung mit Datenspeichern und Computediensten im öffentlichen Netzwerk mit öffentlich zugänglichen Endpunkten. Verwenden Sie eine selbstgehostete Integrationslaufzeit für die virtuelle Azure-Netzwerkumgebung.

### <a name="compute-resource-and-scaling"></a>Computeressource und Skalierung
Die Azure-Integrationslaufzeit stellt in Azure eine vollständig verwaltete, serverlose Computeressource bereit.  Sie müssen sich keine Gedanken um die Infrastrukturbereitstellung, die Softwareinstallation, das Patchen oder die Kapazitätsskalierung machen.  Darüber hinaus zahlen Sie nur für die tatsächliche Nutzungsdauer.

Mit der Azure-Integrationslaufzeit wird die native Computeressource zum sicheren, zuverlässigen und leistungsstarken Verschieben von Daten zwischen Clouddatenspeichern bereitgestellt.  Sie können festlegen, wie viele Datenverschiebungseinheiten für die Kopieraktivität verwendet werden. Die Computegröße der Azure IR wird entsprechend elastisch zentral hochskaliert, ohne dass Sie die Größe der Azure-Integrationslaufzeit explizit anpassen müssen.

Die Aktivitätsverteilung ist ein einfacher Vorgang zum Leiten der Aktivität an den Zielcomputedienst, sodass es nicht erforderlich ist, die Computegröße für dieses Szenario zentral hochzuskalieren.

Informationen zur Erstellung und Konfiguration einer Azure IR finden Sie in den Anleitungen unter „Gewusst wie: Erstellen und Konfigurieren der Azure IR“. 

## <a name="self-hosted-integration-runtime"></a>Selbstgehostete Integrationslaufzeit
Eine selbstgehostete Integrationslaufzeit ermöglicht Folgendes:

- Ausführen einer Kopieraktivität zwischen einem Clouddatenspeicher und einem Datenspeicher im privaten Netzwerk.
- Bereitstellen der folgenden Transformationsaktivitäten für Computeressourcen im lokalen Netzwerk oder im virtuellen Azure-Netzwerk: HDInsight Hive-Aktivität (BYOC), HDInsight Pig-Aktivität (BYOC), HDInsight MapReduce-Aktivität (BYOC), HDInsight Spark-Aktivität (BYOC), HDInsight-Streamingaktivität (BYOC), Machine Learning-Batchausführungsaktivität, Machine Learning-Ressourcenaktualisierungsaktivitäten, Aktivität „Gespeicherte Prozedur“, U-SQL-Aktivität für Data Lake Analytics, benutzerdefinierte .NET-Aktivität, Lookup-Aktivität und Aktivität „Metadaten abrufen“.

> [!NOTE] 
> Verwenden Sie die selbstgehostete Integrationslaufzeit zum Unterstützen von Datenspeichern, für die „Bring-your-own Driver“ erforderlich ist, z.B. SAP Hana, MySQL usw.  Weitere Informationen finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

### <a name="network-environment"></a>Netzwerkumgebung
Wenn Sie die Datenintegration auf sichere Weise in einer privaten Netzwerkumgebung durchführen möchten, die nicht über eine direkte Sichtlinie aus der öffentlichen Cloudumgebung verfügt, können Sie eine selbstgehostete IR in der lokalen Umgebung hinter Ihrer Unternehmensfirewall oder in einem virtuellen privaten Netzwerk installieren.  Die selbstgehostete Integrationslaufzeit stellt nur ausgehende HTTP-basierte Verbindungen in das offene Internet her.

### <a name="compute-resource-and-scaling"></a>Computeressource und Skalierung
Die selbstgehostete Integrationslaufzeit muss auf einem lokalen Computer oder einem virtuellen Computer in einem privaten Netzwerk installiert werden. Derzeit wird nur das Ausführen der selbstgehosteten Integrationslaufzeit auf einem Windows-Betriebssystem unterstützt.  

Zur Erzielung von Hochverfügbarkeit und Skalierbarkeit können Sie die selbstgehostete Integrationslaufzeit horizontal hochskalieren, indem Sie die logische Instanz mehreren lokalen Computern im Aktiv-Aktiv-Modus zuordnen.  Ausführlichere Informationen finden Sie in den Anleitungen unter „Gewusst wie: Erstellen und Konfigurieren von selbstgehosteter Integrationslaufzeit“.

## <a name="azure-ssis-integration-runtime"></a>Azure-SSIS-Integrationslaufzeit
Für die Durchführung von Lift & Shift-Vorgängen für vorhandene SSIS-Workloads können Sie eine Azure-SSIS-Integrationslaufzeit erstellen, um SSIS-Pakete nativ auszuführen.

### <a name="network-environment"></a>Netzwerkumgebung
Die Azure-SSIS--Integrationslaufzeit kann entweder im öffentlichen oder im privaten Netzwerk bereitgestellt werden.  Der lokale Datenzugriff wird unterstützt, indem Azure-SSIS-Integrationslaufzeit mit einem virtuellen Netzwerk (VNET) verknüpft wird, für das eine Verbindung mit Ihrem lokalen Netzwerk besteht. Derzeit wird nur das klassische VNET unterstützt. 

### <a name="compute-resource-and-scaling"></a>Computeressource und Skalierung
Die Azure-SSIS-Integrationslaufzeit ist ein vollständig verwalteter Cluster mit Azure-VMs, die speziell für die Ausführung von SSIS-Paketen bestimmt sind. Sie können Ihren eigenen Server mit Azure SQL-Datenbank oder einer verwalteten Instanz (private Vorschau) bereitstellen, um den Katalog mit SSIS-Projekten/-Paketen (SSISDB) zu hosten, der daran angefügt werden soll. Sie können die Computeleistung zentral hochskalieren, indem Sie die Knotengröße angeben und horizontal hochskalieren. Geben Sie hierzu die Anzahl von Knoten im Cluster an. Sie können die Kosten für die Ausführung Ihrer Azure-SSIS-Integrationslaufzeit verwalten, indem Sie sie je nach Bedarf anhalten und starten.

Weitere Informationen finden Sie in den Anleitungen unter „Gewusst wie: Erstellen und Konfigurieren von Azure-SSIS-Integrationslaufzeit“.  Nach der Erstellung können Sie Ihre vorhandenen SSIS-Pakete mit nur wenigen oder auch ganz ohne Änderungen bereitstellen und verwalten, indem Sie vertraute Tools wie SQL Server Data Tools (SSDT) und SQL Server Management Studio (SSMS) verwenden – genauso wie bei der lokalen Nutzung von SSIS.

Weitere Informationen zur Azure-SSIS-Laufzeit finden Sie in den folgenden Artikeln: 

- [Tutorial: Bereitstellen von SSIS-Paketen in Azure](tutorial-deploy-ssis-packages-azure.md): Dieser Artikel enthält schrittweise Anweisungen zum Erstellen einer Azure-SSIS-Integrationslaufzeit und verwendet eine Azure SQL-Datenbank zum Hosten des SSIS-Katalogs. 
- [Gewusst wie: Erstellen einer Azure-SSIS-Integrationslaufzeit](create-azure-ssis-integration-runtime.md): In diesem Artikel wird das Tutorial vertieft und er enthält Anweisungen zur Verwendung der verwalteten Azure SQL-Instanz (private Vorschau) und zum Verknüpfen der Integrationslaufzeit mit einem VNet. 
- [Überwachen einer Azure-SSIS-Integrationslaufzeit](monitor-integration-runtime.md#azure-ssis-integration-runtime): In diesem Artikel wird das Abrufen von Informationen zu einer Azure-SSIS-Integrationslaufzeit veranschaulicht, und er enthält Beschreibungen der Status in den zurückgegebenen Informationen. 
- [Verwalten einer Azure-SSIS-Integrationslaufzeit](manage-azure-ssis-integration-runtime.md): In diesem Artikel wird beschrieben, wie Sie eine Azure-SSIS-Integrationslaufzeit beenden, starten oder entfernen. Es wird zudem gezeigt, wie Sie Ihre Azure-SSIS-Integrationslaufzeit horizontal hochskalieren, indem Sie der Integrationslaufzeit weitere Knoten hinzufügen. 
- [Verknüpfen einer Azure-SSIS-Integrationslaufzeit mit einem VNet](join-azure-ssis-integration-runtime-virtual-network.md): Dieser Artikel enthält grundlegende Informationen zum Verknüpfen einer Azure-SSIS-Integrationslaufzeit mit einem Azure Virtual Network (VNet). Darüber hinaus enthält er Schritte zur Verwendung des Azure-Portals zum Konfigurieren des VNet, damit die Azure-SSIS-Integrationslaufzeit dem VNet beitreten kann. 

## <a name="determining-which-ir-to-use"></a>Ermitteln der richtigen Integrationslaufzeit
Jede Transformationsaktivität verfügt über einen verknüpften Zielcomputedienst, der auf eine Integrationslaufzeit verweist. Über diese Instanz der Integrationslaufzeit wird die Transformationsaktivität bereitgestellt.

Für die Kopieraktivität sind hierbei verknüpfte Quellen- und Senkendienste zum Definieren der Datenflussrichtung erforderlich. Anhand der folgenden Logik wird ermittelt, welche Integrationslaufzeit-Instanz zum Durchführen des Kopiervorgangs verwendet wird: 

- **Kopieren zwischen zwei Clouddatenquellen**: Wenn Azure-Integrationslaufzeit sowohl für den verknüpften Quellen- als auch für den Senkendienst verwendet wird, wird die vom verknüpften Senkendienst genutzte Integrationslaufzeit zum Ausführen der Kopieraktivität genutzt.
- **Kopieren zwischen einer Clouddatenquelle und einer Datenquelle im privaten Netzwerk**: Wenn entweder der verknüpfte Quellen- oder Senkendienst auf eine selbstgehostete Integrationslaufzeit zeigt, wird die Kopieraktivität unter dieser selbstgehosteten Integrationslaufzeit ausgeführt.
- **Kopieren zwischen zwei Datenquellen im privaten Netzwerk**: Sowohl der verknüpfte Quellendienst als auch der Senkendienst müssen auf dieselbe Integrationslaufzeit-Instanz zeigen, und diese Integrationslaufzeit wird zum Ausführen der Kopieraktivität verwendet.

Im folgenden Diagramm sind zwei Beispiele für Kopieraktivitäten dargestellt:

- Für Kopieraktivität 1 ist die Quelle ein verknüpfter SQL Server-Dienst, der auf die selbsgehostete Integrationslaufzeit A verweist, und die dazugehörige Senke ist ein verknüpfter Azure Storage-Dienst, der auf die Azure-Integrationslaufzeit B verweist. Wenn die Kopieraktivität ausgeführt wird, erfolgt dies über die selbstgehostete Integrationslaufzeit A.
- Für Kopieraktivität 2 ist die Quelle ein verknüpfter Azure SQL-Datenbankdienst, der auf die Azure-Integrationslaufzeit C verweist, und die dazugehörige Senke ist ein verknüpfter Azure Storage-Dienst, der auf die Azure-Integrationslaufzeit B verweist. Die Kopieraktivität wird dann auf der Azure-Integrationslaufzeit B ausgeführt, da dies die Integrationslaufzeit ist, die vom verknüpften Senkendienst verwendet wird.

![Zu verwendende Integrationslaufzeit](media/concepts-integration-runtime/which-integration-runtime-to-use.png)

## <a name="integration-runtime-location"></a>Ort der Integrationslaufzeit
Am Data Factory-Standort werden die Metadaten der Data Factory gespeichert, und von diesem Standort wird auch die Auslösung der Pipeline initiiert. Derzeit werden die folgenden Data Factory-Standorte unterstützt: „USA, Osten“ und „USA, Osten 2“. Eine Data Factory kann jedoch auf Datenspeicher und Compute Services in anderen Azure-Regionen zugreifen, um Daten zwischen Datenspeichern zu verschieben oder Daten mithilfe von Computediensten zu verarbeiten. Dieses Verhalten wird dadurch realisiert, dass die Integrationslaufzeit global in mehreren Regionen verfügbar ist, um für Datenkonformität, Effizienz und geringere Kosten für ausgehenden Netzwerkdatenverkehr zu sorgen.

Mit dem Integrationslaufzeit-Standort wird der Standort der Back-End-Computeinstanz definiert. Somit ist dies auch der Standort, an dem die Datenverschiebung, Aktivitätsverteilung und SSIS-Paketausführung durchgeführt werden. Der Integrationslaufzeit-Standort kann sich vom Standort der Data Factory unterscheiden, zu der er gehört. Im folgenden Diagramm sind die Standorteinstellungen von Data Factory und die dazugehörigen Integrationslaufzeiten dargestellt:

![Ort der Integrationslaufzeit](media/concepts-integration-runtime/integration-runtime-location.png)

### <a name="azure-ir"></a>Azure-Integrationslaufzeit
Für Data Factory wird eine Azure-Integrationslaufzeit in der Region, die der Senke in demselben geografischen Bereich am nächsten liegt, zum Verschieben der Daten verwendet. Informationen zur Zuordnung erhalten Sie in der folgenden Tabelle:

Geografie des Senkendatenspeichers | Standort des Senkendatenspeichers | Für Azure-Integrationslaufzeit verwendeter Standort
-------------------------------| ----------------| ------------------
USA | USA (Ost) | USA, Osten
&nbsp; | USA, Osten 2 | USA, Osten 2
&nbsp; | USA, Mitte | USA (Mitte)
&nbsp; | USA Nord Mitte | USA Nord Mitte
&nbsp; | USA, Süden-Mitte | USA Süd Mitte
&nbsp; | USA, Westen-Mitte | USA, Westen-Mitte
&nbsp; | USA (West) | USA (West)
&nbsp; | USA, Westen 2 | USA (West)
Kanada | Kanada, Osten | Kanada, Mitte
&nbsp; | Kanada, Mitte | Kanada, Mitte
Brasilien | Brasilien Süd | Brasilien Süd
Europa | Nordeuropa | Europa, Norden
&nbsp; | Europa, Westen | Westeuropa
Vereinigtes Königreich | UK, Westen | UK, Süden
&nbsp; | UK, Süden | UK, Süden
Asien-Pazifik | Südostasien | Südostasien
&nbsp; | Ostasien | Südostasien
Australien | Australien (Osten) | Australien, Osten
&nbsp; | Australien, Südosten | Australien (Südost)
Japan | Japan Ost | Japan Ost
&nbsp; | Japan, Westen | Japan Ost
Korea | Korea, Mitte | Korea, Süden
&nbsp; | Korea, Süden | Korea, Süden
Indien | Indien (Mitte) | Indien (Mitte)
&nbsp; | Indien, Westen | Indien (Mitte)
&nbsp; | Indien, Süden | Indien (Mitte)

Sie können den Standort einer Azure-Integrationslaufzeit auch auf „Automatisch auflösen“ festlegen. Dies bedeutet, dass Data Factory versucht, basierend auf der Definition des verknüpften Diensts automatisch den besten Standort zu erkennen.

> [!NOTE] 
> Falls die Region des Zieldatenspeichers nicht in der obigen Liste enthalten oder nicht erkennbar ist, tritt für die Aktivität aus Konformitätsgründen ein Fehler auf, anstatt die Daten über eine Alternativregion zu leiten. Geben Sie in diesem Fall explizit den alternativen Standort an, der zum Durchführen des Kopiervorgangs verwendet werden soll.
 
In der folgenden Abbildung ist ein Beispiel für den geltenden Standort dargestellt, wenn der Azure-Integrationslaufzeit-Standort auf „Automatisch auflösen“ festgelegt ist. Wenn eine Kopieraktivität ausgeführt wird, wird der Standort des Datenziels erkannt. In diesem Beispiel ist dies „Japan, Westen“.  Basierend auf der Tabelle wird eine Azure-Integrationslaufzeit in „Japan, Osten“ verwendet, um den eigentlichen Datenkopiervorgang durchzuführen. Wenn dieselbe Integrationslaufzeit zum Herstellen einer Verbindung mit HDInsight für eine Spark-Aktivität verwendet wird, wird die Übermittlung der Spark-Anwendung über den Data Factory-Standort durchgeführt. In diesem Beispiel ist dies „USA, Osten“. Die eigentliche Ausführung der Spark-Anwendung erfolgt am Standort des HDInsight-Servers. 

![Geltender Speicherort](media/concepts-integration-runtime/effective-location.png)

### <a name="self-hosted-ir"></a>Selbstgehostete Integrationslaufzeit
Die selbstgehostete Integrationslaufzeit wird logisch unter der Data Factory registriert, und die Computekomponente, mit der die Funktionen unterstützt werden, wird von Ihnen bereitgestellt. Aus diesem Grund ist für die selbstgehostete Integrationslaufzeit keine explizite Standorteigenschaft vorhanden. 

Bei Verwendung zum Durchführen der Datenverschiebung extrahiert die selbstgehostete Integrationslaufzeit Daten aus der Quelle und schreibt sie an das Ziel.

### <a name="azure-ssis-ir"></a>Azure-SSIS-Integrationslaufzeit
Die Auswahl des richtigen Standorts für Ihre Azure-SSIS-Integrationslaufzeit ist entscheidend, um für Ihre ETL-Workflows (Extrahieren-Transformieren-Laden) eine hohe Leistung zu erzielen.  Anfänglich sind zwei Standorte als Vorschau verfügbar („USA, Osten“ und „Europa, Norden“).

- Der Standort Ihrer Azure-SSIS-Integrationslaufzeit muss nicht dem Standort Ihrer Data Factory entsprechen, aber es sollte derselbe Standort wie für Ihren eigenen Server mit Azure SQL-Datenbank bzw. einer verwalteten Instanz (private Vorschauversion) sein, auf dem SSISDB gehostet werden soll. Ihre Azure-SSIS-Integrationslaufzeit kann dann leicht auf SSISDB zugreifen, ohne dass es zwischen unterschiedlichen Standorten zu übermäßig viel Datenverkehr kommt.
- Wenn Sie nicht über einen vorhandenen Server mit Azure SQL-Datenbank bzw. einer verwalteten Instanz (private Vorschauversion) zum Hosten von SSISDB verfügen, aber über lokale Datenquellen bzw. -ziele, sollten Sie wie folgt vorgehen: Erstellen Sie einen neuen Server mit Azure SQL-Datenbank bzw. einer verwalteten Instanz (private Vorschauversion) am Standort eines VNET, das mit Ihrem lokalen Netzwerk verbunden ist.  Auf diese Weise können Sie Ihre Azure-SSIS-Integrationslaufzeit mit dem neuen Server mit Azure SQL-Datenbank bzw. einer verwalteten Instanz (private Vorschauversion) erstellen und den Beitritt zu diesem VNET durchführen – alles an einem Standort. Dies führt zu einer wirksamen Verringerung der Datenverschiebungen zwischen unterschiedlichen Standorten.
- Wenn der Standort Ihres vorhandenen Servers mit Azure SQL-Datenbank bzw. einer verwalteten Instanz (private Vorschauversion), auf dem SSISDB gehostet wird, nicht dem Standort eines VNET entspricht, das mit Ihrem lokalen Netzwerk verbunden ist, sollten Sie wie folgt vorgehen: Erstellen Sie zuerst Ihre Azure-SSIS-Integrationslaufzeit, indem Sie einen vorhandenen Server mit Azure SQL-Datenbank bzw. einer verwalteten Instanz (private Vorschauversion) verwenden und an demselben Standort den Beitritt für ein anderes VNET durchführen, und konfigurieren Sie dann eine VNET-zu-VNET-Verbindung zwischen unterschiedlichen Standorten.


## <a name="next-steps"></a>Nächste Schritte
Entsprechende Informationen finden Sie in den folgenden Artikeln:

- [Create self-hosted integration runtime](create-self-hosted-integration-runtime.md) (Erstellen einer selbstgehosteten Integrationslaufzeit)
- [Erstellen einer Azure-SSIS Integration Runtime in Azure Data Factory | Microsoft-Dokumentation](create-azure-ssis-integration-runtime.md). In diesem Artikel wird das Tutorial vertieft und er enthält Anweisungen zur Verwendung der verwalteten Azure SQL-Instanz (private Vorschau) und zum Verknüpfen der Integrationslaufzeit mit einem VNet. 
