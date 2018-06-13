---
title: Beispiele für Azure Resource Manager-Vorlagen – App Service | Microsoft-Dokumentation
description: Beispiele für Azure Resource Manager-Vorlagen für das Web-Apps-Feature von App Service
services: app-service
documentationcenter: app-service
author: tfitzmac
manager: timlt
editor: ggailey777
tags: azure-service-management
ms.service: app-service
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: app-service
ms.date: 02/26/2018
ms.author: tomfitz
ms.custom: mvc
ms.openlocfilehash: 155b47fc4c664701ec0f21bdc5ae34f3d7f666ff
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2018
ms.locfileid: "29933067"
---
# <a name="azure-resource-manager-templates-for-web-apps"></a>Azure Resource Manager-Vorlagen für Web-Apps

Die folgende Tabelle enthält Links zu Azure Resource Manager-Vorlagen für das Web-Apps-Feature von Azure App Service. Empfehlungen zur Vermeidung allgemeiner Fehler beim Erstellen von Web-App-Vorlagen finden Sie unter [Anleitung zum Bereitstellen von Web-Apps mit Azure Resource Manager-Vorlagen](web-sites-rm-template-guidance.md).

| | |
|-|-|
|**Bereitstellen einer Web-App**||
| [Mit einem GitHub-Repository verknüpfte Web-App](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-github-deploy)| Stellt eine Azure-Web-App bereit, die Code aus GitHub bezieht. |
| [Web-App mit benutzerdefinierten Bereitstellungsslots](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-custom-deployment-slots)| Stellt eine Azure-Web-App mit benutzerdefinierten Bereitstellungsslots/-umgebungen bereit. |
|**Konfigurieren einer Web-App**||
| [Web-App-Zertifikat aus Key Vault](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-certificate-from-key-vault)| Stellt ein Azure-Web-App-Zertifikat auf der Grundlage eines Azure Key Vault-Geheimnisses bereit und verwendet es für die SSL-Bindung. |
| [Web-App mit benutzerdefinierter Domäne](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-custom-domain)| Stellt eine Azure-Web-App mit einem benutzerdefinierten Hostnamen bereit. |
| [Web-App mit benutzerdefinierter Domäne und SSL](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-custom-domain-and-ssl)| Stellt eine Azure-Web-App mit einem benutzerdefinierten Hostnamen bereit und ruft aus Key Vault ein Web-App-Zertifikat für die SSL-Bindung ab. |
| [Web-App mit GoLang-Erweiterung](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-with-golang)| Stellt eine Azure-Web-App mit der GoLang-Websiteerweiterung bereit. Dies ermöglicht die Ausführung von mit GoLang entwickelten Webanwendungen in Azure. |
| [Web-App mit Java 8 und Tomcat 8](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-java-tomcat)| Stellt eine Azure-Web-App mit aktiviertem Java 8 und Tomcat 8 bereit. Dies ermöglicht die Ausführung von Java-Anwendungen in Azure. |
|**Linux-Web-App**||
| [Web-App unter Linux mit MySQL](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-linux-managed-mysql) | Stellt eine Azure-Web-App unter Linux mit Azure Database for MySQL bereit. |
| [Web-App unter Linux mit PostgreSQL](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-linux-managed-postgresql) | Stellt eine Azure-Web-App unter Linux mit Azure Database for PostgreSQL bereit. |
|**Web-App mit verbundenen Ressourcen**||
| [Web-App mit MySQL](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-managed-mysql)| Stellt eine Azure-Web-App unter Windows mit Azure Database for MySQL bereit. |
| [Web-App mit PostgreSQL](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-managed-postgresql)| Stellt eine Azure-Web-App unter Windows mit Azure Database for PostgreSQL bereit. |
| [Web-App mit einer SQL-Datenbank](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-sql-database)| Stellt ein Azure-Web-App und eine SQL-Datenbank auf der Dienstebene „Basic“ bereit. |
| [Web-App mit Blob Storage-Verbindung](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-blob-connection)| Stellt eine Azure-Web-App mit einer Azure Blob Storage-Verbindungszeichenfolge bereit. Dies ermöglicht die Verwendung von Blob Storage über die Web-App. |
| [Web-App mit Redis Cache](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-with-redis-cache)| Stellt eine Azure-Web-App mit Redis Cache bereit. |
|**App Service-Umgebung für PowerApps**||
| [Erstellen einer App Service-Umgebung v2](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-asev2-create) | Erstellt eine App Service-Umgebung v2 in Ihrem virtuellen Netzwerk. |
| [Erstellen einer App Service-Umgebung v2 mit einer ILB-Adresse](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-asev2-ilb-create/) | Erstellt eine App Service-Umgebung v2 in Ihrem virtuellen Netzwerk mit einer privaten Adresse des internen Lastenausgleichs. |
| [Konfigurieren des SSL-Standardzertifikats für eine ILB-App Service-Umgebung oder eine ILB-App Service-Umgebung v2](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-ase-ilb-configure-default-ssl) | Konfiguriert das SSL-Standardzertifikat für eine ILB-App Service-Umgebung oder eine ILB-App Service-Umgebung v2. |
| | |
