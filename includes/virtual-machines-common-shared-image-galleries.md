---
title: Includedatei
description: Includedatei
services: virtual-machines
author: axayjo
ms.service: virtual-machines
ms.topic: include
ms.date: 09/20/2018
ms.author: akjosh; cynthn
ms.custom: include file
ms.openlocfilehash: 56f48f6676b3c878effe788316024e642c17ff5a
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49400341"
---
Katalog mit geteilten Images ist ein Dienst, der Ihnen hilft, Ihre benutzerdefinierten VM-Images zu strukturieren und organisieren. Der Katalog mit geteilten Images bietet drei Hauptwertbeiträge
- Einfache Verwaltung
- Skalieren Ihrer benutzerdefinierten Images
- Teilen Ihrer Images – Teilen Sie Ihre Images mit verschiedenen Benutzern, Dienstprinzipalen oder AD-Gruppen in Ihrer Organisation sowie mit verschiedenen Regionen, indem Sie die Replikation in mehrere Regionen verwenden.

Ein verwaltetes Image ist eine Kopie entweder einer vollständigen VM (einschließlich sämtlicher angefügter Datenträger) oder lediglich des Betriebssystemdatenträgers, je nachdem, wie Sie das Image erstellen. Wenn Sie aus dem Image einen virtuellen Computer erstellen, werden die Kopien der virtuellen Festplatten in dem Image verwendet, um die Datenträger für die neue VM zu erstellen. Das verwaltete Image verbleibt im Speicher und kann immer wieder zum Erstellen neuer VMs verwendet werden.

Wenn Sie eine große Anzahl verwalteter Images haben, die Sie pflegen müssen und im gesamten Unternehmen zur Verfügung stellen möchten, können Sie einen Katalog mit geteilten Images als Repository verwenden, mit dem sie Ihre Images ganz einfach aktualisieren und teilen können. Die Gebühren für die Verwendung eines Katalogs mit geteilten Images sind lediglich die Kosten für den von den Images verwendeten Speicher, zuzüglich aller Kosten für ausgehenden Netzwerkdatenverkehr für die Replikation von Images aus der Quellregion in die veröffentlichten Regionen.

Die Funktion „Katalog mit geteilten Images“ verfügt über mehrere Ressourcentypen:

| Ressource | BESCHREIBUNG|
|----------|------------|
| **Verwaltetes Image** | Dies ist ein Basisimage, das eigenständig verwendet werden kann oder zum Erstellen mehrerer **geteilter Imageversionen** in einem Imagekatalog.|
| **Imagekatalog** | Wie der Azure Marketplace ist ein **Imagekatalog** ein Repository zum Verwalten und Teilen von Images, aber Sie kontrollieren, wer Zugriff hat. |
| **Katalogimage** | Abbilder sind innerhalb eines Katalogs definiert und enthalten intern Informationen über das Image und die Anforderungen für seine Verwendung. Dies schließt ein, ob das Image Windows oder Linux ist, Anmerkungen zu dieser Version und Anforderungen an den minimalen und maximalen Arbeitsspeicher. Dieser Typ von Image ist eine Ressource im Resource Manager-Bereitstellungsmodell, er wird aber nicht direkt zum Erstellen von VMs verwendet. Es ist eine Definition eines Imagetyps. |
| **Version eines geteilten Images** | Eine **Imageversion** ist, was Sie verwenden, um einen virtuellen Computer zu erstellen, wenn Sie einen Katalog verwenden. Sie können nach Bedarf mehrere Versionen eines Images für Ihre Umgebung haben. Wie bei einem verwalteten Image wird, wenn Sie eine **Imageversion** zum Erstellen einer VM verwenden, wird die Imageversion verwendet, um neue Datenträger für den virtuellen Computer zu erstellen. Imageversionen können mehrmals verwendet werden. |

<br>


![Eine Abbildung, die zeigt, wie Sie mehrere Versionen eines Images in Ihrem Katalog haben können.](./media/shared-image-galleries/shared-image-gallery.png)

### <a name="regional-support"></a>Regionsunterstützung

