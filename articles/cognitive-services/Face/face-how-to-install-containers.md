---
title: Installieren und Ausführen von Containern
titlesuffix: Face - Azure Cognitive Services
description: Informationen zum Herunterladen, Installieren und Ausführen von Containern für die Gesichtserkennung in diesem Schritt-für-Schritt-Tutorial.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 01/22/2019
ms.author: diberry
ms.openlocfilehash: 310311c22be25960b15f20d573624f50b0f618b1
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54474811"
---
# <a name="install-and-run-containers"></a>Installieren und Ausführen von Containern

Die Verwendung von Containern ist ein Ansatz zur Softwareverteilung, bei dem eine Anwendung oder ein Dienst als Containerimage gepackt wird. Die Konfiguration und die Abhängigkeiten der Anwendung bzw. des Diensts sind im Containerimage enthalten. Das Containerimage kann anschließend mit geringer oder ganz ohne Bearbeitung auf einem Containerhost bereitgestellt werden. Container sind voneinander und vom zugrunde liegenden Betriebssystem isoliert und nehmen weniger Speicher in Anspruch als ein virtueller Computer. Container können für kurzfristige Aufgaben über Containerimages instanziiert und wieder entfernt werden, wenn sie nicht mehr benötigt werden.

Die Gesichtserkennung bietet einen standardisierten Linux-Container für Docker mit dem Namen „Gesichtserkennung“. Dieser erkennt menschliche Gesichter in Bildern und identifiziert zugehörige Attribute wie Gesichtsmerkmale (z.B. Nasen und Augen), Geschlecht, Alter und andere vom Computer vorhergesagte Gesichtsmerkmale. Zusätzlich zur Erkennung kann die Gesichtserkennung über eine Zuverlässigkeitsbewertung überprüfen, ob zwei Gesichter in einem oder verschiedenen Bildern identisch sind, oder Gesichter mit einer Datenbank vergleichen, um festzustellen, ob ein ähnliches oder identisches Gesicht bereits vorhanden ist. Sie kann auch ähnliche Gesichter in Gruppen mit gemeinsamen Gesichtsmerkmalen organisieren.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="preparation"></a>Vorbereitung

Zur Verwendung des Containers für die Gesichtserkennung müssen die folgenden Voraussetzungen erfüllt sein:

