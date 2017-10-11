1. Melden Sie sich bei der [Azure-Portal].
2. Klicken Sie auf **+ neu** > **Web + Mobil** > **Mobile Anwendung**, geben Sie einen Namen für Ihre Mobile App-Back-End.
3. Für die **Ressourcengruppe**, vorhandene Ressourcengruppe auswählen oder erstellen Sie eine neue (mit dem gleichen Namen wie Ihre app). 
   
    Sie können eine andere App Service-Plan auswählen oder ein neues erstellen. Weitere Informationen zu App-Dienste Pläne und erstellen einen neuen Plan in einen anderen Preise Ebene und in die gewünschte Position, finden Sie unter [Azure App Service-Pläne detaillierte Übersicht über](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
4. Für die **App Service-Plan**, den Standard-Plan (in der [Standardebene](https://azure.microsoft.com/pricing/details/app-service/)) ausgewählt ist. Sie können auch einen anderen Plan auswählen oder [erstellen Sie eine neue](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan). Bestimmen der App Service-Plan für die [Speicherort, Funktionen, Kosten und Rechen-Ressourcen](https://azure.microsoft.com/pricing/details/app-service/) Ihrer app zugeordnet. 
   
    Nachdem Sie den Plan entschieden haben, klicken Sie auf **erstellen**. Dadurch wird die Mobile App-Back-End erstellt. 
5. In der **Einstellungen** Blatt für die neuen Mobile App-Back-End, klicken Sie auf **Schnellstart** > Ihre Client-app-Plattform > **verbinden eine Datenbank**. 
   
    ![](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-data-connection.png)
6. In der **Datenverbindung hinzufügen** Blatt, klicken Sie auf **SQL-Datenbank** > **Erstellen einer neuen Datenbank**, geben Sie die Datenbank **Namen**, wählen Sie eine Preisstufe und anschließend auf **Server**.  Sie können diese neue Datenbank wiederverwenden. Wenn Sie bereits eine Datenbank am gleichen Speicherort verfügen, können Sie stattdessen auswählen **mithilfe einer vorhandenen Datenbank**. Die Verwendung einer Datenbank an einem anderen Ort nicht aufgrund der Kosten für Bandbreite und höhere Latenz empfohlen.
   
    ![](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-db.png)
7. In der **neuer Server** Blatt, geben Sie einen eindeutigen Namen in der **Servernamen** Feld, geben Sie einen Anmeldenamen und Kennwort überprüfen **Azure-Diensten Zugriff auf den Server erlauben**, und klicken Sie auf **OK**. Dadurch wird die neue Datenbank erstellt.
8. In der **Datenverbindung hinzufügen** Blatt, klicken Sie auf **Verbindungszeichenfolge**, geben Sie die Anmelde- und Kennwortwerte für Ihre Datenbank, und klicken Sie auf **OK**. Warten Sie einige Minuten, bis die Datenbank vor dem Fortfahren erfolgreich bereitgestellt werden.

<!-- URLs. -->
[Azure-Portal]: https://portal.azure.com/
