---
title: Bereitstellen eines Azure Batch-Pools in einem virtuellen Netzwerk | Microsoft-Dokumentation
description: "Sie können einen Batch-Pool in einem virtuellen Netzwerk erstellen, damit Computeknoten sicher mit anderen VMs (z.B. Dateiserver) im Netzwerk kommunizieren können."
services: batch
author: v-dotren
manager: timlt
ms.service: batch
ms.topic: article
ms.date: 10/16/2017
ms.author: v-dotren
ms.openlocfilehash: 3c62bff7ba37f7e45d73fa2cf67a4aee3b4a7a38
ms.sourcegitcommit: bd0d3ae20773fc87b19dd7f9542f3960211495f9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2017
---
# <a name="create-an-azure-batch-pool-in-a-virtual-network"></a>Erstellen eines Azure Batch-Pools in einem virtuellen Netzwerk


Beim Erstellen eines Azure Batch-Pools können Sie einen Pool in einem von Ihnen angegebenen Subnetz eines [virtuellen Azure-Netzwerks](../virtual-network/virtual-networks-overview.md) (VNET) bereitstellen. In diesem Artikel wird erklärt, wie Sie einen Batch-Pool in einem VNET einrichten. 



## <a name="why-use-a-vnet"></a>Gründe für die Verwendung eines VNETs


Ein Azure Batch-Pool verfügt über Einstellung, durch die Computeknoten miteinander kommunizieren können, z.B. zum Ausführen von Tasks mit mehreren Instanzen. Für diese Einstellungen ist kein separates VNET erforderlich. Allerdings können die Knoten standardmäßig nicht mit VMs kommunizieren, die Teil des Batch-Pools sind, wie etwa Lizenzserver und Dateiserver. Um die sichere Kommunikation zwischen Computeknoten und VMs oder lokalen Netzwerken zu ermöglichen, können Sie den Pool in einem Subnetz eines Azure VNETs bereitstellen. 



## <a name="prerequisites"></a>Voraussetzungen

* **Authentifizierung**. Zur Verwendung eines Azure VNETs muss die Batch-Client-API die Azure Active Directory-Authentifizierung verwenden. Die Azure Batch-Unterstützung für Azure AD ist unter [Authentifizieren von Lösungen des Azure Batch-Diensts mit Active Directory](batch-aad-auth.md) dokumentiert. 

* **Ein Azure VNET** Um ein VNET mit mindestens einem Subnetz vorzubereiten, können Sie das Azure-Portal, Azure PowerShell, die Azure-Befehlszeilenschnittstelle (CLI) oder andere Methoden verwenden. Um ein auf Azure Resource Manager basiertes VNET zu erstellen, können Sie sich unter [Erstellen eines virtuellen Netzwerks mit mehreren Subnetzen](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) informieren. Um ein klassischen VNET zu erstellen, können Sie sich unter [Erstellen eines (klassischen) virtuellen Netzwerks mit mehreren Subnetzen](../virtual-network/create-virtual-network-classic.md) informieren.

### <a name="vnet-requirements"></a>VNET-Anforderungen
[!INCLUDE [batch-virtual-network-ports](../../includes/batch-virtual-network-ports.md)]
    
## <a name="create-a-pool-with-a-vnet-in-the-portal"></a>Erstellen eines Pools mit einem VNET im Portal

Wenn Sie Ihr VNET erstellt und einem Subnetz zugewiesen haben, können Sie einen Batch-Pool mit diesem VNET erstellen. Führen Sie die folgenden Schritte aus, um einen Pool im Azure-Portal zu erstellen: 



1. Navigieren Sie im Azure-Portal zu Ihrem Batch-Konto. Das Konto muss sich im gleichen Abonnement und der gleichen Region wie die Ressourcengruppe befinden, die das VNET enthält, das Sie verwenden möchten. 
2. Wählen Sie links im Fenster **Einstellungen** die Menüoption **Pools** aus.
3. Wählen Sie im Fenster **Pools** den Befehl **Hinzufügen** aus.
4. Wählen Sie im Fenster **Pool hinzufügen** die Option, die Sie verwenden möchten, in der Dropdownliste **Imagetyp** aus. 
5. Wählen Sie **Verleger/Angebot/SKU** für Ihre benutzerdefiniertes Image aus.
6. Geben Sie die übrigen erforderlichen Einstellungen an, einschließlich **Knotengröße**, **Ziel für dedizierte Knoten** und **Knoten mit niedriger Priorität**, sowie alle gewünschten optionalen Einstellungen.
7. Wählen Sie unter **Virtuelles Netzwerk** das virtuelle Netzwerk und Subnetz aus, die Sie verwenden möchten.
  
  ![„Pool hinzufügen“ mit virtuellem Netzwerk](./media/batch-virtual-network/add-vnet-pool.png)

## <a name="user-defined-routes-for-forced-tunneling"></a>Benutzerdefinierte Routen für erzwungenes Tunneln

Ihr Unternehmen erfordert möglicherweise zu Überprüfungs- und Protokollierungszwecken das (erzwungene) Weiterleiten von Internetdatenverkehr vom Subnetz zurück zu ihrem lokalen Standort. Möglicherweise ist die Tunnelerzwingung für das Subnetz in Ihrem VNET aktiviert. 

Um sicherzustellen, dass die Computeknoten Ihres Azure Batch-Pools in einem VNET funktionieren, in dem die Tunnelerzwingung aktiviert ist, müssen Sie folgende [benutzerdefinierte Routen](../virtual-network/virtual-networks-udr-overview.md) für dieses Subnetz hinzufügen:

* Der Batch-Dienst muss für die zeitliche Planung von Tasks mit den Computeknoten des Pools kommunizieren. Um diese Kommunikation zu ermöglichen, fügen Sie eine benutzerdefinierte für jede IP-Adresse hinzu, die vom Batch-Dienst in der Region Ihres Batch-Kontos verwendet werden. Wenden Sie sich an den Azure-Support, um die Liste der IP-Adressen des Batch-Diensts zu erhalten.

* Stellen Sie sicher, dass ausgehender Datenverkehr an Azure Storage (also URLs im Format `<account>.table.core.windows.net`, `<account>.queue.core.windows.net` und `<account>.blob.core.windows.net`) nicht über Ihr lokales Netzwerkgerät blockiert wird.
    
## <a name="next-steps"></a>Nächste Schritte

- Eine detaillierte Übersicht über Batch finden Sie unter [Entwickeln von parallelen Computelösungen in größerem Umfang mit Batch](batch-api-basics.md).
