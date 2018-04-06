---
title: Schützen von Daten und Vorgängen in Azure Search | Microsoft-Dokumentation
description: Die Sicherheit von Azure Search basiert auf SOC 2-Konformität, Verschlüsselung, Authentifizierung und Identitätszugriff durch Benutzer- und Gruppensicherheits-IDs in Azure Search-Filtern.
services: search
documentationcenter: ''
author: HeidiSteen
manager: cgronlun
editor: ''
ms.assetid: ''
ms.service: search
ms.devlang: ''
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 01/19/2018
ms.author: heidist
ms.openlocfilehash: 35f875e5f6345b9ebb9abc4deb71b7bf9c78907d
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="security-and-controlled-access-in-azure-search"></a>Sicherheit und kontrollierter Zugriff in Azure Search

Azure Search ist [SOC 2-konform](https://servicetrust.microsoft.com/ViewPage/MSComplianceGuide?command=Download&downloadType=Document&downloadId=93292f19-f43e-4c4e-8615-c38ab953cf95&docTab=4ce99610-c9c0-11e7-8c2c-f908a777fa4d_SOC%20%2F%20SSAE%2016%20Reports) und verfügt über eine umfassende Sicherheitsarchitektur mit physischer Sicherheit, verschlüsselter Übertragung, verschlüsselter Speicherung und plattformweitem Softwareschutz. Im laufenden Betrieb akzeptiert Azure Search ausschließlich authentifizierte Anforderungen. Optional kann eine benutzerspezifische Zugriffssteuerung für Inhalte hinzugefügt werden. Dieser Artikel geht zwar auf die einzelnen Sicherheitsebenen ein, beschäftigt sich aber hauptsächlich mit dem Schutz von Daten und Vorgängen in Azure Search.

![Blockdiagramm der Sicherheitsebenen](media/search-security-overview/azsearch-security-diagram.png)

## <a name="physical-security"></a>Physische Sicherheit

Microsoft-Rechenzentren sind mit branchenführenden physischen Sicherheitsmaßnahmen ausgestattet und erfüllen eine breite Palette von Standards und Bestimmungen. Weitere Informationen finden Sie auf der Seite [Global Datacenters](https://www.microsoft.com/cloud-platform/global-datacenters) sowie im folgenden kurzen Video zur Sicherheit von Rechenzentren:

> [!VIDEO https://www.youtube.com/embed/r1cyTL8JqRg]

## <a name="encrypted-transmission-and-storage"></a>Verschlüsselte Übertragung und Speicherung

Die Verschlüsselung erstreckt sich auf die gesamte Indexpipeline – von Verbindungen über die Übertragung bis hin zu indizierten in Azure Search gespeicherten Daten.

| Sicherheitsebene | BESCHREIBUNG |
|----------------|-------------|
| Verschlüsselung während der Übertragung | Azure Search lauscht an HTTPS-Port 443. Verbindungen mit Azure-Diensten sind plattformweit verschlüsselt. |
| Verschlüsselung ruhender Daten | Die Verschlüsselung ist vollständig in den Indizierungsprozess integriert, ohne messbare Auswirkungen auf die Durchführungsdauer der Indizierung oder die Indexgröße. Sie wird automatisch auf die gesamte Indizierung angewendet, einschließlich auf inkrementelle Updates für einen nicht vollständig verschlüsselten Index (vor Januar 2018 erstellt).<br><br>Intern basiert die Verschlüsselung auf der [256-Bit-AES-Verschlüsselung](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) von [Microsoft Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-service-encryption).|
| [Compliance mit SOC 2](https://www.aicpa.org/interestareas/frc/assuranceadvisoryservices/aicpasoc2report.html) | Alle Suchdienste sind in sämtlichen Rechenzentren, die Azure Search bereitstellen, vollumfänglich mit AICPA SOC 2 konform. Den vollständigen Bericht finden Sie unter [Azure - and Azure Government SOC 2 Type II Report](https://servicetrust.microsoft.com/ViewPage/MSComplianceGuide?command=Download&downloadType=Document&downloadId=93292f19-f43e-4c4e-8615-c38ab953cf95&docTab=4ce99610-c9c0-11e7-8c2c-f908a777fa4d_SOC%20%2F%20SSAE%2016%20Reports) (SOC 2Type II-Bericht für Azure und Azure Government). |

Die Verschlüsselung ist in Azure Search integriert, wobei Zertifikate und Schlüssel intern von Microsoft verwaltet und universell angewendet werden. Sie können die Verschlüsselung nicht aktivieren bzw. deaktivieren, eigene Schlüssel verwalten oder ersetzen oder Verschlüsselungseinstellungen im Portal oder programmgesteuert anzeigen. 

Die Verschlüsselung ruhender Daten wurde am 24. Januar 2018 angekündigt und gilt für alle Diensttarife wie gemeinsame Dienste (Free) in sämtlichen Regionen. Damit die Verschlüsselung vollständig angewendet werden kann, müssen vor diesem Datum erstellte Indizes gelöscht und neu erstellt werden. Anderenfalls werden nur neue Daten, die nach dem 24. Januar hinzugefügt wurden, verschlüsselt.

## <a name="azure-wide-logical-security"></a>Azure-weite logische Sicherheit

Für die gesamte Azure Stack-Umgebung sind verschiedene Sicherheitsmechanismen verfügbar – und dadurch automatisch auch für die von Ihnen erstellten Azure Search-Ressourcen.

+ [Sperren auf Abonnement- oder Ressourcenebene zur Verhinderung von Löschvorgängen](../azure-resource-manager/resource-group-lock-resources.md)
+ [Rollenbasierte Zugriffssteuerung (Role-based Access Control, RBAC) zur Steuerung des Zugriffs auf Informationen und Verwaltungsvorgänge](../active-directory/role-based-access-control-what-is.md)

Alle Azure-Dienste unterstützen die rollenbasierte Zugriffssteuerung (Role-based Access Control, RBAC) zum einheitlichen Festlegen von Zugriffsebenen für alle Dienste. So ist beispielsweise das Anzeigen sensibler Daten (etwa des Administratorschlüssels) auf die Rollen „Besitzer“ und „Mitwirkender“ beschränkt, der Dienststatus kann dagegen von Mitgliedern aller Rollen angezeigt werden. RBAC bietet die Rollen „Besitzer“, „Mitwirkenden“ und „Leser“. Administratoren sind standardmäßig Mitglieder der Rolle "Besitzer".

## <a name="service-access-and-authentication"></a>Dienstzugriff und -authentifizierung

Azure Search erbt nicht nur die Sicherheitsmaßnahmen der Azure-Plattform, sondern bietet auch eine eigene schlüsselbasierte Authentifizierung. Ein API-Schlüssel ist eine Zeichenfolge, die aus zufällig generierten Zahlen und Buchstaben besteht. Der Typ des Schlüssels (Administrator oder Abfrage) bestimmt die Zugriffsebene. Die Übermittlung eines gültigen Schlüssels gilt als Beleg dafür, dass die Anforderung von einer vertrauenswürdigen Entität stammt. Zwei Arten von Schlüsseln werden für den Zugriff auf Ihren Suchdienst verwendet:

* Admin (gültig für alle Lese-/Schreibvorgänge für den Dienst)
* Abfrage (gültig für Lesevorgänge, z.B. Abfragen eines Index)

Administratorschlüssel werden erstellt, wenn der Dienst bereitgestellt wird. Es gibt zwei Administratorschlüssel, die zur besseren Unterscheidung als *primärer* und *sekundärer* Schlüssel bezeichnet werden. In Wirklichkeit sind sie aber austauschbar. Jeder Dienst verfügt über zwei Admin-Schlüssel, sodass Sie für einen Schlüssel ein Rollover durchführen können, ohne den Zugriff auf den Dienst zu verlieren. Sie können beide Admin-Schlüssel austauschen, können allerdings keine weiteren Schlüssel hinzufügen. Pro Dienst können maximal zwei Admin-Schlüssel vorhanden sein.

Abfrageschlüssel werden bei Bedarf erstellt und sind für Clientanwendungen bestimmt, bei denen Search direkt aufgerufen wird. Sie können bis zu 50 Abfrage-Schlüssel generieren. Im Anwendungscode geben Sie die Such-URL und einen Abfrage-API-Schlüssel ein, um den Lesezugriff auf den Dienst zuzulassen. Im Anwendungscode wird auch der Index angegeben, der von der Anwendung verwendet wird. Zusammen definieren der Endpunkt, ein API-Schlüssel für den Lesezugriff und ein Zielindex den Bereich und die Zugriffsebene der Verbindung von Ihrer Clientanwendung.

Für jede Anforderung – bestehend aus einem obligatorischen Schlüssel, einem Vorgang und einem Objekt – ist eine Authentifizierung erforderlich. In Kombination lässt sich durch die beiden Berechtigungsstufen (uneingeschränkte bzw. Lese-/Schreibberechtigung) und den Kontext (z.B. eine Indexabfrage) ein vollumfängliches Sicherheitsspektrum für Dienstvorgänge erreichen. Weitere Informationen finden Sie unter [Erstellen und Verwalten von API-Schlüsseln für einen Azure Search-Dienst](search-security-api-keys.md).

## <a name="index-access"></a>Indexzugriff

In Azure Search ist ein einzelner Index kein sicherungsfähiges Objekt. Stattdessen wird der Zugriff auf einen Index durch eine Kombination aus Dienstebene (Lese- oder Schreibzugriff) und Vorgangskontext bestimmt.

Für den Endbenutzerzugriff können Sie Abfrageanforderungen in Ihrer Anwendung so strukturieren, dass die Verbindungsherstellung unter Verwendung eines Abfrageschlüssels erfolgt (wodurch alle Abfragen nur schreibgeschützten Zugriff erhalten), und den spezifischen, von Ihrer App verwendeten Index einschließen. Da es in einer Abfrageanforderung kein Konzept für die Verknüpfung von Indizes oder den gleichzeitigen Zugriff auf mehrere Indizes gibt, sind alle Anforderungen definitionsgemäß auf einen einzelnen Index ausgerichtet. Folglich wird die Sicherheitsgrenze durch die Struktur der Abfrage (Kombination aus Schlüssel und einzelnem Zielindex) definiert.

Der Indexzugriff von Administratoren und Entwicklern ist undifferenziert: Beide benötigen Schreibzugriff, um vom Dienst verwaltete Objekte erstellen, löschen und aktualisieren zu können. Ein Benutzer mit einem Administratorschlüssel für Ihren Dienst kann jeden beliebigen Index in diesem Dienst lesen, ändern und löschen. Zum Schutz vor der versehentlichen oder böswilligen Löschung von Indizes können Sie Ihre interne Quellcodeverwaltung für Coderessourcen verwenden und unerwünschte Lösch- oder Änderungsvorgänge für Indizes rückgängig machen. Zur Gewährleistung der Verfügbarkeit verfügt Azure Search über ein clusterinternes Failover-Feature. Ihr proprietärer Code zum Erstellen oder Laden von Indizes wird jedoch nicht gespeichert oder ausgeführt.

Mehrinstanzenfähige Lösungen, die Sicherheitsgrenzen auf der Indexebene erfordern, enthalten in der Regel eine mittlere Ebene, die Kunden für die Indexisolierung verwenden. Weitere Informationen zum mehrinstanzenfähigen Anwendungsfall finden Sie unter [Entwurfsmuster für mehrinstanzenfähige SaaS-Anwendungen und Azure Search](search-modeling-multitenant-saas-applications.md).

## <a name="admin-access-from-client-apps"></a>Administratorzugriff über Client-Apps

Die Verwaltungs-REST-API von Azure Search ist eine Erweiterung von Azure Resource Manager und verfügt über die gleichen Abhängigkeiten. Folglich wird für die Verwaltung des Azure Search-Diensts Active Directory vorausgesetzt. Jegliche aus Clientcode stammende Verwaltungsanforderung muss mithilfe von Azure Active Directory authentifiziert werden, bevor die Anforderung Resource Manager erreicht.

Bei Datenanforderungen für den Azure Search-Dienstendpunkt – beispielsweise „Index erstellen“ (REST-API des Azure Search-Diensts) oder „Dokumente suchen“ (REST-API des Azure Search-Diensts) – wird ein API-Schlüssel im Anforderungsheader verwendet.

Falls Ihr Anwendungscode sowohl Dienstverwaltungsvorgänge als auch Datenvorgänge für Suchindizes oder Dokumente umfasst, implementieren Sie darin zwei Authentifizierungsmethoden: den für Azure Search nativen Zugriffsschlüssel und die für Resource Manager benötigte Active Directory-Authentifizierungsmethode. 

Informationen zum Strukturieren einer Anforderung in Azure Search finden Sie unter [Azure Search Service REST](https://docs.microsoft.com/rest/api/searchservice/) (Azure Search-Dienst: REST). Weitere Informationen zu Authentifizierungsanforderungen für Resource Manager finden Sie unter [Verwenden der Resource Manager-Authentifizierungs-API für den Zugriff auf Abonnements](../azure-resource-manager/resource-manager-api-authentication.md).

## <a name="user-access-to-index-content"></a>Benutzerzugriff auf den Indexinhalt

Benutzerspezifischer Zugriff auf den Inhalt eines Index wird über Sicherheitsfilter für Ihre Abfragen implementiert, sodass mit einer bestimmten Sicherheitsidentität verknüpfte Dokumente zurückgegeben werden. Anstelle vordefinierter Rollen und Rollenzuweisungen wird eine identitätsbasierte Zugriffssteuerung als Filter implementiert, die die Suchergebnisse von Dokumenten und Inhalten auf der Grundlage von Identitäten einschränken. In der folgenden Tabelle werden zwei Ansätze zur Einschränkung von Suchergebnissen für nicht autorisierte Inhalte beschrieben.

| Vorgehensweise | BESCHREIBUNG |
|----------|-------------|
|[Einschränkung aus Sicherheitsgründen auf der Grundlage von Identitätsfiltern](search-security-trimming-for-azure-search.md)  | Beschreibt den grundlegenden Workflow für die Implementierung der Zugriffssteuerung auf der Grundlage der Benutzeridentität. Der Artikel behandelt das Hinzufügen von Sicherheitsbezeichnern zu einem Index sowie das Filtern nach dem entsprechenden Feld, um unzulässige Inhalte aus den Ergebnissen zu entfernen. |
|[Einschränkung aus Sicherheitsgründen auf der Grundlage von Azure Active Directory-Identitäten](search-security-trimming-for-azure-search-with-aad.md)  | Dieser Artikel baut auf dem vorherigen Artikel auf und enthält Schritte zum Abrufen von Identitäten aus Azure Active Directory (AAD) – einem der [kostenlosen Dienste](https://azure.microsoft.com/free/) der Azure-Cloudplattform. |

## <a name="table-permissioned-operations"></a>Tabelle: Berechtigungsbasierte Vorgänge

Die folgende Tabelle enthält eine Zusammenfassung der in Azure Search zulässigen Vorgänge sowie Informationen dazu, welcher Schlüssel für den jeweiligen Vorgang erforderlich ist.

| Vorgang | Berechtigungen |
|-----------|-------------------------|
| Erstellen von Diensten | Azure-Abonnementinhaber|
| Skalieren von Diensten | Administratorschlüssel, RBAC-Rolle „Besitzer“ oder „Mitwirkender“ für die Ressource  |
| Löschen von Diensten | Administratorschlüssel, RBAC-Rolle „Besitzer“ oder „Mitwirkender“ für die Ressource |
| Erstellen/Ändern/Löschen von Objekten für den Dienst: <br>Indizes und Komponententeile (einschließlich Analysedefinitionen, Bewertungsprofile und CORS-Optionen), Indexer, Datenquellen, Synonyme und Vorschläge. | Administratorschlüssel, RBAC-Rolle „Besitzer“ oder „Mitwirkender“ für die Ressource  |
| Abfragen von Indizes | Administrator- oder Abfrageschlüssel (RBAC nicht verfügbar) |
| Abfragen von Systeminformationen (etwa zum Zurückgeben von Statistiken, Anzahlen und Objektlisten) | Administratorschlüssel, RBAC-Rolle für die Ressource („Besitzer“, „Mitwirkender“ oder „Leser“) |
| Verwalten von Administratorschlüsseln | Administratorschlüssel, RBAC-Rolle „Besitzer“ oder „Mitwirkender“ für die Ressource |
| Verwalten von Abfrageschlüsseln |  Administratorschlüssel, RBAC-Rolle „Besitzer“ oder „Mitwirkender“ für die Ressource.  |


## <a name="see-also"></a>Weitere Informationen

+ [Erste Schritte mit .NET (veranschaulicht das Erstellen eines Index unter Verwendung eines Administratorschlüssels)](search-create-index-dotnet.md)
+ [Erste Schritte mit REST (veranschaulicht das Erstellen eines Index unter Verwendung eines Administratorschlüssels)](search-create-index-rest-api.md)
+ [Identitätsbasierte Zugriffssteuerung mit Azure Search-Filtern](search-security-trimming-for-azure-search.md)
+ [Auf der Active Directory-Identität basierende Zugriffssteuerung mit Azure Search-Filtern](search-security-trimming-for-azure-search-with-aad.md)
+ [Filter in Azure Search](search-filters.md)