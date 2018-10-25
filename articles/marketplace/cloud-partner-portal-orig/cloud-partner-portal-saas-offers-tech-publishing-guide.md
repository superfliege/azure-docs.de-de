---
title: Technischer Leitfaden für die Veröffentlichung von SaaS-Anwendungen | Microsoft-Dokumentation
description: Es wird erläutert, wie SaaS-Anwendungsangebote im geeigneten Marketplace veröffentlicht werden.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: pbutlerm
ms.openlocfilehash: 9d73040f11549741643d96913c42df49594b8d41
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48806071"
---
<a name="saas-application-technical-publishing-guide"></a>Technischer Leitfaden für die Veröffentlichung von SaaS-Anwendungen
===========================================

Willkommen beim Technischen Leitfaden für die Veröffentlichung von SaaS-Anwendungen. Dieser Leitfaden soll zukünftige und vorhandene Herausgeber dabei unterstützen, ihre Anwendungen und Dienste mithilfe des Angebots für SaaS-Anwendungen in AppSource oder im Azure Marketplace zu listen.

Eine Übersicht über alle Marketplace-Angebote finden Sie im [Veröffentlichungsleitfaden für Azure Marketplace und AppSource](https://aka.ms/sellerguide).


<a name="what-are-pre-requisites-for-publishing-a-saas-app"></a>Was sind die Voraussetzungen für die Veröffentlichung einer SaaS-Anwendung?
-------------------------------------------------

Das Veröffentlichungsportal bietet rollenbasierten Zugriff auf das Portal, sodass mehrere Personen hinsichtlich der Veröffentlichung eines Angebots zusammenarbeiten können. Weitere Informationen finden Sie unter [Verwalten von Benutzern](./cloud-partner-portal-manage-users.md). 

Bevor ein Angebot im Auftrag eines Herausgeberkontos veröffentlicht werden kann, muss eine der Personen mit *Besitzer*-Rolle bestätigen, dass die [Nutzungsbedingungen](https://azure.microsoft.com/support/legal/website-terms-of-use/), die [Datenschutzerklärung von Microsoft](http://www.microsoft.com/privacystatement/default.aspx) und die [Microsoft Azure Certified-Programmvereinbarung](https://azure.microsoft.com/support/legal/marketplace/certified-program-agreement/) eingehalten werden.


<a name="creating-an-offer"></a>Erstellen eines Angebots
-----------------

In diesem Abschnitt ist die Vorgehensweise beschrieben, wie ein neues SaaS-Anwendungsangebot erstellt wird.

![Übersicht über ein SaaS-Angebot](media/cpp-creating-saas-offers/saas-offer-overview.png)

Ein SaaS-Anwendungsangebot besteht aus fünf Abschnitten, die in der folgenden Tabelle beschrieben sind:

| **Angebotsabschnitt**  | **Beschreibung**                                                                                                     |
|--------------------|-------------------------------------------------------------------------------------------------------------------------------|
| Angebotseinstellungen     | Ermöglichen es, einen eindeutigen Namen und eine eindeutige ID für die SaaS-Anwendung zu definieren.                                                                         |
| Technische Informationen     | Hier werden der Typ der SaaS-Lösung konfiguriert und die Verbindungsdetails für Ihre Anwendung bereitgestellt.                            |
| Testversion         | Optionaler Abschnitt, in dem Sie einen Dienst definieren können, über den Kunden Ihr Angebot testen können, bevor sie es erwerben. |
| Storefrontdetails | Enthält die Abschnitte für Marketing, rechtliche Hinweise, Leadverwaltung und Auflistung:   <br/> – Im Abschnitt für Marketing können Sie die Beschreibung und Logos angeben, die erforderlich sind, damit das Angebot im Marketplace-Benutzerportal ordnungsgemäß angezeigt werden kann.  <br/> – Im Abschnitt für Leadverwaltung können Sie einen Ort definieren, zu dem die Endbenutzerleads umgeleitet werden, die im Azure Marketplace-Portal für Endbenutzer generiert wurden.  <br/> – Im Abschnitt für rechtliche Hinweise können Sie Dokumentation zur Datenschutzrichtlinie und zu den Nutzungsbedingungen angeben.  |
| Kontakt            | Hier können Sie Kontaktinformationen zum Support des Angebots eingeben.                                                                  |
|  |  |


### <a name="creating-a-new-offer"></a>Erstellen eines neuen Angebots

Nachdem Sie sich beim Cloud-Partnerportal angemeldet haben, wählen Sie den Eintrag **Neues Angebot** in der linken Menüleiste aus, in der ein Menü der verfügbaren Angebote angezeigt wird. Die folgende Abbildung zeigt ein Beispiel dieser Angebote:

![Neues SaaS-Angebot](media/cpp-creating-saas-offers/saas-new-offer.png)

Wählen Sie in der Liste das Angebot aus, auf das Sie zugreifen können. Es wird ein neues Angebotsformular geöffnet.

![Neues SaaS-Angebotsformular](media/cpp-creating-saas-offers/saas-new-offer-2.png)

In der folgenden Tabelle sind die Angebotsfelder beschrieben:

| **Angebotsfelder** | **Beschreibung**                                                                                            |
|------------------|----------------------------------------------------------------------------------------------------------- |
| Angebots-ID         | Ein eindeutiger Bezeichner für das Angebot innerhalb eines Herausgeberprofils. Diese ID wird in den Produkt-URLs und den Abrechnungsberichten sichtbar sein. Sie darf nur aus klein geschriebenen alphanumerischen Zeichen oder Bindestrichen (-) bestehen. Die ID darf nicht mit einem Bindestrich enden und darf höchstens 50 Zeichen umfassen. Beachten Sie, dass dieses Feld gesperrt wird, sobald ein Angebot online geschaltet wird. Wenn beispielsweise der Herausgeber „Contoso“ ein Angebot mit der Angebots-ID „sample-vm“ veröffentlicht, wird diese im Azure Marketplace wie folgt angezeigt: [https://azuremarketplace.microsoft.com/marketplace/apps/contoso.sample-vm?tab=Overview](https://azuremarketplace.microsoft.com/) |
| Herausgeber-ID     | Die Herausgeber-ID ist Ihr eindeutiger Bezeichner im Marketplace. In jedes Ihrer Angebote muss Ihre Herausgeber-ID eingefügt werden. Die Herausgeber-ID kann nicht mehr geändert werden, sobald das Angebot gespeichert wurde.                                                                                       |
| NAME             | Hierbei handelt es sich um den Anzeigenamen für Ihr Angebot. Dies ist der Name, der im Azure Marketplace und im Azure-Portal angezeigt wird. Er darf aus höchstens 50 Zeichen bestehen. Hier empfiehlt es sich, einen Markennamen mit gutem Wiedererkennungswert für Ihr Produkt zu verwenden. Verwenden Sie an dieser Stelle nicht den Namen Ihres Unternehmens, es sei denn, das Produkt wird unter diesem Namen beworben. Wenn Sie das Angebot auf Ihrer eigenen Website bewerben, vergewissern Sie sich, dass der Name exakt mit dem auf Ihrer Website übereinstimmt.             |
|  |  |

Klicken Sie auf **Speichern**, um Ihren Fortschritt zu speichern. Im nächste Abschnitt ist beschrieben, wie Sie Pläne zu Ihrem Angebot hinzufügen.


### <a name="technical-information"></a>Technische Informationen

Im Abschnitt für technische Informationen können Sie die folgenden Informationen eingeben:

![Technische Informationen für das SaaS-Angebot](media/cpp-creating-saas-offers/saas-new-offer-technical-info.png)

Die wichtigste Entscheidung ist, ob Sie eine SaaS-Auflistung anbieten möchten oder ein Verkaufsangebot beabsichtigen. Wenn Sie eine SaaS-Auflistung anbieten möchten, müssen Sie eine der folgenden Optionen auswählen:

-   Kostenlos – Geben Sie für die SaaS-Anwendung eine URL an, über die Kunden Ihre Anwendung abrufen können.
-   Kostenlose Testversion – Geben Sie für die SaaS-Anwendung eine URL an, über die Kunden die Testversion ausführen können, bevor sie das Angebot erwerben.
-   Kontakt mit mir aufnehmen – Nur relevant, wenn Sie ein Leadverwaltungssystem eingebunden haben. Über diese Option können Kunden um Kontaktaufnahme bitten, und ein Lead wird Ihnen mitgeteilt.

Wenn Sie eine SaaS-Anwendung haben, die sich im Azure Marketplace befindet, und kommerziellen Vertrieb über Microsoft-Transaktionen ermöglichen möchten, wählen Sie **Verkauf über Azure** aus.  
Weitere Informationen zum Herstellen einer Verbindung mit Ihrer SaaS-Anwendung finden Sie unter [SaaS – Verkauf über Azure](./cloud-partner-portal-saas-offer-subscriptions.md).


### <a name="test-drive"></a>Testversion

Das Erstellen einer Testversion für Ihre Kunden ist eine bewährte Methode, um sicherzustellen, dass sie ruhigen Gewissens kaufen können. Unter den verfügbaren Testoptionen ist „Testversion“ die wirksamste Option, um aussichtsreiche Leads zu gewinnen und eine vermehrte Umwandlung dieser Leads zu erreichen.

Diese Option bietet Kunden einen praktischen, selbstgesteuerten Test der wichtigsten Funktionen und Vorteile Ihres Produkts, die in einem realitätsnahen Implementierungsszenario veranschaulicht werden.

![Testversion eines SaaS-Angebots](media/cpp-creating-saas-offers/saas-new-offer-test-drive.png)


#### <a name="how-a-test-drive-works"></a>So funktioniert eine Testversion

Ein potenzieller Kunde hat Ihre Anwendung im Marketplace entdeckt. Der Kunde meldet sich an und stimmt den Nutzungsbedingungen zu. An diesem Punkt empfängt der Kunde Ihre vorkonfigurierte Umgebung, die er für eine feste Anzahl von Stunden testen kann, und Sie empfangen einen aussichtsreichen Lead, den Sie nachverfolgen können.

![Testversion eines SaaS-Angebots 2](media/cpp-creating-saas-offers/saas-new-offer-test-drive-2.png)

Unabhängig davon, wie komplex Ihre Anwendung ist, hilft Ihre Microsoft-Testversion Ihnen dabei, Ihr Produkt für den Kunden zum Leben zu erwecken. Es sind drei verschiedene Testversionstypen verfügbar, von denen jeder auf dem Typ des Produkts, des Szenarios und des Marketplace basiert, den Sie nutzen.

-   **Azure Resource Manager** – Eine Azure ARM-Testversion ist eine Bereitstellungsvorlage, die alle Azure-Ressourcen enthält, aus denen eine vom Herausgeber erstellte Lösung besteht. Produkte, die für diesen Testversionstyp geeignet sind, sind Produkte, in denen nur Azure-Ressourcen verwendet werden.
-   **Logik-App** -Eine Logik-App-Testversion ist eine Bereitstellungsvorlage, die für die alle komplexen Lösungsarchitekturen vorgesehen ist. Alle Dynamics-Anwendungen oder benutzerdefinierten Produkte sollten mit diesem Testversionstyp genutzt werden.
-   **Power BI** -Eine Power BI-Testversion besteht aus einem eingebetteten Link zu einem kundenspezifischen Dashboard. Für jedes Produkt, mit dem eine interaktive Power BI-Visualisierung demonstriert werden soll, sollte dieser Testversionstyp verwendet werden. Sie müssen lediglich Ihre eingebettete Power BI-URL hochladen.

Die Hauptveröffentlichungsschritte für ein Hinzufügen einer Testversion sind:

1.  Definieren Ihres Testversionszenarios
2.  Erstellen und/oder Ändern Ihrer Resource Manager-Vorlage
3.  Erstellen Sie Ihrer schrittweisen Bedienungsanleitung für die Testversion
4.  Erneutes Veröffentlichen Ihres Angebots

Weitere Informationen finden Sie unter [Testversion](./what-is-test-drive.md).


### <a name="storefront-details"></a>Storefrontdetails

Für eine SaaS-Anwendung müssen die beiden ersten Angebotsabschnitte, Zusammenfassung und Beschreibung, bereitgestellt werden.

![Storefrontdetails eines SaaS-Angebots](media/cpp-creating-saas-offers/saas-new-offer-storefront-details.png)

In der folgenden Tabelle sind die Storefrontdetails (Details der digitalen Ladenzeile) eines Angebots beschrieben:

| **Angebotsfelder**        | **Beschreibung**                                                                                          |
|-------------------------| ---------------------------------------------------------------------------------------------------------|
| Angebotszusammenfassung           | Eine Zusammenfassung des Nutzenversprechens Ihres Angebots. Die Zusammenfassung wird auf der Suchseite Ihres Angebots angezeigt. Sie darf aus höchstens 100 Zeichen bestehen.   |
| Beschreibung des Angebots       | Die Beschreibung, die auf der Detailseite Ihrer Anwendung angezeigt wird. Es sind maximal 1300 Zeichen zulässig. *Hinweis*: Für dieses Feld wird HTML-Inhalt mit Tags wie &ltp\>, &lth1\>, &lth2\>, &ltli\> usw. unterstützt, wodurch es Ihnen ermöglicht wird, den Inhalt viel ansprechender zu gestalten. Das für Portalveröffentlichungsfunktionen zuständige Team arbeitet daran, eine Funktion hinzuzufügen, mit der eine Vorschau der Storefrontdetails angezeigt werden kann, um den Inhalt auf iterative Weise ansprechender zu gestalten. In der Zwischenzeit können Sie jedes online verfügbare Echtzeit-HTML-Tool wie http://htmledit.squarefree.com verwenden, um zu sehen, wie Ihre Beschreibung aussieht. |
| Branchen              | Wählen Sie die Branchen aus, für die Ihr Angebot am besten geeignet ist. Wenn Ihre Anwendung für mehrere Branchen geeignet ist, können Sie maximal zwei auswählen. |
| Vorgeschlagene Kategorien    | Wählen Sie die Kategorien aus, für die Ihr Angebot am besten geeignet ist. Sie können bis zu drei Kategorien auswählen.     |
| Anwendungsversion     | Geben Sie die Versionsnummer Ihrer Anwendung ein.                                                                |
| Suchbegriffe (maximal 3) | Geben Sie bis zu drei Suchbegriffe ein, mit denen Kunden nach Ihrer Anwendung auf der Marketplace-Website der digitalen Ladenzeile (Storefront) suchen können. |
|  |  |


### <a name="marketing-artifacts"></a>Marketingelemente

Im Bereich für Marketingelemente können Sie die Azure Marketplace-Marketingobjekte angeben, z. B. Logos, Videos, Bildschirmfotos (Screenshots) und Dokumente:

![Marketingelemente eines SaaS-Angebots](media/cpp-creating-saas-offers/saas-new-offer-marketing-artifacts.png)

In der folgenden Tabelle sind die Marketingfelder beschrieben:

| **Angebotsfelder** | **Beschreibung**                                                                                                          |
|------------------| ------------------------------------------------------------------------------------------------------------------------ |
| Logos            | Wenn Sie eine **Verkauf über Azure**-SaaS-Anwendung anbieten, sollten Sie alle Logobilder bereitstellen. Wenn Sie lediglich eine Auflistung bereitstellen, sind nur zwei Logos erforderlich. Alle Logos, die in das Cloud-Partnerportal hochgeladen werden, sollten den folgenden Richtlinien entsprechen:     <br/> – Verwenden Sie auf Ihrem Logo möglichst wenige Primär- und Sekundärfarben. Die Farbpalette des Azure-Designs ist einfach und geradlinig.     <br/> – Vermeiden Sie Schwarz oder Weiß als Hintergrundfarbe Ihres Logos. Die Designfarben des Azure-Portals sind Schwarz und Weiß. Verwenden Sie stattdessen Farben, die Ihr Logo im Azure-Portal markant hervorstechen lassen. Sie sollten einfache Primärfarben verwenden. Wenn Sie einen transparenten Hintergrund verwenden, achten Sie darauf, dass das Logo und Text weder weiß, noch schwarz noch blau sind.     <br/> – Verwenden Sie im Logo keinen Hintergrund mit Farbverlauf.     <br/> – Platzieren Sie nach Möglichkeit keinen Text auf dem Logo, auch nicht Ihren Firmen- oder Markennamen. Das Erscheinungsbild Ihres Logos sollte klar und direkt und frei von Farbverläufen sein.    <br/> – Das Logobild sollte nicht gestreckt sein.                   |
| Videos           | Ermöglicht es Ihnen, Links zu Videos Ihres Angebots hinzuzufügen. Sie können Links zu YouTube und/oder Vimeo-Videos angeben, die zusammen mit Ihrem Angebot bei Kunden angezeigt werden. Sie müssen auch ein Miniaturbild des Videos mit einem PNG-Bild von 1280 x 720 Pixel eingeben. Sie können pro Angebot bis zu vier Videos verwenden. |
| Dokumente        | Ermöglicht es Ihnen, Ihrem Angebot Marketingdokumente hinzuzufügen. Alle Dokumente müssen im PDF-Format vorliegen, und pro Angebot können maximal drei Dokumente verwendet werden.                                                                                                                                                      |
| Screenshots      | Ermöglicht es Ihnen, Screenshots von Ihrem Angebot hinzuzufügen. Pro Angebot können bis zu fünf Screenshots hinzugefügt werden. Die maximale Bildgröße ist 1280 x 720 Pixel.                                                                                                                                             |
| Nützliche Links     | Ermöglicht es Ihnen, externe URLs für Ihr Angebot hinzuzufügen, um auf Architekturdiagramme oder andere Websites zu verweisen, die sich ein Kunde ansehen möchte.                                                                                                                                                              |
|  |  |

Die folgende Abbildung zeigt, wie die Informationen in einem Marketplace-Suchergebnis angezeigt werden:

![Marketinginformationen des Herausgebers eines SaaS-Angebots](media/cpp-creating-saas-offers/saas-new-offer-marketing-publisher-info.png)

Die folgende Abbildung zeigt, wie das Angebot im Marketplace angezeigt wird, nachdem ein Kunde auf die kleinere Angebotskachel geklickt hat:

![Marketinginformationen des Herausgebers eines SaaS-Angebots 2](media/cpp-creating-saas-offers/saas-new-offer-marketing-publisher-info-2.png)


### <a name="lead-management-and-legal-information"></a>Leadverwaltung und rechtliche Informationen

In dem Abschnitt für rechtliche Hinweise können Sie die rechtliche Dokumentation des Angebots angeben.
Für jedes SaaS-Anwendungsangebot sind zwei Rechtsdokumente erforderlich: Datenschutzrichtlinien und die Nutzungsbedingungen. Weitere Informationen finden Sie unter  
[Konfigurieren von Kundenleads](./cloud-partner-portal-get-customer-leads.md).

![Rechtliche Marketinginformationen eines SaaS-Angebots](media/cpp-creating-saas-offers/saas-new-offer-legal-info.png)

In der folgenden Tabelle sind die Felder des Abschnitts für rechtliche Hinweise beschrieben:

| **Angebotsfelder**   | **Beschreibung**                                                                                 |
|--------------------| ----------------------------------------------------------------------------------------------- |
| Datenschutzrichtlinien-URL | Die URL zu den Datenschutzrichtlinien Ihres Unternehmens.                                                       |
| Terms of Use (Nutzungsbedingungen)       | Die Nutzungsbedingungen für Ihr Angebot. Geben Sie die Nutzungsbedingungen hier direkt oder über Kopieren und Einfügen ein. Das Verwenden einfacher HTML-Elemente wird unterstützt, z. B. kann das Feld Text mit Tags wie &ltp\>, &lth1\>, &lth2\>, &ltli\> usw. enthalten. *Wichtig:* Es wird dringend empfohlen, dass Sie den von Ihnen erstellten HTML-Inhalt prüfen und sich diesen in einem Browser ansehen, bevor Sie das Angebot absenden. |
|  |  |


### <a name="contact-information"></a>Kontaktinformationen

In diesem Abschnitt geben Sie die Supportkontakte Ihres Unternehmens ein, die für die Unterstützung von Kunden zuständig sind, die dieses Angebot nutzen.
Es gibt drei Hauptbereiche: Engineering-Kontakt, Supportkontakt und Support-URLs:

![Marketingkontaktinformationen eines SaaS-Angebots](media/cpp-creating-saas-offers/saas-new-offer-contact-info.png)


| **Kontakt**         | **Beschreibung**                                                                                                                          |
|---------------------|------------------------------------------------------------------------------------------------------------------------------------------|
| Engineering-Kontakt | Geben Sie den Namen, die E-Mail-Adresse und die Telefonnummer eines Engineering-Kontakts ein, an den sich Microsoft hinsichtlich Support- und Geschäftsproblemen wenden kann. |
| Supportkontakt     | Geben Sie den Namen, die E-Mail-Adresse, die Telefonnummer und die Support-URL an, über die sich Ihre Kunden an Ihren Support wenden können, wenn sie Supportanfragen haben.                  |
|  |  |


Sobald Ihr Angebot fertiggestellt ist und Sie auf „Veröffentlichen“ geklickt haben, durchläuft das Angebot die Zertifizierung. Wir testen Ihre SaaS-Anwendung in einer manuellen Überprüfung, indem wir Ihre URL testen, wenn Sie eine Liste hochgeladen haben, oder Ihre Endpunkte testen, wenn Sie „Verkauf über Azure“ ausgewählt haben. Während dieser manuellen Genehmigung entscheiden wir auch, welche Storefront zum Auflisten Ihrer Anwendung geeignet ist (AppSource, Azure Marketplace oder beide).

<a name="updating-the-offer"></a>Aktualisieren des Angebots
------------------

Es gibt verschiedene Arten von Aktualisierungen, die Sie möglicherweise an Ihrem Angebot vornehmen möchten, nachdem es veröffentlicht und freigeschaltet wurde. Jede Änderung, die Sie an der neuen Version Ihres Angebots vorgenommen haben, sollte gespeichert und veröffentlicht werden, damit sie im Marketplace wirksam wird.

<a name="deleting-an-existing-offer"></a>Löschen eines vorhandenen Angebots
--------------------------

Möglicherweise möchten Sie Ihr Angebot aus dem Marketplace entfernen. Durch das Entfernen eines Angebots wird sichergestellt, dass Ihr Angebot nicht mehr durch neue Kunden erworben oder bereitgestellt werden kann. Dies wirkt sich jedoch nicht auf vorhandene Kunden aus. Durch Beenden des Angebots wird der Dienst und/oder der Lizenzvertrag zwischen Ihnen und Ihren bestehenden Kunden beendet.

Eine Anleitung und Richtlinien zur Entfernung und Beendigung von Angeboten finden Sie im Microsoft Marketplace-Herausgebervertrag (siehe Abschnitt 7) und in den Teilnahmerichtlinien (siehe Abschnitt 6.2). Dieser Abschnitt enthält Informationen zu den unterschiedlichen unterstützten Szenarien für Löschen und zu den dazu ausführbaren Schritten.

### <a name="delete-the-live-offer"></a>Löschen eines aktiven Angebots

Bei einer Anforderung zum Entfernen eines aktiven Angebots müssen verschiedene Aspekte berücksichtigt werden. Führen Sie die unten angegebenen Schritte aus, um vom Supportteam Hilfe beim Entfernen eines aktiven Angebots aus dem Azure Marketplace zu erhalten:

1.  Erstellen Sie über diesen Link ein Supportticket.
2.  Wählen Sie in der Liste „Problem Type“ den Typ **Managing offers** und in der Liste „Category“ die Option **Modifying an offer and/or SKU already in production** aus.
3.  Senden Sie die Anforderung.

Das Supportteam führt Sie durch den Vorgang zum Löschen des Angebots.

> [!NOTE] 
> Löschen eines Angebots wirkt sich nicht auf den aktuelle Käufe dieses Angebots aus. Diese Kundenkäufe funktionieren weiterhin wie zuvor.
Nach Abschluss des Löschvorgangs ist das Angebot jedoch nicht mehr für neue Käufe verfügbar.
