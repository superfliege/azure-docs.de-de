---
title: Verwalten des Azure-Endpunktabonnements | Microsoft-Dokumentation
description: In diesem Artikel erstellen Sie einen gemessenen Endpunktschlüssel für Ihr LUIS-Konto, der unbegrenzten Datenverkehr für Ihren Endpunkt nach einem Zahlungsplan bereitstellt.
services: cognitive-services
author: v-geberr
manager: Kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/21/2018
ms.author: v-geberr
ms.openlocfilehash: 3526871f126ac975f323fe84b14883b361b684ae
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "35378531"
---
# <a name="manage-azure-endpoint-subscription-keys"></a>Verwalten von Azure-Endpunkt-Abonnementschlüsseln

Verwenden Sie den Free-Tarif (F0) ausschließlich für Tests und Prototypen. Verwenden Sie für Produktionssysteme einen [bezahlten](https://aka.ms/luis-price-tier) Tarif. 

> [!NOTE]
> Verwenden Sie den [Erstellungsschlüssel](luis-concept-keys.md#authoring-key) nicht für Endpunktabfragen in der Produktion.

<a name="create-luis-service"></a>
## <a name="create-luis-endpoint-key"></a>Erstellen eines LUIS-Endpunktschlüssels

1. Anmelden bei **[Microsoft Azure](https://ms.portal.azure.com/)** 
2. Klicken Sie auf das grüne Zeichen **+** im linken oberen Bereich, und suchen Sie im Marketplace nach „LUIS“. Klicken Sie dann auf **Language Understanding Intelligent Service**, und befolgen Sie das **Erstellungsverfahren**, um ein LUIS-Abonnementkonto zu erstellen. 

    ![Azure Search](./media/luis-azure-subscription/azure-search.png) 

3. Konfigurieren Sie das Abonnement mit Einstellungen, z.B. dem Kontonamen, Tarifen usw. 

    ![Azure-API-Auswahl](./media/luis-azure-subscription/azure-api-choice.png) 

4. Nach dem Erstellen des LUIS-Diensts werden die generierten Zugriffsschlüssel unter **Ressourcenverwaltung->Schlüssel** angezeigt.  

    ![Azure-Schlüssel](./media/luis-azure-subscription/azure-keys.png)

    > [!Note] 
    > * Melden Sie sich bei der [LUIS](luis-reference-regions.md)-Website für Ihre Region an, und [fügen Sie den neuen LUIS-Endpunktschlüssel hinzu](Manage-Keys.md#assign-endpoint-key). 
    > * Sie müssen sich den Namen des erstellten Azure-Diensts merken, damit Sie ihn auf der [LUIS](luis-reference-regions.md)-Veröffentlichungsseite der Region auswählen können.  

## <a name="change-luis-pricing-tier"></a>Ändern des LUIS-Tarifs

1.  Suchen Sie in [Azure](https://portal.azure.com) nach Ihrem LUIS-Abonnement. Klicken Sie auf das LUIS- Abonnement.
    ![Suchen nach Ihrem LUIS-Abonnement](./media/luis-usage-tiers/find.png)
2.  Klicken Sie auf **Tarif**, um die verfügbaren Tarife anzuzeigen. 
    ![Anzeigen der Tarife](./media/luis-usage-tiers/subscription.png)
3.  Klicken Sie auf den Tarif, und klicken Sie dann auf **Auswählen**, um die Änderung zu speichern. 
    ![Ändern Ihres LUIS-Tarifs](./media/luis-usage-tiers/plans.png)
4.  Wenn die Änderung des Tarifs abgeschlossen ist, wird der neue Tarif in einem Popupfenster bestätigt. 
    ![Überprüfen Ihres LUIS-Tarifs](./media/luis-usage-tiers/updated.png)
5. Denken Sie daran, [diesen Endpunktschlüssel](manage-keys.md#assign-endpoint-key) auf der Seite **Publish** (Veröffentlichen) zuzuweisen und für alle Endpunktabfragen zu verwenden. 

## <a name="exceed-pricing-tier-usage"></a>Überschreitung der Nutzung im Tarif
Jeder Tarif ermöglicht Endpunktanforderungen für Ihr LUIS-Konto mit einer bestimmten Rate. Wenn die Anforderungsrate höher als die zulässige Rate für Ihr gemessenes Konto pro Minute oder pro Monat ist, lösen Anforderungen den HTTP-Fehler „429: zu viele Anfragen“ aus.

Jeder Tarif ermöglicht kumulative Anforderungen pro Monat. Wenn die Gesamtzahl der Anforderungen die zulässige Rate übersteigt, lösen Anforderungen den HTTP-Fehler „403: verboten“ aus.  

## <a name="viewing-summary-usage"></a>Anzeigen der Nutzungszusammenfassung
Sie können in Azure LUIS-Nutzungsinformationen anzeigen. Auf der Seite **Übersicht** werden aktuelle zusammenfassende Informationen angezeigt, einschließlich Aufrufen und Fehlern. Wenn Sie eine LUIS-Endpunktanforderung ausführen, kann es bis zu fünf Minuten dauern, bis die Nutzung auf der Seite **Übersicht** angezeigt wird.

![Anzeigen der Nutzungszusammenfassung](./media/luis-usage-tiers/overview.png)

## <a name="customizing-usage-charts"></a>Anpassen von Nutzungsdiagrammen
Metriken erlauben eine detailliertere Darstellung der Daten.

![Standardmetriken](./media/luis-usage-tiers/metrics-default.png)

Sie können Ihre Metrikdiagramme für Zeitraum und Metriktyp konfigurieren. 

![Benutzerdefinierte Metriken](./media/luis-usage-tiers/metrics-custom.png)

## <a name="total-transactions-threshold-alert"></a>Warnung für Gesamttransaktions-Schwellenwert
Wenn Sie informiert werden möchten, wenn Sie einen bestimmten Transaktionsschwellenwert erreicht haben, z.B. 10.000 Transaktionen, können Sie eine Warnung erstellen. 

![Standardwarnungen](./media/luis-usage-tiers/alert-default.png)

Fügen Sie eine Metrikwarnung für die Metrik der **Summe der Aufrufe** für einen bestimmten Zeitraum hinzu. Fügen Sie die E-Mail-Adressen aller Personen hinzu, die die Warnung erhalten sollen. Fügen Sie Webhooks für alle Systeme hinzu, die die Warnung erhalten sollen. Sie können auch eine Logik-App ausführen, wenn die Warnung ausgelöst wird. 

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Verwendung von [Versionen](luis-how-to-manage-versions.md) zum Verwalten von Änderungen an Ihrer LUIS-App.