Die Regionsunterstützung für Kataloge mit geteilten Images ist beschränkt, wird aber im Laufe der Zeit erweitert. Für die Preview finden Sie hier die Listen der Orte, an denen Sie Kataloge erstellen, sowie die Regionen, in die Sie jeden Katalog replizieren können: 

| Katalog erstellen in  | Version replizieren in |
|--------------------|----------------------|
| USA, Westen-Mitte    |USA Süd Mitte|
| USA (Ost) 2          |USA (Ost)|
| USA Süd Mitte   |USA (Ost) 2|
| Asien, Südosten     |USA (Westen)|
| Europa, Westen        |USA, Westen 2|
|                    |USA (Mitte)|
|                    |USA Nord Mitte|
|                    |Kanada, Mitte|
|                    |Kanada, Osten|
|                    |Nordeuropa|
|                    |Europa, Westen|
|                    |Indien (Süden)|
|                    |Asien, Südosten|



## <a name="scaling"></a>Skalieren
Katalog mit geteilten Images gestatten Ihnen die Angabe der Anzahl der Replikate, die Azure für die Images pflegen soll. Dies ist in Szenarien mit mehreren VM-Bereitstellungen hilfreich, da die VM-Bereitstellungen auf verschiedene Replikate verteilt werden können, wodurch die Wahrscheinlichkeit verringert wird, dass der Instanzerstellungsprozess gedrosselt wird, weil ein einzelnes Replikat überlastet wird.

![Eine Abbildung, die zeigt, wie Sie Images skalieren können](./media/shared-image-galleries/scaling.png)


## <a name="replication"></a>Replikation
Der Katalog mit geteilten Images ermöglicht Ihnen außerdem die automatische Replikation Ihrer Images in andere Azure-Regionen. Jede freigegebene Imageversion kann in unterschiedliche Regionen repliziert werden, je nachdem, was für Ihre Organisation sinnvoll ist. Ein Beispiel ist, immer das neueste Image in mehrere Regionen zu replizieren, während alle ältere Versionen nur in einer Region zur Verfügung stehen. Dadurch können Speicherkosten für geteilte Imageversionen gesenkt werden. Die Regionen, in die eine geteilte Imageversion repliziert wird, können nach dem Zeitpunkt der Erstellung aktualisiert werden. Der Zeitaufwand für die Replikation in verschiedene Regionen hängt von der Datenmenge ab, die kopiert wird, sowie der Anzahl der Regionen, in die die Version repliziert wird. Dies kann in manchen Fällen einige Stunden dauern. Während die Replikation durchgeführt wird, können Sie den Status der Replikation pro Region anzeigen. Sobald die Imagereplikation in einer Region abgeschlossen ist, können Sie einen virtuellen Computer oder eine VMSS unter Verwendung dieser Version in der Region bereitstellen.

![Eine Abbildung, die zeigt, wie Sie Images replizieren können](./media/shared-image-galleries/replication.png)


## <a name="access"></a>Access
Da es sich bei dem Katalog mit geteilten Images, dem geteilten Image und der geteilten Imageversion alles um Ressourcen handelt, lassen sie sich mithilfe der integrierten nativen Azure-RBAC-Steuerelemente teilen. Mithilfe der rollenbasierten Zugriffssteuerung können Sie diese Ressourcen mit anderen Benutzern, Dienstprinzipalen und Gruppen in Ihrer Organisation teilen. Der Umfang des Teilens dieser Ressourcen liegt innerhalb desselben AD-Mandanten. Sobald ein Benutzer Zugriff auf die geteilte Imageversion hat, kann er einen virtuellen Computer oder eine VM-Skalierungsgruppe in jedem der Abonnements bereitstellen, auf die er Zugriff im selben AD-Mandanten wie die geteilte Imageversion hat.  Im Folgenden finden Sie die Matrix für das Teilen, die dabei hilft, zu verstehen, worauf der Benutzer Zugriff erhält:

