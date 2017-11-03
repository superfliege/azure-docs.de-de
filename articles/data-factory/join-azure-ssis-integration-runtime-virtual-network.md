---
title: Beitreten einer Azure-SSIS-Integrationslaufzeit zu einem VNET | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie die Azure-SSIS-Integrationslaufzeit mit einem Azure Virtual Network verknüpfen."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2017
ms.author: spelluru
ms.openlocfilehash: 8a58f55bd627594145661e1c8d5c1da360cd1e30
ms.sourcegitcommit: c50171c9f28881ed3ac33100c2ea82a17bfedbff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2017
---
# <a name="join-an-azure-ssis-integration-runtime-to-a-virtual-network"></a>Beitreten einer Azure-SSIS-Integrationslaufzeit zu einem virtuellen Netzwerk
Sie müssen eine Azure-SSIS-Integrationslaufzeit mit einem Azure Virtual Network (VNet) verknüpfen, wenn eine der folgenden Bedingungen zutrifft: 

- Sie hosten die SSIS-Katalogdatenbank auf einer verwalteten SQL Server-Instanz (private Vorschau), die zu einem virtuellen Netzwerk gehört.
- Sie möchten aus SSIS-Paketen, die in einer Azure-SSIS-Integrationslaufzeit ausgeführt werden, eine Verbindung mit lokalen Datenspeichern herstellen.

 Mit Azure Data Factory Version 2 (Vorschau) können Sie eine Azure-SSIS-Integrationslaufzeit mit einem klassischen VNet verknüpfen. Derzeit wird ein Azure Resource Manager VNet noch nicht unterstützt. Sie können das Problem jedoch umgehen, wie im folgenden Abschnitt gezeigt. 

 > [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie die allgemein verfügbare Version 1 (GA) des Data Factory-Diensts verwenden, helfen Ihnen die Informationen unter [Dokumentation zur Version 1 von Data Factory](v1/data-factory-introduction.md) weiter.

Wenn SSIS-Pakete nur auf Public Cloud-Datenspeicher zugreifen, müssen Sie die Azure-SSIS-Integrationslaufzeit nicht mit einem VNet verknüpfen. Wenn SSIS-Pakete auf lokale Datenspeicher zugreifen, müssen Sie die Azure-SSIS-Integrationslaufzeit mit einem VNet verknüpfen, das mit dem lokalen Netzwerk verbunden ist. Wenn der SSIS-Katalog in Azure SQL-Datenbank gehostet wird, das nicht im VNet enthalten ist, müssen Sie entsprechende Ports öffnen. Wenn der SSIS-Katalog in einer verwalteten Azure SQL-Instanz gehostet wird, die sich in einem klassischen VNet befindet, können Sie die Azure-SSIS-Integrationslaufzeit mit demselben klassischen VNet oder einem anderen klassischen VNet verknüpfen, das über eine Klasse-zu-klassisch-VNet-Verbindung mit dem VNet verfügt, das die verwaltete Azure SQL-Instanz aufweist. Die folgenden Abschnitte enthalten hierzu weitere Informationen.  

## <a name="access-on-premises-data-stores"></a>Zugriff auf lokale Datenspeicher
Wenn Ihre SSIS-Pakete auf lokale Datenspeicher zugreifen, verknüpfen Sie die Azure-SSIS-Integrationslaufzeit mit einem VNet, das mit Ihrem lokalen Netzwerk verbunden ist. Folgende wichtige Punkte sind zu beachten: 

- Wenn kein vorhandenes VNet mit Ihrem lokalen Netzwerk verbunden ist, erstellen Sie zunächst ein [klassisches VNet](../virtual-network/virtual-networks-create-vnet-classic-pportal.md), mit dem Ihre Azure-SSIS-Integrationslaufzeit verknüpft werden kann. Konfigurieren Sie dann eine Standort-zu-Standort-[VPN-Gateway-Verbindung](../vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal.md)/[ExpressRoute](../expressroute/expressroute-howto-linkvnet-classic.md)-Verbindung von diesem VNet zu Ihrem lokalen Netzwerk.
- Wenn ein klassisches VNet an demselben Standort mit Ihrem lokalen Netzwerk verbunden ist, wie Ihre Azure-SSIS-Integrationslaufzeit, können Sie die Azure-SSIS-Integrationslaufzeit damit verknüpfen.
- Wenn ein klassisches VNet an einem anderen Standort als Ihre Azure-SSIS-Integrationslaufzeit mit Ihrem lokalen Netzwerk verbunden ist, können Sie zuerst ein [klassisches VNet](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) erstellen, mit dem die Azure-SSIS-Integrationslaufzeit verknüpft werden kann. Konfigurieren Sie dann eine [klassisch-zu-klassisch-VNet](../vpn-gateway/vpn-gateway-howto-vnet-vnet-portal-classic.md)-Verbindung.
- Wenn ein vorhandenes Azure Resource Manager VNet mit Ihrem lokalen Netzwerk verbunden ist, erstellen Sie zunächst ein [klassisches VNet](../virtual-network/virtual-networks-create-vnet-classic-pportal.md), mit dem Ihre Azure-SSIS-Integrationslaufzeit verknüpft werden kann. Konfigurieren Sie dann eine Verbindung zwischen [klassischem und Azure Resource Manager VNet](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

## <a name="domain-name-services-server"></a>Domain Name Service-Server 
Wenn Sie einen eigenen DNS-Server (Domain Name Service) in einem VNet verwenden müssen, dem Ihre Azure-SSIS-Integrationslaufzeit beigetreten ist, befolgen Sie diese Anleitung und [stellen Sie sicher, dass die Knoten Ihrer Azure-SSIS-Integrationslaufzeit im VNet Azure-Endpunkte auflösen können](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

## <a name="network-security-group"></a>Netzwerksicherheitsgruppen (NSG)
Wenn Sie die Netzwerksicherheitsgruppe (NSG) in einem VNet implementieren müssen, dem Ihre Azure-SSIS-Integrationslaufzeit beigetreten ist, können Sie den eingehenden/ausgehenden Datenverkehr über die folgenden Ports zulassen:

| Ports | Richtung | Transportprotokoll | Zweck | Eingehende Quelle/Ausgehendes Ziel |
| ---- | --------- | ------------------ | ------- | ----------------------------------- |
| 10100<br/>20100<br/>30100  | Eingehend | TCP | Azure-Dienste verwenden diese Ports für die Kommunikation mit den Knoten Ihrer Azure-SSIS-Integrationslaufzeit im VNet. | Internet | 
| 443 | Ausgehend | TCP | Die Knoten Ihrer Azure-SSIS-Integrationslaufzeit im VNet verwenden diesen Port für den Zugriff auf Azure-Dienste, z. B. Azure Storage, Event Hub usw. | INTERNET | 
| 1433<br/>11000-11999<br/>14000-14999  | Ausgehend | TCP | Die Knoten für Ihre Azure-SSIS-Integrationslaufzeit im VNet verwenden diese Ports für den Zugriff auf die SSISDB, die von Ihrem Azure SQL-Datenbankserver gehostet wird (gilt nicht für die SSISDB, die von der verwalteten Azure SQL-Instanz gehostet wird). | Internet | 

## <a name="script-to-configure-vnet"></a>Skript zum Konfigurieren von VNet 
Sie können das gesteuerte PowerShell-Skript aus [diesem Artikel](create-azure-ssis-integration-runtime.md) verwenden, um eine Azure-SSIS-Integrationslaufzeit in einem VNet bereitzustellen. Das Skript konfiguriert automatisch VNet-Berechtigungen und -Einstellungen, damit Sie die Azure-SSIS-Integrationslaufzeit mit dem VNet verknüpfen können.  


## <a name="use-portal-to-configure-vnet"></a>Portal zum Konfigurieren von VNet
Das Ausführen des Skripts ist die einfachste Möglichkeit zum Konfigurieren von VNet. Wenn Sie keinen Zugriff auf dieses VNet haben, tritt ein Fehler bei der automatischen Konfiguration auf. Der Besitzers des VNet bzw. Sie kann/können versuchen, diese in den folgenden Schritten manuell zu konfigurieren:

### <a name="find-the-resource-id-for-your-azure-vnet"></a>Suchen Sie die Ressourcen-ID für Ihr Azure VNet.
 
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie auf **Weitere Dienste**. Filtern Sie nach **Virtuelle Netzwerke (klassisch)**, und wählen Sie die Option aus.
3. Filtern Sie nach Ihrem **virtuellen Netzwerk**, und wählen Sie es in der Liste aus. 
4. Wählen Sie auf der Seite „Virtuelles Netzwerk (klassisch)“ die Option **Eigenschaften** aus. 

    ![Ressourcen-ID für klassisches VNet](media/join-azure-ssis-integration-runtime-virtual-network/classic-vnet-resource-id.png)
5. Klicken Sie für die **RESSOURCEN-ID** auf die Schaltfläche „Kopieren“, um die Ressourcen-ID für das klassische Netzwerk in die Zwischenablage zu kopieren. Speichern Sie die ID aus der Zwischenablage in OneNote oder in einer Datei.
6. Klicken Sie im linken Menü auf **Subnetze**, und stellen Sie sicher, dass die Anzahl der **verfügbaren Adressen** größer ist als die Anzahl der Knoten in Ihrer Azure-SSIS-Integrationslaufzeit.

    ![Anzahl der verfügbaren Adressen im VNet](media/join-azure-ssis-integration-runtime-virtual-network/number-of-available-addresses.png)
7. Verknüpfen Sie **MicrosoftAzureBatch** mit der Rolle **Mitwirkender von klassischen virtuellen Computern** für das VNet. 
    1. Klicken Sie im linken Menü auf „Zugriffssteuerung (IAM)“, und klicken Sie dann auf der Symbolleiste auf **Hinzufügen**.
    
        ![Zugriffssteuerung -> Hinzufügen](media/join-azure-ssis-integration-runtime-virtual-network/access-control-add.png) 
    2. Wählen Sie auf der Seite **Berechtigungen hinzufügen** die Option **Mitwirkender für virtuelle Computer** für **Rolle** aus. Geben Sie **MicrosoftAzureBatch** in das Textfeld **Auswählen** ein, und wählen Sie dann **MicrosoftAzureBatch** aus der Liste der Suchergebnisse aus. 
    
        ![Berechtigungen hinzufügen – Suchen](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-to-vm-contributor.png)
    3. Klicken Sie auf „Speichern“, um die Einstellungen zu speichern und die Seite zu schließen.
    
        ![Speichern der Zugriffseinstellungen](media/join-azure-ssis-integration-runtime-virtual-network/save-access-settings.png)
    4. Vergewissern Sie sich, dass **MicrosoftAzureBatch** in der Liste der Mitwirkenden angezeigt wird.
    
        ![Bestätigen des AzureBatch-Zugriffs](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-in-list.png)
5. Überprüfen Sie, ob dieser Azure Batch-Anbieter im Azure-Abonnement registriert ist, das über das VNet verfügt, oder registrieren Sie den Azure Batch-Anbieter. Wenn in Ihrem Abonnement bereits ein Azure Batch-Konto enthalten ist, wird Ihr Abonnement für Azure Batch registriert.
    1. Klicken Sie im Azure-Portal im linken Menü auf **Abonnements**. 
    2. Wählen Sie Ihr **Abonnement**aus. 
    3. Klicken Sie auf der linken Seite auf **Ressourcenanbieter**, und bestätigen Sie, dass `Microsoft.Batch` ein registrierter Anbieter ist. 
    
        ![Bestätigung-Batch-Registrierung](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

    Wenn `Microsoft.Batch` nicht in der Liste angezeigt wird, [erstellen Sie ein leeres Azure Batch-Konto](../batch/batch-account-create-portal.md) in Ihrem Abonnement, um es zu registrieren. Sie können es später wieder löschen. 
         


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Azure-SSIS-Laufzeit finden Sie in den folgenden Themen: 

- [Azure-SSIS-Integrationslaufzeit](concepts-integration-runtime.md#azure-ssis-integration-runtime): Dieser Artikel enthält konzeptionelle Informationen zu Integrationslaufzeiten, die im Allgemeinen die Azure-SSIS-Integrationslaufzeit einschließen. 
- [Tutorial: Bereitstellen von SSIS-Paketen in Azure](tutorial-deploy-ssis-packages-azure.md): Dieser Artikel enthält schrittweise Anweisungen zum Erstellen einer Azure-SSIS-Integrationslaufzeit und verwendet eine Azure SQL-Datenbank zum Hosten des SSIS-Katalogs. 
- [Gewusst wie: Erstellen einer Azure-SSIS-Integrationslaufzeit](create-azure-ssis-integration-runtime.md): In diesem Artikel wird das Tutorial vertieft und er enthält Anweisungen zur Verwendung der verwalteten Azure SQL-Instanz (private Vorschau) und zum Verknüpfen der Integrationslaufzeit mit einem VNet. 
- [Überwachen einer Azure-SSIS-Integrationslaufzeit](monitor-integration-runtime.md#azure-ssis-integration-runtime): In diesem Artikel wird das Abrufen von Informationen zu einer Azure-SSIS-Integrationslaufzeit veranschaulicht, und er enthält Beschreibungen der Status in den zurückgegebenen Informationen. 
- [Verwalten einer Azure-SSIS-Integrationslaufzeit](manage-azure-ssis-integration-runtime.md): In diesem Artikel wird beschrieben, wie Sie eine Azure-SSIS-Integrationslaufzeit beenden, starten oder entfernen. Es wird zudem gezeigt, wie Sie Ihre Azure-SSIS-Integrationslaufzeit horizontal hochskalieren, indem Sie der Integrationslaufzeit weitere Knoten hinzufügen. 
