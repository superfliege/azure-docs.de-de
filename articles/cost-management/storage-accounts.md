---
title: Konfigurieren von Speicherkonten für Azure Cost Management | Microsoft-Dokumentation
description: In diesem Artikel ist beschrieben, wie Sie Azure-Speicherkonten und AWS-Speicherbuckets für Azure Cost Management konfigurieren.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 06/07/2018
ms.topic: conceptual
ms.service: cost-management
manager: carmonm
ms.custom: ''
ms.openlocfilehash: dab7100c97fab7e086352916ec222ec70a0d0400
ms.sourcegitcommit: 974c478174f14f8e4361a1af6656e9362a30f515
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2018
ms.locfileid: "42142549"
---
# <a name="configure-storage-accounts-for-cost-management"></a>Konfigurieren von Speicherkonten für Cost Management

<!--- intent: As a Cost Management user, I want to configure Cost Management to use my cloud service provider storage account to store my reports. -->

Sie können Cost Management-Berichte im Cloudyn-Portal, in Azure-Speicher oder in AWS-Speicherbuckets speichern. Speichern Ihrer Berichte im Cloudyn-Portal ist kostenfrei. Dagegen ist das Speichern Ihrer Berichte im Speicher Ihres Cloud-Dienstanbieters optional und verursacht zusätzliche Kosten. Dieser Artikel hilft Ihnen bei der Konfiguration von Azure-Speicherkonten und AWS-Speicherbucktes (Amazon Web Services), um Ihre Berichte zu speichern.

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen ein Azure-Speicherkonto oder ein Amazon-Speicherbucket.

Wenn Sie kein Azure-Speicherkonto haben, müssen Sie eines erstellen. Weitere Informationen zum Erstellen eines Azure-Speicherkontos finden Sie unter [Erstellen eines Speicherkontos](../storage/common/storage-quickstart-create-account.md).

Wenn Sie keinen AWS Simple Storage Service Bucket (S3-Bucket) haben, müssen Sie einen erstellen. Weitere Informationen zum Erstellen eines S3-Buckets finden Sie unter [Bucket erstellen](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html).

## <a name="configure-your-azure-storage-account"></a>Konfigurieren Ihres Azure-Speicherkontos

Das Konfigurieren Ihres Azure-Speichers zur Verwendung durch Cost Management ist einfach. Besorgen Sie sich die Details zu dem Speicherkonto, und kopieren Sie diese in das Cloudyn-Portal.

1. Melden Sie sich unter http://portal.azure.com beim Azure-Portal an.
2. Klicken Sie auf **Alle Dienste**, wählen Sie **Speicherkonten** aus, scrollen Sie zu dem Speicherkonto, das Sie verwenden möchten, und wählen Sie es aus.
3. Klicken Sie auf Ihrer Speicherkontoseite unter **Einstellungen** auf **Zugriffsschlüssel**.
4. Kopieren Sie Ihren **Speicherkontonamen** und Ihre **Verbindungszeichenfolge** unter „Schlüssel1“.  
![Azure-Speicherzugriffsschlüssel](./media/storage-accounts/azure-storage-access-keys.png)  
5. Öffnen Sie im Azure-Portal das Cloudyn-Portal, oder navigieren Sie zu https://azure.cloudyn.com, und melden Sie sich an.
6. Klicken Sie auf das Zahnrad-Symbol, und wählen Sie dann **Reports Storage Management** aus.
7. Klicken Sie auf **Add new +**, und vergewissern Sie sich, dass Microsoft Azure ausgewählt ist. Fügen Sie den Namen Ihres Azure-Speicherkontos im Bereich **Name** ein. Fügen Sie Ihre **Verbindungszeichenfolge** im entsprechenden Bereich ein. Geben Sie einen Containernamen ein, und klicken Sie dann auf **Save**.  
![Cloudyn-Speicher, der für Azure konfiguriert ist](./media/storage-accounts/azure-cloudyn-storage.png)

  Ihr neuer Eintrag für Azure-Berichtsspeicher wird in der Speicherkontoliste angezeigt.  
    ![Neuer Azure-Berichtsspeicher in der Liste](./media/storage-accounts/azure-storage-entry.png)


