---
title: Azure- und Dynamics 365-Beratungsdienstangebot | Microsoft-Dokumentation
description: Dies ist ein Leitfaden für die Definition und Veröffentlichung eines Azure- oder Dynamics 365-Beratungsdienstangebots im Cloud-Partnerportal.
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
ms.date: 11/01/2018
ms.author: pbutlerm
ms.openlocfilehash: 7ea4a58f072071f60cf87d1e8817ae19658be5fb
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2018
ms.locfileid: "53310418"
---
# <a name="azure-and-dynamics-365-consulting-service-offer"></a>Azure- und Dynamics 365-Beratungsdienstangebot

<table> <tr> <td>Dieser Abschnitt erläutert, wie Sie einen Beratungsdienst im Microsoft <a href="https://azuremarketplace.microsoft.com">Azure Marketplace</a> oder im <a href="https://appsource.microsoft.com">AppSource Marketplace</a> veröffentlichen. Lösungen, die auf Microsoft <a href="https://dynamics.microsoft.com">Dynamics 365</a>, <a href="https://products.office.com">Office 365</a>, <a href="https://powerbi.microsoft.com">Power BI</a> und <a href="https://powerapps.microsoft.com">PowerApps</a> basieren, können in AppSource gelistet werden. Andere Angebote, die auf anderen Microsoft <a href="https://azure.microsoft.com/services">Azure-Diensten</a> basieren, können im Azure Marketplace gelistet werden. </td> <td><img src="./media/consulting-services-publishing-offer/consulting-icon1.png"  alt="Microsoft consulting icon" /></td> </tr> </table>


## <a name="publishing-benefits"></a>Vorteile der Veröffentlichung

Die Veröffentlichung in einem der Marketplaces von Microsoft bietet erhebliche Vorteile:

- Bewerben Sie Ihr Unternehmen durch Nutzung der Marke Microsoft.
- In AppSource können Sie mehr als 100 Millionen Office 365- und Dynamics 365-Benutzer erreichen, in Azure Marketplace über 200.000 Organisationen.
- Gewinnen Sie hochwertige Leads in diesen Marketplaces.
- Lassen Sie Ihre Dienste durch die Außendienst- und Telefonmarketingteams von Microsoft bewerben.


## <a name="define-your-consulting-services-offer"></a>Definieren Ihres Beratungsdienstangebots

Definieren Sie Ihr konfiguriertes Beratungsdienstangebot. Fokussieren Sie auf Angebote, die einen festen Umfang, eine feste Laufzeit und einen geschätzten Festpreis haben (oder kostenlos sind) und primär presales-orientierte Angebote für einen einzelnen Kunden sind. Wählen Sie wiederholbare konfigurierte Projekte aus, die sich als beliebt und effektiv erwiesen haben, Ihnen neue Geschäfte zu bringen.

## <a name="publish-a-consulting-service-offer"></a>Veröffentlichen eines Beratungsdienstangebots

In den folgenden Abschnitten ist beschrieben, wie Sie Ihr Beratungsdienstangebot veröffentlichen können.

1.  Erstellen eines neuen Angebots
2.  Definieren der Angebotseinstellungen
3.  Eingeben der Details der digitalen Ladenzeile und Festlegen, ob in Azure Marketplace oder in AppSource veröffentlicht werden soll
4.  Veröffentlichen Ihres Angebots

### <a name="create-a-new-offer"></a>Erstellen eines neuen Angebots

Führen Sie die folgenden Schritte aus, um ein neues Angebot zu erstellen:

1.  Wählen Sie im Hauptmenü des Cloud-Partnerportals den Befehl **Neues Angebot** aus.
2.  Wählen Sie im Menü „Neues Angebot“ den Eintrag **Beratungsdienst** aus.

    ![Erstellen eines neuen Beratungsdienstangebots](media/consulting-services-publishing-offer/cppselectnewconsultingoffer.png)

### <a name="define-offer-settings"></a>Definieren der Angebotseinstellungen

Der erste Schritt im Bildschirm „Neues Angebot“ besteht darin, die Angebots-ID zu erstellen.  Eine Angebots-ID besteht aus drei Teilen: **Angebots-ID**, **Herausgeber-ID** und **Name**.  Jeder dieser Teile ist in den nachstehenden Abschnitten beschrieben.

#### <a name="offer-id"></a>Angebots-ID

