---
title: Erstellen einer VM-Skalierungsgruppe mit dem Azure-Portal | Microsoft Docs
description: "Stellen Sie Skalierungsgruppen über das Azure-Portal bereit."
keywords: "Skalierungsgruppen für virtuelle Computer"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: madhana
editor: tysonn
tags: azure-resource-manager
ms.assetid: 9c1583f0-bcc7-4b51-9d64-84da76de1fda
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: negat
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fc52368a1a14ad094e7ab9180b90a9aa4ccdb6d8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-create-a-virtual-machine-scale-set-with-the-azure-portal"></a>Erstellen einer VM-Skalierungsgruppe mit dem Azure-Portal
In diesem Tutorial erfahren Sie, wie Sie in wenigen Minuten eine VM-Skalierungsgruppe über das Azure-Portal erstellen. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="choose-the-vm-image-from-the-marketplace"></a>Auswählen des VM-Image aus dem Marketplace
Über das Portal können Sie problemlos eine Skalierungsgruppe mit CentOS, CoreOS, Debian, Ubuntu Server, anderen Linux-Images und Windows Server-Images bereitstellen.

Navigieren Sie zunächst in einem Browser zum [Azure-Portal](https://portal.azure.com) . Klicken Sie auf **Neu**, suchen Sie nach **Skalierungsgruppe**, und wählen Sie dann den Eintrag **VM-Skalierungsgruppe** aus:

![azure vm skalierungsgruppe portal suchen](./media/virtual-machine-scale-sets-portal-create/portal-search.png)

## <a name="create-the-scale-set"></a>Erstellen der Skalierungsgruppe
Nun können Sie die Standardeinstellungen verwenden und die Skalierungsgruppe schnell erstellen.

* Geben Sie einen Namen für die Skalierungsgruppe ein.  
Dieser Name dient als Grundlage für den FQDN des Load Balancers vor der Skalierungsgruppe und muss daher im gesamten Azure-System eindeutig sein.

* Wählen Sie den gewünschten Betriebssystemtyp aus.

* Geben Sie den gewünschten Benutzernamen ein, und wählen Sie den bevorzugten Authentifizierungstyp aus.  
Falls Sie ein Kennwort auswählen, muss es mindestens 12 Zeichen lang sein und drei der folgenden vier Komplexitätsanforderungen erfüllen: ein Kleinbuchstabe, ein Großbuchstabe, eine Zahl und ein Sonderzeichen. Weitere Informationen finden Sie unter den [Anforderungen für Benutzernamen und Kennwörter](../virtual-machines/windows/faq.md#what-are-the-username-requirements-when-creating-a-vm). Wenn Sie **SSH public key** auswählen, fügen Sie nur Ihren öffentlichen Schlüssel ein, *NICHT* Ihren privaten Schlüssel:

* Wählen Sie für **Skalierung auf über 100 Instanzen aktivieren** eine der Optionen **Ja** oder **Nein** aus.  
Im Fall von „Ja“ kann die Skalierungsgruppe mehrere Platzierungsgruppen umfassen. Weitere Informationen finden Sie in [dieser Dokumentation](./virtual-machine-scale-sets-placement-groups.md).

* Achten Sie darauf, eine passende **Instanzgröße** auszuwählen.  
Weitere Informationen zu Größen von virtuellen Computern finden Sie unter [Windows-VM-Größe](..\virtual-machines\windows\sizes.md) und [Linux-VM-Größen](..\virtual-machines\linux\sizes.md).

* Geben Sie den gewünschten Namen und Speicherort für Ihre Ressourcengruppe ein.  
Wenn Ihre Region und Ihre **Instanzgröße** Verfügbarkeitszonen unterstützen, ist das Feld **Verfügbarkeitszonen** aktiviert. Weitere Informationen zu Verfügbarkeitszonen finden Sie in diesem [Übersichtsartikel](../availability-zones/az-overview.md).

* Geben Sie die gewünschte Domänennamenbezeichnung (die Basis des FQDN für den Lastenausgleich vor der Skalierungsgruppe) ein.  
(Diese fungiert als Grundlage für den FQDN des Load Balancers vor der Skalierungsgruppe.) Die Bezeichnung muss im gesamten Azure-System eindeutig sein.

* Wählen Sie das gewünschte Image für den Betriebssystem-Datenträger, die Anzahl von Instanzen und die Computergröße aus.

* Wählen Sie den gewünschten Datenträgertyp aus: verwaltet oder nicht verwaltet.  
Weitere Informationen finden Sie in [dieser Dokumentation](./virtual-machine-scale-sets-managed-disks.md). Wenn Sie ausgewählt haben, dass die Skalierungsgruppe mehrere Platzierungsgruppen umfasst, ist diese Option nicht verfügbar, da für Skalierungsgruppen, die mehrere Platzierungsgruppen umfassen, ein verwalteter Datenträger erforderlich ist.

* Aktivieren oder deaktivieren Sie die automatische Skalierung, und konfigurieren Sie sie gegebenenfalls.

![azure vm skalierungsgruppe portal eingabeaufforderung erstellen](./media/virtual-machine-scale-sets-portal-create/portal-create.png)

## <a name="connect-to-a-vm-in-the-scale-set"></a>Herstellen einer Verbindung mit einem virtuellen Computer in der Skalierungsgruppe
Wenn Sie ausgewählt haben, dass sich die Skalierungsgruppe auf eine einzelne Platzierungsgruppe beschränkt, wird die Skalierungsgruppe mit NAT-Regeln bereitgestellt, die so konfiguriert sind, dass Sie einfach und schnell eine Verbindung mit der Skalierungsgruppe herstellen können.(Andernfalls müssen Sie wahrscheinlich im selben Netzwerk, in dem sich die Skalierungsgruppe befindet, eine Jumpbox erstellen, um eine Verbindung mit den virtuellen Computern in der Skalierungsgruppe herzustellen.) Um die virtuellen Computer anzuzeigen, navigieren Sie zur Registerkarte `Inbound NAT Rules` des Load Balancers für die Skalierungsgruppe:

![azure vm skalierungsgruppe portal nat-regeln](./media/virtual-machine-scale-sets-portal-create/portal-nat-rules.png)

Mit diesen NAT-Regeln können Sie eine Verbindung mit einem beliebigen virtuellen Computer in der Skalierungsgruppe herstellen. Wenn also etwa für eine Windows-Skalierungsgruppe eine NAT-Regel für den eingehenden Port 50000 vorhanden ist, können Sie mit diesem Computer unter `<load-balancer-ip-address>:50000`eine RDP-Verbindung herstellen. Bei einer Linux-Skalierungsgruppe müsste für die Verbindungsherstellung der Befehl `ssh -p 50000 <username>@<load-balancer-ip-address>`verwendet werden.

## <a name="next-steps"></a>Nächste Schritte
In [dieser Dokumentation](virtual-machine-scale-sets-cli-quick-create.md)erfahren Sie, wie Sie Skalierungsgruppen mithilfe der Befehlszeilenschnittstelle bereitstellen.

In [dieser Dokumentation](virtual-machine-scale-sets-windows-create.md)erfahren Sie, wie Sie Skalierungsgruppen mithilfe von PowerShell bereitstellen.

In [dieser Dokumentation](virtual-machine-scale-sets-vs-create.md)erfahren Sie, wie Sie Skalierungsgruppen mithilfe von Visual Studio bereitstellen.

Eine allgemeine Dokumentation finden Sie in der [Dokumentationsübersicht für Skalierungsgruppen](virtual-machine-scale-sets-overview.md).

Allgemeine Informationen finden Sie auf der [Hauptseite für Skalierungsgruppen](https://azure.microsoft.com/services/virtual-machine-scale-sets/).