| Geteilt mit Benutzer     | Gemeinsamer Image-Katalog | Geteiltes Image | Version eines geteilten Images |
|----------------------|----------------------|--------------|----------------------|
| Gemeinsamer Image-Katalog | JA                  | Ja          | JA                  |
| Geteiltes Image         | Nein                    | Ja          | JA                  |
| Version eines geteilten Images | Nein                    | Nein            | JA                  |



## <a name="billing"></a>Abrechnung
Für die Verwendung des Katalogs mit geteilten Images fällt keine zusätzliche Gebühren an. Für folgende Ressourcen werden Gebühren berechnet:
- Speicherkosten der Speicherung der geteilten Imageversionen. Dies hängt von der Anzahl der Replikate der Version ab sowie der Anzahl der Regionen, in die die Version repliziert wird.
- Gebühren für ausgehenden Netzwerkdatenverkehr für die Replikation aus der Quellregion der Version in die replizierten Regionen.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen 

**F.** Wie registriere ich mich für die öffentliche Preview des Katalogs mit geteilten Images?
 
 A. Um sich für die öffentliche Preview des Katalogs mit geteilten Images zu registrieren, müssen Sie sich für die Funktion registrieren, indem Sie die folgenden Befehle aus jedem der Abonnements ausführen, in dem Sie beabsichtigen, einen Katalog mit geteilten Images, eine Imagedefinition oder Imageversionsressourcen zu erstellen, und ebenfalls wo Sie beabsichtigen, mithilfe der Imageversionen virtuelle Computer bereitzustellen.

**CLI**: 

```bash 
az feature register --namespace Microsoft.Compute --name GalleryPreview
az provider register -n Microsoft.Compute
```

**PowerShell**: 

```powershell
Register-AzureRmProviderFeature -FeatureName GalleryPreview -ProviderNamespace Microsoft.Compute
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
```

**F.** Wie kann ich alle Ressourcen eines Katalogs mit geteilten Images über Abonnements hinweg auflisten? 
 
 A. Um alle Ressourcen eines Katalogs mit geteilten Images über Abonnements hinweg aufzulisten, auf die Sie im Azure-Portal Zugriff haben, führen Sie die folgenden Schritte aus:

 1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
 1. Wechseln Sie zu **Alle Ressourcen**.
 1. Wählen Sie alle Abonnements aus, von denen Sie alle Ressourcen auflisten möchten.
 1. Suchen Sie nach Ressourcen vom Typ **Privater Katalog**.
 
 Um die Imagedefinitionen und Imageversionen anzuzeigen, sollten Sie ebenfalls **Ausgeblendete Typen anzeigen** auswählen.
 
 Um alle Ressourcen eines Katalogs mit geteilten Images über Abonnements hinweg aufzulisten, auf die Sie Zugriff haben, verwenden Sie den folgenden Befehl in der Azure CLI:

 ```bash
 az account list -otsv --query "[].id" | xargs -n 1 az sig list --subscription
 ```


**F.** Wie teile ich meine Images über Abonnements hinweg?
 
 A. Sie können Images zwischen Abonnements mithilfe der rollenbasierten Zugriffssteuerung (Role Based Access Control, RBAC) teilen. Jeder Benutzer, der Leseberechtigungen für eine Imageversion besitzt, auch über Abonnements hinweg, kann mithilfe der Imageversion einen virtuellen Computer bereitstellen.


