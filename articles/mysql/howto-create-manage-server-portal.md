---
title: "Erstellen und Verwalten eines Servers mit Azure-Datenbank für MySQL im Azure-Portal | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie Sie schnell einen neuen Server mit Azure-Datenbank für MySQL erstellen und mithilfe des Azure-Portals verwalten."
services: mysql
author: v-chenyh
ms.author: nolanwu
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 09/15/2017
ms.openlocfilehash: 6e9c541aac1241b6af0e4a58f5591d46f9a98c40
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-manage-azure-database-for-mysql-server-using-azure-portal"></a>Erstellen und Verwalten eines Servers mit Azure-Datenbank für MySQL im Azure-Portal
In diesem Artikel wird beschrieben, wie Sie auf schnelle Weise einen neuen Server für eine Azure Database for MySQL erstellen können. Außerdem erhalten Sie Informationen zum Verwalten des Servers mithilfe des Azure-Portals. Die Serververwaltung umfasst das Anzeigen von Serverdetails und Datenbanken, die Kennwortzurücksetzung und das Löschen des Servers.

## <a name="log-in-to-the-azure-portal"></a>Anmelden beim Azure-Portal
Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.

## <a name="create-an-azure-database-for-mysql-server"></a>Erstellen einer Azure-Datenbank für MySQL-Server
Führen Sie die folgenden Schritte aus, um einen Server für eine Azure Database for MySQL mit dem Namen „mysqlserver4demo“ zu erstellen.

1. Klicken Sie oben links im Azure-Portal auf die Schaltfläche **Neu**.

2. Wählen Sie auf der Seite „Neu“ die Option **Datenbanken** und anschließend auf der Seite „Datenbanken“ die Option **Azure Database for MySQL** aus.

    > Ein Server mit Azure-Datenbank für MySQL wird mit einer definierten Gruppe von [Compute- und Speicherressourcen](./concepts-compute-unit-and-storage.md) erstellt. Die Datenbank wird in einer Azure-Ressourcengruppe und auf einem Server mit Azure-Datenbank für MySQL erstellt.

   ![create-new-server](./media/howto-create-manage-server-portal/create-new-server.png)

3. Tragen Sie folgende Informationen in das Formular „Azure Database for MySQL“ ein:

    | **Formularfeld** | **Feldbeschreibung** |
    |----------------|-----------------------|
    | *Servername* | azure-mysql (Servername ist global eindeutig) |
    | *Abonnement* | MySQLaaS (aus dem Dropdownmenü auswählen) |
    | *Ressourcengruppe* | myresource (neue Ressourcengruppe erstellen oder eine bereits vorhandene verwenden) |
    | *Serveradministratoranmeldung* | myadmin (Administratorkontoname einrichten) |
    | *Kennwort* | Kennwort des Administratorkontos einrichten |
    | *Kennwort bestätigen* | Kennwort des Administratorkontos bestätigen |
    | *Standort* | Europa, Norden (zwischen „Europa, Norden“ und „USA, Westen“ wählen) |
    | *Version* | 5.6 (Serverversion für Azure-Datenbank für MySQL wählen) |

4. Klicken Sie auf **Tarif**, um die Dienstebene und die Leistungsstufe für Ihren neuen Server anzugeben. Die Computeeinheit kann zwischen 50 und 100 im Basic-Tarif und zwischen 100 und 200 im Standard-Tarif festgelegt werden, und basierend auf der enthaltenen Menge kann Speicherplatz hinzugefügt werden. Für diese Anleitung wählen wir den Wert 50 als Computeeinheit und 50 GB aus. Klicken Sie zum Speichern der Auswahl auf **OK** .

   ![Servertarif erstellen](./media/howto-create-manage-server-portal/create-server-pricing-tier.png)

