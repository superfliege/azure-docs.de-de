---
title: Veröffentlichen einer verwalteten Azure-Anwendung in Azure Marketplace
description: Veröffentlichen einer verwalteten Azure-Anwendung in Azure Marketplace
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: qianw211
manager: pbutlerm
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: bc044c8b59c939163336ecab01546fc26a7a2643
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48806404"
---
<a name="publish-an-azure-managed-application-to-azure-marketplace"></a>Veröffentlichen einer in Azure verwalteten Anwendung in Azure Marketplace 
========================================================

In diesem Artikel werden die einzelnen Schritte zum Veröffentlichen eines Angebots für eine verwaltete Anwendung in Azure Marketplace aufgeführt.

<a name="pre-requisites-for-publishing-a-managed-application"></a>Voraussetzungen für das Veröffentlichen einer verwalteten Anwendung 
---------------------------------------------------

Voraussetzungen für die Auflistung in Azure Marketplace

1.  Technisch

    -   [Erstellen von Azure Resource-Manager-Vorlagen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates)

    -   Azure-Schnellstartvorlagen:

        -   <https://azure.microsoft.com/documentation/templates/>

        -   <https://github.com/azure/azure-quickstart-templates>

    -   Erstellen von Benutzeroberflächendefinitionen

        -   [Erstellen einer Definitionsdatei für die Benutzeroberfläche](https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview)

2.  Nicht technisch (Geschäftsanforderungen)

    -   Der Standort Ihres Unternehmens (oder der Niederlassung) muss sich in einem vom Azure Marketplace unterstützten Ursprungsland befinden.

    -   Ihr Produkt muss auf eine Weise lizenziert sein, die mit den vom Azure Marketplace unterstützten Abrechnungsmodellen kompatibel ist.

    -   Sie sind verantwortlich für die Bereitstellung des technischen Supports für Ihre Kunden: kostenlos, kostenpflichtig oder über die Community.

    -   Sie sind verantwortlich für die Lizenzierung Ihrer Software und sämtlicher Abhängigkeiten von Drittanbietersoftware.

    -   Sie müssen Inhalte bereitstellen, die die Kriterien erfüllen, damit Ihr Angebot im Azure Marketplace und im Azure-Verwaltungsportal aufgelistet wird.

    -   Sie müssen den Bedingungen der Azure Marketplace-Beteiligungsrichtlinien und der Herausgebervereinbarung zustimmen.

    -   Sie müssen der Einhaltung der Nutzungsbedingungen, der Datenschutzerklärung von Microsoft und der Microsoft Azure Certified-Programmvereinbarung zustimmen.

<a name="how-to-create-a-new-azure-application-offer"></a>So erstellen Sie ein neues Azure Application-Angebot 
-------------------------------------------

Nachdem Sie die Voraussetzungen erfüllt haben, können Sie mit dem Erstellen Ihres Angebots für die verwaltete Anwendung beginnen. Bevor Sie beginnen, finden Sie hier einen kurzen Überblick über Angebote und SKUs.

**Angebot**

Ein Angebot für eine Azure-Anwendung entspricht einer Produktangebotsklasse eines Herausgebers. Wenn Sie eine neue Lösung/Anwendung im Azure Marketplace veröffentlichen möchten, verwenden Sie hierzu ein neues Angebot. Ein Angebot ist eine Sammlung von SKUs. Jedes Angebot wird als eigene Entität im Azure Marketplace angezeigt.

**SKU**

Eine SKU ist ein Angebot mit der kleinsten kostenpflichtigen Einheit. SKUs befinden sich zwar in derselben Produktklasse (Angebot), ermöglichen es Ihnen aber, zwischen verschiedenen Funktionen, egal ob es sich um ein verwaltetes oder nicht verwaltetes Angebot handelt, und Abrechnungsmodellen zu unterscheiden.

Fügen Sie mehrere SKUs hinzu, wenn Sie verschiedene Abrechnungsmodelle unterstützen möchten, z.B. BYOL (Bring Your Own License), nutzungsbasierte Bezahlung (Pay As You Go, PAYG) usw. 

Sie können mehrere SKUs hinzufügen, wenn jede SKU unterschiedliche Funktionsmerkmale unterstützen und jeweils separate Preise aufweisen soll.

Eine SKU wird im Azure Marketplace unter dem übergeordneten Angebot, unter azure.com jedoch als eigene Kaufeinheit angezeigt.