Dieser Bezeichner ist ein eindeutiger Name, den Sie bei der ersten Übermittlung des Angebots erstellen. Dieser Name darf nur klein geschriebene alphanumerische Zeichen, Bindestriche und Unterstriche enthalten. Die Angebots-ID ist in der URL sichtbar und wirkt sich auf Suchmaschinenergebnisse aus. Beispiel: *nameihresunternehmens\_beispieldienst*

Wie im Beispiel gezeigt ist, wird die Angebots-ID an Ihre Herausgeber-ID angefügt, um einen eindeutigen Bezeichner zu erstellen. Dieser Bezeichner wird als permanenter Link verfügbar gemacht, der abonniert werden kann und von Suchmaschinen indiziert wird. 

*Nachdem ein Angebot live geschaltet wurde, kann der Bezeichner nicht mehr aktualisiert werden.*

#### <a name="publisher-id"></a>Herausgeber-ID

Dieser Bezeichner bezieht sich auf Ihr Konto. Wenn Sie sich mit Ihrem Organisationskonto angemeldet haben, wird Ihre Herausgeber-ID im Dropdownmenü angezeigt.

#### <a name="name"></a>NAME

Dies ist die Zeichenfolge, die als Angebotsname in AppSource oder Azure Marketplace angezeigt wird.

**Wichtig:** Geben Sie hier nur den Namen des eigentlichen Diensts ein. Fügen Sie weder die Dauer noch den Typ des Diensts hinzu.

Das folgende Beispiel von Edgewater Fullscope zeigt, wie der Angebotsname aufgebaut ist. Der Angebotsname wird wie folgt angezeigt:

![Erstellen eines neuen Beratungsdienstangebots](media/consulting-services-publishing-offer/cppsampleconsultingoffer.png)

Ein Angebotsname besteht aus vier Teilen:

-   **Dauer**: Die Dauer wird im Editor auf der Registerkarte **Details der digitalen Ladenzeile** definiert. Eine Dauer kann in Stunden, Tagen oder Wochen ausgedrückt werden.
-   **Diensttyp**: Der Diensttyp wird im Editor auf der Registerkarte **Details der digitalen Ladenzeile** definiert. Die Diensttypen lauten `Assessment` (Bewertung), `Briefing` (Einweisung), `Implementation` (Implementierung), `Proof of concept` und `Workshop`.
-   **Präposition**: Die Präposition wird vom Prüfer eingefügt.
-   **Name**: Der Name wird auf der Seite **Angebotseinstellungen** definiert.

In der folgenden Liste sind einige ordnungsgemäß benannte Angebote aufgeführt:

-   Das Wichtigste für professionelle Dienstleistungen: 1-Std. Einweisung
-   Plattform für Cloudmigration: 1-Std. Einweisung
-   PowerApps und Microsoft Flow: 1-Tag Workshop
-   Azure Machine Learning Services: 3-Wo PoC
-   Einzelhandelslösung für Ladengeschäft und Onlineshop: 1-Std. Einweisung
-   Aufbereitung Ihrer Daten: 1-Wo Workshop
-   Cloud-Analysen: 3-Tag Workshop
-   Power BI-Schulung: 3-Tag Workshop
-   Vertriebsmanagementlösung: 1-Wo Implementierung
-   CRM-Schnellstart: 1-Tag Workshop
-   Dynamics 365 for Sales: 2-Tag Bewertung

Nachdem Sie die Registerkarte **Angebotseinstellungen** ausgefüllt haben, können Sie Ihr Angebot speichern. Der Angebotsname wird jetzt über dem Editor angezeigt, und Sie finden ihn in „Alle Angebote“.

### <a name="enter-storefront-details"></a>Eingeben der Details der digitalen Ladenzeile

Als Nächstes müssen Sie die Details für Ihre digitale Ladenzeile eingeben. Die Details der digitalen Ladenzeile bestehen aus den folgenden Abschnitten:

-   Angebotsdetails
-   Informationen zum Herausgeber
-   Auflistungsdetails
-   Marketingelemente

#### <a name="offer-details"></a>Angebotsdetails

Der Abschnitt für Angebotsdetails enthält die folgenden Felder:

-   Angebotszusammenfassung
-   Angebotsbeschreibung

##### <a name="offer-summary"></a>Angebotszusammenfassung

