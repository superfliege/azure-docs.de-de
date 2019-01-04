---
title: Installieren und Ausführen von Containern
titlesuffix: Computer Vision - Azure Cognitive Services
description: Informationen zum Herunterladen, Installieren und Ausführen von Containern für maschinelles Sehen in diesem Schritt-für-Schritt-Tutorial.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 11/14/2018
ms.author: diberry
ms.openlocfilehash: 9964be07c578cd1cafd93328bffe972483123e03
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2018
ms.locfileid: "53077034"
---
# <a name="install-and-run-containers"></a>Installieren und Ausführen von Containern

Die Verwendung von Containern ist ein Ansatz zur Softwareverteilung, bei dem eine Anwendung oder ein Dienst als Containerimage gepackt wird. Die Konfiguration und die Abhängigkeiten der Anwendung bzw. des Diensts sind im Containerimage enthalten. Das Containerimage kann anschließend mit geringer oder ganz ohne Bearbeitung auf einem Containerhost bereitgestellt werden. Container sind voneinander und vom zugrunde liegenden Betriebssystem isoliert und nehmen weniger Speicher in Anspruch als ein virtueller Computer. Container können für kurzfristige Aufgaben über Containerimages instanziiert und wieder entfernt werden, wenn Sie nicht mehr benötigt werden.

Der Texterkennungsteil des maschinelles Sehens ist auch als Docker-Container verfügbar. Damit können Sie gedruckten Text in Bildern von verschiedensten Objekten mit unterschiedlichen Oberflächen und Hintergründen erkennen und extrahieren. Hierzu zählen z.B. Belege, Poster, und Visitenkarten.  
> [!IMPORTANT]
> Der Texterkennungscontainer funktioniert derzeit nur für Englisch.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="preparation"></a>Vorbereitung

Zur Verwendung des Texterkennungscontainers müssen Sie die folgenden Voraussetzungen erfüllen:

