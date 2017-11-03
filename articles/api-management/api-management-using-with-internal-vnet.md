---
title: Verwenden von Azure API Management mit internen virtuellen Netzwerken | Microsoft-Dokumentation
description: "Hier erfahren Sie, wie Azure API Management für ein internes virtuelles Netzwerk eingerichtet und konfiguriert wird."
services: api-management
documentationcenter: 
author: vladvino
manager: kjoshi
editor: 
ms.assetid: dac28ccf-2550-45a5-89cf-192d87369bc3
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2017
ms.author: apimpm
ms.openlocfilehash: 2a496059d1959a6c9e762e70dfbeff9bf961c4d4
ms.sourcegitcommit: 1131386137462a8a959abb0f8822d1b329a4e474
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2017
---
# <a name="using-azure-api-management-service-with-an-internal-virtual-network"></a>Verwenden von Azure API Management mit einem internen virtuellen Netzwerk
Mit virtuellen Azure-Netzwerken kann API Management APIs verwalten, auf die nicht über das Internet zugegriffen werden kann. Für die Verbindungsherstellung stehen verschiedene VPN-Technologien zur Verfügung. API Management kann in einem virtuellen Netzwerk in zwei Hauptmodi bereitgestellt werden:
* Extern
* Intern


Wenn API Management im Modus für ein internes virtuelles Netzwerk bereitgestellt wird, sind alle Dienstendpunkte (Gateway, Entwicklerportal, Herausgeberportal, direkte Verwaltung und Git) nur in einem virtuellen Netzwerk sichtbar, für das Sie den Zugriff steuern. Keiner der Dienstendpunkte ist auf dem öffentlichen DNS-Server registriert.

Mit API Management im internen Modus sind folgende Szenarien möglich:
* APIs, die in Ihrem privaten Datencenter gehostet werden, können externen Dritten über Site-to-Site- oder Azure ExpressRoute-VPN-Verbindungen sicher zugänglich gemacht werden.
* Das Verfügbarmachen von cloudbasierten und lokalen APIs über ein gemeinsames Gateway ermöglicht Hybrid Cloud-Szenarien.
* APIs, die an mehreren geografischen Standorten gehostet werden, können mit einem einzelnen Gatewayendpunkt verwaltet werden. 


## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen der in diesem Artikel beschriebenen Schritte benötigen Sie Folgendes:

+ **Ein aktives Azure-Abonnement.**

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ **Eine API Management-Instanz.** Weitere Informationen finden Sie unter [Erstellen einer neuen Azure API Management-Dienstinstanz](get-started-create-service-instance.md).

## <a name="enable-vpn"> </a>Einrichten von API Management in einem internen virtuellen Netzwerk
Der API Management-Dienst in einem internen virtuellen Netzwerk wird hinter einem internen Lastenausgleich (Internal Load Balancer, ILB) gehostet.

### <a name="enable-a-virtual-network-connection-using-the-azure-portal"></a>Aktivieren einer Verbindung mit einem virtuellen Netzwerk über das Azure-Portal

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) zu Ihrer Azure API Management-Instanz.
2. Klicken Sie auf **Benutzerdefinierte Domänen und SSL**.
3. Konfigurieren Sie die API Management-Instanz für die Bereitstellung innerhalb eines virtuellen Netzwerks.

    ![Menü zum Einrichten einer Azure API Management-Instanz in einem internen virtuellen Netzwerk][api-management-using-internal-vnet-menu]

4. Wählen Sie **Speichern** aus.

Nach erfolgreicher Bereitstellung sollte die interne virtuelle IP-Adresse des Diensts auf dem Dashboard angezeigt werden.

![API Management-Dashboard mit konfiguriertem internem virtuellen Netzwerk][api-management-internal-vnet-dashboard]

### <a name="enable-a-virtual-network-connection-by-using-powershell-cmdlets"></a>Aktivieren einer Verbindung mit einem virtuellen Netzwerk mithilfe von PowerShell-Cmdlets
Verbindungen mit virtuellen Netzwerken können auch mithilfe von PowerShell-Cmdlets aktiviert werden.

* Erstellen eines API Management-Diensts in einem virtuellen Netzwerk: Verwenden Sie das Cmdlet [New-AzureRmApiManagement](/powershell/module/azurerm.apimanagement/new-azurermapimanagement), um einen Azure API Management-Dienst in einem virtuellen Netzwerk zu erstellen und so zu konfigurieren, dass als Typ ein internes virtuelles Netzwerk verwendet wird.

* Bereitstellen eines vorhandenen API Management-Diensts in einem virtuellen Netzwerk: Verwenden Sie das Cmdlet [Update-AzureRmApiManagementDeployment](/powershell/module/azurerm.apimanagement/update-azurermapimanagementdeployment), um einen vorhandenen API Management-Dienst in ein virtuelles Netzwerk zu verschieben und so zu konfigurieren, dass als Typ ein internes virtuelles Netzwerk verwendet wird.

