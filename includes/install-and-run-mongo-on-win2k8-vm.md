Führen Sie diese Schritte zum Installieren und Ausführen von MongoDB auf einem virtuellen Computer unter Windows Server.

> [!IMPORTANT]
> MongoDB-Sicherheitsfunktionen, wie beispielsweise Authentifizierung und die Bindung, die IP-Adresse werden nicht standardmäßig aktiviert. Sicherheitsfunktionen sollte vor der Bereitstellung von MongoDB in einer produktiven Umgebung aktiviert werden.  Weitere Informationen finden Sie unter [Sicherheit und Authentifizierung](http://www.mongodb.org/display/DOCS/Security+and+Authentication).
>
>

1. Nachdem Sie eine Verbindung mit dem virtuellen Computer mithilfe von Remotedesktop hergestellt haben, öffnen Sie Internet Explorer über die **starten** Menü auf der virtuellen Maschine.
2. Wählen Sie die **Tools** Schaltfläche in der oberen rechten Ecke.  In **Internetoptionen**, wählen die **Sicherheit** Registerkarte, und wählen Sie dann die **vertrauenswürdige Sites** Symbol, und klicken Sie dann auf die **Sites** Schaltfläche. Hinzufügen *https://\*. mongodb.org* zur Liste der vertrauenswürdigen Sites.
3. Wechseln Sie zu [Downloads – MongoDB](https://www.mongodb.com/download-center#community).
4. Suchen der **aktuelle stabile Version** von **Community Server**, wählen Sie die neueste **64-Bit-** Version in der Windows-Spalte. Laden Sie herunter, und führen Sie das MSI-Installationsprogramm.
5. MongoDB wird in der Regel unter c:\Programme\Microsoft Files\MongoDB installiert. Umgebungsvariablen auf dem Desktop suchen Sie, und fügen Sie den MongoDB-Binärdateien-Pfad der PATH-Variable. Beispielsweise können Sie die Binärdateien am c:\Programme\Microsoft Files\MongoDB\Server\3.4\bin auf Ihrem Computer finden.
6. Erstellen von MongoDB Daten- und Protokollverzeichnisse auf dem Datenträger (beispielsweise Laufwerk **F:**) Sie in den vorherigen Schritten erstellt haben. Von **starten**Option **Eingabeaufforderung** um ein Eingabeaufforderungsfenster zu öffnen.  Type:

        C:\> F:
        F:\> mkdir \MongoData
        F:\> mkdir \MongoLogs
7. Führen Sie die Datenbank ausführen:

        F:\> C:
        C:\> mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log

    Alle Meldungen werden weitergeleitet, um die *F:\MongoLogs\mongolog.log* wie mongod.exe Server beginnt und Journaldateien reserviert. Dauert möglicherweise mehrere Minuten für MongoDB die Journaldateien vorab zugewiesen werden soll, und starten die Überwachung auf Verbindungen. Der Eingabeaufforderung bleibt konzentriert sich auf diese Aufgabe, während die MongoDB-Instanz ausgeführt wird.
8. Öffnen Sie zum Starten der administrativen MongoDB-Shell aus einem anderen Befehlsfenster **starten** und geben Sie die folgenden Befehle:

        C:\> cd \my_mongo_dir\bin  
        C:\my_mongo_dir\bin> mongo  
        >db  
        test
        > db.foo.insert( { a : 1 } )  
        > db.foo.find()  
        { _id : ..., a : 1 }  
        > show dbs  
        ...  
        > show collections  
        ...  
        > help  

    Die Datenbank wird von der INSERT-Anweisung erstellt.
9. Alternativ können Sie mongod.exe als Dienst installieren:

        C:\> mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log --logappend  --install

    Ein Dienst installiert ist mit dem Namen MongoDB mit einer Beschreibung des "Mongo DB". Die `--logpath` Option muss verwendet werden, um eine Protokolldatei anzugeben, da es sich bei der ausgeführte Dienst kein Befehlsfenster, das die Ausgabe angezeigt.  Die `--logappend` Option gibt an, dass ein Neustart des Diensts bewirkt, dass die Ausgabe, um die vorhandene Protokolldatei angefügt werden soll.  Die `--dbpath` Option gibt den Speicherort des Datenverzeichnisses. Weitere dienstbezogene Befehlszeilenoptionen finden Sie unter [Befehlszeilenoptionen dienstbezogene][MongoWindowsSvcOptions].

    Um den Dienst zu starten, führen Sie diesen Befehl aus:

        C:\> net start MongoDB
10. Jetzt, wo MongoDB installiert ist und ausgeführt wird, müssen Sie einen Port in der Windows-Firewall öffnen, damit Sie MongoDB herstellen können.  Aus der **starten** klicken Sie im Menü **Verwaltung** und dann **Windows-Firewall mit erweiterter Sicherheit**.
11. eine) Wählen Sie klicken Sie im linken Bereich **eingehende Regeln**.  In der **Aktionen** auf der rechten Seite Wählen Sie im Bereich **neue Regel...** .

    ![Windows-Firewall][Image1]

    (b) in der **Assistenten für neue eingehende Regel**Option **Port** , und klicken Sie dann auf **Weiter**.

    ![Windows-Firewall][Image2]

    c) Wählen Sie **TCP** und dann **bestimmte lokale Ports**.  Geben Sie einen Port, der "27017" (Standardport MongoDB lauscht), und klicken Sie auf **Weiter**.

    ![Windows-Firewall][Image3]

    d) Wählen Sie **Verbindung zulassen** , und klicken Sie auf **Weiter**.

    ![Windows-Firewall][Image4]

    e) klicken Sie auf **Weiter** erneut aus.

    ![Windows-Firewall][Image5]

    f) Geben Sie einen Namen für die Regel ein, z. B. "MongoPort", und klicken Sie auf **Fertig stellen**.

    ![Windows-Firewall][Image6]

