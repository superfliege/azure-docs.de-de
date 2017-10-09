---
title: "Sicherheitsüberlegungen in Azure Data Factory | Microsoft-Dokumentation"
description: "Beschreibt die grundlegende Sicherheitsinfrastruktur, die von Datenverschiebungsdiensten in Azure Data Factory verwendet wird, um Ihre Daten zu schützen."
services: data-factory
documentationcenter: 
author: nabhishek
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/24/2017
ms.author: abnarain
ms.translationtype: HT
ms.sourcegitcommit: cb9130243bdc94ce58d6dfec3b96eb963cdaafb0
ms.openlocfilehash: 00673cac31eb25bb8dd8228d6a36617c3812f5db
ms.contentlocale: de-de
ms.lasthandoff: 09/26/2017

---

# <a name="azure-data-factory---security-considerations-for-data-movement"></a>Azure Data Factory – Sicherheitsüberlegungen für Datenverschiebung
In diesem Artikel ist die grundlegenden Sicherheitsinfrastruktur beschrieben, die von Datenverschiebungsdiensten in Azure Data Factory verwendet wird, um Ihre Daten zu schützen. Azure Data Factory-Verwaltungsressourcen setzen auf der Azure Sicherheitsinfrastruktur auf und nutzen alle möglichen Sicherheitsmaßnahmen, die von Azure bereitgestellt werden.

In einer Data Factory-Lösung erstellen Sie mindestens eine [Pipeline](concepts-pipelines-activities.md). Bei einer Pipeline handelt es sich um eine logische Gruppierung von Aktivitäten, die zusammen eine Aufgabe bilden. Diese Pipelines befinden sich in der Region, in der die Data Factory erstellt wurde. 

