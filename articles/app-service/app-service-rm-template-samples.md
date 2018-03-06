---
title: "Beispiele für Azure Resource Manager-Vorlagen – App Service | Microsoft-Dokumentation"
description: "Beispiele für Azure Resource Manager-Vorlagen – App Service"
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
ms.openlocfilehash: e28a27b9a00164fcbba6eb5d3e5435548486ffa4
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="azure-resource-manager-templates-for-azure-web-apps"></a>Azure Resource Manager-Vorlagen für Azure-Web-Apps

Die folgende Tabelle enthält Links zu Azure Resource Manager-Vorlagen. Empfehlungen zur Vermeidung allgemeiner Fehler beim Erstellen von Web-App-Vorlagen finden Sie unter [Guidance on deploying web apps with Azure Resource Manager templates](web-sites-rm-template-guidance.md) (Leitfaden zur Bereitstellung von Web-Apps mit Azure Resource Manager-Vorlagen).

| | |
|-|-|
|**Web-App bereitstellen**||
| [Mit einem GitHub-Repository verknüpfte Web-App](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-github-deploy)| Stellt eine Azure-Web-App bereit, die Code aus GitHub bezieht. |
| [Web-App mit benutzerdefinierten Bereitstellungsslots](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-custom-deployment-slots)| Stellt eine Azure-Web-App mit benutzerdefinierten Bereitstellungsslots/-umgebungen bereit. |
|**Web-App konfigurieren**||
| [Web-App-Zertifikat aus Key Vault](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-certificate-from-key-vault)| Stellt ein Azure-Web-App-Zertifikat auf der Grundlage eines Key Vault-Geheimnisses bereit und verwendet es für die SSL-Bindung. |
| [Web-App mit benutzerdefinierter Domäne](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-custom-domain)| Stellt eine Azure-Web-App mit einem benutzerdefinierten Hostnamen bereit. |
| [Web-App mit benutzerdefinierter Domäne und SSL](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-custom-domain-and-ssl)| Stellt eine Azure-Web-App mit einem benutzerdefinierten Hostnamen bereit und ruft aus Key Vault ein Web-App-Zertifikat für die SSL-Bindung ab. |
| [Web-App mit GoLang-Erweiterung](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-with-golang)| Stellt eine Azure-Web-App mit Golang-Websiteerweiterung bereit, wodurch in Golang entwickelte Webanwendungen in Azure ausgeführt werden können. |
| [Web-App mit Java 8 und Tomcat 8](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-java-tomcat)| Stellt eine Azure-Web-App mit aktiviertem Java 8 und Tomcat 8 bereit, um die Ausführung von Java-Anwendungen in Azure zu ermöglichen. |
|**Linux-Web-App**||
| [Web App on Linux mit MySQL](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-linux-managed-mysql) | Stellt eine Azure-Web-App unter Linux mit Azure-Datenbank für MySQL bereit. |
| [Web App on Linux mit PostgreSQL](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-linux-managed-postgresql) | Stellt eine Azure-Web-App unter Linux mit Azure-Datenbank für PostgreSQL bereit. |
|**Web-App mit verbundenen Ressourcen**||
| [Web-App mit MySQL](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-managed-mysql)| Stellt eine Azure-Web-App unter Windows mit Azure-Datenbank für MySQL bereit. |
| [Web-App mit PostgreSQL](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-managed-postgresql)| Stellt eine Azure-Web-App unter Windows mit Azure-Datenbank für PostgreSQL bereit. |
| [Web-App mit einer SQL-Datenbank](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-sql-database)| Stellt ein Azure-Web-App und eine SQL-Datenbank auf der Dienstebene „Basic“ bereit. |
| [Web-App mit Blob Storage-Verbindung](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-blob-connection)| Stellt eine Azure-Web-App mit einer Blob Storage-Verbindungszeichenfolge bereit, um in der Web-App die Verwendung von Azure Blob Storage zu ermöglichen. |
| [Web-App mit Redis Cache](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-with-redis-cache)| Stellt eine Azure-Web-App mit Redis Cache bereit. |
|**App Service-Umgebung**||
| [Erstellen einer App Service-Umgebung v2](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-asev2-create) | Erstellt eine App Service-Umgebung v2 in Ihrem virtuellen Netzwerk. |
| [Erstellen einer App Service-Umgebung v2 mit einer ILB-Adresse](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-asev2-ilb-create/) | Erstellt eine App Service-Umgebung v2 in Ihrem virtuellen Netzwerk mit einer privaten Adresse des internen Lastenausgleichs. |
| [Konfigurieren des SSL-Standardzertifikats für eine ILB-App Service-Umgebung oder eine ILB-App Service-Umgebung v2](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-ase-ilb-configure-default-ssl) | Konfiguriert das SSL-Standardzertifikat für eine ILB-App Service-Umgebung oder eine ILB-App Service-Umgebung v2. |
| | |
