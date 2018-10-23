---
title: Netzwerksicherheit in Azure Data Lake Storage Gen1 | Microsoft-Dokumentation
description: Grundlegendes zur Funktionsweise der IP-Firewall und der Integration virtueller Netzwerke in Azure Data Lake Storage Gen1
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/09/2018
ms.author: elsung
ms.openlocfilehash: 0da5962bc0b48a387ee82a1db36099682e14bca3
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/12/2018
ms.locfileid: "49168120"
---
# <a name="virtual-network-integration-for-azure-data-lake-storage-gen1---preview"></a>Virtual Network-Integration für Azure Data Lake Storage Gen1: Vorschauversion

Einführung in die Virtual Network-Integration für Azure Data Lake Storage Gen1 (in der Vorschauphase). Mit der VNET-Integration können Sie nicht autorisierten Zugriff auf Ihre Azure Data Lake Storage Gen1-Konten verhindern, indem Sie diese Konten auf Ihre bestimmten virtuellen Netzwerke und Subnetze beschränken. Sie können Ihr ADLS Gen1-Konto jetzt so konfigurieren, dass es nur Datenverkehr aus den vorgesehenen virtuellen Netzwerken akzeptiert und jeglichen anderen Zugriff blockiert. Dadurch wird Ihr ADLS-Konto vor externen Bedrohungen geschützt.

Die VNET-Integration für ADLS Gen1 nutzt die Dienstendpunktsicherheit für virtuelle Netzwerke zwischen Ihrer Virtual Network-Instanz und dem Azure Active Directory-Dienst, um zusätzliche Sicherheitsansprüche im Zugriffstoken zu generieren. Diese Ansprüche werden dann zur Authentifizierung Ihres virtuellen Netzwerks bei Ihrem ADLS Gen1-Konto und zum Gewähren von Zugriff verwendet.

