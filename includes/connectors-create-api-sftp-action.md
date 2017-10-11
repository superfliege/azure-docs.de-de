Nun, dass Sie einen Trigger hinzugefügt haben, interessante Zeit eine Aktion mit den Daten, die vom Trigger generiert wird. Führen Sie diese Schritte zum Hinzufügen einer der **SFTP - Ordner extrahieren** Aktion. Diese Aktion wird der Inhalt einer Datei extrahiert, wenn die definierten Bedingungen erfüllt sind. 

So konfigurieren Sie diesen Vorgang müssen Sie die folgenden Informationen bereitstellen. Sie werden feststellen, dass er einfach zu verwendende Daten als Eingabe für einige der Eigenschaften für die neue Datei durch den Trigger generiert:

| SFTP - Eigenschaft extrahieren | Beschreibung |
| --- | --- |
| Pfad zur Quelldatei Archiv |Dies ist der Pfad für die Datei extrahiert wird. Sie können wählen Sie eines der Token aus einer früheren Aktivität oder durchsuchen die SFTP-Server, um den Dateipfad zu suchen. |
| Der Zielordnerpfad |Dies ist der Pfad, in dem die entpackten Dateien abgelegt werden. Wählen Sie eine der Token aus einer früheren erreichen Sie, wenn der Zielpfad oder durchsuchen die SFTP-Server, und wählen Sie einen Pfad. |
| Überschreiben? |Gibt an, ob eine Datei mit dem gleichen Namen wie die extrahierte Datei im Ordner Zielpfad gefunden wird, wenn die vorhandene Datei überschrieben werden soll. |

Beginnen wir zunächst die Aktion aus, um die Dateien zu extrahieren, wenn die Bedingung, die zuvor definierten ergibt hinzufügen *"true"*. 

1. Wählen Sie **fügen Sie eine Aktion**.        
   ![Bedingung Abbildung 6 – SFTP Aktion](./media/connectors-create-api-sftp/condition-6.png)   
2. Wählen Sie die **SFTP - Ordner extrahieren** Aktion      
   ![Bedingung Abbildung 7 – SFTP Aktion](./media/connectors-create-api-sftp/condition-7.png)   
3. Wählen Sie **Quelldateipfad für das Archiv**              
   ![Bedingung Abbildung 9 – SFTP Aktion](./media/connectors-create-api-sftp/condition-9.png)   
4. Wählen Sie die **Dateipfad** token. Dies bedeutet, dass Sie den Pfad der Datei, die den Trigger gefunden verwenden werden, als Pfad zur Quelldatei Archiv.           
   ![Bedingung Abbildung 10 – SFTP Aktion](./media/connectors-create-api-sftp/condition-10.png)   
5. Wählen Sie **Zielordnerpfad an**           
   ![Bedingung für die SFTP-Aktion, Bild 11](./media/connectors-create-api-sftp/condition-11.png)   
6. Wählen Sie die **Dateipfad** token. Dies gibt an, das Sie verwenden den Pfad der Datei, die den Trigger gefunden als der Zielpfad für die extrahierten Dateien.   
7. Geben Sie *\ExtractedFile* in der **Zielordnerpfad** Steuerelement. Dazu direkt hinter das File-Pfad-Token in das Ziel Ordner Path-Steuerelement.         
   ![Bedingung Abbildung 12 – SFTP Aktion](./media/connectors-create-api-sftp/condition-12.png)   
8. Geben Sie *"true"* in der **überschreiben?* Steuerelement, um anzugeben, dass vorhandene Dateien überschrieben werden soll, wenn sie den gleichen Namen wie die extrahierten Dateien verfügen.      
   ![Bedingung Abbildung 13 – SFTP Aktion](./media/connectors-create-api-sftp/condition-13.png)   
9. Speichern Sie die Änderungen an den workflow  

