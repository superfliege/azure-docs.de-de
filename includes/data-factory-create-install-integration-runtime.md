## <a name="create-a-self-hosted-ir"></a>Erstellen einer selbstgehosteten IR

In diesem Abschnitt erstellen Sie eine selbstgehostete Integration Runtime und ordnen sie einem lokalen Computer mit der SQL Server-Datenbank zu. Die selbstgehostete Integration Runtime ist die Komponente, die Daten aus SQL Server auf Ihrem Computer in den Azure-Blobspeicher kopiert. 

1. Erstellen Sie eine Variable für den Namen der Integration Runtime. Verwenden Sie einen eindeutigen Namen, und notieren Sie ihn. Sie benötigen ihn später in diesem Tutorial. 

    ```powershell
   $integrationRuntimeName = "ADFTutorialIR"
    ```
1. Erstellen Sie eine selbstgehostete Integration Runtime. 

   ```powershell
   Set-AzureRmDataFactoryV2IntegrationRuntime -Name $integrationRuntimeName -Type SelfHosted -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName
   ```

   Hier ist die Beispielausgabe:

   ```json
    Id                : /subscriptions/<subscription ID>/resourceGroups/ADFTutorialResourceGroup/providers/Microsoft.DataFactory/factories/onpremdf0914/integrationruntimes/myonpremirsp0914
    Type              : SelfHosted
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Name              : myonpremirsp0914
    Description       :
    ```
 

2. Führen Sie den folgenden Befehl aus, um den Status der erstellten Integration Runtime abzurufen. Vergewissern Sie sich, dass der Wert der Eigenschaft **State** auf **NeedRegistration** festgelegt ist. 

   ```powershell
   Get-AzureRmDataFactoryV2IntegrationRuntime -name $integrationRuntimeName -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -Status
   ```

   Hier ist die Beispielausgabe:

   ```json
   Nodes                     : {}
   CreateTime                : 9/14/2017 10:01:21 AM
   InternalChannelEncryption :
   Version                   :
   Capabilities              : {}
   ScheduledUpdateDate       :
   UpdateDelayOffset         :
   LocalTimeZoneOffset       :
   AutoUpdate                :
   ServiceUrls               : {eu.frontend.clouddatahub.net, *.servicebus.windows.net}
   ResourceGroupName         : <ResourceGroup name>
   DataFactoryName           : <DataFactory name>
   Name                      : <Integration Runtime name>
   State                     : NeedRegistration
   ```

3. Führen Sie den folgenden Befehl aus, um **Authentifizierungsschlüssel** abzurufen, mit denen Sie die selbstgehostete Integration Runtime beim Data Factory-Dienst in der Cloud registrieren können. 

   ```powershell
   Get-AzureRmDataFactoryV2IntegrationRuntimeKey -Name $integrationRuntimeName -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName | ConvertTo-Json
   ```

   Hier ist die Beispielausgabe:

   ```json
   {
       "AuthKey1":  "IR@0000000000-0000-0000-0000-000000000000@xy0@xy@xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx=",
       "AuthKey2":  "IR@0000000000-0000-0000-0000-000000000000@xy0@xy@yyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy="
   }
   ```    
4. Kopieren Sie einen der Schlüssel (ohne doppelte Anführungszeichen) zum Registrieren der selbstgehosteten Integration Runtime, die Sie im nächsten Schritt auf Ihrem Computer installieren.  