5. Klicken Sie auf **Erstellen**, um den Server bereitzustellen. Die Bereitstellung dauert einige Minuten.

    > Wählen Sie die Option **An das Dashboard anheften** aus, um das leichte Nachverfolgen Ihrer Bereitstellungen zu ermöglichen.
    > [!NOTE]
    > Für den Speicher werden zwar bis zu 1.000 GB im Basic-Tarif und 10.000 GB im Standard-Tarif unterstützt, jedoch ist die maximale Speichergröße für die öffentliche Vorschauversion weiterhin vorübergehend auf 1.000 GB beschränkt.</Include>

## <a name="update-an-azure-database-for-mysql-server"></a>Aktualisieren eines Servers mit Azure-Datenbank für MySQL
Nach der Bereitstellung des neuen Servers gibt es für den Benutzer zwei Optionen zum Bearbeiten eines vorhandenen Servers: Zurücksetzen des Administratorkennworts oder zentrales Hoch- bzw. Herunterskalieren des Servers durch Ändern der Computeeinheiten.

### <a name="change-the-administrator-user-password"></a>Ändern des Administratorbenutzerkennworts
1. Klicken Sie auf dem Serverblatt **Übersicht** auf **Kennwort zurücksetzen**, um eine Kennworteingabe und ein Bestätigungsfenster aufzufüllen.

2. Geben Sie ein neues Kennwort ein, und bestätigen Sie es wie unten gezeigt im Fenster:

   ![Kennwort zurücksetzen](./media/howto-create-manage-server-portal/reset-password.png)

3. Klicken Sie auf **OK**, um das neue Kennwort zu speichern.

### <a name="scale-updown-by-changing-compute-units"></a>Zentrales Hoch/Herunterskalieren des durch Ändern der Compute-Einheiten

1. Klicken Sie auf dem Serverblatt unter **Einstellungen** auf **Tarif**, um das Blatt „Tarif“ für den Server für die Azure Database for MySQL zu öffnen.

2. Führen Sie Schritt 4 in der Anleitung **Erstellen eines Servers für Azure Database for MySQL** aus, um Computeeinheiten im gleichen Tarif zu ändern.

## <a name="delete-an-azure-database-for-mysql-server"></a>Löschen eines Servers mit Azure-Datenbank für MySQL

1. Klicken Sie auf dem Serverblatt **Übersicht** auf die Befehlsschaltfläche **Löschen**, um das Blatt mit der Löschbestätigung zu öffnen.

2. Geben Sie zur doppelten Bestätigung den richtigen Servernamen im Eingabefeld des Blatts ein.

3. Klicken Sie erneut auf die Schaltfläche **Löschen**, um den Löschvorgang zu bestätigen. Warten Sie anschließend darauf, dass die Popupmeldung „Erfolgreich gelöscht“ auf der Benachrichtigungsleiste erscheint.

## <a name="list-the-azure-database-for-mysql-databases"></a>Auflisten der Datenbanken von Azure-Datenbank für MySQL
Scrollen Sie auf dem Serverblatt **Übersicht** nach unten, bis die Datenbankkachel im unteren Bereich angezeigt wird. Alle Datenbanken werden in der Tabelle aufgeführt. Klicken Sie auf die Befehlsschaltfläche **Löschen**, um das Blatt mit der Löschbestätigung zu öffnen.

   ![show-databases](./media/howto-create-manage-server-portal/show-databases.png)

## <a name="show-details-of-an-azure-database-for-mysql-server"></a>Anzeigen von Details zu einem Server mit Azure-Datenbank für MySQL
Klicken Sie auf dem Serverblatt unter **Einstellungen** auf **Eigenschaften**, um das Blatt mit den **Eigenschaften** zu öffnen. Dort finden Sie alle ausführlichen Informationen über den Server.

## <a name="next-steps"></a>Nächste Schritte

[Schnellstart: Erstellen eines Servers mit Azure-Datenbank für MySQL im Azure-Portal](./quickstart-create-mysql-server-database-using-azure-portal.md)