---
title: Einrichten und Konfigurieren der Integration von AWS-Kosten- und Nutzungsberichten in Azure Cost Management
description: Dieser Artikel erläutert das Einrichten und Konfigurieren der Integration von AWS-Kosten- und Nutzungsberichten in Azure Cost Management.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 06/06/2019
ms.topic: conceptual
ms.service: cost-management
manager: ormaoz
ms.custom: ''
ms.openlocfilehash: e87e95ec9e4e20ee4785c2b1f448a7ca5f442b8a
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2019
ms.locfileid: "65409191"
---
# <a name="set-up-and-configure-aws-cost-and-usage-report-integration"></a>Einrichten und Konfigurieren der Integration von AWS-Kosten- und Nutzungsberichten

Mit der Integration der Kosten- und Nutzungsberichte von Amazon Web Services können Sie Ihre AWS-Ausgaben in Azure Cost Management überwachen und steuern. Die Integration bietet Ihnen die Möglichkeit, Ausgaben für Azure und AWS zentral im Azure-Portal zu überwachen und zu steuern. In diesem Artikel wird erläutert, wie Sie die Integration einrichten und so konfigurieren, dass Sie die Cost Management-Funktionen zur Kostenanalyse und Budgetüberprüfung nutzen können.

Cost Management verarbeitet den in einem S3-Bucket gespeicherten AWS-Kosten- und Nutzungsbericht, indem es Ihre AWS-Anmeldeinformationen verwendet, um Berichtsdefinitionen abzurufen und GZIP-CSV-Berichtsdateien herunterzuladen.

## <a name="create-a-cost-and-usage-report-in-aws"></a>Erstellen eines Kosten- und Nutzungsberichts in AWS

AWS empfiehlt zur Erfassung und Verarbeitung von AWS-Kosten die Verwendung eines Kosten- und Nutzungsberichts. Weitere Informationen finden Sie im Dokumentationsartikel zum [AWS-Kosten und Verwendungsbericht](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/billing-reports-costusage.html).

Verwenden Sie die Seite **Berichte** der Billing and Cost Management Console in AWS, um mit den folgenden Schritten einen Kosten- und Nutzungsbericht zu erstellen.

1. Melden Sie sich bei der AWS Management Console an, und öffnen Sie die Billing and Cost Management Console unter https://console.aws.amazon.com/billing.
2. Klicken Sie im Navigationsbereich auf **Berichte**.
3. Klicken Sie auf **Bericht erstellen**.
4. Für **Berichtsnamen** geben Sie einen Namen für den Bericht ein.
5. Für **Zeiteinheit** wählen Sie **Stündlich** aus.
6. Fügen Sie für **Einschließen** die IDs jeder Ressource in den Bericht hinzu, und wählen Sie **Ressourcen-IDs**.
7. Für **Support aktivieren für** ist keine Auswahl erforderlich.
8. Wählen Sie für **Datenaktualisierungseinstellungen** **Kosten- &amp; Nutzungsbericht automatisch aktualisieren, wenn Gebühren für die letzten Monate mit geschlossenen Rechnungen erkannt werden** aus.
9. Klicken Sie auf **Weiter**.
10. Geben Sie für **Amazon S3-Bucket** den Namen des Amazon S3-Buckets ein, in den die Berichte gesendet werden sollen, und klicken Sie auf **Überprüfen**. Der Bucket muss über entsprechende Berechtigungen verfügen, um gültig zu sein. Weitere Informationen zum Hinzufügen von Berechtigungen zum Bucket finden Sie unter [Einrichten von Bucket- und Objektzugriffsberechtigungen](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/set-permissions.html).
11. Geben Sie unter **Berichtpfadpräfix** das Berichtspfadpräfix ein, das dem Namen Ihres Berichts hinzugefügt werden soll.
12. Für **Komprimierung** wählen Sie die Option **GZIP** aus.
13. Klicken Sie auf **Weiter**.
14. Nachdem Sie die Einstellungen für Ihren Bericht überprüft haben, klicken Sie auf **Überprüfen und Fertig stellen**.
    Notieren Sie sich den **Berichtsnamen**. Sie werden ihn später benötigen.