## <a name="install-integration-runtime"></a>Installieren der Integration Runtime
1. Falls **Microsoft Integration Runtime** bereits auf Ihrem Computer installiert ist, deinstallieren Sie es über **Programme hinzufügen oder entfernen**. 
2. Führen Sie den [Download](https://www.microsoft.com/download/details.aspx?id=39717) der selbstgehosteten Integration Runtime auf einen lokalen Windows-Computer durch, und führen Sie die Installation aus. 
3. Klicken Sie auf der **Willkommensseite des Einrichtungsassistenten für die Microsoft Integration Runtime** auf **Weiter**.  
4. Stimmen Sie auf der Seite mit dem **Endbenutzer-Lizenzvertrag** den Bedingungen bzw. dem Lizenzvertrag zu, und klicken Sie auf **Weiter**. 
5. Klicken Sie auf der Seite **Zielordner** auf **Weiter**. 
6. Klicken Sie unter **Ready to install Microsoft Integration Runtime** (Bereit für Installation der Microsoft Integration Runtime) auf **Installieren**. 
7. Klicken Sie auf **OK**, wenn eine Warnmeldung mit dem Hinweis angezeigt wird, dass der zu konfigurierende Computer in den Energiesparmodus oder den Ruhezustand versetzt wird, falls er nicht verwendet wird. 
8. Wenn das Fenster **Energieoptionen** angezeigt wird, schließen Sie es, und wechseln Sie zum Fenster für das Setup. 
9. Klicken Sie auf der Seite **Completed the Microsoft Integration Runtime Setup Wizard** (Einrichtungsassistent für Microsoft Integration Runtime abgeschlossen) auf **Fertig stellen**.
10. Fügen Sie auf der Seite **Integrationslaufzeit (selbstgehostet) registrieren** den Schlüssel ein, den Sie im vorherigen Abschnitt gespeichert haben, und klicken Sie auf **Registrieren**. 

   ![Registrieren der Integration Runtime](media/data-factory-create-install-integration-runtime/register-integration-runtime.png)
2. Folgende Meldung wird angezeigt, wenn die selbstgehostete Integration Runtime erfolgreich registriert wurde:

   ![Erfolgreich registriert](media/data-factory-create-install-integration-runtime/registered-successfully.png)

3. Klicken Sie auf der Seite **Neuer Knoten der Integrationslaufzeit (selbstgehostet)** auf **Weiter**. 

    ![Seite „Neuer Knoten der Integrationslaufzeit“](media/data-factory-create-install-integration-runtime/new-integration-runtime-node-page.png)
4. Klicken Sie unter **Intranetkommunikationskanal** auf **Überspringen**. Sie können eine TLS/SSL-Zertifizierung wählen, um in einer Integration Runtime-Umgebung mit mehreren Knoten die Kommunikation zwischen Knoten zu schützen. 

    ![Seite „Intranetkommunikationskanal“](media/data-factory-create-install-integration-runtime/intranet-communication-channel-page.png)
5. Klicken Sie auf der Seite **Integrationslaufzeit (selbstgehostet) registrieren** auf **Konfigurations-Manager starten**. 
6. Folgende Seite wird angezeigt, wenn der Knoten mit dem Clouddienst verbunden ist:

   ![Knoten ist verbunden](media/data-factory-create-install-integration-runtime/node-is-connected.png)
7. Testen Sie nun die Verbindung mit Ihrer SQL Server-Datenbank.

    ![Registerkarte „Diagnose“](media/data-factory-create-install-integration-runtime/config-manager-diagnostics-tab.png)   

    - Wechseln Sie im Fenster **Konfigurations-Manager** zur Registerkarte **Diagnose**.
    - Wählen Sie für **Datenquellentyp** die Option **SqlServer**.
    - Geben Sie den **Servernamen** ein.
    - Geben Sie den **Datenbanknamen** ein. 
    - Wählen Sie den **Authentifizierungsmodus** aus. 
    - Geben Sie einen **Benutzernamen** ein. 
    - Geben Sie das **Kennwort** für den Benutzernamen ein.
    - Klicken Sie auf **Test**, um zu überprüfen, ob die Integration Runtime eine Verbindung mit SQL Server herstellen kann. Wenn die Verbindung erfolgreich hergestellt wurde, wird ein grünes Häkchen angezeigt. Andernfalls wird eine Fehlermeldung angezeigt. Beheben Sie alle Probleme, und stellen Sie sicher, dass die Integration Runtime eine Verbindung mit SQL Server herstellen kann.    

    > [!NOTE]
    > Notieren Sie sich diese Werte (Authentifizierungstyp, Server, Datenbank, Benutzer, Kennwort). Sie benötigen sie später in diesem Tutorial. 
    