Die Angebotszusammenfassung ist eine kurze Beschreibung Ihres Angebots, die direkt unter dem Angebotsnamen angezeigt wird. Verwenden Sie unformatierten Text, wenn Sie die Angebotszusammenfassung eingeben, und es sollten keine Zeilenumbrüche enthalten sein. Im Folgenden sind gute Beispiele für Angebotszusammenfassungen samt deren entsprechenden Angebotsnamen aufgeführt:

*Beispiel 1*

-   **Angebotsname:** Cloud-Analysen: 3-Tag Workshop
-   **Angebotszusammenfassung:** Übersicht über Microsoft Azure und Power BI, Bewertung der aktuellen Umgebung und Mini-PoC.

*Beispiel 2*

-   **Angebotsname:** Azure-IoT in der Industrie: 30-Tag Proof of Concept
-   **Angebotszusammenfassung:** Es wird ein für den industriellen Einsatz vorgesehenes Produktpilotprojekt erstellt, in dem betrieblich genutzte Geräte sicher mit einer Azure IoT Hub-Lösung verbunden werden, die Dashboards, Berichte und Benachrichtigungen bereitstellt.

*Beispiel 3*

-   **Angebotsname:** Dienstleistungsunternehmen: 1-Std. Einweisung
-   **Angebotszusammenfassung:** Übersicht und Vorführung einer vorkonfigurierten erweiterten Dynamics 365 for Operations-Lösung, die verbesserte Verwaltung von Projekten, Abrechnung und Ressourcen für professionelle Dienstleistungen bereitstellt.

*Beispiel 4*

-   **Angebotsname:** Power BI in Ihrer Welt: 4-Std. Workshop
-   **Angebotszusammenfassung:** Sie konfigurieren und starten Ihr erstes Dashboard und lernen bewährte Methoden kennen. Für bis zu 12 Teilnehmer, Durchführung in Ihrem Hause.

*Beispiel 5*

-   **Angebotsname:** Dynamics und Projekte: 3-Tag Bewertung
-   **Angebotszusammenfassung:** Anforderungserfassung und -bewertung für eine ERP-Lösung, die für Dienstleistungsunternehmen und projektausführende Unternehmen konzipiert ist.

##### <a name="offer-description"></a>Beschreibung des Angebots

Beschreibung des Beratungsdienstangebots. Eine gute Angebotsbeschreibung enthält genaue Angaben darüber, wie das Projekt aussieht und wie sich das Endergebnis für den Kunden darstellt. Der Beschreibung sollte ein Kunde klar entnehmen können, welche Leistung er bekommt.

Fügen Sie in Ihre Angebotsbeschreibung keine E-Mail-Links oder Telefonnummern ein, über die Sie zu erreichen sind. Es wird eine „Kontakt mit mir aufnehmen“-Schaltfläche in Ihrem Angebot geben, über die Leads in das Leadverwaltungsziel hochgeladen werden, das Sie in Ihrem Angebot angeben.

