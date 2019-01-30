---
title: Red Hat Enterprise Linux-Images in Azure | Microsoft-Dokumentation
description: Erfahren Sie etwas über Red Hat Enterprise Linux-Images in Microsoft Azure.
services: virtual-machines-linux
documentationcenter: ''
author: BorisB2015
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 01/18/2019
ms.author: borisb
ms.openlocfilehash: fd73863dacd7914e23199ba92211ea67e68f7d0d
ms.sourcegitcommit: c31a2dd686ea1b0824e7e695157adbc219d9074f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2019
ms.locfileid: "54402148"
---
# <a name="red-hat-enterprise-linux-images-in-azure"></a>Red Hat Enterprise Linux-Images in Azure
Dieser Artikel beschreibt die verfügbaren Red Hat Enterprise Linux-Images (RHEL) im Azure Marketplace sowie Richtlinien für die Benennung und Aufbewahrung.

Informationen zu Red Hat-Supportrichtlinien für alle RHEL-Versionen finden Sie auf der Seite [Red Hat Enterprise Linux Life Cycle (Red Hat Enterprise Linux-Lebenszyklus)](https://access.redhat.com/support/policy/updates/errata).

>[!Important]
> Für RHEL-Images, die derzeit auf dem Azure-Marketplace verfügbar sind, wird entweder das Lizenzierungsmodell Bring-Your-Own-Subscription (BYOS) oder die nutzungsbasierte Zahlung (Pay-As-You-Go, PAYG) unterstützt. Der [Azure-Vorteil bei Hybridnutzung](../windows/hybrid-use-benefit-licensing.md) und das dynamische Wechseln zwischen BYOS und PAYG wird nicht unterstützt. Für einen Wechsel des Lizenzierungsmodus ist eine erneute Bereitstellung der VM über das entsprechende Image erforderlich.

>[!Note]
> Erstellen Sie ein Supportticket bei Microsoft, wenn bei Ihnen im Azure Marketplace-Katalog ein Problem mit RHEL-Images auftritt.

## <a name="images-available-in-the-ui"></a>Auf der Benutzeroberfläche verfügbare Images
Wenn Sie im Marketplace nach „Red Hat“ suchen oder eine Ressource auf der Benutzeroberfläche des Azure-Portals erstellen, werden eine Teilmenge der verfügbaren RHEL-Images und verwandte Red Hat-Produkte angezeigt. Sie können immer den vollständigen Satz der verfügbaren VM-Images mithilfe der Azure-Befehlszeilenschnittstelle, von PowerShell oder der API abrufen.

Führen Sie den folgenden Befehl aus, um den vollständigen Satz von verfügbaren Red Hat-Images in Azure anzuzeigen.

```azurecli-interactive
az vm image list --publisher RedHat --all
```

## <a name="naming-convention"></a>Benennungskonvention
VM-Images in Azure sind nach Herausgeber, Angebot, SKU und Version organisiert. Die Kombination aus Herausgeber:Angebot:SKU:Version ist der Image-URN, durch den das zu verwendende Image eindeutig identifiziert wird.

Beispielsweise verweist `RedHat:RHEL:7-RAW:7.6.2018103108` auf ein RHEL 7.6-Image mit Rohpartitionierung, das am 31. Oktober 2018 erstellt wurde.

Ein Beispiel zum Erstellen einer RHEL 7.6-VM ist weiter unten dargestellt.
```azurecli-interactive
az vm create --name RhelVM --resource-group TestRG --image RedHat:RHEL:7-RAW:7.6.2018103108 --no-wait
```

### <a name="the-latest-moniker"></a>Der Moniker „latest“
Die Azure-REST-API ermöglicht die Verwendung des Monikers „latest“ für die Version anstelle der genauen Version. Mit „latest“ wird das neueste verfügbare Image für die angegebene Kombination aus Herausgeber, Angebot und SKU bereitgestellt.

Beispielsweise verweist `RedHat:RHEL:7-RAW:latest` auf das neueste verfügbare Image der RHEL 7 Familie mit Rohpartitionierung.

```azurecli-interactive
az vm create --name RhelVM --resource-group TestRG --image RedHat:RHEL:7-RAW:latest --no-wait
```

>[!NOTE]
> Im Allgemeinen folgt der Vergleich von Versionen zum Ermitteln der neuesten Version den Regeln der [CompareTo](https://msdn.microsoft.com/library/a5ts8tb6.aspx)-Methode.

### <a name="current-naming-convention"></a>Aktuelle Namenskonvention
Alle derzeit veröffentlichten RHEL-Images verwenden das Modell mit nutzungsbasierter Bezahlung und sind mit der [Red Hat-Updateinfrastruktur (RHUI) in Azure](https://aka.ms/rhui-update) verbunden. Aufgrund einer Designeinschränkung in der RHUI wurde eine neue Namenskonvention für Images der RHEL 7-Familie übernommen. Die Benennung der RHEL 6-Familie wurde zu diesem Zeitpunkt noch nicht geändert.

Die Einschränkung besteht darin, dass beim Ausführen eines nicht selektiven `yum update` für einen mit der RHUI verbundenen virtuellen Computer die RHEL-Version auf die neueste in der aktuellen Familie aktualisiert wird. Weitere Informationen finden Sie unter [diesem Link](https://aka.ms/rhui-udate). Dies kann zu Problemen führen, wenn ein bereitgestelltes RHEL 7.2-Image nach einem Update zu RHEL 7.6 wird. Sie können immer noch eine Bereitstellung über ein älteres Image ausführen, wie in den obigen Beispielen veranschaulicht wird, indem Sie die erforderliche Version explizit angeben. Wenn die erforderliche Version bei der Bereitstellung eines neuen RHEL 7-Images nicht angegeben wurde, wird das aktuelle Image bereitgestellt.

>[!NOTE]
> In dem Satz von RHEL for SAP-Images ist die RHEL-Version festgeschrieben. Daher enthält ihre Namenskonvention eine bestimmte Version in der SKU.

>[!NOTE]
> Der Satz von RHEL 6-Images wurde nicht in die neue Namenskonvention umgewandelt.

Die folgenden Angebote sind SKUs, die derzeit zur allgemeinen Verwendung verfügbar sind:
Angebot| SKU | Partitionierung | Bereitstellung | Notizen
:----|:----|:-------------|:-------------|:-----
RHEL | 7-RAW | RAW | Linux-Agent | RHEL 7-Familie von Images
| | 7-LVM | LVM | Linux-Agent | RHEL 7-Familie von Images
| | 7-RAW-CI | RAW-CI | cloud-init | RHEL 7-Familie von Images
| | 6.7 | RAW | Linux-Agent | RHEL 6.7-Images, alte Namenskonvention
| | 6,8 | RAW | Linux-Agent | Wie oben für RHEL 6.8 angegeben
| | 6.9 | RAW | Linux-Agent | Wie oben für RHEL 6.9 angegeben
| | 6.10 | RAW | Linux-Agent | Wie oben für RHEL 6.10 angegeben
| | 7.2 | RAW | Linux-Agent | Wie oben für RHEL 7.2 angegeben
| | 7.3 | RAW | Linux-Agent | Wie oben für RHEL 7.3 angegeben
| | 7.4 | RAW | Linux-Agent | Wie oben für RHEL 7.4 angegeben
| | 7,5 | RAW | Linux-Agent | Wie oben für RHEL 7.5 angegeben
RHEL-SAP | 7.4 | LVM | Linux-Agent | RHEL 7.4 for SAP HANA und Business Applications
| | 7,5 | LVM | Linux-Agent | RHEL 7.5 for SAP HANA und Business Applications
RHEL-SAP-HANA | 6.7 | RAW | Linux-Agent | RHEL 6.7 for SAP HANA
| | 7.2 | LVM | Linux-Agent | RHEL 7.2 for SAP HANA
| | 7.3 | LVM | Linux-Agent | RHEL 7.3 for SAP HANA
RHEL-SAP-APPS | 6,8 | RAW | Linux-Agent | RHEL 6.8 for SAP Business Applications
| | 7.3 | LVM | Linux-Agent | RHEL 7.3 for SAP Business Applications

### <a name="old-naming-convention"></a>Alte Namenskonvention
In der RHEL 7-Familie von Images und der RHEL 6-Familie von Images wurden bis zur oben erläuterten Änderung der Namenskonvention spezifische Versionen in den SKUs verwendet.

In der vollständigen Liste der Images sind numerische SKUs aufgeführt. Microsoft und Red Hat erstellen neue numerische SKUs, wenn eine neue Nebenversion veröffentlicht wird.

### <a name="other-available-offers-and-skus"></a>Weitere verfügbare Angebote und SKUs
Die vollständige Liste der verfügbaren Angebote und SKUs enthält eventuell weitere Images über die in der obigen Tabelle aufgeführten hinaus, z.B. `RedHat:rhel-ocp-marketplace:rhel74:7.4.1`. Diese Angebote können zur Unterstützung bestimmter Marketplace-Lösungen verwendet werden, oder sie können für Vorschauversionen und zu Testzwecken veröffentlicht werden. Sie können jederzeit ohne vorherige Warnung geändert oder entfernt werden. Verwenden Sie sie nicht, sofern ihr Vorhandensein nicht öffentlich von Microsoft oder Red Hat dokumentiert wurde.

## <a name="publishing-policy"></a>Veröffentlichungsrichtlinie
Microsoft und Red Hat aktualisieren Images, wenn neue Nebenversionen veröffentlicht werden, nach Bedarf für bestimmte CVEs oder gelegentlich bei Konfigurationsänderungen/-updates. Wir bemühen uns, die aktualisierten Abbilder möglichst bald bereitzustellen – binnen drei Werktagen nach der Veröffentlichung bzw. Verfügbarkeit eines CVE-Updates.

Wir aktualisieren nur die aktuelle Nebenversion in einer bestimmten Imagefamilie. Wenn eine neuere Nebenversion veröffentlicht wird, beenden wir die Aktualisierung älterer Nebenversionen. Beispielsweise werden mit der Veröffentlichung von RHEL 7.6 keine RHEL 7.5-Images mehr aktualisiert.

>[!NOTE]
> Aktive Azure-VMs, die mit RHEL-Images mit nutzungsbasierter Bezahlung bereitgestellt wurden, sind mit der Azure-RHUI verbunden und erhalten Updates und Korrekturen, sobald sie von Red Hat veröffentlicht und in die Azure-RHUI repliziert wurden (normalerweise in weniger als 24 Stunden nach der offiziellen Veröffentlichung durch Red Hat). Für diese VMs ist kein neues veröffentlichte Image erforderlich, um die Updates zu erhalten, und Kunden haben vollständige Kontrolle über den Zeitpunkt des Updates.

## <a name="image-retention-policy"></a>Aufbewahrungsrichtlinie für Images
Nach unserer aktuellen Richtlinie werden alle veröffentlichten Images beibehalten. Wir behalten uns vor, Images zu entfernen, die bekanntermaßen Probleme jeglicher Art verursachen. Beispielsweise können Images mit falschen Konfigurationen aufgrund späterer Plattform- oder Komponentenaktualisierungen entfernt werden. Falls ein Image entfernt wird, gilt die aktuelle Marketplace-Richtlinie, nach der bis zu 30 Tage vor dem Entfernen des Images Benachrichtigungen bereitgestellt werden sollten.

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie [hier](https://aka.ms/rhui-update) mehr über die Azure Red Hat-Updateinfrastruktur.
* Informationen zu Red Hat-Supportrichtlinien für alle RHEL-Versionen finden Sie auf der Seite [Red Hat Enterprise Linux Life Cycle (Red Hat Enterprise Linux-Lebenszyklus)](https://access.redhat.com/support/policy/updates/errata).