**Docker-Engine:** Die Docker-Engine muss lokal installiert sein. Docker stellt Pakete zur Konfiguration der Docker-Umgebung unter [macOS](https://docs.docker.com/docker-for-mac/), [Linux](https://docs.docker.com/engine/installation/#supported-platforms) und [Windows](https://docs.docker.com/docker-for-windows/) zur Verfügung. Unter Windows muss Docker für die Unterstützung von Linux-Containern konfiguriert werden. Docker-Container können auch direkt für [Azure Kubernetes Service](../../aks/index.yml), [Azure Container Instances](../../container-instances/index.yml) oder einen [Kubernetes](https://kubernetes.io/)-Cluster mit Bereitstellung in [Azure Stack](../../azure-stack/index.yml) bereitgestellt werden. Weitere Informationen zum Bereitstellen von Kubernetes in Azure Stack finden Sie unter [Bereitstellen von Kubernetes in Azure Stack](../../azure-stack/user/azure-stack-solution-template-kubernetes-deploy.md).

Docker muss so konfiguriert werden, dass die Container eine Verbindung mit Azure herstellen und Abrechnungsdaten an Azure senden können.

**Kenntnisse von Microsoft Container Registry und Docker:** Sie sollten über Grundkenntnisse der Konzepte von Microsoft Container Registry und Docker verfügen, einschließlich Registrierungen, Repositorys, Container und Containerimages, und die grundlegenden `docker`-Befehle kennen.  

Eine Einführung in Docker und Container finden Sie in der [Docker-Übersicht](https://docs.docker.com/engine/docker-overview/).

### <a name="container-requirements-and-recommendations"></a>Containeranforderungen und -empfehlungen

Für den Container für die Gesichtserkennung sind mindestens 1 CPU-Kern, mindestens 2,6 Gigahertz (GHz) und 4 Gigabyte (GB) zugewiesener Arbeitsspeicher erforderlich. Wir empfehlen jedoch mindestens 2 CPU-Kerne und eine Speicherzuweisung von 6 GB.

## <a name="request-access-to-the-private-container-registry"></a>Anfordern des Zugriffs auf die private Containerregistrierung

Sie müssen zuerst das [Formular zum Anfordern von Cognitive Services-Containern für maschinelles Sehen](https://aka.ms/VisionContainersPreview) ausfüllen und senden, um Zugriff auf den Container für die Gesichtserkennung anzufordern. Im Formular müssen Sie Informationen über Sie selbst, Ihr Unternehmen und das Benutzerszenario eintragen, für das Sie den Container verwenden möchten. Nach der Übermittlung überprüft das Azure Cognitive Services-Team das Formular, um sicherzustellen, dass Sie die Kriterien für den Zugriff auf die private Containerregistrierung erfüllen.

> [!IMPORTANT]
> Sie müssen im Formular eine E-Mail-Adresse verwenden, die entweder einem Microsoft-Konto (MSA) oder einem Azure Active Directory-Konto (Azure AD) zugeordnet ist.

Wenn Ihre Anforderung genehmigt wurde, erhalten Sie eine E-Mail mit Anweisungen zum Abrufen Ihrer Anmeldeinformationen und zum Zugreifen auf die private Containerregistrierung.

## <a name="create-a-face-resource-on-azure"></a>Erstellen einer Gesichtserkennungsressource in Azure

Sie müssen in Azure eine Gesichtserkennungsressource erstellen, wenn Sie den Container für die Gesichtserkennung verwenden möchten. Nachdem Sie die Ressource erstellt haben, verwenden Sie den Abonnementschlüssel und die Endpunkt-URL der Ressource zum Instanziieren des Containers. Weitere Informationen zum Instanziieren eines Containers finden Sie unter [Instanziieren eines Containers über ein heruntergeladenes Containerimage](#instantiate-a-container-from-a-downloaded-container-image).

Führen Sie die folgenden Schritte aus, um eine Gesichtserkennungsressource zu erstellen und daraus Informationen abzurufen:

1. Erstellen Sie eine Gesichtserkennungsressource im Azure-Portal.  
   Wenn Sie den Container für die Gesichtserkennung verwenden möchten, müssen Sie zuerst im Azure-Portal eine entsprechende Gesichtserkennungsressource erstellen. Weitere Informationen finden Sie unter [Schnellstart: Erstellen eines Cognitive Services-Kontos im Azure-Portal](../cognitive-services-apis-create-account.md).

1. Rufen Sie die Endpunkt-URL und den Abonnementschlüssel für die Azure-Ressource ab.  
   Nachdem die Azure-Ressource erstellt wurde, müssen Sie die Endpunkt-URL und den Abonnementschlüssel aus dieser Ressource verwenden, um den entsprechenden Container für die Gesichtserkennung zu instanziieren. Sie können die Endpunkt-URL und den Abonnementschlüssel im Azure-Portal auf den Seiten „Schnellstart“ bzw. „Schlüssel“ der Gesichtserkennungsressource kopieren.

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

Das Containerimage für den Container für die Gesichtserkennung steht über die private Docker-Containerregistrierung `containerpreview.azurecr.io` in Azure Container Registry zur Verfügung. Das Containerimage für die Gesichtserkennung muss aus dem Repository heruntergeladen werden, um den Container lokal auszuführen.

Verwenden Sie den Befehl [docker pull](https://docs.docker.com/engine/reference/commandline/pull/), um ein Containerimage aus dem Repository herunterzuladen. Verwenden Sie beispielsweise den folgenden Befehl, um das neueste Containerimage für die Gesichtserkennung aus dem Repository herunterzuladen:

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-face:latest
```

Eine vollständige Beschreibung der verfügbaren Tags für den Container für die Gesichtserkennung finden Sie unter [Recognize Text](https://go.microsoft.com/fwlink/?linkid=2018655) (Texterkennung) auf Docker Hub.

> [!TIP]
> Mithilfe des Befehls [docker images](https://docs.docker.com/engine/reference/commandline/images/) können Sie Ihre heruntergeladenen Containerimages auflisten. Mit dem folgenden Befehl werden beispielsweise die ID, das Repository und das Tag jedes heruntergeladenen Containerimages in Form einer Tabelle aufgelistet:
>
>  ```Docker
>  docker images --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}"
>  ```
>

## <a name="instantiate-a-container-from-a-downloaded-container-image"></a>Instanziieren eines Containers über ein heruntergeladenes Containerimage

Verwenden Sie den Befehl [docker run](https://docs.docker.com/engine/reference/commandline/run/), um einen Container über ein heruntergeladenes Containerimage zu instanziieren. Der folgende Befehl führt beispielsweise folgende Aktionen durch:

* Instanziieren eines Containers über das Containerimage für die Gesichtserkennung
* Zuweisen von zwei CPU-Kernen und 6 GB Arbeitsspeicher
* Verfügbarmachen des TCP-Ports 5000 und Zuweisen einer Pseudo-TTY-Verbindung für den Container
* Automatisches Entfernen des Containers, nachdem er beendet wurde

```Docker
docker run --rm -it -p 5000:5000 --memory 6g --cpus 2 containerpreview.azurecr.io/microsoft/cognitive-services-face Eula=accept Billing=https://westus.api.cognitive.microsoft.com/face/v1.0 ApiKey=0123456789
```

> [!IMPORTANT]
> Die Optionen `Eula`, `Billing` und `ApiKey` müssen angegeben werden, um den Container zu instanziieren, andernfalls wird der Container nicht gestartet.  Weitere Informationen finden Sie unter [Abrechnung](#billing).

Nach der Instanziierung können Sie Vorgänge aus dem Container über den Host-URI des Containers aufrufen. Beispielsweise steht der folgende Host-URI für den Container für die Gesichtserkennung, der im vorherigen Beispiel instanziiert wurde:

```http
http://localhost:5000/
```

> [!TIP]
> Über den relativen `/swagger`-URI für einen Container können Sie auf die [OpenAPI-Spezifikation](https://swagger.io/docs/specification/about/) (früher Swagger-Spezifikation) zugreifen, in der die vom instanziierten Container unterstützten Vorgänge beschrieben werden. Der folgende URI bietet beispielsweise Zugriff auf die OpenAPI-Spezifikation für den Container für die Gesichtserkennung, der im vorherigen Beispiel instanziiert wurde:
>
>  ```http
>  http://localhost:5000/swagger
>  ```

Sie können die in Ihrem Container verfügbaren [REST-API-Vorgänge aufrufen](https://docs.microsoft.com/azure/cognitive-services/face/face-api-how-to-topics/howtodetectfacesinimage) oder die [Clientbibliothek für die Gesichtserkennungs-API von Azure Cognitive Services](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/) verwenden, um diese Vorgänge aufzurufen.  
> [!IMPORTANT]
> Sie benötigen Version 2.0 oder höher der Clientbibliothek für die Gesichtserkennungs-API von Azure Cognitive Services, wenn Sie die Clientbibliothek mit Ihrem Container verwenden möchten.

Der einzige Unterschied zwischen dem Aufrufen eines bestimmten Vorgangs aus dem Container und dem Aufrufen desselben Vorgangs von einem entsprechenden Dienst in Azure ist, dass Sie den Host-URI des Containers anstelle des Host-URI einer Azure-Region verwenden, um den Vorgang aufzurufen. Wenn Sie z.B. eine Instanz der Gesichtserkennung, die in der Azure-Region „USA, Westen“ ausgeführt wird, verwenden möchten, um Gesichter zu erkennen, rufen Sie den folgenden REST-API-Vorgang auf:

```http
POST https://westus.api.cognitive.microsoft.com/face/v1.0/detect
```

Wenn Sie einen Container für die Gesichtserkennung, der auf dem lokalen Computer ausgeführt wird, mit der Standardkonfiguration für die Gesichtserkennung verwenden möchten, rufen Sie den folgenden REST-API-Vorgang auf:

```http
POST http://localhost:5000/face/v1.0/detect
```

### <a name="billing"></a>Abrechnung

Der Container für die Gesichtserkennung sendet Abrechnungsinformationen an Azure und verwendet dafür eine entsprechende Gesichtserkennungsressource in Ihrem Azure-Konto. Die folgenden Befehlszeilenoptionen werden zu Abrechnungszwecken vom Container für die Gesichtserkennung verwendet:

| Option | BESCHREIBUNG |
|--------|-------------|
| `ApiKey` | Der API-Schlüssel der Gesichtserkennungsressource, der zum Nachzuverfolgen von Abrechnungsinformationen verwendet wird.<br/>Der Wert dieser Option muss für die bereitgestellte Azure-Ressource für die Gesichtserkennung, die in `Billing` angegeben wurde, auf einen API-Schlüssel festgelegt werden. |
| `Billing` | Der Endpunkt der Gesichtserkennungsressource, der zum Nachzuverfolgen von Abrechnungsinformationen verwendet wird.<br/>Der Wert dieser Option muss auf den Endpunkt-URI einer bereitgestellten Azure-Ressource für die Gesichtserkennung festgelegt werden.|
| `Eula` | Gibt an, dass Sie die Lizenz für den Container akzeptiert haben.<br/>Der Wert dieser Option muss auf `accept` festgelegt werden. |

> [!IMPORTANT]
> Alle drei Optionen müssen mit gültigen Werten angegeben werden, damit der Container gestartet wird.

Weitere Informationen zu diesen Optionen finden Sie unter [Konfigurieren von Containern](face-resource-container-config.md).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben Sie die Konzepte und den Workflow zum Herunterladen, Installieren und Ausführen von Containern für die Gesichtserkennung kennengelernt. Zusammenfassung:

* Die Gesichtserkennung stellt einen Linux-Container für Docker mit dem Namen „Gesichtserkennung“ bereit, mit dem Sie Gesichter erkennen oder anhand einer Benutzerdatenbank identifizieren können.
* Containerimages werden aus einer privaten Containerregistrierung in Azure heruntergeladen.
* Containerimages werden in Docker ausgeführt.
* Sie können entweder die REST-API oder das SDK verwenden, um Vorgänge in Containern für die Gesichtserkennung über den Host-URI des Containers aufzurufen.
* Bei der Instanziierung eines Containers müssen Sie Abrechnungsinformationen angeben.

> [!IMPORTANT]
> Für die Ausführung von Cognitive Services-Containern besteht keine Lizenz, wenn sie nicht zu Messzwecken mit Azure verbunden sind. Kunden müssen sicherstellen, dass Container jederzeit Abrechnungsinformationen an den Messungsdienst übermitteln können. Cognitive Services-Container senden keine Kundendaten (z.B. das analysierte Bild oder den analysierten Text) an Microsoft.

## <a name="next-steps"></a>Nächste Schritte

* Konfigurationseinstellungen finden Sie unter [Konfigurieren von Containern](face-resource-container-config.md).
* In der [Übersicht über die Gesichtserkennung](Overview.md) finden Sie weitere Informationen zum Erkennen und Identifizieren von Gesichtern.  
* Details zu den vom Container unterstützten Methoden finden Sie unter [Gesichtserkennungs-API](//westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).
* Unter [Häufig gestellte Fragen (FAQ)](FAQ.md) finden Sie Informationen zum Beheben von Problemen im Zusammenhang mit der Funktionalität der Gesichtserkennung.
* Weitere [Cognitive Services-Container](../cognitive-services-container-support.md) verwenden