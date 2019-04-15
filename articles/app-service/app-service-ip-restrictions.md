---
title: Beschränken von Client-IPs – Azure App Service | Microsoft-Dokumentation
description: Informationen zu Zugriffseinschränkungen in Azure App Service
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
ms.date: 07/30/2018
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 805de614246028bc75268e83991fa7831b990325
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2019
ms.locfileid: "58882326"
---
# <a name="azure-app-service-static-access-restrictions"></a>Azure App Service – Statische Zugriffseinschränkungen #

Mithilfe von Zugriffseinschränkungen können Sie eine nach Priorität sortierte Liste mit zulässigen/unzulässigen IP-Adressen definieren, die auf Ihre App zugreifen dürfen. Die Zulassen-Liste kann IPv4- und IPv6-Adressen enthalten. Wenn mindestens ein Eintrag vorhanden ist, enthält die Liste am Ende einen impliziten Eintrag vom Typ „Alle ablehnen“.

Die Funktion „Zugriffseinschränkungen“ funktioniert mit allen von App Service gehosteten Workloads. Hierzu gehören Web-Apps, API-Apps, Linux-Apps, Linux-Container-Apps und Functions.

Wenn eine Anforderung an Ihre App gesendet wird, wird die VON-IP-Adresse anhand der Liste mit den Zugriffseinschränkungen ausgewertet. Wenn der Zugriff für die Adresse basierend auf den Regeln in der Liste nicht zugelassen wird, antwortet der Dienst mit dem Statuscode [HTTP 403](https://en.wikipedia.org/wiki/HTTP_403).

Die Funktion „Zugriffseinschränkungen“ wird in den App Service-Front-End-Rollen implementiert, die den Workerhosts vorgeschaltet sind, auf denen Ihr Code ausgeführt wird. Also handelt es sich bei Zugriffseinschränkungen im Grunde um Zugriffssteuerungslisten für das Netzwerk.  

![Flow von Zugriffseinschränkungen](media/app-service-ip-restrictions/ip-restrictions-flow.png)

Eine Zeit lang war die Funktion „Zugriffseinschränkungen“ im Portal eine Schicht, die oberhalb der Funktion „ipSecurity“ in IIS angeordnet war. Die aktuelle Funktion „Zugriffseinschränkungen“ unterscheidet sich hiervon. Sie können „ipSecurity“ in der Datei „web.config“ Ihrer Anwendung weiterhin konfigurieren, aber die Front-End-basierten Regeln für Zugriffseinschränkungen werden angewendet, bevor der Datenverkehr IIS erreicht.

## <a name="adding-and-editing-access-restriction-rules-in-the-portal"></a>Hinzufügen und Bearbeiten von Regeln für Zugriffseinschränkungen im Portal ##

Um Ihrer App eine Zugriffseinschränkungsregel hinzuzufügen, wählen Sie im Menü **Netzwerk**>**Zugriffseinschränkungen** aus und klicken auf **Zugriffseinschränkungen konfigurieren**.

![App Service-Netzwerkoptionen](media/app-service-ip-restrictions/ip-restrictions.png)  

Auf der Benutzeroberfläche für Zugriffseinschränkungen können Sie die Liste mit den Regeln für die Zugriffseinschränkungen überprüfen, die für Ihre App definiert sind.

![Liste mit Zugriffseinschränkungen](media/app-service-ip-restrictions/ip-restrictions-browse.png)

Wenn Ihre Regeln wie in dieser Abbildung konfiguriert sind, akzeptiert Ihre App nur Datenverkehr von 131.107.159.0/24, und der Datenverkehr von allen anderen IP-Adressen wird abgelehnt.

Klicken Sie auf **[+] Hinzufügen**, um eine neue Zugriffseinschränkungsregel hinzuzufügen. Wenn Sie eine Regel hinzufügen, wird sie sofort wirksam. Regeln werden in der Reihenfolge ihrer Priorität erzwungen, wobei mit der niedrigsten Zahl begonnen wird. Implizit gilt die Regel „Alle ablehnen“, die wirksam ist, sobald Sie die erste Regel hinzugefügt haben.

![Hinzufügen einer Zugriffseinschränkungsregel](media/app-service-ip-restrictions/ip-restrictions-add.png)

Die Notation von IP-Adressen muss für IPv4- und IPv6-Adressen in CIDR-Notation angegeben werden. Zum Angeben einer genauen Adresse können Sie beispielsweise 1.2.3.4/32 verwenden, wobei die ersten vier Oktette für Ihre IP-Adresse stehen und mit /32 die Maske angegeben wird. Die IPv4-CIDR-Notation für alle Adressen ist 0.0.0.0/0. Weitere Informationen zur CIDR-Notation finden Sie unter [Classless Inter-Domain Routing](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing).  

Sie können auf eine beliebige Zeile klicken, um eine vorhandene Zugriffseinschränkungsregel zu bearbeiten. Änderungen sind sofort wirksam. Dies gilt auch für Änderungen der Prioritätsreihenfolge.

![Bearbeiten einer Zugriffseinschränkungsregel](media/app-service-ip-restrictions/ip-restrictions-edit.png)

Klicken Sie zum Löschen einer Regel für die entsprechende Regel auf **...** und dann auf **Entfernen**.

![Löschen einer Zugriffseinschränkungsregel](media/app-service-ip-restrictions/ip-restrictions-delete.png)

Auf der nächsten Registerkarte können Sie auch den Bereitstellungszugriff einschränken. Zum Hinzufügen, Bearbeiten oder Löschen der einzelnen Regeln befolgen Sie die zuvor beschriebenen Schritte.

![Liste mit Zugriffseinschränkungen](media/app-service-ip-restrictions/ip-restrictions-scm-browse.png)

## <a name="programmatic-manipulation-of-access-restriction-rules"></a>Programmgesteuerte Bearbeitung von Zugriffseinschränkungsregeln ##

Derzeit können Sie weder die CLI noch PowerShell für die neue Funktion „Zugriffseinschränkungen“ verwenden, aber Sie können die Werte manuell per PUT-Vorgang in der App-Konfiguration in Resource Manager festlegen. Sie können beispielsweise „resources.azure.com“ verwenden und den ipSecurityRestrictions-Block bearbeiten, um den erforderlichen JSON-Code hinzuzufügen.

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