**F.** Kann ich mein vorhandenes Image in den Katalog mit geteilten Images verschieben?
 
 A. Ja. Es gibt 3 Szenarien, die auf den Typen von Images basieren, die Sie haben können.

 Szenario 1: Wenn Sie ein verwaltetes Image haben, können Sie daraus eine Imagedefinition und eine Imageversion erstellen.

 Szenario 2: Wenn Sie ein nicht verwaltetes, generalisiertes Image haben, können Sie daraus ein verwaltetes Image erstellen und dann daraus eine Imagedefinition und eine Imageversion erstellen. 

 Szenario 3: Wenn Sie eine VHD in Ihrem lokalen Dateisystem haben, müssen Sie die VHD hochladen, ein verwaltetes Image erstellen, und dann können Sie daraus eine Imagedefinition und eine Imageversion erstellen. 
    - Wenn die VHD von einer Windows-VM stammt, lesen Sie [Hochladen einer generalisierten VHD](https://docs.microsoft.com/azure/virtual-machines/windows/upload-generalized-managed).
    - Wenn die VHD für eine Linux-VM ist, lesen Sie [Hochladen einer VHD](https://docs.microsoft.com/azure/virtual-machines/linux/upload-vhd#option-1-upload-a-vhd).


**F.** Kann ich eine Imageversion von einem speziellen Datenträger erstellen?

 A. Nein, zurzeit unterstützen wir keine speziellen Datenträger als Images. Wenn Sie einen speziellen Datenträger haben, müssen Sie [einen virtuellen Computers aus der VHD erstellen](https://docs.microsoft.com/azure/virtual-machines/windows/create-vm-specialized-portal#create-a-vm-from-a-disk), indem Sie den speziellen Datenträger einem neuen virtuellen Computer anfügen. Nachdem Sie einen virtuellen Computer haben, der ausgeführt wird, müssen Sie die Anweisungen befolgen, um ein verwaltetes Image aus der [Windows-VM] (https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-custom-images) oder [Linux-VM](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-custom-images)) zu erstellen. Nachdem Sie ein generalisiertes, verwaltetes Image haben, können Sie den Prozess zum Erstellen einer geteilten Imagebeschreibung und Imageversion starten.


**F.** Kann ich einen Katalog mit geteilten Images, eine Imagedefinition und eine Imageversion über das Azure-Portal erstellen?

 A. Nein, zurzeit unterstützen wir keine Erstellung jeglicher Ressourcen von Katalogen mit geteilten Images über das Azure-Portal. Wir unterstützen aber die Erstellung der Ressourcen von Katalogen mit geteilten Images mittels CLI, Vorlagen und SDKs. PowerShell wird ebenfalls bald freigegeben.

 
**F.** Kann ich nach der Erstellung die Imagedefinition oder die Imageversion aktualisieren? Welche Arten von Details kann ich ändern?

 A. Die Details, die für jede der Ressourcen aktualisiert werden können, sind unten aufgeführt:
 
Katalog mit geteilten Images:
- BESCHREIBUNG

Imagedefinition:
- Empfohlene vCPUs
- Arbeitsspeicher
- BESCHREIBUNG
- Datum für Ende des Lebenszyklus

Imageversion:
- Anzahl regionaler Replikate
- Zielregionen
- Ausschluss aus neuester Version
- Datum für Ende des Lebenszyklus


**F.** Kann ich nach der Erstellung die Katalogressource mit geteilten Images in ein anderes Abonnement verschieben?

 A. Nein, Sie können Katalogressource mit geteilten Images nicht in ein anderes Abonnement verschieben. Sie können allerdings die Imageversionen im Katalog nach Bedarf in andere Regionen replizieren.

**F.** Kann ich meine Imageversionen zwischen Clouds replizieren – Azure China 21Vianet, Azure Deutschland und Azure Government Cloud? 

 A. Nein, Sie können Imageversionen nicht zwischen Clouds replizieren.

**F.** Kann ich meine Imageversionen zwischen Abonnements replizieren? 

 A. Nein, Sie können die Imageversionen zwischen Regionen in einem Abonnement replizieren und mittels rollenbasierter Zugriffssteuerung in anderen Abonnements verwenden.

**F.** Kann ich Imageversionen zwischen AD-Mandanten teilen? 

 A. Nein, zurzeit unterstützt der Katalog mit geteilten Images kein Teilen von Imageversionen zwischen AD-Mandanten. Allerdings können Sie das Feature „Private Angebote“ im Azure Marketplace verwenden, um dies zu erreichen.


**F.** Wie lange dauert die Replikation von Imageversionen zwischen Zielregionen?

 A. Die Replikationszeit für Imageversionen ist vollständig abhängig von der Größe des Images und der Anzahl der Regionen, in die es repliziert wird. Allerdings wird als bewährte Methode empfohlen, dass Sie das Image klein halten und die Quell- und Zielregionen möglichst benachbart, um optimale Ergebnisse zu erzielen. Sie können den Status der Replikation mit dem Flag „-ReplicationStatus“ überprüfen.


**F.** Wie viele Kataloge mit geteilten Images kann ich in einem Abonnement erstellen?

 A. Das Standardkontingent ist 
- 10 Kataloge mit geteilten Images pro Abonnement und Region
- 200 Imagedefinitionen pro Abonnement und Region
- 2000 Imageversionen pro Abonnement und Region


**F.** Was ist der Unterschied zwischen Quellregion und Zielregion?

 A. Quellregion ist die Region, in der Ihre Imageversion erstellt wird, und Zielregionen sind die Regionen, in denen eine Kopie Ihrer Imageversion gespeichert werden soll. Für jede Imageversion können Sie nur eine Quellregion haben. Stellen Sie außerdem sicher, dass Sie beim Erstellen einer Imageversion den Ort der Quellregion als eine der Zielregionen übergeben.  


**F.** Wie gebe ich die Quellregion beim Erstellen der Imageversion an?

 A. Beim Erstellen einer Imageversion können Sie den Parameter **--location** in der CLI und den Parameter **-Location** in der PowerShell verwenden, um die Quellregion anzugeben. Stellen Sie sicher, dass sich das verwaltete Image, das Sie als Basisimage für die Erstellung der Imageversion verwenden, am selben Ort befindet, an dem Sie die Imageversion erstellen möchten. Stellen Sie außerdem sicher, dass Sie beim Erstellen einer Imageversion den Ort der Quellregion als eine der Zielregionen übergeben.  


**F.** Wie gebe ich die Anzahl der Imageversionsreplikate an, die in jeder Region erstellt werden sollen?

 A. Es gibt zwei Möglichkeiten, wie Sie die Anzahl der Imageversionsreplikate angeben können, die in jeder Region erstellt werden sollen:
 
1. Die regionale Replikatanzahl, die die Anzahl der Replikate angibt, die Sie pro Region erstellen möchten. 
2. Die allgemeine Replikatanzahl, bei der es sich um die Standardanzahl pro Region handelt für den Fall, dass die regionale Replikatanzahl nicht angegeben ist. 

Um die regionale Replikatanzahl anzugeben, übergeben Sie den Ort zusammen mit der Anzahl der Replikate, die Sie in dieser Region erstellen möchten, wie folgt : „USA, Süden-Mitte=2“. 

Wenn die regionale Replikatanzahl nicht für jeden Ort angegeben ist, entspricht die Standardanzahl der Replikate der allgemeinen Replikatanzahl, die Sie angegeben haben. 

Um die allgemeine Replikatanzahl in der CLI anzugeben, verwenden Sie das Argument **--replica-count** in dem Befehl `az sig image-version create`.


**F.** Kann ich den Katalog mit geteilten Images an einem anderen Ort als dem erstellen, an dem ich die Imagedefinition und Imageversion erstellen möchte?

 A. Ja, das ist möglich. Als bewährte Methode empfehlen wir Ihnen aber, dass Sie die Ressourcengruppe, den Katalog mit geteilten Images, die Imagedefinition und die Imageversion am selben Ort halten.


**F.** Welche Gebühren fallen für die Verwendung des Katalogs mit geteilten Images an?

 A. Es fallen keine Gebühren für die Verwendung des Diensts für Kataloge mit geteilten Images an, außer den Speichergebühren für das Speichern der Imageversionen sowie den Gebühren für ausgehenden Netzwerkdatenverkehr für die Replikation der Imageversionen aus der Quellregion in Zielregionen.

**F.** Welche API-Version sollte ich verwenden, um einen Katalog mit geteilten Images, eine Imagedefinition, eine Imageversion und aus der Imageversion eine VM/VMSS zu erstellen?

 A. Für Bereitstellungen von VMs und VM-Skalierungsgruppen mithilfe einer Imageversion empfehlen wir Ihnen die Verwendung der API-Version 2018-04-01 oder höher. Um mit Katalogen mit geteilten Images, Imagedefinitionen und Imageversionen zu arbeiten, empfehlen wir Ihnen die Verwendung der API-Version 2018-06-01. 