Sie können jetzt Berichte im Azure-Speicher speichern. Klicken Sie in irgendeinem Bericht auf **Actions**, und wählen Sie dann **Schedule report** aus. Geben Sie dem Bericht einen Namen, und fügen Sie dann entweder Ihre eigene URL hinzu, oder verwenden Sie die automatisch erstellte URL. Wählen Sie **Save to storage** aus, und wählen Sie dann das Speicherkonto aus. Geben Sie ein Präfix ein, das an den Namen der Berichtdatei angefügt wird. Wählen Sie entweder CSV oder JSON als Dateiformat aus, und speichern Sie dann den Bericht.

## <a name="configure-an-aws-storage-bucket"></a>Konfigurieren eines AWS-Speicherbuckets

Cloudyn verwendet vorhandene AWS-Anmeldeinformationen: Benutzer oder Rolle, um die Berichte in Ihrem Bucket zu speichern. Um den Zugriff zu testen, versucht Cloudyn, eine kleine Textdatei namens _check-bucket-permission.txt_ im Bucket zu speichern.

Sie Stellen die Cloudyn-Rolle oder den Cloudyn-Benutzer bereit, die oder der die Berechtigung „PutObject“ für Ihr Bucket hat. Danach verwenden Sie ein vorhandenes oder ein neues Bucket, um Berichte zu speichern. Schließlich geben Sie an, wie die Speicherklasse verwaltet werden soll, legen Sie die Lebenszyklusregeln fest, oder entfernen Sie alle nicht benötigten Dateien.

###  <a name="assign-permissions-to-your-aws-user-or-role"></a>Zuweisen von Berechtigungen zu Ihrem AWS-Benutzer oder Ihrer AWS-Rolle

Wenn Sie eine neue Richtlinie erstellen, geben Sie die genauen Berechtigungen an, die dazu erforderlich sind, einen Bericht in einem S3-Buckets zu speichern.

1. Melden Sie sich bei der AWS-Konsole an, und wählen Sie **Services** aus.
2. Wählen Sie **IAM** in der Liste der Dienste aus.
3. Wählen Sie **Policies** auf der linken Seite der Konsole aus, und klicken Sie dann auf **Create Policy**.
4. Klicken Sie auf die Registerkarte **JSON**.
5. Die folgende Richtlinien ermöglicht es Ihnen, einen Bericht in einem S3-Bucket zu speichern. Kopieren Sie das folgende Richtlinienbeispiel auf die Registerkarte **JSON**. Ersetzen Sie &lt;bucketname&gt; durch den Namen Ihres Buckets.

  ```
{
    "Version": "2012-10-17",
    "Statement": [
      {
        "Sid":  "CloudynSaveReport2S3",
        "Effect":      "Allow",
        "Action": [
          "s3:PutObject"
        ],
        "Resource": [
          "arn:aws:s3:::<bucketname>/*"
        ]
      }
    ]
}
```

6. Klicken Sie auf **Review policy**.  
    ![Review policy](./media/storage-accounts/aws-policy.png)  
7. Geben Sie auf der Seite „Review policy“ einen Namen für Ihre Richtlinie ein. Zum Beispiel _CloudynSaveReport2S3_.
8. Klicken Sie auf **Create policy**.

### <a name="attach-the-policy-to-a-cloudyn-role-or-user-in-your-account"></a>Anfügen der Richtlinie an eine Cloudyn-Rolle oder einen Cloudyn-Benutzer in Ihrem Konto

Um die neue Richtlinie anzufügen, öffnen Sie die AWS-Konsole, und bearbeiten Sie die Cloudyn-Rolle oder den Cloudyn-Benutzer.

1. Melden Sie sich bei der AWS-Konsole an, wählen Sie **Services** aus, und wählen Sie dann **IAM** in der Liste der Dienste aus.
2. Wählen Sie entweder **Roles** oder **Users** auf der linken Seite der Konsole aus.

**Für Rollen:**

  1. Klicken Sie auf Ihren Cloudyn-Rollennamen.
  2. Klicken Sie auf der Registerkarte **Permissions** auf **Attach Policy**.
  3. Suchen Sie nach der von Ihnen erstellten Richtlinie, wählen Sie diese aus, und klicken Sie auf **Attach Policy**.
    ![AWS: Richtlinie für eine Rolle anfügen](./media/storage-accounts/aws-attach-policy-role.png)