Es kann bis zu 24 Stunden dauern, bis AWS mit der Übermittlung von Berichten an Ihren Amazon S3-Bucket beginnt. Nach dem Start der Übermittlung aktualisiert AWS mindestens einmal täglich die Dateien für den AWS-Kosten- und Nutzungsbericht. Sie können Ihre AWS-Umgebung weiter konfigurieren, ohne auf den Start der Übermittlung zu warten.

## <a name="create-a-role-and-policy-in-aws"></a>Erstellen einer Rolle und Richtlinie in AWS

Azure Cost Management greift mehrmals täglich auf den S3-Bucket zu, in dem sich der Kosten- und Nutzungsbericht befindet. Cost Management benötigt Zugriff auf Anmeldeinformationen, um neue Daten zu überprüfen. Sie erstellen eine Rolle und die Richtlinie in AWS, um den Zugriff auf Cost Management zu erlauben.

Um den rollenbasierten Zugriff auf ein AWS-Konto im Azure Cost Management zu ermöglichen, wird die Rolle in der AWS-Konsole angelegt. Sie benötigen die _Rollen-ARN_ und die _Externe ID_ der AWS-Konsole. Diese Angaben benötigen Sie später auf der Seite „AWS-Connector erstellen“ in Azure Cost Management.

Verwenden Sie den Assistenten „Eine neue Rolle erstellen“:

1. Melden Sie sich bei Ihrer AWS-Konsole an, und wählen Sie **Dienste** aus.
2. Wählen Sie in der Liste der Dienste **IAM** aus.
3. Klicken Sie auf **Rollen** und dann auf **Rolle erstellen**.
4. Wählen Sie auf der nächsten Seite **Ein anderes AWS-Konto** aus.
5. Geben Sie als **Konto-ID** _432263259397_ ein.
6. Wählen Sie unter **Optionen** **Externe ID erfordern (Best Practice ist, wenn ein Dritter diese Rolle übernimmt)**.
7. Geben Sie in **Externe ID** die externe ID ein. Die externe ID ist ein gemeinsames Kennwort für die AWS-Rolle und Azure Cost Management. Die gleiche externe ID wird auch auf der Seite „Neuer Connector“ im Cost Management verwendet. Eine externe ID wäre beispielsweise _Companyname1234567890123_.
    Ändern Sie nicht die Auswahl für **MFA erfordern**. Dieses Feld sollte leer bleiben.
8. Klicken Sie auf **Weiter: Permissions** (Weiter: Berechtigungen).
9. Klicken Sie auf **Create policy**. Es wird eine neue Browserregisterkarte geöffnet, in der Sie eine neue Richtlinie erstellen.
10. Klicken Sie auf **Skript auswählen**.

Konfigurieren Sie die Berechtigung für den Kosten- und Nutzungsbericht:

1. Geben Sie **Kosten- und Nutzungsbericht** ein.
2. Wählen Sie **Zugriffsebene**, **Lesen** > **DescribeReportDefinitions** aus. Auf diese Weise kann das Cost Management lesen, welche CUR-Berichte definiert sind und kann feststellen, ob sie der Voraussetzung für die Berichtsdefinition entsprechen.
3. Klicken Sie auf **Zusätzliche Berechtigungen hinzufügen**.

Konfigurieren Sie den S3-Bucket und die Objektberechtigung:

