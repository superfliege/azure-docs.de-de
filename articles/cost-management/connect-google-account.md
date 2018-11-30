---
title: Herstellen einer Verbindung eines Google Cloud Platform-Kontos mit Cloudyn in Azure| Microsoft-Dokumentation
description: Stellen Sie eine Verbindung eines Google Cloud Platform-Kontos her, um Kosten und Nutzungsdaten in Cloudyn-Berichten anzuzeigen.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 10/05/2018
ms.topic: conceptual
ms.service: cost-management
manager: benshy
ms.custom: ''
ms.openlocfilehash: 1877acbd39f4e312e3a567e092bb0bcf7531b96b
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/21/2018
ms.locfileid: "52276328"
---
# <a name="connect-a-google-cloud-platform-account"></a>Herstellen einer Verbindung eines Google Cloud Platform-Kontos

Sie können eine Verbindung Ihres bestehenden Google Cloud Platform-Kontos mit Cloudyn herstellen. Nachdem Sie eine Verbindung Ihres Kontos mit Cloudyn hergestellt haben, sind Kosten und Verwendungsdaten in Cloudyn-Berichten verfügbar. Dieser Artikel unterstützt Sie beim Herstellen einer Verbindung Ihres Google-Kontos mit Cloudyn und beim Konfigurieren.

> [!NOTE]
> Google hat die Kontosicherheit geändert, was verhindert, dass neue Verbindungen zwischen Cloudyn und Google hergestellt werden. Cloudyn sammelt weiterhin Google-Daten für Benutzer, die Cloudyn bereits mit Google verbunden haben. Allerdings können Cloudyn zurzeit keine neuen Google-Konten hinzufügen. Das Cloudyn-Team weiß nicht, wann die Unterstützung für das Hinzufügen neuer Google-Konten zu Cloudyn fortgesetzt wird. Wir werden diesen Hinweis entfernen, wenn die Unterstützung fortgesetzt wird.

## <a name="collect-project-information"></a>Sammeln von Projektinformationen

Sie beginnen mit dem Sammeln von Informationen zu Ihrem Projekt.

1. Melden Sie sich bei der Google Cloud Platform-Konsole unter [https://console.cloud.google.com](https://console.cloud.google.com) an.
2. Überprüfen Sie die Projektinformationen, die Sie Cloudyn übergeben möchten, und notieren Sie **Projektname** und **Projekt-ID**. Halten Sie die Informationen für spätere Schritte bereit.  
    ![Google Cloud Platform-Konsole](./media/connect-google-account/gcp-console01.png)
3. Wenn die Abrechnung nicht aktiviert und mit Ihrem Projekt verknüpft ist, erstellen Sie ein Abrechnungskonto. Weitere Informationen finden Sie unter [Neues Rechnungskonto erstellen](https://cloud.google.com/billing/docs/how-to/manage-billing-account#create\_a\_new\_billing\_account).

## <a name="enable-storage-bucket-billing-export"></a>Aktivieren des Exportierens der Speicherbucketabrechnung

Cloudyn ruft Ihre Google-Abrechnungsdaten aus einem Speicherbucket ab. Halten Sie die Daten für **Bucketname** und **Berichtpräfix** für die spätere Verwendung während der Cloudyn-Registrierung bereit.

Wenn Sie Google Cloud Storage zum Speichern von Verwendungsberichten nutzen, fallen minimale Gebühren an. Weitere Informationen finden Sie unter [Google Cloud Storage – Preise](https://cloud.google.com/storage/pricing).

1. Wenn Sie das Exportieren der Abrechnung in eine Datei nicht aktiviert haben, befolgen Sie die Anweisungen unter [Abrechnungsdaten in Datei exportieren](https://cloud.google.com/billing/docs/how-to/export-data-file#how_to_enable_billing_export_to_a_file). Sie können entweder das JSON- oder CSV-Abrechnungsexportformat verwenden.
2. Wechseln Sie andernfalls in der Google Cloud Platform-Konsole zu **Billing (Abrechnung)** > **Billing export (Abrechnungsexport)**. Notieren Sie **Bucket name (Bucket name)** und **Report prefix (Berichtpräfix)** für Ihre Abrechnung.  
    ![Billing export (Abrechnungsexport)](./media/connect-google-account/billing-export.png)

## <a name="enable-google-cloud-platform-apis"></a>Aktivieren der Google Cloud Platform-APIs

Zum Sammeln von Nutzungs- und Ressourceninformationen setzt Cloudyn voraus, dass die folgenden Google Cloud Platform-APIs aktiviert sind:

- BigQuery API
- Google Cloud SQL
- Google Cloud Datastore API
- Google Cloud Storage
- Google Cloud Storage JSON API
- Google Compute Engine API

### <a name="enable-or-verify-apis"></a>Aktivieren oder Überprüfen von APIs

1. Wählen Sie in der Google Cloud Platform-Konsole das Projekt aus, das Sie bei Cloudyn registrieren möchten.
2. Navigieren Sie zu **APIs & Services (APIs und Dienste)** > **Library (Bibliothek)**.
3. Nutzen Sie die Suche, um die oben aufgelisteten APIs zu finden.
4. Überprüfen Sie, ob für jede API **API enabled (API aktiviert)** angezeigt wird. Klicken Sie andernfalls auf **ENABLE (AKTIVIEREN)**.

## <a name="add-a-google-cloud-account-to-cloudyn"></a>Hinzufügen eines Google Cloud-Kontos zu Cloudyn

1. Öffnen Sie im Azure-Portal das Cloudyn-Portal, oder navigieren Sie zu [https://azure.cloudyn.com](https://azure.cloudyn.com/), und melden Sie sich an.
2. Klicken Sie auf **Einstellungen** (Zahnrad-Symbol), und wählen Sie dann **Cloud Accounts (Cloudkonten)**.
3. Wählen Sie unter **Accounts Management (Kontenverwaltung)** die Registerkarte **Google Accounts (Google-Konten)** aus, und klicken Sie dann auf **Add new + (Neu hinzufügen)**.
4. Geben Sie in **Google Account Name (Google-Kontoname)** die E-Mail-Adresse für das Abrechnungskonto ein, und klicken Sie auf **Next (Weiter)**.
5. Wählen Sie im Google-Authentifizierungsdialogfeld ein Google-Konto aus, bzw. geben Sie es ein, und **ALLOW (ERLAUBEN)** Sie „cloudyn.com“ dann den Zugriff auf Ihr Konto.
6. Fügen Sie die Anforderungsprojektinformationen hinzu, die Sie vorher notiert haben. Dazu zählen **Projekt-ID**, **Projektname**, **Name des Abrechnungsbuckets** und Berichtpräfix der **Abrechnungsdatei**. Klicken Sie dann auf **Save (Speichern)**.  
    ![Google-Projekt hinzufügen](./media/connect-google-account/add-project.png)

Ihr Google-Konto wird in der Liste der Benutzerkonten angezeigt, und es müsste **Authenticated (Authentifiziert)** angegeben werden. Darunter werden Ihr Google-Projektname und die Projekt-ID angezeigt, beide mit einem grünen Häkchensymbol versehen. Der „Account Status“ (Kontostatus) sollte **Completed (Abgeschlossen)** lauten.

Innerhalb von wenigen Stunden zeigen Cloudyn-Berichte Kosten- und Nutzungsinformationen von Google an.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Cloudyn finden Sie im Tutorial [Überprüfen der Nutzung und der Kosten](./tutorial-review-usage.md) für Cloudyn.