## <a name="apim-dns-configuration"></a>DNS-Konfiguration
Wenn sich API Management im Modus eines externen virtuellen Netzwerks befindet, wird DNS von Azure verwaltet. Beim Modus eines internen virtuellen Netzwerks müssen Sie das Routing selbst verwalten.

> [!NOTE]
> Der API Management-Dienst lauscht nicht auf von IP-Adressen stammende Anforderungen. Er reagiert nur auf Anforderungen für den Hostnamen, der für seine Dienstendpunkte konfiguriert ist. Zu diesen Endpunkten zählen Gateway, Entwicklerportal, Herausgeberportal, Endpunkt für die direkte Verwaltung und Git.

### <a name="access-on-default-host-names"></a>Zugreifen über Standardhostnamen
Wenn Sie beispielsweise einen API Management-Dienst namens „Contoso“ erstellen, werden standardmäßig die folgenden Dienstendpunkte konfiguriert:

   * Gateway oder Proxy: contoso.azure-api.net

   * Herausgeberportal und Entwicklerportal: contoso.portal.azure-api.net

   * Endpunkt für die direkte Verwaltung: contoso.management.azure-api.net

   * Git: contoso.scm.azure-api.net

Für den Zugriff auf diese API Management-Dienstendpunkte können Sie einen virtuellen Computer in einem Subnetz erstellen, das mit dem virtuellen Netzwerk verbunden ist, in dem API Management bereitgestellt wird. Wenn die interne virtuelle IP-Adresse für Ihren Dienst 10.0.0.5 lautet, können Sie die Hostdatei (% SystemDrive%\drivers\etc\hosts) wie folgt zuordnen:

   * 10.0.0.5     contoso.azure-api.net

   * 10.0.0.5     contoso.portal.azure-api.net

   * 10.0.0.5     contoso.management.azure-api.net

   * 10.0.0.5     contoso.scm.azure-api.net

Anschließend können Sie über den virtuellen Computer, den Sie erstellt haben, auf alle Dienstendpunkte zugreifen. Wenn Sie einen benutzerdefinierten DNS-Server in einem virtuellen Netzwerk verwenden, können Sie auch DNS-A-Einträge erstellen und auf diese Endpunkte von überall in Ihrem virtuellen Netzwerk zugreifen. 

### <a name="access-on-custom-domain-names"></a>Zugreifen über benutzerdefinierte Domänennamen

   1. Wenn Sie auf den API Management-Dienst nicht mit den Standardhostnamen zugreifen möchten, können Sie benutzerdefinierte Domänennamen für alle Dienstendpunkte einrichten, wie in der folgenden Abbildung zu sehen: 

   ![Einrichten einer benutzerdefinierten Domäne für API Management][api-management-custom-domain-name]

   2. Anschließend können Sie A-Einträge auf Ihrem DNS-Server erstellen, um auf die Endpunkte zuzugreifen, auf die nur über das virtuelle Netzwerk zugegriffen werden kann.

## <a name="routing"> </a> Routing
+ Eine private virtuelle IP-Adresse mit Lastenausgleich aus dem Subnetzbereich wird reserviert und für den Zugriff auf die API Management-Dienstendpunkte aus dem VNet verwendet.
+ Eine öffentliche IP-Adresse (VIP) mit Lastenausgleich wird auch reserviert, um den Zugriff auf den Verwaltungsdienst-Endpunkt nur über Port 3443 zu ermöglichen.
+ Eine IP-Adresse aus einem Subnetz-IP-Bereich (DIP) wird für den Zugriff auf Ressourcen innerhalb des VNet und eine öffentliche IP-Adresse (VIP) für den Zugriff auf Ressourcen außerhalb des VNet verwendet.
+ Öffentliche und private IP-Adressen mit Lastenausgleich finden Sie auf dem Blatt „Übersicht/Essentials“ im Azure-Portal.

## <a name="related-content"></a>Verwandte Inhalte
Weitere Informationen finden Sie in den folgenden Artikeln:
* [Gängige Probleme mit der Netzwerkkonfiguration][Common network configuration problems]
* [Azure Virtual Network – häufig gestellte Fragen](../virtual-network/virtual-networks-faq.md)
* [Managing DNS Records](https://msdn.microsoft.com/en-us/library/bb727018.aspx) (Verwalten von DNS-Einträgen)

[api-management-using-internal-vnet-menu]: ./media/api-management-using-with-internal-vnet/api-management-internal-vnet-menu.png
[api-management-internal-vnet-dashboard]: ./media/api-management-using-with-internal-vnet/api-management-internal-vnet-dashboard.png
[api-management-custom-domain-name]: ./media/api-management-using-with-internal-vnet/api-management-custom-domain-name.png


[Create API Management service]: api-management-get-started.md#create-service-instance
[Common network configuration problems]: api-management-using-with-vnet.md#network-configuration-issues

