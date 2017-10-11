Nun, dass Sie eine Bedingung hinzugefügt haben, interessante Zeit eine Aktion mit den Daten, die vom Trigger generiert wird. Führen Sie diese Schritte zum Hinzufügen der **Salesforce - Get-Objekt** Aktion. Diese Aktion werden die Daten jedes Mal abzurufen neuen potenziellen Kunden erstellt wird. Sie fügen auch eine zweite Aktion, die die Daten aus dem Salesforce - Get ein Objektaktion senden eine e-Mail mit dem Office 365-Connector verwenden kann.  

So konfigurieren Sie diesen Vorgang müssen Sie die folgenden Informationen bereitstellen. Sie werden feststellen, dass er einfach zu verwendende Daten als Eingabe für einige der Eigenschaften für die neue Datei durch den Trigger generiert:

| Erstellen Sie File (Eigenschaft) | Beschreibung |
| --- | --- |
| Objekttyp |Dies ist der Typ des Salesforce-Objekt, das Sie interessiert sind. Beispiele sind Lead "," Konto "," usw. an. |
| Objekt-ID |Dies stellt einen Bezeichner für das Objekt dar. |

1. Wählen Sie **fügen Sie eine Aktion** Link. Dieser wird geöffnet, das Suchfeld, in dem Sie für jede Aktion suchen können, übernehmen möchten. In diesem Beispiel sind Salesforce Aktionen von Interesse sind.      
   ![Abbildung 1 – Salesforce Aktion](./media/connectors-create-api-salesforce/action-1.png)  
2. Geben Sie *Salesforce* , für die Aktionen im Zusammenhang mit Salesforce gesucht werden soll.
3. Wählen Sie **Salesforce - Get-Objekt** als die zu ergreifende Maßnahme.   **Hinweis**: Sie werden aufgefordert, das Autorisieren der Logik-app auf Ihrem Salesforce-Konto zugreifen, wenn Sie nicht bereits erledigt haben.    
   ![Abbildung 2 – Salesforce Aktion](./media/connectors-create-api-salesforce/action-2.png)    
4. Die **Get Objekt** steuern wird geöffnet.  
5. Wählen Sie *führen* als Objekttyp.
6. Wählen Sie die **Objekt-ID** Steuerelement.
7. Wählen Sie **...**  um die Liste mit Token zu erweitern, die als Eingabe für Aktionen verwendet werden kann.       
   ![Abbildung 3 – Salesforce Aktion](./media/connectors-create-api-salesforce/action-3.png)    
8. Wählen Sie **Kennung** steuern wird geöffnet.   
   ![Abbildung 4 – Salesforce Aktion](./media/connectors-create-api-salesforce/action-4.png)     
9. Beachten Sie, dass die Lead-ID-Token jetzt im Objekt-ID-Steuerelement ist, der angibt, dass die Get-Objekt-Aktion für einen potenziellen Kunden mit der ID suchen, die die potenziellen Kunden-ID der potenziellen Kunden gleich ist, die diese Logik-app ausgelöst.  
   ![Abbildung 5 – Salesforce Aktion](./media/connectors-create-api-salesforce/action-5.png)  
10. Speichern Sie Ihre Arbeit. Das ist alles, die Get-Objekt-Aktion der Logik-app hinzugefügt haben. Das Get-Objekt-Steuerelement sollte wie folgt aussehen:    
    ![Abbildung 6 – Salesforce Aktion](./media/connectors-create-api-salesforce/action-6.png)  

Nun, dass Sie eine Aktion zum Abrufen eines potenziellen Kunden hinzugefügt haben, empfiehlt es sich, die neu erstellte Lead relevante Aufgaben übernehmen. In einem Unternehmen möchte Sie per e-Mail an einer Verteilerliste zu benachrichtigen, dass es sich bei einer neuen potenziellen Kunden erstellt wurde. Ermöglicht die Verwendung des Connectors für Office 365 an um eine e-Mail mit einigen der relevanten Informationen aus der neuen Lead-Objekts in Salesforce zu senden.  

1. Wählen Sie **fügen Sie eine Aktion** Geben Sie dann *e-Mail* in das Steuerelement für die Suche. Die Aktionen an, die im Zusammenhang mit senden und Empfangen von e-Mails gefiltert.  
2. Wählen Sie die **Office 365 Outlook - senden eine e-Mail** Listenelement. Wenn Sie bereits erstellt haben eine *Verbindung* mit Ihrem Office 365-Konto werden Sie aufgefordert, Ihre Office 365-Anmeldeinformationen, um ihn jetzt erstellen einzugeben. Wenn Sie fertig sind, die **per E-Mail** steuern wird geöffnet.        
   ![Abbildung 7 – Salesforce Aktion](./media/connectors-create-api-salesforce/action-7.png)  
3. Geben Sie die e-Mail-Adresse, die Sie das Senden von e-Mails, die in möchten der **auf** Steuerelement.
4. In der **Betreff** steuern, geben Sie *erstellt neue führen* – wählen Sie dann die *Unternehmen* token. Dies zeigt der *Unternehmen* Feld aus dem neuen potenziellen Kunden in Salesforce erstellt.  
5. In der **Text** -Steuerelement, können Sie eine der Token aus dem neuen Lead Objekt auswählen und Sie können auch den Text im Hauptteil der e-Mail anzeigen möchten eingeben. Beispiel:  
   ![Abbildung 8 – Salesforce Aktion](./media/connectors-create-api-salesforce/action-8.png)   
6. Speichern Sie den Workflow ein.  

Das wars. Die Logik-app ist nun abgeschlossen.  

Sie können jetzt Ihre Logik-app testen: in Salesforce, erstellen Sie einen neuen potenziellen Kunden, die die Bedingung erfüllt Sie erstellt haben.  Wenn Sie diese exemplarische Vorgehensweise vollständig befolgt haben, erstellen Sie einfach einen potenziellen Kunden mit einer e-Mail-Adresse, die enthält *amazon.com* darin. Nach wenigen Sekunden Ihrer Logik-app sollte ausgelöst werden, und die Ergebnisse möglicherweise etwa wie folgt aussehen:  
![Abbildung 9 – Salesforce Aktion](./media/connectors-create-api-salesforce/action-9.png)  

