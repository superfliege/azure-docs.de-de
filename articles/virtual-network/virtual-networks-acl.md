---
title: Was ist eine Azure-Netzwerk-Zugriffssteuerungsliste?
description: "Erfahren Sie mehr über Zugriffssteuerungslisten in Azure"
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 83d66c84-8f6b-4388-8767-cd2de3e72d76
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.openlocfilehash: 9a0c85367968c9b38104012d75b1f3975be82cc1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="what-is-an-endpoint-access-control-list"></a>Was ist eine Endpunkt-Zugriffssteuerungsliste?

> [!IMPORTANT]
> Azure bietet zwei verschiedene [Bereitstellungsmodelle](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) zum Erstellen und Arbeiten mit Ressourcen: Ressourcen-Manager und Classic. Dieser Artikel umfasst, mit dem klassischen Bereitstellungsmodell. Microsoft empfiehlt, dass die meisten neue Bereitstellungen, die Ressourcen-Manager-Bereitstellungsmodell verwenden. 

Eine Endpunkt-Zugriffssteuerungsliste (ACL) ist eine sicherheitserweiterung, die für die Azure-Bereitstellung zur Verfügung. Eine ACL bietet die Möglichkeit, selektiv zulassen oder Verweigern von Datenverkehr für einen Endpunkt des virtuellen Computers. Diese Paketfilterungsfunktion stellt eine zusätzliche Sicherheitsebene bereit. Sie können die Netzwerk-ACLs nur für Endpunkte angeben. Sie können nicht angeben, dass eine ACL für ein virtuelles Netzwerk oder ein bestimmtes Subnetz in einem virtuellen Netzwerk enthalten. Es wird mit netzwerksicherheitsgruppen (NSGs) empfohlen, anstelle von ACLs, wann immer möglich. Weitere Informationen zu Netzwerksicherheitsgruppen finden Sie unter [Übersicht über die Gruppe von Netzwerk-Sicherheit](virtual-networks-nsg.md)

