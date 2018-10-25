---
title: Speichern des AppSource-Pakets in Azure Storage und Generieren einer URL mit SAS-Schlüssel | Microsoft-Dokumentation
description: Beschreibt die Schritte, die erforderlich sind, um ein AppSource-Paket hochzuladen und zu sichern.
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
ms.date: 09/18/2018
ms.author: pbutlerm
ms.openlocfilehash: 335590cc616035020f66cfc5208e21d4c5128fe6
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48806533"
---
<a name="store-your-appsource-package-to-azure-storage-and-generate-a-url-with-sas-key"></a>Speichern des AppSource-Pakets in Azure Storage und Generieren einer URL mit SAS-Schlüssel
=============================================================================

Um die Sicherheit Ihrer Dateien sicherzustellen, müssen alle Partner ihre AppSource-Paketdatei in einem Azure Blob Storage-Konto speichern und mit einem SAS-Schlüssel freigeben. Wir werden die Paketdatei von Ihrem Azure Storage-Speicherort zur Zertifizierung abrufen und für AppSource-Tests verwenden.

Führen Sie die folgenden Schritte aus, um Ihr Paket in den Blobspeicher hochzuladen:

1.  Wechseln Sie zu <http://azure.microsoft.com> und erstellen Sie eine kostenlose Testversion oder ein kostenpflichtiges Konto.

2.  Melden Sie sich beim [Azure-Portal](http://portal.azure.com/)an.

3.  Erstellen Sie ein neues Speicherkonto, indem Sie auf **+ Neu** klicken und zum Konto **Daten und Speicher** wechseln.

  ![Blatt „Daten und Speicher“ im Microsoft Azure-Portal](media/CRMScreenShot7.png)

4. Geben Sie einen **Namen** und **Ressourcengruppennamen** ein und klicken Sie auf die Schaltfläche **Erstellen**.

  ![Erstellen eines Speicherkontos im Microsoft Azure-Portal](media/CRMScreenShot8.png)

5. Navigieren Sie zu Ihrer neu erstellten Ressourcengruppe und erstellen Sie einen neuen Blobcontainer.

  ![Hochladen des Pakets als Blob über das Microsoft Azure-Portal](media/CRMScreenShot9.png)

6.  Laden Sie den [Microsoft Azure Storage-Explorer](http://storageexplorer.com/) herunter, und installieren Sie ihn, falls dies noch nicht geschehen ist.

7.  Öffnen Sie den Storage Explorer und verwenden Sie das Symbol, um eine Verbindung zu Ihrem Azure Storage-Konto herzustellen.

8.  Navigieren Sie zu dem von Ihnen erstellten Blobcontainer und klicken Sie auf **Hochladen**, um Ihre ZIP-Paketdatei hinzuzufügen.

  ![Hochladen von Paketen mit dem Microsoft Storage-Explorer](media/CRMScreenShot10.png)

9.  Klicken Sie mit der rechten Maustaste auf Ihre Datei, und wählen Sie **Shared Access Signature abrufen** aus.

  ![Abrufen der Shared Access Signature einer Azure-Datei](media/CRMScreenShot11.png)

10.  Ändern Sie die **Ablaufzeit**, um die SAS für einen Monat zu aktivieren, und klicken Sie dann auf **Erstellen**.

  ![Ändern des SAS-Ablaufdatums einer Azure-Datei](media/CRMScreenShot12.png)

11.  Kopieren Sie das URL-Feld und speichern Sie es zur späteren Verwendung. Diese URL müssen Sie beim Erstellen des zugehörigen Angebots eingeben. 

  ![Kopieren der SAS-URL einer Azure-Datei](media/CRMScreenShot13.png)

