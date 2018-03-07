---
title: "Schützen von B2B-Nachrichten mit Zertifikaten in Azure Logic Apps | Microsoft Docs"
description: "Hinzufügen von Zertifikaten zum Schützen von B2B-Nachrichten mit dem Enterprise Integration Pack"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 4cbffd85-fe8d-4dde-aa5b-24108a7caa7d
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 708bdcddede71186c48ae7d4034cc9df0bd61391
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="secure-b2b-messages-with-certificates"></a>Schützen von B2B-Nachrichten mit Zertifikaten

In einigen Fällen müssen Sie dafür sorgen, dass die B2B-Kommunikation vertraulich bleibt. Um die B2B-Kommunikation für Ihre Unternehmensintegrations-Apps, insbesondere Logik-Apps, zu schützen, können Sie Ihrem Integrationskonto Zertifikate hinzufügen. Zertifikate sind digitale Dokumente, mit denen die Identität der Teilnehmer einer elektronischen Kommunikation überprüft wird.
Mit Zertifikaten können Sie die Kommunikation auf folgende Weise schützen:

* Verschlüsseln des Nachrichteninhalts
* Digitales Signieren von Nachrichten  

In Ihren Unternehmensintegrations-Apps können Sie die folgenden Zertifikate verwenden:

* Öffentliche Zertifikate, die bei einer Zertifizierungsstelle erworben werden müssen.
* Private, selbst ausgestellte Zertifikate. Diese Zertifikate werden gelegentlich auch als selbstsignierte Zertifikate bezeichnet.

## <a name="upload-a-public-certificate"></a>Hochladen eines öffentlichen Zertifikats

Um ein *öffentliches Zertifikat* in Logik-Apps mit B2B-Funktionen verwenden zu können, müssen Sie das Zertifikat zuerst in Ihr Integrationskonto hochladen. Nachdem Sie die Eigenschaften in den von Ihnen erstellten [Vereinbarungen](logic-apps-enterprise-integration-agreements.md) definiert haben, steht das Zertifikat zum Schützen Ihrer B2B-Nachrichten zur Verfügung.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Wählen Sie im Azure-Hauptmenü die Option **Alle Dienste** aus. Geben Sie im Suchfeld das Wort „Integration“ ein, und wählen Sie dann **Integrationskonten** aus.

   ![Suchen Ihres Integrationskontos](media/logic-apps-enterprise-integration-certificates/overview-1.png)  

3. Wählen Sie unter **Integrationskonten** das Integrationskonto aus, dem Sie das Zertifikat hinzufügen möchten.

   ![Integrationskonto auswählen, dem Sie das Zertifikat hinzufügen möchten](media/logic-apps-enterprise-integration-certificates/overview-3.png)  

4. Wählen Sie die Kachel **Zertifikate** aus.  

   ![„Zertifikate“ auswählen](media/logic-apps-enterprise-integration-certificates/certificate-1.png)

5. Wählen Sie unter **Zertifikate** die Option **Hinzufügen** aus.

   ![„Hinzufügen“ auswählen](media/logic-apps-enterprise-integration-certificates/certificate-2.png)

6. Geben Sie unter **Zertifikat hinzufügen** die Details für Ihr Zertifikat an.
   
   1. Geben Sie den **Namen** für Ihr Zertifikat ein. Wählen Sie als Zertifikattyp die Option **Öffentlich** aus.

   2. Wählen Sie rechts neben dem Feld **Zertifikat** das Ordnersymbol aus. 
   Suchen Sie nach der Zertifikatsdatei, die Sie hochladen möchten, und wählen Sie diese aus. 
   Wählen Sie **OK** aus, wenn Sie fertig sind.

      ![Hochladen eines öffentlichen Zertifikats](media/logic-apps-enterprise-integration-certificates/certificate-3.png)

   Azure lädt das Zertifikat nach dem Überprüfen Ihrer Auswahl hoch.

   ![Neues Zertifikat anzeigen](media/logic-apps-enterprise-integration-certificates/certificate-4.png) 

