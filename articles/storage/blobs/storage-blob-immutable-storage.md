---
title: Funktion „Unveränderlicher Speicher“ von Azure-Blobspeicher (Vorschauversion) | Microsoft-Dokumentation
description: Azure Storage verfügt jetzt über WORM-Unterstützung für Blobobjektspeicher, der Ihnen das Speichern in einem nicht löschbaren und nicht änderbaren Zustand für einen vom Benutzer angegebenen Zeitraum ermöglicht. Mit diesem Feature können Organisationen in vielen Branchen mit hoher Regulierung – vor allem Broker-Dealer-Organisationen – Daten so speichern, dass die Konformität mit SEC 17a-4(f) und anderen Bestimmungen sichergestellt ist.
services: storage
author: sangsinh
manager: twooley
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 05/29/2018
ms.author: sangsinh
ms.openlocfilehash: 4a83ebbcf045ac2b74957effceadfe80609e960c
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36237427"
---
# <a name="immutable-storage-feature-of-azure-blob-storage-preview"></a>Funktion „Unveränderlicher Speicher“ von Azure-Blobspeicher (Vorschauversion)

Die Funktion „Unveränderlicher Speicher“ für Azure-Blobs ermöglicht es Benutzern, unternehmenskritische Daten in Azure-Blobspeicher im WORM-Zustand (Write Once Read Many) zu speichern. In diesem Zustand ist der Speicher für einen vom Benutzer angegebenen Zeitraum nicht löschbar und nicht änderbar. Blobs können erstellt und gelesen werden, aber das Ändern oder Löschen ist für den Aufbewahrungszeitraum nicht möglich.

## <a name="overview"></a>Übersicht

Mit der Funktion „Unveränderlicher Speicher“ können Organisationen in vielen regulierten Branchen – vor allem Broker-Dealer-Organisationen – Daten so speichern, dass die Konformität mit SEC 17a-4(f) und anderen Bestimmungen sichergestellt ist.

Beispiele für typische Anwendungen:

- **Einhaltung gesetzlicher Bestimmungen**: Die Funktion „Unveränderlicher Speicher“ für Azure-Blobs ist so konzipiert, dass sie von Finanzinstituten und in anderen Branchen für die Erfüllung von Anforderungen in Bezug auf SEC 17a-4(f), CFTC 1.31©-(d), FINRA usw. genutzt werden kann.

- **Sichere Dokumentaufbewahrung**: Benutzer erzielen hiermit maximalen Schutz von Daten, da mit dem Blobspeicherdienst sichergestellt ist, dass Daten nicht von Benutzern geändert oder gelöscht werden können, auch wenn diese über Berechtigungen für die Kontoverwaltung verfügen.

- **Gesetzliche Aufbewahrungspflicht**: Mit der Funktion „Unveränderlicher Speicher“ für Azure-Blobs können Benutzer sensible Informationen, die für Beweissicherungsverfahren, strafrechtliche Untersuchungen usw. wichtig sind, für den gewünschten Zeitraum in einem vor Manipulationen geschützten Zustand speichern.

Die Funktion „Unveränderlicher Speicher“ ermöglicht Folgendes:

- **Unterstützung einer zeitbasierten Aufbewahrungsrichtlinie:** Benutzer legen Richtlinien für die Speicherung von Daten für einen bestimmten Zeitraum fest.

- **Unterstützung einer Richtlinie zur gesetzlichen Aufbewahrungspflicht:** Wenn der Aufbewahrungszeitraum nicht bekannt ist, können Benutzer Zeiträume für die gesetzliche Aufbewahrungspflicht festlegen, um Daten auf unveränderliche Weise zu speichern, bis die Aufbewahrungspflicht nicht mehr gilt.  Wenn ein Zeitraum für die gesetzliche Aufbewahrungspflicht festgelegt wird, können Blobs erstellt und gelesen, aber nicht geändert oder gelöscht werden. Jeder gesetzlichen Aufbewahrungspflicht ist ein benutzerdefiniertes alphanumerisches Tag zugeordnet, das als Bezeichnerzeichenfolge (z.B. eine Fall-ID) verwendet wird.

