---
title: "Beispiele für Richtlinienvorlagen | Microsoft-Dokumentation"
description: "JSON-Beispiele für Azure Policy"
services: azure-policy
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
tags: 
ms.assetid: 
ms.service: azure-policy
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: 
ms.date: 11/13/2017
ms.author: banders
ms.custom: mvc
ms.openlocfilehash: 4042c3606155d1d37947afccafd64652c9659894
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2017
---
# <a name="templates-for-azure-policy"></a>Vorlagen für Azure Policy

In der folgenden Tabelle sind Links zu JSON-Vorlagen für Azure Policy enthalten. Diese Beispiele befinden sich im [Azure Policy-Beispielrepository](https://github.com/Azure/azure-policy).

| | |
|---|---|
|**Compute**||
| [Genehmigte VM-Images](scripts/allowed-custom-images.md) | Verlangt, dass nur genehmigte benutzerdefinierte Images in Ihrer Umgebung bereitgestellt werden. Sie geben ein Array von genehmigten Image-IDs an. |
| [Erstellen einer VM mithilfe eines verwalteten Datenträgers](scripts/create-vm-managed-disk.md) | Überwachung wird vorgenommen, wenn ein virtueller Computer erstellt wird, der keine verwalteten Datenträger verwendet.|
| [Überwachung, wenn keine Erweiterung vorhanden ist](scripts/audit-ext-not-exist.md) | Überwacht, wenn eine Erweiterung nicht mit einem virtuellen Computer bereitgestellt wird. Sie geben den Herausgeber und den Typ der Erweiterung an, um zu überprüfen, ob diese bereitgestellt wurde. |
| [Zulassen eines benutzerdefinierten VM-Images aus einer Ressourcengruppe](scripts/allow-custom-vm-image.md) |  Verlangt, dass benutzerdefinierte Images aus einer genehmigten Ressourcengruppe stammen. Sie bestimmen den Namen der genehmigten Ressourcengruppe. |
| [Verweigern des Hybridnutzungsvorteils](scripts/deny-hybrid-use.md) | Untersagt die Verwendung des Hybridnutzungsvorteils für Azure. Nutzen Sie diese Funktion, wenn Sie die Verwendung von lokalen Lizenzen verweigern möchten. |
| [Nicht zulässige VM-Erweiterungen](scripts/not-allowed-vm-ext.md) | Untersagt die Verwendung bestimmter Erweiterungen. Sie geben ein Array an, das die unzulässigen Erweiterungstypen enthält. |
| [Zulassen von nur einem bestimmten VM-Plattformimage](scripts/allow-certain-vm-image.md) | Erfordert, dass virtuelle Computer eine bestimmte Version von UbuntuServer verwenden. |
| [Erstellen einer VM mithilfe eines verwalteten Datenträgers](scripts/use-managed-disk-vm.md) | Erfordert, dass virtuelle Computer verwaltete Datenträger verwenden.|
|**Überwachung**||
| [Überwachen der Diagnoseeinstellung](scripts/audit-diag-setting.md) | Überwacht, ob Diagnoseeinstellungen für angegebene Ressourcentypen nicht aktiviert sind. Sie geben ein Array von Ressourcentypen an, um zu überprüfen, ob die Diagnoseeinstellungen aktiviert sind. |
|**Namens- und Textkonventionen**||
| [Zulassen mehrerer Namensmuster](scripts/allow-multiple-name-patterns.md) | Lassen Sie die Verwendung von einem der vielen Namensmuster für Ressourcen zu. |
| [Ähnliches Muster erfordern](scripts/enforce-like-pattern.md) | Stellen Sie sicher, dass Ressourcennamen eine ähnliche Bedingung für ein Muster erfüllen. |
| [Übereinstimmungsmuster erfordern](scripts/enforce-match-pattern.md) | Stellen Sie sicher, dass Ressourcennamen dem Benennungsmuster entsprechen. |
| [Tagübereinstimmungsmuster erfordern](scripts/enforce-tag-match-pattern.md) | Stellen Sie sicher, dass ein Tagwert einem Textmuster entspricht. |
|**Netzwerk**||
| [Zulässige Application Gateway-SKUs](scripts/allowed-app-gate-sku.md) | Verlangt, dass Application Gateways eine zulässige SKU verwenden. Sie geben ein Array von zulässigen SKUs an. |
| [Überwachen, wenn Network Watcher nicht für die Region aktiviert ist](scripts/net-watch-not-enabled.md) | Überwacht, wenn Network Watcher nicht für eine bestimme Region aktiviert ist. Sie geben den Namen der Region an, um zu überprüfen, ob Network Watcher aktiviert ist. |
| [Netzwerksicherheitsgruppe X auf jeder Netzwerkschnittstelle](scripts/nsg-on-nic.md) | Verlangt, dass eine bestimmte Netzwerksicherheitsgruppe für jede virtuelle Netzwerkschnittstelle verwendet wird. Sie geben die ID der zu verwendenden Netzwerksicherheitsgruppe an. |
| [Netzwerksicherheitsgruppe X auf jedem Subnetz](scripts/nsg-on-subnet.md) | Verlangt, dass eine bestimmte Netzwerksicherheitsgruppe für jedes virtuelle Subnetz verwendet wird. Sie geben die ID der zu verwendenden Netzwerksicherheitsgruppe an. |
| [Zulässige ExpressRoute-Bandbreiten](scripts/allowed-er-band.md) | Verlangt, dass ExpressRoutes verschiedene angegebene Bandbreiten verwenden. Sie geben ein Array von SKUs an, das für ExpressRoute angegeben werden kann. |
| [Zulässige Peeringstandorte für ExpressRoute](scripts/allowed-peering-er.md) | Verlangt, dass ExpressRoute angegebene Peeringstandorte verwendet. Sie geben ein Array von zulässigen Peeringstandorten an. |
| [Zulässige ExpressRoute-SKUs](scripts/allowed-er-skus.md) | Verlangt, dass ExpressRoutes eine zulässige SKU verwenden. Sie geben ein Array von zulässigen SKUs an. |
| [Zulässige Load Balancer-SKUs](scripts/allowed-lb-skus.md) | Verlangt, dass Load Balancers eine zulässige SKU verwenden. Sie geben ein Array von zulässigen SKUs an. |
| [Kein Netzwerkpeering in ein ER-Netzwerk](scripts/no-peering-er-net.md) | Verhindert, dass ein Netzwerkpeering einem Netzwerk in einer angegebenen Ressourcengruppe zugeordnet wird. Verwenden Sie diese Richtlinie, um eine Verbindung zu zentraler verwalteter Netzwerkinfrastruktur zu vermeiden. Sie geben den Namen der Ressourcengruppe an, um eine Zuordnung zu verhindern. |
| [Keine benutzerdefinierten Routentabellen](scripts/no-user-def-route-table.md)  |Verhindert, dass virtuelle Netzwerke mit einer benutzerdefinierten Routentabelle bereitgestellt werden. |
| [Zulässige SKUs für Gateways für virtuelle Netzwerke](scripts/no-user-def-route-table.md) | Verlangt, dass Gateways für virtuelle Netzwerke eine zulässige SKU und einen Gatewaytyp festlegen. Sie geben ein Array von zulässigen SKUs und ein Array von zulässigen Gatewaytypen an. |
| [Verwenden eines zulässigen Subnetzes für VM-Netzwerkschnittstellen](scripts/use-approved-subnet-vm-nics.md) | Verlangt, dass Netzwerkschnittstellen ein zulässiges Subnetz verwenden. Sie geben die ID des zulässigen Subnetzes an. |
| [Verwenden eines zulässigen virtuellen Netzwerks für VM-Netzwerkschnittstellen](scripts/use-approved-vnet-vm-nics.md) | Verlangt, dass Netzwerkschnittstellen ein zulässiges virtuelles Netzwerk verwenden. Sie geben die ID des zulässigen virtuellen Netzwerks an. |
|**Tags**||
| [Richtlinieninitiative zur Abrechnung von Tags](scripts/billing-tags-policy-init.md) | Verlangt angegebene Tagwerte für die Kostenstelle und den Produktnamen. Verwendet integrierte Richtlinien, um erforderliche Tags anzuwenden und zu erzwingen. Sie geben die erforderlichen Werte für die Tags an.  |
| [Erzwingen eines Tags und dessen Werts für Ressourcengruppen](scripts/enforce-tag-rg.md) | Verlangt einen Tag und einen Wert für eine Ressourcengruppe. Sie geben den erforderlichen Tagnamen und -wert an.  |
|**SQL**||
| [Überwachen von Einstellungen der Überwachung auf SQL-Datenbank-Ebene](scripts/audit-sql-db-audit-setting.md) | Überwacht SQL-Datenbank-Überwachungseinstellungen, wenn diese Einstellungen nicht mit einer angegebenen Einstellung übereinstimmen. Sie geben einen Wert an, der angibt, ob Überwachungseinstellungen aktiviert oder deaktiviert sein sollen.  |
| [Überwachen des Status der transparenten Datenverschlüsselung](scripts/audit-trans-data-enc-status.md) | Überwacht die transparente Datenverschlüsselung von SQL-Datenbank, falls diese nicht aktiviert ist.  |
| [Überwachen der Einstellung für die Bedrohungserkennung auf Datenbankebene](scripts/audit-db-threat-det-setting.md) | Überwacht Richtlinien für Sicherheitswarnungen für SQL-Datenbank, wenn diese Richtlinien nicht auf den angegebenen Status festgelegt sind. Sie geben einen Wert an, der angibt, ob die Bedrohungserkennung aktiviert oder deaktiviert ist.  |
| [Überwachen von Einstellungen der Überwachung auf SQL-Server-Ebene](scripts/audit-sql-ser-leve-audit-setting.md) | Überwacht SQL-Server-Überwachungseinstellungen, wenn diese Einstellungen nicht mit einer angegebenen Einstellung übereinstimmen. Sie geben einen Wert an, der angibt, ob Überwachungseinstellungen aktiviert oder deaktiviert sein sollen. |
| [Überwachen von Einstellungen für die Bedrohungserkennung auf Serverebene](scripts/audit-sql-ser-threat-det-setting.md) | Überwacht Richtlinien für Sicherheitswarnungen für SQL-Datenbank, wenn diese Richtlinien nicht auf den angegebenen Status festgelegt sind. Sie geben einen Wert an, der angibt, ob die Bedrohungserkennung aktiviert oder deaktiviert ist.  |
| [Überwachen bei nicht vorhandenem Azure Active Directory-Administrator](scripts/audit-no-aad-admin.md) | Lassen Sie eine Überwachung zu, wenn dem SQL-Server kein Azure Active Directory-Administrator zugewiesen ist. |
| [Zulässige SQL-Datenbank-SKUs](scripts/allowed-sql-db-skus.md) | Verlangt, dass SQL-Datenbanken eine genehmigte SKU verwenden. Sie geben ein Array von zulässigen SKU-IDs oder von zulässigen SKU-Namen an. |
|**Speicher**||
| [Zulässige SKUs für Speicherkonten und virtuelle Computer](scripts/allowed-skus-storage.md) | Erfordert, dass Speicherkonten und virtuelle Computer genehmigte SKUs verwenden. Verwendet integrierte Richtlinien, um die Verwendung von genehmigten SKUs zu gewährleisten. Sie geben ein Array von genehmigten SKUs für virtuelle Computer und ein Array von genehmigten SKUs für Speicherkonten an. |
| [Gewährleisten eines HTTPS-Datenverkehrs ausschließlich für Speicherkonten](scripts/ensure-https-stor-acct.md) | Verlangt, dass Speicherkonten HTTPS-Datenverkehr verwenden.  |
| [Verweigern des kalten Zugriffstierings für Speicherkonten](scripts/deny-cool-access-tiering.md) | Untersagt die Verwendung des kalten Zugriffstierings für Blob-Speicherkonten.  |
| [Gewährleisten der Verschlüsselung von Speicherdateien](scripts/ensure-store-file-enc.md) | Verlangt, dass die Dateiverschlüsselung für Speicherkonten aktiviert ist.  |
|**Integrierte Richtlinie**||
| [Zulässige Speicherorte](scripts/allowed-locs.md) | Verlangt, dass alle Ressourcen für die zulässigen Speicherorte bereitgestellt werden. Sie geben ein Array von zulässigen Speicherorten an.  |
| [Zulässige Ressourcentypen](scripts/allowed-res-types.md) | Stellt sicher, dass nur genehmigte Ressourcentypen bereitgestellt werden. Sie geben ein Array von Ressourcentypen an, die erlaubt sind.  |
| [Zulässige Speicherkonten-SKUs](scripts/allowed-stor-acct-skus.md) | Verlangt, dass Speicherkonten eine genehmigte SKU verwenden. Sie geben ein Array von zulässigen SKUs an. |
| [Tag und Standardwert anwenden](scripts/apply-tag-def-val.md) | Fügt einen angegebenen Tagnamen und -wert an, wenn dieses Tag nicht bereitgestellt wird. Sie geben den anzuwendenden Tagnamen und -wert an.  |
| [Erzwingen eines Tags und dessen zugehörigen Werts](scripts/enforce-tag-val.md) | Verlangt einen angegebenen Tagnamen und-wert. Sie geben den zu erzwingenden Tagnamen und -wert an.  |
| [Nicht zulässige Ressourcentypen](scripts/not-allowed-res-type.md) | Untersagt die Bereitstellung der angegebenen Ressourcentypen. Sie geben ein Array der zu blockierenden Ressourcentypen an.  |
| [Verlangen der SQL Server-Version 12.0](scripts/req-sql-12.md) | Verlangen der SQL Server-Version 12.0.  |
| [Verlangen der Speicherkontenverschlüsselung](scripts/req-store-acct-enc.md) | Erfordert das Verwenden der Blobverschlüsselung für das Speicherkonto.  |