Sie geben die Angebotsbeschreibung im Markdown-Format ein. Wenn Sie mit Markdown oder Formatierung für HTML nicht vertraut sind, sollten Sie die entsprechenden Artikel auf der [Microsoft Docs-Website](https://docs.microsoft.com/contribute/how-to-write-use-markdown) lesen.

Durch Verwenden dieser Formate wird sichergestellt, dass Ihr Angebot eine optimale Lesbarkeit für Kunden hat.

Achten Sie darauf, dass Ihre Angebotsbeschreibung kurz bleibt, und halten Sie sich an die Beschränkung für die Zeichenzahl, da Benutzer nicht gerne sehr langen Text lesen. Sie haben immer noch die Option, Marketingbroschüren, Merkblätter und andere Dokumente hochzuladen, in denen Ihr Angebot ausführlicher beschrieben ist.

Im folgenden Beispiel werden eine gut gestaltete Angebotsbeschreibung und deren zugehöriger Name und zugehörige Zusammenfassung veranschaulicht:

**Angebotsname:** Cloud-Analysen: 3-Tag Workshop

**Angebotszusammenfassung:** Übersicht über Microsoft Azure und Power BI, Bewertung der aktuellen Umgebung und Mini-PoC.

**Angebotsbeschreibung:** Dieser dreitägige Workshop ist für technische und geschäftliche Führungskräfte vorgesehen und findet in den Räumlichkeiten des Kunden statt.

***Agenda***

Tag 1

-   Der Schwerpunkt liegt darauf, wie Daten in der Microsoft-Cloud mit Azure Data Lake, HDInsight oder Azure SQL Data Warehouse geschützt, skaliert und strukturiert werden können.

Tag 2

-   Es wird erläutert, wie erweitere Analyselösungen mit Microsoft R und Azure Machine Learning konfiguriert und bereitgestellt werden können.

Tag 3

-   Es wird beschrieben, wie Power BI verwendet werden kann, um verwertbare Erkenntnisse und operative Analysen zu gewinnen. Dazu gehört eine in Zusammenarbeit durchgeführte Sitzung, in der gemeinsam ein Power BI-Dashboard erstellt wird.

Am Ende des Workshops wird der Kunde in der Lage sein, einen umfassenden Plan und eine Implementierungsroadmap für Daten- und Analyselösungen in der Microsoft-Cloud zu definieren.

*Markdown-Datei als Beispiel für ein Angebot, das in diesem Format vorliegt:*

    This 3-day workshop is for technical and business leaders and is held on-site at the client’s facility.

      ### Agenda

      **Day 1**

      * Focuses on how to secure, scale, and organize data within the Microsoft cloud using Azure Data Lake, HDInsight, or Azure SQL Data Warehouse

      **Day 2**

      * Covers how to configure and deploy advanced analytics solutions with Microsoft R and Azure Machine Learning

      **Day 3**

      * Covers how to draw actionable insights and operationalize analytics with Power BI, including a collaborative session to co-build a Power BI dashboard.


      ### Deliverables
      By the end of the workshop, the client will be able to define a high-level plan and an implementation roadmap for data and analytics solutions in the Microsoft cloud.


#### <a name="publisher-information"></a>**Informationen zum Herausgeber**

**MPN-ID**

Ihre MPN-ID (Microsoft Partner Network) aus 9 Ziffern. Wenn Sie keine MPN-ID haben, navigieren Sie zum Microsoft Partner Center, um eine MPN-ID zu erlangen.

**Partner Center-ID**

Die neue Partner Center-ID, sofern Sie eine solche haben.

**MPN-ID**

Geben Sie einen geheimen Schlüssel ein, um eine Vorschau Ihres Angebots in AppSource anzuzeigen, bevor Sie das Angebot freischalten.
Dieser Bezeichner ist kein Kennwort.

#### <a name="listing-details"></a>Auflistungsdetails

**Typ des Beratungsdiensts**

Microsoft legt den Fokus ausschließlich auf Angebote, die einen festen Umfang, eine feste Laufzeit und einen geschätzten Festpreis haben (oder kostenlos sind) und primär presales-orientierte Angebote für einen einzelnen Kunden sind, sowie auf Bewertungs-, Implementierungs-, Proof of Concept-, Einweisungs- oder Workshopangebote, die entweder direkt beim Kunden oder virtuell durchgeführt werden (Einweisungen müssen direkt beim Kunden durchgeführt werden).

Es gibt die folgenden fünf Angebotstypen:

-   **Bewertung:** Eine Auswertung der Umgebung eines Kunden, um die Anwendbarkeit einer Lösung zu bestimmen und eine Schätzung der Kosten und zeitlichen Steuerung zu bieten.
-   **Einweisung:** Eine Einführung in eine Lösung oder einen Beratungsdienst, um die Aufmerksamkeit von Kunden durch Frameworks, Demos und Beispiele für Kunden zu erhöhen. Einweisungen müssen direkt beim Kunden durchgeführt werden.
-   **Implementierung:** Eine umfassende Installation, die zu einer vollständig funktionierenden Lösung führt. Für diesen Pilotversuch empfiehlt Microsoft ein Beschränken auf Lösungen, die innerhalb maximal einer Woche implementiert werden können.
-   **Proof of Concept:** Eine Implementierung in einem begrenzten Bereich, um zu bestimmen, ob eine Lösung die Anforderungen eines Kunden erfüllt.
-   **Workshop:** Ein interaktives Projekt, das am Kundenstandort durchgeführt wird und Schulungssitzungen, Einweisungen, Bewertungen oder Demos umfassen kann, die auf den Daten oder der Umgebung des Kunden basieren.

**Verfügbarkeit in Land/Region**

Wählen Sie das Land und die Region aus, wo dieses Beratungsdienstangebot verfügbar ist. Ein einzelnes Angebot kann nicht in mehreren Ländern oder Regionen veröffentlicht werden. Es muss für jedes Land oder jede Region ein neues Angebot erstellt werden.

**Branchen**

Wählen Sie die Branchen aus, für die Ihr Beratungsdienstangebot am geeignetsten ist.

**Duration**

Wählen Sie unter „Dauer“ eine Zahl (z. B. 3, 4 usw.) aus, und wählen Sie dann „Stunde“, „Tag“ oder „Woche“ aus.

**Primäre Produkte**

Wenn Sie in Azure Marketplace veröffentlichen möchten, wählen Sie **Azure** als primäres Produkt aus, und wählen Sie dann relevante Lösungsbereiche aus.

Wenn Sie in AppSource veröffentlichen möchten, wählen Sie **Dynamics 365**, **Power BI** oder **PowerApps** als Ihr primäres Produkt aus. Sie können auch andere relevante geeignete Produkte auswählen, und Ihr Beratungsangebot wird in Listen angezeigt, die mit jedem dieser Produkte in AppSource verknüpft sind.

**Relevante Kompetenzen**

Wählen Sie die Kompetenzen aus, die für dieses Angebot relevant sind, damit sie zusammen mit den Angebotsdetails angezeigt werden.

#### <a name="marketing-artifacts"></a>Marketingelemente

**Unternehmenslogo (PNG-Format, 48 x 48 Pixel)**

Laden Sie ein Bild hoch, das auf der Kachel Ihres Angebots auf der Katalogseite für Angebote angezeigt wird. Das Bild muss ein PNG-Bild mit einer Auflösung von 48 x 48 Pixel sein.

**Unternehmenslogo (PNG-Format, 216 x 216 Pixel)**

Laden Sie ein Bild hoch, das auf der Detailseite Ihres Angebots angezeigt wird. Das Bild muss ein PNG-Bild mit einer Auflösung von 216 x 216 Pixel sein.

**Videos (max. 4)**

Laden Sie bis zu vier Kundenfallstudien- oder Kundenreferenzvideos hoch. Haben Sie kein solches Video, laden Sie ein Video hoch, in dem die Fachkenntnisse erläutert werden, die Ihr Unternehmen hinsichtlich des Angebots hat. Wenn Sie einen Schaukasten (Showcase) für Power BI- oder PowerApps-Lösungen haben, laden Sie das Schaukastenvideo hierhin hoch. Videolinks müssen für YouTube oder Vimeo geeignet sein.

**Dokumente (max. 3)**

Laden Sie die Marketingbroschüre hoch, in der Ihr Beratungsdienstangebot ausführlich beschrieben wird. Alternativ können Sie eine Beschreibung, Merkblätter oder Fallstudien Ihres Unternehmens hochladen.

**Bildschirmfotos (max. 5)**

Laden Sie bis zu fünf Bilder hoch, die weitere Informationen über das Angebot, die Angebotsleistungen oder Ihr Unternehmen bieten. Dies kann z. B. ein Ausschnitt aus Ihrer Marketingbroschüre, eine wichtige Folie aus einer Präsentation oder ein Bild sein, das die Dynamik oder Kompetenz Ihres Unternehmens veranschaulicht.

### <a name="publish-your-offer"></a>Veröffentlichen Ihres Angebots

Nachdem Sie die Felder für Angebotseinstellungen, Details der digitalen Ladenzeile und Kontakte ausgefüllt haben, wählen Sie **Veröffentlichen** aus, und geben Sie eine E-Mail-Adresse an. Sobald Microsoft zum Veröffentlichen Ihres Angebots bereit ist, erhalten Sie eine E-Mail, damit Sie eine Vorschau Ihres Angebots anzeigen können, bevor es freigeschaltet wird. Sie können zum Portal zurückkehren, um jederzeit den Status Ihres Angebots zu prüfen.

Während des Veröffentlichungsprozesses kann ein Angebot im Status „Veröffentlichen abgebrochen“ oder „Veröffentlichungsfehler“ angezeigt werden. Dies ist ein normaler Bestandteil des Prozesses und ermöglicht es Microsoft, Ihr Angebot zu bearbeiten. Wenn Sie sehen, dass Ihr Angebot den Status „Veröffentlichen abgebrochen“, belassen sie es in diesem Status.
