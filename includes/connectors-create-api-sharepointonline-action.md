Nun, dass Sie einen Trigger hinzugefügt haben, interessante Zeit eine Aktion mit den Daten, die vom Trigger generiert wird. Führen Sie diese Schritte zum Hinzufügen der **SharePoint Online - Datei erstellen** Aktion. Diese Aktion wird eine Datei in SharePoint Online jedes Mal erstellt das neue Element Trigger auslöst. 

So konfigurieren Sie diesen Vorgang müssen Sie die folgenden Informationen bereitstellen. Wie Sie diese Informationen bereitgestellt, bemerken Sie, dass es einfach zu verwendende Daten als Eingabe für einige der Eigenschaften für die neue Datei durch den Trigger generiert:

| Erstellen Sie File (Eigenschaft) | Beschreibung |
| --- | --- |
| Website-URL |Dies ist die URL der SharePoint Online-Website, in dem Sie die neue Datei erstellen möchten. Wählen Sie den Standort aus der Liste angezeigt. |
| Ordnerpfad |Dies ist der Ordner (auf der Website-URL, die im vorherigen Schritt ausgewählt), in dem die neue Datei abgelegt wird. Suchen nach, und wählen Sie den Ordner. |
| Dateiname |Dies ist der Name der Datei erstellt wird. |
| Dateiinhalt |Der Inhalt, der in die Datei geschrieben wird. |

1. Wählen Sie **+ neuen Schritt** die Aktion hinzufügen.  
   ![Abbildung 1 – SharePoint online Aktion](./media/connectors-create-api-sharepointonline/action-1.png)  
2. Wählen Sie die **fügen Sie eine Aktion** Link. Dieser wird geöffnet, das Suchfeld, in dem Sie für jede Aktion suchen können, übernehmen möchten. In diesem Beispiel sind SharePoint-Aktionen von Interesse sind.    
   ![Abbildung 2 – SharePoint online Aktion](./media/connectors-create-api-sharepointonline/action-2.png)    
3. Geben Sie *Sharepoint* , für die Aktionen im Zusammenhang mit SharePoint gesucht werden soll.
4. Wählen Sie **SharePoint Online - Datei erstellen** als die zu ergreifende Maßnahme.   **Hinweis**: Sie werden aufgefordert, das Autorisieren der Logik-app auf Ihr Konto SharePoint zuzugreifen, wenn Sie eine Verbindung mit SharePoint Online nicht zuvor erstellt haben.    
   ![Abbildung 3 – SharePoint online Aktion](./media/connectors-create-api-sharepointonline/action-3.png)    
5. Die **Datei erstellen** steuern wird geöffnet.   
   ![Abbildung 4 – SharePoint online Aktion](./media/connectors-create-api-sharepointonline/action-4.png)     
6. Wählen Sie **Website-URL** und suchen Sie den Standort zu finden, wo Sie die Datei zu erstellen möchten.     
   ![Abbildung 5 – SharePoint online Aktion](./media/connectors-create-api-sharepointonline/action-5.png)  
7. Wählen Sie **Ordnerpfad** und navigieren Sie zu dem Ordner zu suchen, in dem die neue Datei abgelegt wird.  
   ![Abbildung 6 – SharePoint online Aktion](./media/connectors-create-api-sharepointonline/action-6.png)  
8. Wählen Sie die **Dateiname** steuern, und geben Sie den Namen der Datei, die Sie erstellen möchten. Hier, Sie können den Dateinamen direkt eingeben, oder können Sie die Eigenschaften aus den Trigger, den Sie zuvor erstellt haben. Hierzu wählen Eigenschaften aus der Liste der **Ausgaben von der Erstellung eines neuen Elements**. Diese Liste ist die einzige Anzeige nach der Auswahl der **Dateiname** Steuerelement. In diesem Walkthough ausgewählt ich ID (die ID des neuen Listenelements) als den Namen der Datei erstellt wird, indem Sie die **SharePoint Online - Datei erstellen** Aktion.    
   ![Abbildung 7 – SharePoint online Aktion](./media/connectors-create-api-sharepointonline/action-7.png)  
9. Wählen Sie die **Dateiinhalt** steuern, und geben Sie den Inhalt, der die Datei geschrieben wird, die erstellt werden soll. Beachten Sie, dass Sie eine der Eigenschaften aus den Trigger verwenden können, die Sie zuvor erstellt haben, für den Dateiinhalt. Wählen Sie einfach die Eigenschaften aus der Liste angezeigt. Alternativ können Sie eingeben der **Dateiinhalt** Text direkt in das Steuerelement. In diesem Beispiel ich einige Eigenschaften ausgewählt und Leerzeichen und ein Bindestrich zwischen jeder Eigenschaft hinzugefügt.        
   ![Abbildung 8 – SharePoint online Aktion](./media/connectors-create-api-sharepointonline/action-8.png)  
10. Speichern Sie die Änderungen an den workflow  
11. Herzlichen Glückwunsch, Sie verfügen nun über eine voll funktionsfähige Logik-app, die ausgelöst wird, wenn eine SharePoint Online-Liste ein neues Element hinzugefügt wird. Die app wird dann eine Datei, die mithilfe einiger der Eigenschaften auf das neue Listenelement erstellen.  Sie können es jetzt testen, durch Erstellen eines neuen Elements in der SharePoint-Liste. 