1. Klicken Sie auf **Skript auswählen**.
2. Geben Sie _S3_ ein.
3. Wählen Sie **Zugriffsebene**, **Liste** > **ListBucket** aus. Diese Aktion ruft die Liste der Objekte im S3-Bucket ab.
4. Wählen Sie **Zugriffsebene**, **Lesen** > **GetObject** aus. Mit dieser Aktion können Sie Abrechnungsdateien herunterladen.
5. Wählen Sie **Ressourcen** aus.
6. Wählen Sie **Bucket – ARN hinzufügen** aus.
7. Geben Sie unter **Bucketname** den Bucket ein, der zum Speichern der CUR-Dateien verwendet wird.
8. Wählen Sie **Objekt – ARN hinzufügen** aus.
9. Geben Sie unter **Bucketname** den Bucket ein, der zum Speichern der CUR-Dateien verwendet wird.
10. Wähle Sie unter **Objektname** die Option **Alle**.
11. Klicken Sie auf **Zusätzliche Berechtigungen hinzufügen**.

Konfigurieren Sie die Cost Explorer-Berechtigung:

1. Klicken Sie auf **Skript auswählen**.
2. Geben Sie _Cost Explorer Service_ ein.
3. Wählen Sie **Alle Cost Explorer Service-Aktionen (ce:\*) aus**. Diese Aktion überprüft, ob die Sammlung richtig ist.
4. Klicken Sie auf **Zusätzliche Berechtigungen hinzufügen**.

Berechtigung für die Organisation hinzufügen:

1. Geben Sie **Organisationen** ein.
2. Wählen Sie **Zugriffsebene, Liste** > **ListAccounts** aus. Diese Aktion ruft die Namen der Konten ab.
3. Geben Sie unter **Überprüfungsrichtlinie** einen Namen für die neue Richtlinie ein. Überprüfen Sie, ob Sie die richtigen Informationen eingegeben haben, und klicken Sie dann auf **Richtlinie erstellen**.
4. Gehen Sie zur vorherigen Registerkarte zurück, und aktualisieren Sie die Webseite Ihres Browsers. Suchen Sie über die Suchleiste nach der neuen Richtlinie.
5. Wählen Sie **Weiter: Überprüfung**.
6. Geben Sie einen Namen für die neue Rolle ein. Überprüfen Sie, ob Sie die richtigen Informationen eingegeben haben, und klicken Sie dann auf **Rolle erstellen**.
    Notieren Sie sich die **Rollen-ARN** und die **Externe ID**, die in den vorherigen Schritten beim Erstellen der Rolle verwendet wurden. Sie werden diese später verwenden, wenn Sie den Azure Cost Management-Connector einrichten.

Der JSON-Code der Richtlinie sollte wie im folgenden Beispiel aussehen: Ersetzen Sie _bucketname_ durch den Namen Ihres S3-Buckets.

```JSON
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
"organizations:ListAccounts",
            "ce:*",
            "cur:DescribeReportDefinitions"
            ],
            "Resource": "*"
        },
        {
            "Sid": "VisualEditor1",
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::bucketname",
                "arn:aws:s3:::bucketname/*"
            ]
        }
    ]
}
```

## <a name="set-up-a-new-aws-connector-in-azure"></a>Einrichten eines neuen AWS-Connectors in Azure

Verwenden Sie die folgenden Informationen, um einen neuen AWS-Connector zu erstellen und mit der Überwachung Ihrer AWS-Kosten zu beginnen.

1. Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.
2. Navigieren Sie zu **Cost Management + Abrechnung** > **Cost Management**.
3. Klicken Sie unter **Einstellungen** auf **Cloudconnectors (Vorschau)**.  
    ![Beispiel für die Cloudconnectors (Vorschaueinstellung)](./media/aws-integration-setup-configure/cloud-connectors-preview01.png).
4. Klicken Sie auf **+ Hinzufügen** oben auf der Seite, um einen neuen Connector zu erstellen.
5. Geben Sie auf der Seite „AWS-Connector erstellen“ einen **Anzeigenamen** ein, um Ihren Connector zu benennen.  
    ![Beispiel für eine Seite „AWS-Connector erstellen“](./media/aws-integration-setup-configure/create-aws-connector01.png)
