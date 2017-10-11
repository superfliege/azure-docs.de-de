
<!--
includes/sql-database-include-ip-address-22-v12portal.md

Latest Freshness check:  2016-03-21 , daleche.

As of circa 2015-09-04, the following topics might include this include:
articles/sql-database/sql-database-configure-firewall-settings.md
articles/sql-database/sql-database-connect-query.md


## Server-level firewall rules

### Add a server-level firewall rule through the new Azure portal
-->


1. Melden Sie sich auf die [Azure-Portal](https://portal.azure.com/) am http://portal.azure.com/.
2. Klicken Sie in dem linken Banner auf **alle Durchsuchen**. Die **Durchsuchen** Blatt wird angezeigt.
3. Führen Sie einen Bildlauf aus, und klicken Sie auf **SQL Server**. Die **SQL Servern** Blatt wird angezeigt.
   
    ![Suchen nach Ihrem Azure SQL-Datenbank-Server im portal][b21-FindServerInPortal]
4. Der Einfachheit halber klicken Sie auf das Steuerelement minimieren, auf den früheren **Durchsuchen** Blatt.
5. Starten Sie in das Textfeld "Filter" Geben Sie den Namen des Servers ein. Es wird eine Zeile angezeigt.
6. Klicken Sie auf die Zeile für den Server. Es wird ein Blatt für den Server angezeigt.
7. Klicken Sie auf der Seite "Server" auf **Einstellungen**. Die **Einstellungen** Blatt wird angezeigt.
8. Klicken Sie auf **Firewall**. Die **Firewalleinstellungen** Blatt wird angezeigt.
   
    ![Klicken Sie auf ' Einstellungen > Firewall][b31-SettingsFirewallNavig]
9. Klicken Sie auf **Hinzufügen von Client-IP-**. Geben Sie einen Namen für die neue Regel ein, in das erste Textfeld.
10. Geben Sie die Werte der niedrigen und hohen IP-Adresse für den Bereich an, die, den Sie aktivieren möchten.
    
    * Kann es praktisch, wenn das Ende der niedrigen Wert mit **.0** und High mit **.255**.
    
    ![Fügen Sie einen IP-Adressbereich zu ermöglichen][b41-AddRange]
11. Klicken Sie auf **Speichern**.

<!-- Image references. -->

[b21-FindServerInPortal]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b21-v12portal-findsvr.png

[b31-SettingsFirewallNavig]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b31-v12portal-settingsfirewall.png

[b41-AddRange]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b41-v12portal-addrange.png



<!--
These includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-ip-address-22-v12portal.md
? includes/sql-database-include-ip-address-*.md
-->