**Docker-Engine:** Die Docker-Engine muss lokal installiert sein. Docker stellt Pakete zur Konfiguration der Docker-Umgebung unter [macOS](https://docs.docker.com/docker-for-mac/), [Linux](https://docs.docker.com/engine/installation/#supported-platforms) und [Windows](https://docs.docker.com/docker-for-windows/) zur Verfügung. Unter Windows muss Docker für die Unterstützung von Linux-Containern konfiguriert werden. Docker-Container können auch direkt für [Azure Kubernetes Service](/azure/aks/), [Azure Container Instances](/azure/container-instances/) oder einen [Kubernetes](https://kubernetes.io/)-Cluster mit Bereitstellung in [Azure Stack](/azure/azure-stack/) bereitgestellt werden. Weitere Informationen zum Bereitstellen von Kubernetes in Azure Stack finden Sie unter [Bereitstellen von Kubernetes in Azure Stack](/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy).

Docker muss so konfiguriert werden, dass die Container eine Verbindung mit Azure herstellen und Abrechnungsdaten an Azure senden können.

**Kenntnisse von Microsoft Container Registry und Docker:** Sie sollten über Grundkenntnisse der Konzepte von Microsoft Container Registry und Docker verfügen, einschließlich Registrierungen, Repositorys, Container und Containerimages, und die grundlegenden `docker`-Befehle kennen.  

Eine Einführung in Docker und Container finden Sie in der [Docker-Übersicht](https://docs.docker.com/engine/docker-overview/).

### <a name="container-requirements-and-recommendations"></a>Containeranforderungen und -empfehlungen

Für den Texterkennungscontainer sind mindestens 1 CPU-Kern, mindestens 2,6 Gigahertz (GHz) und 8 Gigabyte (GB) zugewiesener Arbeitsspeicher erforderlich. Wir empfehlen jedoch mindestens 2 CPU-Kerne und eine Speicherzuweisung von 8 GB.

## <a name="request-access-to-the-private-container-registry"></a>Anfordern des Zugriffs auf die private Containerregistrierung

Sie müssen zuerst das [Formular zum Anfordern von Cognitive Services-Containern für maschinelles Sehen](https://aka.ms/VisionContainersPreview) ausfüllen und senden, um Zugriff auf den Texterkennungscontainer anzufordern. Im Formular müssen Sie Informationen über Sie selbst, Ihr Unternehmen und das Benutzerszenario eintragen, für das Sie den Container verwenden möchten. Nach der Übermittlung überprüft das Azure Cognitive Services-Team das Formular, um sicherzustellen, dass Sie die Kriterien für den Zugriff auf die private Containerregistrierung erfüllen.

> [!IMPORTANT]
> Sie müssen im Formular eine E-Mail-Adresse verwenden, die entweder einem Microsoft-Konto (MSA) oder einem Azure Active Directory-Konto (Azure AD) zugeordnet ist.

Wenn Ihre Anforderung genehmigt wurde, erhalten Sie eine E-Mail mit Anweisungen zum Abrufen Ihrer Anmeldeinformationen und zum Zugreifen auf die private Containerregistrierung.

## <a name="create-a-computer-vision-resource-on-azure"></a>Erstellen einer Ressource für maschinelles Sehen in Azure

Sie müssen in Azure eine Ressource für maschinelles Sehen erstellen, wenn Sie den Texterkennungscontainer verwenden möchten. Nachdem Sie die Ressource erstellt haben, verwenden Sie den Abonnementschlüssel und die Endpunkt-URL der Ressource zum Instanziieren des Containers. Weitere Informationen zum Instanziieren eines Containers finden Sie unter [Instanziieren eines Containers über ein heruntergeladenes Containerimage](#instantiate-a-container-from-a-downloaded-container-image).

Führen Sie die folgenden Schritte aus, um eine Azure-Ressource zu erstellen und daraus Informationen abzurufen:

1. Erstellen Sie eine Azure-Ressource im Azure-Portal.  
   Wenn Sie den Texterkennungscontainer verwenden möchten, müssen Sie zuerst im Azure-Portal eine entsprechende Ressource für maschinelles Sehen erstellen. Weitere Informationen finden Sie unter [Schnellstart: Erstellen eines Cognitive Services-Kontos im Azure-Portal](../cognitive-services-apis-create-account.md).

1. Rufen Sie die Endpunkt-URL und den Abonnementschlüssel für die Azure-Ressource ab.  
   Nachdem die Azure-Ressource erstellt wurde, müssen Sie die Endpunkt-URL und den Abonnementschlüssel aus dieser Ressource verwenden, um den entsprechenden Texterkennungscontainer zu instanziieren. Sie können die Endpunkt-URL und den Abonnementschlüssel im Azure-Portal von den Seiten „Schnellstart“ bzw. „Schlüssel“ der Ressource für maschinelles Sehen kopieren.

## <a name="log-in-to-the-private-container-registry"></a>Anmelden bei der privaten Containerregistrierung

Es gibt mehrere Möglichkeiten, sich bei der privaten Containerregistrierung für Cognitive Services-Container zu authentifizieren, aber die empfohlene Methode über die Befehlszeile ist die Verwendung der [Docker-CLI](https://docs.docker.com/engine/reference/commandline/cli/).

Verwenden Sie den Befehl [docker login](https://docs.docker.com/engine/reference/commandline/login/) wie im folgenden Beispiel gezeigt, um sich bei der privaten Containerregistrierung für Cognitive Services-Container (`containerpreview.azurecr.io`) anzumelden. Ersetzen Sie *\<username\>* durch den Benutzernamen und *\<password\>* durch das Kennwort. Diese Anmeldeinformationen wurden Ihnen vom Azure Cognitive Services-Team bereitgestellt.

```docker
docker login containerpreview.azurecr.io -u <username> -p <password>
```

Wenn Sie Ihre Anmeldeinformationen in einer Textdatei gesichert haben, können Sie den Inhalt dieser Textdatei über den Befehl `cat` mit dem Befehl `docker login` verketten, wie im folgenden Beispiel gezeigt. Ersetzen Sie *\<passwordFile\>* durch den Pfad und den Namen der Textdatei mit dem Kennwort und *\<username\>* durch den Benutzernamen, der in Ihren Anmeldeinformationen angegeben wurde.

```docker
cat <passwordFile> | docker login containerpreview.azurecr.io -u <username> --password-stdin
```

## <a name="download-container-images-from-the-private-container-registry"></a>Herunterladen von Containerimages aus der privaten Containerregistrierung

Das Containerimage für den Texterkennungscontainer steht über eine private Docker-Containerregistrierung namens `containerpreview.azurecr.io` in Azure Container Registry zur Verfügung. Das Containerimage für den Texterkennungscontainer muss aus dem Repository heruntergeladen werden, um den Container lokal auszuführen.

Verwenden Sie den Befehl [docker pull](https://docs.docker.com/engine/reference/commandline/pull/), um ein Containerimage aus dem Repository herunterzuladen. Verwenden Sie beispielsweise den folgenden Befehl, um das neueste Texterkennungscontainer-Image aus dem Repository herunterzuladen:

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text:latest
```

Eine vollständige Beschreibung der verfügbaren Tags für den Texterkennungscontainer finden Sie unter [Recognize Text](https://go.microsoft.com/fwlink/?linkid=2018655) (Texterkennung) auf Docker Hub.

> [!TIP]
> Mithilfe des Befehls [docker images](https://docs.docker.com/engine/reference/commandline/images/) können Sie Ihre heruntergeladenen Containerimages auflisten. Mit dem folgenden Befehl werden beispielsweise die ID, das Repository und das Tag jedes heruntergeladenen Containerimages in Form einer Tabelle aufgelistet:
>
>  ```Docker
>  docker images --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}"
>  ```
>

## <a name="instantiate-a-container-from-a-downloaded-container-image"></a>Instanziieren eines Containers über ein heruntergeladenes Containerimage

Verwenden Sie den Befehl [docker run](https://docs.docker.com/engine/reference/commandline/run/), um einen Container über ein heruntergeladenes Containerimage zu instanziieren. Der folgende Befehl führt beispielsweise folgende Aktionen durch:

* Instanziieren eines Containers über das Texterkennungscontainer-Image
* Zuweisen von zwei CPU-Kernen und 8 GB Arbeitsspeicher
* Verfügbarmachen des TCP-Ports 5000 und Zuweisen einer Pseudo-TTY-Verbindung für den Container
* Automatisches Entfernen des Containers, nachdem er beendet wurde

```docker
docker run --rm -it -p 5000:5000 --memory 8g --cpus 2 containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text Eula=accept Billing=https://westus.api.cognitive.microsoft.com/vision/v2.0 ApiKey=0123456789
```

Nach der Instanziierung können Sie Vorgänge aus dem Container über den Host-URI des Containers aufrufen. Beispielsweise steht der folgende Host-URI für den Texterkennungscontainer, der im vorherigen Beispiel instanziiert wurde:

```http
http://localhost:5000/
```

> [!IMPORTANT]
> Über den relativen `/swagger`-URI für einen Container können Sie auf die [OpenAPI-Spezifikation](https://swagger.io/docs/specification/about/) (früher Swagger-Spezifikation) zugreifen, in der die vom instanziierten Container unterstützten Vorgänge beschrieben werden. Der folgende URI bietet beispielsweise Zugriff auf die OpenAPI-Spezifikation für den Texterkennungscontainer, der im vorherigen Beispiel instanziiert wurde:
>
>  ```http
>  http://localhost:5000/swagger
>  ```

Zur asynchronen oder synchronen Texterkennung können Sie entweder die in Ihrem Container verfügbaren [REST-API-Vorgänge aufrufen](https://docs.microsoft.com/azure/cognitive-services/computer-vision/vision-api-how-to-topics/howtocallvisionapi) oder die Clientbibliothek des [Azure Cognitive Services SDK für maschinelles Sehen](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.ComputerVision) zum Aufrufen dieser Vorgänge verwenden.  
> [!IMPORTANT]
> Sie benötigen Version 3.2.0 oder höher des Azure Cognitive Services SDK für maschinelles Sehen, wenn Sie die Clientbibliothek mit Ihrem Container verwenden möchten.

### <a name="asynchronous-text-recognition"></a>Asynchrone Texterkennung

Sie können die Vorgänge `POST /vision/v2.0/recognizeText` und `GET /vision/v2.0/textOperations/*{id}*` kombiniert verwenden, um gedruckten Text in einem Bild asynchron erkennen, ähnlich wie der Dienst für maschinelles Sehen die entsprechenden REST-Vorgänge verwendet. Der Texterkennungscontainer erkennt zurzeit nur gedruckten Text, keinen handgeschriebenen Text, daher wird der Parameter `mode`, der normalerweise für den Vorgang des Diensts für maschinelles Sehen angegeben wird, vom Texterkennungscontainer ignoriert.

### <a name="synchronous-text-recognition"></a>Synchrone Texterkennung

Mit dem Vorgang `POST /vision/v2.0/recognizeTextDirect` können Sie gedruckten Text in einem Bild synchron erkennen. Da dieser Vorgang synchron ist, entspricht der Anforderungstext für diesen Vorgang dem des Vorgangs `POST /vision/v2.0/recognizeText`, aber der Antworttext für diesen Vorgang ist identisch mit dem des Vorgangs `GET /vision/v2.0/textOperations/*{id}*`.

### <a name="billing"></a>Abrechnung

Der Texterkennungscontainer sendet Abrechnungsinformationen an Azure und verwendet dafür eine entsprechende Ressource für maschinelles Sehen in Ihrem Azure-Konto. Die folgenden Optionen werden zu Abrechnungszwecken vom Texterkennungscontainer verwendet:

| Option | BESCHREIBUNG |
|--------|-------------|
| `ApiKey` | Der API-Schlüssel der Ressource für maschinelles Sehen, der zum Nachzuverfolgen von Abrechnungsinformationen verwendet wird.<br/>Der Wert dieser Option muss für die bereitgestellte Azure-Ressource für maschinelles Sehen, die in `Billing` angegeben wurde, auf einen API-Schlüssel festgelegt werden. |
| `Billing` | Der Endpunkt der Ressource für maschinelles Sehen, der zum Nachzuverfolgen von Abrechnungsinformationen verwendet wird.<br/>Der Wert dieser Option muss auf den Endpunkt-URI einer bereitgestellten Azure-Ressource für maschinelles Sehen festgelegt werden.|
| `Eula` | Weist darauf hin, dass Sie die Lizenz für den Container akzeptiert haben.<br/>Der Wert dieser Option muss auf `accept` festgelegt werden. |

> [!IMPORTANT]
> Alle drei Optionen müssen mit gültigen Werten angegeben werden, damit der Container gestartet wird.

Weitere Informationen zu diesen Optionen finden Sie unter [Konfigurieren von Containern](computer-vision-resource-container-config.md).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben Sie die Konzepte und den Workflow zum Herunterladen, Installieren und Ausführen von Containern für maschinelles Sehen kennengelernt. Zusammenfassung:

* Maschinelles Sehen bietet einen Linux-Container für Docker, um gedruckten Text zu erkennen und zu extrahieren.
* Containerimages werden aus einer privaten Containerregistrierung in Azure heruntergeladen.
* Containerimages werden in Docker ausgeführt.
* Sie können entweder die REST-API oder das SDK verwenden, um Vorgänge in Containern für maschinelles Sehen über den Host-URI des Containers aufzurufen.
* Bei der Instanziierung eines Containers müssen Sie Abrechnungsinformationen angeben.

> [!IMPORTANT]
> Für die Ausführung von Cognitive Services-Containern besteht keine Lizenz, wenn sie nicht zu Messzwecken mit Azure verbunden sind. Kunden müssen sicherstellen, dass Container jederzeit Abrechnungsinformationen an den Messungsdienst übermitteln können. Cognitive Services-Container senden keine Kundendaten (z.B. das analysierte Bild oder den analysierten Text) an Microsoft.

## <a name="next-steps"></a>Nächste Schritte

* Konfigurationseinstellungen finden Sie unter [Konfigurieren von Containern](computer-vision-resource-container-config.md).
* Lesen Sie [Übersicht über maschinelles Sehen](Home.md), um weitere Informationen zur Erkennung von gedrucktem und handschriftlichem Text zu erhalten.  
* Details zu den vom Container unterstützten Methoden finden Sie unter [Maschinelles Sehen-API](//westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa).
* Unter [Häufig gestellte Fragen (FAQ)](FAQ.md) finden Sie Informationen zur Problembehandlung im Zusammenhang mit der Funktionalität des maschinellen Sehens.
