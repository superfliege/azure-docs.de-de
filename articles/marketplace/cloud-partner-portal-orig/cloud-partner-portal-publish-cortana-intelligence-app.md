---
title: Cortana Intelligence | Microsoft-Dokumentation
description: Veröffentlichen eines Cortana Intelligence-Angebots.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: pbutlerm
ms.openlocfilehash: f12a15a0d739ae6e98be3871fa8bb4104f49565c
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48806734"
---
# <a name="publish-a-cortana-intelligence-offer-using-the-cloud-partner-portal"></a>Veröffentlichen eines Cortana Intelligence-Angebots über das Cloud-Partnerportal

In diesem Artikel ist beschrieben, wie ein Cortana Intelligence-Angebot über das Cloud-Partnerportal veröffentlicht wird.

## <a name="prerequisites"></a>Voraussetzungen

Das Cloud-Partnerportal unterstützt rollenbasierten Zugriff auf das Portal, sodass Mitwirkende bei der Veröffentlichung eines Angebots zusammenarbeiten können. Weitere Informationen finden Sie unter [Verwalten von Benutzern im Cloud-Partnerportal](./cloud-partner-portal-manage-users.md).

Bevor ein Angebot im Auftrag eines Herausgeberkontos veröffentlicht werden kann, muss eine der Personen mit \"Besitzer\"-Rolle bestätigen, dass die [Nutzungsbedingungen](https://azure.microsoft.com/support/legal/website-terms-of-use/), die [Datenschutzerklärung von Microsoft](http://www.microsoft.com/privacystatement/default.aspx) und die [Microsoft Azure Certified-Programmvereinbarung](https://azure.microsoft.com/support/legal/marketplace/certified-program-agreement/) eingehalten werden.

## <a name="to-start-creating-a-cortana-inteligence-offer"></a>So beginnen Sie mit dem Erstellen eines Cortana Intelligence-Angebots

Sobald alle Voraussetzungen erfüllt sind, können Sie mit der Erstellung Ihres Cortana Intelligence-Angebots beginnen.

