---
title: Verwenden von Datenbanken, die vom SQL-Adapter-Ressourcenanbieter in Azure Stack bereitgestellt werden | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie SQL-Datenbanken, die mit dem SQL-Adapter-Ressourcenanbieter bereitgestellt wurden, erstellen und verwalten.
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
ms.date: 05/01/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 2808847642639069e60102b195ac97957c8593f0
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/04/2018
ms.locfileid: "33203458"
---
# <a name="create-sql-databases"></a>Erstellen von SQL-Datenbanken
Self-Service-Datenbanken werden über das Benutzerportal bereitgestellt. Ein Benutzer benötigt ein Abonnement mit einem Angebot, das den Datenbankdienst einschließt.

1. Melden Sie sich beim [Azure Stack](azure-stack-poc.md)-Benutzerportal an (Dienstadministratoren können auch das Administratorportal verwenden).

2. Klicken Sie auf **+ Neu** &gt;**Daten und Speicher** &gt; **SQL Server-Datenbank** &gt; **Hinzufügen**.

3. Füllen Sie das Formular mit den Details der Datenbank aus, einschließlich eines **Datenbanknamens** und der **maximalen Größe**, und ändern Sie die anderen Parameter bei Bedarf. Sie werden aufgefordert, eine SKU für Ihre Datenbank auszuwählen. Wenn Hostserver hinzugefügt werden, wird ihnen eine SKU zugewiesen. Datenbanken werden in dem Pool von Hostservern, aus denen die SKU besteht, erstellt.

  ![Neue Datenbank](./media/azure-stack-sql-rp-deploy/newsqldb.png)

  >[!NOTE]
  > Die Datenbank muss mindestens 64 MB groß sein. Sie kann über die Einstellungen vergrößert werden.

4. Füllen Sie die Anmeldeeinstellungen aus: **Datenbankanmeldung** und **Kennwort**. Bei diesen Einstellungen handelt es sich um Anmeldeinformationen für die SQL-Authentifizierung, die nur für Ihren Zugriff auf diese Datenbank erstellt werden. Der Benutzername für die Anmeldung muss global eindeutig sein. Erstellen Sie entweder eine neue Anmeldeeinstellung, oder wählen Sie eine vorhandene aus. Sie können die Anmeldeeinstellungen für andere Datenbanken wiederverwenden, die dieselbe SKU verwenden.

    ![Erstellen einer neuen Datenbankanmeldung](./media/azure-stack-sql-rp-deploy/create-new-login.png)


5. Übermitteln Sie das Formular, und warten Sie, bis die Bereitstellung abgeschlossen ist.

    Beachten Sie im resultierenden Blatt das Feld „Verbindungszeichenfolge“. Sie können diese Zeichenfolge in jeder Anwendung (z.B. eine Web-App) in Azure Stack verwenden, die den Zugriff auf SQL Server erfordert.

    ![Abrufen der Verbindungszeichenfolge](./media/azure-stack-sql-rp-deploy/sql-db-settings.png)

## <a name="delete-sql-alwayson-databases"></a>Löschen von SQL Always On-Datenbanken
Wenn eine SQL-Always On-Datenbank vom Ressourcenanbieter gelöscht wird, erfolgt die Löschung aus den primären und AlwaysOn-Verfügbarkeitsgruppen. Die SQL-Verfügbarkeitsgruppe platziert die Datenbank jedoch im Wiederherstellungszustand in jedes Replikat und entfernt die Datenbank erst nach expliziter Auslösung endgültig. Wenn eine Datenbank nicht gelöscht wird, wechseln die sekundären Replikate in den nicht synchronisierten Zustand. Das erneute Hinzufügen einer neuen Datenbank zur Verfügbarkeitsgruppe über den Ressourcenanbieter funktioniert weiterhin.

## <a name="verify-sql-alwayson-databases"></a>Überprüfen von SQL Always On-Datenbanken
Always On-Datenbanken sollten als synchronisiert und für alle Instanzen verfügbar in der Verfügbarkeitsgruppe angezeigt werden. Nach dem Failover sollte die Datenbank nahtlos eine Verbindung herstellen können. Sie können SQL Server Management Studio verwenden, um zu überprüfen, ob eine Datenbank synchronisiert wird:

![Überprüfen von Always On](./media/azure-stack-sql-rp-deploy/verifyalwayson.png)


## <a name="next-steps"></a>Nächste Schritte

[Warten des SQL Server-Ressourcenanbieters](azure-stack-sql-resource-provider-maintain.md)