- **Unterstützung für alle Blobebenen**: WORM-Richtlinien sind unabhängig von der Azure-Blobspeicherebene und gelten für alle Ebenen: „Heiß“ (Hot), „Kalt“ (Cool) und „Archiv“. Auf diese Weise können Kunden die Daten auf der Ebene speichern, für die die Workloadkosten bestmöglich optimiert sind, während gleichzeitig die Unveränderlichkeit der Daten sichergestellt ist.

- **Konfiguration auf Containerebene**: Mit der Funktion „Unveränderlicher Speicher“ können Benutzer zeitbasierte Aufbewahrungsrichtlinien und Tags für gesetzliche Aufbewahrungspflichten auf Containerebene konfigurieren.  Benutzer können zeitbasierte Aufbewahrungsrichtlinien erstellen und sperren, Aufbewahrungszeiträume verlängern, Zeiträume für gesetzliche Aufbewahrungspflichten festlegen und aufheben usw., indem sie einfache Einstellungen auf Containerebene vornehmen.  Diese Richtlinien gelten für alle Blobs im Container (vorhandene und neue).

- **Unterstützung für Überwachungsprotokolle:** Jeder Container enthält ein Überwachungsprotokoll, in dem bis zu fünf zeitbasierte Aufbewahrungsbefehle für gesperrte zeitbasierte Aufbewahrungsrichtlinien mit maximal drei Protokollen zur Verlängerung von Aufbewahrungszeiträumen angezeigt werden.  Für die zeitbasierte Aufbewahrung enthält das Protokoll die Benutzer-ID, den Befehlstyp, Zeitstempel und den Aufbewahrungszeitraum. Für Zeiträume zur gesetzlichen Aufbewahrungspflicht enthält das Protokoll die Benutzer-ID, den Befehlstyp, Zeitstempel und die entsprechenden Tags. Dieses Protokoll wird für die Lebensdauer des Containers gemäß den SEC 17a-4(f)-Bestimmungsrichtlinien aufbewahrt. Ein umfassenderes Protokoll mit allen Aktivitäten der Steuerebene finden Sie unter [Azure-Aktivitätsprotokoll](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs). Der Benutzer ist für die dauerhafte Speicherung dieser Protokolle verantwortlich, die aus gesetzlichen oder anderen Gründen ggf. erforderlich ist.

 Die Funktion ist in allen öffentlichen Azure-Regionen aktiviert.

## <a name="how-it-works"></a>So funktioniert's