6. Wählen Sie optional die Standardverwaltungsgruppe aus. Darin werden alle erkannten verknüpftem Konten gespeichert. Sie können sie später einrichten.
7. Wählen Sie im Abschnitt **Abrechnung** die Option **1 % bei allgemeiner Verfügbarkeit automatisch abrechnen**, wenn Sie den kontinuierlichen Betrieb nach Ablauf der Vorschau sicherstellen möchten. Wenn Sie die automatische Option auswählen, müssen Sie eine **Abrechnungsabonnement** auswählen.
8. Geben Sie die **Rollen-ARN** ein. Dies ist der Wert, den Sie beim Einrichten der Rolle in AWS verwendet haben.
9. Geben sie die **Externe ID** ein. Dies ist der Wert, den Sie beim Einrichten der Rolle in AWS verwendet haben.
10. Geben Sie den **Berichtsnamen** ein, den Sie in AWS erstellt haben.
11. Klicken Sie auf **Weiter** und anschließend auf **Erstellen**.

Es kann einige Stunden dauern, bis die neuen AWS-Bereiche, das konsolidierte AWS-Konto und die verknüpften AWS-Konten sowie deren Kostendaten angezeigt werden.

Nachdem Sie den Connector erstellt haben, empfehlen wir Ihnen, dem Connector eine Zugriffssteuerung zuzuweisen. Benutzern werden Berechtigungen für die neu erkannten Bereiche zugewiesen: Konsolidiertes AWS-Konto und verknüpfte AWS Konten. Der Benutzer, der den Connector erstellt, ist der Besitzer des Connectors, des konsolidierten Kontos und aller verknüpften Konten.

Wenn den Benutzern nach dem Erkennen Connectorberechtigungen zugewiesen werden, werden den vorhandenen AWS-Bereichen keine Berechtigungen zugewiesen. Stattdessen werden nur neu verknüpften Konten Berechtigungen zugewiesen.

## <a name="additional-steps"></a>Zusätzliche Schritte

