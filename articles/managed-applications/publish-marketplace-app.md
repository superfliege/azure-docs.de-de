---
title: Verwaltete Azure-Anwendungen im Marketplace | Microsoft-Dokumentation
description: "In diesem Artikel werden verwaltete Azure-Anwendungen beschrieben, die im Marketplace zur Verfügung stehen."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/20/2017
ms.author: gauravbh
ms.openlocfilehash: b4fda06f85c7dab52ff38558b0d928193e0694f6
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="azure-managed-applications-in-the-marketplace"></a>Verwaltete Azure-Anwendungen im Marketplace

Hersteller können ihre Lösungen mithilfe von verwalteten Azure-Anwendungen für alle Azure Marketplace-Kunden anbieten. Zu diesen Herstellern zählen Anbieter verwalteter Dienste (Managed Service Providers, MSPs), unabhängige Softwarehersteller (Independent Software Vendors, ISVs) und Systemintegratoren (SIs). Verwaltete Anwendungen verringern den Verwaltungs- und Wartungsaufwand für Kunden. Anbieter können Infrastruktur und Software über den Marketplace verkaufen. Sie können für verwaltete Anwendungen Dienste und Support für den Betrieb hinzufügen. Weitere Informationen finden Sie in der [Übersicht über verwaltete Anwendungen](overview.md).

In diesem Artikel erfahren Sie, wie Sie eine Anwendung im Marketplace veröffentlichen und für ein breites Kundenspektrum verfügbar machen.

## <a name="prerequisites-for-publishing-a-managed-application"></a>Voraussetzungen für das Veröffentlichen einer verwalteten Anwendung

Für diesen Artikel wird vorausgesetzt, dass Sie bereits über die ZIP-Datei für die Definition Ihrer verwalteten Anwendung verfügen. Weitere Informationen finden Sie unter [Erstellen einer Dienstkataloganwendung](publish-service-catalog-app.md).

Darüber hinaus müssen einige geschäftliche Voraussetzungen erfüllt sein. Sie lauten wie folgt:

* Ihr Unternehmen oder dessen Niederlassung muss sich in einem Land befinden, in dem Verkäufe über den Marketplace unterstützt werden.
* Ihr Produkt muss auf eine Weise lizenziert werden, die mit den vom Marketplace unterstützten Abrechnungsmodellen kompatibel ist.
* Für Kunden muss in wirtschaftlich vertretbarem Rahmen technischer Support bereitgestellt werden. Der Support kann kostenlos, kostenpflichtig oder mittels der Unterstützung durch die Community erfolgen.
* Lizenzieren Sie Ihre Software und sämtliche Abhängigkeiten von Drittanbietersoftware.
* Stellen Sie Inhalte bereit, die die erforderlichen Kriterien erfüllen, damit Ihr Angebot im Marketplace und im Azure-Portal gelistet wird.
* Stimmen Sie den Bedingungen der Azure Marketplace-Teilnahmerichtlinien und der Herausgebervereinbarung zu.
* Stimmen Sie der Einhaltung der Nutzungsbedingungen, der Datenschutzerklärung von Microsoft und der Microsoft Azure Certified-Programmvereinbarung zu.

## <a name="set-up-your-account-for-publishing-portal"></a>Einrichten Ihres Kontos für das Veröffentlichungsportal

Das Veröffentlichungsportal dient zum Veröffentlichen und Verwalten Ihrer Angebote. Sie benötigen einen genehmigten Microsoft-Entwickler für den Azure Marketplace, um eine Marketplace-Anwendung veröffentlichen zu können. Wenn Sie noch kein genehmigtes Konto registriert haben, finden Sie weitere Informationen unter [Erstellen eines Microsoft-Entwicklerkontos](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).

