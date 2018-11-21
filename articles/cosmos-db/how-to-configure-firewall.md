---
title: Konfigurieren der IP-Firewall für Ihr Azure Cosmos-Konto
description: Erfahren Sie, wie IP-Zugriffssteuerungsrichtlinien für die Firewallunterstützung in Azure Cosmos DB-Datenbankkonten konfiguriert werden.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: govindk
ms.openlocfilehash: d9a00bccb83fc60c96594ffacc5abde98c0f8470
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2018
ms.locfileid: "51632576"
---
# <a name="how-to-configure-ip-firewall-for-your-azure-cosmos-account"></a>Konfigurieren der IP-Firewall für Ihr Azure Cosmos-Konto

Sie können die in Ihrem Azure Cosmos-Konto gespeicherten Daten mithilfe von IP-Firewalls sichern. Azure Cosmos DB unterstützt die IP-basierte Zugriffssteuerung zur Unterstützung der Firewall für eingehende Verbindungen. Sie können eine IP-Firewall für das Azure Cosmos-Konto mit einer der folgenden Methoden festlegen:

1. Über das Azure-Portal
2. Deklarativ durch Verwenden einer Azure Resource Manager-Vorlage
3. Programmgesteuert über die Azure-Befehlszeilenschnittstelle oder mit Azure PowerShell durch Aktualisieren der Eigenschaft **ipRangeFilter**

## <a id="configure-ip-policy"></a> Konfigurieren der IP-Firewall über das Azure-Portal

Um die IP-Zugriffssteuerungsrichtlinie im Azure-Portal festzulegen, wechseln Sie zur Seite des Azure Cosmos DB-Kontos, klicken Sie im Navigationsmenü auf **Firewall und virteulle Netzwerke**, und ändern Sie den Wert für **IP-Zugriffssteuerung zulassen** in **Ausgewählte Netzwerke**. Klicken Sie dann auf **Speichern**. 

![Screenshot: Öffnen der Seite „Firewall“ im Azure-Portal](./media/how-to-configure-firewall/azure-portal-firewall.png)

Nach dem Aktivieren der IP-Zugriffssteuerung können Sie im Azure-Portal IP-Adressen, IP-Adressbereiche sowie Schalter angeben, um den Zugriff auf andere Azure-Dienste und auf das Azure-Portal zu ermöglichen. Details zu diesen Schaltern finden Sie in den folgenden Abschnitten.

> [!NOTE]
> Nach dem Aktivieren der IP-Zugriffssteuerungsrichtlinie für Ihr Azure Cosmos-Konto werden alle Anforderungen für das Azure Cosmos-Konto von Computern außerhalb der Liste der zulässigen IP-Adressbereiche abgelehnt. Darüber hinaus wird das Durchsuchen der Azure Cosmos DB-Ressourcen über das Portal blockiert, um die Integrität der Zugriffssteuerung sicherzustellen.

### <a name="allow-requests-from-the-azure-portal"></a>Zulassen von Anforderungen über das Azure-Portal 

Wenn Sie eine IP-Zugriffssteuerungsrichtlinie programmgesteuert aktivieren, müssen Sie der Eigenschaft **ipRangeFilter** die IP-Adresse für das Azure-Portal hinzufügen, damit der Zugriff weiterhin möglich ist. Die IP-Adressen des Portals lauten wie folgt:

|Region|IP-Adresse|
|------|----------|
|Deutschland|51.4.229.218|
|China|139.217.8.252|
|US Gov|52.244.48.71|
|Alle Regionen, mit Ausnahme der drei oben genannten|104.42.195.92,40.76.54.131,52.176.6.30,52.169.50.45,52.187.184.26|

Sie können den Zugriff auf das Azure-Portal ermöglichen, indem Sie wie im folgenden Screenshot gezeigt die Option **Zugriff über das Azure-Portal zulassen** auswählen: 

![Screenshot: Aktivieren des Zugriffs auf das Azure-Portal](./media/how-to-configure-firewall/enable-azure-portal.png)

