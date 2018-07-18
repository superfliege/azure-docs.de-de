---
title: Firewallunterstützung und IP-Access Control in Azure Cosmos DB | Microsoft-Dokumentation
description: Erfahren Sie, wie IP-Access Control-Richtlinien für die Firewallunterstützung in Azure Cosmos DB-Datenbankkonten verwendet werden.
keywords: IP-Access Control, Firewallunterstützung
services: cosmos-db
author: SnehaGunda
manager: kfile
tags: azure-resource-manager
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 03/30/2018
ms.author: sngun
ms.openlocfilehash: e67fc5e00a638f116a69dbb36c60cf183cbde808
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37857829"
---
# <a name="azure-cosmos-db-firewall-support"></a>Azure Cosmos DB-Firewallunterstützung
Zum Sichern von in einem Azure Cosmos DB-Datenbankkonto gespeicherten Daten bietet Azure Cosmos DB Unterstützung für ein auf einem Geheimnis basierendes [Autorisierungsmodell](https://msdn.microsoft.com/library/azure/dn783368.aspx), das einen starken hashbasierten Nachrichtenauthentifizierungscode (HMAC) nutzt. Azure Cosmos DB unterstützt nun neben dem auf einem Geheimnis basierenden Autorisierungsmodell durch Richtlinien gesteuerte IP-basierte Access Control für die eingehende Firewallunterstützung. Dieses Modell ähnelt den Firewallregeln eines herkömmlichen Datenbanksystems und bietet zusätzliche Sicherheit für das Azure Cosmos DB-Datenbankkonto. Mit diesem Modell können Sie nun ein Azure Cosmos DB-Datenbankkonto so konfigurieren, dass es nur über eine genehmigte Gruppe von Computern und/oder Clouddiensten zugänglich ist. Für den Zugriff auf Azure Cosmos DB-Ressourcen über diese genehmigten Gruppen von Computern und Diensten muss der Aufrufer weiterhin ein gültiges Autorisierungstoken vorlegen.

> [!NOTE]
> Die Firewallunterstützung ist derzeit für Azure Cosmos DB-SQL-API- und Mongo-API-Konten verfügbar. Die Funktion zum Konfigurieren von Firewalls für andere APIs und unabhängige Clouds (Sovereign Clouds) wie Azure Deutschland oder Azure Government wird bald verfügbar sein. Wenn Sie beabsichtigen, eine Dienstendpunkt-ACL für Ihr Azure Cosmos DB-Konto zu konfigurieren, für das eine bestehende IP-Firewall konfiguriert ist, notieren Sie sich die Firewallkonfiguration, entfernen Sie die IP-Firewall, und konfigurieren Sie dann die Dienstendpunkt-ACL. Nachdem Sie den Dienstendpunkt konfiguriert haben, können Sie die IP-Firewall bei Bedarf wieder aktivieren.

## <a name="ip-access-control-overview"></a>Übersicht über die IP-Access Control
Standardmäßig ist ein Azure Cosmos DB-Datenbankkonto über das öffentliche Internet zugänglich, solange die Anforderung zusammen mit einem gültigen Autorisierungstoken erfolgt. Zum Konfigurieren der auf IP-Richtlinien basierten Access Control muss der Benutzer die Gruppe der IP-Adressen oder IP-Adressbereiche im CIDR-Format angeben, die als Liste der zulässigen Client-IPs für ein bestimmtes Datenbankkonto aufgenommen wird. Nachdem diese Konfiguration angewendet wurde, werden alle Anforderungen von Computern, die nicht in dieser Zulassungsliste enthalten sind, vom Server blockiert.  In der folgenden Abbildung ist der Verbindungsverarbeitungsablauf für die IP-basierte Access Control dargestellt.

![Abbildung zum Verbindungsprozess für die IP-basierte Access Control](./media/firewall-support/firewall-support-flow.png)

## <a id="configure-ip-policy"></a> Konfigurieren der IP-Access Control-Richtlinie
Die IP-Zugriffssteuerungsrichtlinie kann im Azure-Portal oder programmgesteuert über die [Azure-Befehlszeilenschnittstelle](cli-samples.md), über [Azure PowerShell](powershell-samples.md) oder über die [REST-API](/rest/api/cosmos-db/) durch Aktualisieren der Eigenschaft **ipRangeFilter** festgelegt werden. 

Um die IP-Zugriffssteuerungsrichtlinie im Azure-Portal festzulegen, wechseln Sie zur Seite des Azure Cosmos DB-Kontos, klicken Sie im Navigationsmenü auf **Firewall und virteulle Netzwerke**, und ändern Sie den Wert für **IP-Zugriffssteuerung zulassen** in **Ausgewählte Netzwerke**. Klicken Sie dann auf **Speichern**. 

![Screenshot: Öffnen der Seite „Firewall“ im Azure-Portal](./media/firewall-support/azure-portal-firewall.png)

Nach dem Aktivieren der IP-Zugriffssteuerung können Sie im Portal IP-Adressen und -Bereiche sowie Schalter angeben, um den Zugriff auf andere Azure-Dienste und auf das Azure-Portal zu ermöglichen. Weitere Informationen zu diesen Schaltern finden Sie in den folgenden Abschnitten.

> [!NOTE]
> Durch Aktivieren einer IP-Access Control-Richtlinie für Ihr Azure Cosmos DB-Datenbankkonto wird der Zugriff auf das Azure Cosmos DB-Datenbankkonto von Computern außerhalb der konfigurierten Liste der zulässigen IP-Adressbereiche blockiert. Aufgrund dieses Modells wird auch das Durchsuchen der Vorgänge auf Datenebene über das Portal blockiert, um die Integrität der Access Control sicherzustellen.

## <a name="connections-from-the-azure-portal"></a>Verbindungen über das Azure-Portal 

Wenn Sie eine IP-Zugriffssteuerungsrichtlinie programmgesteuert aktivieren, müssen Sie der Eigenschaft **ipRangeFilter** die IP-Adresse für das Azure-Portal hinzufügen, damit der Zugriff weiterhin möglich ist. Die IP-Adressen des Portals lauten wie folgt:

|Region|IP-Adresse|
|------|----------|
|Alle Regionen, mit Ausnahme der unten angegebenen|104.42.195.92,40.76.54.131,52.176.6.30,52.169.50.45,52.187.184.26|
|Deutschland|51.4.229.218|
|China|139.217.8.252|
|US Gov|52.244.48.71|

Zugriff auf das Azure-Portal ist standardmäßig aktiviert, wenn Sie die Firewall-Einstellung im Azure-Portal in **Ausgewählte Netzwerke** ändern. 

![Screenshot: Aktivieren des Zugriffs auf das Azure-Portal](./media/firewall-support/enable-azure-portal.png)

## <a name="connections-from-global-azure-datacenters-or-azure-paas-services"></a>Verbindungen von globalen Azure-Rechenzentren oder Azure-PaaS-Diensten
In Azure werden PaaS-Dienste wie Azure Stream Analytics, Azure Functions und Azure App Service in Verbindung mit Azure Cosmos DB verwendet. Um den Zugriff auf ein Azure Cosmos DB-Datenbankkonto über diese Art von Diensten zu ermöglichen, deren IP-Adresse nicht sofort verfügbar ist, fügen Sie die IP-Adresse „0.0.0.0“ der Liste der zulässigen IP-Adressen hinzu, die dem Azure Cosmos DB-Datenbankkonto programmgesteuert zugeordnet ist. 

Der Zugriff auf die Verbindungen von globalen Azure-Rechenzentren aus ist standardmäßig aktiviert, wenn Sie die Firewalleinstellung im Azure-Portal in **Ausgewählte Netzwerke** ändern. 

![Screenshot: Öffnen der Seite „Firewall“ im Azure-Portal](./media/firewall-support/enable-azure-services.png)

## <a name="connections-from-your-current-ip"></a>Verbindungen über Ihre aktuelle IP-Adresse

Um die Entwicklung zu vereinfachen, unterstützt das Azure-Portal Sie dabei, die IP-Adresse Ihres Clientcomputers zu ermitteln und der Zulassungsliste hinzuzufügen, sodass auf Ihrem Computer ausgeführte Apps auf das Azure Cosmos DB-Konto zugreifen können. Die Client-IP-Adresse wird hier so ermittelt, wie sie im Portal zu sehen ist. Es kann sich um die Client-IP-Adresse Ihres Computers handeln, aber auch um die IP-Adresse Ihres Netzwerkgateways. Vergessen Sie nicht, diese Adresse vor dem Übergang in die Produktion zu entfernen.

Um die aktuelle IP-Adresse zu aktivieren, wählen Sie **Meine aktuelle IP hinzufügen** aus. Dadurch wird die aktuelle IP-Adresse zur Liste der IP-Adressen hinzugefügt. Klicken Sie dann auf **Speichern**.

![Screenshot: Konfigurieren der Firewalleinstellungen für die aktuelle IP-Adresse](./media/firewall-support/enable-current-ip.png)

## <a name="connections-from-cloud-services"></a>Verbindungen über Clouddienste
In Azure ist es üblich, dass Clouddienste Servicelogik der mittleren Ebene mithilfe von Azure Cosmos DB hosten. Um den Zugriff auf ein Azure Cosmos DB-Datenbankkonto über einen Clouddienst zu ermöglichen, muss die öffentliche IP-Adresse des Clouddiensts der Liste der zulässigen IP-Adressen, die dem Azure Cosmos DB-Datenbankkonto zugeordnet ist, durch [Konfigurieren der IP-Access Control-Richtlinie](#configure-ip-policy) hinzugefügt werden. Dadurch wird sichergestellt, dass alle Rolleninstanzen von Clouddiensten Zugriff auf das Azure Cosmos DB-Datenbankkonto haben. Sie können die IP-Adressen für Ihre Clouddienste im Azure-Portal abrufen (siehe dazu den folgenden Screenshot).

![Screenshot mit der öffentlichen IP-Adresse für einen Clouddienst im Azure-Portal](./media/firewall-support/public-ip-addresses.png)

Wenn Sie Ihren Clouddienst horizontal hochskalieren, indem Sie zusätzliche Rolleninstanzen hinzufügen, haben diese neuen Instanzen automatisch Zugriff auf das Azure Cosmos DB-Datenbankkonto, da sie zum gleichen Clouddienst gehören.

## <a name="connections-from-virtual-machines"></a>Verbindungen über virtuelle Computer
[Virtuelle Computer](https://azure.microsoft.com/services/virtual-machines/) oder [VM-Skalierungsgruppen](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) können auch zum Hosten von Diensten der mittleren Ebene mithilfe von Azure Cosmos DB verwendet werden.  Um das Azure Cosmos DB-Datenbankkonto für den Zugriff von virtuellen Computern zu konfigurieren, müssen die öffentlichen IP-Adressen des virtuellen Computers und/oder der VM-Skalierungsgruppe durch [Konfigurieren der IP-Access Control-Richtlinie](#configure-ip-policy) als eine der zulässigen IP-Adressen für das Azure Cosmos DB-Datenbankkonto konfiguriert werden. Sie können die IP-Adressen für virtuelle Computer im Azure-Portal abrufen (siehe dazu den folgenden Screenshot).

![Screenshot mit einer öffentlichen IP-Adresse für einen virtuellen Computer im Azure-Portal](./media/firewall-support/public-ip-addresses-dns.png)

Wenn Sie der Gruppe weitere virtuelle Computerinstanzen hinzufügen, erhalten diese automatisch Zugriff auf das Azure Cosmos DB-Datenbankkonto.

## <a name="connections-from-the-internet"></a>Verbindungen über das Internet
Wenn Sie über einen Computer im Internet auf ein Azure Cosmos DB-Datenbankkonto zugreifen, muss die Client-IP-Adresse oder der Client-IP-Adressbereich des Computers der Liste der zulässigen IP-Adressen für das Azure Cosmos DB-Datenbankkonto hinzugefügt werden. 

## <a name="using-azure-resource-manager-template-to-set-up-the-ip-access-control"></a>Einrichten der IP-Zugriffssteuerung mithilfe der Azure Resource Manager-Vorlage

Fügen Sie folgenden JSON-Code zu Ihrer Vorlage hinzu, um die IP-Zugriffssteuerung einzurichten. Die Resource Manager-Vorlage für ein Konto weist ein ipRangeFilter-Attribut auf, das eine Liste mit IP-Adressbereichen darstellt, die in der Whitelist enthalten sein soll.

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
     "ipRangeFilter":"10.0.0.1,10.0.0.2,183.240.196.255"
   }
   }
```

## <a name="troubleshooting-the-ip-access-control-policy"></a>Problembehandlung für die IP-Access Control-Richtlinie
### <a name="portal-operations"></a>Portalvorgänge
Durch Aktivieren einer IP-Access Control-Richtlinie für Ihr Azure Cosmos DB-Datenbankkonto wird der Zugriff auf das Azure Cosmos DB-Datenbankkonto von Computern außerhalb der konfigurierten Liste der zulässigen IP-Adressbereiche blockiert. Wenn Sie also Vorgänge auf Portaldatenebene wie das Durchsuchen von Containern und das Abfragen von Dokumenten ermöglichen möchten, müssen Sie den Zugriff durch das Azure-Portal auf der Seite **Firewall** im Portal explizit zulassen. 

### <a name="sdk--rest-api"></a>SDK und REST-API
Aus Sicherheitsgründen wird beim Zugriff über das SDK oder die REST-API von Computern, die in der Zulassungsliste nicht enthalten sind, die generische Antwort „404 Nicht gefunden“ ohne weitere Einzelheiten zurückgegeben. Überprüfen Sie die Liste der zulässigen IP-Adressen, die für Ihr Azure Cosmos DB-Datenbankkonto konfiguriert ist, um sicherzustellen, dass die passende Richtlinienkonfiguration für das Azure Cosmos DB-Datenbankkonto angewendet wird.

## <a name="next-steps"></a>Nächste Schritte
Informationen zur netzwerkbezogenen Leistungssteigerung finden Sie in den [Tipps zur Leistungssteigerung](performance-tips.md).

