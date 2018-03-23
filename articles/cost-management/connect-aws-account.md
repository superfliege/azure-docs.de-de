---
title: Herstellen einer Verbindung eines Amazon Web Services-Kontos mit Azure Cost Management| Microsoft-Dokumentation
description: Stellen Sie eine Verbindung eines Amazon Web Services-Kontos her, um Kosten und Nutzungsdaten in Cost Management-Berichten anzuzeigen.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 02/08/2018
ms.topic: article
ms.service: cost-management
manager: carmonm
ms.custom: ''
ms.openlocfilehash: 4a0280420132aad9f1e0b17d5998ec225bb0eaa1
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="connect-an-amazon-web-services-account"></a>Herstellen einer Verbindung mit einem Amazon Web Services-Konto

Sie haben zwei Optionen zum Herstellen einer Verbindung Ihres Amazon Web Services-Kontos (AWS) mit Azure Cost Management. Sie können eine Verbindung mit einer IAM-Rolle oder einem IAM-Benutzerkonto ohne Schreibzugriff herstellen. Die IAM-Rolle wird empfohlen, da sie Ihnen ermöglicht, den Zugriff mit definierten Berechtigungen an vertrauenswürdige Personen zu delegieren. Die IAM-Rolle setzt nicht voraus, dass Sie langfristige Zugriffsschlüssel freigeben. Nachdem Sie eine Verbindung Ihres AWS-Kontos mit Cost Management hergestellt haben, sind Kosten und Verwendungsdaten in Cost Management-Berichten verfügbar. Dieses Dokument führt Sie durch beide Optionen.

Weitere Informationen zu AWS-IAM-Identitäten finden Sie unter [Identities (Users, Groups, and Roles) (Identitäten [Benutzer, Gruppen und Rollen])](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html).

## <a name="aws-role-based-access"></a>Rollenbasierter AWS-Zugriff

Die folgenden Abschnitte führen Sie durch das Erstellen einer IAM-Rolle ohne Schreibzugriff, um Zugriff auf Cost Management zu ermöglichen.

### <a name="get-your-cost-management-account-external-id"></a>Abrufen Ihrer externen Cost Management-Konto-ID

Im ersten Schritt rufen Sie die eindeutige Verbindungspassphrase im Azure Cost Management-Portal ab. Sie dient in AWS als **External ID (Externe ID)**.

