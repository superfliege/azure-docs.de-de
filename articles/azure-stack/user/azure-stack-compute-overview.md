---
title: Einführung in virtuelle Azure Stack-Computer
description: Erfahren Sie mehr über virtuelle Azure Stack-Computer.
services: azure-stack
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.topic: get-started-article
ms.date: 05/21/2018
ms.author: mabrigg
ms.reviewer: kivenkat
ms.openlocfilehash: 967fcb86c1bf0c85517bc13c2066ed32e8fa28d9
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34604130"
---
# <a name="introduction-to-azure-stack-virtual-machines"></a>Einführung in virtuelle Azure Stack-Computer

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Azure Stack bietet virtuelle Computer als Option für bedarfsgesteuerte, skalierbare Computingressourcen. Sie können virtuelle Computer verwenden, wenn Sie mehr Kontrolle über Ihre Computingumgebung benötigen als bei den anderen Optionen. Lesen Sie vor dem Erstellen des virtuellen Computers die Informationen in diesem Artikel.

Ein virtueller Azure Stack-Computer bietet die Flexibilität der Virtualisierung, ohne dass Sie Cluster oder einzelne Computer verwalten müssen. Der virtuelle Computer muss allerdings weiterhin verwaltet werden – beispielsweise durch Konfigurieren, Patchen und Verwalten der darauf ausgeführten Software.

Virtuelle Azure Stack-Computer können auf vielfältige Weise genutzt werden. Beispiel: 

- **Entwickeln und Testen**  
    Virtuelle Azure Stack-Computer stellen eine schnelle und einfache Möglichkeit zum Erstellen eines Computers mit speziellen Konfigurationen dar, die zum Programmieren und Testen einer Anwendung erforderlich sind.

- **Anwendungen in der Cloud**  
    Da die Nutzung Ihrer Anwendung Schwankungen unterliegen kann, ist es unter Umständen wirtschaftlich sinnvoll, sie auf einem virtuellen Computer in Azure Stack auszuführen. Sie bezahlen für zusätzliche virtuelle Computer, wenn Sie sie benötigen, und fahren sie andernfalls einfach herunter.

- **Erweitertes Datencenter**  
    Virtuelle Computer in einem virtuellen Azure Stack-Netzwerk lassen sich problemlos mit dem Netzwerk Ihrer Organisation oder mit Azure verbinden.

Die virtuellen Computer, die von Ihrer Anwendung genutzt werden, können zur Erfüllung Ihrer Anforderungen zentral oder horizontal hochskaliert werden.

## <a name="what-do-i-need-to-think-about-before-creating-a-vm"></a>Was muss ich vor dem Erstellen eines virtuellen Computers berücksichtigen?

Beim Einrichten einer Anwendungsinfrastruktur in Azure Stack müssen immer zahlreiche Designaspekte berücksichtigt werden. Stellen Sie daher vor der Erstellung Ihrer Infrastruktur Überlegungen zu folgenden VM-Aspekten an:

- Die Namen Ihrer Anwendungsressourcen
- Die Größe des virtuellen Computers
- Die maximal erstellbare Anzahl virtueller Computer
- Das Betriebssystem für den virtuellen Computer
- Die Konfiguration des virtuellen Computers nach dem Start
- Die zugehörigen Ressourcen, die der virtuelle Computer benötigt

### <a name="naming"></a>Benennung

Einem virtuellen Computer wird ein Name zugewiesen, und es wird im Betriebssystem ein Computername konfiguriert. Der Name eines virtuellen Computers kann bis zu 15 Zeichen lang sein.

Wenn Sie den Betriebssystem-Datenträger mithilfe von Azure Stack erstellen, sind der Computername und der Name des virtuellen Computers identisch. Wenn Sie ein eigenes Image hochladen und verwenden, das ein vorab konfiguriertes Betriebssystem enthält, und auf dessen Grundlage einen virtuellen Computer erstellen, können die Namen unterschiedlich sein. Beim Hochladen einer eigenen Imagedatei empfiehlt es sich, für den Computernamen im Betriebssystem und für den Namen des virtuellen Computers den gleichen Namen zu verwenden.

### <a name="vm-size"></a>Größe des virtuellen Computers

Die Größe des virtuellen Computers richtet sich nach der Workload, die Sie ausführen möchten. Von der gewählten Größe hängen Faktoren wie Rechenleistung, Arbeitsspeicher und Speicherplatz ab. Azure Stack bietet eine Vielzahl von Größen zur Unterstützung vieler Verwendungsarten.

