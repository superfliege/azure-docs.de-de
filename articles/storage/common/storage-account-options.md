---
title: Optionen für Azure Storage-Konten | Microsoft-Dokumentation
description: Enthält eine Beschreibung der Optionen zur Verwendung von Azure Storage.
services: storage
author: jirwin
manager: jwillis
ms.service: storage
ms.workload: storage
ms.topic: get-started-article
ms.date: 01/17/2018
ms.author: jirwin
ms.openlocfilehash: 75d1580df5e36b2c88939fde9077c5a1948f6348
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
---
# <a name="azure-storage-account-options"></a>Optionen für Azure Storage-Konten

## <a name="overview"></a>Übersicht
Azure Storage enthält drei verschiedene Kontooptionen, für die jeweils unterschiedliche Preise und unterstützte Features gelten. Berücksichtigen Sie diese Unterschiede, bevor Sie ein Speicherkonto erstellen, um die Option zu ermitteln, die für Ihre Anwendungen am besten geeignet ist. Die drei verschiedenen Speicherkontooptionen lauten:

* Konten vom Typ **General Purpose v2 (GPv2)** 
* Konten vom Typ **General Purpose v1 (GPv1)**
* Konten vom Typ **Blobspeicher**

Die einzelnen Kontotypen werden im folgenden Abschnitt ausführlicher beschrieben:

## <a name="storage-account-options"></a>Speicherkontooptionen

### <a name="general-purpose-v2"></a>General Purpose v2

Konten vom Typ „General Purpose v2 (GPv2)“ unterstützen alle aktuellen Features für Blobs, Dateien, Warteschlangen und Tabellen. GPv2-Konten unterstützen alle APIs und Features, die auch für GPv1- und Blob-Speicherkonten unterstützt werden. Außerdem werden die gleichen Features in Bezug auf Dauerhaftigkeit, Verfügbarkeit, Skalierbarkeit und Leistung dieser Kontotypen unterstützt. Die Preise für GPv2-Konten wurden so konzipiert, dass die niedrigsten Preise pro GB sowie konkurrenzfähige Transaktionspreise erzielt werden.

Sie können für Ihr GPv1-Konto ein Upgrade auf ein GPv2-Konto per PowerShell oder Azure CLI durchführen. 

Für Blockblobs in einem GPv2-Speicherkonto können Sie auf Grundlage von Zugriffsmustern zwischen den Speicherebenen „Hot“ und „Cool“ auf Kontoebene bzw. zwischen den Ebenen „Hot“, „Cool“ und „Archiv“ auf Blobebene wählen. Speichern Sie Daten, auf die häufig, weniger häufig und selten zugegriffen wird, auf den Speicherebenen „Hot“, „Cool“ bzw. „Archiv“, um die Kosten zu optimieren. 

GPv2-Speicherkonten machen das Attribut **Access Tier** (Zugriffsebene) auf Kontoebene verfügbar, über das die Standard-Speicherkontoebene als **Hot** oder **Cool** angegeben wird. Die Standard-Speicherkontoebene wird auf alle Blobs angewendet, für die auf Blobebene keine explizite Ebene festgelegt ist. Bei einer Änderung des Nutzungsmusters der Daten können Sie jederzeit zwischen den beiden Speicherebenen wechseln. Die **Ebene „Archiv“** steht ausschließlich auf der Blobebene zur Verfügung.

> [!NOTE]
> Die Änderung der Speicherebene kann mit zusätzlichen Kosten verbunden sein. Weitere Informationen finden Sie im Abschnitt [Preise und Abrechnung](#pricing-and-billing).
>
> Microsoft empfiehlt für die meisten Szenarien die Verwendung von Speicherkonten vom Typ „General Purpose v2“ über Blob-Speicherkonten.

### <a name="upgrade-a-storage-account-to-gpv2"></a>Aktualisieren eines Speicherkontos auf GPv2

Benutzer können ein GPv1-Konto jederzeit per PowerShell oder Azure CLI auf ein GPv2-Konto aktualisieren. Diese Änderung kann nicht rückgängig gemacht werden, und es sind keine anderen Änderungen zulässig.

#### <a name="upgrade-with-powershell"></a>Aktualisieren mit PowerShell

Wenn Sie ein GPv1-Konto per PowerShell auf ein GPv2-Konto aktualisieren möchten, sollten Sie zuerst PowerShell aktualisieren, damit die aktuelle Version des **AzureRm.Storage**-Moduls verwendet wird. Informationen zur Installation von PowerShell finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). Rufen Sie anschließend den folgenden Befehl auf, um das Konto zu aktualisieren, und ersetzen Sie den Namen Ihrer Ressourcengruppe und des Speicherkontos:

```powershell
Set-AzureRmStorageAccount -ResourceGroupName <resource-group> -AccountName <storage-account> -UpgradeToStorageV2
```

#### <a name="upgrade-with-azure-cli"></a>Aktualisieren per Azure CLI

