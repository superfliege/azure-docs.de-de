1. Melden Sie sich an den [Azure-Portal][Azure portal].
2. Klicken Sie im linken Navigationsbereich des Portals auf **neu**, klicken Sie dann auf **Enterprise Integration**, und klicken Sie dann auf **Relay**.
3. In der **Namespace erstellen** Dialogfeld, geben Sie einen Namespace ein. Das System überprüft sofort, um festzustellen, ob der Name verfügbar ist.
4. In der **Abonnement** Feld, und wählen Sie ein Azure-Abonnement, um den Namespace zu erstellen.
5. In der  **[Ressourcengruppe](../articles/azure-resource-manager/resource-group-portal.md)**  Feld, und wählen Sie eine vorhandene Ressourcengruppe, in dem der Namespace wird Live- oder ein neues erstellen.      
6. In **Speicherort**, wählen Sie das Land bzw. die Region, die in der der Namespace gehostet werden soll.
   
    ![Namespace erstellen][create-namespace]
7. Klicken Sie auf **Erstellen**. Das System jetzt Ihren Namespace erstellt und aktiviert. Nach einigen Minuten wird das System Ressourcen für Ihr Konto bereitgestellt.

### <a name="obtain-the-management-credentials"></a>Abrufen der Anmeldeinformationen
1. Klicken Sie in der Liste der Namespaces auf die neu erstellte Namespacename.
2. Klicken Sie auf dem Blatt "Namespace" auf **mit gemeinsamen Zugriffsrichtlinien**.
3. In der **mit gemeinsamen Zugriffsrichtlinien** Blatt, klicken Sie auf **RootManageSharedAccessKey**.
   
    ![Verbindungsinformationen][connection-info]
4. In der **Richtlinie: RootManageSharedAccessKey** Blatt, klicken Sie auf die Kopie Schaltfläche neben **Zeichenfolge – primäre Verbindungsschlüssel**, um die Verbindungszeichenfolge in die Zwischenablage zur späteren Verwendung zu kopieren. Fügen Sie diesen Wert in Editor oder einem anderen temporären Speicherort aus.
   
    ![Verbindungszeichenfolgen][connection-string]

5. Wiederholen Sie den vorherigen Schritt, kopieren und Einfügen den Wert der **Primärschlüssel** an einen temporären Speicherort für die spätere Verwendung.  

<!--Image references-->

[create-namespace]: ./media/relay-create-namespace-portal/create-namespace.png
[connection-info]: ./media/relay-create-namespace-portal/connection-info.png
[connection-string]: ./media/relay-create-namespace-portal/connection-string.png
[Azure portal]: https://portal.azure.com
