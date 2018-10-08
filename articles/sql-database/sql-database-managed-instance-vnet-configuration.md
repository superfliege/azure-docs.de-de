---
title: Verwaltete Azure SQL-Datenbank-Instanz – VNET-Konfiguration | Microsoft-Dokumentation
description: In diesem Thema werden die Konfigurationsoptionen für ein virtuelles Netzwerk (VNET) mit einer verwalteten Azure SQL-Datenbank-Instanz beschrieben.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: bonova, carlrab
manager: craigg
ms.date: 09/20/2018
ms.openlocfilehash: 9d3f867dad40017e8e97ec4f5e370533b018263c
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47181171"
---
# <a name="configure-a-vnet-for-azure-sql-database-managed-instance"></a>Konfigurieren eines VNET für eine verwaltete Azure SQL-Datenbank-Instanz

In einem [virtuellen Azure-Netzwerk (VNET)](../virtual-network/virtual-networks-overview.md) muss eine verwaltete Azure SQL-Datenbank-Instanz bereitgestellt werden. Diese Bereitstellung ermöglicht die folgenden Szenarien: 
- Direktes Herstellen einer Verbindung mit einer verwalteten Instanz von einem lokalen Netzwerk 
- Herstellen einer Verbindung zwischen einer verwalteten Instanz und einem Verbindungsserver oder einem anderen lokalen Datenspeicher 
- Herstellen einer Verbindung zwischen einer verwalteten Instanz und Azure-Ressourcen  

## <a name="plan"></a>Plan