1.  Melden Sie sich beim [Cloud-Partnerportal](http://cloudpartner.azure.com/) an.
2.  Klicken Sie in der linken Navigationsleiste auf **+ Neues Angebot**.
3. Wählen Sie **Cortana Intelligence** aus.
4. Wählen Sie die Registerkarte **Editor** in der Ansicht **Neues Angebot** aus, um die folgenden Optionen zu konfigurieren:
    -   Angebotseinstellungen
    -   Technische Informationen
    -   Storefrontdetails
    -   Kontakte

    Für jede dieser Optionen wird ein Formular angezeigt, das Sie ausfüllen müssen.  Auf jedem Formular sind erforderliche Felder mit einem roten Sternchen (\*) gekennzeichnet.

## <a name="to-configure-offer-settings"></a>So konfigurieren Sie Angebotseinstellungen

Die Angebotseinstellungen stellen Basisinformationen über das Angebot bereit, z. B. die Angebots-ID, die Herausgeber-ID und den Angebotsnamen.

### <a name="offer-id"></a>Angebots-ID

Dies ist ein eindeutiger Bezeichner für das Angebot innerhalb eines Herausgeberprofils.
Diese ID ist in Produkt-URLs sichtbar. Sie darf nur aus klein geschriebenen alphanumerischen Zeichen oder Bindestrichen (-) bestehen. Die ID darf nicht mit einem Bindestrich enden und höchstens 50 Zeichen umfassen. 
>[!Note]
>Dieses Feld wird gesperrt, sobald ein Angebot live geschaltet wird.

**Beispiel:**

Wenn der Herausgeber **contoso** ein Angebot mit der Angebots-ID *Beispiel-Cortana Intelligence* erstellt, wird dieses Angebot in AppSource als „https://appsource.microsoft.com/marketplace/apps/**contoso**.*Beispiel-Cortana Intelligence*?tab=Overview“ angezeigt.

### <a name="publisher-id"></a>Herausgeber-ID

Eine Dropdownliste mit Herausgeberprofilen. Verwenden Sie diese Dropdownliste, um das Herausgeberprofil auszuwählen, unter dem das Angebot veröffentlicht werden soll.

>[!Note]
>Dieses Feld wird gesperrt, sobald ein Angebot live geschaltet wird.

### <a name="name"></a>NAME

Der Anzeigenamen für Ihr Angebot. Dieser Name wird in [AppSource](https://appsource.microsoft.com) angezeigt. Er darf aus höchstens 50 Zeichen bestehen.

Wenn Sie alle Informationen, die in den Angebotseinstellungen erforderlich sind, eingegeben haben, wählen Sie **Speichern** aus, um zu dem Angebotsbereich für die technischen Informationen zu gelangen. Option.

## <a name="to-configure-technical-info"></a>So konfigurieren Sie „Technische Informationen“

In dieser Ansicht stellen Sie die technischen Informationen bereit, die auf Ihrer Angebotsseite angezeigt werden. Die folgenden Felder gehören zu diese Ansicht.

### <a name="partner-mpn-id"></a>Partner-MPN-ID

Wenn Sie registrierter Microsoft-Partner sind, wechseln Sie zur [Partner-Website](https://partners.microsoft.com/), und melden Sie sich an, um die Partner-ID Ihrer Organisation abzurufen. Geben Sie diese ID als **Partner-MPN-ID** ein.

### <a name="analytics-components-used"></a>Verwendete Analysekomponenten

Wählen Sie alle Komponenten aus, die in Ihrem Angebot verwendet werden. Wählen Sie mindestens eine Komponente aus. Wählen Sie **Microsoft R** nur aus, wenn Sie eine der MRS-Lizenzen von SQL 2016 R-Diensten, HDInsight Premium oder MRS mit Ausführung auf VMs verwenden.

### <a name="additional-components-used"></a>Weitere verwendete Komponenten

Wählen Sie alle anderen Komponenten aus, die in Ihrer Lösung verwendet werden.

### <a name="customer-name-using-solution"></a>Name des Kunden, der die Lösung verwendet

Geben Sie den Namen des Kunden an, der diese Lösung in der Produktion verwendet. 

>[!Note]
>Diese Information wird nicht in AppSource veröffentlicht. Sie wird nur für die Evaluation der Lösung verwendet.

### <a name="azure-consumption-requirement-met"></a>Anforderungen an Azure-Nutzung erfüllt?

Geben Sie an, ob die Lösung mindestens 1.000 USD pro Monat pro Kunde der Azure-Suite-Komponenten generiert oder ob in der Lösung Microsoft R verwendet wird.

>[!Note]
>Diese Information wird nicht in AppSource veröffentlicht. Sie wird nur für die Evaluation der Lösung verwendet.

### <a name="demo-video-url"></a>Demovideo-URL

Geben Sie eine URL für ein Demovideo für die Lösung bzw. Anwendung an, das Ihr Vertriebsteam Kunden zeigen kann. 

>[!Note]
>Diese Information wird nicht in AppSource veröffentlicht. Sie wird nur für die Evaluation der Lösung verwendet.

#### <a name="demo-video-guidelines"></a>Richtlinien für Demovideos

- Ein Video sollte maximal 15 Minuten lang sein.
- Das Video sollte eine nur wenig bearbeitete Aufzeichnung der Funktionalität der Lösung sein. 
- Das Video sollte die Schlüsselaspekte der für einen Benutzer sichtbaren Funktionalität hervorheben und sich auf die Integration von erweiterten Analysen konzentrieren. 
- Die Demovideos **werden nicht** öffentlich für die Kunden verfügbar gemacht, sondern sollen widerspiegeln, wie die Lösung für potenzielle Kunden präsentiert *werden würde*. Daher sollten sie skriptbasiert und wiederholbar sein.
- Verwenden Sie zum Erstellen Ihres Videos ein beliebiges Tool für Bildschirmaufzeichnungen, mit dem ein Standardvideoformat (z. B. MPEG) exportiert werden kann. 

In den folgenden Anweisungen wird gezeigt, wie Sie ein Video mit Skype for Business erstellen. 

1. [Starten einer Skype for Business-Telefonkonferenz](https://support.office.com/article/Start-a-Skype-for-Business-conference-call-8dc8ac52-91ac-4db9-8672-11551fdaf997)
2. [Freigeben Ihres Bildschirms in Skype for Business](https://support.office.com/article/Share-your-screen-in-Skype-for-Business-2d436dc9-d092-4ef1-83f1-dd9f7a7cd3fc)
3. [Starten einer Aufzeichnung](https://support.office.com/article/Share-sites-or-documents-with-people-outside-your-organization-80e49744-e30f-44db-8d51-16661b1d4232)
4. Nachdem Sie die Aufzeichnung beendet haben, [verwenden Sie den Aufzeichnungs-Manager zur Veröffentlichung der Aufzeichnung](https://support.office.com/article/Recording-Manager-save-and-publish-59a3beb7-c700-40cf-ab21-bc82a2b06351)

Laden Sie Ihr aufgezeichnetes Video in einen Dienst hoch, der es Ihnen ermöglicht, eine freigegebene URL zu generieren. Dies kann beispielsweise ein [Gästelink in OneDrive oder Sharepoint](https://support.office.com/article/Share-sites-or-documents-with-people-outside-your-organization-80e49744-e30f-44db-8d51-16661b1d4232) sein.

### <a name="supported-regions"></a>Unterstützte Regionen

Wählen Sie alle Regionen aus, die von Ihre Lösung unterstützt werden. Wählen Sie mindestens eine Region aus.

### <a name="power-bi-desktop-file-pbix"></a>Power BI Desktop-Datei (PBIX)

Sofern zutreffend, laden Sie eine PBIX-Datei hoch. Stellen Sie sicher, dass die Daten in die Datei importiert und nicht extern referenziert werden. Wir erstellen den eingebetteten Bericht für Sie.

### <a name="solution-architecture"></a>Lösungsarchitektur

Laden Sie ein Dokument hoch, in dem die Architektur Ihrer Lösung beschrieben ist. Eine Richtlinien zum Hochladen des Architekturdiagramms einer Lösung finden Sie in [Advanced Analytics Solution Workflow Template](https://partnersprofilesint.blob.core.windows.net/partner-assets/documents/AppSource%20Solution%20Publishing%20Guide%20Docs/Advanced%20Analytics%20Solution%20Workflow%20Template%20V.2017.3.23.pptx).

>[!Note]
>Diese Information wird nicht in AppSource veröffentlicht. Sie wird nur für die Evaluation der Lösung verwendet.

### <a name="select-segments"></a>Segmente auswählen

Wählen Sie alle zugehörigen Branchensegmente aus. Ist Ihre Anwendung für ein nicht aufgeführtes Segment vorgesehen, geben Sie den Segmentnamen in das Feld **Andere** ein. Sie können maximal drei Wörter für das Segment eingeben.

### <a name="select-business-processes"></a>Geschäftsprozesse auswählen

Wählen Sie die Geschäftsprozesse aus, die Ihre Lösung am besten beschreiben. Ist Ihre Anwendung für einen nicht aufgeführten Prozess vorgesehen, geben Sie den Prozessnamen in das Feld **Andere** ein. Sie können maximal drei Wörter für den Prozess eingeben.

### <a name="trial-info"></a>Testinformationen

-   **URL für SaaS-Test:** Geben Sie die URL für die Testbenutzeroberfläche Ihrer Anwendung ein.
-   **URL für Testversion:** Geben Sie die URL für die Testversionsoberfläche Ihrer Anwendung ein.

Weitere Informationen zu Testmöglichkeiten finden Sie unter **App-Typ** im nächsten Abschnitt dieses Artikels.

Wenn Sie alle Informationen, die in den technischen Informationen erforderlich sind, eingegeben haben, wählen Sie **Speichern** aus, um zu dem Angebotsbereich für die Storefrontdetails zu gelangen. 

## <a name="to-configure-storefront-details"></a>So konfigurieren Sie die Storefrontdetails

### <a name="offer-summary"></a>Angebotszusammenfassung

Dies ist eine Zusammenfassung des Nutzenversprechens Ihres Angebots. Die Zusammenfassung wird auf der Suchseite Ihres Angebots angezeigt. Die jeweilige Zusammenfassung darf maximal 100 Zeichen lang sein.

### <a name="offer-description"></a>Beschreibung des Angebots

Dies ist die Beschreibung, die auf der Detailseite Ihrer Anwendung angezeigt wird.
Die maximale Länge der Beschreibung beträgt 1300 Zeichen.

Beachten Sie, dass für dieses Feld HTML-Inhalt mit Tags wie \<p\>, \<h1\>, \<h2\>, \<li\> usw. unterstützt wird, wodurch es Ihnen ermöglicht wird, den Inhalt viel ansprechender zu gestalten. Das für Portalveröffentlichungsfunktionen zuständige Team arbeitet daran, eine Funktion hinzuzufügen, mit der eine Vorschau der Storefrontdetails angezeigt werden kann, um den Inhalt auf iterative Weise ansprechender zu gestalten. In der Zwischenzeit können Sie jedes online verfügbare Echtzeit-HTML-Tool wie [http://htmledit.squarefree.com](http://htmledit.squarefree.com/) verwenden, um zu sehen, wie Ihre Beschreibung aussieht.

Es wird empfohlen, den Text der Beschreibung in Unterabschnitte aufzuteilen, die sich an den Nutzenversprechen orientieren und jeweils durch einen Untertitel hervorgehoben werden. Besucher werfen in der Regel einen kurzen Blick auf das Feld „Angebotszusammenfassung“ und die Untertitel, um sich einen Überblick darüber zu verschaffen, welches Problem die App anspricht und warum sie die App in Betracht ziehen sollten. Deshalb ist es wichtig, die Aufmerksamkeit der Nutzer zu erhalten und ihnen einen Grund zu geben, sich die Einzelheiten durchzulesen.

#### <a name="partner-examples"></a>Partner-Beispiele

- [Bestandsoptimierung bei Neal Analytics](https://appsource.microsoft.com/product/web-apps/neal_analytics.8066ad01-1e61-40cd-bd33-9b86c65fa73a?tab=Overview)
- [Plexure Retail Personalization (Einzelhandelpersonalisierung)](https://appsource.microsoft.com/product/web-apps/plexure.c82dc2fc-817b-487e-ae83-1658c1bc8ff2?tab=Overview)
- [AvePoint Citizen Services (Bürgerdienste)](https://appsource.microsoft.com/product/web-apps/avepoint.7738ac97-fd40-4ed3-aaab-327c3e0fe0b3?tab=Overview)

### <a name="industries"></a>Branchen

Wählen Sie die Branchen aus, für die Ihre Anwendung (App) am besten geeignet ist. Ist Ihre App für mehrere Branchen nutzbar, belassen Sie dieses Feld leer.

### <a name="categories"></a>Categories

Wählen Sie die Kategorien aus, die für Ihre App relevant sind. Sie können maximal zwei Kategorien auswählen.

### <a name="app-type"></a>App-Typ

Wählen Sie den Testtyp aus, der für Ihre App in AppSource unterstützt wird. Wählen Sie einen der folgenden Testtypen aus:
- **Kostenlos** bedeutet, dass Ihre App ist kostenlos.
- **Testversion** bedeutet, dass Kunden Ihre App für einen bestimmten Zeitraum testen können.
- **Testversion anfordern** bedeutet, dass Kunden eine Testversion der App anfordern können.

Partner können zwei Typen von Testversionen in AppSource bereitstellen.

- Für die Option **Testversion**, die auch als **kundengesteuerte Testversion** bezeichnet wird, kann einer der folgenden Typen verwendet werden: 
    - SaaS-Testversion
        - Der Kunde kann zu einer URL navigieren, die Sie oder Ihr Partner bereitstellen. Der Kunde durchläuft die einmalige AAD-Verbundanmeldung, damit ihm eine Testversion mit Timeboxing bereitgestellt wird.
        - Dies ist eine SaaS-Anwendung (App) mit mehreren Mandanten, in der die Konfiguration bzw. Daten eines Benutzers zu anderen Benutzern isoliert sind. In dieser Testversion kann aber auch nur eine Teilmenge bereitgestellt werden, in der nur schreibgeschützte Vorgänge verwendet werden.

        **Beispiele:**

        - [AFS POP Retail Execution](https://appsource.microsoft.com/product/web-apps/afs.fa9fc926-3bc3-43dd-becd-3ef41b52c10b?tab=Overview)
        - [AvePoint Citizen Services (Bürgerdienste)](https://appsource.microsoft.com/product/web-apps/avepoint.7738ac97-fd40-4ed3-aaab-327c3e0fe0b3?tab=Overview). (Diese App bietet eine geführte Oberfläche mit klaren Pfaden für einen ausgewählte Gruppe von Benutzern.)

     - Testversion
        - Ihre Lösung (oder die Ihres Partners) oder eine Teilmenge kann mit Azure Resource Manager-Vorlagen konfiguriert werden, die AppSource instanziieren und in einem Azure-Partnerabonnement mit Timeboxing und der Verwaltung von aktiven/inaktiven Instanzenpools usw. verwalten kann.
        - Wenn Sie sich für diese Option entscheiden, können wir zur Unterstützung Vorlagen und Beispielcode bereitstellen.

        **Beispiele:**

        - [Bestandsoptimierung bei Neal Analytics](https://appsource.microsoft.com/product/web-apps/neal_analytics.8066ad01-1e61-40cd-bd33-9b86c65fa73a?tab=Overview&tag=CISHome)

- Die Option **Testversion anfordern** (**partnergesteuerte Testversionen**) erfordert, dass der Kunde ein Kontaktformular ausfüllt, um Partnernachverfolgung zu ermöglichen. Der Partner verfolgt nach und stellt eine Demonstration oder Testversion der App bereit. Weitere Informationen finden Sie im Video [AppSource trial experience walkthrough](http://aka.ms/trialexperienceforwebapps), das eine allgemeine Übersicht bietet.

>[!Note]
>Durch Daten lässt sich belegen, dass **kundengesteuerte Testversionen** ein höhere Potenzial zur Leadgenerierung haben als **partnergesteuerte Testversionen**.


### <a name="help-link-for-your-app"></a>Hilfe-Link für Ihre App

Stellen Sie eine URL zu einer Seite bereit, die Hilfeinformationen für Ihre App enthält.

### <a name="supported-countriesregions"></a>Unterstützte Länder/Regionen

In diesem Feld werden die Länder/Regionen bestimmt, in denen Ihr Angebot für Testzwecke zur Verfügung steht.

![Unterstützte Länder/Regionen](./media/cloud-partner-portal-publish-cortana-intelligence-app/CISScreenshot3.png)

### <a name="supported-languages"></a>Unterstützte Sprachen

Wählen Sie die Sprachen, die von Ihrer App unterstützt werden. Wenn Ihre App Sprachen unterstützt, die nicht in dieser Liste enthalten sind, veröffentlichen Sie Ihr Angebot, und senden Sie uns eine E-Mail (<appsource@microsoft.com>), um uns über die Unterstützung anderer Sprachen zu informieren.

### <a name="app-version"></a>App-Version

Geben Sie die Versionsnummer Ihrer App ein.

### <a name="products-your-app-works-with"></a>Produkte, mit denen Ihre App zusammenarbeitet

Listen Sie die bestimmten Produkte auf, mit denen Ihre App zusammenarbeitet. Sie können bis zu drei Produkte auflisten. Um ein Produkt aufzulisten, wählen Sie das **Pluszeichen** (neben „Neu“) aus. Daraufhin wird ein Textfeld für Sie erstellt. Geben Sie den Namen eines Produkts ein, mit dem Ihre App zusammenarbeitet.

### <a name="hide-key"></a>Ausblendschlüssel

Dies ist ein Schlüssel, der mit der Vorschau-URL des Angebots kombiniert wird, um das Angebot aus der öffentlichen Ansicht auszublenden. Es ist kein Kennwort. Sie können eine beliebige alphanummerische Zeichenfolge eingeben.

### <a name="offer-logo-small"></a>Angebotslogo (klein)

Dieses Logo wird auf der Suchseite Ihrer App angezeigt. Das PNG-Bildformat ist das einzige unterstützte Format. Die Bildgröße ist 48 x 48 Pixel.

### <a name="offer-logo-large"></a>Angebotslogo (groß)

Dieses Logo wird auf der Detailseite Ihrer App angezeigt. Das PNG-Bildformat ist das einzige unterstützte Format. Die Bildgröße ist 48 x 48 Pixel.

### <a name="video"></a>Video

Sie können bis zu vier Videos hochladen. Für jedes Video, das Sie hochladen möchten, müssen Sie Folgendes angeben:
- Den Namen des Videos
- Eine URL (nur YouTube oder Vimeo)
- Ein Miniaturbild, das dem Videos zugeordnet werden soll. Das Miniaturbild muss im PNG-Bildformat vorliegen und eine Größe von 1280 x 720 Pixel haben. 

Um neue Videos hinzuzufügen, wählen Sie **+ Neu** aus (siehe nächstes Bildschirmfoto).

![Neues Video hinzufügen](./media/cloud-partner-portal-publish-cortana-intelligence-app/CISScreenshot4.png)

### <a name="document"></a>Dokument

Sie können bis zu drei Dokumente hochladen. Jedes Dokument muss im PDF-Dateiformat vorliegen. So fügen Sie ein neues Dokument hinzu:

- Wählen Sie **+ Neu** aus.
- Geben Sie einen Dokumentnamen ein.
- Wählen Sie **Hochladen** aus, um ein Dokument hochzuladen.

![Neues Dokument hinzufügen](./media/cloud-partner-portal-publish-cortana-intelligence-app/CISScreenshot5.png)

### <a name="privacy-policy"></a>Datenschutzrichtlinie

Geben Sie die URL für die Datenschutzrichtlinie Ihrer App ein.

### <a name="terms-of-use"></a>Nutzungsbedingungen

Geben Sie die Nutzungsbedingungen Ihrer App an. AppSource-Kunden müssen diese Bedingungen akzeptieren, bevor sie Ihre App testen können.

>[!Note]
>In diesem Feld wird Inhalt mit HTML-Tags unterstützt, z. B.  <p\>, <h1\> und <li\>. Mit diesen Tags können Sie den jeweiligen Inhalt formatieren. 

### <a name="lead-destination"></a>Leadzielgruppe

Wählen Sie ein CRM-System aus, in dem Ihre Leads gespeichert werden. 

Wählen Sie **Azure-Tabelle** aus, wenn Sie eines der folgenden CRM-Systeme verwenden: Salesforce, Marketo oder Microsoft Dynamics CRM. 

Wenn Sie weitere Informationen über das CRM-System wünschen, das Sie verwenden möchten, wählen Sie einen der folgenden Links für die unterstützten Systeme aus.

-   [Azure-Tabelle](./cloud-partner-portal-lead-management-instructions-azure-table.md)
-   [Marketo](./cloud-partner-portal-lead-management-instructions-marketo.md)
-   [Microsoft Dynamics CRM](./cloud-partner-portal-lead-management-instructions-dynamics.md)
-   [Salesforce](./cloud-partner-portal-lead-management-instructions-salesforce.md)