Wenn Sie über ein genehmigtes **Microsoft Developer Center**-Konto verfügen, aber bislang nicht das [Azure-Veröffentlichungsportal](https://cloudpartner.azure.com/) verwendet haben, müssen Sie sich für das Veröffentlichungsportal registrieren.

1. Öffnen Sie eine neue Chrome-Inkognito- oder Internet Explorer-InPrivate-Browsersitzung, um sicherzustellen, dass Sie nicht bei einem persönlichen Konto angemeldet sind.
2. Wechseln Sie zu [https://cloudpartner.azure.com/](https://cloudpartner.azure.com/).
3. Wenn Sie sich als neuer Benutzer zum ersten Mal beim Veröffentlichungsportal anmelden, müssen Sie sich mit der gleichen E-Mail-ID wie für Ihr Developer Center-Konto anmelden. Jetzt sind das Developer Center-Konto und das Konto für das Veröffentlichungsportal verknüpft.

Sie können die anderen Mitglieder des Unternehmens später als [Co-Administrator](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md#4-steps-to-add-a-co-admin-in-the-publishing-portal) im Veröffentlichungsportal hinzufügen. Wenn Sie im Veröffentlichungsportal als Co-Administrator hinzugefügt werden, können Sie sich mit Ihrem Co-Administratorkonto anmelden.

> [!TIP]
> Die Teilnahmerichtlinien werden auf der [Azure-Website](https://azure.microsoft.com/support/legal/marketplace/participation-policies/)beschrieben.
>
>

## <a name="create-a-new-azure-application-offer"></a>Erstellen eines neuen Angebots für eine Azure-Anwendung

Wenn Sie die Voraussetzungen erfüllen, können Sie ein Angebot für eine verwaltete Anwendung erstellen.

### <a name="set-up-an-offer"></a>Einrichten eines Angebots

Das Angebot für eine verwaltete Anwendung entspricht einer Produktklasse, die ein Herausgeber anbietet. Wenn Sie über eine neue Art von Anwendung verfügen, die Sie im Marketplace zur Verfügung stellen möchten, können Sie sie als neues Angebot einrichten. Ein Angebot ist eine Sammlung von SKUs. Jedes Angebot wird im Marketplace als eigene Entität angezeigt.

1. Melden Sie sich beim [Cloudpartnerportal](https://cloudpartner.azure.com/) an.

1. Wählen Sie im Navigationsbereich auf der linken Seite **+ Neues Angebot** > **Azure-Anwendungen**.

   ![Neues Angebot](./media/publish-marketplace-app/newOffer.png)

1. In der Ansicht **Editor** werden die erforderlichen Formulare angezeigt. Die einzelnen Formulare werden weiter unten in diesem Artikel beschrieben.

   ![Angebotseinstellungen](./media/publish-marketplace-app/newOffer_OfferSettings.png)

## <a name="offer-settings-form"></a>Formular für Angebotseinstellungen

Das Formular **Angebotseinstellungen** enthält folgende Felder:

* **Angebots-ID**: Dieser eindeutige Bezeichner gibt das Angebot innerhalb eines Herausgeberprofils an. Diese ID ist in den Produkt-URLs, den Ressourcen-Manager-Vorlagen und in den Abrechnungsberichten sichtbar. Sie darf nur aus klein geschriebenen alphanumerischen Zeichen oder Bindestrichen (-) bestehen. Die ID darf nicht mit einem Gedankenstrich enden. Sie ist auf maximal 50 Zeichen beschränkt. Nachdem ein Angebot online geschaltet wurde, wird dieses Feld gesperrt.
* **Herausgeber-ID**: In dieser Dropdownliste können Sie das Herausgeberprofil auswählen, unter dem das Angebot veröffentlicht werden soll. Nachdem ein Angebot online geschaltet wurde, wird dieses Feld gesperrt.
* **Name**: Dieser Anzeigename für Ihr Angebot wird im Marketplace und im Portal angezeigt. Er darf aus höchstens 50 Zeichen bestehen. Verwenden Sie einen Markennamen mit hohem Wiedererkennungswert für Ihr Produkt. Verwenden Sie an dieser Stelle nicht den Namen Ihres Unternehmens, es sei denn, das Produkt wird unter diesem Namen beworben. Wenn Sie dieses Angebot auf Ihrer eigenen Website bewerben, vergewissern Sie sich, dass der Name exakt mit dem auf Ihrer Website übereinstimmt.

Klicken Sie abschließend auf **Speichern**, um Ihre Eingaben zu speichern.

## <a name="skus-form"></a>Formular für SKUs

Als Nächstes fügen Sie SKUs für Ihr Angebot hinzu.

Eine SKU ist die kleinste kostenpflichtige Einheit eines Angebots. Mithilfe einer SKU innerhalb derselben Produktklasse (Angebot) können Sie zwischen Folgendem unterscheiden:

* Verschiedenen unterstützten Features
* Verwalteten oder nicht verwalteten Angeboten
* Unterstützten Abrechnungsmodellen

Eine SKU wird im Marketplace unterhalb des übergeordneten Angebots angezeigt. Sie wird als eigene kostenpflichtige Entität im Azure-Portal angezeigt.

1. Wählen Sie **SKUs** > **Neue SKU**.

   ![Auswählen einer neuen SKU](./media/publish-marketplace-app/newOffer_skus.png)

1. Geben Sie eine **SKU-ID** ein. Eine SKU-ID ist ein eindeutiger Bezeichner für die SKU in einem Angebot. Diese ID ist in den Produkt-URLs, den Ressourcen-Manager-Vorlagen und in den Abrechnungsberichten sichtbar. Sie darf nur aus klein geschriebenen alphanumerischen Zeichen oder Bindestrichen (-) bestehen. Die ID darf nicht mit einem Gedankenstrich enden und ist auf maximal 50 Zeichen beschränkt. Nachdem ein Angebot online geschaltet wurde, wird dieses Feld gesperrt. Innerhalb eines Angebots können Sie mehrere SKUs einschließen. Sie benötigen eine SKU für jedes Image, das Sie veröffentlichen möchten.

1. Füllen Sie im folgenden Formular den Abschnitt **SKU-Details** aus:

   ![Angeben einer neuen SKU](./media/publish-marketplace-app/sku-settings.png)

   Füllen Sie folgende Felder aus:

   * **Titel**: Geben Sie einen Titel für diese SKU ein. Dieser Titel wird im Katalog für dieses Element angezeigt.
   * **Zusammenfassung**: Geben Sie eine kurze Zusammenfassung für diese SKU ein. Dieser Text wird unterhalb des Titels angezeigt.
   * **Beschreibung**: Geben Sie eine ausführliche Beschreibung der SKU ein.
   * **SKU-Typ**: Die zulässigen Werte sind *Verwaltete Anwendung* und *Lösungsvorlagen*. Wählen Sie für diesen Fall *verwaltete Anwendung* aus.
   * **Verfügbarkeit in Land/Region**: Wählen Sie die Länder und Regionen aus, in denen die verwaltete Anwendung verfügbar ist.

      ![Länder auswählen](./media/publish-marketplace-app/select-country.png)

   * **Preise**: Geben Sie einen Preis für die Verwaltung der Anwendung an. Wählen Sie vor der Preisfestlegung die verfügbaren Länder und Regionen aus.

1. Fügen Sie ein neues Paket hinzu. Füllen Sie im folgenden Formular den Abschnitt **Paketdetails** aus:

   ![Paket](./media/publish-marketplace-app/new-package.png)

   Füllen Sie folgende Felder aus:

   * **Aktuelle Version**: Geben Sie eine Version für das Paket ein, das Sie hochladen. Sie sollte folgendes Format aufweisen: `{number}.{number}.{number}{number}`.
   * **Paketdatei auswählen**: Dieses Paket enthält die erforderliche Datei, die in eine ZIP-Datei komprimiert wird. Weitere Informationen finden Sie unter [Erstellen einer Dienstkataloganwendung](publish-service-catalog-app.md).
   * **PrincipalId**: Bei dieser Eigenschaft handelt es sich um den Azure AD-Bezeichner (Azure Active Directory) eines Benutzers, einer Benutzergruppe oder einer Anwendung, dem bzw. der Zugriff auf die Ressourcen im Kundenabonnement gewährt wird. Die Berechtigungen werden in der Rollendefinition beschrieben.
   * **Rollendefinition**: Bei dieser Eigenschaft handelt es sich um eine Liste von allen integrierten rollenbasierten Zugriffssteuerungsrollen (RBAC), die von Azure AD bereitgestellt werden. Sie können die Rolle auswählen, die am besten für die Verwaltung der Ressourcen im Auftrag des Kunden geeignet ist.

Sie können mehrere Autorisierungen hinzufügen. Es wird empfohlen, eine AD-Benutzergruppe zu erstellen und die jeweilige ID unter **PrincipalId** anzugeben. Auf diese Weise können Sie der Benutzergruppe weitere Benutzer hinzufügen, ohne die SKU aktualisieren zu müssen.

Weitere Informationen zur RBAC finden Sie unter [Erste Schritte mit der RBAC im Azure-Portal](../active-directory/role-based-access-control-what-is.md).

## <a name="marketplace-form"></a>Marketplace-Formular

Das Marketplace-Formular verlangt nach Feldern, die im [Azure Marketplace](https://azuremarketplace.microsoft.com) und im [Azure-Portal](https://portal.azure.com/) erscheinen.

### <a name="preview-subscription-ids"></a>Preview-Abonnement-IDs

Geben Sie eine Liste der Azure-Abonnement-IDs ein, die auf das Angebot zugreifen können, nachdem es veröffentlicht wurde. Mithilfe dieser zugelassenen Abonnements können Sie das Angebot in der Vorschau testen, bevor es online geschaltet wird. Sie können eine Whitelist von bis zu 100 Abonnements im Partnerportal kompilieren.

### <a name="suggested-categories"></a>Vorgeschlagene Kategorien

Wählen Sie bis zu fünf Kategorien aus der Liste aus, denen Ihr Angebot am besten zugeordnet werden kann. Diese Kategorien werden verwendet, um Ihr Angebot den Produktkategorien zuzuordnen, die im [Azure Marketplace](https://azuremarketplace.microsoft.com) und im [Azure-Portal](https://portal.azure.com/) verfügbar sind.

#### <a name="azure-marketplace"></a>Azure Marketplace

In der Zusammenfassung Ihrer verwalteten Anwendung werden die folgenden Felder angezeigt:

![Marketplace-Zusammenfassung](./media/publish-marketplace-app/publishvm10.png)

Auf der Registerkarte **Übersicht** für Ihre verwaltete Anwendung werden die folgenden Felder angezeigt:

![Marketplace-Übersicht](./media/publish-marketplace-app/publishvm11.png)

Auf der Registerkarte **Pläne + Preise** für Ihre verwaltete Anwendung werden die folgenden Felder angezeigt:

![Marketplace-Pläne](./media/publish-marketplace-app/publishvm15.png)

#### <a name="azure-portal"></a>Azure-Portal

In der Zusammenfassung Ihrer verwalteten Anwendung werden die folgenden Felder angezeigt:

![Zusammenfassung Portal](./media/publish-marketplace-app/publishvm12.png)

In der Übersicht für Ihre verwaltete Anwendung werden die folgenden Felder angezeigt:

![Portalübersicht](./media/publish-marketplace-app/publishvm13.png)

#### <a name="logo-guidelines"></a>Richtlinien für Logos

Befolgen Sie die folgenden Richtlinien für Logos, die Sie im Cloudpartnerportal hochladen:

*   Die Farbpalette des Azure-Designs ist einfach und geradlinig. Beschränken Sie die Anzahl der Primär- und Sekundärfarben auf Ihrem Logo.
*   Die Designfarben des Azure-Portals sind Weiß und Schwarz. Verwenden Sie diese Farben nicht als Hintergrundfarbe für Ihr Logo. Verwenden Sie eine Farbe, die Ihr Logo im Portal ideal zur Geltung bringt. Sie sollten einfache Primärfarben verwenden. *Wenn Sie einen transparenten Hintergrund verwenden, stellen Sie sicher, dass das Logo und der Text nicht weiß, schwarz oder blau sind.*
*   Verwenden Sie im Logo keinen Hintergrund mit Farbverlauf.
*   Platzieren Sie keinen Text auf dem Logo, auch nicht Ihren Firmen- oder Markennamen. Das Erscheinungsbild Ihres Logos sollte klar und direkt und frei von Farbverläufen sein.
*   Stellen Sie sicher, dass das Logo nicht verzerrt wird.

#### <a name="hero-logo"></a>Herologo

Das Herologo ist optional. Der Herausgeber muss kein Herologo hochladen. Nachdem das Herosymbol hochgeladen wurde, kann es nicht gelöscht werden. Zu diesem Zeitpunkt muss der Partner die Marketplace-Richtlinien für Herosymbole einhalten.

Befolgen Sie die folgenden Richtlinien für das Herologosymbol:

*   Anzeigename des Herausgebers, Plantitel und ausführliche Angebotszusammenfassung werden in Weiß angezeigt. Verwenden Sie aus diesem Grund keine helle Farbe für den Hintergrund des Herosymbols. Ein schwarzer, weißer und transparenter Hintergrund ist für Herosymbole nicht zulässig.
*   Nachdem das Angebot aufgeführt ist, werden Elemente programmgesteuert im Herologo eingebettet. Die eingebetteten Elemente umfassen den Anzeigename des Herausgebers, den Plantitel, die lange Zusammenfassung des Angebots und die Schaltfläche **Erstellen**. Geben Sie daher beim Entwurf des Herologos keinen Text ein. Lassen Sie auf der rechten Seite einen leeren Bereich, da der Text programmgesteuert an dieser Stelle eingefügt wird. Der leere Bereich für den Text sollte auf der rechten Seite 415 x 100 Pixel betragen. Er wird um 370 Pixel von links versetzt.

    ![Beispiel für Herologo](./media/publish-marketplace-app/publishvm14.png)

## <a name="support-form"></a>Supportformular

Füllen Sie das **Support**-Formular mit Supportkontakten von Ihrem Unternehmen aus. Bei diesen Informationen kann es sich um Kontakte des Entwicklerteams und Kontakte des Kundensupports handeln.

## <a name="publish-an-offer"></a>Veröffentlichen eines Angebots

Nachdem Sie alle Abschnitte ausgefüllt haben, wählen Sie **Veröffentlichen**, um den Vorgang zum Bereitstellen Ihres Angebots für Kunden zu starten.

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in verwaltete Anwendungen finden Sie in der [Übersicht über verwaltete Anwendungen](overview.md).
* Informationen zum Veröffentlichen einer verwalteten Dienstkataloganwendung finden Sie unter [Erstellen und Veröffentlichen einer verwalteten Dienstkataloganwendung](publish-service-catalog-app.md).