12. Wenn Sie keinen Endpunkt für MongoDB beim Konfigurieren Sie den virtuellen Computer erstellt haben, können Sie dies jetzt tun. Sie benötigen die Firewall-Regel und den Endpunkt, um eine Remoteverbindung mit MongoDB herstellen zu können.

  Klicken Sie im Azure-Portal auf **virtuelle Computer (klassisch)**, klicken Sie auf den Namen des neuen virtuellen Computer aus, und klicken Sie dann auf **Endpunkte**.

    ![Endpunkte][Image7]

13. Klicken Sie auf **Hinzufügen**.

14. Fügen Sie einen Endpunkt mit dem Namen "Mongo", Protokoll **TCP**, und die beiden **öffentlichen** und **Private** Ports auf "27017" festgelegt. Öffnen diesen Port ermöglicht MongoDB Remote zugegriffen werden.

    ![Endpunkte][Image9]

> [!NOTE]
> Der Port 27017 ist der Standardport von MongoDB verwendet. Sie können diese Standardport ändern, indem die `--port` Parameter beim Starten des Servers mongod.exe. Stellen Sie sicher, die gleiche Portnummer in der Firewall und den Endpunkt "Mongo" in der zuvor beschriebenen Anweisungen zu erteilen.
>
>

[MongoDownloads]: http://www.mongodb.org/downloads

[MongoWindowsSvcOptions]: http://www.mongodb.org/display/DOCS/Windows+Service


[Image1]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall1.png
[Image2]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall2.png
[Image3]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall3.png
[Image4]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall4.png
[Image5]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall5.png
[Image6]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall6.png
[Image7]: ./media/install-and-run-mongo-on-win2k8-vm/menusendpointadd.png
<!-- Removed 03/08/2017. Not in new portal. -->
<!-- [Image8]: ./media/install-and-run-mongo-on-win2k8-vm/WinVmAddEndpoint2.png
-->
[Image9]: ./media/install-and-run-mongo-on-win2k8-vm/newendpointdetails.png
