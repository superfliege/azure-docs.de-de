---
title: Beispiele für die Azure CLI 2.0 | Microsoft-Dokumentation
description: Beispiele für die Azure CLI 2.0
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 69fc81d4d0569ee7a66fbda5ab500ef2ee15c694
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="azure-cli-20-samples-for-virtual-machine-scale-sets"></a>Azure CLI 2.0-Beispiele für VM-Skalierungsgruppen

Die folgende Tabelle enthält Links zu Bash-Skripts, die mithilfe der Azure CLI 2.0 erstellt wurden:

| | |
|---|---|
|**Erstellen und Verwalten einer Skalierungsgruppe**||
| [Erstellen einer VM-Skalierungsgruppe](scripts/cli-sample-create-simple-scale-set.md?toc=%2fcli%2fazure%2ftoc.json) | Erstellt eine VM-Skalierungsgruppe mit Minimalkonfiguration. |
| [Erstellen einer Skalierungsgruppe auf der Grundlage eines benutzerdefinierten VM-Images](scripts/cli-sample-create-scale-set-from-custom-image.md?toc=%2fcli%2fmodule%2ftoc.json) | Erstellt eine VM-Skalierungsgruppe mit einem benutzerdefinierten VM-Image. |
| [Installieren von Anwendungen in einer Skalierungsgruppe](scripts/cli-sample-install-apps.md?toc=%2fcli%2fmodule%2ftoc.json) | Verwendet die benutzerdefinierte Skripterweiterung von Azure, um eine einfache Webanwendung in einer Skalierungsgruppe zu installieren. |
|**Verwalten von Speicher**||
| [Erstellen von Datenträgern und Anfügen der Datenträger an eine Skalierungsgruppe](scripts/cli-sample-attach-disks.md?toc=%2fcli%2fmodule%2ftoc.json) | Erstellt eine VM-Skalierungsgruppe mit angefügten Datenträgern. |
|**Verwalten von Skalierung und Redundanz**||
| [Aktivieren der hostbasierten automatischen Skalierung](scripts/cli-sample-enable-autoscale.md?toc=%2fcli%2fazure%2ftoc.json) | Erstellt eine VM-Skalierungsgruppe, die für die automatische Skalierung auf der Grundlage der CPU-Auslastung konfiguriert ist. |
| [Erstellen einer Skalierungsgruppe mit einzelner Zone](scripts/cli-sample-single-availability-zone-scale-set.md?toc=%2fcli%2fazure%2ftoc.json) | Erstellt eine VM-Skalierungsgruppe mit einer einzelnen Verfügbarkeitszone. |
| [Erstellen einer zonenredundanten Skalierungsgruppe](scripts/cli-sample-zone-redundant-scale-set.md?toc=%2fcli%2fazure%2ftoc.json) | Erstellt eine übergreifende VM-Skalierungsgruppe für mehrere Verfügbarkeitszonen. |
| | |