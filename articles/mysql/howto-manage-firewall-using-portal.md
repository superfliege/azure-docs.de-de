---
title: Erstellen und Verwalten von MySQL-Firewallregeln in Azure Database for MySQL | Microsoft-Dokumentation
description: "Erstellen und Verwalten von Firewallregeln für Azure-Datenbank für MySQL mithilfe des Azure-Portals"
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 01/20/2018
ms.openlocfilehash: 3ab65ad99b3219060bb044b0e6b84edf3f1737e0
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2018
---
# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-by-using-the-azure-portal"></a>Erstellen und Verwalten von Firewallregeln für Azure Database for MySQL mithilfe des Azure-Portals
Mithilfe von Firewallregeln auf Serverebene können Administratoren über eine bestimmte IP-Adresse oder über einen IP-Adressbereich auf einen Server für Azure Database for MySQL zugreifen. 

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Erstellen einer Firewallregel auf Serverebene im Azure-Portal

1. Klicken Sie auf der Seite des MySQL-Servers unter der Überschrift „Einstellungen“ auf **Verbindungssicherheit**, um die Seite „Verbindungssicherheit“ für Azure Database for MySQL zu öffnen.

   ![Azure-Portal – Klicken auf „Verbindungssicherheit“](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. Klicken Sie auf der Symbolleiste auf **Meine IP-Adresse hinzufügen**. Dadurch wird automatisch eine Firewallregel mit der öffentlichen IP-Adresse Ihres Computers (gemäß Erkennung durch das Azure-System) erstellt.

   ![Azure-Portal – Klicken auf „Meine IP-Adresse hinzufügen“](./media/howto-manage-firewall-using-portal/2-add-my-ip.png)

3. Überprüfen Sie Ihre IP-Adresse, bevor Sie die Konfiguration speichern. In einigen Situationen weicht die vom Azure-Portal erkannte IP-Adresse von der IP-Adresse ab, die für den Zugriff auf das Internet und die Azure-Server verwendet wird. Aus diesem Grund müssen Sie die Start-IP und die End-IP ändern, damit die Regel wie erwartet funktioniert.

   Verwenden Sie eine Suchmaschine oder ein anderes Onlinetool, um Ihre eigene IP-Adresse zu überprüfen. Suchen Sie in Bing beispielsweise nach „Wie lautet meine IP?“. 

   ![Bing-Ergebnisse für „Wie lautet meine IP-Adresse?“](./media/howto-manage-firewall-using-portal/3-what-is-my-ip.png)

4. Fügen Sie weitere Adressräume hinzu. In den Firewallregeln für Azure Database for MySQL können Sie eine einzelne IP-Adresse oder einen Adressbereich angeben. Wenn Sie die Regel auf eine einzelne IP-Adresse beschränken möchten, geben Sie dieselbe Adresse in die Felder „Start-IP“ und „End-IP“ ein. Durch Öffnen der Firewall wird es Administratoren, Benutzern und Anwendungen ermöglicht, auf dem MySQL-Server auf alle Datenbanken zuzugreifen, für die sie über gültige Anmeldeinformationen verfügen.

   ![Azure-Portal – Firewallregeln ](./media/howto-manage-firewall-using-portal/4-specify-addresses.png)

5. Klicken Sie auf der Symbolleiste auf **Speichern**, um diese Firewallregel auf Serverebene zu speichern. Warten Sie auf die Bestätigung, dass das Update für die Firewallregeln erfolgreich war.

   ![Azure-Portal – Klicken auf „Speichern“](./media/howto-manage-firewall-using-portal/5-save-firewall-rule.png)

## <a name="connecting-from-azure"></a>Herstellen einer Verbindung über Azure
Um Anwendungen von Azure die Verbindung mit dem Azure Database for MySQL-Server zu ermöglichen, müssen Azure-Verbindungen aktiviert sein. Beispiele: Hosten einer Azure-Web-Apps-Anwendung oder einer auf einem virtuellen Azure-Computer ausgeführten Anwendung und Herstellen einer Verbindung über ein Azure Data Factory-Datenverwaltungsgateway. Die Ressourcen müssen sich nicht im gleichen virtuellen Netzwerk (VNet) oder in der gleichen Ressourcengruppe für die Firewallregel befinden, um diese Verbindungen zu ermöglichen. Wenn eine Azure-Anwendung versucht, eine Verbindung mit dem Datenbankserver herzustellen, prüft die Firewall, ob Azure-Verbindungen zulässig sind. Diese Arten von Verbindungen können auf unterschiedliche Weise ermöglicht werden. Eine Firewalleinstellung, bei der die Start- und Endadresse den Wert 0.0.0.0 aufweist, gibt an, dass diese Verbindungen zulässig sind. Alternativ können Sie im Portal im Bereich **Verbindungssicherheit** die Option **Zugriff auf Azure-Dienste erlauben** auf **EIN** festlegen und auf **Speichern** klicken. Ist der Verbindungsversuch nicht zulässig, erreicht die Anforderung den Azure Database for MySQL-Server nicht.

> [!IMPORTANT]
> Diese Option konfiguriert die Firewall derart, dass alle von Azure ausgehenden Verbindungen zugelassen werden, einschließlich Verbindungen von den Abonnements anderer Kunden. Wenn Sie diese Option auswählen, stellen Sie sicher, dass die Anmelde- und die Benutzerberechtigungen den Zugriff nur auf autorisierte Benutzer beschränken.
> 

## <a name="manage-existing-server-level-firewall-rules-by-using-the-azure-portal"></a>Verwalten vorhandener Firewallregeln auf Serverebene mithilfe des Azure-Portals
Wiederholen Sie die Schritte zum Verwalten der Firewallregeln.
* Um den aktuellen Computer hinzuzufügen, klicken Sie auf **+ Meine IP-Adresse hinzufügen**. Klicken Sie zum Speichern der Änderungen auf **Speichern**.
* Um zusätzliche IP-Adressen hinzuzufügen, geben Sie **REGELNAME**, **START-IP** und **END-IP** ein. Klicken Sie zum Speichern der Änderungen auf **Speichern**.
* Um eine vorhandene Regel zu ändern, klicken Sie auf eines der Felder in der Regel, und ändern Sie den betreffenden Wert. Klicken Sie zum Speichern der Änderungen auf **Speichern**.
* Um eine vorhandene Regel zu löschen, klicken Sie auf die Auslassungspunkte [...] und dann auf **Löschen**. Klicken Sie zum Speichern der Änderungen auf **Speichern**.


## <a name="next-steps"></a>Nächste Schritte
- Sie können auch ein Skript zum [Erstellen und Verwalten von Firewallregeln für Azure Database for MySQL mithilfe der Azure CLI](howto-manage-firewall-using-cli.md) schreiben.
- Wenn Sie Unterstützung beim Herstellen einer Verbindung mit einem Server für Azure-Datenbank für MySQL benötigen, lesen Sie die Informationen unter [Connection libraries for Azure Database for MySQL](./concepts-connection-libraries.md) (Verbindungsbibliotheken für Azure-Datenbank für MySQL).