> [!NOTE]
> Diese Technologie befindet sich in der Vorschauphase, und wir raten von einem Einsatz in Produktionsumgebungen ab.
>
> Für die Verwendung dieser Funktionen fallen keine zusätzlichen Gebühren an. Die Nutzung Ihres Kontos wird Ihnen zum Standardtarif für ADLS Gen1 ([Preise](https://azure.microsoft.com/pricing/details/data-lake-store/?cdn=disable)) und alle von Ihnen verwendeten Azure-Dienste ([Preise](https://azure.microsoft.com/pricing/#product-picker)) berechnet.

## <a name="scenarios-for-vnet-integration-for-adls-gen1"></a>Szenarien für die VNET-Integration für ADLS Gen 1

Die VNET-Integration für ADLS Gen1 ermöglicht Ihnen das Beschränken des Zugriffs auf Ihr ADLS Gen1-Konto auf vorgesehene virtuelle Netzwerke und Subnetze.  Anderen VNETs/virtuellen Computern in Azure ist der Zugriff auf Ihr Konto nicht mehr möglich, sobald es an das vorgesehene VNET-Subnetz gebunden ist.  Funktionell ermöglicht die VNET-Integration für ADLS Gen1 dasselbe Szenario wie [Dienstendpunkte im virtuellen Netzwerk](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview).  Es bestehen jedoch einige wichtige Unterschiede, die in den folgenden Abschnitten erläutert werden. 

![Szenariodiagramm zur VNET-Integration für ADLS Gen1](media/data-lake-store-network-security/scenario-diagram.png)

> [!NOTE]
> Die vorhandenen IP-Firewallregeln können zusätzlich zu VNET-Regeln verwendet werden, um auch Zugriff aus lokalen Netzwerken zuzulassen. 

## <a name="optimal-routing-with-adls-gen1-vnet-integration"></a>Optimales Routing VNET-Integration für ADLS Gen1

Ein Hauptvorteil von VNET-Dienstendpunkten ist das [optimale Routing](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview#key-benefits) aus Ihrem virtuellen Netzwerk.  Zum Durchführen der gleichen Routenoptimierung für ADLS Gen1-Konten verwenden Sie die folgenden [benutzerdefinierten Routen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#user-defined) aus Ihrem VNET an Ihr ADLS Gen1-Konto:

- **Öffentliche ADLS-IP-Adresse**: Verwenden Sie die öffentliche IP-Adresse für Ihre ADLS Gen1-Zielkonten.  Sie können die IP-Adressen für Ihr ADLS Gen1-Konto ermitteln, indem Sie die [DNS-Namen der Konten auflösen](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-connectivity-from-vnets#enabling-connectivity-to-azure-data-lake-storage-gen1-from-vms-with-restricted-connectivity).  Erstellen Sie für jede Adresse einen separaten Eintrag.

```azurecli
# Create a Route table for your resource group
az network route-table create --resource-group $RgName --name $RouteTableName

# Create Route Table Rules for ADLS Public IP Addresses
# There will be one rule per ADLS Public IP Addresses 
az network route-table route create --name toADLSregion1 --resource-group $RgName --route-table-name $RouteTableName --address-prefix <ADLS Public IP Address> --next-hop-type Internet

# Update the VNet and apply the newly created Route Table to it
az network vnet subnet update --vnet-name $VnetName --name $SubnetName --resource-group $RgName --route-table $RouteTableName
```

## <a name="data-exfiltration-from-the-customer-vnet"></a>Herausfilterung von Daten aus dem Kunden-VNET

Neben der Sicherung der ADLS-Konten für den Zugriff aus Virtual Network sind Sie möglicherweise auch daran interessiert, sicherzustellen, dass keine Herausfilterung für ein nicht autorisiertes Konto erfolgt.

Unsere Empfehlung ist die Verwendung einer Firewalllösung in Ihrem VNET, um den ausgehenden Datenverkehr basierend auf der URL des Zielkontos zu filtern und nur autorisierten ADLS Gen1-Konten den Zugriff zu ermöglichen.

Einige der verfügbaren Optionen sind:
- [Azure-Firewall](https://docs.microsoft.com/azure/firewall/overview): Sie können [eine Azure Firewall-Instanz](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal) für Ihr VNET bereitstellen und konfigurieren, den ausgehenden ADLS-Datenverkehr sichern und auf die URL des bekannten und autorisierten Kontos beschränken.
- Firewall für [virtuelle Netzwerkgeräte (Network Virtual Appliance, NVA)](https://azure.microsoft.com/solutions/network-appliances/): Wenn Ihr Administrator nur die Verwendung bestimmter kommerzieller Firewallanbieter autorisiert, können Sie eine in Azure Marketplace verfügbare NVA-Firewalllösung nutzen, um die gleiche Funktion zu erzielen.

> [!NOTE]
> Die Verwendung von Firewalls im Datenpfad führt einen zusätzlichen Hop im Datenpfad ein und beeinträchtigt u.U. die Leistung beim End-to-End-Datenaustausch, einschließlich des verfügbaren Durchsatzes und der Verbindungswartezeit. 

## <a name="limitations"></a>Einschränkungen
1.  HDInsight-Cluster müssen nach dem Hinzufügen zur Vorschauversion neu erstellt werden.  Cluster, die erstellt wurden, bevor die Unterstützung VNET-Integration für ADLS Gen1 verfügbar war, müssen neu erstellt werden, um diese neue Funktion zu unterstützen. 
2.  Die Auswahl eines ADLS Gen1-Kontos mit aktivierter VNET-Integration beim Erstellen eines neuen HDInsight-Clusters führt dazu, dass der Prozess fehlschlägt. Sie müssen zuerst die VNET-Regel deaktivieren, oder Sie können auf dem Blatt **Firewall und virtuelle Netzwerke** des ADLS-Kontos **den Zugriff aus allen Netzwerken und Diensten zulassen**.  Weitere Informationen finden Sie im Abschnitt [Ausnahmen](##Exceptions).
3.  Die Vorschauversion der VNET-Integration für ADLS Gen1 funktioniert nicht mit [verwalteten Identitäten für Azure-Ressourcen](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).  
4.  Der Zugriff auf Datei-/Ordnerdaten in Ihrem VNET-fähigen ADLS Gen1-Konto über das Portal ist nicht möglich.  Dies gilt auch für den Zugriff von einem virtuellen Computer innerhalb des VNET und Aktivitäten wie die Verwendung des Daten-Explorers.  Aktivitäten zur Kontoverwaltung funktionieren weiterhin.  Der Zugriff auf Datei-/Ordnerdaten in Ihrem VNET-fähigen ADLS-Konto ist über alle Ressourcen außerhalb des Portals möglich: SDK-Zugriff, PowerShell-Skripte, andere Azure-Dienste (die nicht aus dem Portal stammen) usw. … 

## <a name="configuration"></a>Konfiguration

### <a name="step1-configure-your-vnet-to-use-aad-service-endpoint"></a>Schritt 1: Konfigurieren Ihres VNET für die Verwendung des AAD-Dienstendpunkts
1.  Melden Sie sich im Azure-Portal bei Ihrem Konto an. 
2.  [Erstellen Sie ein neues virtuelles Netzwerk ](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)in Ihrem Abonnement, oder navigieren Sie zu einem vorhandenen virtuellen Netzwerk.  Das VNET muss sich derzeit in der gleichen Region wie das ADLS Gen 1-Konto befinden. 
3.  Wählen Sie auf dem Blatt „Virtuelle Netzwerk“ die Option **Dienstendpunkte** aus. 
4.  Klicken Sie auf **Hinzufügen**, um einen neuen Dienstendpunkt hinzuzufügen.
![Hinzufügen eines VNET-Dienstendpunkts](media/data-lake-store-network-security/config-vnet-1.png)
5.  Wählen Sie **Microsoft.AzureActiveDirectory** als Dienst für den Endpunkt.
![Auswählen des Dienstendpunkts „Microsoft.AzureActiveDirectory“](media/data-lake-store-network-security/config-vnet-2.png)
6.  Wählen Sie die Subnetze aus, für die Sie Verbindungen zulassen möchten, und klicken Sie auf **Hinzufügen**.
![Auswählen des Subnetzes](media/data-lake-store-network-security/config-vnet-3.png)
7.  Das Hinzufügen des Dienstendpunkts kann bis zu 15 Minuten dauern. Sobald er hinzugefügt ist, wird er in der Liste angezeigt. Vergewissern Sie sich, dass er angezeigt wird, und alle Details der Konfiguration entsprechen. 
![Erfolgreiches Hinzufügen des Dienstendpunkts](media/data-lake-store-network-security/config-vnet-4.png)

### <a name="step-2-set-up-the-allowed-vnetsubnet-for-your-adls-gen1-account"></a>Schritt 2: Einrichten des zulässigen VNET/Subnetzes für Ihr ADLS Gen1-Konto
1.  Nach der Konfiguration Ihres VNET [erstellen Sie ein neues Azure Data Lake Storage Gen1-Konto](data-lake-store-get-started-portal.md#create-a-data-lake-storage-gen1-account) in Ihrem Abonnement, oder navigieren Sie zu einem vorhandenen ADLS Gen1-Konto. Das ADLS Gen1-Konto muss sich derzeit in der gleichen Region wie das VNET befinden. 
2.  Wählen Sie **Firewall und virtuelle Netzwerke**.

  > [!NOTE]
  > Wenn **Firewall und virtuelle Netzwerke** in den Einstellungen nicht angezeigt wird, melden Sie sich vom Portal ab. Schließen Sie den Browser. Löschen Sie den Browsercache. Starten Sie den Computer neu, und versuchen Sie es erneut.

  ![Hinzufügen einer VNET-Regel zu Ihrem ADLS-Konto](media/data-lake-store-network-security/config-adls-1.png)
3.  Wählen Sie **Ausgewählte Netzwerke**. 
4.  Klicken Sie auf **Vorhandenes virtuelles Netzwerk hinzufügen**.
  ![Vorhandenes virtuelles Netzwerk wird hinzugefügt.](media/data-lake-store-network-security/config-adls-2.png)
5.  Wählen Sie die VNETS und Subnetze aus, für die Konnektivität zugelassen werden soll, klicken Sie auf **Hinzufügen**.
  ![Wählen des VNET und der Subnetze](media/data-lake-store-network-security/config-adls-3.png)
6.  Stellen Sie sicher, dass die VNETs und Subnetze ordnungsgemäß in der Liste angezeigt werden, und **speichern** Sie diese.
  ![Speichern der neuen Regel](media/data-lake-store-network-security/config-adls-4.png)

  > [!NOTE]
  > Nach dem Speichern kann es bis zu 5 Minuten dauern, bis die Einstellungen wirksam werden.

7.  [Optional] Wenn Sie neben VNETs und Subnetzen auch Konnektivität aus bestimmten IP-Adressen zulassen möchten, können Sie diese Einstellung im Abschnitt **Firewall** auf derselben Seite vornehmen. 

## <a name="exceptions"></a>Ausnahmen
Im Bereich „Ausnahmen“ auf dem Blatt **Firewall und virtuelle Netzwerke** befinden sich zwei Kontrollkästchen, mit denen Konnektivität aus einer Reihe von Diensten und virtuellen Computern in Azure zugelassen wird.
![Ausnahmen für Firewall und virtuelle Netzwerke](media/data-lake-store-network-security/firewall-exceptions.png)
- **Allow all Azure services to access this Data Lake Storage Gen1 account** (Allen Azure-Diensten den Zugriff auf dieses Data Lake Storage Gen1-Konto erlauben) ermöglicht allen Azure-Diensten wie Azure Data Factory, Event Hubs, alle virtuellen Azure-Computer usw. die Kommunikation mit Ihrem ADLS-Konto.

- **Allow Azure Data Lake Analytics to access this Data Lake Storage Gen1 account** (Azure Data Lake Analytics den Zugriff auf dieses Data Lake Storage Gen1-Konto erlauben) ermöglicht die Verbindung aus dem Azure Data Lake Analytics-Dienst mit diesem ADLS-Konto. 

Wir empfehlen, diese Ausnahmen deaktiviert zu lassen und nur zu aktivieren, wenn Sie Konnektivität aus diesen anderen Diensten außerhalb Ihres VNET benötigen.