- [Richten Sie Verwaltungsgruppen ein](../governance/management-groups/index.md#initial-setup-of-management-groups), sofern das noch nicht geschehen ist.
- Stellen Sie sicher, dass Ihrer Bereichsauswahl neue Bereiche hinzugefügt wurden. Vergessen Sie nicht, auf **Aktualisieren** zu klicken, um die neuesten Daten anzuzeigen.
- Wählen Sie auf der Seite „Cloudconnector“ Ihren Connector aus, und klicken Sie auf **Zu Abrechnungskonto wechseln**, um das verknüpfte Konto Verwaltungsgruppen zuzuweisen.

## <a name="manage-cloud-connectors"></a>Verwalten von Cloudconnectors

Wenn Sie auf der Seite „Cloudconnector“ einen Connector auswählen, haben Sie dann folgende Möglichkeiten:

- Klicken Sie auf **Zum Abrechnungskonto wechseln**, um konsolidierte AWS-Kontoinformationen anzuzeigen.
- Klicken Sie auf **Zugriffssteuerung**, um die Rollenzuweisung für den Connector zu verwalten.
- Klicken Sie auf **Bearbeiten**, um den Connector zu aktualisieren. Sie können die AWS-Kontonummer nicht ändern, wie sie in der Rollen-ARN angezeigt wird. Sie können jedoch einen neuen Connector erstellen.
- Klicken Sie auf **Überprüfen**, um den Überprüfungstest erneut auszuführen, um sicherzustellen, dass Cost Management Daten mithilfe der Connectoreinstellungen erfassen kann.

![Beispiel für die Liste der erstellten AWS-Connectors](./media/aws-integration-setup-configure/list-aws-connectors.png)

## <a name="role-of-azure-management-groups"></a>Rolle der Azure-Verwaltungsgruppen

Um einen zentralen Ort für die Anzeige von Cloudanbieterinformationen zu erstellen, müssen Sie Ihre Azure-Abonnements und verknüpften AWS-Konten in derselben Verwaltungsgruppe einordnen. Wenn Sie Ihre Azure-Umgebung nicht bereits mit Verwaltungsgruppen konfiguriert haben, finden Sie unter [Erstmalige Einrichtung von Verwaltungsgruppen](../governance/management-groups/index.md#initial-setup-of-management-groups) weitere Informationen.

Wenn Sie die Kosten trennen möchten, können Sie eine Verwaltungsgruppe anlegen, die nur die mit verknüpften AWS-Konten enthält.

## <a name="aws-consolidated-account"></a>Konsolidiertes AWS­-Konto

Das konsolidierte AWS-Konto wird verwendet, Abrechnungen und Zahlungen für mehrere AWS-Konten zu konsolidieren. Ihr konsolidiertes AWS-Konto fungiert außerdem als verknüpftes AWS-Konto.

![Beispiel für Details zum konsolidierten AWS-Konto](./media/aws-integration-setup-configure/aws-consolidated-account01.png)

Auf der Seite haben Sie folgende Möglichkeiten:

- Klicken Sie auf **Aktualisieren**, um eine Massenaktualisierung für die Zuordnung der verknüpften AWS-Konten zu einer Verwaltungsgruppe durchzuführen.
- Klicken Sie auf **Zugriffssteuerung**, um die Rollenzuweisung für den Bereich einzurichten.

### <a name="aws-consolidated-account-permissions"></a>Berechtigungen für das konsolidierte AWS­-Konto

Standardmäßig werden die Berechtigungen für das konsolidierte AWS-Konto beim Erstellen festgelegt, basierend auf den Berechtigungen des AWS-Connectors. Der Ersteller des Connectors ist der Besitzer.

Sie verwalten die Zugriffsebene, indem Sie die Seite Zugriffsebene“ des konsolidierten AWS-Kontos verwenden. Berechtigungen für das konsolidierte AWS-Konto werden jedoch nicht an verknüpfte AWS-Konten vererbt.

## <a name="aws-linked-account"></a>Verknüpftes AWS-Konto

Im verknüpften AWS-Konto werden AWS-Ressourcen erstellt und verwaltet. Ein verknüpftes Konto fungiert auch als Sicherheitsgrenze.

Auf der Seite haben Sie folgende Möglichkeiten:

- Klicken Sie auf **Aktualisieren**, um eine Aktualisierung für die Zuordnung des verknüpften AWS-Kontos zu einer Verwaltungsgruppe durchzuführen.
- Klicken Sie auf **Zugriffssteuerung**, um eine Rollenzuweisung für den Bereich einzurichten.

![Beispiel für die Seite „Verknüpftes AWS-Konto“](./media/aws-integration-setup-configure/aws-linked-account01.png)

### <a name="aws-linked-account-permissions"></a>Berechtigungen für ein verknüpftes AWS-Konto

Standardmäßig werden die Berechtigungen für das verknüpfte AWS-Konto beim Erstellen festgelegt, basierend auf den Berechtigungen des AWS-Connectors. Der Ersteller des Connectors ist der Besitzer. Sie verwalten die Zugriffsebene, indem Sie die Seite Zugriffsebene“ des verknüpften AWS-Kontos verwenden. Berechtigungen für das konsolidierte AWS-Konto werden nicht an verknüpfte AWS-Konten vererbt.

Verknüpfte AWS-Konten erben immer Berechtigungen von der Verwaltungsgruppe, zu der sie gehören.

## <a name="next-steps"></a>Nächste Schritte

- Nachdem Sie nun die Integration des AWS-Kosten- und Nutzungsberichts eingerichtet und konfiguriert haben, fahren Sie mit [Verwalten von AWS-Kosten und -Nutzung](aws-integration-manage.md) fort.
- Wenn Sie mit der Kostenanalyse nicht vertraut sind, lesen Sie den Schnellstart [Ermitteln und Analysieren von Kosten mit der Kostenanalyse](quick-acm-cost-analysis.md).
- Wenn Sie mit Budgets in Azure nicht vertraut sind, lesen Sie das Tutorial [Erstellen und Verwalten von Azure-Budgets](tutorial-acm-create-budgets.md).