Planen Sie die Bereitstellung einer verwalteten Instanz im virtuellen Netzwerk, indem Sie folgende Fragen beantworten: 
- Planen Sie, einzelne oder mehrere verwaltete Instanzen bereitzustellen? 

  Die Anzahl der verwalteten Instanzen bestimmt die Mindestgröße des Subnetzes, die Ihren verwalteten Instanzen zugeordnet wird. Weitere Informationen finden Sie unter [Ermitteln der Größe des Subnetzes für verwaltete Instanzen](#determine-the-size-of-subnet-for-managed-instances). 
- Müssen Sie Ihre verwaltete Instanz in einem bestehenden virtuellen Netzwerk bereitstellen, oder erstellen Sie ein neues Netzwerk? 

   Wenn Sie die Verwendung eines vorhandenen virtuellen Netzwerks beabsichtigen, müssen Sie diese Netzwerkkonfiguration ändern, um Ihre verwaltete Instanz einzubeziehen. Weitere Informationen finden Sie unter [Ändern eines vorhandenen virtuellen Netzwerks für die verwaltete Instanz](#modify-an-existing-virtual-network-for-managed-instances). 

   Wenn Sie vorhaben, ein neues virtuelles Netzwerk zu erstellen, lesen Sie [Erstellen eines neuen virtuellen Netzwerks für die verwaltete Instanz](#create-a-new-virtual-network-for-a-managed-instance).

## <a name="requirements"></a>Requirements (Anforderungen)

Um eine verwaltete Instanz zu erstellen, erstellen Sie ein dediziertes Subnetz (das Subnetz der verwalteten Instanz) innerhalb des virtuellen Netzwerks, das den folgenden Anforderungen entspricht:
- **Dediziertes Subnetz:** Das Subnetz der verwalteten Instanz darf mit keinem anderen Clouddienst verknüpft und kein Gatewaysubnetz sein. Sie können weder eine verwaltete Instanz in einem Subnetz erstellen, das andere Ressourcen als die verwaltete Instanz enthält, noch zu einem späteren Zeitpunkt Ressourcen im Subnetz hinzufügen.
- **Kompatible Netzwerksicherheitsgruppe (NSG)**: Eine NSG, die mit einem Subnetz einer verwalteten Instanz verbunden ist, muss Regeln in den folgenden Tabellen („Obligatorische Eingangssicherheitsregeln“ und „Obligatorische Ausgangssicherheitsregeln“) den Vorrang vor anderen Regeln geben. Mit NSGs können Sie den Zugriff auf den Datenendpunkt der verwalteten Instanz vollständig steuern, indem Sie den Datenverkehr über Port 1433 filtern. 
- **Kompatible benutzerdefinierte Routingtabelle**: Das Subnetz der verwalteten Instanz benötigt eine benutzerdefinierte Routingtabelle, der **0.0.0.0.0.0/0 mit dem nächsten Hop zum Internet** als obligatorische benutzerdefinierte Route zugewiesen ist. Darüber hinaus können Sie eine benutzerdefinierte Route hinzufügen, die Datenverkehr mit lokalen privaten IP-Bereichen als Ziel über ein virtuelles Netzwerkgateway oder ein virtuelles Netzwerkgerät leitet. 
- **Optionales benutzerdefiniertes DNS**: Wenn ein benutzerdefiniertes DNS im virtuellen Netzwerk angegeben ist, muss die IP-Adresse des rekursiven Azure-Resolvers (z.B. 168.63.129.16) der Liste hinzugefügt werden. Weitere Informationen finden Sie unter [Konfigurieren des benutzerdefinierten DNS](sql-database-managed-instance-custom-dns.md). Der benutzerdefinierte DNS-Server muss Hostnamen in den folgenden Domänen und deren Subdomänen auflösen können: *microsoft.com*, *windows.net*, *windows.com*, *msocsp.com*, *digicert.com*, *live.com*, *microsoftonline.com* und *microsoftonline-p.com*. 
- **Kein Dienstendpunkte**: Dem Subnetz der verwalteten Instanz darf kein Dienstendpunkt zugeordnet sein. Überprüfen Sie, ob die Option „Dienstendpunkte“ beim Erstellen des virtuellen Netzwerks auf „Deaktiviert“ festgelegt ist.
- **Ausreichende IP-Adressen**: Das Subnetz der verwalteten Instanz muss mindestens 16 IP-Adressen aufweisen. Die empfohlene Mindestanzahl sind 32 IP-Adressen. Weitere Informationen finden Sie unter [Ermitteln der Größe des Subnetzes für verwaltete Instanzen](#determine-the-size-of-subnet-for-managed-instances).

> [!IMPORTANT]
> Wenn das Zielsubnetz nicht alle genannten Anforderungen erfüllt, können Sie keine neue verwaltete Instanz bereitstellen. Wenn eine verwaltete Instanz erstellt wird, wird eine *Netzwerkabsichtsrichtlinie* auf das Subnetz angewendet, um nicht konforme Änderungen an der Netzwerkkonfiguration zu verhindern. Nachdem die letzte Instanz aus dem Subnetz entfernt wurde, wird auch die *Netzwerkabsichtsrichtlinie* entfernt.

### <a name="mandatory-inbound-security-rules"></a>Obligatorische Eingangssicherheitsregeln 

| NAME       |PORT                        |PROTOKOLL|QUELLE           |ZIEL|AKTION|
|------------|----------------------------|--------|-----------------|-----------|------|
|management  |9000, 9003, 1438, 1440, 1452|TCP     |Beliebig              |Beliebig        |ZULASSEN |
|mi_subnet   |Beliebig                         |Beliebig     |MI-SUBNETZ        |Beliebig        |ZULASSEN |
|health_probe|Beliebig                         |Beliebig     |AzureLoadBalancer|Beliebig        |ZULASSEN |

### <a name="mandatory-outbound-security-rules"></a>Obligatorische Ausgangssicherheitsregeln 

| NAME       |PORT          |PROTOKOLL|QUELLE           |ZIEL|AKTION|
|------------|--------------|--------|-----------------|-----------|------|
|management  |80, 443, 12000|TCP     |Beliebig              |Beliebig        |ZULASSEN |
|mi_subnet   |Beliebig           |Beliebig     |Beliebig              |MI-SUBNETZ  |ZULASSEN |

##  <a name="determine-the-size-of-subnet-for-managed-instances"></a>Ermitteln der Größe des Subnetzes für verwaltete Instanzen

Bei der Erstellung einer verwalteten Instanz ordnet Azure abhängig vom Tarif, den Sie bei der Bereitstellung ausgewählt haben, eine Reihe von virtuellen Computern zu. Da diese virtuellen Computer Ihrem Subnetz zugeordnet sind, benötigen sie IP-Adressen. Um bei regulären Vorgängen und bei der Wartung von Diensten Hochverfügbarkeit sicherzustellen, kann Azure zusätzliche virtuelle Computer zuordnen. Daher ist die Anzahl der erforderlichen IP-Adressen in einem Subnetz größer als die Anzahl der verwalteten Instanzen in diesem Subnetz. 

Prinzipiell benötigt eine verwaltete Instanz zwischen 16 und 256 IP-Adressen in einem Subnetz. Daher können Sie bei der Definition Ihrer Subnetz-IP-Adressbereiche die Subnetzmasken /28 bis /24 verwenden. 

> [!IMPORTANT]
> Als Größe für das Subnetz stellen 16 IP-Adressen das absolute Minimum dar, das nur beschränkte Möglichkeiten für eine weitere horizontale Hochskalierung der verwalteten Instanz bietet. Die Auswahl eines Subnetzes mit dem Präfix /27 oder darunter wird dringend empfohlen. 

Wenn Sie mehrere verwaltete Instanzen innerhalb des Subnetzes bereitstellen möchten und die Subnetzgröße optimieren müssen, führen Sie mithilfe der folgenden Parameter eine Berechnung durch: 

- Azure verwendet fünf IP-Adressen im Subnetz für den eigenen Bedarf. 
- Jede allgemeine Instanz benötigt zwei Adressen. 
- Jede unternehmenskritisch Instanz benötigt vier Adressen.

**Beispiel**: Sie planen, mit drei universelle und zwei unternehmenskritischen verwalteten Instanzen zu arbeiten. Dies bedeutet, dass Sie 5 + 3 * 2 + 2 * 4 = 19 IP-Adressen benötigen. Da IP-Adressbereiche in Zweierpotenzen definiert sind, benötigen Sie den IP-Adressbereich von 32 (2^5) IP-Adressen. Aus diesem Grund müssen Sie das Subnetz mit der Subnetzmaske /27 reservieren. 

> [!IMPORTANT]
> Die oben gezeigte Berechnung wird mit weiteren Verbesserungen demnächst veralten. 

## <a name="create-a-new-virtual-network-for-a-managed-instance"></a>Erstellen eines neuen virtuellen Netzwerks für eine verwaltete Instanz

Die einfachste Möglichkeit zum Erstellen und Konfigurieren eines virtuellen Netzwerks ist die Verwendung der Azure Resource Manager-Bereitstellungsvorlage.

1. Melden Sie sich beim Azure-Portal an.

2. Verwenden Sie die Schaltfläche **In Azure bereitstellen**, um das virtuelle Netzwerk in Azure-Cloud bereitzustellen:

  <a target="_blank" href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-sql-managed-instance-azure-environment%2Fazuredeploy.json" rel="noopener" data-linktype="external"> <img src="http://azuredeploy.net/deploybutton.png" data-linktype="external"> </a>

  Diese Schaltfläche öffnet ein Formular zum Konfigurieren der Netzwerkumgebung, in der Sie die verwaltete Instanz bereitstellen können.

  > [!Note]
  > Diese Azure Resource Manager-Vorlage stellt das virtuelle Netzwerk mit zwei Subnetzen bereit. Ein Subnetz namens **ManagedInstances** ist für verwaltete Instanzen reserviert und verfügt über eine vorkonfigurierte Routingtabelle, während das andere Subnetz namens **Standard** für andere Ressourcen verwendet wird, die auf die verwaltete Instanz zugreifen sollen (z. B. Azure Virtual Machines). Sie können das Subnetz **Standard** entfernen, wenn Sie es nicht benötigen.

3. Konfigurieren Sie die Netzwerkumgebung. Im folgenden Formular können Sie Parameter für Ihre Netzwerkumgebung konfigurieren:

![Azure-Netzwerk konfigurieren](./media/sql-database-managed-instance-vnet-configuration/create-mi-network-arm.png)

Sie können die Namen des VNETs und der Subnetze ändern und die Ihren Netzwerkressourcen zugeordneten IP-Adressbereiche anpassen. Sobald Sie auf die Schaltfläche „Kaufen“ klicken, erstellt und konfiguriert dieses Formular Ihre Umgebung. Wenn Sie keine zwei Subnetze benötigen, können Sie das Subnetz „Standard“ löschen. 

## <a name="modify-an-existing-virtual-network-for-managed-instances"></a>Ändern eines bestehenden virtuellen Netzwerks für verwaltete Instanzen 

In den Fragen und Antworten in diesem Abschnitt wird erläutert, wie eine verwaltete Instanz zu einem vorhandenen virtuellen Netzwerk hinzugefügt wird. 

**Verwenden Sie das klassische oder das Resource Manager-Bereitstellungsmodell für das bestehende virtuelle Netzwerk?** 

Sie können nur eine verwaltete Instanz in virtuellen Resource Manager-Netzwerken erstellen. 

**Möchten Sie ein neues Subnetz für verwaltete SQL-Instanzen erstellen oder ein vorhandenes verwenden?**

Wenn Sie ein neues Subnetz erstellen möchten, gehen Sie wie folgt vor: 

- Berechnen Sie die Subnetzgröße entsprechend der Richtlinien im Abschnitt [Ermitteln der Größe des Subnetzes für verwaltete Instanzen](#determine-the-size-of-subnet-for-managed-instances).
- Führen Sie die Schritte unter [Hinzufügen, Ändern oder Löschen von Subnetzen virtueller Netzwerke](../virtual-network/virtual-network-manage-subnet.md) durch. 
- Erstellen Sie eine Routingtabelle, die einen einzelnen Eintrag mit dem nächsten Hop **0.0.0.0/0** enthält, und verknüpfen Sie sie mit dem Subnetz für die verwaltete Instanz.  

Wenn Sie eine verwaltete Instanz in einem vorhandenen Subnetz erstellen möchten, empfehlen wir das folgende PowerShell-Skript zum Vorbereiten des Subnetzes.
```powershell
$scriptUrlBase = 'https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/manage/azure-sql-db-managed-instance/prepare-subnet'

$parameters = @{
    subscriptionId = '<subscriptionId>'
    resourceGroupName = '<resourceGroupName>'
    virtualNetworkName = '<virtualNetworkName>'
    subnetName = '<subnetName>'
    }

Invoke-Command -ScriptBlock ([Scriptblock]::Create((iwr ($scriptUrlBase+'/prepareSubnet.ps1?t='+ [DateTime]::Now.Ticks)).Content)) -ArgumentList $parameters
```
Die Subnetzvorbereitung erfolgt in drei einfachen Schritten:

- Überprüfen: Das ausgewählte virtuelle Netzwerk und das Subnetz werden im Hinblick auf die Netzwerkanforderungen der verwalteten Instanz überprüft.
- Bestätigen: Dem Benutzer wird eine Reihe von Änderungen angezeigt, mit denen das Subnetz für die Bereitstellung der verwalteten Instanz vorbereitet werden soll und die er bestätigen muss.
- Vorbereiten: Das virtuelle Netzwerk und das Subnetz werden ordnungsgemäß konfiguriert.

**Haben Sie einen benutzerdefinierten DNS-Server konfiguriert?** 

Falls ja, lesen Sie den Artikel [Konfigurieren eines benutzerdefinierten DNS](sql-database-managed-instance-custom-dns.md). 

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht finden Sie unter [Was ist eine verwaltete Instanz?](sql-database-managed-instance.md).
- Ein Tutorial zum Erstellen eines VNET, Erstellen einer verwalteten Instanz und Wiederherstellen einer Datenbank anhand einer Datenbanksicherung finden Sie unter [Erstellen einer verwalteten Azure SQL-Datenbank-Instanz](sql-database-managed-instance-get-started.md).
- Bei Problemen im Zusammenhang mit dem DNS lesen Sie [Konfigurieren eines benutzerdefinierten DNS](sql-database-managed-instance-custom-dns.md).