### <a name="vm-limits"></a>Grenzwerte für virtuelle Computer

Für Ihr Abonnement gelten standardmäßig bestimmte Kontingentgrenzen, die die Bereitstellung einer hohen Anzahl virtueller Computer für Ihr Projekt beeinträchtigen können. Der derzeitige Grenzwert pro Abonnement liegt bei 20 VMs pro Region.

### <a name="operating-system-disks-and-images"></a>Betriebssystem-Datenträger und Images

Virtuelle Computer verwenden virtuelle Festplatten (VHDs), um ihr Betriebssystem (OS) und die Daten zu speichern. VHDs werden auch für die Images verwendet, die Sie auswählen können, um ein Betriebssystem zu installieren.
Azure Stack bietet einen Marketplace für verschiedene Versionen und Arten von Betriebssystemen. Marketplace-Images werden anhand von Herausgeber, Angebot, SKU und Version (üblicherweise die aktuelle Version) identifiziert.

Die folgende Tabelle enthält einige Methoden zur Ermittlung von Informationen zu einem Image:

|Methode|BESCHREIBUNG|
|---------|---------|
|Azure Stack-Portal|Die Werte werden automatisch angegeben, wenn Sie ein zu verwendendes Image auswählen.|
|Azure Stack PowerShell|`Get-AzureRMVMImagePublisher -Location "location"`<br>`Get-AzureRMVMImageOffer -Location "location" -Publisher "publisherName"`<br>`Get-AzureRMVMImageSku -Location "location" -Publisher "publisherName" -Offer "offerName"`|
|REST-APIs     |[List image publishers](https://docs.microsoft.com/rest/api/compute/platformimages/platformimages-list-publishers) (Imageherausgeber auflisten)<br>[List image offers](https://docs.microsoft.com/rest/api/compute/platformimages/platformimages-list-publisher-offers) (Imageangebote auflisten)<br>[List image SKUs](https://docs.microsoft.com/rest/api/compute/platformimages/platformimages-list-publisher-offer-skus) (Image-SKUs auflisten)|

Sie können ein eigenes Image hochladen und verwenden. In diesem Fall werden Herausgebername, Angebot und SKU nicht verwendet.

### <a name="extensions"></a>Erweiterungen

Mit VM-Erweiterungen können Sie den Funktionsumfang Ihres virtuellen Computers mittels Konfiguration nach der Bereitstellung sowie mittels automatisierter Aufgaben erweitern.
Mit Erweiterungen können folgende allgemeine Aufgaben umgesetzt werden:

- **Ausführen benutzerdefinierter Skripts**  
    Die Erweiterung für benutzerdefinierte Skripts unterstützt Sie beim Konfigurieren von Workloads auf dem virtuellen Computer, indem beim Bereitstellen des virtuellen Computers Ihr Skript ausgeführt wird.

- **Bereitstellen und Verwalten von Konfigurationen**  
    Die PowerShell-Erweiterung zum Konfigurieren des gewünschten Zustands (Desired State Configuration, DSC) unterstützt Sie beim Einrichten der Konfiguration des gewünschten Zustands auf einem virtuellen Computer, um Konfigurationen und Umgebungen zu verwalten.

- **Sammeln von Diagnosedaten**  
    Die Azure-Diagnoseerweiterung unterstützt Sie dabei, den virtuellen Computer für die Sammlung von Diagnosedaten zu konfigurieren, die zum Überwachen der Integrität Ihrer Anwendung verwendet werden können.

### <a name="related-resources"></a>Zugehörige Ressourcen

Die Ressourcen in der folgenden Tabelle werden vom virtuellen Computer verwendet und müssen beim Erstellen des virtuellen Computers vorhanden sein oder erstellt werden.


|Ressource|Erforderlich|BESCHREIBUNG|
|---------|---------|---------|
|Ressourcengruppe|Ja|Der virtuelle Computer muss sich in einer Ressourcengruppe befinden.|
|Speicherkonto|Ja|Der virtuelle Computer benötigt das Speicherkonto zum Speichern der virtuellen Festplatten.|
|Virtuelles Netzwerk|Ja|Der virtuelle Computer muss einem virtuellen Netzwerk angehören.|
|Öffentliche IP-Adresse|Nein |Für den Remotezugriff kann dem virtuellen Computer eine öffentliche IP-Adresse zugewiesen werden.|
|Netzwerkschnittstelle|Ja|Der virtuelle Computer benötigt die Netzwerkschnittstelle für die Kommunikation im Netzwerk.|
|Datenträger|Nein |Der virtuelle Computer kann Datenträger enthalten, um die Speicherkapazität zu erhöhen.|

## <a name="create-your-first-vm"></a>Erstellen Ihres ersten virtuellen Computers

Zum Erstellen eines virtuellen Computers stehen Ihnen mehrere Möglichkeiten zur Verfügung. Welche Möglichkeit Sie auswählen, richtet sich nach Ihrer Umgebung.
Die folgende Tabelle enthält hilfreiche Informationen zur Erstellung Ihres ersten virtuellen Computers.


|Methode|Artikel|
|---------|---------|
|Azure Stack-Portal|Erstellen eines virtuellen Windows-Computers im Azure Stack-Portal<br>[Erstellen eines virtuellen Linux-Computers im Azure Stack-Portal](azure-stack-quick-linux-portal.md)|
|Vorlagen|Azure Stack-Schnellstartvorlagen befinden sich hier:<br> [https://github.com/Azure/AzureStack-QuickStart-Templates](https://github.com/Azure/AzureStack-QuickStart-Templates)|
|PowerShell|[Erstellen eines virtuellen Windows-Computers mithilfe von PowerShell in Azure Stack](azure-stack-quick-create-vm-windows-powershell.md)<br>[Erstellen eines virtuellen Linux-Computers mithilfe von PowerShell in Azure Stack](azure-stack-quick-create-vm-linux-powershell.md)|
|Befehlszeilenschnittstelle (CLI)|[Erstellen eines virtuellen Windows-Computers mithilfe der Befehlszeilenschnittstelle in Azure Stack](azure-stack-quick-create-vm-windows-cli.md)<br>[Erstellen eines virtuellen Linux-Computers mithilfe der Befehlszeilenschnittstelle in Azure Stack](azure-stack-quick-create-vm-linux-cli.md)|

## <a name="manage-your-vm"></a>Verwalten Ihres virtuellen Computers

Sie können virtuelle Computer über ein browserbasiertes Portal, über Befehlszeilentools mit Skriptunterstützung oder direkt über APIs verwalten. Zu den typischen Verwaltungsaufgaben zählen unter anderem folgende:

- Abrufen von Informationen zu einem virtuellen Computer
- Herstellen einer Verbindung mit einem virtuellen Computer
- Verwalten der Verfügbarkeit
- Erstellen von Sicherungskopien

### <a name="get-information-about-your-vm"></a>Abrufen von Informationen zu Ihrem virtuellen Computer

Die folgende Tabelle zeigt einige der Methoden, mit denen Sie Informationen zu einem virtuellen Computer abrufen können.


|Methode|BESCHREIBUNG|
|---------|---------|
|Azure Stack-Portal|Klicken Sie im Hubmenü auf „Virtuelle Computer“, und wählen Sie dann in der Liste den gewünschten virtuellen Computer aus. Auf der Seite des virtuellen Computers finden Sie Übersichtsinformationen, Einstellungswerte und Überwachungsmetriken.|
|Azure PowerShell|Die Verwaltung von virtuellen Computern erfolgt in Azure und Azure Stack auf die gleiche Weise. Weitere Informationen zur Verwendung von PowerShell finden Sie im folgenden Azure-Thema:<br>[Erstellen und Verwalten von virtuellen Windows-Computern mit dem Azure PowerShell-Modul](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-manage-vm#understand-vm-sizes)|
|Client-SDKs|Die Verwendung von C# zur Verwaltung von virtuellen Computern erfolgt in Azure und Azure Stack auf die gleiche Weise. Weitere Informationen finden Sie im folgenden Azure-Thema:<br>[Erstellen und Verwalten von virtuellen Windows-Computern in Azure mithilfe von C#](https://docs.microsoft.com/azure/virtual-machines/windows/csharp)|

### <a name="connect-to-your-vm"></a>Herstellen einer Verbindung mit Ihrer VM

Sie können die Schaltfläche **Verbinden** im Azure Stack-Portal verwenden, um eine Verbindung mit Ihrem virtuellen Computer herzustellen.

## <a name="next-steps"></a>Nächste Schritte

- [Überlegungen zu virtuellen Computern in Azure Stack](azure-stack-vm-considerations.md)