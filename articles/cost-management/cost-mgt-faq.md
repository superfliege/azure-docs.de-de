---
title: "Häufig gestellte Fragen zu Azure Cost Management | Microsoft-Dokumentation"
description: "Enthält Antworten auf einige der häufig gestellten Fragen zu Azure Cost Management."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 10/23/2017
ms.topic: article
ms.service: cost-management
manager: carmonm
ms.custom: 
ms.openlocfilehash: a01d8d1ed0f5234f4950d448b54087767353c8ef
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="frequently-asked-questions-for-azure-cost-management"></a>Häufig gestellte Fragen zu Azure Cost Management


In diesem Artikel werden einige häufig gestellte Fragen zu Azure Cost Management (auch als Cloudyn bezeichnet) beantwortet. Wenn Sie Fragen zu Azure Cost Management haben, können Sie diese unter [FAQs for Azure Cost Management by Cloudyn](https://social.msdn.microsoft.com/Forums/en-US/231bf072-2c71-4121-8339-ac9d868137b9/faqs-for-azure-cost-management-by-cloudyn?forum=Cloudyn) (Häufig gestellte Fragen zu Azure Cost Management per Cloudyn) stellen.

## <a name="how-can-i-resolve-common-indirect-enterprise-setup-problems"></a>Wie kann ich Fehler beheben, die häufig bei der indirekten Enterprise-Einrichtung auftreten?

Wenn Sie das Cloudyn-Portal zum ersten Mal verwenden, werden möglicherweise die folgenden Meldungen angezeigt, wenn Sie ein Benutzer von Enterprise Agreement oder von Cloudlösungsanbieter (CSP) sind:

- Im Assistenten für die Einrichtung von Azure Cost Management: **The specified API key is not a top level enrollment key** (Der angegebene API-Schlüssel ist kein Schlüssel für die Registrierung auf der obersten Ebene)
- Im Enterprise Agreement-Portal: „Direct Enrollment – No“ (Direkte Registrierung – Nein)
- Im Cloudyn-Portal: „No usage data was found for the last 30 days.“ Please contact your distributor to make sure markup was enabled for your Azure account“ (Für die letzten 30 Tage wurden keine Nutzungsdaten gefunden. Wenden Sie sich an Ihren Distributor, um sicherzustellen, dass für Ihr Azure-Konto Markup aktiviert wurde.)

Mit den obigen Fehlermeldungen wird angegeben, dass Sie über einen Handelspartner oder CSP ein Azure Enterprise Agreement (EA) erworben haben. Ihr Handelspartner oder CSP muss _Markup_ für Ihr Azure-Konto aktivieren, damit Sie Ihre Daten in Cloudyn anzeigen können.

Sie können die Probleme wie folgt beheben:

1. Ihr Handelspartner muss _Markup_ für Ihr Konto aktivieren. Die Anleitung finden Sie unter [Indirect Customer Onboarding Guide (Anleitung für indirektes Kundenonboarding)](https://ea.azure.com/api/v3Help/v2IndirectCustomerOnboardingGuide).

2. Sie generieren den Azure EA-Schlüssel für die Verwendung mit Cloudyn. Eine Anleitung hierzu finden Sie unter [Adding Your Azure EA (Hinzufügen Ihres Azure EA)](https://support.cloudyn.com/hc/en-us/articles/210429585-Adding-Your-AZURE-EA) oder [How to Find Your EA Enrollment ID and API Key (Ermitteln Ihrer EA-Registrierungs-ID und Ihres API-Schlüssels)](https://youtu.be/u_phLs_udig).

Cost Management kann nur von einem Azure-Dienstadministrator aktiviert werden. Co-Administrator-Berechtigungen sind nicht ausreichend.

Sie müssen die Azure-Abrechnungs-API aktivieren, bevor Sie den Azure EA-API-Schlüssel für die Einrichtung von Cloudyn generieren können. Befolgen Sie hierfür diese Anleitungen:

- [Überblick über Berichterstellungs-APIs für Unternehmenskunden](../billing/billing-enterprise-api.md)
- [Microsoft Azure Enterprise Portal-Berichterstellungs-API](https://ea.azure.com/helpdocs/reportingAPI) unter **Aktivieren des Datenzugriffs für die API**


Unter Umständen müssen Sie auch Abteilungsadministratoren, Kontobesitzern und Unternehmensadministratoren Berechtigungen zum _Anzeigen von Gebühren_ per Abrechnungs-API gewähren.

## <a name="how-do-i-enable-suspended-or-locked-out-users"></a>Wie kann ich angehaltene oder gesperrte Benutzer aktivieren?

Wenn Sie eine Warnung mit der Anforderung erhalten, dass der Zugriff für einen Benutzer zugelassen werden soll, müssen Sie das Benutzerkonto aktivieren.

Aktivieren Sie das Benutzerkonto wie folgt:

1. Melden Sie sich bei Cloudyn mit dem Azure-Administratorbenutzerkonto an, das Sie zum Einrichten von Cloudyn verwendet haben. Oder melden Sie sich mit einem Benutzerkonto an, für das der Administratorzugriff gewährt wurde.

2. Klicken Sie oben rechts auf das Zahnradsymbol, und klicken Sie dann auf **User Management** (Benutzerverwaltung).

3. Suchen Sie nach dem Benutzer, und klicken Sie dann auf das Stiftsymbol, um den Benutzer zu bearbeiten.

4. Ändern Sie den Status unter **User status** (Benutzerstatus) von **Suspended** (Angehalten) in **Active** (Aktiv).

Für Cloudyn-Benutzerkonten wird eine Verbindung hergestellt, indem das einmalige Anmelden von Azure verwendet wird. Wenn ein Benutzer sein Kennwort falsch eingibt, wird er ggf. für Cloudyn gesperrt, auch wenn der Zugriff auf Azure weiterhin möglich ist.

Wenn Sie Ihre E-Mail-Adresse in Cloudyn von der Standardadresse in Azure in eine andere ändern, kann dies dazu führen, dass Ihr Konto gesperrt wird. Unter Umständen wird der folgende Fehler angezeigt: „Status initiallySuspended“. Wenn Ihr Benutzerkonto gesperrt ist, können Sie einen anderen Administrator bitten, Ihr Konto zurückzusetzen.

Es wird empfohlen, mindestens zwei Cloudyn-Administratorkonten zu erstellen, falls eines der Konten gesperrt wird.

Wenn Sie sich nicht beim Cloudyn-Portal anmelden können, sollten Sie sicherstellen, dass Sie die richtige Azure Cost Management-URL für die Anmeldung bei Cloudyn verwenden. Verwenden Sie eine der folgenden URLs:

- https://azure.cloudyn.com
- https://ms.portal.azure.com/#blade/Microsoft_Azure_CostManagement/CloudynMainBlade

Vermeiden Sie die Verwendung der direkten Cloudyn-URL „https://app.cloudyn.com“.

## <a name="how-do-i-activate-unactivated-accounts-with-azure-credentials"></a>Wie aktiviere ich deaktivierte Konten mit Azure-Anmeldeinformationen?

Nachdem Ihre Azure-Konten von Cloudyn ermittelt wurden, werden in kostenbasierten Berichten sofort Kostendaten bereitgestellt. Damit Cloudyn Daten zur Nutzung und Leistung bereitstellen kann, müssen Sie Ihre Azure-Anmeldeinformationen für die Konten registrieren. Anweisungen finden Sie unter [Hinzufügen von Azure Resource Manager](https://support.cloudyn.com/hc/en-us/articles/212784085-Adding-Azure-Resource-Manager).

Klicken Sie zum Hinzufügen von Azure-Anmeldeinformationen für ein Konto im Cloudyn-Portal auf das Bearbeitungssymbol rechts vom Kontonamen (nicht neben dem Abonnement).

Bis Ihre Azure-Anmeldeinformationen Cloudyn hinzugefügt wurden, wird Ihr Konto als _deaktiviert_ angezeigt.

## <a name="how-do-i-add-multiple-accounts-and-entities-to-an-existing-subscription"></a>Wie füge ich einem vorhandenen Abonnement mehrere Konten und Entitäten hinzu?

Zusätzliche Entitäten werden verwendet, um einem Cloudyn-Abonnement weitere Enterprise Agreements hinzuzufügen. Unter den folgenden Links wird beschrieben, wie Sie zusätzliche Entitäten hinzufügen:

- Artikel [Adding an Entity](https://support.cloudyn.com/hc/en-us/articles/212016145-Adding-an-Entity) (Hinzufügen einer Entität)
- Video [Defining your hierarchy with Cost Entities](https://support.cloudyn.com/hc/en-us/articles/115005142529-Video-Defining-your-hierarchy-with-Cost-Entities) (Definieren Ihrer Hierarchie mit Kostenentitäten)

Für CSPs:

Wählen Sie zum Hinzufügen von zusätzlichen CSP-Konten zu einer Entität beim Erstellen der neuen Entität anstelle von **Enterprise** die Option **MSP Access** (MSP-Zugriff). Wenn Ihr Konto als Enterprise Agreement registriert ist und Sie CSP-Anmeldeinformationen hinzufügen möchten, müssen Ihre Kontoeinstellungen ggf. vom Cloudyn-Support geändert werden. Wenn Sie über ein kostenpflichtiges Azure-Abonnement verfügen, können Sie im Azure-Portal eine neue Supportanforderung erstellen. Klicken Sie auf **Hilfe + Support** und dann auf **Neue Supportanforderung**.

## <a name="how-do-i-change-the-currency-symbol-used-in-cloudyn"></a>Wie ändere ich das in Cloudyn verwendete Währungssymbol?

Wenn für alle Azure-Konten einer einzelnen Entität die gleiche Währung verwendet wird, wird die jeweilige Währung automatisch erkannt. Für die folgenden Währungen wird das Währungssymbol jedoch fälschlicherweise als **$** angezeigt:

- GBP = Pfund Sterling (Vereinigtes Königreich)
- EUR = Europäische Euro
- INR = Indische Rupie
- NOK = Norwegische Krone

Auch wenn das Währungssymbol ggf. als **$** (US-Dollar) angezeigt wird, werden die Kosten für Ihre Währung richtig angegeben. Wenn für Ihre gesamten Konten für eine Entität beispielsweise jeweils Euro verwendet wird, sind die _Werte_ in Cloudyn Eurowerte – auch wenn fälschlicherweise das Symbol **$** angezeigt wird.

Wenn Sie Azure EA-Kunde sind, kann der Cloudyn-Support Ihr Währungssymbol ändern, falls in Kostenberichten „$“ verwendet wird. Sie können im Azure-Portal eine neue Supportanfrage erstellen. Klicken Sie auf **Hilfe + Support** und dann auf **Neue Supportanforderung**.

Wenn Sie CSP-Kunde sind, können Sie Ihr Währungssymbol nicht ändern. Cloudyn unterstützt nur Gebührenkarten mit US-Dollar als Währung. Für Cloudyn wird derzeit die Unterstützung von Gebührenkarten in unterschiedlichen Währungen geprüft.

## <a name="what-are-cloudyn-data-refresh-timelines"></a>Was sind Cloudyn-Zeitachsen für die Datenaktualisierung?

Cloudyn verfügt über die folgenden Zeitachsen für die Datenaktualisierung:

- **Initial** (Anfang): Nach dem Einrichten kann es bis zu 24 Stunden dauern, bis Kostendaten in Cloudyn angezeigt werden. Es kann auch bis zu zehn Tage dauern, bis Cloudyn genügend Daten gesammelt hat, um Größenempfehlungen anzeigen zu können.
- **Daily** (Täglich): Vom 10. bis zum Ende jedes Monats sollte der aktuelle Stand Ihrer Daten in Cloudyn jeweils bis zum vorherigen Tag angezeigt werden (ab ca. UTC+3 am nächsten Tag).
- **Monthly** (Monatlich): Vom 1. bis zum 10. des Monats werden Ihre Daten in Cloudyn ggf. nur bis zum Ende des vorherigen Monats angezeigt.

In Cloudyn werden Daten für den vorherigen Tag verarbeitet, wenn alle Daten des vorherigen Tags verfügbar sind. Die Daten des vorherigen Tags stehen in Cloudyn normalerweise jeweils ab ca. UTC+3 zur Verfügung. Die Verarbeitung einiger Daten, z.B. von Tags, kann 24 Stunden länger dauern.

Daten für den aktuellen Monat können am Anfang des Monats nicht gesammelt werden. Während dieses Zeitraums schließen Dienstanbieter ihre Abrechnungen für den vorherigen Monat ab. Die Daten des vorherigen Monats werden in Cloudyn fünf bis zehn Tage nach Beginn eines Monats angezeigt. Während dieses Zeitraums werden ggf. nur amortisierte Kosten des vorherigen Monats angegeben. Tägliche Abrechnungs- oder Nutzungsdaten werden unter Umständen nicht angezeigt. Wenn die Daten verfügbar sind, werden sie von Cloudyn im Nachhinein verarbeitet. Nach der Verarbeitung werden alle monatlichen Daten für die Zeit zwischen dem 5. und 10. jedes Monats angezeigt.

Wenn es beim Senden von Daten aus Azure an Cloudyn zu einer Verzögerung kommt, werden die Daten trotzdem in Azure aufgezeichnet. Die Daten werden an Cloudyn übertragen, nachdem die Verbindung wiederhergestellt wurde.

## <a name="how-can-a-direct-csp-configure-cloudyn-access-for-indirect-csp-customers-or-partners"></a>Wie kann ein direkter Cloudlösungsanbieter (CSP) den Cloudyn-Zugriff für Kunden oder Partner eines indirekten CSP konfigurieren?

Eine Anleitung hierzu finden Sie unter [Configure indirect CSP access in Cloudyn (Konfigurieren des Zugriffs für indirekte CSPs in Cloudyn)](quick-register-csp.md#configure-indirect-csp-access-in-cloudyn).
