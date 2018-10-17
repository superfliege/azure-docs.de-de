---
title: Konfigurieren von Python-Apps für Azure App Service unter Linux
description: In diesem Tutorial werden die Optionen zum Erstellen und Konfigurieren von Python-Apps für Azure App Service unter Linux beschrieben.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 10/09/2018
ms.author: astay;cephalin;kraigb
ms.custom: mvc
ms.openlocfilehash: 71cbf0bb31a72e3b257f25c159d9d9eea31dbfbb
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "48901617"
---
# <a name="configure-your-python-app-for-the-azure-app-service-on-linux"></a>Konfigurieren Ihrer Python-App für Azure App Service unter Linux

In diesem Artikel wird beschrieben, wie Python-Apps mit [Azure App Service unter Linux](app-service-linux-intro.md) ausgeführt werden und wie Sie das Verhalten von App Service bei Bedarf anpassen können.

## <a name="container-characteristics"></a>Eigenschaften von Containern

Python-Apps, die für App Service unter Linux bereitgestellt werden, werden in einem Docker-Container ausgeführt, der im GitHub-Repository [Azure-App-Service/python container](https://github.com/Azure-App-Service/python/tree/master/3.7.0) definiert ist.

Dieser Container verfügt über die folgenden Eigenschaften:

- Das grundlegende Containerimage ist `python-3.7.0-slim-stretch`. Dies bedeutet, dass Apps mit Python 3.7 ausgeführt werden. Falls Sie eine andere Version von Python benötigen, müssen Sie stattdessen ein eigenes Containerimage erstellen und bereitstellen. Weitere Informationen finden Sie unter [Verwenden eines benutzerdefinierten Docker-Images für Web-App für Container](tutorial-custom-docker-image.md).

- Apps werden per [Gunicorn WSGI HTTP Server](http://gunicorn.org/) mit den zusätzlichen Argumenten `--bind=0.0.0.0 --timeout 600` ausgeführt.

- Standardmäßig enthält das Basisimage das Flask-Webframework, aber der Container unterstützt auch andere Frameworks, die WSGI-konform und mit Python 3.7 kompatibel sind, z.B. Django.

- Erstellen Sie zum Installieren von zusätzlichen Paketen, z.B. Django, die Datei [*requirements.txt*](https://pip.pypa.io/en/stable/user_guide/#requirements-files) im Stamm Ihres Projekts, indem Sie `pip freeze > requirements.txt` verwenden. Veröffentlichen Sie Ihr Projekt anschließend per Git-Bereitstellung in App Service. Hierbei wird automatisch `pip install -r requirements.txt` im Container ausgeführt, um die Abhängigkeiten Ihrer App zu installieren.

## <a name="container-startup-process-and-customizations"></a>Startprozess und Anpassungen für Container

Beim Starten führt der Container vom Typ „App Service unter Linux“ die folgenden Schritte aus:

1. Es wird eine Prüfung auf einen benutzerdefinierten Startbefehl durchgeführt, und falls dieser vorhanden ist, wird er angewendet.
1. Es wird geprüft, ob die Datei *wsgi.py* einer Django-App vorhanden ist. Wenn ja, wird mit dieser Datei Gunicorn gestartet.
1. Es wird geprüft, ob die Datei *application.py* vorhanden ist. Wenn ja, wird Gunicorn mit `application:app` gestartet und eine Flask-App vorausgesetzt.
1. Wenn keine andere App gefunden wird, können Sie eine Standard-App starten, die im Container integriert ist.

Die folgenden Abschnitte enthalten weitere Details zu den einzelnen Optionen.

### <a name="django-app"></a>Django-App

Für Django-Apps sucht App Service in Ihrem App-Code nach der Datei `wsgi.py` und führt dann mit dem folgenden Befehl Gunicorn aus:

```bash
# <module> is the path to the folder containing wsgi.py
gunicorn --bind=0.0.0.0 --timeout 600 <module>.wsgi
```

Wenn Sie eine genauere Kontrolle über den Startbefehl wünschen, können Sie einen [benutzerdefinierten Startbefehl](#custom-startup-command) verwenden und `<module>` durch den Namen des Moduls ersetzen, das *wsgi.py* enthält.

### <a name="flask-app"></a>Flask-App

Für Flask sucht App Service nach der Datei *application.py* und startet Gunicorn wie folgt:

```bash
gunicorn --bind=0.0.0.0 --timeout 600 application:app
```

Falls Ihr App-Hauptmodul in einer anderen Datei enthalten ist, sollten Sie einen anderen Namen für das App-Objekt verwenden. Wenn Sie zusätzliche Argumente für Gunicorn angeben möchten, können Sie einen [benutzerdefinierten Startbefehl](#custom-startup-command) nutzen. Der Abschnitt enthält ein Beispiel für Flask, indem Zugangscode in *hello.py* und ein Flask-App-Objekt mit dem Namen `myapp` verwendet werden.

### <a name="custom-startup-command"></a>Benutzerdefinierter Startbefehl

Sie können das Startverhalten des Containers steuern, indem Sie einen benutzerdefinierten Gunicorn-Startbefehl angeben. Wenn Sie beispielsweise über eine Flask-App verfügen, deren Hauptmodul *hello.py* lautet, und das Flask-App-Objekt den Namen `myapp` hat, lautet der Befehl wie folgt:

```bash
gunicorn --bind=0.0.0.0 --timeout 600 hello:myapp
```

Sie können dem Befehl auch zusätzliche Argumente für Gunicorn hinzufügen, z.B. `--workers=4`. Weitere Informationen finden Sie unter [Running Gunicorn](http://docs.gunicorn.org/en/stable/run.html) (Ausführen von Gunicorn) (docs.gunicorn.org).

Führen Sie die folgenden Schritte aus, um einen benutzerdefinierten Befehl anzugeben:

1. Navigieren Sie im Azure-Portal zur Seite [Anwendungseinstellungen](../web-sites-configure.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json).

1. Legen Sie in den Einstellungen unter **Laufzeit** die Option **Stapel** auf **Python 3.7** fest, und geben Sie den Befehl direkt in das Feld **Startdatei** ein.

    Alternativ hierzu können Sie den Befehl auch in einer Textdatei im Stamm Ihres Projekts speichern, z.B. *startup.txt* (oder ein beliebiger anderer Name). Stellen Sie diese Datei dann in App Service bereit, und geben Sie stattdessen diesen Namen im Feld **Startdatei** an. Mit dieser Option können Sie den Befehl in Ihrem Quellcoderepository verwalten, anstatt über das Azure-Portal.

1. Wählen Sie **Speichern**aus. App Service wird automatisch neu gestartet, und nach einigen Sekunden sollten Sie erkennen können, dass der benutzerdefinierte Startbefehl angewendet wird.

> [!Note]
> App Service ignoriert alle Fehler, die beim Verarbeiten einer benutzerdefinierten Befehlsdatei auftreten, und setzt den Startprozess dann fort, indem nach Django- und Flask-Apps gesucht wird. Wenn das erwartete Verhalten nicht zu beobachten ist, sollten Sie überprüfen, ob Ihre Startdatei für App Service bereitgestellt wird und keine Fehler enthält.

### <a name="default-behavior"></a>Standardverhalten

Wenn App Service keinen benutzerdefinierten Befehl und keine Django-App oder Flask-App findet, wird eine schreibgeschützte Standard-App ausgeführt, die sich im Ordner _opt/defaultsite_ befindet. Die Standard-App wird wie folgt angezeigt:

![App Service-Standard-App auf Linux-Webseite](media/how-to-configure-python/default-python-app.png)

## <a name="troubleshooting"></a>Problembehandlung

- **Die Standard-App wird angezeigt, nachdem Sie Ihren eigenen App-Code bereitgestellt haben.**  Die Standard-App wird angezeigt, weil Sie entweder Ihren App-Code nicht in App Service bereitgestellt haben oder App Service Ihren App-Code nicht finden konnte und stattdessen die Standard-App ausgeführt hat.
  - Starten Sie App Service neu, warten Sie 15 bis 20 Sekunden, und prüfen Sie die App dann erneut.
  - Verwenden Sie SSH oder die Kudu-Konsole, um eine direkte Verbindung mit App Service herzustellen, und stellen Sie sicher, dass Ihre Dateien unter *site/wwwroot* vorhanden sind. Falls Ihre Dateien nicht vorhanden sind, sollten Sie Ihren Bereitstellungsprozess überprüfen und die App erneut bereitstellen.
  - Wenn Ihre Dateien vorhanden sind, konnte App Service Ihre spezifische Startdatei nicht identifizieren. Überprüfen Sie, ob Ihre App so strukturiert ist, wie App Service dies für [Django](#django-app) oder [Flask](#flask-app) erwartet, oder verwenden Sie einen [benutzerdefinierten Startbefehl](#custom-startup-command).

- **Im Browser wird die Meldung „Dienst nicht verfügbar“ angezeigt.** Für den Browser ist ein Timeout aufgetreten, während auf eine Antwort von App Service gewartet wurde. Dies weist darauf hin, dass App Service den Gunicorn-Server gestartet hat, aber die Argumente, mit denen der App-Code angegeben wird, fehlerhaft sind.
  - Aktualisieren Sie den Browser. Dies gilt besonders, wenn Sie in Ihrem App Service-Plan die niedrigsten Tarife nutzen. Bei Verwendung von Free-Tarifen kann das Starten der App beispielsweise länger dauern. Sie reagiert dann wieder, nachdem Sie den Browser aktualisiert haben.
  - Überprüfen Sie, ob Ihre App so strukturiert ist, wie App Service dies für [Django](#django-app) oder [Flask](#flask-app) erwartet, oder verwenden Sie einen [benutzerdefinierten Startbefehl](#custom-startup-command).
  - Verwenden Sie SSH oder die Kudu-Konsole, um eine Verbindung mit App Service herzustellen, und untersuchen Sie dann die im Ordner *LogFiles* gespeicherten Diagnoseprotokolle. Weitere Informationen zur Protokollierung finden Sie unter [Aktivieren der Diagnoseprotokollierung für Web-Apps in Azure App Service](../web-sites-enable-diagnostic-log.md).