1.  Melden Sie sich beim [Cloudpartnerportal](http://cloudpartner.azure.com) an.

2.  Klicken Sie in der Navigationsleiste links auf \"+ Neues Angebot\", und wählen Sie \"Azure-Anwendungen\" aus.

    ![Neues Angebot](./media/cloud-partner-portal-publish-managed-app/newOffer.png)

3.  Eine neue \"Editor\"-Ansicht wird nun geöffnet, und Sie können mit der Erstellung beginnen.

4.  Die \"Formulare\", die ausgefüllt werden müssen, werden in der \"Editor\"-Ansicht auf der linken Seite angezeigt. Jedes \"Formular\" besteht aus einer Reihe von Feldern, die ausgefüllt werden müssen. Erforderliche Felder sind mit einem roten Sternchen gekennzeichnet (\*).

    > Es gibt vier Hauptformulare zum Erstellen einer verwalteten Anwendung.

    -   Angebotseinstellungen

    -   SKUs

    -   Marketplace

    -   Support

<a name="how-to-fill-out-the-offer-settings-form"></a>So füllen Sie das Formular „Angebotseinstellungen“ aus 
---------------------------------------

Das Formular für die Angebotseinstellungen ist ein grundlegendes Formular, in dem Sie die Angebotseinstellungen angeben.
Die einzelnen Felder werden nachfolgend beschrieben.

**Angebots-ID**

Ein eindeutiger Bezeichner für das Angebot innerhalb eines Herausgeberprofils.
Sie wird in den Produkt-URLs, den Resource Manager-Vorlagen und in den Abrechnungsberichten angezeigt. Sie darf nur aus klein geschriebenen alphanumerischen Zeichen oder Bindestrichen (-) bestehen. Sie darf nicht mit einem Bindestrich enden und höchstens 50 Zeichen umfassen. Beachten Sie, dass dieses Feld gesperrt wird, sobald ein Angebot online geschaltet wird.

**Herausgeber-ID**

In dieser Dropdownliste können Sie das Herausgeberprofil auswählen, unter dem das Angebot veröffentlicht werden soll. Beachten Sie, dass dieses Feld gesperrt wird, sobald ein Angebot online geschaltet wird.

**Name**

Der Anzeigenamen für Ihr Angebot. Dieser Name wird im Azure Marketplace und im Azure-Portal angezeigt. Er darf aus höchstens 50 Zeichen bestehen. Hier empfiehlt es sich, einen Markennamen mit gutem Wiedererkennungswert für Ihr Produkt zu verwenden. Verwenden Sie an dieser Stelle nicht den Namen Ihres Unternehmens, es sei denn, das Produkt wird unter diesem Namen beworben. Wenn Sie das Angebot auf Ihrer eigenen Website bewerben, vergewissern Sie sich, dass der Name exakt mit dem auf Ihrer Website übereinstimmt.

Klicken Sie auf \"Speichern\", um Ihre Eingaben zu speichern. Als Nächstes fügen Sie SKUs für Ihr Angebot hinzu.

<a name="how-to-create-skus"></a>So erstellen Sie SKUs 
------------------

Klicken Sie auf das Formular \"SKUs\". Klicken Sie auf die angezeigte Option \"SKU hinzufügen\", um eine \"SKU-ID\" einzugeben.

![SKUs für neue Angebote](./media/cloud-partner-portal-publish-managed-app/newOffer_skus.png)

\"SKU ID\" ist ein eindeutiger Bezeichner für die SKU in einem Angebot. Diese ID wird in den Produkt-URLs, den ARM-Vorlagen und den Abrechnungsberichten sichtbar sein. Sie darf nur aus klein geschriebenen alphanumerischen Zeichen oder Bindestrichen (-) bestehen.
Sie darf nicht mit einem Bindestrich enden und höchstens 50 Zeichen umfassen. Beachten Sie, dass dieses Feld gesperrt wird, sobald ein Angebot online geschaltet wird. Innerhalb eines Angebots können Sie mehrere SKUs einschließen. Sie benötigen eine SKU für jedes Image, das Sie veröffentlichen möchten.

Nachdem eine SKU hinzugefügt wurde, wird diese in der SKU-Liste des Formulars \"SKUs\" aufgeführt. Klicken Sie auf den SKU-Namen, um die Details der betreffenden SKU zu öffnen. Hier finden Sie einige Details für einige der Felder.

Nach dem Klicken auf \"Neue SKU\" müssen Sie das folgende Formular ausfüllen:

![Neues Angebot – neue SKU](./media/cloud-partner-portal-publish-managed-app/newOffer_newsku.png)

### <a name="how-to-fill-sku-details-section"></a>So füllen Sie den Abschnitt mit den SKU-Details aus 

**Titel:** Geben Sie einen Titel für diese SKU ein. Dieser wird im Katalog für dieses Element angezeigt.

**Zusammenfassung:** Geben Sie eine kurze Zusammenfassung für diese SKU ein. Dieser Text wird rechts unterhalb des Titels angezeigt.

**Beschreibung:** Geben Sie eine ausführliche Beschreibung der SKU an.

**SKU-Typ:** Die zulässigen Werte sind \"Verwaltete Anwendung\" und \"Lösungsvorlagen\". Wählen Sie für diesen Fall \"Verwaltete Anwendung\" aus.

### <a name="how-to-fill-package-details-section"></a>So füllen Sie den Abschnitt mit den Paketdetails aus 

Der Paket-Abschnitt enthält die folgenden Felder, die ausgefüllt werden müssen

![Neues Angebot – neues SKU-Paket](./media/cloud-partner-portal-publish-managed-app/newOffer_newsku_package.png)

**Aktuelle Version:** Geben Sie eine Version für das Paket an, das Sie hochladen werden.
Sie sollte das folgende Format aufweisen.

**Paketdatei:** Dieses Paket enthält die folgenden Dateien, die in einer ZIP-Datei komprimiert werden.

applianceMainTemplate.json: Die Vorlagendatei für die Bereitstellung, die zum Bereitstellen der Lösung/Anwendung und zum Erstellen der darin definierten Ressourcen verwendet wird. Weitere Informationen zum Erstellen von Bereitstellungsvorlagendateien finden Sie unter <https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template>.

applianceCreateUIDefinition.json: Diese Datei wird vom Portal unter azure.com verwendet, um die Benutzeroberfläche für die Bereitstellung dieser Lösung/Anwendung zu generieren. Weitere Einzelheiten zum Erstellen dieser Datei finden Sie unter <https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview>.

mainTemplate.json: Dies ist die Vorlagendatei, die nur die Microsoft.Solution/appliances-Ressource enthält. Die Schlüsseleigenschaften, die es für diese Ressource zu berücksichtigen gilt, sind Folgende:

-   \"kind:\" Der Wert muss bei einem Szenario mit einer im Marketplace verwalteten Anwendung \"Marketplace\" lauten.
-   \"ManagedResourceGroupId:\" Ressourcengruppe im Abonnement des Kunden, in der alle Ressourcen, die in der Datei „applianceMainTemplate.json“ definiert sind, bereitgestellt werden.
-   \"PublisherPackageId:\" Zeichenfolge, die das Paket eindeutig identifiziert. 

Der Wert muss durch die Verkettung \[publisherId\].\[OfferId\]-preview\[SKUID\].\[PackageVersion\] erstellt werden.
Jeder dieser Werte kann wie unten dargestellt abgerufen werden.

Dieses Paket sollte alle anderen geschachtelten Vorlagen oder Skripts enthalten, die erforderlich sind, um diese Anwendung erfolgreich bereitzustellen. Die Dateien „mainTemplate.json“, „applianceMainTemplate.json“ und „applianceCreateUIDefinition.json“ müssen im Stammordner enthalten sein.

**Authorizations:** Diese Eigenschaft definiert, wer auf welcher Ebene Zugriff auf Ressourcen in Abonnements von Kunden erhält. Dies ermöglicht dem Herausgeber, die Anwendung für die Kunden zu verwalten.

-   PrincipalId: Azure Active Directory-Bezeichner eines Benutzers, einer Benutzergruppe oder einer Anwendung, dem bzw. der bestimmte Berechtigungen (wie in der Rollendefinition erläutert) auf die Ressourcen im Kunden-Abonnement gewährt werden.

-   RoleDefinition: Liste aller integrierten RBAC-Rollen, die von Azure AD bereitgestellt werden. Sie können die Rolle auswählen, die am besten geeignet ist und Ihnen ermöglicht, die Ressourcen für die Kunden zu verwalten.

Mehrere Autorisierungen können hinzugefügt werden. Es wird jedoch empfohlen, eine Active Directory-Benutzergruppe zu erstellen und ihre ID in der \"PrincipalId\. festzulegen.  Dies ermöglicht das Hinzufügen weiterer Benutzer zu der Benutzergruppe, ohne dass ein SKU-Update erforderlich ist.

Weitere Einzelheiten zu RBAC finden Sie unter <https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is>.

<a name="marketplace-form"></a>Marketplace-Formular
----------------

Im Marketplace-Formular eines Angebots für eine Azure-Anwendung müssen Felder ausgefüllt werden, die im [Azure Marketplace](https://azuremarketplace.microsoft.com) und im [Azure-Portal](https://portal.azure.com/) angezeigt werden. Hier finden Sie einige Details für einige der Felder.

#### <a name="preview-subscription-ids"></a>Abonnement-IDs für die Vorschauversion

Geben Sie hier eine Liste der Azure-Abonnement-IDs ein, die nach der Veröffentlichung Zugriff auf das Angebot haben sollen. Mithilfe dieser zugelassenen Abonnements können Sie das Angebot in der Vorschau testen, bevor es live geschaltet wird. Im Partnerportal können Sie bis zu 100 Abonnements zulassen.

#### <a name="suggested-categories"></a>Vorgeschlagene Kategorien

Wählen Sie bis zu fünf Kategorien aus der vorhandenen Liste, denen Ihr Angebot am besten zugeordnet werden kann. Die ausgewählten Kategorien werden verwendet, um Ihr Angebot den Produktkategorien zuzuordnen, die im [Azure Marketplace](https://azuremarketplace.microsoft.com) und im [Azure-Portal](https://portal.azure.com/) verfügbar sind.

Im Folgenden sind einige Bereiche aufgeführt, in denen die in diesem Formular angegebenen Daten angezeigt werden.

##### <a name="azure-marketplace"></a>Azure Marketplace

![publishvm10](./media/cloud-partner-portal-publish-managed-app/publishvm10.png)

![publishvm11](./media/cloud-partner-portal-publish-managed-app/publishvm11.png)

![publishvm15](./media/cloud-partner-portal-publish-managed-app/publishvm15.png)

##### <a name="portal-at-azurecom"></a>Portal unter azure.com

![publishvm12](./media/cloud-partner-portal-publish-managed-app/publishvm12.png)

![publishvm13](./media/cloud-partner-portal-publish-managed-app/publishvm13.png)

##### <a name="logo-guidelines"></a>Richtlinien für Logos

Alle in das Cloud Partner-Portal hochgeladen Logos müssen folgende Richtlinien erfüllen:

-   Die Farbpalette des Azure-Designs ist einfach und geradlinig. Verwenden Sie auf Ihrem Logo möglichst wenige Primär- und Sekundärfarben.

-   Die Designfarben des Portals unter azure.com sind Weiß und Schwarz. Verwenden Sie diese Farben nicht als Hintergrundfarbe für Ihre Logos. Verwenden Sie Farben, die Ihre Logos im Portal unter azure.com markant hervorstechen lassen.
    Sie sollten einfache Primärfarben verwenden. **Wenn Sie transparenten Hintergrund verwenden, stellen Sie sicher, dass Logos und Text nicht weiß, schwarz oder blau sind.**

-   Verwenden Sie im Logo keinen Hintergrund mit Farbverlauf.

-   Platzieren Sie nach Möglichkeit keinen Text auf dem Logo, auch nicht Ihren Firmen- oder Markennamen.
    Das Erscheinungsbild Ihres Logos sollte \'klar und direkt\' und frei von Farbverläufen sein.

-   Vermeiden Sie ein Strecken des Logos.

##### <a name="hero-logo"></a>Herologo

Ein Herologo ist optional. Der Herausgeber muss kein Herologo hochladen. Ist das Herosymbol jedoch einmal hochgeladen, dann kann es nicht mehr gelöscht werden. In diesem Fall muss der Partner die Azure Marketplace-Richtlinien für Herosymbole einhalten.

###### <a name="guidelines-for-the-hero-logo-icon"></a>Richtlinien für das Herologosymbol

-   Anzeigename des Herausgebers, Plantitel und ausführliche Angebotsübersicht werden in weißer Schrift angezeigt. Vermeiden Sie helle Farben im Hintergrund des Herosymbols. Ein schwarzer, weißer oder transparenter Hintergrund ist für Herosymbole nicht zulässig.

-   Der Anzeigename des Herausgebers, der Plantitel, die ausführliche Angebotsübersicht und die Erstellen-Schaltfläche werden programmgesteuert in das Herosymbol eingebettet, wenn das Angebot live geschaltet wird. Sie müssen beim Entwerfen des Herosymbols keinen Text eingeben. Lassen Sie rechts neben dem Herausgebernamen, Plantitel, der ausführlichen Angebotsübersicht usw. Freiraum. Sie sind programmgesteuert enthalten.
    Der Freiraum für den Text sollte 415 x 100 auf der rechten Seite betragen und ist 370 Pixel von links versetzt.

![publishvm14](./media/cloud-partner-portal-publish-managed-app/publishvm14.png)

<a name="support-form"></a>Supportformular
------------

Das nächste auszufüllende Formular ist das Supportformular. In diesem Formular geben Sie die Supportkontakte Ihres Unternehmens ein.  Beispiele sind Kontaktdaten Ihrer Techniker und die Informationen zum Kundensupport.

<a name="how-to-publish-an-offer"></a>So veröffentlichen Sie ein Angebot
-----------------------

Sie haben Ihr Angebot entworfen. Im nächsten Schritt veröffentlichen Sie das Angebot im Azure Marketplace. Klicken Sie auf die Schaltfläche \"Veröffentlichen\", um den Vorgang, mit dem Ihr Angebot live geschaltet wird, zu starten.