### <a name="allow-requests-from-global-azure-datacenters-or-other-sources-within-azure"></a>Zulassen von Anforderungen von globalen Azure-Rechenzentren oder anderen Quellen in Azure

Wenn Sie in Diensten, die keine statische IP bereitstellen, z.B. Azure Stream Analytics oder Azure Functions, auf das Azure Cosmos-Konto zugreifen, können Sie dennoch den Zugriff über die IP-Firewall einschränken. Für den Zugriff auf das Azure Cosmos-Konto in diesen Diensten muss eine Firewalleinstellung aktiviert werden. Um diese Firewalleinstellung zu aktivieren, fügen Sie die IP-Adresse 0.0.0.0 zur Liste der zulässigen IP-Adressen hinzu. 0.0.0.0 beschränkt Anforderungen für das Azure Cosmos-Konto auf den IP-Adressbereich des Azure-Rechenzentrums. Mit dieser Einstellung ist der Zugriff auf das Azure Cosmos-Konto über andere IP-Adressbereiche nicht zulässig.

> [!NOTE]
> Über diese Option wird die Firewall so konfiguriert, dass alle von Azure ausgehenden Anforderungen zugelassen werden, einschließlich Anforderungen von den in Azure bereitgestellten Abonnements anderer Kunden. Die Liste der durch diese Option zulässigen IP-Adressen ist umfangreich, sodass die Wirksamkeit der Firewallrichtlinie eingeschränkt ist. Diese Option sollte nur verwendet werden, wenn die Anforderungen nicht von statischen IP-Adressen oder Subnetzen in VNETs stammen. Durch die Auswahl dieser Option wird automatisch der Zugriff über das Azure-Portal ermöglicht, da das Azure-Portal in Azure bereitgestellt wird.

Sie können den Zugriff auf das Azure-Portal ermöglichen, indem Sie wie im folgenden Screenshot gezeigt die Option **Verbindungen aus öffentlichen Azure-Rechenzentren akzeptieren** auswählen: 

![Screenshot: Öffnen der Seite „Firewall“ im Azure-Portal](./media/how-to-configure-firewall/enable-azure-services.png)

### <a name="requests-from-your-current-ip"></a>Anforderungen über Ihre aktuelle IP-Adresse

Zur Vereinfachung der Entwicklung können Sie im Azure-Portal die IP-Adresse Ihres Clientcomputers ermitteln und der Zulassungsliste hinzufügen, sodass die auf Ihrem Computer ausgeführten Apps auf das Azure Cosmos-Konto zugreifen können. Die Client-IP-Adresse wird im Portal automatisch erkannt. Es kann sich um die Client-IP-Adresse Ihres Computers oder die IP-Adresse Ihres Netzwerkgateways handeln. Sie müssen diese IP-Adresse entfernen, bevor Sie Ihre Workloads in die Produktion bringen. 

Um die aktuelle IP-Adresse zu aktivieren, wählen Sie **Meine aktuelle IP hinzufügen** aus. Dadurch wird die aktuelle IP-Adresse zur Liste der IP-Adressen hinzugefügt. Klicken Sie dann auf **Speichern**.

![Screenshot: Konfigurieren der Firewalleinstellungen für die aktuelle IP-Adresse](./media/how-to-configure-firewall/enable-current-ip.png)

### <a name="requests-from-cloud-services"></a>Anforderungen über Clouddienste

