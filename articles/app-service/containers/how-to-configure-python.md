---
title: Konfigurieren des eingebauten Python-Image in Azure App Service
description: In diesem Tutorial werden Optionen zum Erstellen und Konfigurieren einer Python-Anwendung in Azure App Service mit dem integrierten Python-Image beschrieben.
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
ms.date: 09/25/2018
ms.author: astay;cephalin
ms.custom: mvc
ms.openlocfilehash: 9316805993b81e4d2511e833e0cc8f240807a1f9
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47228547"
---
# <a name="configure-built-in-python-image-in-azure-app-service-preview"></a>Konfigurieren des eingebauten Python-Image in Azure App Service (Vorschau)

In diesem Artikel wird veranschaulicht, wie Sie das in Python integrierte Image in [App Service unter Linux](app-service-linux-intro.md) konfigurieren, um Ihre Python-Anwendungen auszuführen.

## <a name="python-version"></a>Python-Version

Die Python-Runtime in App Service unter Linux verwendet die Version `python-3.7.0`.

## <a name="supported-frameworks"></a>Unterstützte Frameworks

Alle Versionen von WSGI-konformen (Web Server Gateway Interface) Webframeworks, die mit der Runtime `python-3.7` kompatibel sind, werden unterstützt.

## <a name="package-management"></a>Paketverwaltung

Während der Git-Veröffentlichung sucht die Kudu-Engine im Stammverzeichnis des Repository nach [requirements.txt](https://pip.pypa.io/en/stable/user_guide/#requirements-files) und installiert die Pakete automatisch mit `pip` in Azure.

Um diese Datei vor der Veröffentlichung zu generieren, navigieren Sie zum Stammverzeichnis des Repository, und führen Sie den folgenden Befehl in Ihrer Python-Umgebung aus:

```bash
pip freeze > requirements.txt
```

## <a name="configure-your-python-app"></a>Konfigurieren der Python-App

Das integrierte Python-Image in App Service verwendet den [Gunicorn](http://gunicorn.org/)-Server, um Ihre Python-Anwendung auszuführen. Gunicorn ist ein Python-WSGI-HTTP-Server für UNIX. App Service automatisiert die Konfiguration von Gunicorn automatisch für Django- und Flask-Projekte.

### <a name="django-app"></a>Django-App

Wenn Sie ein Django-Projekt veröffentlichen, das ein `wsgi.py`-Modul enthält, ruft Azure Gunicorn automatisch mit dem folgenden Befehl an:

```bash
gunicorn <path_to_wsgi>
```

### <a name="flask-app"></a>Flask-App

Wenn Sie eine Flask-App veröffentlichen und der Einstiegspunkt sich in einem `application.py`- oder `app.py`-Modul befindet, ruft Azure Gunicorn automatisch mit einem der folgenden Befehle auf:

```bash
gunicorn application:app
```

oder 

```bash
gunicorn app:app
```

### <a name="customize-start-up"></a>Anpassen des Startvorgangs

Um einen benutzerdefinierten Einstiegspunkt für Ihre App zu definieren, erstellen Sie zunächst eine _TXT_-Datei mit einem benutzerdefinierten Gunicorn-Befehl und speichern Sie sie im Stammverzeichnis Ihres Projekts. Erstellen Sie z. B. zum Starten des Servers mit dem Modul _helloworld.py_ und der Variablen `app` eine _startup.txt_ mit folgendem Inhalt:

```bash
gunicorn helloworld:app
```

Wählen Sie auf der Seite [Anwendungseinstellungen](../web-sites-configure.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) **Python|3.7** als **Laufzeitstapel** aus, und geben Sie den Namen Ihrer **Startdatei** aus dem vorherigen Schritt an. Beispiel: _startup.txt_.
