---
title: Erstellen, Verwalten und Sichern von Administrator- und Abfrage-API-Schlüsseln für Azure Search | Microsoft-Dokumentation
description: Zugriffssteuerung über API-Schlüssel für den Dienstendpunkt. Administratorschlüssel gewähren Schreibzugriff. Abfrageschlüssel können nur für Lesezugriff erstellt werden.
author: HeidiSteen
manager: cgronlun
tags: azure-portal
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 03/20/2018
ms.author: heidist
ms.openlocfilehash: 4215795b7cd2a25427a3ce9b3cde16bfc69cb009
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="create-and-manage-api-keys-for-an-azure-search-service"></a>Erstellen und Verwalten von API-Schlüsseln für einen Azure Search-Dienst

Für alle Anforderungen an einen Suchdienst wird ein API-Schlüssel benötigt, der speziell für Ihren Dienst generiert wurde. Dieser API-Schlüssel ist der einzige Authentifizierungsmechanismus für den Zugriff auf Ihren Dienstendpunkt. 

Ein API-Schlüssel ist eine Zeichenfolge, die aus zufällig generierten Zahlen und Buchstaben besteht. Durch [rollenbasierte Berechtigungen](search-security-rbac.md) können Sie die Schlüssel löschen oder lesen, aber Sie können einen Schlüssel nicht durch ein benutzerdefiniertes Kennwort ersetzen oder Active Directory als primäre Authentifizierungsmethode für den Zugriff auf Suchvorgänge verwenden. 

Zwei Arten von Schlüsseln werden für den Zugriff auf Ihren Suchdienst verwendet: Administrator (Lesen/Schreiben) und Abfrage (schreibgeschützt).

