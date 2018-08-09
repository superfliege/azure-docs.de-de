---
title: Azure App Service-IP-Einschränkungen | Microsoft-Dokumentation
description: Informationen zum Verwenden von IP-Einschränkungen mit Azure App Service
author: ccompy
manager: stefsch
editor: ''
services: app-service\web
documentationcenter: ''
ms.assetid: 3be1f4bd-8a81-4565-8a56-528c037b24bd
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 7/30/2018
ms.author: ccompy
ms.openlocfilehash: fb26d91ae772c4da1055da80366d6e8c6b80a6ac
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2018
ms.locfileid: "39364307"
---
# <a name="azure-app-service-static-ip-restrictions"></a>Statische Azure App Service-IP-Einschränkungen #

Mit IP-Einschränkungen können Sie eine nach Priorität sortierte Zulassen/Ablehnen-Liste mit IP-Adressen definieren, die auf Ihre App zugreifen dürfen. Die Zulassen-Liste kann IPv4- und IPv6-Adressen enthalten. Wenn mindestens ein Eintrag vorhanden ist, enthält die Liste am Ende einen impliziten Eintrag vom Typ „Alle ablehnen“. 

Die Funktion „IP-Einschränkungen“ funktioniert mit allen von App Service gehosteten Workloads. Hierzu gehören Web-Apps, API-Apps, Linux-Apps, Linux-Container-Apps und Functions. 

Wenn eine Anforderung an Ihre App gesendet wird, wird die VON-IP-Adresse anhand der Liste mit den IP-Einschränkungen ausgewertet. Wenn der Zugriff für die Adresse basierend auf den Regeln in der Liste nicht zugelassen wird, antwortet der Dienst mit dem Statuscode [HTTP 403](https://en.wikipedia.org/wiki/HTTP_403).

Die Funktion „IP-Einschränkungen“ wird in den App Service-Front-End-Rollen implementiert, die den Workerhosts vorgeschaltet sind, auf denen Ihr Code ausgeführt wird. Bei IP-Einschränkungen handelt es sich daher um Netzwerk-ACLs.  

![Datenfluss bei IP-Einschränkungen](media/app-service-ip-restrictions/ip-restrictions-flow.png)

Eine Zeit lang war die Funktion „IP-Einschränkungen“ im Portal eine Schicht, die oberhalb der ipSecurity-Funktion in IIS angeordnet war. Die aktuelle Funktion „IP-Einschränkungen“ unterscheidet sich hiervon. Sie können ipSecurity in der web.config-Datei Ihrer Anwendung weiterhin konfigurieren, aber die Front-End-basierten Regeln für IP-Einschränkungen werden angewendet, bevor der Datenverkehr IIS erreicht.

## <a name="adding-and-editing-ip-restriction-rules-in-the-portal"></a>Hinzufügen und Bearbeiten von Regeln für IP-Einschränkungen im Portal ##

Um eine IP-Einschränkungsregel zu Ihrer App hinzuzufügen, wählen Sie im Menü **Netzwerk**>**IP-Einschränkungen**, und klicken Sie auf **IP-Einschränkungen konfigurieren**.

![App Service-Netzwerkoptionen](media/app-service-ip-restrictions/ip-restrictions.png)  

Auf der Benutzeroberfläche für IP-Einschränkungen können Sie die Liste mit den Regeln für die IP-Einschränkungen überprüfen, die für Ihre App definiert sind.

![Liste der IP-Einschränkungen](media/app-service-ip-restrictions/ip-restrictions-browse.png)

Wenn Ihre Regeln wie in dieser Abbildung konfiguriert sind, akzeptiert Ihre App nur Datenverkehr von 131.107.159.0/24, und der Datenverkehr von allen anderen IP-Adressen wird abgelehnt.

Klicken Sie auf **[+] Hinzufügen**, um eine neue IP-Einschränkungsregel hinzuzufügen. Wenn Sie eine Regel hinzufügen, wird sie sofort wirksam. Regeln werden in der Reihenfolge ihrer Priorität erzwungen, wobei mit der niedrigsten Zahl begonnen wird. Implizit gilt die Regel „Alle ablehnen“, die wirksam ist, sobald Sie die erste Regel hinzugefügt haben. 

![Hinzufügen einer IP-Einschränkungsregel](media/app-service-ip-restrictions/ip-restrictions-add.png)

Die Notation von IP-Adressen muss für IPv4- und IPv6-Adressen in CIDR-Notation angegeben werden. Zum Angeben einer genauen Adresse können Sie beispielsweise 1.2.3.4/32 verwenden, wobei die ersten vier Oktette für Ihre IP-Adresse stehen und mit /32 die Maske angegeben wird. Die IPv4-CIDR-Notation für alle Adressen ist 0.0.0.0/0. Weitere Informationen zur CIDR-Notation finden Sie unter [Classless Inter-Domain Routing](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing).  

Sie können auf eine beliebige Zeile klicken, um eine vorhandene IP-Einschränkungsregel zu bearbeiten. Änderungen sind sofort wirksam. Dies gilt auch für Änderungen der Prioritätsreihenfolge.

![Bearbeiten einer IP-Einschränkungsregel](media/app-service-ip-restrictions/ip-restrictions-edit.png)

Klicken Sie zum Löschen einer Regel für die entsprechende Regel auf **...** und dann auf **Entfernen**. 

![Löschen einer IP-Einschränkungsregel](media/app-service-ip-restrictions/ip-restrictions-delete.png)

## <a name="programmatic-manipulation-of-ip-restriction-rules"></a>Programmgesteuerte Bearbeitung von IP-Einschränkungsregeln ##

Derzeit können eine CLI und PowerShell für die neue Funktion „IP-Einschränkungen“ nicht verwendet werden, aber die Werte können manuell per PUT-Vorgang in der App-Konfiguration im Resource Manager festgelegt werden. Sie können beispielsweise „resources.azure.com“ verwenden und den ipSecurityRestrictions-Block bearbeiten, um den erforderlichen JSON-Code hinzuzufügen. 

Der Speicherort dieser Informationen im Resource Manager ist:

management.azure.com/subscriptions/**Abonnement-ID**/resourceGroups/**Ressourcengruppen**/providers/Microsoft.Web/sites/**Web-App-Name**/config/web?api-version=2018-02-01

Die JSON-Syntax für das obige Beispiel lautet:

    "ipSecurityRestrictions": [
      {
        "ipAddress": "131.107.159.0/24",
        "action": "Allow",
        "tag": "Default",
        "priority": 100,
        "name": "allowed access"
      }
    ],