ACLs können mithilfe von PowerShell oder Azure-Portal konfiguriert werden. Zum Konfigurieren einer Netzwerks-ACL mit PowerShell finden Sie unter [Managing Access Control lists für Endpunkte, die mithilfe von PowerShell](virtual-networks-acl-powershell.md). Zum Konfigurieren einer Netzwerks-ACL mit dem Azure-Portal finden Sie unter [zum Einrichten von Endpunkten für einen virtuellen Computer](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Verwenden die Netzwerk-ACLs, können Sie Folgendes tun:

* Erlauben Sie oder verweigern Sie des eingehenden Datenverkehrs basierend auf Remotesubnetz IPv4-Adressbereich auf einen eingabeendpunkt des virtuellen Computers.
* Schwarze IP-Adressen
* Erstellen Sie mehrerer Regeln pro Endpunkt des virtuellen Computers
* Verwendung Regel bestellen, um sicherzustellen, dass den richtigen Satz von Regeln gelten für einen vorhandenen virtuellen Computer-Endpunkt (niedrigsten zur höchsten)
* Geben Sie eine ACL für einen bestimmten Remotesubnetz IPv4-Adresse ein.

Finden Sie unter der [Azure schränkt](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits) Artikel für die ACL-Grenzwerte.

## <a name="how-acls-work"></a>Funktionsweise von ACLs
Eine ACL ist ein Objekt, das eine Liste von Regeln enthält. Wenn Sie eine ACL erstellen und auf einen Endpunkt des virtuellen Computers wird angewendet, findet Paketfilter für den Hostknoten der VM. Dies bedeutet, dass der Datenverkehr von remote-IP-Adressen von der Hostknoten für übereinstimmende ACL-Regeln anstelle von auf Ihren virtuellen Computer gefiltert wird. Dadurch wird verhindert, dass Ihre VM wertvollen CPU-Zyklen für die paketfilterung verbringt.

Wenn eine virtuelle Maschine erstellt wird, wird eine Standard-ACL vorhanden, die alle eingehenden Datenverkehr blockiert eingefügt. Allerdings wird ein Endpunkt für (Port 3389), klicken Sie dann die Standardeinstellung erstellt ACL für alle eingehenden Datenverkehr für diesen Endpunkt geändert wird. Wird an diesen Endpunkt eingehender Datenverkehr von einem beliebigen Remotesubnetz zugelassen, und keine Firewall bereitgestellt ist erforderlich. Allen anderen Ports werden für eingehenden Datenverkehr blockiert, es sei denn, Sie Endpunkte für diese Ports erstellt werden. Ausgehender Datenverkehr wird standardmäßig zulässig.

**Standard-ACL-Beispieltabelle**

| **Regel Nr.** | **Remotesubnetz** | **Endpunkt** | **Zulassen/Verweigern** |
| --- | --- | --- | --- |
| 100 |0.0.0.0/0 |3389 |Zulassen |

## <a name="permit-and-deny"></a>Zulassen und verweigern
Sie können selektiv zulassen oder verweigern den Netzwerkdatenverkehr für einen eingabeendpunkt des virtuellen Computers, durch das Erstellen von Regeln, die "zulassen" oder "Verweigern". Es ist wichtig zu beachten, dass standardmäßig, wenn ein Endpunkt erstellt wird, der gesamte Datenverkehr an den Endpunkt zugelassen wird. Aus diesem Grund ist es wichtig zu verstehen, wie Sie Zulassungs-/Verweigerungsregeln erstellen und Ablegen in der richtigen Reihenfolge der Rangfolge gegebenenfalls eine präzise Kontrolle über den Netzwerkdatenverkehr, die Sie auswählen, um zuzulassen, um den Endpunkt des virtuellen Computers zu erreichen.

Zu berücksichtigende Punkte:

1. **Keine ACL –** standardmäßig Wenn ein Endpunkt erstellt wird, wir erlauben alle für den Endpunkt.
2. **Zulassen –** , wenn Sie eine oder mehrere "zulassen" Bereiche hinzufügen, werden Sie für alle anderen Bereiche standardmäßig verweigert. Nur Pakete aus dem zulässigen IP-Adressbereich ist für die Kommunikation mit dem Endpunkt des virtuellen Computers möglich.
3. **Deny -** Wenn Sie eine oder mehrere "verweigerte" Bereiche hinzufügen, werden Sie standardmäßig für alle anderen Bereiche des Datenverkehrs zulassen.
4. **Kombination aus zulassen und Verweigern -** können Sie eine Kombination aus "zulassen" und "Verweigern", wenn herausstellen ein bestimmter IP-Adressbereich zugelassen oder verweigert werden soll.

## <a name="rules-and-rule-precedence"></a>Regeln und Rangfolge
Netzwerk-ACLs können für bestimmte virtuelle Maschine Endpunkte eingerichtet werden. Beispielsweise können Sie eine Netzwerk-ACL für einen RDP-Endpunkt erstellt, die auf einem virtuellen Computer, welche Sperren Sie den Zugriff für bestimmte IP-Adressen, angeben. In der folgenden Tabelle zeigt eine Möglichkeit zum Zugriff auf öffentliche virtuelle IP-Adressen (VIPs) eines bestimmten Bereichs zu Zugriff RDP-Endpunkt zu gewähren. Alle anderen remote-IPs wird verweigert. Wir führen Sie eine *niedrigste hat Vorrang vor* Regelreihenfolge.

### <a name="multiple-rules"></a>Mehrere Regeln
Im folgenden Beispiel, wenn Sie den Zugriff auf den RDP-Endpunkt nur aus zwei öffentlichen IPv4-Adressbereiche (65.0.0.0/8 und 159.0.0.0/8) zulassen möchten dies erreichen Sie durch Angabe von zwei *zulassen* Regeln. Da RDP standardmäßig für eine virtuelle Maschine erstellt wird, sollten Sie in diesem Fall Zugriff auf den RDP-Port basierend auf einem Remotesubnetz sperren. Das folgende Beispiel zeigt eine Möglichkeit zum Zugriff auf öffentliche virtuelle IP-Adressen (VIPs) eines bestimmten Bereichs zu Zugriff RDP-Endpunkt zu gewähren. Alle anderen remote-IPs wird verweigert. Dies funktioniert, da die Netzwerk-ACLs kann für einen bestimmten virtuellen Maschine-Endpunkt eingerichtet werden und Zugriff standardmäßig verweigert wird.

**Beispiel – mehrere Regeln**

| **Regel Nr.** | **Remotesubnetz** | **Endpunkt** | **Zulassen/Verweigern** |
| --- | --- | --- | --- |
| 100 |65.0.0.0/8 |3389 |Zulassen |
| 200 |159.0.0.0/8 |3389 |Zulassen |

### <a name="rule-order"></a>Regelreihenfolge
Da für einen Endpunkt mehrere Regeln angegeben werden können, muss eine Möglichkeit zum Organisieren von Regeln, um zu bestimmen, welche Regel Vorrang hat vorhanden sein. Die Regelreihenfolge gibt die Rangfolge an. Netzwerk-ACLs gilt eine *niedrigste hat Vorrang vor* Regelreihenfolge. Im folgenden Beispiel wird der Endpunkt an Port 80 selektiv Zugriff auf bestimmte IP-Adressbereiche gewährt. Um dies zu konfigurieren, haben wir eine Verweigerungsregel (Regel \# 100) für Adressen im Bereich 175.1.0.1/24. Eine zweite Regel wird dann mit dem Rang 200 angegeben, die Zugriff auf alle anderen Adressen unter 175.0.0.0/8 zulässt.

**Beispiel – regelrangfolge**

| **Regel Nr.** | **Remotesubnetz** | **Endpunkt** | **Zulassen/Verweigern** |
| --- | --- | --- | --- |
| 100 |175.1.0.1/24 |80 |Deny |
| 200 |175.0.0.0/8 |80 |Zulassen |

## <a name="network-acls-and-load-balanced-sets"></a>Netzwerk-ACLs und Lastenausgleich
Netzwerk-ACLs können für einen Endpunkt mit Lastenausgleich Satz angegeben werden. Wenn eine ACL für eine Gruppe mit Lastenausgleich angegeben wird, wird die Netzwerk-ACL auf alle virtuellen Computer in dieser Gruppe mit Lastenausgleich angewendet. Z. B. wenn ein Lastenausgleich Satz mit "Port 80" erstellt wird und die Gruppe mit Lastenausgleich enthält 3 VMs, die Netzwerk-ACL erstellt, auf dem Endpunkt "Port 80" einer VM automatisch auf anderen virtuellen Computer angewendet werden.

![Netzwerk-ACLs und Lastenausgleich](./media/virtual-networks-acl/IC674733.png)

## <a name="next-steps"></a>Nächste Schritte
[Verwalten von Zugriffssteuerungslisten für Endpunkte, die mithilfe von PowerShell](virtual-networks-acl-powershell.md)