|Schlüssel|BESCHREIBUNG|Einschränkungen|  
|---------|-----------------|------------|  
|Administrator|Gewährt Vollzugriff auf alle Vorgänge. Dazu zählen die Dienstverwaltung sowie das Erstellen und Löschen von Indizes, Indexern und Datenquellen.<br /><br /> Für Administratoren werden zwei Schlüssel generiert, wenn der Dienst erstellt wird. Diese werden im Portal als *primärer* bzw. *sekundärer* Schlüssel bezeichnet und können bei Bedarf einzeln neu generiert werden. Durch die Verwendung von zwei Schlüsseln kann weiterhin auf den Dienst zugegriffen werden, während für einen der Schlüssel ein Rollover durchgeführt wird.<br /><br /> Administratorschlüssel werden nur in HTTP-Anforderungsheadern angegeben. Ein API-Schlüssel für Administratoren kann nicht in einer URL platziert werden.|Maximal zwei pro Dienst|  
|Abfragen|Gewährt Lesezugriff auf Indizes und Dokumente. Diese werden in der Regel auf Clientanwendungen verteilt, die Suchanfragen ausgeben.<br /><br /> Abfrageschlüssel werden bei Bedarf erstellt. Sie können manuell über das Portal oder programmgesteuert über die [Verwaltungs-REST-API](https://docs.microsoft.com/rest/api/searchmanagement/) erstellt werden.<br /><br /> Abfrageschlüssel können in einem HTTP-Anforderungsheader für Such-, Vorschlags- oder Recherchevorgänge angegeben werden. Sie können einen Abfrageschlüssel aber auch als Parameter in einer URL übergeben. Je nachdem, wie Ihre Clientanwendung die Anforderung formuliert, ist es unter Umständen einfacher, den Schlüssel als Abfrageparameter zu übergeben:<br /><br /> `GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2017-11-11&api-key=[query key]`|50 pro Dienst|  

 Administrator- und Abfrageschlüssel sind rein optisch nicht zu unterscheiden. Bei beiden Schlüsseln handelt es sich um eine Zeichenfolge mit 32 nach dem Zufallsprinzip generierten alphanumerischen Zeichen. Sollten Sie nicht mehr wissen, welche Art von Schlüssel in Ihrer Anwendung angegeben ist, können Sie [die Schlüsselwerte im Portal überprüfen](https://portal.azure.com) oder über die [REST-API](https://docs.microsoft.com/rest/api/searchmanagement/) den Wert und die Art des Schlüssels zurückgeben.  

> [!NOTE]  
>  Aus Sicherheitsgründen sollten im Anforderungs-URI keine vertraulichen Daten wie etwa ein API-Schlüssel (`api-key`) übergeben werden. Aus diesem Grund akzeptiert Azure Search als `api-key` in der Abfragezeichenfolge nur einen Abfrageschlüssel. Es empfiehlt sich, diese Richtlinie einzuhalten – es sei denn, der Inhalt Ihres Index soll öffentlich verfügbar sein. Wir empfehlen generell, `api-key` als Anforderungsheader zu übergeben.  

## <a name="find-api-keys-for-your-service"></a>Suchen nach API-Schlüsseln für Ihren Dienst

Zugriffsschlüssel stehen über das Portal oder über die [Verwaltungs-REST-API](https://docs.microsoft.com/rest/api/searchmanagement/) zur Verfügung. Weitere Informationen finden Sie unter [Verwalten der Administrator- und Abfrage-API-Schlüssel](search-security-api-keys.md).

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Listen Sie die [Suchdienste](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) für Ihr Abonnement auf.
3. Wählen Sie den Dienst aus, und suchen Sie auf der Dienstseite nach **Einstellungen** >**Schlüssel**, um Administrator- und Abfrageschlüssel anzuzeigen.

![Portalseite > „Einstellungen“ > Abschnitt „Schlüssel“](media/search-security-overview/settings-keys.png)

## <a name="regenerate-admin-keys"></a>Erneutes Generieren von Administratorschlüsseln

Für jeden Dienst werden zwei Administratorschlüssel erstellt, sodass Sie ein Rollover für einen Primärschlüssel mit dem Sekundärschlüssel für den weiteren Zugriff ausführen können.

Wenn Sie sowohl Primär- als auch Sekundärschlüssel gleichzeitig neu generieren, haben alle Anwendungen, die einen der beiden Schlüssel für den Zugriff auf Dienstvorgänge verwenden, keinen Zugriff mehr auf den Dienst.

1. Kopieren Sie den Sekundärschlüssel auf der Seite **Einstellungen** >**Schlüssel**.
2. Aktualisieren Sie für alle Anwendungen die Einstellungen des API-Schlüssels so, dass der Sekundärschlüssel verwendet wird.
3. Generieren Sie den Primärschlüssel neu.
4. Aktualisieren Sie alle Anwendungen so, dass sie den neuen Primärschlüssel verwenden.

## <a name="secure-api-keys"></a>Sichern von API-Schlüsseln
Die Sicherheit der Schlüssel wird erreicht, indem der Zugriff über das Portal oder Resource Manager-Oberflächen (PowerShell oder Befehlszeilenschnittstelle) eingeschränkt wird. Wie bereits erwähnt, können Abonnementadministratoren alle API-Schlüssel anzeigen und neu generieren. Informieren Sie sich über Rollenzuweisungen, damit Sie wissen, wer Zugriff auf die Admin-Schlüssel hat.

+ Klicken Sie im Dashboard für den Dienst auf **Zugriffssteuerung (IAM)**, um die Rollenzuweisungen für Ihren Dienst anzuzeigen.

Mitglieder der folgenden Rollen können Schlüssel anzeigen und neu generieren: Besitzer, Mitwirkender, [Suchdienstmitwirkender](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#search-service-contributor)

> [!Note]
> Für den identitätsbasierten Zugriff über Suchergebnisse können Sie Sicherheitsfilter erstellen, um die Ergebnisse anhand der Identität einzugrenzen. Auf diese Weise werden Dokumente entfernt, auf die der Anforderer keinen Zugriff haben sollte. Weitere Informationen finden Sie unter [Sicherheitsfilter](search-security-trimming-for-azure-search.md) und [Sichern mit Active Directory](search-security-trimming-for-azure-search-with-aad.md).

## <a name="see-also"></a>Weitere Informationen

+ [Rollenbasierte Zugriffssteuerung in Azure Search](search-security-rbac.md)
+ [Verwalten mit PowerShell](search-manage-powershell.md) 
+ [Artikel zu Leistung und Optimierung](search-performance-optimization.md)