In Azure ist es üblich, dass Clouddienste Servicelogik der mittleren Ebene mithilfe von Azure Cosmos DB hosten. Um den Zugriff auf das Azure Cosmos-Konto über einen Clouddienst zu ermöglichen, muss die öffentliche IP-Adresse des Clouddiensts der Liste der zulässigen IP-Adressen, die dem Azure Cosmos-Konto zugeordnet ist, durch [Konfigurieren der IP-Zugriffssteuerungsrichtlinie](#configure-ip-policy) hinzugefügt werden. Dadurch wird sichergestellt, dass alle Rolleninstanzen von Clouddiensten Zugriff auf das Azure Cosmos-Konto haben. Sie können die IP-Adressen für Ihre Clouddienste im Azure-Portal abrufen (siehe dazu den folgenden Screenshot).

![Screenshot mit der öffentlichen IP-Adresse für einen Clouddienst im Azure-Portal](./media/how-to-configure-firewall/public-ip-addresses.png)

Wenn Sie Ihren Clouddienst horizontal hochskalieren, indem Sie zusätzliche Rolleninstanzen hinzufügen, haben diese neuen Instanzen automatisch Zugriff auf das Azure Cosmos-Konto, da sie zum gleichen Clouddienst gehören.

### <a name="requests-from-virtual-machines"></a>Anforderungen über virtuelle Computer

[Virtuelle Computer](https://azure.microsoft.com/services/virtual-machines/) oder [VM-Skalierungsgruppen](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) können auch zum Hosten von Diensten der mittleren Ebene mithilfe von Azure Cosmos DB verwendet werden. Um das Azure Cosmos-Konto für den Zugriff über virtuelle Computer zu konfigurieren, müssen die öffentlichen IP-Adressen des virtuellen Computers und/oder der VM-Skalierungsgruppe durch [Konfigurieren der IP-Zugriffssteuerungsrichtlinie](#configure-ip-policy) als eine der zulässigen IP-Adressen für das Azure Cosmos-Konto konfiguriert werden. Sie können die IP-Adressen für virtuelle Computer wie im folgenden Screenshot gezeigt im Azure-Portal abrufen:

![Screenshot mit einer öffentlichen IP-Adresse für einen virtuellen Computer im Azure-Portal](./media/how-to-configure-firewall/public-ip-addresses-dns.png)

Wenn Sie der Gruppe weitere virtuelle Computerinstanzen hinzufügen, erhalten diese automatisch Zugriff auf das Azure Cosmos-Konto.

### <a name="requests-from-the-internet"></a>Anforderungen aus dem Internet

Wenn Sie über einen Computer im Internet auf das Azure Cosmos-Konto zugreifen, muss die Client-IP-Adresse oder der Client-IP-Adressbereich des Computers der Liste der zulässigen IP-Adressen für das Konto hinzugefügt werden.

## <a id="configure-ip-firewall-arm"></a>Konfigurieren der IP-Firewall mithilfe einer Resource Manager-Vorlage

Zum Konfigurieren der Zugriffssteuerung für Ihr Azure Cosmos-Konto muss in der Resource Manager-Vorlage das Attribut **ipRangeFilter** mit einer Liste von IP-Adressbereichen angegeben werden, die in der Whitelist enthalten sind. Fügen Sie der Vorlage beispielsweise folgenden JSON-Code hinzu:

```json
   {
     "apiVersion": "2015-04-08",
     "type": "Microsoft.DocumentDB/databaseAccounts",
     "kind": "GlobalDocumentDB",
     "name": "[parameters('databaseAccountName')]",
     "location": "[resourceGroup().location]",
     "properties": {
     "databaseAccountOfferType": "Standard",
     "name": "[parameters('databaseAccountName')]",
     "ipRangeFilter":"183.240.196.255, 104.42.195.92,40.76.54.131, 52.176.6.30,52.169.50.45,52.187.184.26"
   }
```

## <a id="configure-ip-firewall-cli"></a>Konfigurieren der IP-Zugriffssteuerungsrichtlinie über die Azure-Befehlszeilenschnittstelle

Anhand des folgenden Befehls wird veranschaulicht, wie ein Azure Cosmos-Konto mit IP-Zugriffssteuerung erstellt wird: 

```azurecli-interactive

name="<Azure Cosmos account name>"
resourceGroupName="<Resource group name>"

az cosmosdb create \
  --name $name \
  --kind GlobalDocumentDB \
  --resource-group $resourceGroupName \
  --max-interval 10 \
  --max-staleness-prefix 200 \
  --ip-range-filter "183.240.196.255, 104.42.195.92,40.76.54.131, 52.176.6.30,52.169.50.45,52.187.184.26"
```

Führen Sie den folgenden Befehl aus, um die Firewalleinstellungen für ein vorhandenes Konto zu aktualisieren:

```azurecli-interactive
az cosmosdb update \
      --name $name \
      --resource-group $resourceGroupName \
      --ip-range-filter "183.240.196.255, 104.42.195.92,40.76.54.131, 52.176.6.30,52.169.50.45,52.187.184.26"
```

## <a id="troubleshoot-ip-firewall"></a>Beheben von Problemen bei der IP-Zugriffssteuerungsrichtlinie

Sie haben folgende Optionen, um Probleme bei der IP-Zugriffssteuerungsrichtlinie zu beheben: 

### <a name="azure-portal"></a>Azure-Portal 
Durch Aktivieren einer IP-Zugriffssteuerungsrichtlinie für Ihr Azure Cosmos-Konto werden alle Anforderungen für das Konto von Computern außerhalb der Liste der zulässigen IP-Adressbereiche blockiert. Wenn Sie also Vorgänge auf Portaldatenebene wie das Durchsuchen von Containern und das Abfragen von Dokumenten ermöglichen möchten, müssen Sie den Zugriff über das Azure-Portal im Bereich **Firewall** im Portal explizit zulassen.

### <a name="sdks"></a>SDKs 
Beim Zugriff auf Azure Cosmos-Ressourcen mithilfe von SDKs über Computer, die in der Zulassungsliste nicht enthalten sind, **wird eine generische „404 Nicht gefunden“-Antwort ohne weitere Details zurückgegeben**. Überprüfen Sie die Liste der zulässigen IP-Adressen für Ihr Konto, und stellen Sie sicher, dass die richtige Richtlinienkonfiguration auf das Cosmos-Konto angewandt wird. 

### <a name="check-source-ips-in-blocked-requests"></a>Überprüfen von Quell-IP-Adressen in blockierten Anforderungen
Aktivieren Sie die Diagnoseprotokollierung für das Azure Cosmos-Konto. In diesen Protokollen werden alle Anforderungen und Antworten angezeigt. **Die firewallbezogenen Meldungen werden intern mit einem 403-Rückgabecode protokolliert**. Wenn Sie diese Meldungen filtern, können Sie die Quell-IP-Adressen für die blockierten Anforderungen anzeigen. Siehe [Diagnoseprotokollierung für Azure Cosmos DB](logging.md).

### <a name="requests-from-subnet-with-service-endpoint-for-azure-cosmos-database-enabled"></a>Anforderungen über ein Subnetz mit aktiviertem Dienstendpunkt für die Azure Cosmos-Datenbank
Bei Anforderungen über ein Subnetz im VNET, für das der Dienstendpunkt für Azure Cosmos DB aktiviert ist, werden die Identitäten des VNET und des Subnetzes an Azure Cosmos-Konten gesendet. Diese Anforderungen enthalten nicht die öffentliche IP-Adresse der Quelle, daher werden sie durch die IP-Filter abgelehnt. Um den Zugriff über bestimmte Subnetze in VNETs zu ermöglichen, fügen Sie eine VNET-Zugriffssteuerungsliste hinzu. Informationen dazu finden Sie unter [Konfigurieren des VNET- und subnetzbasierten Zugriff für Ihr Azure Cosmos-Konto](how-to-configure-vnet-service-endpoint.md). Es kann bis zu 15 Minuten dauern, bis die Firewallregeln angewandt werden.


## <a name="next-steps"></a>Nächste Schritte

Informationen zum Konfigurieren von Dienstendpunkten von virtuellen Netzwerken für Ihr Azure Cosmos DB-Konto finden Sie in den folgenden Artikeln:

* [VNET- und Subnetzzugriffssteuerung für Ihr Azure Cosmos-Konto](vnet-service-endpoint.md)
* [Konfigurieren des VNET- und subnetzbasierten Zugriffs für Ihr Azure Cosmos-Konto](how-to-configure-vnet-service-endpoint.md)