Installieren Sie zuerst die aktuelle Version der Azure CLI, um für ein GPv1-Konto per Azure CLI ein Upgrade auf ein GPv2-Konto durchzuführen. Informationen zum Installieren der CLI finden Sie unter [Installieren von Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Rufen Sie anschließend den folgenden Befehl auf, um das Konto zu aktualisieren, und ersetzen Sie den Namen Ihrer Ressourcengruppe und des Speicherkontos:

```cli
az storage account update -g <resource-group> -n <storage-account> --set kind=StorageV2
```` 

### <a name="general-purpose-v1"></a>General Purpose v1

Konten vom Typ „General Purpose v1 (GPv1)“ ermöglichen den Zugriff auf alle Azure Storage-Dienste, bieten aber ggf. nicht die aktuellen Features oder die niedrigsten GB-Preise. Beispielsweise werden die Speicherebenen „Cool“ und „Archiv“ in GPv1 nicht unterstützt. Die Preise für GPv1-Transaktionen sind niedriger, sodass Workloads mit hohen Datenänderungs- oder Leseraten ggf. von der Nutzung dieses Kontotyps profitieren.

Speicherkonten vom Typ „General Purpose v1 (GPv1)“ sind der älteste Typ von Speicherkonto, und nur dieser Typ kann mit dem klassischen Bereitstellungsmodell verwendet werden. 

### <a name="blob-storage-accounts"></a>Blob-Speicherkonten

Blob-Speicherkonten unterstützen alle Blockblobfeatures, die auch für GPv2 unterstützt werden, aber sie sind ausschließlich auf die Unterstützung von Blockblobs beschränkt. Die Preise sind weitestgehend mit den Preisen für allgemeine Konten vom Typ „General Purpose v2“ identisch. Kunden sollten die Preisunterschiede zwischen Blob-Speicherkonten und GPv2-Konten und die Durchführung eines Upgrades auf GPv2 prüfen. Dieses Upgrade kann nicht rückgängig gemacht werden.

Ein Upgrade auf GPv2 ist in Kürze auch für Blob-Speicherkonten möglich.

> [!NOTE]
> BLOB-Speicherkonten unterstützen nur Block- und Anfügeblobs, keine Seitenblobs.

## <a name="recommendations"></a>Empfehlungen

Weitere Informationen zu Speicherkonten finden Sie unter [Informationen zu Azure-Speicherkonten](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

Bei Anwendungen, die nur Block- oder Anfügeblobspeicher benötigen, empfiehlt sich die Verwendung von GPv2-Speicherkonten, um vom differenzierten Preismodell des mehrstufigen Speichers profitieren zu können. In bestimmten Szenarien ist aber die Nutzung von GPv1 ratsam, z.B.:

* Sie müssen weiterhin das klassische Bereitstellungsmodell verwenden. Blob-Speicherkonten sind nur im Rahmen des Azure Resource Manager-Bereitstellungsmodells verfügbar.

* Sie verwenden große Mengen von Transaktionen oder Bandbreite für die Georeplikation, wofür in GPv2- und Blob-Speicherkonten jeweils höhere Kosten als in GPv1 anfallen, und verfügen nicht über genügend Speicher, für den sich die geringeren GB-Speicherkosten lohnen.

* Sie verwenden eine ältere Version der [REST-API für Speicherdienste](https://msdn.microsoft.com/library/azure/dd894041.aspx) (vor 2014-02-14) oder eine Clientbibliothek mit einer niedrigeren Version als 4.x und können kein Upgrade für Ihre Anwendung durchführen.

> [!NOTE]
> Blob-Speicherkonten werden derzeit in allen Azure-Regionen unterstützt.

## <a name="pricing-and-billing"></a>Preise und Abrechnung
Für alle Speicherkonten wird ein Blobspeicher-Preismodell verwendet, das auf der Ebene der einzelnen Blobs basiert. Bei Verwendung eines Speicherkontos sollten folgende Abrechnungsaspekte berücksichtigt werden:

* **Speicherkosten**: Die Kosten für die Datenspeicherung hängen nicht nur von der gespeicherten Datenmenge ab, sondern auch von der Speicherebene. Je „cooler“ die Ebene, desto geringer die Kosten pro GB.

* **Kosten für den Datenzugriff**: Je „cooler“ die Ebene, desto höher die Gebühren für den Datenzugriff. Bei den Speicherebenen „Cool“ und „Archiv“ fallen Zugriffsgebühren für Lesevorgänge pro Gigabyte an.

* **Transaktionskosten**: Für alle Ebenen fällt eine Gebühr pro Transaktion an, die sich erhöht, je „cooler“ die Ebene ist.

* **Datenübertragungskosten bei Georeplikation**: Diese Gebühr gilt nur für Konten mit konfigurierter Georeplikation, z.B. GRS und RA-GRS. Die Datenübertragung für die Georeplikation wird pro Gigabyte abgerechnet.

* **Kosten für ausgehende Datenübertragungen**: Ausgehende Datenübertragungen (Daten, die aus einer Azure-Region übertragen werden) werden genau wie bei allgemeinen Speicherkonten nach Bandbreitennutzung pro Gigabyte abgerechnet.

* **Änderung der Speicherebene**: Bei einem Wechsel der Kontospeicherebene von „Cool“ zu „Hot“ fällt eine Gebühr an, die den Kosten entspricht, die durch das Lesen aller im Speicherkonto vorhandenen Daten entstehen. Beim Ändern der Kontospeicherebene von „Hot“ in „Cool“ fällt aber eine Gebühr an, die dem Schreiben aller Daten auf die Ebene „Cool“ entspricht (nur GPv2-Konten).

> [!NOTE]
> Weitere Informationen zum Preismodell für Blob-Speicherkonten finden Sie auf der Seite [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/). Weitere Informationen zu den Kosten für ausgehende Datenübertragungen finden Sie auf der Seite [Datenübertragungen – Preisdetails](https://azure.microsoft.com/pricing/details/data-transfers/).

## <a name="quickstart-scenarios"></a>Schnellstartszenarien

In diesem Abschnitt werden unter Verwendung des Azure-Portals die folgenden Szenarien veranschaulicht:

* Erstellen eines GPv2-Speicherkontos
* Konvertieren eines GPv1- oder Blob-Speicherkontos in ein GPv2-Speicherkonto
* Festlegen der Konto- und Blobebene in einem GPv2-Speicherkonto

In den folgenden Beispielen kann die Zugriffsebene nicht auf „Archiv“ festgelegt werden, da diese Einstellung für das gesamte Speicherkonto gilt. „Archiv“ kann nur für ein bestimmtes Blob festgelegt werden.

### <a name="create-a-gpv2-storage-account-using-the-azure-portal"></a>Erstellen eines GPv2-Speicherkontos mit dem Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Klicken Sie im Menü „Hub“ auf **Ressource erstellen** > **Daten und Speicher** > **Speicherkonto**.

3. Geben Sie einen Namen für Ihr Speicherkonto ein.

    Dieser Name muss global eindeutig sein. Er wird als Teil der URL verwendet, über die auf die Objekte im Speicherkonto zugegriffen wird.  

4. Wählen Sie als Bereitstellungsmodell die Option **Resource Manager** aus.

    Mehrstufiger Speicher kann nur mit Resource Manager-Speicherkonten verwendet werden. Resource Manager ist das empfohlene Bereitstellungsmodell für neue Ressourcen. Weitere Informationen finden Sie unter [Übersicht über Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).  

5. Wählen Sie in der Dropdownliste **Kontoart** die Option **General Purpose v2**.

    Die Leistungsebene ist bei Auswahl von GPv2 auf „Standard“ festgelegt. Mehrstufiger Speicher ist für die Leistungsebene „Premium“ nicht verfügbar.

6. Wählen Sie die Replikationsoption für das Speicherkonto aus: **LRS**, **ZRS**, **GRS** oder **RA-GRS**. Die Standardeinstellung ist **RA-GRS**.

    LRS = lokal redundanter Speicher, ZRS = zonenredundanter Speicher, GRS = georedundanter Speicher (zwei Regionen), RA-GRS = georedundanter Speicher mit Lesezugriff (zwei Regionen mit Lesezugriff für die zweite Region).

    Weitere Details zu den Replikationsoptionen für Azure Storage finden Sie unter [Azure Storage-Replikation](../common/storage-redundancy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

7. Wählen Sie die richtige Speicherebene für Ihre Zwecke aus: Legen Sie **Zugriffstarif** entweder auf **Kalte Daten** oder **Heiße Daten** fest. Der Standardwert ist **Heiße Daten**.

8. Wählen Sie das Abonnement aus, in dem Sie das neue Speicherkonto erstellen möchten.

9. Geben Sie eine neue Ressourcengruppe an, oder wählen Sie eine vorhandene Ressourcengruppe aus. Weitere Informationen zu Ressourcengruppen finden Sie unter [Übersicht über den Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).

10. Wählen Sie die Region für Ihr Speicherkonto aus.

11. Klicken Sie auf **Erstellen** , um das Speicherkonto zu erstellen.

### <a name="convert-a-gpv1-account-to-a-gpv2-storage-account-using-the-azure-portal"></a>Konvertieren eines GPv1-Kontos in ein GPv2-Speicherkonto über das Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Navigieren Sie zu Ihrem Speicherkonto, klicken Sie auf **Alle Ressourcen**, und wählen Sie dann Ihr Speicherkonto aus.

3. Klicken Sie im Abschnitt „Einstellungen“ auf **Konfiguration**.

4. Klicken Sie unter **Kontoart** auf **Upgrade**.

5. Geben Sie unter **Confirm upgrade** (Upgrade bestätigen) den Namen Ihres Kontos ein. 

5. Klicken Sie unten auf dem Blatt auf „Upgrade“ (Upgrade durchführen).

### <a name="change-the-storage-tier-of-a-gpv2-storage-account-using-the-azure-portal"></a>Ändern der Speicherebene für ein GPv2-Speicherkonto mithilfe des Azure-Portals

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Navigieren Sie zu Ihrem Speicherkonto, klicken Sie auf **Alle Ressourcen**, und wählen Sie dann Ihr Speicherkonto aus.

3. Klicken Sie auf dem Blatt „Einstellungen“ auf **Konfiguration**, um die Kontokonfiguration anzuzeigen bzw. zu ändern.

4. Wählen Sie die richtige Speicherebene für Ihre Zwecke aus: Legen Sie **Zugriffstarif** entweder auf **Kalte Daten** oder **Heiße Daten** fest.

5. Klicken Sie oben im Blatt auf „Speichern“.

### <a name="change-the-storage-tier-of-a-blob-using-the-azure-portal"></a>Ändern der Speicherebene für ein Blob mithilfe des Azure-Portals

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Klicken Sie auf **Alle Ressourcen**, und wählen Sie Ihr Speicherkonto, Ihren Container und dann Ihr Blob aus, um zu Ihrem Blob in Ihrem Speicherkonto zu navigieren.

3. Klicken Sie auf dem Blatt „Blob-Eigenschaften“ auf das Dropdownmenü **Zugriffsebene**, um die Speicherebene **Hot**, **Cool** oder **Archiv** auszuwählen.

5. Klicken Sie oben im Blatt auf „Speichern“.

> [!NOTE]
> Die Änderung der Speicherebene kann mit zusätzlichen Kosten verbunden sein. Weitere Informationen finden Sie im Abschnitt [Preise und Abrechnung](#pricing-and-billing).


## <a name="evaluating-and-migrating-to-gpv2-storage-accounts"></a>Evaluieren von und Migrieren zu GPv2-Speicherkonten
Dieser Abschnitt dient dazu, Benutzern einen reibungslosen Übergang zur Nutzung von GPv2-Speicherkonten zu ermöglichen (anstelle von GPv1). Es gibt zwei Benutzerszenarien:

* Sie verfügen über ein vorhandenes GPv1-Speicherkonto und möchten eine Umstellung auf ein GPv2-Speicherkonto mit der richtigen Speicherebene evaluieren.
* Sie haben sich für die Nutzung eines GPv2-Speicherkontos entschieden oder besitzen bereits ein Konto dieser Art und möchten evaluieren, ob Sie die Speicherebene „Hot“ oder „Cool“ verwenden sollen.

In beiden Fällen sollten Sie zuerst die Kosten für die Speicherung und den Zugriff auf Ihre Daten in einem GPv2-Speicherkonto schätzen und diesen Betrag mit Ihren derzeitigen Kosten vergleichen.

## <a name="evaluating-gpv2-storage-account-tiers"></a>Evaluieren von GPv2-Speicherkontoebenen

Zur Ermittlung der ungefähren Kosten für die Speicherung und den Zugriff auf die Daten, die in einem GPv2-Speicherkonto gespeichert sind, müssen Sie Ihr vorhandenes Nutzungsmuster evaluieren bzw. Ihr voraussichtliches Nutzungsmuster einschätzen. Dazu benötigen Sie im Allgemeinen folgende Informationen:

* Ihr Speicherverbrauch: Welche Datenmengen werden gespeichert, und wie verändert sich dies von Monat zu Monat?

* Ihre Speicherzugriffsmuster: Welche Datenmengen werden im Rahmen des Kontos gelesen und geschrieben (einschließlich neuer Daten)? Wie viele Transaktionen werden für den Datenzugriff verwendet, und welche Arten von Transaktionen fallen an?

## <a name="monitoring-existing-storage-accounts"></a>Überwachen von vorhandenen Speicherkonten

Um Ihre vorhandenen Speicherkonten zu überwachen und diese Daten zu sammeln, können Sie Azure Storage Analytics nutzen. Damit wird eine Protokollierung durchgeführt, und es werden Metrikdaten für ein Speicherkonto bereitgestellt. Von Storage Analytics können Metriken gespeichert werden, zu denen aggregierte Transaktionsstatistiken und Kapazitätsdaten für die an einen Speicherdienst für GPv1-, GPv2- und Blob-Speicherkonten gesendeten Anforderungen zählen. Diese Daten werden in bekannten Tabellen in demselben Speicherkonto gespeichert.

Weitere Informationen finden Sie unter [Informationen zu Metriken der Speicheranalyse](https://msdn.microsoft.com/library/azure/hh343258.aspx) und [Schema der Tabellen für Speicheranalysemetriken](https://msdn.microsoft.com/library/azure/hh343264.aspx).

> [!NOTE]
> Blob-Speicherkonten machen den Tabellenspeicherdienst-Endpunkt nur zum Speichern und Zugreifen auf die Metrikdaten für das Konto verfügbar. 

Zur Überwachung des Speicherbedarfs für den Blobspeicher müssen Sie die Kapazitätsmetriken aktivieren.
Wenn dies aktiviert ist, werden täglich Kapazitätsdaten für den Blob-Dienst eines Speicherkontos aufgezeichnet. Sie werden als Tabelleneintrag aufgezeichnet, der in die Tabelle *$MetricsCapacityBlob* desselben Speicherkontos geschrieben wird.

Zum Überwachen des Datenzugriffsmusters für Blobspeicher müssen Sie die Stundentransaktionsmetriken über die API aktivieren. Wenn die Stundentransaktionsmetriken aktiviert sind, werden jede Stunde die Transaktionen pro API aggregiert und als Tabelleneintrag aufgezeichnet, der in die Tabelle *$MetricsHourPrimaryTransactionsBlob* desselben Speicherkontos geschrieben wird. In der Tabelle *$MetricsHourSecondaryTransactionsBlob* werden bei Verwendung von RA-GRS-Speicherkonten die Transaktionen für den sekundären Endpunkt aufgezeichnet.

> [!NOTE]
> Falls Sie über ein allgemeines Speicherkonto verfügen, in dem Sie neben Block- und Anfügeblob-Daten Seitenblobs und Datenträger virtueller Computer oder Warteschlangen, Dateien oder Tabellen gespeichert haben, ist dieser Schätzungsprozess nicht geeignet. Für die Kapazitätsdaten wird nicht zwischen Blockblobs und anderen Typen unterschieden, sodass keine Kapazitätsdaten für andere Datentypen bereitgestellt werden. Bei Verwendung dieser Typen besteht eine alternative Methode darin, sich die Mengen auf der letzten Rechnung anzusehen.

Um eine gute Annäherung des Datenverbrauchs und der Zugriffsmuster zu erhalten, empfehlen wir Ihnen die Auswahl eines Aufbewahrungszeitraums für die Metriken, der für die reguläre Nutzung repräsentativ ist und den Sie dann extrapolieren können. Eine Option besteht darin, die Metrikdaten für sieben Tage aufzubewahren und die Daten jede Woche für die Analyse am Monatsende zu erfassen. Eine andere Möglichkeit ist die Aufbewahrung der Metrikdaten der letzten 30 Tage, um sie dann am Ende der 30 Tage zu erfassen und zu analysieren.

Weitere Informationen zum Aktivieren, Erfassen und Anzeigen von Metrikdaten finden Sie unter [Aktivieren der Azure-Speichermetriken und Anzeigen von Metrikdaten](../common/storage-enable-and-view-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

> [!NOTE]
> Speicherung, Verwendung und Download von Analysedaten werden genau wie bei regulären Benutzerdaten in Rechnung gestellt.

### <a name="utilizing-usage-metrics-to-estimate-costs"></a>Verwenden von Nutzungsmetriken zum Schätzen von Kosten

#### <a name="storage-costs"></a>Speicherkosten

Der letzte Eintrag in der Kapazitätsmetrikentabelle *$MetricsCapacityBlob* mit dem Zeilenschlüssel *'data'* enthält die von den Benutzerdaten verbrauchte Speicherkapazität. Der letzte Eintrag in der Kapazitätsmetrikentabelle *$MetricsCapacityBlob* mit dem Zeilenschlüssel *'analytics'* enthält die von den Analyseprotokollen verbrauchte Speicherkapazität.

Diese gesamte Kapazität, die von Benutzerdaten und Analyseprotokollen (falls aktiviert) verbraucht wird, kann dann verwendet werden, um die Kosten für das Speichern der Daten im Speicherkonto zu schätzen. Diese Vorgehensweise kann auch zum Schätzen von Speicherkosten in GPv1-Speicherkonten verwendet werden.

#### <a name="transaction-costs"></a>Transaktionskosten

Die Summe von *'TotalBillableRequests'*über alle Einträge für eine API in der Transaktionsmetrikentabelle hinweg gibt die Gesamtzahl der Transaktionen für die jeweilige API an. *Beispiel:* Die Gesamtanzahl von Transaktionen vom Typ *GetBlob* eines bestimmten Zeitraums kann anhand der Gesamtsumme von abrechenbaren Anforderungen für alle Einträge mit dem Zeilenschlüssel *user;GetBlob* berechnet werden.

Zur Ermittlung der ungefähren Transaktionskosten für Blob-Speicherkonten müssen die Transaktionen in drei Gruppen unterteilt werden, da jeweils unterschiedliche Preise gelten.

* Schreibtransaktionen wie *'PutBlob'*, *'PutBlock'*, *'PutBlockList'*, *'AppendBlock'*, *'ListBlobs'*, *'ListContainers'*, *'CreateContainer'*, *'SnapshotBlob'* und *'CopyBlob'*.
* Löschtransaktionen wie *'DeleteBlob'* und *'DeleteContainer'*.
* Alle anderen Transaktionen.

Um die Transaktionskosten für GPv1-Speicherkonten zu schätzen, müssen Sie alle Transaktionen unabhängig vom Vorgang bzw. von der API aggregieren.

### <a name="data-access-and-geo-replication-data-transfer-costs"></a>Datenübertragungskosten für Datenzugriff und Georeplikation

Die Speicheranalyse liefert zwar nicht die Menge der Daten, die aus einem Speicherkonto gelesen und in das Speicherkonto geschrieben wird, aber dieser Wert kann grob geschätzt werden, indem die Tabelle mit den Transaktionsmetriken verwendet wird. Die Summe von *'TotalIngress'* über alle Einträge für eine API in der Transaktionsmetrikentabelle hinweg gibt die Gesamtmenge der Eingangsdaten in Byte für die jeweilige API an. Analog dazu gibt die Summe von *'TotalEgress'* die Gesamtmenge der Ausgangsdaten in Byte an.

Zur Ermittlung der ungefähren Datenzugriffskosten für Blob-Speicherkonten müssen die Transaktionen in zwei Gruppen unterteilt werden.

* Die Menge der Daten, die aus dem Speicherkonto abgerufen werden, kann geschätzt werden, indem vor allem für die Vorgänge *'GetBlob'* und *'CopyBlob'* die Summe von *'TotalEgress'* geprüft wird.

* Die Menge der Daten, die in das Speicherkonto geschrieben werden, kann anhand der Summe von *'TotalIngress'* für die Vorgänge *'PutBlob'*, *'PutBlock'*, *'CopyBlob'* und *'AppendBlock'* geschätzt werden.

Bei Verwendung eines GRS- oder RA-GRS-Speicherkontos können die Datenübertragungskosten mit Georeplikation für Blob-Speicherkonten auch auf der Grundlage der Schätzung für die Menge an geschriebenen Daten berechnet werden.

> [!NOTE]
> Ein ausführlicheres Beispiel zur Berechnung der Kosten für die Verwendung der Speicherebene „Hot“ oder „Cool“ finden Sie unter *Was sind die Zugriffsebenen „Heiß“ und „Kalt“, und wie bestimme ich, welche Zugriffsebene ich wählen sollte?* im FAQ-Bereich mit dem Titel [Was sind die Zugriffsebenen „Heiß“ und „Kalt“, und wie bestimme ich, welche Zugriffsebene ich wählen sollte?](https://azure.microsoft.com/pricing/details/storage/)an.

## <a name="migrating-existing-data"></a>Migrieren vorhandener Daten

Ein GPv1-Konto kann problemlos auf GPv2 aktualisiert werden, ohne dass es zu Ausfallzeiten oder API-Änderungen kommt oder Daten migriert werden müssen. Aus diesem Grund wird empfohlen, dass Sie GPv1-Konten nicht zu Blob-Speicherkonten migrieren, sondern zu GPv2-Konten.

Falls Sie aber eine Migration zu einem Blob-Speicherkonto durchführen müssen, können Sie die unten angegebene Anleitung verwenden.

Ein Blob-Speicherkonto ist ein spezielles Konto, in dem nur Blockblobs und Anfügeblobs gespeichert werden. Bereits vorhandene allgemeine Speicherkonten, die Ihnen das Speichern von Tabellen, Warteschlangen, Dateien und Datenträgern sowie Blobs ermöglichen, können nicht in Blob-Speicherkonten konvertiert werden. Zum Verwenden der Speicherebenen müssen Sie neue Blob-Speicherkonten erstellen und Ihre vorhandenen Daten zu den neu erstellten Konten migrieren.

Mithilfe der folgenden Methoden können Sie vorhandene Daten von lokalen Speichergeräten, aus dem Cloudspeicher von Drittanbietern oder aus Ihren vorhandenen allgemeinen Azure-Speicherkonten zu Blob-Speicherkonten migrieren:

### <a name="azcopy"></a>AzCopy

AzCopy ist ein Windows-Befehlszeilenprogramm, mit dem sehr effizient Daten in und aus Azure Storage kopiert werden können. Mit AzCopy können Sie Daten aus Ihren vorhandenen allgemeinen Speicherkonten in das Blob-Speicherkonto kopieren oder Daten von Ihren lokalen Speichergeräten in das Blob-Speicherkonto hochladen.

Weitere Informationen finden Sie unter [Übertragen von Daten mit dem Befehlszeilenprogramm AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

### <a name="data-movement-library"></a>Data Movement-Bibliothek

Die Azure Storage Data Movement-Bibliothek für .NET basiert auf dem Kernframework für die Datenverschiebung, das AzCopy zugrunde liegt. Die Bibliothek ist ähnlich wie AzCopy auf leistungsfähige, zuverlässige und einfache Datenübertragungsvorgänge ausgelegt. Sie ermöglicht Ihnen die native Verwendung aller Vorteile der AzCopy-Features in Ihrer Anwendung, ohne externe Instanzen von AzCopy ausführen und überwachen zu müssen.

Weitere Informationen finden Sie unter [Azure Storage Data Movement-Bibliothek für .NET](https://github.com/Azure/azure-storage-net-data-movement).

### <a name="rest-api-or-client-library"></a>REST-API oder Clientbibliothek

Sie können eine benutzerdefinierte Anwendung erstellen, um Ihre Daten mithilfe einer der Azure-Clientbibliotheken oder mithilfe der REST-API der Azure Storage-Dienste in ein Blob-Speicherkonto zu migrieren. Azure Storage bietet umfassende Clientbibliotheken für mehrere Programmiersprachen und Plattformen wie .NET, Java, C++, Node.JS, PHP, Ruby und Python. Die Clientbibliotheken bieten erweiterte Funktionen, beispielsweise Wiederholungslogik, Protokollierung und parallele Uploads. Die Entwicklung kann direkt mit der REST-API erfolgen. Diese API lässt sich mithilfe jeder Sprache aufrufen, die HTTP/HTTPS-Anforderungen verarbeitet.

Weitere Informationen finden Sie unter [Erste Schritte mit Azure Blob Storage mit .NET](../blobs/storage-dotnet-how-to-use-blobs.md).

> [!IMPORTANT]
> Mit clientseitiger Verschlüsselung verschlüsselte Blobs speichern Metadaten im Zusammenhang mit der Verschlüsselung mit dem Blob. Wenn Sie einen mit clientseitiger Verschlüsselung verschlüsselten Blob kopieren, müssen Sie sicherstellen, dass die Blobmetadaten und insbesondere die verschlüsselungsbezogenen Metadaten beim Kopiervorgang erhalten bleiben. Wenn Sie einen Blob ohne die Verschlüsselungsmetadaten kopieren, ist der Blobinhalt nicht mehr abrufbar. Weitere Informationen zu den auf die Verschlüsselung bezogenen Metadaten finden Sie unter [Clientseitige Azure Storage-Verschlüsselung](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="faq"></a>Häufig gestellte Fragen

**Bleiben bereits vorhandene Speicherkonten weiterhin verfügbar?**

Ja. Bereits vorhandene Speicherkonten (GPv1) sind weiterhin zu gleichen Preisen und mit unveränderten Funktionen verfügbar. Für GPv1-Konten kann keine Speicherebene gewählt werden, und diese Möglichkeit ist auch in Zukunft nicht geplant.

**Warum und wann sollte ich GPv2-Speicherkonten verwenden?**

GPv2-Speicherkonten sind auf geringstmögliche GB-Speicherkosten ausgelegt und verfügen gleichzeitig über wettbewerbsfähige Branchenpreise für Transaktionen und Datenzugriff. In Zukunft wird für die Speicherung von Blobs die Verwendung von GPv2-Speicherkonten empfohlen, da Funktionen, z.B. Änderungsbenachrichtigungen, basierend auf diesem Kontotyp eingeführt werden. Es liegt aber bei Ihnen, wann Sie das Upgrade (abhängig von Ihren geschäftlichen Anforderungen) durchführen. Beispielsweise können Sie vor dem Durchführen des Upgrades Ihre Transaktionsmuster optimieren.

Eine Herabstufung von GPv2 wird nicht unterstützt. Machen Sie sich daher vor dem GPv2-Upgrade Ihrer Konten mit den preislichen Auswirkungen vertraut.

**Kann ich mein vorhandenes Speicherkonto auf ein GPv2-Speicherkonto aktualisieren?**

Ja. GPv1-Konten lassen sich über das Portal komfortabel auf GPv2 upgraden. Sie können dazu aber auch PowerShell oder die Befehlszeilenschnittstelle verwenden. Blob-Speicherkonten können mithilfe von PowerShell oder über die Befehlszeilenschnittstelle auf GPv2 aktualisiert werden. GPv2-Upgrades für Blob-Speicherkonten sind in Kürze aber auch über das Portal möglich.

Eine Herabstufung von GPv2 wird nicht unterstützt. Machen Sie sich daher vor dem GPv2-Upgrade Ihrer Konten mit den preislichen Auswirkungen vertraut.

**Kann ich Objekte innerhalb des gleichen Kontos auf beiden Speicherebenen speichern?**

Ja. Das Attribut **Access Tier** (Zugriffsebene) auf Kontoebene ist die Standardebene, die für alle Objekte des Kontos gilt, für die keine explizite Ebene festgelegt ist. Mit dem Blobebenentiering können Sie die Zugriffsebene auf dieser Objektebene unabhängig davon festlegen, welche Zugriffsebene für das Konto eingestellt ist. In einem Konto können Blobs in allen drei Speicherebenen („Hot“, „Cool“ oder „Archiv“) vorhanden sein.

**Kann ich die Speicherebene meines GPv2-Speicherkontos ändern?**

Ja. Sie können die Kontospeicherebene ändern, indem Sie für das Speicherkonto das Attribut **Access Tier** (Zugriffsebene) festlegen. Die Änderung der Kontospeicherebene gilt für alle gespeicherten Objekte des Kontos, für die keine explizite Ebene festgelegt ist. Für das Ändern der Speicherebene von „Hot“ in „Cool“ fallen Gebühren für Schreibvorgänge (pro 10.000) an (nur GPv2-Speicherkonten). Für das Ändern von „Cool“ in „Hot“ fallen sowohl Gebühren für Lesevorgänge (pro 10.000) als auch für den Datenabruf (pro GB) zum Lesen aller Daten des Kontos an.

**Wie oft kann ich die Speicherebene für mein Blob-Speicherkonto ändern?**

Die Anzahl von Speicherebenenänderungen wird von uns zwar nicht begrenzt, aber Sie sollten bedenken, dass ein Wechsel von „Cool“ zu „Hot“ mit erheblichen Kosten verbunden ist. Daher ist von häufigen Speicherebenenwechseln abzuraten.

**Verhalten sich die Blobs der Speicherebene „Cool“ anders als die Blobs der Speicherebene „Hot“?**

Blobs der Speicherebene „Hot“ von GPv2- und Blob-Speicherkonten weisen die gleiche Latenz wie Blobs in GPv1-Speicherkonten auf. Blobs der Speicherebene „Cool“ verfügen über eine ähnliche Latenz (in Millisekunden) wie Blobs der Ebene „Hot“. Für Blobs auf Archivspeicherebene gilt eine Latenz von mehreren Stunden.

Für Blobs der Speicherebene „Cool“ gilt in Bezug auf die Verfügbarkeit ein etwas geringerer Servicelevel (SLA) als für Blobs, die auf der Speicherebene „Hot“ gespeichert sind. Weitere Informationen finden Sie unter [SLA für Speicher](https://azure.microsoft.com/support/legal/sla/storage).

**Kann ich Seitenblobs und Datenträger virtueller Computer unter Blob-Speicherkonten speichern?**

Nein. BLOB-Speicherkonten unterstützen nur Block- und Anfügeblobs, keine Seitenblobs. Datenträger virtueller Azure-Computer werden durch Seitenblobs unterstützt. Daher können Blob-Speicherkonten nicht zum Speichern der Datenträger von virtuellen Computern verwendet werden. Es ist aber möglich, Sicherungen der Datenträger virtueller Computer als Blockblobs unter einem Blob-Speicherkonto zu speichern. Dies ist einer der Gründe, die Sie in Bezug auf die Verwendung von GPv2 anstelle von Blob-Speicherkonten berücksichtigen sollten.

**Muss ich zur Verwendung von GPv2-Speicherkonten meine vorhandenen Anwendungen ändern?**

GPv2-Speicherkonten sind zu 100% API-konsistent mit GPv1- und Blob-Speicherkonten. Solange Sie für Ihre Anwendung Blockblobs oder Anfügeblobs verwenden und Version 2014-02-14 (oder höher) der [REST-API für Speicherdienste](https://msdn.microsoft.com/library/azure/dd894041.aspx) nutzen, sollte Ihre Anwendung funktionieren. Bei Verwendung einer älteren Protokollversion müssen Sie Ihre Anwendung aktualisieren, sodass sie die neue Version verwendet und beide Arten von Speicherkonten problemlos genutzt werden können. Unabhängig von der Art des verwendeten Speicherkontos empfehlen wir grundsätzlich die Verwendung der neuesten Version.

Bei GPv2 sind die Preise für Transaktionen und Bandbreite generell höher als bei GPv1. Aus diesem Grund empfiehlt es sich unter Umständen, vor dem Upgrade eine Optimierung Ihrer Transaktionsmuster durchzuführen, um einen Anstieg der Gesamtkosten zu vermeiden.

**Ändert sich etwas für die Benutzer?**

GPv2-Speicherkonten weisen eine hohe Ähnlichkeit mit GPv1-Speicherkonten auf und unterstützen alle wesentlichen Funktionen von Azure Storage, z.B. hohe Stabilität und Verfügbarkeit, Skalierbarkeit, Leistung und Sicherheit. Mit Ausnahme der spezifischen Features und Einschränkungen für Blob-Speicherkonten und der weiter oben erläuterten Speicherebenen ändert sich beim Durchführen eines Upgrades auf GPv2 oder Blobspeicher nichts.

## <a name="next-steps"></a>Nächste Schritte

### <a name="evaluate-blob-storage-accounts"></a>Auswerten von Blob-Speicherkonten

[Überprüfen der Verfügbarkeit von Blob-Speicherkonten nach Region unter Azure-Regionen](https://azure.microsoft.com/regions/#services)

[Auswerten der Nutzung vorhandener Speicherkonten durch Aktivierung von Azure Storage-Metriken](../common/storage-enable-and-view-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[Überprüfen der Preise für Blobspeicher nach Region unter Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/)

[Überprüfen der Preise für Datenübertragungen unter Datenübertragungen – Preisdetails](https://azure.microsoft.com/pricing/details/data-transfers/)

### <a name="start-using-gpv2-storage-accounts"></a>Beginnen mit der Nutzung von GPv2-Speicherkonten

[Erste Schritte mit Azure Blob Storage mit .NET](../blobs/storage-dotnet-how-to-use-blobs.md)

[Verschieben von Daten in und aus Azure Storage](../common/storage-moving-data.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[Übertragen von Daten mit dem Befehlszeilenprogramm AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[Durchsuchen und Erkunden von Speicherkonten](http://storageexplorer.com/)
