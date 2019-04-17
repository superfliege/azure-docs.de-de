---
title: Entwicklerhandbuch für Azure China 21Vianet | Microsoft-Dokumentation
description: Microsoft bietet verschiedene Tools, die Entwicklern beim Erstellen und Bereitstellen von Cloudanwendungen für die globale Azure-Umgebung und Azure China 21Vianet helfen. Erfahren Sie, welche Dienste und Features sowohl für die globale Azure -Umgebung als auch für Azure China verfügbar sind und auch welche Features in China möglicherweise nicht verfügbar sind.
services: china
cloud: na
documentationcenter: na
author: v-wimarc
manager: edprice
ms.assetid: na
ms.service: china
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/29/2017
ms.author: v-wimarc
ms.openlocfilehash: b48add54e83eb2d30b40faf64bc1b5c2c769f237
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/12/2019
ms.locfileid: "59521676"
---
# <a name="developer-guide-for-azure-china-21vianet"></a>Entwicklerhandbuch für Azure China 21Vianet
Microsoft bietet verschiedene Tools, die Entwicklern beim Erstellen und Bereitstellen von Cloudanwendungen für die globalen Microsoft Azure-Dienste (die globale Azure-Umgebung) und Microsoft Azure in der Umgebung von 21Vianet (Azure China 21Vianet) helfen. Im Großteil der derzeit verfügbaren technischen Inhalte, z.B. im [Azure-Dokumentationscenter](https://azure.microsoft.com/documentation/), wird davon ausgegangen, dass Anwendungen für die globale Azure-Plattform entwickelt werden. Daher ist es wichtig zu erfahren, welche [Dienste und Features](https://www.azure.cn/home/features/products-by-region) in Microsoft Azure China 21Vianet verfügbar sind.

Wenn Sie sich auf öffentlich zugängliche Inhalte für globale Azure-Dienste beziehen, stellen Sie sicher, dass Sie die Schritte oder den Beispielcode anpassen, der Einstellungen für globale Azure-Dienste festlegt. Passen Sie beispielsweise die [Azure-Dienstendpunkte](#check-endpoints-in-azure) an.

## <a name="migrate-existing-applications-and-workloads"></a>Migrieren vorhandener Anwendungen und Workloads
Die Migration einer Anwendung oder Workload, die an anderer Stelle gehostet wird, erfordert Zeit und Planung. So erfolgt die Migration zu Microsoft Azure China 21Vianet
- [Verschieben Sie virtuelle Computer](/azure/china/china-how-to-rehost) aus dem klassischen Azure Service Manager (ASM) in Azure Resource Manager.
- [Gestalten Sie eine bestehende Anwendung, die von einem anderen Cloudanbieter gehostet wird, um](/azure/china/china-how-to-refactor), damit eine bessere und schnellere Migration möglich wird. 
- [Weisen Sie einer bestehenden Anwendung, die in der globalen Azure-Umgebung gehostet wird, einen neuen Host zu](/azure/china/china-how-to-rehost).
- [Führen Sie mit dem Global Connection Toolkit in Ihrer Testumgebung eine Testmigration durch](https://github.com/Azure/AzureGlobalConnectionToolkit), um die Migrationsrisiken zu minimieren.

## <a name="develop-for-azure-users"></a>Entwickeln für Azure-Benutzer
Wenn Sie an der Entwicklung von Clouddiensten für Benutzer in anderen Regionen gewöhnt sind, stellen Sie sicher, dass Sie die folgenden Erwartungen der Benutzer in China berücksichtigen:
- **Oberste Priorität für Mobilgeräte:** Mobile Geräte und nicht PCs gelten als Grundpfeiler der Onlinewelt. Stellen Sie sicher, dass Ihre Entwicklungsstrategie auf Mobilität ausgerichtet ist.
- **Scannen von QR-Codes:** Websites, Printwerbung, Visitenkarten und andere Medien enthalten häufig QR-Codes. Fügen Sie QR-Codes in die Kopf- und Fußzeile Ihrer Website ein, damit Besucher die mobile Version der Website schnell auf ihr Smartphone laden können.
- **Lokalisierung von Inhalten:** Lokalisieren ist weit mehr als nur das Übersetzen von Inhalten. Nehmen Sie sich Zeit, das digitale Umfeld Ihres Zielmarkts und die kulturellen Folgen Ihrer Geschäftsentscheidungen zu verstehen, und passen Sie Ihre Inhalte dann entsprechend an. 

## <a name="use-social-sites-and-media-services-in-china"></a>Verwenden sozialer Sites und Medien in China
In der westlichen Welt gebräuchliche Websites und Dienste sozialer Medien werden in China möglicherweise blockiert. Beachten Sie beim Betrieb einer Webpräsenz in China Folgendes:
- Vermeiden Sie auf dem Front-End Ihrer Website Verbindungen mit Google-Diensten. Google und alle seine Dienste werden in China blockiert. Daher muss Ihre Website möglicherweise auf Google-Dienste verzichten, um ein optimales Ergebnis zu bieten. Ersetzen Sie beispielsweise Google Maps durch Baidu Maps, und verwenden Sie selbst gehostete Schriftarten anstelle von Google-Schriftarten.
- Betten Sie keine YouTube- oder Vimeo-Videos ein. Beide Dienste werden in China blockiert. Hosten Sie Ihr Video lokal oder auf chinesischen Websites zum Hosten von Videos, wie z.B. Youku, Qiyi, Tudou, oder nutzen Sie Azure Media Services. Optimieren Sie Ihre Website für Baidu, die am häufigsten verwendete Suchmaschine in China, mithilfe eines Überprüfungstools für die Suchmaschinenoptimierung.
- Erstellen Sie eine Präsenz in einem für China spezifischen sozialen Netzwerk. Weltweit beliebte soziale Netzwerke wie Facebook, Twitter und Instagram werden blockiert. Entwickeln Sie eine speziell auf die sozialen Netzwerke in China wie WeChat und Sina Weibo zugeschnittene Marketingstrategie. Azure bietet derzeit keine lokale Integration in soziale Netzwerke an (d.h. Identitätsanbieter für soziale Medien).

## <a name="check-endpoints-in-azure"></a>Überprüfen von Endpunkten in Azure
Microsoft Azure China 21Vianet unterscheidet sich von der globalen Azure-Umgebung, weshalb alle Endpunkte von Azure-Diensten in globalen Azure-Quellen, wie z.B. Beispielcode oder veröffentlichte Dokumentation, entsprechend angepasst werden müssen. 

Die folgende Tabelle zeigt die zu ändernden Endpunkte. 

Weitere Informationen:
- [Hinweise für Entwickler zu Azure in Anwendungen für China](https://msdn.microsoft.com/library/azure/dn578439.aspx)
- [IP-Bereiche für Azure-Rechenzentren in China](https://www.microsoft.com/en-us/download/confirmation.aspx?id=57062) 
- [Anleitung für Entwickler](https://www.azure.cn/documentation/articles/developerdifferences/#dev-guide) (auf Chinesisch)

| Dienstkategorie | URI der globalen Azure-Umgebung | Azure-URI (in China) |
|-|-|-|
| Azure (im Allgemeinen) | \*.windows.net | \*.chinacloudapi.cn |
| Azure Compute | \*.cloudapp.net | \*.chinacloudapp.cn |
| Azure-Speicher | \*.blob.core.windows.net \*.queue.core.windows.net \*.table.core.windows.net | \*.blob.core.chinacloudapi.cn \*.queue.core.chinacloudapi.cn \*.table.core.chinacloudapi.cn |
| Azure-Dienstverwaltung | https://management.core.windows.net | [https://management.core.chinacloudapi.cn](https://management.core.chinacloudapi.cn/) |
| Azure Resource Manager | [https://management.azure.com](https://management.azure.com/) | [https://management.chinacloudapi.cn](https://management.chinacloudapi.cn/) |
| Azure-Verwaltungsportal | [https://portal.azure.com](https://portal.azure.com/) | [https://portal.azure.cn](https://portal.azure.cn/) |
| SQL-Datenbank | \*.database.windows.net | \*.database.chinacloudapi.cn |
| Verwaltungs-API für Azure SQL-Datenbank | [https://management.database.windows.net](https://management.database.windows.net/) | [https://management.database.chinacloudapi.cn](https://management.database.chinacloudapi.cn/) |
| Azure-Servicebus | \*.servicebus.windows.net | \*.servicebus.chinacloudapi.cn |
| Azure Access Control Service | \*.accesscontrol.windows.net | \*.accesscontrol.chinacloudapi.cn |
| Azure HDInsight | \*.azurehdinsight.net | \*.azurehdinsight.cn |
| Endpunkt des Import/Export-Diensts für SQL-Datenbank | |  1. China, Osten[https://sh1prod-dacsvc.chinacloudapp.cn/dacwebservice.svc](https://sh1prod-dacsvc.chinacloudapp.cn/dacwebservice.svc) <br>2. China, Norden[https://bj1prod-dacsvc.chinacloudapp.cn/dacwebservice.svc](https://bj1prod-dacsvc.chinacloudapp.cn/dacwebservice.svc) |
| MySQL PaaS | | \*.mysqldb.chinacloudapi.cn |
| Azure Service Fabric-Cluster | \*.cloudapp.azure.com | \*.chinaeast.chinacloudapp.cn |
| Azure Active Directory (AD) | \*.onmicrosoft.com | \*.partner.onmschina.cn |
| Azure AD-Anmeldung | [https://login.microsoftonline.com](https://login.windows.net/) | [https://login.partner.microsoftonline.cn](https://login.chinacloudapi.cn/) |
| Azure AD Graph-API | [https://graph.windows.net](https://graph.windows.net/) | [https://graph.chinacloudapi.cn](https://graph.chinacloudapi.cn/) |
| Microsoft Graph | [https://graph.microsoft.com](https://graph.microsoft.com/) | [https://microsoftgraph.chinacloudapi.cn](https://microsoftgraph.chinacloudapi.cn/) |
| Cognitive Services | <https://api.projectoxford.ai/face/v1.0> | <https://api.cognitive.azure.cn/face/v1.0> |
| Azure Key Vault-API | \*.vault.azure.net | \*.vault.azure.cn |
| Mit PowerShell anmelden: <br>– Klassisches Azure <br>– Azure Resource Manager <br>– Azure AD| – Add-AzureAccount<br>– Connect-AzureRmAccount <br> – Connect-msolservice |  – Add-AzureAccount -Environment AzureChinaCloud <br> – Connect-AzureRmAccount -Environment AzureChinaCloud <br>– Connect-msolservice -AzureEnvironment AzureChinaCloud |

## <a name="next-steps"></a>Nächste Schritte
- [Anleitung für Entwickler](https://www.azure.cn/documentation/articles/developerdifferences/#dev-guide) (auf Chinesisch)
- [IP-Bereiche für Azure-Rechenzentren in China](https://www.microsoft.com/en-us/download/confirmation.aspx?id=57062)
- [Verwalten von Leistung und Konnektivität](/azure/china/china-how-to-manage-performance)
- [Azure Architecture Center](https://docs.microsoft.com/azure/architecture/)