**Für Benutzer:**

1. Wählen Sie den Cloudyn-Benutzer aus.
2. Klicken Sie auf der Registerkarte **Permissions** auf **Add permissions**.
3. Wählen Sie im Abschnitt **Grant Permission** die Option **Attach existing policies directly** aus.
4. Suchen Sie nach der von Ihnen erstellten Richtlinie, wählen Sie diese aus, und klicken Sie auf **Next: Review**.
5. Klicken Sie auf der Seite „Add permissions to role name“ auf **Add permissions**.  
    ![AWS: Richtlinie für einen Benutzer anfügen](./media/storage-accounts/aws-attach-policy-user.png)


### <a name="optional-set-permission-with-bucket-policy"></a>Optional: Festlegen der Berechtigung mit einer Bucketrichtlinie

Sie können die Berechtigung zum Erstellen von Berichten in Ihrem S3-Bucket auch mit einer Bucketrichtlinie festlegen. In der klassischen S3-Ansicht:

1. Erstellen Sie einen Bucket, oder wählen Sie einen vorhandenen aus.
2. Wählen Sie die Registerkarte **Permissions** aus, und klicken Sie dann auf **Bucket policy**.
3. Kopieren Sie das folgende Richtlinienbeispiel, und fügen Sie es ein. Ersetzen Sie &lt;bucket\_name&gt; und &lt;Cloudyn\_principle&gt; durch den ARN Ihrer Buckets. Ersetzen Sie den ARN der Rolle oder des Benutzers, die oder der von Cloudyn verwendet wird.

  ```
{
  "Id": "Policy1485775646248",
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "SaveReport2S3",
      "Action": [
        "s3:PutObject"
      ],
      "Effect": "Allow",
      "Resource": "<bucket_name>/*",
      "Principal": {
        "AWS": [
          "<Cloudyn_principle>"
        ]
      }
    }
  ]
}
```

4. Klicken Sie im Editor für Bucketrichtlinien auf **Save**.

### <a name="add-aws-report-storage-to-cloudyn"></a>Hinzufügen von AWS-Berichtsspeicher zu Cloudyn

1. Öffnen Sie im Azure-Portal das Cloudyn-Portal, oder navigieren Sie zu https://azure.cloudyn.com, und melden Sie sich an.
2. Klicken Sie auf das Zahnrad-Symbol, und wählen Sie dann **Reports Storage Management** aus.
3. Klicken Sie auf **Add new +**, und vergewissern Sie sich, dass AWS ausgewählt ist.
4. Wählen Sie ein Konto und einen Speicherbucket aus. Der Name des AWS-Speicherbuckets wird automatisch ausgefüllt.  
    ![Hinzufügen von Berichtsspeicher für einen AWS Bucket](./media/storage-accounts/aws-cloudyn-storage.png)  
5. Klicken Sie auf **Save**, und klicken Sie dann auf **Ok**.

    Ihr neuer Eintrag für AWS-Berichtsspeicher wird in der Speicherkontoliste angezeigt.  
    ![Neuer AWS-Berichtsspeicher in der Liste](./media/storage-accounts/aws-storage-entry.png)


Sie können jetzt Berichte im Azure-Speicher speichern. Klicken Sie in irgendeinem Bericht auf **Actions**, und wählen Sie dann **Schedule report** aus. Geben Sie dem Bericht einen Namen, und fügen Sie dann entweder Ihre eigene URL hinzu, oder verwenden Sie die automatisch erstellte URL. Wählen Sie **Save to storage** aus, und wählen Sie dann das Speicherkonto aus. Geben Sie ein Präfix ein, das an den Namen der Berichtdatei angefügt wird. Wählen Sie entweder CSV oder JSON als Dateiformat aus, und speichern Sie dann den Bericht.

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie [Grundlegendes zu Kostenverwaltungsberichten](understanding-cost-reports.md), um mehr über die grundlegende Struktur und die grundlegenden Funktionen von Kostenverwaltungsberichten zu erfahren.
