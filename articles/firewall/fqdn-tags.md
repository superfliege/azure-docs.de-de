---
title: Übersicht über die FQDN-Tags für Azure Firewall
description: Erfahren Sie mehr über die FQDN-Tags in Azure Firewall
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 11/1/2018
ms.author: victorh
ms.openlocfilehash: 897ea3856516b5429ffb770164f863d71e7ae0dd
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50419009"
---
# <a name="fqdn-tags-overview"></a>Übersicht über FQDN-Tags

Ein FQDN-Tag stellt eine Gruppe von vollqualifizierten Domänennamen (FQDNs) dar, die bekannten Microsoft-Diensten zugeordnet sind. Sie können ein FQDN-Tag in den Anwendungsregeln verwenden, um den erforderlichen ausgehenden Netzwerkdatenverkehr über die Firewall zuzulassen.

Um beispielsweise den Netzwerkdatenverkehr von Windows Update manuell über Ihre Firewall zuzulassen, müssen Sie mehrere Anwendungsregeln gemäß der Microsoft-Dokumentation erstellen. Mithilfe von FQDN-Tags können Sie eine Anwendungsregel erstellen, das Tag **Windows Updates** einfügen, damit der Netzwerkdatenverkehr zu Microsoft Windows Update-Endpunkten durch Ihre Firewall fließen kann.

Sie können keine eigenen FQDN-Tags erstellen und auch nicht festlegen, welche FQDNs in einem Tag enthalten sind. Microsoft verwaltet die FQDNs, die ein FQDN-Tag umfasst und aktualisiert das Tag, wenn sich die FQDNs ändern. 

<!--- screenshot of application rule with a FQDN tag.-->

Die folgende Tabelle enthält die aktuellen FQDN-Tags, die Sie verwenden können. Microsoft pflegt diese Tags, und fügt regelmäßig weitere Tags hinzu.

|FQDN-Tag  |BESCHREIBUNG  |
|---------|---------|
|Windows-Update     |Erlaubt den ausgehenden Zugriff auf Microsoft Update, wie in [Konfigurieren einer Firewall für Softwareupdates](https://technet.microsoft.com/library/bb693717.aspx) beschrieben.|
|Windows-Diagnose|Erlaubt den ausgehenden Zugriff auf alle [Windows-Diagnoseendpunkte](https://docs.microsoft.com/windows/privacy/configure-windows-diagnostic-data-in-your-organization#endpoints).|
|Microsoft Active Protection Service (MAPS)|Erlaubt den ausgehenden Zugriff auf [MAPS](https://cloudblogs.microsoft.com/enterprisemobility/2016/05/31/important-changes-to-microsoft-active-protection-service-maps-endpoint/).|
|App Service-Umgebung (ASE)|Erlaubt den ausgehenden Zugriff auf den Datenverkehr der ASE-Plattform. Dieses Tag umfasst keine kundenspezifischen Speicher und SQL-Endpunkte, die von ASE erstellt wurden. Diese müssen über [Dienstendpunkte](../virtual-network/tutorial-restrict-network-access-to-resources.md) aktiviert oder manuell hinzugefügt werden.<br><br>Weitere Informationen zur Integration von Azure Firewall in ASE finden Sie unter [Sperren einer App Service-Umgebung](../app-service/environment/firewall-integration.md#configuring-azure-firewall-with-your-ase).|
|Azure Backup|Erlaubt den ausgehenden Zugriff auf die Azure Backup-Dienste.

> [!NOTE]
> Beim Auswählen des FQDN-Tags in einer Anwendungsregel muss das Feld „protocol:port“ auf **https** festgelegt werden.

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Bereitstellen einer Azure Firewall finden Sie im [Tutorial: Bereitstellen und Konfigurieren von Azure Firewall über das Azure-Portal](tutorial-firewall-deploy-portal.md).