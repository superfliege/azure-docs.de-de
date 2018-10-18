---
title: Zusätzliche Netzwerkanforderungen für SAP HANA in Azure (große Instanzen) | Microsoft-Dokumentation
description: Zusätzliche Netzwerkanforderungen für SAP HANA in Azure (große Instanzen)
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8ee89553846309bd96f3cad3648e337488cf07d3
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44167599"
---
# <a name="additional-network-requirements"></a>Zusätzliche Netzwerkanforderungen

Manchmal müssen im Rahmen der Bereitstellung von HANA (große Instanzen) zusätzliche Netzwerkanforderungen erfüllt werden. Dieser Artikel veranschaulicht die folgenden Netzwerkanforderungen:
- [Hinzufügen weiterer IP-Adressen oder Subnetze](#adding-more-ip-addresses-or-subnets)
- [Hinzufügen von virtuellen Netzwerken](#adding-vnets)
- [Erhöhen der Bandbreite für die ExpressRoute-Verbindung](#increasing-expressroute-circuit-bandwidth)
- [Hinzufügen einer zusätzlichen ExpressRoute-Verbindung](#adding-an-additional-expressroute-circuit)
- [Löschen eines Subnetzes](#deleting-a-subnet)
- [Löschen eines virtuellen Netzwerks](#deleting-a-vnet)
- [Löschen einer ExpressRoute-Verbindung](#deleting-an-expressroute-circuit)


## <a name="adding-more-ip-addresses-or-subnets"></a>Hinzufügen weiterer IP-Adressen oder Subnetze

Verwenden Sie zum Hinzufügen weiterer IP-Adressen und Subnetze das Azure-Portal, PowerShell oder die CLI.

In diesem Fall empfiehlt es sich, den neuen IP-Adressbereich dem VNet-Adressraum als neuen Bereich hinzuzufügen, anstatt einen neuen aggregierten Bereich zu erstellen. In beiden Fällen müssen Sie diese Änderung an Microsoft übermitteln, damit Verbindungen aus diesem neuen IP-Adressbereich mit den SAP HANA (große Instanzen)-Einheiten in Ihrem Client zugelassen werden. Sie können eine Azure-Supportanfrage öffnen, um den neuen VNet-Adressraum hinzufügen zu lassen. Führen Sie nach Erhalt einer Bestätigung die nächsten Schritte aus.

Wenn Sie im Azure-Portal ein zusätzliches Subnetz erstellen möchten, lesen Sie den Artikel [Erstellen eines virtuellen Netzwerks im Azure-Portal](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-virtual-network). Wie diese Schritte mit PowerShell erfolgen, finden Sie unter [Erstellen eines virtuellen Netzwerks mithilfe von PowerShell](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-virtual-network).

## <a name="adding-vnets"></a>Hinzufügen von VNets

Nach dem anfänglichen Herstellen von Verbindungen mit einem oder mehreren Azure VNets möchten Sie ggf. weitere hinzufügen, die auf SAP HANA in Azure (große Instanzen) zugreifen. Senden Sie zunächst eine Azure-Supportanfrage, in der Sie sowohl die speziellen Informationen zur Bestimmung der jeweiligen Azure-Bereitstellung als auch die IP-Adressbereiche des Azure VNet-Adressraums angeben. Das SAP HANA in Azure-Dienstverwaltungsteam stellt Ihnen die erforderlichen Informationen bereit, die Sie zum Verbinden der zusätzlichen VNets und von ExpressRoute benötigen. Für jedes VNet benötigen Sie einen eindeutigen Autorisierungsschlüssel, um eine Verbindung mit der ExpressRoute-Leitung für SAP HANA (große Instanzen) herzustellen.

Schritte zum Hinzufügen eines neuen Azure VNet:

1. Informationen zum ersten Schritt in diesem Prozess finden Sie im Abschnitt _Erstellen eines Azure VNet_.
2. Informationen zum zweiten Schritt in diesem Prozess finden Sie im Abschnitt _Erstellen eines Gatewaysubnetzes_.
3. Öffnen Sie eine Azure-Supportanfrage mit Informationen zum neuen VNet, und fordern Sie einen neuen Autorisierungsschlüssel zum Verknüpfen Ihrer zusätzlichen VNets mit der ExpressRoute-Verbindung von HANA (große Instanzen) an.
4. Nach Erhalt einer Autorisierungsbestätigung verwenden Sie die von Microsoft bereitgestellten Autorisierungsinformationen zum Ausführen des dritten Schritts in diesem Prozess. Siehe dazu _Verknüpfen von VNets_.

## <a name="increasing-expressroute-circuit-bandwidth"></a>Erhöhen der Bandbreite für die ExpressRoute-Verbindung

Besprechen Sie sich mit dem SAP HANA in Azure-Dienstverwaltungsteam. Falls Ihnen geraten wird, die Bandbreite der ExpressRoute-Verbindung für SAP HANA in Azure (große Instanzen) zu erhöhen, erstellen Sie eine Azure-Supportanforderung. (Sie können eine Erhöhung der Bandbreite einer einzelnen Verbindung um maximal 10 Gbit/s anfordern.) Sie erhalten eine Benachrichtigung, sobald der Vorgang abgeschlossen ist. Es sind keine weiteren Aktionen erforderlich, um diese höhere Geschwindigkeit in Azure zu aktivieren.

## <a name="adding-an-additional-expressroute-circuit"></a>Hinzufügen einer zusätzlichen ExpressRoute-Verbindung

Besprechen Sie sich mit dem SAP HANA in Azure-Dienstverwaltungsteam. Falls Ihnen gesagt wird, dass eine weitere ExpressRoute-Verbindung nötig sei, erstellen Sie eine Supportanforderung zum Erstellen einer weiteren ExpressRoute-Verbindung. Rufen Sie außerdem die entsprechenden Autorisierungsinformationen zum Herstellen einer Verbindung damit auf. Der Adressraum, der in den VNets verwendet wird, muss vor dem Übermitteln der Anforderung definiert werden, damit das SAP HANA in Azure-Dienstverwaltungsteam die Autorisierung erteilen kann.

Nachdem die neue Verbindung erstellt wurde und die SAP HANA in Azure-Dienstverwaltungskonfiguration abgeschlossen ist, erhalten Sie die Benachrichtigung mit den Informationen, die Sie zum Fortsetzen des Vorgangs benötigen. Führen Sie die zuvor beschriebenen Schritte zum Erstellen des neuen VNet und zum Herstellen einer Verknüpfung mit dieser zusätzlichen Verbindung durch. Sie können keine Azure-VNets mit dieser zusätzlichen Leitung verbinden, wenn die VNets bereits mit einer anderen ExpressRoute-Leitung von SAP HANA in Azure (große Instanzen) in derselben Azure-Region verbunden sind.

## <a name="deleting-a-subnet"></a>Löschen eines Subnetzes

Zum Entfernen eines VNet-Subnetzes kann das Azure-Portal, PowerShell oder die CLI verwendet werden. Für den Fall, dass Ihr Azure-VNet-IP-Adressbereich/Azure VNet-Adressraum ein aggregierter Bereich ist, besteht keine Notwendigkeit zur Nachsorge mit Microsoft. Die einzige Ausnahme ist, dass das VNet weiterhin BGP-Routenadressraum weitergibt, der das gelöschte Subnetz umfasst. Wenn Sie den Azure-VNet-IP-Adressbereich/Azure-VNet-Adressraum als mehrere IP-Adressbereiche definiert haben, von denen einer dem gelöschten Subnetz zugewiesen war, sollten Sie diesen aus Ihrem VNet-Adressraum löschen und dann die SAP HANA in Azure-Dienstverwaltung informieren, den Bereich aus den Bereichen zu entfernen, mit denen SAP HANA in Azure (große Instanzen) kommunizieren darf.

Wenn Sie ein Subnetz löschen möchten, finden Sie unter [Löschen eines Subnetzes](../../../virtual-network/virtual-network-manage-subnet.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#delete-a-subnet) weitere Informationen zum Erstellen von Subnetzen.

## <a name="deleting-a-vnet"></a>Löschen eines VNet

Informationen zum Löschen eines virtuellen Netzwerks finden Sie unter [Löschen eines virtuellen Netzwerks](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#delete-a-virtual-network). Das SAP HANA in Azure-Dienstverwaltungsteam entfernt die vorhandenen Autorisierungen für die ExpressRoute-Leitung von SAP HANA in Azure (große Instanzen) und den Azure-VNet-IP-Adressbereich/Azure-VNet-Adressraum für die Kommunikation mit SAP HANA (große Instanzen).

Nachdem das VNet entfernt wurde, beantragen Sie in einer Azure-Supportanfrage das Entfernen der IP-Adressbereiche.

Um sicherzustellen, dass alles entfernt wurde, löschen Sie die folgenden Elemente:

- Die ExpressRoute-Verbindung, das VNet-Gateway, die öffentliche IP-Adresse des VNet-Gateways und das VNet

## <a name="deleting-an-expressroute-circuit"></a>Löschen einer ExpressRoute-Verbindung

Um eine weitere ExpressRoute-Verbindung für SAP HANA in Azure (große Instanzen) zu entfernen, bitten Sie das SAP HANA in Azure-Dienstverwaltungsteam in einer Azure-Supportanfrage, die Verbindung zu löschen. Im Azure-Abonnement können Sie das VNet je nach Anforderung löschen oder beibehalten. Allerdings müssen Sie die Verbindung zwischen der SAP HANA (große Instanzen)-ExpressRoute-Leitung und dem verknüpften VNet-Gateway löschen.

Wenn Sie auch ein VNet entfernen möchten, befolgen Sie die Anleitung zum Löschen eines VNet im vorherigen Abschnitt.

**Nächste Schritte**

- Lesen Sie [Installieren und Konfigurieren von SAP HANA in Azure (große Instanzen)](hana-installation.md).
