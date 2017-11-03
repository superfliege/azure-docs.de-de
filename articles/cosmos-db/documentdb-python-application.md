---
title: "Python Flask-Webanwendungstutorial für Azure Cosmos DB | Microsoft-Dokumentation"
description: "Datenbanktutorial zur Verwendung von Azure Cosmos DB zum Speichern von Daten aus einer in Azure gehosteten Python Flask-Webanwendung und zum Zugreifen auf diese Daten. Finden Sie Lösungen zur Anwendungsentwicklung."
keywords: Anwendungsentwicklung, Python Flask, Python Webanwendung, Python Webentwicklung
services: cosmos-db
documentationcenter: python
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 20ebec18-67c2-4988-a760-be7c30cfb745
ms.service: cosmos-db
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 10/17/2017
ms.author: mimig
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0629b0aa7e7981cf04330b407604e4612a58b168
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2017
---
# <a name="build-a-python-flask-web-application-using-azure-cosmos-db"></a>Erstellen einer Python Flask-Webanwendung mithilfe von Azure Cosmos DB
> [!div class="op_single_selector"]
> * [.NET](documentdb-dotnet-application.md)
> * [Node.js](documentdb-nodejs-application.md)
> * [Java](documentdb-java-application.md)
> * [Python](documentdb-python-application.md)
> 
> 

In diesem Tutorial erfahren Sie, wie Sie mithilfe von Azure Cosmos DB Daten aus einer in Azure App Service gehosteten Python Flask-Web-Anwendung speichern und abrufen. Dieses Tutorial setzt gewisse Erfahrung mit der Verwendung von Python und Azure-Websites voraus.

In diesem Datenbanklernprogramm wird Folgendes behandelt:

1. Erstellen und Bereitstellen eines Azure Cosmos DB-Kontos.
2. Erstellen einer Python Flask-Anwendung
3. Herstellen einer Verbindung mit Azure Cosmos DB über eine Webanwendung und anschließendes Verwenden von Azure Cosmos DB.
4. Bereitstellen der Webanwendung für Azure App Service.

Im Verlauf dieses Lernprogramms erstellen Sie eine einfache Wahlanwendung, über die Sie für eine Wahl abstimmen können.

![Screenshot der in diesem Datenbanktutorial erstellten Abstimmungsanwendung](./media/documentdb-python-application/cosmos-db-pythonr-run-application.png)

## <a name="database-tutorial-prerequisites"></a>Voraussetzungen für das Datenbanklernprogramm
Bevor Sie diesen Artikel durcharbeiten, sollten Sie sicherstellen, dass Folgendes installiert ist:

* [Ein Azure-Abonnement](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [Visual Studio 2017]((https://www.visualstudio.com/downloads/)) mit aktivierter **Azure-Entwicklung** und **Python-Entwicklung**. Sie können überprüfen, ob diese erforderlichen Komponenten installiert sind, und sie installieren, indem Sie den **Visual Studio-Installer** lokal öffnen.   
* [Microsoft Azure SDK für Python 2.7](https://azure.microsoft.com/downloads/) 
* [Python 2.7](https://www.python.org/downloads/windows/). Sie können die 32-Bit- oder 64-Bit-Installation verwenden.

> [!IMPORTANT]
> Wenn Sie Python 2.7 zum ersten Mal installieren, stellen Sie sicher, dass Sie auf dem Bildschirm zum Anpassen von Python 2.7.13 die Option **Add python.exe to Path** (python.exe zum Pfad hinzufügen) auswählen.
> 
> ![Screenshot des Bildschirms zum Anpassen von Python 2.7.11, auf dem Sie „Add python.exe to Path“ auswählen müssen.](./media/documentdb-python-application/cosmos-db-python-install.png)
> 
> 

* [Microsoft Visual C++-Compiler für Python 2.7](https://www.microsoft.com/en-us/download/details.aspx?id=44266)

## <a name="step-1-create-an-azure-cosmos-db-database-account"></a>Schritt 1: Erstellen eines Azure Cosmos DB-Datenbankkontos
Wir beginnen, indem wir ein Azure Cosmos DB-Konto erstellen. Falls Sie bereits ein Konto besitzen oder den Azure Cosmos DB-Emulator für dieses Tutorial verwenden, können Sie mit [Schritt 2: Erstellen einer neuen Python Flask-Webanwendung](#step-2-create-a-new-python-flask-web-application) fortfahren.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

<br/>
Im Folgenden wird erläutert, wie Sie eine Python Flask-Webanwendung von Grund auf neu erstellen.

## <a name="step-2-create-a-new-python-flask-web-application"></a>Schritt 2: Erstellen einer neuen Python Flask-Webanwendung
1. Zeigen Sie in Visual Studio im Menü **Datei** auf **Neu**, und klicken Sie dann auf **Projekt**.
   
    Das Dialogfeld **Neues Projekt** wird angezeigt.
2. Erweitern Sie im linken Bereich **Vorlagen** und **Python**, und klicken Sie dann auf **Web**. 
3. Wählen Sie im mittleren Bereich **Flask-Webprojekt**, geben Sie in das Feld **Name** den Namen **tutorial** ein, und klicken Sie dann auf **OK**. Denken Sie daran, dass Python-Paketnamen in Kleinbuchstaben geschrieben werden sollten, wie im [Style Guide for Python Code](https://www.python.org/dev/peps/pep-0008/#package-and-module-names)beschrieben.
   
    Möglicherweise kennen Sie Python Flask noch nicht. Dies ist ein Entwicklungsframework für Webanwendungen, das Sie beim schnelleren Erstellen von Webanwendungen in Python unterstützt.
   
    ![Screenshot des Fensters "Neues Projekt" in Visual Studio: Links ist Python hervorgehoben, das Python Flask-Webprojekt ist in der Mitte ausgewählt, und der Name "tutorial" steht im Feld "Name".](./media/documentdb-python-application/image9.png)
4. Klicken Sie im Fenster **Python-Tools für Visual Studio** auf **In einer virtuellen Umgebung installieren**. 
   
    ![Screenshot des Datenbanklernprogramms – Python-Tools für Visual Studio-Fenster](./media/documentdb-python-application/python-install-virtual-environment.png)
5. Aktivieren Sie im Fenster **Virtuelle Umgebung hinzufügen** Python 2.7 oder Python 3.5 im Interpreterauswahlfeld, akzeptieren Sie die anderen Standardwerte, und klicken Sie dann auf **Erstellen**. Dadurch wird die erforderliche virtuelle Python-Umgebung für das Projekt eingerichtet.
   
    ![Screenshot des Datenbanklernprogramms – Python-Tools für Visual Studio-Fenster](./media/documentdb-python-application/image10_A.png)
   
    Im Ausgabefenster wird `Successfully installed Flask-0.10.1 Jinja2-2.8 MarkupSafe-0.23 Werkzeug-0.11.5 itsdangerous-0.24 'requirements.txt' was installed successfully.` angezeigt, wenn die Umgebung erfolgreich installiert wurde.

## <a name="step-3-modify-the-python-flask-web-application"></a>Schritt 3: Ändern der Python Flask-Webanwendung
### <a name="add-the-python-flask-packages-to-your-project"></a>Hinzufügen der Python Flask-Pakete zum Projekt
Nachdem das Projekt eingerichtet ist, müssen Sie Ihrem Projekt die benötigten Flask-Pakete hinzufügen, z. B. „pydocumentdb“, das Python-Paket für DocumentDB.

1. Öffnen Sie im Projektmappen-Explorer die Datei **requirements.txt** , und ersetzen Sie den Inhalt durch Folgendes:
   
        flask==0.9
        flask-mail==0.7.6
        sqlalchemy==0.7.9
        flask-sqlalchemy==0.16
        sqlalchemy-migrate==0.7.2
        flask-whooshalchemy==0.55a
        flask-wtf==0.8.4
        pytz==2013b
        flask-babel==0.8
        flup
        pydocumentdb>=1.0.0
2. Speichern Sie die Datei **requirements.txt** . 
3. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf **env**, und klicken Sie auf **Aus requirements.txt installieren**.
   
    ![Screenshot mit „env“ (Python 2.7) ausgewählt, „Installieren aus requirements.txt“ in der Liste markiert](./media/documentdb-python-application/cosmos-db-python-install-from-requirements.png)
   
    Nach der erfolgreichen Installation zeigt das Ausgabefenster Folgendes an:
   
        Successfully installed Babel-2.3.2 Tempita-0.5.2 WTForms-2.1 Whoosh-2.7.4 blinker-1.4 decorator-4.0.9 flask-0.9 flask-babel-0.8 flask-mail-0.7.6 flask-sqlalchemy-0.16 flask-whooshalchemy-0.55a0 flask-wtf-0.8.4 flup-1.0.2 pydocumentdb-1.6.1 pytz-2013b0 speaklater-1.3 sqlalchemy-0.7.9 sqlalchemy-migrate-0.7.2
   
   > [!NOTE]
   > In seltenen Fällen wird eventuell ein Fehler im Ausgabefenster angezeigt. Wenn dies geschieht, prüfen Sie, ob der Fehler mit der Bereinigung zusammenhängt. Gelegentlich misslingt die Bereinigung, aber die Installation verläuft dennoch erfolgreich (führen Sie im Ausgabefenster einen Bildlauf nach oben durch, um dies zu prüfen). Sie können Ihre Installation überprüfen, siehe [Überprüfen der virtuellen Umgebung](#verify-the-virtual-environment). Wenn bei der Installation Fehler aufgetreten sind, die Überprüfung jedoch erfolgreich durchgeführt wurde, können Sie fortfahren.
   > 
   > 

### <a name="verify-the-virtual-environment"></a>Überprüfen der virtuellen Umgebung
Stellen Sie sicher, dass alles einwandfrei installiert ist.

1. Erstellen Sie die Lösung, indem Sie **STRG**+**UMSCHALT**+**B** drücken.
2. Sobald die Erstellung erfolgreich abgeschlossen wurde, starten Sie die Website, indem Sie **F5**drücken. Dadurch werden der Flask-Entwicklungsserver und anschließend der Webbrowser gestartet. Daraufhin sollte die folgende Seite angezeigt werden.
   
    ![Das leere Python Flask-Webentwicklungsprojekt, angezeigt in einem Browser](./media/documentdb-python-application/image12.png)
3. Beenden Sie das Debuggen der Website durch Drücken von **UMSCHALT**+**F5** in Visual Studio.

### <a name="create-database-collection-and-document-definitions"></a>Erstellen der Datenbank-, Sammlungs- und Dokumentdefinitionen
Nun erstellen Sie die Abstimmungsanwendung, indem Sie neue Dateien hinzufügen und andere Dateien aktualisieren.

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **tutorial**, und klicken Sie erst auf **Hinzufügen** und dann auf **Neues Element**. Wählen Sie **Leere Python-Datei** aus, und benennen Sie die Datei **forms.py**.  
2. Fügen Sie der Datei „forms.py“ den folgenden Code hinzu, und speichern Sie die Datei.

```python
from flask.ext.wtf import Form
from wtforms import RadioField

class VoteForm(Form):
    deploy_preference  = RadioField('Deployment Preference', choices=[
        ('Web Site', 'Web Site'),
        ('Cloud Service', 'Cloud Service'),
        ('Virtual Machine', 'Virtual Machine')], default='Web Site')
```


### <a name="add-the-required-imports-to-viewspy"></a>Fügen Sie die erforderlichen Importe zu views.py hinzu.
1. Erweitern Sie im Projektmappen-Explorer den Ordner **tutorial**, und öffnen Sie die Datei **views.py**. 
2. Fügen Sie die folgenden import-Anweisungen am Anfang der Datei **views.py** ein, und speichern Sie die Datei. So werden die Azure Cosmos DB-Pakete für PythonSDK und Flask importiert.
   
    ```python
    from forms import VoteForm
    import config
    import pydocumentdb.document_client as document_client
    ```

### <a name="create-database-collection-and-document"></a>Erstellen von Datenbank, Sammlung und Dokument
* Fügen Sie den folgenden Code am Ende der Datei **views.py**hinzu. Dieser ist für die Erstellung der vom Formular verwendeten Datenbank zuständig. Löschen Sie keinen vorhandenen Code in **views.py**. Fügen Sie diesen am Ende ein.

```python
@app.route('/create')
def create():
    """Renders the contact page."""
    client = document_client.DocumentClient(config.DOCUMENTDB_HOST, {'masterKey': config.DOCUMENTDB_KEY})

    # Attempt to delete the database.  This allows this to be used to recreate as well as create
    try:
        db = next((data for data in client.ReadDatabases() if data['id'] == config.DOCUMENTDB_DATABASE))
        client.DeleteDatabase(db['_self'])
    except:
        pass

    # Create database
    db = client.CreateDatabase({ 'id': config.DOCUMENTDB_DATABASE })

    # Create collection
    collection = client.CreateCollection(db['_self'],{ 'id': config.DOCUMENTDB_COLLECTION })

    # Create document
    document = client.CreateDocument(collection['_self'],
        { 'id': config.DOCUMENTDB_DOCUMENT,
          'Web Site': 0,
          'Cloud Service': 0,
          'Virtual Machine': 0,
          'name': config.DOCUMENTDB_DOCUMENT 
        })

    return render_template(
       'create.html',
        title='Create Page',
        year=datetime.now().year,
        message='You just created a new database, collection, and document.  Your old votes have been deleted')
```


### <a name="read-database-collection-document-and-submit-form"></a>Lesen von Datenbank, Sammlung und Dokument und Übermitteln des Formulars
* Fügen Sie den folgenden Code am Ende der Datei **views.py**hinzu. Dieser Code dient zum Einrichten des Formulars sowie zum Lesen der Datenbank, der Sammlung und des Dokuments. Löschen Sie keinen vorhandenen Code in **views.py**. Fügen Sie diesen am Ende ein.

```python
@app.route('/vote', methods=['GET', 'POST'])
def vote(): 
    form = VoteForm()
    replaced_document ={}
    if form.validate_on_submit(): # is user submitted vote  
        client = document_client.DocumentClient(config.DOCUMENTDB_HOST, {'masterKey': config.DOCUMENTDB_KEY})

        # Read databases and take first since id should not be duplicated.
        db = next((data for data in client.ReadDatabases() if data['id'] == config.DOCUMENTDB_DATABASE))

        # Read collections and take first since id should not be duplicated.
        coll = next((coll for coll in client.ReadCollections(db['_self']) if coll['id'] == config.DOCUMENTDB_COLLECTION))

        # Read documents and take first since id should not be duplicated.
        doc = next((doc for doc in client.ReadDocuments(coll['_self']) if doc['id'] == config.DOCUMENTDB_DOCUMENT))

        # Take the data from the deploy_preference and increment our database
        doc[form.deploy_preference.data] = doc[form.deploy_preference.data] + 1
        replaced_document = client.ReplaceDocument(doc['_self'], doc)

        # Create a model to pass to results.html
        class VoteObject:
            choices = dict()
            total_votes = 0

        vote_object = VoteObject()
        vote_object.choices = {
            "Web Site" : doc['Web Site'],
            "Cloud Service" : doc['Cloud Service'],
            "Virtual Machine" : doc['Virtual Machine']
        }
        vote_object.total_votes = sum(vote_object.choices.values())

        return render_template(
            'results.html', 
            year=datetime.now().year, 
            vote_object = vote_object)

    else :
        return render_template(
            'vote.html', 
            title = 'Vote',
            year=datetime.now().year,
            form = form)
```


### <a name="create-the-html-files"></a>Erstellen der HTML-Dateien
1. Klicken Sie im Projektmappen-Explorer im Ordner **tutorial** mit der rechten Maustaste auf den Ordner **templates**, und klicken Sie auf **Hinzufügen** und dann auf **Neues Element**. 
2. Wählen Sie **HTML-Seite** aus, und geben Sie im Namensfeld den Namen **create.html** ein. 
3. Wiederholen Sie die Schritte 1 und 2, um zwei weitere HTML-Dateien hinzuzufügen: „results.html“ und „vote.html“.
4. Fügen Sie den folgenden Code zur Datei **create.html** in the `<body>` -Element hinzu. Damit wird eine Meldung angezeigt, dass eine neue Datenbank, eine neue Sammlung und ein neues Dokument erstellt wurden.
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>{{ title }}.</h2>
    <h3>{{ message }}</h3>
    <p><a href="{{ url_for('vote') }}" class="btn btn-primary btn-large">Vote &raquo;</a></p>
    {% endblock %}
    ```
5. Fügen Sie der Datei **results.html** im `<body`>-Element den folgenden Code hinzu. Dadurch werden die Wahlergebnisse angezeigt.
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>Results of the vote</h2>
        <br />
   
    {% for choice in vote_object.choices %}
    <div class="row">
        <div class="col-sm-5">{{choice}}</div>
            <div class="col-sm-5">
                <div class="progress">
                    <div class="progress-bar" role="progressbar" aria-valuenow="{{vote_object.choices[choice]}}" aria-valuemin="0" aria-valuemax="{{vote_object.total_votes}}" style="width: {{(vote_object.choices[choice]/vote_object.total_votes)*100}}%;">
                                {{vote_object.choices[choice]}}
                </div>
            </div>
            </div>
    </div>
    {% endfor %}
   
    <br />
    <a class="btn btn-primary" href="{{ url_for('vote') }}">Vote again?</a>
    {% endblock %}
    ```
6. Fügen Sie der Datei **vote.html** im `<body`>-Element den folgenden Code hinzu. Damit wird die Umfrage angezeigt, und es werden die Stimmen angenommen. Beim Registrieren der Stimmen wird die Kontrolle an die Datei „views.py“ übergeben, wo Azure Cosmos DB die abgegebene Stimme erkennt und das Dokument entsprechend anfügt.
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>What is your favorite way to host an application on Azure?</h2>
    <form action="" method="post" name="vote">
        {{form.hidden_tag()}}
            {{form.deploy_preference}}
            <button class="btn btn-primary" type="submit">Vote</button>
    </form>
    {% endblock %}
    ```
7. Ersetzen Sie im Ordner **Vorlagen** den Inhalt von **index.html** durch Folgendes. Dies dient als Startseite für Ihre Anwendung.
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>Python + Azure Cosmos DB Voting Application.</h2>
    <h3>This is a sample Cosmos DB voting application using PyDocumentDB</h3>
    <p><a href="{{ url_for('create') }}" class="btn btn-primary btn-large">Create/Clear the Voting Database &raquo;</a></p>
    <p><a href="{{ url_for('vote') }}" class="btn btn-primary btn-large">Vote &raquo;</a></p>
    {% endblock %}
    ```

### <a name="add-a-configuration-file-and-change-the-initpy"></a>Hinzufügen einer Konfigurationsdatei und Ändern von \_\_init\_\_.py
1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **tutorial**, klicken Sie nacheinander auf **Hinzufügen** und **Neues Element**, wählen Sie **Leere Python-Datei** aus, und benennen Sie die Datei **config.py**. Diese Konfiguration ist für Formulare in Flask erforderlich. Sie können damit auch einen geheimen Schlüssel bereitstellen. Dies ist jedoch für dieses Tutorial nicht erforderlich.
2. Fügen Sie folgenden Code zur Datei „config.py“ hinzu. Sie werden die Werte von **DOCUMENTDB\_HOST** und **DOCUMENTDB\_KEY** im nächsten Schritt ändern.
   
    ```python
    CSRF_ENABLED = True
    SECRET_KEY = 'you-will-never-guess'
   
    DOCUMENTDB_HOST = 'https://YOUR_DOCUMENTDB_NAME.documents.azure.com:443/'
    DOCUMENTDB_KEY = 'YOUR_SECRET_KEY_ENDING_IN_=='
   
    DOCUMENTDB_DATABASE = 'voting database'
    DOCUMENTDB_COLLECTION = 'voting collection'
    DOCUMENTDB_DOCUMENT = 'voting document'
    ```
3. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) zur Seite **Schlüssel**, indem Sie auf **Durchsuchen** und **Azure Cosmos DB-Konten** klicken. Doppelklicken Sie auf den Namen des Kontos, das Sie verwenden möchten, und klicken Sie dann im Bereich **Zusammenfassung** auf die Schaltfläche **Schlüssel**. Kopieren Sie auf der Seite **Schlüssel** den **URI**-Wert, und fügen Sie ihn in der Datei **config.py** als Wert für die **DOCUMENTDB\_HOST**-Eigenschaft ein. 
4. Kopieren Sie im Azure-Portal auf der Seite **Schlüssel** den Wert für **Primärer Schlüssel** oder **Sekundärer Schlüssel**, und fügen Sie ihn in der Datei **config.py** als Wert für die **DOCUMENTDB\_KEY**-Eigenschaft ein.
5. Fügen Sie in der Datei **\_\_init\_\_.py** folgende Zeile ein: 
   
        app.config.from_object('config')
   
    Damit lautet der Inhalt der Datei wie folgt:
   
    ```python
    from flask import Flask
    app = Flask(__name__)
    app.config.from_object('config')
    import tutorial.views
    ```
6. Nachdem Sie alle Dateien hinzugefügt haben, sollte der Projektmappen-Explorer folgendermaßen aussehen:
   
    ![Screenshot des Explorer-Fensters der Visual Studio-Lösung](./media/documentdb-python-application/cosmos-db-python-solution-explorer.png)

## <a name="step-4-run-your-web-application-locally"></a>Schritt 4: Lokales Ausführen der Webanwendung
1. Erstellen Sie die Lösung, indem Sie **STRG**+**UMSCHALT**+**B** drücken.
2. Sobald die Erstellung erfolgreich abgeschlossen wurde, starten Sie die Website, indem Sie **F5**drücken. Auf Ihrem Bildschirm sollte Folgendes angezeigt werden.
   
    ![Screenshot der Python- und Azure Cosmos DB-Abstimmungsanwendung in einem Webbrowser](./media/documentdb-python-application/cosmos-db-pythonr-run-application.png)
3. Klicken Sie auf **Wahldatenbank erstellen/löschen** , um die Datenbank zu erstellen.
   
    ![Screenshot der „Erstellen“-Seite der Webanwendung – Entwicklungsdetails](./media/documentdb-python-application/cosmos-db-python-run-create-page.png)
4. Klicken Sie anschließend auf **Abstimmen** , und wählen Sie Ihre Option aus.
   
    ![Screenshot der Webanwendung mit einem Abstimmungsfrage](./media/documentdb-python-application/cosmos-db-vote.png)
5. Für jede abgegebene Stimme wird der entsprechende Zähler erhöht.
   
    ![Screenshot der Ergebnisse der Abstimmungsseite](./media/documentdb-python-application/cosmos-db-voting-results.png)
6. Beenden Sie das Debuggen des Projekts, indem Sie UMSCHALT+F5 drücken.

## <a name="step-5-deploy-the-web-application-to-azure"></a>Schritt 5: Bereitstellen der Webanwendung in Azure
Jetzt funktioniert die vollständige Anwendung ordnungsgemäß lokal für Azure Cosmos DB, und wir erstellen eine Datei „web.config“, aktualisieren die Dateien auf dem Server, sodass sie mit der lokalen Umgebung übereinstimmen, und zeigen dann die fertig gestellte App in Azure an. Dieses Verfahren gilt ausschließlich für Visual Studio 2017. Wenn Sie eine andere Version von Visual Studio verwenden, lesen Sie [Publishing to Azure App Service](/visualstudio/python/publishing-to-azure.md) (Veröffentlichen in Azure App Service).

1. Klicken Sie im **Projektmappen-Explorer** von Visual Studio mit der rechten Maustaste auf das Projekt, und wählen Sie **Hinzufügen > Neues Element...** aus. Wählen Sie in dem dann angezeigten Dialogfeld die Vorlage **Azure web.config (Fast CGI)** und dann **OK**. Dies erstellt eine `web.config`-Datei in Ihrem Projektstamm. 

2. Ändern Sie den `<system.webServer>`-Abschnitt in `web.config`, damit der Pfad der Python-Installation entspricht. Für Python 2.7 x64 sollte der Eintrag beispielsweise folgendermaßen aussehen:
    
    ```xml
    <system.webServer>
        <handlers>
            <add name="PythonHandler" path="*" verb="*" modules="FastCgiModule" scriptProcessor="D:\home\Python27\python.exe|D:\home\Python27\wfastcgi.py" resourceType="Unspecified" requireAccess="Script"/>
        </handlers>
    </system.webServer>
    ```

3. Legen Sie den `WSGI_HANDLER`-Eintrag in `web.config` als `tutorial.app` fest, damit er mit Ihrem Projektnamen übereinstimmt. 

    ```xml
    <!-- Flask apps only: change the project name to match your app -->
    <add key="WSGI_HANDLER" value="tutorial.app"/>
    ```

4. Erweitern Sie im **Projektmappen-Explorer** von Visual Studio den Ordner **tutorial**, klicken Sie mit der rechten Maustaste auf den `static`-Ordner, wählen Sie **Hinzufügen > Neues Element...**, wählen Sie die Vorlage „Azure static files web.config“ und dann **OK**. Diese Aktion erstellt eine andere `web.config` im `static`-Ordner, die für diesen Ordner die Python-Verarbeitung deaktiviert. Diese Konfiguration sendet Anforderungen von statischen Dateien an den Standardwebserver, statt die Python-Anwendung zu verwenden.

5. Speichern Sie die Dateien, klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt (stellen Sie sicher, dass es nicht noch lokal ausgeführt wird), und wählen Sie dann **Veröffentlichen** aus.  
   
     ![Screenshot des im Projektmappen-Explorer ausgewählten Lernprogramms mit hervorgehobener Option „Veröffentlichen“](./media/documentdb-python-application/image20.png)
6. Wählen Sie im Dialogfeld **Veröffentlichen** die Optionen **Microsoft Azure App Service** und **Neu erstellen** aus, und klicken Sie dann auf **Veröffentlichen**.
   
    ![Screenshot des Fensters „Veröffentlichen“ mit Hervorhebung von „Microsoft Azure App Service“](./media/documentdb-python-application/cosmos-db-python-publish.png)
7. Geben Sie im Dialogfeld **App Service erstellen** den Namen Ihrer Web-App sowie Ihr **Abonnement**, die **Ressourcengruppe** und den **App Service-Plan** ein, und klicken Sie auf **Erstellen**.
   
    ![Screenshot des Fensters „Microsoft Azure-Web-Apps“](./media/documentdb-python-application/cosmos-db-python-create-app-service.png)
8. In wenigen Sekunden hat Visual Studio das Kopieren Ihrer Dateien auf den Server abgeschlossen und zeigt „Die Seite kann aufgrund eines internen Serverfehlers nicht angezeigt werden.“ auf der `http://<your app service>.azurewebsites.net/`-Seite an.

9. Öffnen Sie im Azure-Portal Ihr neues App Service-Konto, scrollen Sie dann im Navigationsmenü zum Abschnitt **Entwicklungstools**, wählen Sie **Erweiterungen**, und klicken Sie dann auf **+ Hinzufügen**.

10. Scrollen Sie auf der Seite **Erweiterung auswählen** nach unten zur letzten Python 2.7-Installation, wählen Sie die x86- oder x64-Bit-Option, und klicken Sie auf **OK**, um die rechtlichen Bedingungen zu akzeptieren.  
   
11. Installieren Sie mithilfe der Kudu-Konsole, zu der Sie unter `https://<your app service name>.scm.azurewebsites.net/DebugConsole` navigieren können, die in der `requirements.txt`-Datei Ihrer App aufgelisteten Pakete. Navigieren Sie dazu in der Kudu Diagnostic Console zu Ihrem Python-Ordner `D:\home\Python27`, und führen Sie dann den folgenden Befehl wie im Abschnitt [Kudu console](/visual-studio/python/managing-python-on-azure-app-service.md#azure-app-service-kudu-console) (Kudu-Konsole) beschrieben aus:

    ```
    D:\home\Python27>python -m pip install --upgrade -r /home/site/wwwroot/requirements.txt
    ```          

12. Starten Sie den App Service nach der Installation der neuen Pakete durch Drücken der Schaltfläche **Neustart** im Azure-Portal neu. 

    > [!Tip] 
    > Wenn Sie Änderungen an der `requirements.txt`-Datei Ihrer App vornehmen, achten Sie darauf, dass Sie zur Installation alle Pakete, die jetzt in dieser Datei aufgeführt werden, die Kudu-Konsole verwenden. 

13. Nachdem Sie die Serverumgebung vollständig konfiguriert und die Seite im Browser aktualisiert haben, sollte die Web-App angezeigt werden.

    ![Ergebnisse der Veröffentlichung von Bottle-, Flask- und Django-Apps in App Service](./media/documentdb-python-application/python-published-app-services.png)

    > [!Tip] 
    > Wenn die Webseite nicht angezeigt wird, oder Sie immer noch die Fehlermeldung „Die Seite kann aufgrund eines internen Serverfehlers nicht angezeigt werden.“ erhalten, öffnen Sie die Datei „web.config“ in Kudo, fügen Sie ` <httpErrors errorMode="Detailed"></httpErrors>` dem Abschnitt „system.webServer“ hinzu, und aktualisieren Sie die Seite. Damit erfolgt eine ausführliche Fehlerausgabe im Browser. 

## <a name="troubleshooting"></a>Problembehandlung
Wenn dies die erste Python-App ist, die Sie auf Ihrem Computer ausgeführt haben, stellen Sie sicher, dass die folgenden Ordner (bzw. die entsprechenden Installationsverzeichnisse) in Ihrer PATH-Variable enthalten sind:

    C:\Python27\site-packages;C:\Python27\;C:\Python27\Scripts;

Wenn auf Ihrer Abstimmungsseite ein Fehler angezeigt wird und Sie dem Projekt einen anderen Namen als **tutorial** gegeben haben, stellen Sie sicher, dass **\_\_init\_\_.py** in der Zeile `import tutorial.view` auf den richtigen Projektnamen verweist.

## <a name="next-steps"></a>Nächste Schritte
Glückwunsch! Sie haben Ihre erste Python-Webanwendung unter Verwendung von Azure Cosmos DB fertiggestellt und in Azure veröffentlicht.

Um Ihrer Webanwendung weitere Funktionen hinzuzufügen, überprüfen Sie die im [Azure Cosmos DB-Python-SDK](documentdb-sdk-python.md) verfügbaren APIs.

Weitere Informationen zu Azure, Visual Studio und Python finden Sie im [Python Developer Center](https://azure.microsoft.com/develop/python/). 

Weitere Python Flask-Tutorials finden Sie unter [The Flask Mega-Tutorial, Part I: Hello, World!](http://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world). 

[Visual Studio Express]: http://www.visualstudio.com/products/visual-studio-express-vs.aspx
[2]: https://www.python.org/downloads/windows/
[3]: https://www.microsoft.com/download/details.aspx?id=44266
[Microsoft Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[Azure portal]: http://portal.azure.com