Die Funktion „Unveränderlicher Speicher“ für Azure-Blobs unterstützt zwei Arten von WORM-Richtlinien bzw. Richtlinien für die unveränderliche Speicherung: zeitbasierte Aufbewahrung und gesetzliche Aufbewahrungspflicht. Ausführliche Informationen zur Erstellung dieser Richtlinien für die unveränderliche Speicherung finden Sie im Abschnitt [Erste Schritte](#Getting-started).
Wenn eine zeitbasierte Aufbewahrungsrichtlinie oder ein Zeitraum für die gesetzliche Aufbewahrungspflicht auf einen Container angewendet wird, werden alle vorhandenen Blobs in den unveränderlichen Zustand versetzt (Schreib- und Löschschutz). Alle neuen Blobs, die in den Container hochgeladen werden, werden auch in den unveränderlichen Zustand versetzt.

> [!IMPORTANT]
> Eine zeitbasierte Aufbewahrungsrichtlinie muss *gesperrt* sein, damit das Blob für die Konformität mit SEC 17a-4(f) und anderen gesetzlichen Bestimmungen in einem unveränderlichen Zustand ist (Schreib- und Löschschutz). Es wird empfohlen, für die Sperrung der Richtlinie einen ausreichenden Zeitraum zu wählen, z.B. 24 Stunden. Wir raten davon ab, den *entsperrten* Zustand nur für kurzzeitige Funktionstests zu verwenden.

 Wenn eine zeitbasierte Aufbewahrungsrichtlinie auf einen Container angewendet wird, bleiben alle Blobs im Container so lange im unveränderlichen Zustand, wie der Aufbewahrungszeitraum *gilt*. Die Gültigkeit des Aufbewahrungszeitraums für vorhandene Blobs entspricht der Differenz zwischen der Bloberstellung und dem vom Benutzer angegebenen Aufbewahrungszeitraum. Für neue Blobs entspricht die Gültigkeit des Aufbewahrungszeitraums dem vom Benutzer angegebenen Aufbewahrungszeitraum. Da Benutzer den Aufbewahrungszeitraum ändern können, wird der letzte Wert des vom Benutzer angegebenen Aufbewahrungszeitraums genutzt, um den Aufbewahrungszeitraum zu berechnen.

> [!TIP]
> Beispiel: Der Benutzer erstellt eine zeitbasierte Aufbewahrungsrichtlinie mit einem Aufbewahrungszeitraum von fünf Jahren.
> Dieser Container enthält das bereits vorhandene Blob „testblob1“, das vor einem Jahr erstellt wurde. Der effektive Aufbewahrungszeitraum für „testblob1“ beträgt vier Jahre.
> Das neue Blob „testblob2“ wird jetzt in den Container hochgeladen. Der effektive Aufbewahrungszeitraum für dieses neue Blob beträgt fünf Jahre.

### <a name="legal-holds"></a>Gesetzliche Aufbewahrungspflichten

Bei Zeiträumen für die gesetzliche Aufbewahrungspflicht verbleiben alle vorhandenen und neuen Blobs im unveränderlichen Zustand, bis die gesetzliche Aufbewahrungspflicht aufgehoben wird.
Ausführliche Informationen zum Festlegen und Aufheben von Zeiträumen für die gesetzliche Aufbewahrungspflicht finden Sie im Abschnitt [Erste Schritte](#Getting-started).

Für einen Container können gleichzeitig eine gesetzliche Aufbewahrungspflicht und eine zeitbasierte Aufbewahrungsrichtlinie gelten. Alle Blobs in diesem Container verbleiben so lange im unveränderlichen Zustand, bis alle gesetzlichen Aufbewahrungspflichten aufgehoben wurden. Dies gilt auch, wenn die effektive Aufbewahrungsdauer bereits abgelaufen ist. Dagegen verbleibt ein Blob auch dann so lange im unveränderlichen Zustand, bis der effektive Aufbewahrungszeitraum abgelaufen ist, wenn alle Zeiträume für die gesetzliche Aufbewahrungspflicht aufgehoben wurden.
In der folgenden Tabelle sind die Arten von Blobvorgängen angegeben, die für die unterschiedlichen Szenarien der unveränderlichen Speicherung deaktiviert sind.
Ausführliche Informationen zur Blob-REST-API finden Sie in der Dokumentation zur [Azure Blob-Dienst-API](https://docs.microsoft.com/en-us/rest/api/storageservices/blob-service-rest-api).

|Szenario  |Blobzustand  |Blobvorgänge sind nicht zulässig  |
|---------|---------|---------|
|Effektiver Aufbewahrungszeitraum für das Blob ist noch nicht abgelaufen bzw. ein Zeitraum für die gesetzliche Aufbewahrungspflicht wurde festgelegt     |Unveränderlich: Lösch- und Schreibschutz         |Delete Container, Delete Blob, Put Blob1, Put Block, Put Block List, Set Blob Metadata, Put Page, Set Blob Properties, Snapshot Blob, Incremental Copy Blob, Append Block         |
|Effektiver Aufbewahrungszeitraum für das Blob abgelaufen     |Nur Schreibschutz (Löschvorgänge sind zulässig)         |Put Blob, Put Block, Put Block List, Set Blob Metadata, Put Page, Set Blob Properties, Snapshot Blob, Incremental Copy Blob, Append Block         |
|Alle Zeiträume zur gesetzlichen Aufbewahrungspflicht wurden aufgehoben, und für den Container wurde keine zeitbasierte Aufbewahrungsrichtlinie festgelegt     |Veränderlich         |Keine         |
|Keine WORM-Richtlinie erstellt (zeitbasierte Aufbewahrung oder gesetzliche Aufbewahrungspflicht)     |Veränderlich         |Keine         |

> [!NOTE]
> Der erste „Put Blob“-Vorgang und die Vorgänge „Put Block List“ und „Put Block“, die für das Erstellen eines Blobs erforderlich sind, sind in den ersten beiden Szenarien der obigen Tabelle zulässig. Alle anderen Vorgänge sind nicht zulässig.
> Die Funktion „Unveränderliche Speicherung“ ist nur für GPv2 verfügbar, und die Blobspeicherkonten müssen über den [Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) erstellt werden.

## <a name="pricing"></a>Preise

Für diese Funktion fallen keine zusätzlichen Gebühren an, und für unveränderliche Daten gelten die gleichen Preise wie für veränderliche Daten. Die Preisdetails finden Sie auf der [Seite mit den Preisen für Azure Storage](https://azure.microsoft.com/pricing/details/storage/blobs/).

### <a name="restrictions"></a>Einschränkungen

Während der öffentlichen Vorschauphase gelten die folgenden Einschränkungen:

- **Keine Speicherung von Produktionsdaten oder unternehmenskritischen Daten**
- Es gelten alle Vorschauversion-/Geheimhaltungsvertrag-Einschränkungen

## <a name="getting-started"></a>Erste Schritte

Unveränderlicher Azure-Speicher für Azure-Blobs wird für die aktuellen Releases des [Azure-Portals](http://portal.azure.com), von Azure [CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) und von Azure [PowerShell](https://github.com/Azure/azure-powershell/releases/tag/Azure.Storage.v4.4.0-preview-May2018) unterstützt.

### <a name="azure-portal"></a>Azure-Portal

1. Erstellen Sie einen neuen Container, oder wählen Sie einen vorhandenen Container aus, um die Blobs zu speichern, die im unveränderlichen Zustand aufbewahrt werden müssen.
 Der Container muss sich unter einem GPv2-Speicherkonto befinden.
2. Klicken Sie in den Containereinstellungen auf die Zugriffsrichtlinie und dann unter der Richtlinie **Unveränderlicher Blobspeicher** auf **+ Richtlinie hinzufügen**. Dies ist unten dargestellt.

    ![Portal](media/storage-blob-immutable-storage/portal-image-1.png)

3. Wählen Sie im Dropdownmenü die Option „Zeitbasierte Aufbewahrung“, um die zeitbasierte Aufbewahrung zu aktivieren.

    ![Aufbewahrung](media/storage-blob-immutable-storage/portal-image-2.png)

4. Geben Sie den gewünschten Aufbewahrungszeitraum in Tagen ein (Minimum ist 1 Tag).

    ![Aufbewahrungszeitraum](media/storage-blob-immutable-storage/portal-image-5-retention-interval.png)

    Wie oben zu sehen ist, ist die Richtlinie im anfänglichen Zustand entsperrt. Auf diese Weise können Sie die Funktion mit einem kleineren Aufbewahrungszeitraum testen und vor dem Sperren Änderungen an der Richtlinie vornehmen. Das Sperren ist wichtig, um SEC 17a-4 und andere Bestimmungen einzuhalten.

5. Sperren Sie die Richtlinie, indem Sie mit der rechten Maustaste auf die Auslassungspunkte (...) klicken. Das folgende Menü wird angezeigt:

    ![Richtlinie sperren](media/storage-blob-immutable-storage/portal-image-4-lock-policy.png)

    Klicken Sie auf „Richtlinie sperren“. Die Richtlinie wird jetzt als gesperrt angezeigt. Nach dem Sperren kann die Richtlinie nicht mehr gelöscht werden, und es sind lediglich Verlängerungen des Aufbewahrungszeitraums zulässig.

6. Klicken Sie zum Aktivieren von gesetzlichen Aufbewahrungspflichten auf „+ Richtlinie hinzufügen“, und wählen Sie im Dropdownmenü die Option „Gesetzliche Aufbewahrungspflicht“.

    ![Gesetzliche Aufbewahrungspflicht](media/storage-blob-immutable-storage/portal-image-legal-hold-selection-7.png)

7. Erstellen Sie einen Zeitraum für die gesetzliche Aufbewahrungspflicht mit mindestens einem Tag.

    ![Legen Sie Tags für die gesetzliche Aufbewahrungspflicht fest.](media/storage-blob-immutable-storage/portal-image-set-legal-hold-tags.png)

### <a name="cli-20"></a>CLI 2.0

Installieren Sie die [CLI-Erweiterung](http://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) mit `az extension add -n storage-preview`.

Wenn Sie die Erweiterung bereits installiert haben, können Sie den folgenden Befehl verwenden, um die Funktion „Unveränderlicher Speicher“ zu aktivieren: `az extension update -n storage-preview`.

Die Funktion ist in den folgenden Befehlsgruppen enthalten (führen Sie „-h“ dafür aus, um die Befehle anzuzeigen): `az storage container immutability-policy` und `az storage container legal-hold`.

### <a name="powershell"></a>PowerShell

Die Funktion „Unveränderlicher Speicher“ wird für die [PowerShell-Version 4.4.0-preview](https://github.com/Azure/azure-powershell/releases/tag/Azure.Storage.v4.4.0-preview-May20180) unterstützt.
Führen Sie die folgenden Schritte aus, um die Funktion zu aktivieren:

1. Vergewissern Sie sich mit `Install-Module PowerShellGet –Repository PSGallery –Force`, dass die aktuelle Version von PowerShellGet installiert ist.
2. Entfernen Sie alle vorherigen Installationen von Azure PowerShell.
3. Installieren Sie AzureRM (Azure kann über dieses Repository auf ähnliche Weise installiert werden): `Install-Module AzureRM –Repository PSGallery –AllowClobber`.
4. Installieren Sie die Vorschauversion der Cmdlets für die Speicherverwaltungsebene: `Install-Module -Name AzureRM.Storage -AllowPrerelease -Repository PSGallery -AllowClobber`.

Unten ist ein einfacher PowerShell-Code angegeben, mit dem die Nutzung der Funktion veranschaulicht wird.

## <a name="client-libraries"></a>Clientbibliotheken

Die Funktion „Unveränderlicher Speicher“ für Azure-Blobs wird für die folgenden Releases der Clientbibliothek unterstützt:

- [.NET-Clientbibliothek (Version 7.2.0-preview oder höher)](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/7.2.0-preview)
- [node.js-Clientbibliothek (Version 4.0.0 oder höher)](https://www.npmjs.com/package/azure-arm-storage)
- [Python-Clientbibliothek (Version 2.0.0 Release Candidate 2 oder höher)](https://pypi.org/project/azure-mgmt-storage/2.0.0rc1/)

## <a name="supported-values"></a>Unterstützte Werte

- Der Mindestwert für den Aufbewahrungszeitraum beträgt einen Tag, und der Höchstwert beträgt 400 Jahre.
- Für ein bestimmtes Speicherkonto beträgt die maximale Anzahl von Containern pro Speicherkonto mit gesperrten Unveränderlichkeitsrichtlinien 1.000.
- Für ein bestimmtes Speicherkonto beträgt die maximale Anzahl von Containern mit einer gesetzlichen Aufbewahrungspflicht 1.000.
- Für einen bestimmten Container beträgt die maximale Anzahl von Tags für die gesetzliche Aufbewahrungspflicht 10.
- Ein Tag für die gesetzliche Aufbewahrungspflicht darf maximal 23 alphanumerische Zeichen lang sein und muss mindestens drei Zeichen lang sein.
- Für einen bestimmten Container beträgt die maximale Anzahl von zulässigen Verlängerungen des Aufbewahrungszeitraums für gesperrte Unveränderlichkeitsrichtlinien 3.
- Für einen bestimmten Container mit einer gesperrten Unveränderlichkeitsrichtlinie können maximal 5 Protokolle für zeitbasierte Aufbewahrungsrichtlinien und maximal 10 Protokolle für Richtlinien zur gesetzlichen Aufbewahrungspflicht verwendet werden, die für den Containerzeitraum aufbewahrt werden.

## <a name="faq"></a>Häufig gestellte Fragen

**Gilt die Funktion nur für Blockblobs oder auch für Seiten- und Anfügeblobs?**

Die Funktion „Unveränderlicher Speicher“ für Blobs kann mit einem beliebigen Blobtyp verwendet werden.  Beachten Sie aber die Empfehlung, die Funktion hauptsächlich für Blockblobs zu verwenden. Im Gegensatz zu Blockblobs müssen Seiten- und Anfügeblobs außerhalb eines WORM-Containers erstellt und dann hineinkopiert werden.  Nach dem Kopieren in einen WORM-Container sind keine weiteren *Anfügevorgänge* an ein Anfügeblob oder Änderungen eines Seitenblobs zulässig.

**Muss ich immer ein neues Speicherkonto erstellen, um diese Funktion zu nutzen?**

Sie können die Funktion „Unveränderlicher Speicher“ mit allen vorhandenen GPv2-Konten oder für neue Speicherkonten verwenden, wenn der Kontotyp GPv2 lautet. Diese Funktion ist nur für Blobspeicher verfügbar.

**Was passiert, wenn ich versuche, einen Container mit einer *gesperrten* zeitbasierten Aufbewahrungsrichtlinie oder gesetzlichen Aufbewahrungspflicht zu löschen?**

Beim Vorgang „Delete Container“ tritt ein Fehler auf, wenn es sich um mindestens ein Blob mit einer gesperrten zeitbasierten Aufbewahrungsrichtlinie oder einer gesetzlichen Aufbewahrungspflicht handelt. Der Vorgang „Delete Container“ ist erfolgreich, wenn kein Blob mit einem aktiven Aufbewahrungszeitraum vorhanden ist und keine Zeiträume für die gesetzliche Aufbewahrungspflicht aktiv sind. Sie müssen zuerst die Blobs löschen, bevor Sie den Container löschen können.

**Was passiert, wenn ich versuche, ein Speicherkonto mit einem WORM-Container zu löschen, das über eine *gesperrte* zeitbasierte Aufbewahrungsrichtlinie oder eine gesetzliche Aufbewahrungspflicht verfügt?**

Für das Löschen des Speicherkontos tritt ein Fehler auf, wenn mindestens ein WORM-Container mit einer gesetzlichen Aufbewahrungspflicht oder ein Blob mit einem aktiven Aufbewahrungszeitraum vorhanden ist.  Alle WORM-Container müssen gelöscht werden, bevor das Speicherkonto gelöscht werden kann.  Siehe Frage #2 mit Informationen zum Löschen des Containers.

**Kann ich die Daten zwischen unterschiedlichen Blobebenen („Hot“, „Cool“, „Cold“) verschieben, wenn sich das Blob im unveränderlichen Zustand befindet?**

Ja. Sie können den Befehl „Set Blob Tier“ nutzen, um Daten zwischen den Blobebenen zu verschieben, während die Daten im unveränderlichen Zustand verbleiben. Die Funktion „Unveränderlicher Speicher“ wird für die Blobebenen „Hot“, „Cool“ und „Cold“ unterstützt.

**Was passiert, wenn ich nicht bezahle und mein Aufbewahrungszeitraum abgelaufen ist?**

Bei einer fehlenden Zahlung gelten die üblichen Richtlinien zur Karenz der Datenaufbewahrung gemäß den Bedingungen Ihres Vertrags mit Microsoft.

**Bieten Sie einen Test- oder Karenzzeitraum an, in dem die Funktion ausprobiert werden kann?**

Ja. Wenn eine zeitbasierte Aufbewahrungsrichtlinie erstellt wird, befindet sie sich zunächst im *entsperrten* Zustand. In diesem Zustand können Sie alle gewünschten Änderungen am Aufbewahrungszeitraum vornehmen, z.B. diesen verlängern oder verkürzen und auch die Richtlinie löschen. Nachdem die Richtlinie gesperrt wurde, bleibt die Sperrung für immer erhalten, um das Löschen zu verhindern. Außerdem kann der Aufbewahrungszeitraum nicht mehr verkürzt werden, nachdem die Richtlinie gesperrt wurde. Es wird dringend empfohlen, den *entsperrten* Zustand nur für Testzwecke zu nutzen und die Richtlinie innerhalb eines Zeitraums von 24 Stunden wieder zu sperren, damit nicht das Risiko einer Nichteinhaltung von SEC 17a-4(f) und anderen Bestimmungen besteht.

**Ist die Funktion auch für nationale und behördliche Cloudumgebungen verfügbar?**

Die Funktion „Unveränderlicher Speicher“ ist derzeit nur in öffentlichen Azure-Regionen verfügbar. Senden Sie eine E-Mail an azurestoragefeedback@microsoft.com, wenn Sie an einer bestimmten nationalen Cloudumgebung interessiert sind.

## <a name="sample-code"></a>Beispielcode

Unten ist ein PowerShell-Beispielskript als Referenz angegeben.
Mit diesem Skript werden ein neues Speicherkonto und ein Container erstellt. Anschließend wird veranschaulicht, wie Sie Zeiträume für gesetzliche Aufbewahrungspflichten festlegen und löschen und eine zeitbasierte Aufbewahrungsrichtlinie (ImmutabilityPolicy) erstellen und sperren, den Aufbewahrungszeitraum verlängern usw.

```powershell
\$ResourceGroup = "\<Enter your resource group\>”

\$StorageAccount = "\<Enter your storage account name\>"

\$container = "\<Enter your container name\>"

\$container2 = "\<Enter another container name\>”

\$location = "\<Enter the storage account location\>"

\# Login to the Azure Resource Manager Account

Login-AzureRMAccount

Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Storage"

\# Create your Azure Resource Group

New-AzureRmResourceGroup -Name \$ResourceGroup -Location \$location

\# Create your Azure storage account

New-AzureRmStorageAccount -ResourceGroupName \$ResourceGroup -StorageAccountName
\$StorageAccount -SkuName Standard_LRS -Location \$location -Kind Storage

\# Create a new container

New-AzureRmStorageContainer -ResourceGroupName \$ResourceGroup
-StorageAccountName \$StorageAccount -Name \$container

\# Create Container 2 with Storage Account object

\$accountObject = Get-AzureRmStorageAccount -ResourceGroupName \$ResourceGroup
-StorageAccountName \$StorageAccount

New-AzureRmStorageContainer -StorageAccount \$accountObject -Name \$container2

\# Get container

Get-AzureRmStorageContainer -ResourceGroupName \$ResourceGroup
-StorageAccountName \$StorageAccount -Name \$container

\# Get Container with Account object

\$containerObject = Get-AzureRmStorageContainer -StorageAccount \$accountObject
-Name \$container

\#list container

Get-AzureRmStorageContainer -ResourceGroupName \$ResourceGroup
-StorageAccountName \$StorageAccount

\#remove container (Add -Force to dismiss prompt)

Remove-AzureRmStorageContainer -ResourceGroupName \$ResourceGroup
-StorageAccountName \$StorageAccount -Name \$container2

\#with Account object

Remove-AzureRmStorageContainer -StorageAccount \$accountObject -Name
\$container2

\#with Container object

\$containerObject2 = Get-AzureRmStorageContainer -StorageAccount \$accountObject
-Name \$container2

Remove-AzureRmStorageContainer -InputObject \$containerObject2

\#Set LegalHold

Add-AzureRmStorageContainerLegalHold -ResourceGroupName \$ResourceGroup
-StorageAccountName \$StorageAccount -Name \$container -Tag tag1,tag2

\#with Account object

Add-AzureRmStorageContainerLegalHold -StorageAccount \$accountObject -Name
\$container -Tag tag3

\#with Container object

Add-AzureRmStorageContainerLegalHold -Container \$containerObject -Tag tag4,tag5

\#Clear LegalHold

Remove-AzureRmStorageContainerLegalHold -ResourceGroupName \$ResourceGroup
-StorageAccountName \$StorageAccount -Name \$container -Tag tag2

\#with Account object

Remove-AzureRmStorageContainerLegalHold -StorageAccount \$accountObject -Name
\$container -Tag tag3,tag5

\#with Container object

Remove-AzureRmStorageContainerLegalHold -Container \$containerObject -Tag tag4

\# create/update ImmutabilityPolicy

\#\# with account/container name

Set-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName \$ResourceGroup
-StorageAccountName \$StorageAccount -ContainerName \$container
-ImmutabilityPeriod 10

\#with Account object

Set-AzureRmStorageContainerImmutabilityPolicy -StorageAccount \$accountObject
-ContainerName \$container -ImmutabilityPeriod 1 -Etag \$policy.Etag

\#with Container object

\$policy = Set-AzureRmStorageContainerImmutabilityPolicy -Container
\$containerObject -ImmutabilityPeriod 7

\#\# with ImmutabilityPolicy object

Set-AzureRmStorageContainerImmutabilityPolicy -ImmutabilityPolicy \$policy
-ImmutabilityPeriod 5

\#get ImmutabilityPolicy

Get-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName \$ResourceGroup
-StorageAccountName \$StorageAccount -ContainerName \$container

\#with Account object

Get-AzureRmStorageContainerImmutabilityPolicy -StorageAccount \$accountObject
-ContainerName \$container

\#with Container object

Get-AzureRmStorageContainerImmutabilityPolicy -Container \$containerObject

\#Lock ImmutabilityPolicy (Add -Force to dismiss prompt)

\#\# with ImmutabilityPolicy object

\$policy = Get-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName
\$ResourceGroup -StorageAccountName \$StorageAccount -ContainerName \$container

\$policy = Lock-AzureRmStorageContainerImmutabilityPolicy -ImmutabilityPolicy
\$policy -force

\#\# with account/container name

\$policy = Lock-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName
\$ResourceGroup -StorageAccountName \$StorageAccount -ContainerName \$container
-Etag \$policy.Etag

\#with Account object

\$policy = Lock-AzureRmStorageContainerImmutabilityPolicy -StorageAccount
\$accountObject -ContainerName \$container -Etag \$policy.Etag

\#with Container object

\$policy = Lock-AzureRmStorageContainerImmutabilityPolicy -Container
\$containerObject -Etag \$policy.Etag -force

\#Extend ImmutabilityPolicy

\#\# with ImmutabilityPolicy object

\$policy = Get-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName
\$ResourceGroup -StorageAccountName \$StorageAccount -ContainerName \$container

\$policy = Set-AzureRmStorageContainerImmutabilityPolicy -ImmutabilityPolicy
\$policy -ImmutabilityPeriod 11 -ExtendPolicy

\#\# with account/container name

\$policy = Set-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName
\$ResourceGroup -StorageAccountName \$StorageAccount -ContainerName \$container
-ImmutabilityPeriod 11 -Etag \$policy.Etag -ExtendPolicy

\#with Account object

\$policy = Set-AzureRmStorageContainerImmutabilityPolicy -StorageAccount
\$accountObject -ContainerName \$container -ImmutabilityPeriod 12 -Etag
\$policy.Etag -ExtendPolicy

\#with Container object

\$policy = Set-AzureRmStorageContainerImmutabilityPolicy -Container
\$containerObject -ImmutabilityPeriod 13 -Etag \$policy.Etag -ExtendPolicy

\#Remove ImmutabilityPolicy (Add -Force to dismiss prompt)

\#\# with ImmutabilityPolicy object

\$policy = Get-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName
\$ResourceGroup -StorageAccountName \$StorageAccount -ContainerName \$container

Remove-AzureRmStorageContainerImmutabilityPolicy -ImmutabilityPolicy \$policy

\#\# with account/container name

Remove-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName
\$ResourceGroup -StorageAccountName \$StorageAccount -ContainerName \$container
-Etag \$policy.Etag

\#with Account object

Remove-AzureRmStorageContainerImmutabilityPolicy -StorageAccount \$accountObject
-ContainerName \$container -Etag \$policy.Etag

\#with Container object

Remove-AzureRmStorageContainerImmutabilityPolicy -Container \$containerObject
-Etag \$policy.Etag
```