## <a name="upload-a-private-certificate"></a>Hochladen eines privaten Zertifikats

Um ein *privates Zertifikat* in Logik-Apps mit B2B-Funktionen verwenden zu können, müssen Sie das Zertifikat zuerst in Ihr Integrationskonto hochladen. Außerdem benötigen Sie einen privaten Schlüssel, den Sie zuerst zu [Azure Key Vault](../key-vault/key-vault-get-started.md) hinzufügen müssen. 

Nachdem Sie die Eigenschaften in den von Ihnen erstellten [Vereinbarungen](logic-apps-enterprise-integration-agreements.md) definiert haben, steht das Zertifikat zum Schützen Ihrer B2B-Nachrichten zur Verfügung.

> [!NOTE]
> Stellen Sie bei privaten Zertifikaten sicher, dass Sie ein entsprechendes öffentliches Zertifikat hinzufügen, das in der [AS2-Vereinbarung](logic-apps-enterprise-integration-as2.md) in den Sende- und Empfangseinstellungen für das Signieren und Verschlüsseln von Nachrichten angezeigt wird.

1. [Fügen Sie Ihren privaten Schlüssel zu Azure Key Vault hinzu](../key-vault/key-vault-get-started.md#add), und geben Sie einen **Schlüsselnamen** an.
   
2. Autorisieren Sie Azure Logic Apps für die Ausführung von Vorgängen für Azure Key Vault. Verwenden Sie zum Erteilen des Zugriffs auf den Logic Apps-Dienstprinzipal den PowerShell-Befehl [Set-AzureRmKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy). Beispiel:

   `Set-AzureRmKeyVaultAccessPolicy -VaultName 'TestcertKeyVault' -ServicePrincipalName 
   '7cd684f4-8a78-49b0-91ec-6a35d38739ba' -PermissionsToKeys decrypt, sign, get, list`
 
3. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

4. Wählen Sie im Azure-Hauptmenü die Option **Alle Dienste** aus. Geben Sie im Suchfeld das Wort „Integration“ ein, und wählen Sie dann **Integrationskonten** aus.

   ![Suchen Ihres Integrationskontos](media/logic-apps-enterprise-integration-certificates/overview-1.png) 

5. Wählen Sie unter **Integrationskonten** das Integrationskonto aus, dem Sie das Zertifikat hinzufügen möchten.

6. Wählen Sie die Kachel **Zertifikate** aus.  

   ![Kachel „Zertifikate“ auswählen](media/logic-apps-enterprise-integration-certificates/certificate-1.png)

7. Wählen Sie unter **Zertifikate** die Option **Hinzufügen** aus.   

   ![Schaltfläche „Hinzufügen“ auswählen](media/logic-apps-enterprise-integration-certificates/certificate-2.png)

8. Geben Sie unter **Zertifikat hinzufügen** die Details für Ihr Zertifikat an.
   
   1. Geben Sie den **Namen** für Ihr Zertifikat ein. Wählen Sie als Zertifikattyp die Option **Privat** aus.

   2. Wählen Sie rechts neben dem Feld **Zertifikat** das Ordnersymbol aus. 
   Suchen Sie nach der Zertifikatsdatei, die Sie hochladen möchten, und wählen Sie diese aus. 
   Wählen Sie außerdem die **Ressourcengruppe**, den **Schlüsseltresor**, und den **Schlüsselnamen** aus. 
   Wählen Sie **OK** aus, wenn Sie fertig sind.

      ![Hinzufügen des Zertifikats](media/logic-apps-enterprise-integration-certificates/privatecertificate-1.png)

   Azure lädt das Zertifikat nach dem Überprüfen Ihrer Auswahl hoch.

   ![Neues Zertifikat anzeigen](media/logic-apps-enterprise-integration-certificates/privatecertificate-2.png)

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen einer B2B-Vereinbarung](logic-apps-enterprise-integration-agreements.md)
