---
title: Sperren des ausgehenden Datenverkehrs der App Service-Umgebung – Azure
description: Beschreibung der Integration mit Azure Firewall zum Sichern des ausgehenden Datenverkehrs
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 955a4d84-94ca-418d-aa79-b57a5eb8cb85
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/24/2018
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 52051ea221a3d49d86cc6b95e020e1075ce8cba2
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2018
ms.locfileid: "53275549"
---
# <a name="locking-down-an-app-service-environment"></a>Sperren einer App Service-Umgebung

Die App Service-Umgebung verfügt über externe Abhängigkeiten, auf die sie Zugriff benötigt, um ordnungsgemäß zu funktionieren. Die App Service-Umgebung befindet sich in der Azure Virtual Network-Instanz (VNET) des Benutzers. Benutzer müssen den Datenverkehr für Abhängigkeiten der App Service-Umgebung zulassen. Dies ist jedoch problematisch, wenn sie den gesamten ausgehenden Datenverkehr ihres VNET sperren wollen.

Eine App Service-Umgebung weist zahlreiche Abhängigkeiten für den eingehenden Datenverkehr auf. Der eingehende Verwaltungsdatenverkehr kann über eine Firewallgerät nicht gesendet werden. Die Quelladressen für diesen Datenverkehr sind bekannt und werden im Dokument [Verwaltungsadressen der App Service-Umgebung](https://docs.microsoft.com/azure/app-service/environment/management-addresses) veröffentlicht. Sie können Netzwerksicherheitsgruppen-Regeln mit diesen Informationen erstellen, um eingehenden Datenverkehr zu sichern.

Die Abhängigkeiten der App Service-Umgebung für den ausgehenden Datenverkehr werden fast ausschließlich mit FQDNs definiert, hinter denen sich keine statischen Adressen befinden. Das Fehlen statischer Adressen bedeutet, dass Netzwerksicherheitsgruppen (NSGs) nicht verwendet werden können, um den ausgehenden Datenverkehr einer App Service-Umgebung zu sperren. Die Adressen ändern sich häufig, sodass keine Regeln auf Grundlage der aktuellen Auflösung aufgestellt und keine NSGs damit erstellt werden können. 

Die Lösung zum Sichern ausgehender Adressen besteht in der Verwendung eines Firewallgeräts, das den ausgehenden Datenverkehr basierend auf Domänennamen kontrolliert. Azure Firewall kann ausgehenden HTTP- und HTTPS-Datenverkehr basierend auf den FQDN des Ziels beschränken.  

## <a name="configuring-azure-firewall-with-your-ase"></a>Konfigurieren von Azure Firewall mit Ihrer App Service-Umgebung 

Gehen Sie wie folgt vor, um den ausgehenden Datenverkehr Ihrer App Service-Umgebung mit Azure Firewall zu sperren:

1. Erstellen Sie eine Azure Firewall-Instanz in dem VNET, in dem sich Ihre App Service-Umgebung befindet bzw. befinden wird. Weitere Informationen finden Sie in der [Azure Firewall-Dokumentation](https://docs.microsoft.com/azure/firewall/).
2. Wählen Sie in der Azure Firewall-Benutzeroberfläche den FQDN-Tag der App Service-Umgebung aus.
3. Erstellen Sie eine Routingtabelle mit den Verwaltungsadressen aus [Verwaltungsadressen der App Service-Umgebung]( https://docs.microsoft.com/azure/app-service/environment/management-addresses) mit dem nächsten Hop zum Internet. Die Routingtabelleneinträge werden benötigt, um asymmetrische Routingprobleme zu vermeiden.
4. Fügen Sie mit dem nächsten Hop zum Internet den IP-Adressabhängigkeiten (s. weiter unten „IP-Adressabhängigkeiten“) Routen hinzu.
5. Fügen Sie Ihrer Routingtabelle eine Route für 0.0.0.0/0 hinzu, wobei als nächster Hop Azure Firewall festgelegt ist.
6. Erstellen Sie Dienstendpunkte für das Subnetz Ihrer App Service-Umgebung für Azure SQL und Azure Storage.
7. Weisen Sie die erstellte Routingtabelle dem Subnetz Ihrer App Service-Umgebung zu.

## <a name="application-traffic"></a>Anwendungsdatenverkehr 

Mithilfe der oben genannten Schritte kann Ihre App Service-Umgebung problemlos ausgeführt werden. Sie müssen die Umgebung noch so konfigurieren, dass sie Ihren Anwendungsanforderungen entspricht. Es gibt zwei Probleme bei Anwendungen in einer App Service-Umgebung, die mit Azure Firewall konfiguriert wurde.  

- FQDNs der Anwendungsabhängigkeit müssen der Azure Firewall-Instanz oder der Routingtabelle hinzugefügt werden.
- Für die Adressen, von denen der Datenverkehr kommt, müssen Routen erstellt werden, um asymmetrische Routingprobleme zu vermeiden.

Wenn Ihre Anwendungen Abhängigkeiten aufweisen, müssen diese Ihrer Azure Firewall-Instanz hinzugefügt werden. Erstellen Sie Anwendungsregeln, um HTTP/HTTPS-Datenverkehr und Netzwerkregeln zuzulassen. 

Wenn Sie den Adressbereich kennen, aus dem der von Ihrer Anwendung angeforderte Datenverkehr kommt, können Sie ihn der Routingtabelle hinzufügen, die dem Subnetz Ihrer App Service-Umgebung zugeordnet ist. Wenn der Adressbereich groß oder nicht angegeben ist, können Sie ein Netzwerkgerät wie Application Gateway verwenden, um eine Adresse Ihrer Routingtabelle hinzufügen zu können. Weitere Informationen zum Konfigurieren einer Application Gateway-Instanz mit dem internen Lastenausgleichsmodul der App Service-Umgebung finden Sie unter [Integrieren eines internen Lastenausgleichs einer App Service-Umgebung in eine Application Gateway-Instanz](https://docs.microsoft.com/azure/app-service/environment/integrate-with-application-gateway).


## <a name="dependencies"></a>Abhängigkeiten

Azure App Service verfügt über eine Reihe von externen Abhängigkeiten. Diese können kategorisch in einige Hauptbereiche unterteilt werden:

- Dienstendpunktfähige Dienste müssen mit Dienstendpunkten eingerichtet werden, wenn Sie den ausgehenden Netzwerkdatenverkehr sperren möchten.
- IP-Adressendpunkte werden nicht mit einem Domänennamen adressiert. Dies kann problematisch für Firewallgeräte sein, die erwarten, dass der gesamte HTTPS-Datenverkehr Domänennamen verwendet. Die IP-Adressendpunkte müssen der Routingtabelle hinzugefügt werden, die im Subnetz der App Service-Umgebung eingerichtet ist.
- FQDN-HTTP/HTTPS-Endpunkte können in Ihrem Firewallgerät bereitgestellt werden.
- Platzhalter-HTTP/HTTPS-Endpunkte sind Abhängigkeiten, die von Ihrer App Service-Umgebung abhängig sein können, basierend auf einer Reihe von Qualifizierern. 
- Linux-Abhängigkeiten sind nur relevant, wenn Sie in Ihrer App Service-Umgebung Linux-Apps bereitstellen. Wenn Sie in Ihrer App Service-Umgebung keine Linux-Apps bereitstellen, müssen diese Adressen Ihrer Firewall nicht hinzugefügt werden. 


#### <a name="service-endpoint-capable-dependencies"></a>Dienstendpunktfähige Abhängigkeiten 

| Endpunkt |
|----------|
| Azure SQL |
| Azure Storage |
| Azure Key Vault |


#### <a name="ip-address-dependencies"></a>IP-Adressabhängigkeiten 

| Endpunkt |
|----------|
| 40.77.24.27:443 |
| 13.82.184.151:443 |
| 13.68.109.212:443 |
| 13.90.249.229:443 |
| 13.91.102.27:443 |
| 104.45.230.69:443 |
| 168.62.226.198:12000 |


#### <a name="fqdn-httphttps-dependencies"></a>FQDN-HTTP/HTTPS-Abhängigkeiten 

| Endpunkt |
|----------|
|graph.windows.net:443 |
|login.live.com:443 |
|login.windows.com:443 |
|login.windows.net:443 |
|login.microsoftonline.com:443 |
|client.wns.windows.com:443 |
|definitionupdates.microsoft.com:443 |
|go.microsoft.com:80 |
|go.microsoft.com:443 |
|www.microsoft.com:80 |
|www.microsoft.com:443 |
|wdcpalt.microsoft.com:443 |
|wdcp.microsoft.com:443 |
|ocsp.msocsp.com:443 |
|mscrl.microsoft.com:443 |
|mscrl.microsoft.com:80 |
|crl.microsoft.com:443 |
|crl.microsoft.com:80 |
|www.thawte.com:443 |
|crl3.digicert.com:80 |
|ocsp.digicert.com:80 |
|csc3-2009-2.crl.verisign.com:80 |
|crl.verisign.com:80 |
|ocsp.verisign.com:80 |
|azperfcounters1.blob.core.windows.net:443 |
|azurewatsonanalysis-prod.core.windows.net:443 |
|global.metrics.nsatc.net:80   |
|az-prod.metrics.nsatc.net:443 |
|antares.metrics.nsatc.net:443 |
|azglobal-black.azglobal.metrics.nsatc.net:443 |
|azglobal-red.azglobal.metrics.nsatc.net:443 |
|antares-black.antares.metrics.nsatc.net:443 |
|antares-red.antares.metrics.nsatc.net:443 |
|maupdateaccount.blob.core.windows.net:443 |
|clientconfig.passport.net:443 |
|packages.microsoft.com:443 |
|schemas.microsoft.com:80 |
|schemas.microsoft.com:443 |
|management.core.windows.net:443 |
|management.core.windows.net:80 |
|www.msftconnecttest.com:80 |
|shavamanifestcdnprod1.azureedge.net:443 |
|validation-v2.sls.microsoft.com:443 |
|flighting.cp.wd.microsoft.com:443 |
|dmd.metaservices.microsoft.com:80 |
|admin.core.windows.net:443 |
|azureprofileruploads.blob.core.windows.net:443 |
|azureprofileruploads2.blob.core.windows.net:443 |
|azureprofileruploads3.blob.core.windows.net:443 |
|azureprofileruploads4.blob.core.windows.net:443 |
|azureprofileruploads5.blob.core.windows.net:443 |

#### <a name="wildcard-httphttps-dependencies"></a>Platzhalter-HTTP/HTTPS-Abhängigkeiten 

| Endpunkt |
|----------|
|gr-Prod-\*.cloudapp.net:443 |
| \*.management.azure.com:443 |
| \*.update.microsoft.com:443 |
| \*.windowsupdate.microsoft.com:443 |
|grmdsprod\*mini\*.servicebus.windows.net:443 |
|grmdsprod\*lini\*.servicebus.windows.net:443 |
|grsecprod\*mini\*.servicebus.windows.net:443 |
|grsecprod\*lini\*.servicebus.windows.net:443 |
|graudprod\*mini\*.servicebus.windows.net:443 |
|graudprod\*lini\*.servicebus.windows.net:443 |

#### <a name="linux-dependencies"></a>Linux-Abhängigkeiten 

| Endpunkt |
|----------|
|wawsinfraprodbay063.blob.core.windows.net:443 |
|registry-1.docker.io:443 |
|auth.docker.io:443 |
|production.cloudflare.docker.com:443 |
|download.docker.com:443 |
|us.archive.ubuntu.com:80 |
|download.mono-project.com:80 |
|packages.treasuredata.com:80|
|security.ubuntu.com:80 |

