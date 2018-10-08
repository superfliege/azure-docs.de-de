---
title: Azure App Service-Umgebung Management-Adressen
description: Listet die Management-Adressen auf, die verwendet werden, um eine App Service-Umgebung zu befehligen
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: a7738a24-89ef-43d3-bff1-77f43d5a3952
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2018
ms.author: ccompy
ms.openlocfilehash: 590679daff20f9c469fb8fcfcc0fbbad77f91b5b
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47162853"
---
# <a name="app-service-environment-management-addresses"></a>App Service-Umgebung Management-Adressen

Die App Service-Umgebung (ASE) ist eine Bereitstellung des Azure App Service in einem Subnetz in Ihrem Azure Virtual Network (VNet).  Die ASE muss von der von Azure App Service verwendeten Verwaltungsebene aus zugänglich sein.  Dieser ASE-Verwaltungsdatenverkehr durchläuft das benutzergesteuerte Netzwerk. Wenn dieser Datenverkehr blockiert oder falsch weitergeleitet wird, wird die ASE angehalten. Weitere Informationen zu den Netzwerkabhängigkeiten der App Service-Umgebung finden Sie unter [Überlegungen zum Netzwerkbetrieb in der App Service-Umgebung][networking]. Allgemeine Informationen zur ASE finden Sie unter [Einführung in die App Service-Umgebung][intro].

Dieses Dokument listet die App Service-Quelladressen für den Verwaltungsdatenverkehr zur ASE auf und erfüllt zwei wichtige Funktionen.  

1. Diese Adressen können zum Erstellen von Netzwerksicherheitsgruppen zum Sperren des eingehenden Datenverkehrs verwendet werden.  
2. Sie können Routen anhand dieser Adressen erstellen, um die Bereitstellung von Tunnelerzwingung zu unterstützen. Ausführliche Informationen zum Konfigurieren Ihrer ASE für den Betrieb in einer Umgebung, in der ausgehender Datenverkehr lokal gesendet wird, finden Sie unter [Konfigurieren Ihrer App Service-Umgebung mit erzwungenem Tunneling][forcedtunnel]

Alle ASEs besitzen eine öffentliche VIP, an der Verwaltungsdatenverkehr eingeht. Der eingehende Verwaltungsdatenverkehr von diesen Adressen geht an den Ports 454 und 455 der öffentlichen VIP Ihrer ASE ein.  

## <a name="list-of-management-addresses"></a>Liste der Verwaltungsadressen ##

| Region | Adressen |
|--------|-----------|
| Alle öffentlichen Regionen | 70.37.57.58, 157.55.208.185, 52.174.22.21, 13.94.149.179, 13.94.143.126, 13.94.141.115, 52.178.195.197, 52.178.190.65, 52.178.184.149, 52.178.177.147, 13.75.127.117, 40.83.125.161, 40.83.121.56, 40.83.120.64, 52.187.56.50, 52.187.63.37, 52.187.59.251, 52.187.63.19, 52.165.158.140, 52.165.152.214, 52.165.154.193, 52.165.153.122, 104.44.129.255, 104.44.134.255, 104.44.129.243, 104.44.129.141, 23.102.188.65, 191.236.154.88, 13.64.115.203, 65.52.193.203, 70.37.89.222, 52.224.105.172, 23.102.135.246 |
| Microsoft Azure Government (Fairfax oder MAG) | 23.97.29.209, 13.72.53.37, 13.72.180.105, 23.97.0.17, 23.97.16.184 |

## <a name="get-your-management-addresses-from-api"></a>Abrufen der Verwaltungsadressen über eine API ##

Sie können die Verwaltungsadressen, die zu Ihrer App Service-Umgebung passen, mit dem folgenden API-Aufruf auflisten.

    get /subscriptions/<subscription ID>/resourceGroups/<resource group>/providers/Microsoft.Web/hostingEnvironments/<ASE Name>/inboundnetworkdependenciesendpoints?api-version=2016-09-01

Die API gibt ein JSON-Dokument zurück, das alle eingehenden IP-Adressen für Ihre App Service-Umgebung enthält. Die Adressliste enthält die Verwaltungsadressen, die die virtuelle IP Ihrer App Service-Umgebung und der Subnetz-Adressbereich der App Service-Umgebung selbst verwendet haben.  

Verwenden Sie die folgenden Befehle zum Aufrufen der API mit [armclient](http://github.com/projectkudu/ARMClient), setzen Sie jedoch Ihre Abonnement-ID, Ihre Ressourcengruppe und den Namen Ihrer App Service-Umgebung ein.  

    armclient login
    armclient get /subscriptions/<subscription ID>/resourceGroups/<resource group>/providers/Microsoft.Web/hostingEnvironments/<ASE Name>/inboundnetworkdependenciesendpoints?api-version=2016-09-01


<!-- LINKS -->
[networking]: ./network-info.md
[intro]: ./intro.md
[forcedtunnel]: ./forced-tunnel-support.md
