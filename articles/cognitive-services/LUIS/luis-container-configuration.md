---
title: Docker-Containereinstellungen
titleSuffix: Language Understanding - Azure Cognitive Services
description: Die Runtimeumgebung für LUIS-Container wird über die Argumente des Befehls `docker run` konfiguriert. LUIS verfügt über mehrere erforderliche Einstellungen sowie einige optionale Einstellungen.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 01/22/2019
ms.author: diberry
ms.openlocfilehash: 5437e81397182ede37ef98ad40b54c64f94e2092
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55294722"
---
# <a name="configure-language-understanding-docker-containers"></a>Konfigurieren von Docker-Containern für Language Understanding 

Die Laufzeitumgebung für LUIS-Container (**Language Understanding**) wird über die Argumente des Befehls `docker run` konfiguriert. LUIS verfügt über mehrere erforderliche Einstellungen sowie einige optionale Einstellungen. Es sind noch viele [Beispiele](#example-docker-run-commands) für den Befehl verfügbar. Die containerspezifischen Einstellungen sind die für [Eingabebereitstellung](#mount-settings) und Abrechnung. 

Containereinstellungen sind [hierarchisch](#hierarchical-settings) und können mit [Umgebungsvariablen](#environment-variable-settings) oder [Befehlszeilenargumenten](#command-line-argument-settings) für Docker festgelegt werden.

## <a name="configuration-settings"></a>Konfigurationseinstellungen

Dieser Container hat die folgenden Konfigurationseinstellungen:

|Erforderlich|Einstellung|Zweck|
|--|--|--|
|JA|[ApiKey](#apikey-setting)|Wird zum Nachverfolgen von Abrechnungsinformationen verwendet.|
|Nein |[ApplicationInsights](#applicationinsights-setting)|Ermöglicht das Hinzufügen von Unterstützung für [Azure Application Insights](https://docs.microsoft.com/azure/application-insights)-Telemetriedaten in Ihrem Container.|
|JA|[Billing](#billing-setting)|Gibt den Endpunkt-URI der Dienstressource in Azure an.|
|JA|[Eula](#eula-setting)| Gibt an, dass Sie die Lizenz für den Container akzeptiert haben.|
|Nein |[Fluentd](#fluentd-settings)|Schreibt Protokoll- und optional auch Metrikdaten auf einen Fluentd-Server.|
|Nein |[HTTP-Proxy](#http-proxy-credentials-settings)|Konfigurieren Sie einen HTTP-Proxy für ausgehende Anforderungen.|
|Nein |[Logging](#logging-settings)|Bietet Unterstützung für die ASP.NET Core-Protokollierung für Ihren Container. |
|JA|[Mounts](#mount-settings)|Liest und schreibt Daten vom Hostcomputer in den Container und umgekehrt.|

> [!IMPORTANT]
> Die Einstellungen [`ApiKey`](#apikey-setting), [`Billing`](#billing-setting) und [`Eula`](#eula-setting) werden gemeinsam verwendet, und Sie müssen gültige Werte für alle drei angeben, da der Container andernfalls nicht startet. Weitere Informationen zum Instanziieren eines Containers mithilfe dieser Konfigurationseinstellungen finden Sie unter [Billing](luis-container-howto.md#billing).

## <a name="apikey-setting"></a>ApiKey-Einstellung

Die `ApiKey`-Einstellung gibt den Schlüssel der Azure-Ressourcen an, mit dem die Abrechnungsinformationen für den Container verfolgt werden. Sie müssen einen Wert für ApiKey angeben, und bei dem Wert muss es sich um einen gültigen Schlüssel für die _Language Understanding_-Ressource handeln, die für die Konfigurationseinstellung [`Billing`](#billing-setting) angegeben wurde.

Diese Einstellung finden Sie hier:

* Azure-Portal: Ressourcenverwaltung von **Language Understanding** unter **Schlüssel**
* LUIS-Portal: Seite mit den Einstellungen für **Schlüssel und Endpunkt**. 

Verwenden Sie nicht den Starter- oder Erstellungsschlüssel. 

## <a name="applicationinsights-setting"></a>ApplicationInsights-Einstellung

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

## <a name="billing-setting"></a>Billing-Einstellung

Die `Billing`-Einstellung gibt den Endpunkt-URI der _Language Understanding_-Ressource in Azure an, der zum Messen der Abrechnungsinformationen für den Container verwendet wird. Sie müssen einen Wert für diese Konfigurationseinstellung angeben, und bei dem Wert muss es sich um einen gültigen Endpunkt-URI für eine _Language Understanding_-Ressource in Azure handeln.

Diese Einstellung finden Sie hier:

* Azure-Portal: Übersicht von **Language Understanding** mit der Bezeichnung `Endpoint`
* LUIS-Portal: Einstellungsseite für **Schlüssel und Endpunkt** als Teil des Endpunkt-URI.

|Erforderlich| NAME | Datentyp | BESCHREIBUNG |
|--|------|-----------|-------------|
|JA| `Billing` | Zeichenfolge | URI des Abrechnungsendpunkts<br><br>Beispiel:<br>`Billing=https://westus.api.cognitive.microsoft.com/luis/v2.0` |

## <a name="eula-setting"></a>Eula-Einstellung

[!INCLUDE [Container shared configuration eula settings](../../../includes/cognitive-services-containers-configuration-shared-settings-eula.md)]

## <a name="fluentd-settings"></a>Fluentd-Einstellungen


[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-fluentd.md)]

## <a name="http-proxy-credentials-settings"></a>Anmeldeinformationseinstellungen für HTTP-Proxy

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-http-proxy.md)]

## <a name="logging-settings"></a>Logging-Einstellungen
 
[!INCLUDE [Container shared configuration logging settings](../../../includes/cognitive-services-containers-configuration-shared-settings-logging.md)]


## <a name="mount-settings"></a>Einbindungseinstellungen

Verwenden Sie Bindungsbereitstellungen zum Lesen und Schreiben von Daten im Container. Sie können eine Eingabe- oder Ausgabebereitstellung über die Option `--mount` im Befehl [docker run](https://docs.docker.com/engine/reference/commandline/run/) angeben. 

Der LUIS-Container verwendet die Eingabe- oder Ausgabebereitstellungen nicht zum Speichern von Trainings- oder Dienstdaten. 

Die genaue Syntax für den Bereitstellungspunkt auf dem Host variiert je nach Betriebssystem des Hosts. Darüber hinaus ist es eventuell nicht möglich, auf den Bereitstellungspunkt auf dem [Hostcomputer](luis-container-howto.md#the-host-computer) zuzugreifen, wenn ein Konflikt zwischen den vom Docker-Dienstkonto verwendeten Berechtigungen und den für den Bereitstellungspunkt auf dem Host verwendeten Berechtigungen besteht. 

In der folgenden Tabelle werden die unterstützten Einstellungen beschrieben.

|Erforderlich| NAME | Datentyp | BESCHREIBUNG |
|-------|------|-----------|-------------|
|JA| `Input` | Zeichenfolge | Das Ziel der Eingabeeinbindung. Standardwert: `/input`. Dies ist der Speicherort der LUIS-Paketdateien. <br><br>Beispiel:<br>`--mount type=bind,src=c:\input,target=/input`|
|Nein | `Output` | Zeichenfolge | Das Ziel der Ausgabeeinbindung. Standardwert: `/output`. Dies ist der Speicherort der Protokolle. Dazu gehören auch LUIS-Abfrageprotokolle und -Containerprotokolle. <br><br>Beispiel:<br>`--mount type=bind,src=c:\output,target=/output`|

## <a name="hierarchical-settings"></a>Hierarchische Einstellungen

[!INCLUDE [Container shared configuration hierarchical settings](../../../includes/cognitive-services-containers-configuration-shared-hierarchical-settings.md)]


## <a name="example-docker-run-commands"></a>Beispiele für den Befehl „docker run“

Die folgenden Beispiele verwenden die Konfigurationseinstellungen, um zu veranschaulichen, wie `docker run`-Befehle geschrieben und verwendet werden.  Nach dem Ausführen wird der Container so lange ausgeführt, bis Sie ihn [beenden](luis-container-howto.md#stop-the-container).


* **Zeilenfortsetzungszeichen:** In den Docker-Befehlen in den folgenden Abschnitten wird der umgekehrte Schrägstrich (`\`) als Zeilenfortsetzungszeichen verwendet. Ersetzen oder entfernen Sie diesen je nach den Anforderungen des Hostbetriebssystems. 
* **Argumentreihenfolge:** Ändern Sie die Reihenfolge der Argumente nur, wenn Sie mit Docker-Containern sehr gut vertraut sind.

Ersetzen Sie {_argument_name_} durch Ihre eigenen Werte:

| Platzhalter | Wert | Format oder Beispiel |
|-------------|-------|---|
|{ENDPOINT_KEY} | Der Endpunktschlüssel der trainierten LUIS-Anwendung. |xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx|
|{BILLING_ENDPOINT} | Den Wert für den Abrechnungsendpunkt finden Sie im Azure-Portal auf der Übersichtsseite von Language Understanding.|https://westus.api.cognitive.microsoft.com/luis/v2.0|

> [!IMPORTANT]
> Die Optionen `Eula`, `Billing` und `ApiKey` müssen angegeben werden, um den Container auszuführen, andernfalls wird der Container nicht gestartet.  Weitere Informationen finden Sie unter [Billing](luis-container-howto.md#billing).
> Der Wert ApiKey ist der **Schlüssel** von der Seite „Schlüssel und Endpunkte“ im LUIS-Portal. Er ist auch auf der Seite mit den Schlüsseln der Azure Language Understanding-Ressourcen verfügbar. 

### <a name="basic-example"></a>Einfaches Beispiel

Das folgende Beispiel enthält die wenigsten Argumente, die zum Ausführen des Containers erforderlich sind:

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 2 \
--mount type=bind,src=c:\input,target=/input \
--mount type=bind,src=c:\output,target=/output \
mcr.microsoft.com/azure-cognitive-services/luis:latest \
Eula=accept \
Billing={BILLING_ENDPOINT} \
ApiKey={ENDPOINT_KEY}
```

> [!Note] 
> Der gezeigte Befehl verwendet das Verzeichnis auf dem Laufwerk `c:`, um Berechtigungskonflikte in Windows zu vermeiden. Wenn Sie ein bestimmtes Verzeichnis als Eingabeverzeichnis verwenden möchten, müssen Sie dem Docker-Dienst möglicherweise die erforderliche Berechtigung gewähren. Im obigen Docker-Befehl wird der umgekehrte Schrägstrich (`\`) als Zeilenfortsetzungszeichen verwendet. Ersetzen oder entfernen Sie diesen je nach den Anforderungen des Betriebssystems Ihres [Hostcomputers](luis-container-howto.md#the-host-computer). Ändern Sie die Reihenfolge der Argumente nur, wenn Sie mit Docker-Containern sehr gut vertraut sind.


### <a name="applicationinsights-example"></a>Beispiel für ApplicationInsights

Im folgenden Beispiel wird das ApplicationInsights-Argument so festgelegt, dass während der Ausführung des Containers Telemetriedaten an Application Insights gesendet werden:

```bash
docker run --rm -it -p 5000:5000 --memory 6g --cpus 2 \
--mount type=bind,src=c:\input,target=/input \
--mount type=bind,src=c:\output,target=/output \
mcr.microsoft.com/azure-cognitive-services/luis:latest \
Eula=accept \
Billing={BILLING_ENDPOINT} \
ApiKey={ENDPOINT_KEY}
InstrumentationKey={INSTRUMENTATION_KEY}
```

### <a name="logging-example-with-command-line-arguments"></a>Beispiel für die Protokollierung mit Befehlszeilenargumenten

Der folgende Befehl legt in `Logging:Console:LogLevel` die Protokollierungsstufe auf [`Information`](https://msdn.microsoft.com) fest. 

```bash
docker run --rm -it -p 5000:5000 --memory 6g --cpus 2 \
--mount type=bind,src=c:\input,target=/input \
--mount type=bind,src=c:\output,target=/output \
mcr.microsoft.com/azure-cognitive-services/luis:latest \
Eula=accept \
Billing={BILLING_ENDPOINT} \
ApiKey={ENDPOINT_KEY} \
Logging:Console:LogLevel=Information
```

### <a name="logging-example-with-environment-variable"></a>Beispiel für die Protokollierung mit Umgebungsvariablen

Der folgende Befehl legt mithilfe der Umgebungsvariable `Logging:Console:LogLevel` die Protokollierungsstufe auf [`Information`](https://msdn.microsoft.com) fest. 

```bash
SET Logging:Console:LogLevel=Information
docker run --rm -it -p 5000:5000 --memory 6g --cpus 2 \
--mount type=bind,src=c:\input,target=/input \
--mount type=bind,src=c:\output,target=/output \
mcr.microsoft.com/azure-cognitive-services/luis:latest \
Eula=accept \
Billing={BILLING_ENDPOINT} \
ApiKey={APPLICATION_ID} \
```

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen finden Sie unter [Installieren und Ausführen von Containern](luis-container-howto.md).
* Unter [Häufig gestellte Fragen (FAQ)](luis-resources-faq.md) finden Sie Informationen zum Beheben von Problemen im Zusammenhang mit LUIS-Funktionen.
* Verwenden weiterer [Cognitive Services-Container](../cognitive-services-container-support.md)