1. Öffnen Sie das Cloudyn-Portal über das Azure-Portal, oder navigieren Sie zu [https://azure.cloudyn.com](https://azure.cloudyn.com), und melden Sie sich dort an.
2. Klicken Sie auf **Einstellungen** (Zahnradsymbol), und wählen Sie dann **Cloud Accounts (Cloudkonten)**.
3. Wählen Sie unter „Accounts Management (Kontenverwaltung)“ die Registerkarte **AWS Accounts (AWS-Konten)** aus, und klicken Sie dann auf **Add new + (Neu hinzufügen)**.
4. Kopieren Sie im Dialogfeld **Add AWS Account (AWS-Konto hinzufügen)** die **External ID (Externe ID)**, und speichern Sie diesen Wert für die Schritte zum Erstellen der AWS-Rolle im nächsten Abschnitt. In der folgenden Abbildung ist die Beispiel-ID _Cloudyn_. Ihre ID lautet anders.  
    ![Externe ID](./media/connect-aws-account/external-id.png)

### <a name="add-aws-read-only-role-based-access"></a>Hinzufügen des rollenbasierten schreibgeschützten AWS-Zugriffs

1. Melden Sie sich bei der AWS-Konsole unter https://console.aws.amazon.com/iam/home, und wählen Sie **Roles (Rollen)**.
2. Klicken Sie auf **Create Role (Rolle erstellen)**, und wählen Sie dann **Another AWS account (Anderes AWS-Konto)**.
3. Fügen Sie die Identität `432263259397` in das Feld **Account ID (Konto-ID)** ein. Diese Konto-ID ist das Cost Management-Datensammlerkonto, das Sie verwenden müssen.
4. Aktivieren Sie neben **Options (Optionen)** das Kontrollkästchen **Require external ID (Externe ID erforderlich)**, fügen Sie den zuvor in das Feld **External ID (Externe ID)** kopierten Wert ein, und klicken Sie dann auf **Next: Permissions (Weiter: Berechtigungen)**.  
    ![Rolle erstellen](./media/connect-aws-account/create-role01.png)
5. Geben Sie unter **Attach permissions policies (Richtlinien für Berechtigungen anfügen)** im Filtersuchfeld **Policy type (Richtlinientyp)** `ReadOnlyAccess` ein, wählen Sie **ReadOnlyAccess** aus, und klicken Sie dann auf **Next: Review (Weiter: Überprüfen)**.  
    ![Schreibgeschützter Zugriff](./media/connect-aws-account/readonlyaccess.png)
6. Stellen Sie auf der Seite „Review“ (Überprüfen) sicher, dass Ihre Auswahl richtig ist, und geben Sie einen **Role name (Rollennamen)** ein. Beispiel: *Azure-Cost-Mgt*. Geben Sie eine **Role description (Rollenbeschreibung)** ein. Beispiel: _Rollenzuweisung für Azure Cost Management_, und klicken Sie dann auf **Create role (Rolle erstellen)**.
7. Klicken Sie in der Liste **Roles (Rollen)** auf die Rolle, die Sie erstellt haben, und kopieren Sie den Wert von **Role ARN (Rollen-ARN)** aus der Seite „Summary“ (Zusammenfassung). Verwenden Sie später den Wert von „Role ARN“ (Rollen-ARN), wenn Sie Ihre Konfiguration in Azure Cost Management registrieren.  
    ![Rollen-ARN](./media/connect-aws-account/role-arn.png)

### <a name="configure-aws-iam-role-access-in-cost-management"></a>Konfigurieren des AWS-IAM-Rollenzugriffs in Cost Management

1. Öffnen Sie das Cloudyn-Portal über das Azure-Portal, oder navigieren Sie zu https://azure.cloudyn.com, und melden Sie sich dort an.
2. Klicken Sie auf **Einstellungen** (Zahnradsymbol), und wählen Sie dann **Cloud Accounts (Cloudkonten)**.
3. Wählen Sie unter „Accounts Management (Kontenverwaltung)“ die Registerkarte **AWS Accounts (AWS-Konten)** aus, und klicken Sie dann auf **Add new + (Neu hinzufügen)**.
4. Geben Sie in **Account Name (Kontoname)** einen Namen für das Konto ein.
5. Wählen Sie neben **Access Type (Zugriffstyp)** die Option **IAM Role (IAM-Rolle)**.
6. Fügen Sie im Feld **Role ARN (Rollen-ARN)** den Wert ein, den Sie zuvor kopiert haben, und klicken Sie dann auf **Speichern**.  
    ![AWS-Kontostatus](./media/connect-aws-account/aws-account-status01.png)

Ihr AWS-Konto wird in der Liste der Konten angezeigt. Die aufgelistete **Owner ID (Besitzer-ID)** entspricht Ihrem Wert für „Role ARN“ (Rollen-ARN). Ihr **Account Status (Kontostatus)** müsste mit einem grünen Häkchensymbol versehen sein.

Cost Management startet das Sammeln der Daten und Auffüllen von Berichten. Einige Optimierungsberichte erfordern jedoch möglicherweise Daten mehrerer Tage, bevor genaue Empfehlungen generiert werden.

## <a name="aws-user-based-access"></a>Zugriff auf AWS-Benutzerbasis

Die folgenden Abschnitte führen Sie durch das Erstellen eines Benutzers ohne Schreibzugriff, um Zugriff auf Cost Management zu ermöglichen.

### <a name="add-aws-read-only-user-based-access"></a>Hinzufügen des benutzerbasierten schreibgeschützten AWS-Zugriffs

1. Melden Sie sich bei der AWS-Konsole unter https://console.aws.amazon.com/iam/home, und wählen Sie **Users (Benutzer)**.
2. Klicken Sie auf **Benutzer hinzufügen**.
3. Geben Sie in das Feld **User name (Benutzername)** einen Benutzernamen ein.
4. Aktivieren Sie für **Access type (Zugriffstyp)** das Kontrollkästchen **Programmatic access (Programmgesteuerter Zugriff)**, und klicken Sie auf **Next: Permissions (Weiter: Berechtigungen)**.  
    ![Benutzer hinzufügen](./media/connect-aws-account/add-user01.png)
5. Wählen Sie für Berechtigungen **Attach existing policies directly** (Vorhandene Richtlinien direkt anfügen).
6. Geben Sie unter **Attach permissions policies (Richtlinien für Berechtigungen anfügen)** im Filtersuchfeld **Policy type (Richtlinientyp)** `ReadOnlyAccess` ein, wählen Sie **ReadOnlyAccess** aus, und klicken Sie dann auf **Next: Review (Weiter: Überprüfen)**.  
    ![Berechtigungen für Benutzer festlegen](./media/connect-aws-account/set-permission-for-user.png)
7. Stellen Sie auf der Seite „Review“ (Überprüfen) sicher, dass Ihre Auswahl richtig ist, und klicken Sie dann auf **Create user (Benutzer erstellen)**.
8. Auf der Seite „Complete“ (Fertig stellen) werden Ihre Zugriffsschlüssel-ID und der geheime Zugriffsschlüssel angezeigt. Sie können diese Informationen zum Konfigurieren der Registrierung in Cost Management verwenden.
9. Klicken Sie auf **Download .csv (CSV-Datei herunterladen)**, und speichern Sie die credentials.csv-Datei an einem sicheren Ort.  
    ![Anmeldeinformationen herunterladen](./media/connect-aws-account/download-csv.png)


### <a name="configure-aws-iam-user-based-access-in-cost-management"></a>Konfigurieren des Zugriffs auf AWS-IAM-Benutzerbasis in Cost Management

1. Öffnen Sie das Cloudyn-Portal über das Azure-Portal, oder navigieren Sie zu https://azure.cloudyn.com, und melden Sie sich dort an.
2. Klicken Sie auf **Einstellungen** (Zahnradsymbol), und wählen Sie dann **Cloud Accounts (Cloudkonten)**.
3. Wählen Sie unter „Accounts Management (Kontenverwaltung)“ die Registerkarte **AWS Accounts (AWS-Konten)** aus, und klicken Sie dann auf **Add new + (Neu hinzufügen)**.
4. Geben Sie für **Account Name (Kontoname)** einen Kontonamen ein.
5. Wählen Sie neben **Access Type (Zugriffstyp)** die Option **IAM User (IAM-Benutzer)**.
6. Fügen Sie in **Access Key (Zugriffsschlüssel)** den Wert **Access key ID (Zugriffsschlüssel-ID)** aus der Datei „credentials.csv“ ein.
7. Fügen Sie in **Secret Key (Geheimschlüssel)** den Wert **Secret access key (Geheimer Zugriffsschlüssel)** aus der Datei „credentials.csv“ ein, und klicken Sie dann auf **Save (Speichern)**.  
    ![AWS-Benutzerkontostatus](./media/connect-aws-account/aws-user-account-status.png)

Ihr AWS-Konto wird in der Liste der Konten angezeigt. Ihr **Account Status (Kontostatus)** müsste mit einem grünen Häkchensymbol versehen sein.

Cost Management startet das Sammeln der Daten und Auffüllen von Berichten. Einige Optimierungsberichte erfordern jedoch möglicherweise Daten mehrerer Tage, bevor genaue Empfehlungen generiert werden.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Azure Cost Management finden Sie im Tutorial [Überprüfen der Nutzung und der Kosten](tutorial-review-usage.md).
