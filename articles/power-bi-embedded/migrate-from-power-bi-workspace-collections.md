---
title: Migrieren von Power BI-Arbeitsbereichssammlungsinhalten zu Power BI Embedded | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie Power BI-Arbeitsbereichssammlungen zu Power BI Embedded migrieren und die Einbettung in Apps durchführen."
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 09/28/2017
ms.author: asaxton
ms.openlocfilehash: 069f31c8213bd0d8586f7ca50e543acfdad8a2b3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-migrate-power-bi-workspace-collection-content-to-power-bi-embedded"></a>Migrieren von Power BI-Arbeitsbereichssammlungsinhalten zu Power BI Embedded

In diesem Artikel erfahren Sie, wie Power BI-Arbeitsbereichssammlungen zu Power BI Embedded migriert werden. Dieser Artikel enthält Anweisungen zum Migrieren von Inhalten aus den Azure Power BI-Arbeitsbereichssammlungen zu Power BI Embedded. Zudem werden die erwarteten Änderungen an der Anwendung beschrieben.

Die Ressource mit den Power BI-Arbeitsbereichssammlungen bleibt nach der allgemeinen Verfügbarkeit der Power BI Premium-Version für einen begrenzten Zeitraum weiterhin verfügbar. Kunden mit einem Enterprise Agreement haben bis zum Ablauf ihrer bestehenden Vereinbarungen Zugriff auf ihre vorhandenen Arbeitsbereichssammlungen. Kunden, die Power BI-Arbeitsbereichssammlungen über Direct- oder CSP-Kanäle geladen haben, haben für ein Jahr ab der allgemeinen Verfügbarkeit von Power BI Premium Zugriff.

> [!IMPORTANT]
> Während die Migration vom Power BI-Dienst abhängig ist, besteht bei Verwendung eines **Einbettungstokens** für die Benutzer Ihrer Anwendung keine Abhängigkeit von Power BI. Die Benutzer müssen sich nicht für Power BI registrieren, um die eingebetteten Inhalte in der Anwendung anzuzeigen. Für Ihre Kunden können Sie diese Methode für die Einbettung von Power BI nutzen.

![Ablauf für Power BI Embedded](media/migrate-from-power-bi-workspace-collections/powerbi-embed-flow.png)

## <a name="prepare-for-the-migration"></a>Vorbereiten der Migration

Für die Migration von Elementen aus Power BI-Arbeitsbereichssammlungen zu Power BI Embedded bedarf es einiger Vorbereitungen. Sie benötigen einen verfügbaren Mandanten sowie einen Benutzer mit einer Power BI Pro-Lizenz.