Obwohl Data Factory nur in den Regionen **USA, Osten** und **USA, Osten 2** (Version 2, Vorschau) verfügbar ist, ist der Datenverschiebungsdienst [global in verschiedenen Regionen](concepts-integration-runtime.md#azure-ir) verfügbar. Wenn der Datenverschiebungsdienst noch nicht in dieser Region bereitgestellt wurde, stellt der Data Factory-Dienst sicher, dass Daten einen geografischen Bereich/eine geografische Region nicht verlassen, es sei denn, Sie weisen den Dienst ausdrücklich an, eine alternative Region zu verwenden. 

Azure Data Factory selbst speichert keine Daten mit Ausnahme für verknüpfte Dienstanmeldeinformationen für Clouddatenspeicher, die mit Zertifikaten verschlüsselt sind. Der Dienst ermöglicht das Erstellen von datengesteuerten Workflows, um die Verschiebung von Daten zwischen [unterstützten Datenspeichern](copy-activity-overview.md#supported-data-stores-and-formats) und die Verarbeitung von Daten mithilfe von [Compute Services](compute-linked-services.md) in anderen Regionen oder in einer lokalen Umgebung zu orchestrieren. Außerdem können Sie mithilfe von SDKs und Azure Monitor Workflows überwachen und verwalten.

Datenverschiebung mit Azure Data Factory ist **zertifiziert** für:
-   [HIPAA/HITECH](https://www.microsoft.com/en-us/trustcenter/Compliance/HIPAA) 
-   [ISO/IEC 27001](https://www.microsoft.com/en-us/trustcenter/Compliance/ISO-IEC-27001)  
-   [ISO/IEC 27018](https://www.microsoft.com/en-us/trustcenter/Compliance/ISO-IEC-27018)
-   [CSA STAR](https://www.microsoft.com/en-us/trustcenter/Compliance/CSA-STAR-Certification)

Informationen zur Compliance von Azure und zur eigenständigen Sicherung der Azure-Infrastruktur finden Sie im [Microsoft Trust Center](https://www.microsoft.com/TrustCenter/default.aspx). 

In diesem Artikel werden Sicherheitsüberlegungen zu den beiden folgenden Datenverschiebungsszenarien erläutert: 

- **Cloudszenario** In diesem Szenario sind sowohl die Quelle als auch das Ziel über das Internet öffentlich zugänglich. Dazu gehören verwaltete Cloudspeicherdienste wie Azure Storage, Azure SQL Data Warehouse, Azure SQL-Datenbank, Azure Data Lake Store, Amazon S3, Amazon Redshift, SaaS-Dienste wie Salesforce und Webprotokolle wie FTP und OData. Eine vollständige Liste der unterstützten Datenquellen finden Sie [hier](copy-activity-overview.md#supported-data-stores-and-formats).
- **Hybridszenario** In diesem Szenario befindet sich entweder die Quelle oder das Ziel hinter einer Firewall oder in einem lokalen Unternehmensnetzwerk, oder der Datenspeicher befindet sich in einem privaten Netzwerk/virtuellen Netzwerk (meist die Quelle) und ist nicht öffentlich zugänglich. Zu diesem Szenario zählen auch Datenbankserver, die auf virtuellen Computern gehostet werden.

## <a name="cloud-scenarios"></a>Cloudszenarien
###<a name="securing-data-store-credentials"></a>Schützen von Datenspeicher-Anmeldeinformationen
- Speichern Sie verschlüsselte Anmeldeinformationen im verwalteten Speicher von Azure Data Factory.

   Azure Data Factory schützt Ihre Datenspeicher-Anmeldeinformationen dadurch, dass sie **verschlüsselt** werden, wozu **von Microsoft verwaltete Zertifikate** verwendet werden. Diese Zertifikate werden alle **zwei Jahre** ausgetauscht (wozu Erneuerung des Zertifikats und Migration von Anmeldeinformationen gehören). Diese verschlüsselten Anmeldeinformationen werden sicher in einem **von Azure Data Factory-Verwaltungsdienste verwalteten Azure Storage** gespeichert. Weitere Informationen zur Azure Storage-Sicherheit finden Sie unter [Übersicht über die Sicherheit von Azure Storage](../security/security-storage-overview.md).
- Speichern von Anmeldeinformationen in Azure Key Vault 

   Sie können jetzt die Anmeldeinformationen des Datenspeichers in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) speichern. Während der Ausführung einer Aktivität können Sie sie dann von Azure Data Factory abrufen lassen. Weitere Informationen finden Sie unter [Speichern von Anmeldeinformationen in Azure Key Vault](store-credentials-in-key-vault.md).

   > [!NOTE]
   > Derzeit unterstützt nur der [Dynamics-Connector](connector-dynamics-crm-office-365.md) dieses Feature. 

### <a name="data-encryption-in-transit"></a>Datenverschlüsselung während der Übertragung
Wenn der Clouddatenspeicher HTTPS oder TLS unterstützt, erfolgen alle Datenübertragungen zwischen Datenverschiebungsdiensten in Data Factory und einem Clouddatenspeicher über einen sicheren Kanal (HTTPS oder TLS).

> [!NOTE]
> Für alle Verbindungen mit **Azure SQL-Datenbank** und **Azure SQL Data Warehouse** ist eine Verschlüsselung (SSL/TLS) erforderlich, solange Daten in die und aus der Datenbank übertragen werden. Wenn Sie eine Pipeline mit JSON erstellen, fügen Sie die **encryption**-Eigenschaft zur Verbindungszeichenfolge (**connection string**) hinzu, und legen Sie die Eigenschaft auf **true** fest. Für **Azure Storage** können Sie **HTTPS** in der Verbindungszeichenfolge verwenden.

### <a name="data-encryption-at-rest"></a>Datenverschlüsselung ruhender Daten
Einige Datenspeicher unterstützen die Verschlüsselung von ruhenden Daten. Es empfiehlt sich, dass Sie einen Datenverschlüsselungsmechanismus für solche Datenspeicher aktivieren. 

#### <a name="azure-sql-data-warehouse"></a>Azure SQL Data Warehouse
Transparent Data Encryption (TDE) in Azure SQL Data Warehouse bietet Schutz vor der Bedrohung durch schädliche Aktivitäten, indem die ruhenden Daten in Echtzeit ver- und entschlüsselt werden. Dieses Verhalten ist für den Client transparent. Weitere Informationen finden Sie unter [Sichern einer Datenbank in SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md).

#### <a name="azure-sql-database"></a>Azure SQL-Datenbank
Azure SQL-Datenbank unterstützt auch Transparent Data Encryption (TDE), die Schutz vor der Bedrohung durch schädliche Aktivitäten bietet. Hierzu werden die Daten in Echtzeit ver- und entschlüsselt, ohne dass Änderungen der Anwendung erforderlich sind. Dieses Verhalten ist für den Client transparent. Weitere Informationen finden Sie unter [Transparent Data Encryption mit Azure SQL-Datenbank](/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database). 

#### <a name="azure-data-lake-store"></a>Azure Data Lake Store
Azure Data Lake Store bietet auch eine Verschlüsselung für Daten, die im Konto gespeichert sind. Wenn diese Option aktiviert ist, verschlüsselt Data Lake Store Daten automatisch vor der dauerhaften Speicherung und entschlüsselt Daten vor dem Abrufen, sodass der Vorgang für den Client, der auf die Daten zugreift, transparent ist. Weitere Informationen finden Sie unter [Sicherheit in Azure Data Lake Store](../data-lake-store/data-lake-store-security-overview.md). 

#### <a name="azure-blob-storage-and-azure-table-storage"></a>Azure Blob Storage und Azure Table Storage
Azure Blob Storage und Azure Table Storage unterstützen die Speicherdienstverschlüsselung (Storage Service Encryption, SSE), bei der Ihre Daten vor der Weitergabe an den Speicher automatisch verschlüsselt und vor dem Abrufen entschlüsselt werden. Weitere Informationen finden Sie unter [Azure Storage Service Encryption für ruhende Daten](../storage/common/storage-service-encryption.md).

#### <a name="amazon-s3"></a>Amazon S3
Amazon S3 unterstützt die Client- und Serververschlüsselung von ruhenden Daten. Weitere Informationen finden Sie unter [Schutz von Daten mittels Verschlüsselung](http://docs.aws.amazon.com/AmazonS3/latest/dev/UsingEncryption.html). Derzeit bietet Data Factory keine Unterstützung für Amazon S3 in einer Virtual Private Cloud (VPC).

#### <a name="amazon-redshift"></a>Amazon Redshift
Amazon Redshift unterstützt die Clusterverschlüsselung für ruhende Daten. Weitere Informationen finden Sie unter [Amazon Redshift-Datenbankverschlüsselung](http://docs.aws.amazon.com/redshift/latest/mgmt/working-with-db-encryption.html). Derzeit bietet Data Factory keine Unterstützung für Amazon Redshift in einer VPC. 

#### <a name="salesforce"></a>Salesforce
Salesforce unterstützt Shield Platform Encryption, die eine Verschlüsselung aller Dateien, Anlagen und benutzerdefinierten Felder ermöglicht. Weitere Informationen finden Sie unter [Grundlegendes zum OAuth-Webserver-Authentifizierungsfluss](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_understanding_web_server_oauth_flow.htm).  

## <a name="hybrid-scenarios-using-self-hosted-integration-runtime"></a>Hybridszenarien (unter Verwendung der selbstgehosteten Integrationslaufzeit)
Hybridszenarien erfordern, dass die selbstgehostete Integrationslaufzeit in einem lokalen Netzwerk oder einem virtuellen Netzwerk (Azure) oder einer virtuellen Private Cloud (Amazon) installiert wird. Die selbstgehostete Integrationslaufzeit muss auf die lokalen Datenspeicher zugreifen können. Weitere Informationen zur selbstgehosteten Integrationslaufzeit finden Sie unter [Selbstgehostete Integrationslaufzeit](create-self-hosted-integration-runtime.md). 

![Kanäle der selbstgehosteten Integrationslaufzeit](media/data-movement-security-considerations/data-management-gateway-channels.png)

Der **Befehlskanal** ermöglicht Kommunikation zwischen Datenverschiebungsdiensten in Data Factory und der selbstgehosteten Integrationslaufzeit. Die Kommunikation enthält die Informationen, die sich auf die Aktivität beziehen. Der Datenkanal wird dazu verwendet, Daten zwischen lokalen Datenspeichern und Clouddatenspeichern zu übertragen.    

### <a name="on-premises-data-store-credentials"></a>Anmeldeinformationen für lokale Datenspeicher
Die Anmeldeinformationen für Ihre lokalen Datenspeicher sind immer verschlüsselt und gespeichert. Sie können entweder lokal auf dem Computer der selbstgehosteten Integrationslaufzeit oder im verwalteten Speicher von Azure Data Factory gespeichert werden (genau wie Anmeldeinformationen für den Cloudstore). 

1. Sie können **Anmeldeinformationen lokal speichern**. Wenn Sie Anmeldeinformationen lokal auf der selbstgehosteten Integrationslaufzeit verschlüsseln und speichern möchten, führen Sie die Schritte in [Verschlüsseln von Anmeldeinformationen für die selbstgehostete Integrationslaufzeit](encrypt-credentials-self-hosted-integration-runtime.md) aus. Diese Option wird von allen Connectors unterstützt. Die selbstgehostete Integrationslaufzeit verwendet Windows-[DPAPI](https://msdn.microsoft.com/library/ms995355.aspx) zum Verschlüsseln der vertraulichen Daten/Anmeldeinformationen. 

   Verwenden Sie das **New-AzureRmDataFactoryV2LinkedServiceEncryptCredential**-Cmdlet, um Anmeldeinformationen des verknüpften Diensts bzw. vertrauliche Details im verknüpften Dienst zu verschlüsseln. Anschließend können Sie das zurückgegebene JSON (mit dem **EncryptedCredential**-Element in **connectionString**) verwenden, um mit dem **Set-AzureRmDataFactoryV2LinkedSevrice**-Cmdlet einen verknüpften Dienst zu erstellen.  

2. Wenn Sie das **New-AzureRmDataFactoryV2LinkedServiceEncryptCredential**-Cmdlet nicht gemäß der obigen Beschreibung verwenden und stattdessen das **Set-AzureRmDataFactoryV2LinkedSevrice**-Cmdlet direkt mit den im JSON integrierten Verbindungszeichenfolgen/Anmeldeinformationen verwenden, wird der verknüpfte Dienst **verschlüsselt und im verwalteten Speicher von Azure Data Factory gespeichert**. Diese vertraulichen Informationen werden weiterhin mithilfe eines Zertifikats verschlüsselt, und diese Zertifikate werden von Microsoft verwaltet.



#### <a name="ports-used-during-encrypting-linked-service-on-self-hosted-integration-runtime"></a>Während der Verschlüsselung des verknüpften Diensts auf der selbstgehosteten Integrationslaufzeit verwendete Ports
Standardmäßig verwendet PowerShell den Port **8050** auf dem Computer mit der selbstgehosteten Integrationslaufzeit für die sichere Kommunikation. Dieser Port kann bei Bedarf geändert werden.  

![HTTPS-Port für das Gateway](media/data-movement-security-considerations/https-port-for-gateway.png)

 


### <a name="encryption-in-transit"></a>Verschlüsselung während der Übertragung
Alle Datenübertragungen erfolgen über den sicheren Kanal **HTTPS** und **TLS über TCP**, um Man-in-the-Middle-Angriffe während der Kommunikation mit Azure-Diensten zu verhindern.

Sie können auch [IPSec-VPN](../vpn-gateway/vpn-gateway-about-vpn-devices.md) oder [ExpressRoute](../expressroute/expressroute-introduction.md) verwenden, um den Kommunikationskanal zwischen Ihrem lokalen Netzwerk und Azure zusätzlich zu schützen.

Das virtuelle Netzwerk ist eine logische Darstellung Ihres Netzwerks in der Cloud. Sie können ein lokales Netzwerk mit Ihrem virtuellen Azure-Netzwerk (VNet) verbinden, indem Sie IPSec-VPN (Standort-zu-Standort) oder ExpressRoute (privates Peering) einrichten.     

In der folgenden Tabelle sind die Empfehlungen für die Konfiguration von Netzwerk und selbstgehosteter Integrationslaufzeit zusammengefasst, die sich aus verschiedenen Kombinationen von Quell- und Zielstandort für hybride Datenverschiebung ergeben.

| Quelle      | Ziel                              | Network Configuration                    | Setup der Integrationslaufzeit                |
| ----------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| Lokal | Virtuelle Computer und Clouddienste, die in virtuellen Netzwerken bereitgestellt werden | IPSec-VPN (Punkt-zu-Standort oder Standort-zu-Standort) | Die selbstgehostete Integrationslaufzeit kann entweder lokal oder auf einem virtuellen Azure-Computer in VNet installiert sein |
| Lokal | Virtuelle Computer und Clouddienste, die in virtuellen Netzwerken bereitgestellt werden | ExpressRoute (privates Peering)           | Die selbstgehostete Integrationslaufzeit kann entweder lokal oder auf einem virtuellen Azure-Computer in VNet installiert sein |
| Lokal | Azure-basierte Dienste, die einen öffentlichen Endpunkt haben | ExpressRoute (öffentliches Peering)            | Die selbstgehostete Integrationslaufzeit muss lokal installiert sein |

Die folgenden Abbildungen veranschaulichen die Verwendung der selbstgehosteten Integrationslaufzeit zum Verschieben von Daten zwischen einer lokalen Datenbank und Azure-Diensten über ExpressRoute und IPSec-VPN (mit virtuellem Netzwerk):

**ExpressRoute:**

![ExpressRoute mit Gateway verwenden](media/data-movement-security-considerations/express-route-for-gateway.png) 

**IPSec-VPN:**

![IPSec-VPN mit Gateway](media/data-movement-security-considerations/ipsec-vpn-for-gateway.png)

### <a name="firewall-configurations-and-whitelisting-ip-address-self-hosted-integration-runtime"></a>Firewallkonfigurationen und Whitelist-IP-Adresse (selbstgehostete Integrationslaufzeit)

#### <a name="firewall-requirements-for-on-premisesprivate-network"></a>Firewallanforderungen für lokales/privates Netzwerk  
In einem Unternehmen wird eine **Unternehmensfirewall** auf dem zentralen Router der Organisation ausgeführt. Außerdem wird die **Windows-Firewall** als Daemon auf dem lokalen Computer ausgeführt, auf dem die selbstgehostete Integrationslaufzeit installiert ist. 

Die folgende Tabelle enthält die Anforderungen für **ausgehende Ports** und die Domänenanforderungen für die **Unternehmensfirewall**.

| Domänennamen                  | Ausgehende Ports | Beschreibung                              |
| ----------------------------- | -------------- | ---------------------------------------- |
| `*.servicebus.windows.net`    | 443, 80        | Erforderlich für die selbstgehostete Integrationslaufzeit, um Verbindungen mit Datenverschiebungsdiensten in Data Factory herzustellen. |
| `*.core.windows.net`          | 443            | Wird von der selbstgehosteten Integrationslaufzeit verwendet, um Verbindungen mit dem Azure Storage-Konto herzustellen, wenn Sie das Feature [gestaffeltes Kopieren](copy-activity-performance.md#staged-copy) verwenden. |
| `*.frontend.clouddatahub.net` | 443            | Erforderlich für die selbstgehostete Integrationslaufzeit, um Verbindungen mit dem Azure Data Factory-Dienst herzustellen. |
| `*.database.windows.net`      | 1433           | (OPTIONAL) Erforderlich, wenn Ihr Ziel Azure SQL-Datenbank/Azure SQL Data Warehouse ist. Verwenden Sie das Feature „gestaffeltes Kopieren“ um Daten nach Azure SQL-Datenbank/Azure SQL Data Warehouse zu kopieren, ohne den Port 1433 zu öffnen. |
| `*.azuredatalakestore.net`    | 443            | (OPTIONAL) Erforderlich, wenn Ihr Ziel Azure Data Lake Store ist. |

> [!NOTE] 
> Möglicherweise müssen Sie Ports/Whitelistdomänen auf Ebene der Unternehmensfirewall verwalten, wie dies für die jeweiligen Datenquellen erforderlich ist. In dieser Tabelle werden nur Azure SQL-Datenbank, Azure SQL Data Warehouse und Azure Data Lake Store als Beispiele verwendet.   

Die folgende Tabelle enthält die Anforderungen für **eingehende Ports** für die **Windows-Firewall**.

| Eingehende Ports | Beschreibung                              |
| ------------- | ---------------------------------------- |
| 8050 (TCP)    | Vom PowerShell-Cmdlet für die Verschlüsselung erforderlich, wie in [Verschlüsseln von Anmeldeinformationen für selbstgehostete Integrationslaufzeit](encrypt-credentials-self-hosted-integration-runtime.md) bzw. für die Anwendung zur Anmeldeinformationsverwaltung beschrieben, um Anmeldeinformationen für die lokalen Datenspeicher auf der selbstgehosteten Integrationslaufzeit sicher festzulegen. |

![Gatewayportanforderungen](media\data-movement-security-considerations/gateway-port-requirements.png) 

#### <a name="ip-configurationswhitelisting-in-data-store"></a>IP-Konfigurationen/Whitelists in Datenspeicher
Einige Datenspeicher in der Cloud erfordern auch, dass die IP-Adresse des Computers, über den auf sie zugegriffen wird, in der Whitelist enthalten sind. Stellen Sie sicher, dass die IP-Adresse des Computers der selbstgehosteten Integrationslaufzeit in der Firewall ordnungsgemäß in der Whitelist enthalten bzw. ordnungsgemäß konfiguriert ist.

Die folgenden Clouddatenspeicher erfordern, dass die IP-Adresse des Computers der selbstgehosteten Integrationslaufzeit in der Whitelist enthalten ist. Einige dieser Datenspeicher erfordern standardmäßig möglicherweise nicht, dass die IP-Adresse in der Whitelist enthalten ist. 

- [Azure SQL-Datenbank](../sql-database/sql-database-firewall-configure.md) 
- [Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md#create-a-server-level-firewall-rule-in-the-azure-portal)
- [Azure Data Lake-Speicher](../data-lake-store/data-lake-store-secure-data.md#set-ip-address-range-for-data-access)
- [Azure Cosmos DB](../cosmos-db/firewall-support.md)
- [Amazon Redshift](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) 

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

**Frage:** Kann die selbstgehostete Integrationslaufzeit für unterschiedliche Data Factorys gemeinsam genutzt werden?
**Antwort:** Dieses Feature wird noch nicht unterstützt. Wir arbeiten mit Hochdruck daran.

**Frage:** Welche Portanforderungen sind erforderlich, damit die selbstgehostete Integrationslaufzeit funktioniert?
**Antwort:** Die selbstgehostete Integrationslaufzeit erstellt HTTP-basierte Verbindungen in das offene Internet. Die **ausgehenden Ports 443 und 80** müssen geöffnet sein, damit die selbstgehostete Integrationslaufzeit diese Verbindung herstellen kann. Öffnen Sie den **eingehenden Port 8050** nur auf Computerebene (nicht auf Ebene der Unternehmensfirewall) für die Anwendung „Anmeldeinformationsverwaltung“. Wenn Azure SQL-Datenbank oder Azure SQL Data Warehouse als Quelle/Ziel verwendet wird, müssen Sie auch den Port **1433** öffnen. Weitere Informationen finden Sie im Abschnitt [IP-Konfigurationen/Whitelists in Datenspeicher](#firewall-configurations-and-whitelisting-ip-address-of gateway). 


## <a name="next-steps"></a>Nächste Schritte
Informationen zur Leistung der Kopieraktivität finden Sie im [Handbuch zur Leistung und Optimierung der Kopieraktivität](copy-activity-performance.md).

 

