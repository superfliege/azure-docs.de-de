---
title: Verwenden von Datenbanken, die vom MySQL-Adapter-Ressourcenanbieter in Azure Stack bereitgestellt werden | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie MySQL-Datenbanken, die mit dem MySQL-Adapter-Ressourcenanbieter bereitgestellt wurden, erstellen und verwalten.
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/16/2018
ms.author: jeffgilb
ms.reviewer: quying
ms.lastreviewed: 10/16/2018
ms.openlocfilehash: 3063a3f3f199a591fa94c3a665ea9ecc5457e502
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55247594"
---
# <a name="create-mysql-databases"></a>Erstellen von MySQL-Datenbanken

Self-Service-Datenbanken können im Benutzerportal erstellt und verwaltet werden. Ein Azure Stack-Benutzer benötigt ein Abonnement mit einem Angebot, das den MySQL-Datenbankdienst einschließt.

## <a name="test-your-deployment-by-creating-a-mysql-database"></a>Testen der Bereitstellung durch Erstellen einer MySQL-Datenbank

1. Melden Sie sich beim Azure Stack-Benutzerportal an.
2. Wählen Sie **+Ressource erstellen** > **Daten und Speicher** > **MySQL-Datenbank** > **Hinzufügen** aus.
3. Geben Sie unter **MySQL-Datenbank erstellen** den Datenbanknamen ein, und konfigurieren Sie die anderen Einstellungen für Ihre Umgebung.

    ![Erstellen einer MySQL-Testdatenbank](./media/azure-stack-mysql-rp-deploy/mysql-create-db.png)

4. Klicken Sie unter **Datenbank erstellen** auf **SKU**. Wählen Sie unter **Select a MySQL SKU** (MySQL-SKU auswählen) die SKU für Ihre Datenbank aus.

    ![Auswählen einer MySQL-SKU](./media/azure-stack-mysql-rp-deploy/mysql-select-sku.png)

    >[!Note]
    >Wenn Hostserver in Azure Stack hinzugefügt werden, wird ihnen eine SKU zugewiesen. Datenbanken werden im Pool von Hostservern in einer SKU erstellt.

5. Klicken Sie unter **Anmeldung** auf ***Erforderliche Einstellungen konfigurieren***.
6. Unter **Anmeldung auswählen** können Sie eine bereits vorhandene Anmeldung auswählen oder auf **+ Neue Anmeldung erstellen** klicken, um eine neue Anmeldung einzurichten.  Geben Sie einen Namen für **Datenbankanmeldung** sowie ein **Kennwort** ein, und klicken Sie anschließend auf **OK**.

    ![Erstellen einer neuen Datenbankanmeldung](./media/azure-stack-mysql-rp-deploy/create-new-login.png)

    >[!NOTE]
    >Der Name für die Datenbankanmeldung darf in MySQL 5.7 maximal 32 Zeichen lang sein. In früheren Versionen durften sie 16 Zeichen nicht überschreiten.

7. Klicken Sie auf **Erstellen**, um die Einrichtung der Datenbank abzuschließen.

Notieren Sie sich nach der Bereitstellung der Datenbank die **Verbindungszeichenfolge** aus der **Zusammenfassung**. Diese Zeichenfolge kann in jeder Anwendung verwendet werden, die auf die MySQL-Datenbank zugreifen muss.

![Abrufen der Verbindungszeichenfolge für die MySQL-Datenbank](./media/azure-stack-mysql-rp-deploy/mysql-db-created.png)

## <a name="update-the-administrative-password"></a>Aktualisieren des Administratorkennworts

Das Kennwort kann in der MySQL Server-Instanz geändert werden.

1. Wählen Sie **VERWALTUNGSRESSOURCEN** > **MySQL-Hostserver** aus. Wählen Sie den Hostserver aus.
2. Wählen Sie unter **Einstellungen** die Option **Kennwort** aus.
3. Geben Sie unter **Kennwort** das neue Kennwort ein, und klicken Sie anschließend auf **Speichern**.

![Aktualisieren des Administratorkennworts](./media/azure-stack-mysql-rp-deploy/mysql-update-password.png)

## <a name="next-steps"></a>Nächste Schritte

[Aktualisieren des MySQL-Ressourcenanbieters](azure-stack-mysql-resource-provider-update.md)