1. Stellen Sie sicher, dass Sie Zugriff auf einen Azure Active Directory-Mandanten (Azure AD) haben.

    Welcher Mandant soll verwendet werden?

    * Soll der vorhandene Power BI-Mandant Ihres Unternehmens verwendet werden?
    * Soll ein separater Mandant für die Anwendung verwendet werden?
    * Soll jeweils ein separater Mandant für jeden Kunden verwendet werden?

    Wenn Sie einen neuen Mandanten für die Anwendung oder für jeden Kunden erstellen möchten, finden Sie entsprechende Informationen in den folgenden Artikeln:

    * [Erstellen eines Azure Active Directory-Mandanten](https://powerbi.microsoft.com/documentation/powerbi-developer-create-an-azure-active-directory-tenant/)
    * [Einrichten eines Azure Active Directory-Mandanten](https://docs.microsoft.com/azure/active-directory/develop/active-directory-howto-tenant)

2. Erstellen Sie einen Benutzer in diesem neuen Mandanten, der als „master“-Konto der Anwendung dient. Dieses Konto muss für Power BI registriert werden. Zudem muss dem Konto eine Power BI Pro-Lizenz zugewiesen sein.

## <a name="accounts-within-azure-ad"></a>Konten in Azure AD

Die folgenden Konten müssen in Ihrem Mandanten vorhanden sein.

> [!NOTE]
> Diesen Konten müssen Power BI Pro-Lizenzen zugewiesen sein, damit App-Arbeitsbereiche verwendet werden können.

1. Ein Administratorbenutzer für den Mandanten.

    Es wird empfohlen, dass der Mandantenadministrator im einbettenden App-Arbeitsbereich als Mitglied aufgeführt ist.

2. Konten für Analysten, die Inhalte erstellen.

    Diese Benutzer sollten nach Bedarf App-Arbeitsbereichen zugewiesen werden.

3. Ein *master*-Benutzerkonto für die Anwendung oder ein Dienstkonto.

    Die Anmeldeinformationen für dieses Konto werden im Back-End der Anwendung gespeichert. Verwenden Sie das *master*-Konto zum Abrufen eines Azure AD-Tokens zur Verwendung mit den Power BI-REST-APIs. Über dieses Konto wird das Einbettungstoken für die Anwendung generiert. Das *master*-Konto muss ein Administrator der für die Einbettung erstellten App-Arbeitsbereiche sein.

    **Dieses Konto ist einfach ein normales Benutzerkonto in Ihrer Organisation, das für die Einbettung verwendet wird.**

## <a name="app-registration-and-permissions"></a>App-Registrierung und Berechtigungen

Registrieren Sie eine Anwendung in Azure AD, um Aufrufe von REST-APIs durchführen zu können. Zusätzlich zur Power BI-App-Registrierungsseite wird die Konfiguration im Microsoft Azure-Portal angewendet. Weitere Informationen finden Sie unter [Registrieren einer Azure AD-App zum Einbetten von Power BI-Inhalten](https://powerbi.microsoft.com/documentation/powerbi-developer-register-app/).

Es empfiehlt sich, die Anwendung über das **master**-Konto der Anwendung zu registrieren.

## <a name="create-app-workspaces-required"></a>Erstellen von App-Arbeitsbereichen (erforderlich)

Wenn die Anwendung für mehrere Kunden bereitgestellt wird, können Sie mithilfe von App-Arbeitsbereichen eine bessere Isolierung erreichen. Die einzelnen Dashboards und Berichte sind dann für Ihre Kunden voneinander getrennt. Zudem können Sie jeweils ein Power BI-Konto pro App-Arbeitsbereich verwenden, um die Anwendungsnutzung der Kunden weiter zu isolieren. Der Einfachheit halber können Sie jedoch auch nur ein Konto verwenden.

> [!IMPORTANT]
> Sie können keinen persönlichen Arbeitsbereich („Eigener Arbeitsbereich“) verwenden, um die Einbettung für Ihre Kunden durchzuführen.

Sie benötigen einen Benutzer mit einer Pro-Lizenz, um einen App-Arbeitsbereich in Power BI erstellen zu können. Der Power BI-Benutzer, der den App-Arbeitsbereich erstellt, ist standardmäßig als Administrator für diesen Arbeitsbereich festgelegt. **Das *master*-Konto der Anwendung muss Administrator des Arbeitsbereichs sein.**

## <a name="content-migration"></a>Migration von Inhalten

Die Migration der Inhalte aus Ihren Arbeitsbereichssammlungen zu Power BI Embedded kann parallel zu Ihrer aktuellen Lösung erfolgen und erfordert keine Downtime.

Zur Unterstützung des Kopierens von Inhalten aus Power BI-Arbeitsbereichssammlungen in Power BI Embedded steht Ihnen ein **Migrationstool** zur Verfügung. Dies ist besonders nützlich, wenn viele Berichte migriert werden sollen. Weitere Informationen finden Sie unter [Verwenden des Power BI Embedded-Migrationstools](migrate-tool.md).

Die Migration von Inhalten basiert hauptsächlich auf zwei APIs.

1. Download PBIX: Über diese API können PBIX-Dateien heruntergeladen werden, die nach Oktober 2016 in Power BI hochgeladen wurden.
2. Import PBIX: Über diese API werden alle PBIX-Dateien in Power BI hochgeladen.

Entsprechende Codeausschnitte finden Sie unter [Codeausschnitte zum Migrieren von Inhalten aus Power BI-Arbeitsbereichssammlungen](migrate-code-snippets.md).

### <a name="report-types"></a>Berichtstypen

Es stehen mehrere Berichtstypen zur Verfügung, für die jeweils ein unterschiedlicher Migrationsablauf erforderlich ist.

#### <a name="cached-dataset-and-report"></a>Zwischengespeichertes DataSet und zwischengespeicherter Bericht

Zwischengespeicherte DataSets beziehen sich auf PBIX-Dateien, die importierte Daten enthalten, im Gegensatz zu einer Liveverbindung oder DirectQuery-Verbindung.

**Ablauf**

1. Rufen Sie die Download PBIX-API über Ihren Arbeitsbereich der Power BI-Arbeitsbereichssammlung auf.
2. Speichern Sie die PBIX-Datei.
3. Rufen Sie die Import PBIX-API für Ihren Power BI Embedded-Arbeitsbereich auf.

#### <a name="directquery-dataset-and-report"></a>DirectQuery-DataSet und DirectQuery-Bericht

**Ablauf**

1. Rufen Sie „GET https://api.powerbi.com/v1.0/collections/{Sammlungs-ID}/workspaces/{Arbeitsbereichs-ID}/datasets/{DataSet-ID}/Default.GetBoundGatewayDataSources“ auf, und speichern Sie die empfangene Verbindungszeichenfolge.
2. Rufen Sie die Download PBIX-API über Ihren Arbeitsbereich der Power BI-Arbeitsbereichssammlung auf.
3. Speichern Sie die PBIX-Datei.
4. Rufen Sie die Import PBIX-API für Ihren Power BI Embedded-Arbeitsbereich auf.
5. Aktualisieren Sie die Verbindungszeichenfolge durch Aufrufen von „POST https://api.powerbi.com/v1.0/myorg/datasets/{DataSet-ID}/Default.SetAllConnections“.
6. Rufen Sie die Gateway-ID und Datenquellen-ID durch Aufrufen von „GET https://api.powerbi.com/v1.0/myorg/datasets/{DataSet-ID}/Default.GetBoundGatewayDataSources“ ab.
7. Aktualisieren Sie die Anmeldeinformationen des Benutzers durch Aufrufen von „PATCH https://api.powerbi.com/v1.0/myorg/gateways/{Gateway-ID}/datasources/{Datenquellen-ID}“.

#### <a name="old-dataset-and-reports"></a>Altes DataSet und alte Berichte

Berichte, die vor Oktober 2016 hochgeladen wurden, unterstützen das Feature „Download PBIX“ nicht.

**Ablauf**

1. Rufen Sie PBIX von Ihrer Entwicklungsumgebung (Ihre interne Quellcodeverwaltung) ab.
2. Rufen Sie die Import PBIX-API für Ihren Power BI Embedded-Arbeitsbereich auf.

#### <a name="push-dataset-and-report"></a>Push-DataSet und Push-Bericht

Download PBIX unterstützt keine *Push API*-Datasets. Daten des Push API-DataSets können nicht aus Power BI-Arbeitsbereichssammlungen in Power BI Embedded importiert werden.

**Ablauf**

1. Rufen Sie die Create DataSet-API mit dem JSON-Code des DataSets auf, um ein DataSet für Ihren Power BI Embedded-Arbeitsbereich zu erstellen.
2. Erstellen Sie den Bericht für das erstellte DataSet neu*.

Es ist möglich, einige Problemumgehungen zum Migrieren des Push API-Berichts aus Power BI-Arbeitsbereichssammlungen zu Power BI Embedded zu verwenden, indem Sie Folgendes versuchen:

1. Laden Sie einige PBIX-Dummydateien in Ihren Arbeitsbereich der BI-Arbeitsbereichssammlung hoch.
2. Klonen Sie den Push API-Bericht, und binden Sie ihn an eine PBIX-Dummydatei aus Schritt 1.
3. Laden Sie den Push API-Bericht mit der PBIX-Dummydatei herunter.
4. Laden Sie die PBIX-Dummydatei in Ihren Power BI Embedded-Arbeitsbereich hoch.
5. Erstellen Sie ein Push-DataSet in Ihrem Power BI Embedded-Arbeitsbereich.
6. Binden Sie den Bericht neu an das DataSet der Push API.

## <a name="create-and-upload-new-reports"></a>Erstellen und Hochladen neuer Berichte

Zusätzlich zu den Inhalten, die Sie aus Power BI-Arbeitsbereichssammlungen migriert haben, können Sie Berichte und DataSets mithilfe von Power BI Desktop erstellen und dann in einem App-Arbeitsbereich veröffentlichen. Endbenutzer, die die Berichte veröffentlichen, müssen über eine Power BI Pro-Lizenz verfügen, um sie in einem App-Arbeitsbereich veröffentlichen zu können.

## <a name="rebuild-your-application"></a>Neuerstellen der Anwendung

1. Ändern Sie Ihre Anwendung so, dass die Power BI-REST-APIs und der Berichtsspeicherort in powerbi.com verwendet werden.

2. Erstellen Sie die AuthN/AuthZ-Authentifizierung über das *master*-Konto für die Anwendung neu. Sie können ein [Einbettungstoken](https://msdn.microsoft.com/library/mt784614.aspx) nutzen, um diesem Benutzer zu ermöglichen, im Namen anderer Benutzer zu agieren.

3. Betten Sie Ihre Berichte aus Power BI Embedded in die Anwendung ein. Weitere Informationen finden Sie unter [Einbetten von Power BI-Dashboards, -Berichten und -Kacheln](https://powerbi.microsoft.com/documentation/powerbi-developer-embedding-content/).

## <a name="map-your-users-to-a-power-bi-user"></a>Zuordnen von Benutzern zu einem Power BI-Benutzer

In Ihrer Anwendung können Sie Benutzer, die Sie in der Anwendung verwalten, den Anmeldeinformationen eines *master*-Power BI-Kontos für Ihre Anwendung zuordnen. Die Anmeldeinformationen für dieses *master*-Power BI-Konto werden in der Anwendung gespeichert und können zum Erstellen von Einbettungstoken verwendet werden.

## <a name="what-to-do-when-you-are-ready-for-production"></a>Vorgehensweise für die Verwendung in der Produktion

Wenn Sie bereit für den Übergang in die Produktion sind, sollten Sie wie folgt vorgehen:

- Wenn Sie einen separaten Mandanten für die Entwicklung verwenden, müssen Sie sicherstellen, dass Ihre App-Arbeitsbereiche sowie die Dashboards und Berichte in Ihrer Produktionsumgebung verfügbar sind. Vergewissern Sie sich, dass Sie wie in Schritt 1 angegeben die Anwendung in Azure AD für den Produktionsmandanten erstellt und die entsprechenden App-Berechtigungen zugewiesen haben.

- Erwerben Sie eine Kapazität, die Ihren Anforderungen entspricht. Entsprechende Informationen finden Sie im [Embedded analytics capacity planning whitepaper](https://aka.ms/pbiewhitepaper) (Whitepaper zur Kapazitätsplanung in Power BI Embedded). Sie können eine Power BI Embedded-Ressource über das Azure-Portal erwerben.

- Bearbeiten Sie den App-Arbeitsbereich, und weisen Sie ihn unter „Erweitert“ einer Kapazität zu.

    ![Zuweisen des App-Arbeitsbereichs zu einer Kapazität in powerbi.com](media/migrate-from-power-bi-workspace-collections/embedded-capacity.png)

- Stellen Sie die aktualisierte Anwendung in der Produktionsumgebung bereit, und betten Sie Berichte aus Power BI Embedded ein.

## <a name="after-migration"></a>Nach der Migration

In den Power BI-Arbeitsbereichssammlungen sind einige Löschvorgänge erforderlich.

- Entfernen Sie alle Arbeitsbereiche aus der bereitgestellten Lösung im Azure-Dienst der Power BI-Arbeitsbereichssammlungen.
- Löschen Sie alle Arbeitsbereichssammlungen, die in Azure vorhanden sind.

## <a name="next-steps"></a>Nächste Schritte

Herzlichen Glückwunsch. Ihre Anwendung wurde nun zu Power BI Embedded migriert. Informationen zum Einbetten von Power BI-Dashboards, -Berichten und -DataSets finden Sie unter [Einbetten von Power BI-Dashboards, -Berichten und -Kacheln](https://powerbi.microsoft.com/documentation/powerbi-developer-embedding-content/).

Weitere Fragen? [Fragen Sie die Power BI-Community](http://community.powerbi.com/).