---
title: Konfigurieren von Containern – maschinelles Sehen
titlesuffix: Azure Cognitive Services
description: Konfigurieren Sie verschiedene Einstellungen für Texterkennungscontainer für maschinelles Sehen.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: conceptual
ms.date: 01/22/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 97de65acf724d12afd131ede25713e8f29d30bad
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54477633"
---
# <a name="configure-recognize-text-containers"></a>Konfigurieren von Texterkennungscontainern

Maschinelles Sehen stellt für den Texterkennungscontainer ein allgemeines Konfigurationsframework bereit, sodass Sie ganz einfach Aspekte wie Speicher, Protokollierung und Telemetrie sowie Sicherheitseinstellungen für Ihre Container konfigurieren und verwalten können.

## <a name="configuration-settings"></a>Konfigurationseinstellungen

Die Konfigurationseinstellungen in Containern für Maschinelles Sehen sind hierarchisch, und alle Container verwenden eine Hierarchie mit folgender Hauptstruktur:

* [ApiKey](#apikey-configuration-setting)
* [ApplicationInsights](#applicationinsights-configuration-settings)
* [Authentifizierung](#authentication-configuration-settings)
* [Abrechnung](#billing-configuration-setting)
* [Eula](#eula-configuration-setting)
* [Fluentd](#fluentd-configuration-settings)
* [Anmeldeinformationseinstellungen für HTTP-Proxy](#http-proxy-credentials-settings)
* [Protokollierung](#logging-configuration-settings)
* [Mounts](#mounts-configuration-settings)

Verwenden Sie entweder [Umgebungsvariablen](#configuration-settings-as-environment-variables) oder [Befehlszeilenargumente](#configuration-settings-as-command-line-arguments), um Konfigurationseinstellungen anzugeben, wenn Sie über Container für Maschinelles Sehen einen Container instanziieren.

Die Werte für Umgebungsvariablen haben Vorrang vor den Werten für Befehlszeilenargumente, welche wiederum Vorrang vor den Standardwerten für das Containerimage haben. Wenn Sie also in einer Umgebungsvariable und einem Befehlszeilenargument unterschiedliche Werte für die gleiche Konfigurationseinstellung (beispielsweise `Logging:Disk:LogLevel`) angeben und anschließend einen Container instanziieren, wird für diesen der Wert aus der Umgebungsvariable verwendet.

### <a name="configuration-settings-as-environment-variables"></a>Konfigurationseinstellungen als Umgebungsvariablen

Sie können die [ASP.NET Core-Syntax für Umgebungsvariablen](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.1&tabs=basicconfiguration#environment-variables-configuration-provider) verwenden, um Konfigurationseinstellungen anzugeben.

Der Container liest Benutzerumgebungsvariablen, wenn er instanziiert wird. Ist eine Umgebungsvariable vorhanden, setzt deren Wert den Standardwert für die angegebene Konfigurationseinstellung außer Kraft. Die Verwendung von Umgebungsvariablen hat den Vorteil, dass vor dem Instanziieren von Containern mehrere Konfigurationseinstellungen festgelegt werden und mehrere Container automatisch den gleichen Satz von Konfigurationseinstellungen verwenden können.

Die folgenden Befehle verwenden beispielsweise eine Umgebungsvariable, um die Protokollierungsstufe der Konsole auf [LogLevel.Information](https://msdn.microsoft.com) festzulegen und anschließend einen Container auf der Grundlage des Images für den Texterkennungscontainer zu instanziieren. Der Wert der Umgebungsvariablen überschreibt die Standardkonfigurationseinstellung.

  ```Docker
  SET Logging:Console:LogLevel=Information
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/vision/v1.0 ApiKey=0123456789
  ```

### <a name="configuration-settings-as-command-line-arguments"></a>Konfigurationseinstellungen als Befehlszeilenargumente

Sie können die [ASP.NET Core-Syntax für Befehlszeilenargumente](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.1&tabs=basicconfiguration#arguments) verwenden, um Konfigurationseinstellungen anzugeben.

Sie können Konfigurationseinstellungen im optionalen Parameter `ARGS` des Befehls [docker run](https://docs.docker.com/engine/reference/commandline/run/) angeben, mit dem ein Container auf der Grundlage eines heruntergeladenen Containerimages instanziiert wird. Die Verwendung von Befehlszeilenargumenten hat den Vorteil, dass jeder Container einen anderen benutzerdefinierten Satz von Konfigurationseinstellungen verwenden kann.

Der folgende Befehl instanziiert beispielsweise einen Container auf der Grundlage des Images für den Texterkennungscontainer und legt die Protokollierungsstufe der Konsole auf „LogLevel.Information“ fest, wodurch die Standardkonfigurationseinstellung überschrieben wird.

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/vision/v1.0 ApiKey=0123456789 Logging:Console:LogLevel=Information
  ```

## <a name="apikey-configuration-setting"></a>Konfigurationseinstellung für den API-Schlüssel

Die `ApiKey`-Konfigurationseinstellung gibt den Konfigurationsschlüssel der Maschinelles Sehen-Ressource in Azure an, der zum Nachverfolgen der Abrechnungsinformationen für den Container verwendet wird. Sie müssen einen Wert für diese Konfigurationseinstellung angeben, und bei dem Wert muss es sich um einen gültigen Konfigurationsschlüssel für die Maschinelles Sehen-Ressource handeln, die für Konfigurationseinstellung [`Billing`](#billing-configuration-setting) angegeben wurde.

> [!IMPORTANT]
> Die Konfigurationseinstellungen [`ApiKey`](#apikey-configuration-setting), [`Billing`](#billing-configuration-setting) und [`Eula`](#eula-configuration-setting) werden gemeinsam verwendet, und Sie müssen gültige Werte für alle drei angeben, da der Container andernfalls nicht startet. Weitere Informationen zum Instanziieren eines Containers mithilfe dieser Konfigurationseinstellungen finden Sie unter [Abrechnung](computer-vision-how-to-install-containers.md#billing).

## <a name="applicationinsights-configuration-settings"></a>Konfigurationseinstellungen für Application Insights

Mithilfe der Konfigurationseinstellungen im Abschnitt `ApplicationInsights` können Sie Ihrem Container Unterstützung für [Azure Application Insights](https://docs.microsoft.com/azure/application-insights)-Telemetriedaten hinzufügen. Application Insights ermöglicht eine eingehende Überwachung Ihrer Container bis hinunter zur Codeebene. Sie können ganz einfach die Verfügbarkeit, Leistung und Nutzung Ihrer Container überwachen. Außerdem können Sie schnell Fehler in Ihrem Container erkennen und diagnostizieren, ohne darauf warten zu müssen, bis diese von Benutzern gemeldet werden.

In der folgenden Tabelle werden die Konfigurationseinstellungen beschrieben, die unter dem Abschnitt `ApplicationInsights` unterstützt werden.

| NAME | Datentyp | BESCHREIBUNG |
|------|-----------|-------------|
| `InstrumentationKey` | Zeichenfolge | Der Instrumentierungsschlüssel der Application Insights-Instanz, an die Telemetriedaten für den Container gesendet werden. Weitere Informationen finden Sie unter [Application Insights für ASP.NET Core](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core). |

## <a name="authentication-configuration-settings"></a>Konfigurationseinstellungen für die Authentifizierung

Die `Authentication`-Konfigurationseinstellungen stellen Azure-Sicherheitsoptionen für Ihren Container bereit. Die Konfigurationseinstellungen in diesem Abschnitt sind zwar verfügbar, der Abschnitt wird vom Texterkennungscontainer jedoch nicht verwendet.

## <a name="billing-configuration-setting"></a>Konfigurationseinstellung „Billing“

Die `Billing`-Konfigurationseinstellung gibt den Endpunkt-URI der Maschinelles Sehen-Ressource in Azure an, der zum Messen der Abrechnungsinformationen für den Container verwendet wird. Sie müssen einen Wert für diese Konfigurationseinstellung angeben, und bei dem Wert muss es sich um einen gültigen URI-Endpunkt für eine Maschinelles Sehen-Ressource in Azure handeln.

> [!IMPORTANT]
> Die Konfigurationseinstellungen [`ApiKey`](#apikey-configuration-setting), [`Billing`](#billing-configuration-setting) und [`Eula`](#eula-configuration-setting) werden gemeinsam verwendet, und Sie müssen gültige Werte für alle drei angeben, da der Container andernfalls nicht startet. Weitere Informationen zum Instanziieren eines Containers mithilfe dieser Konfigurationseinstellungen finden Sie unter [Abrechnung](computer-vision-how-to-install-containers.md#billing).

## <a name="eula-configuration-setting"></a>Konfigurationseinstellung „Eula“

Die Konfigurationseinstellung `Eula` gibt an, dass Sie die Lizenz für den Container akzeptiert haben. Sie müssen einen Wert für diese Konfigurationseinstellung angeben, und der Wert muss auf `accept` festgelegt werden.

> [!IMPORTANT]
> Die Konfigurationseinstellungen [`ApiKey`](#apikey-configuration-setting), [`Billing`](#billing-configuration-setting) und [`Eula`](#eula-configuration-setting) werden gemeinsam verwendet, und Sie müssen gültige Werte für alle drei angeben, da der Container andernfalls nicht startet. Weitere Informationen zum Instanziieren eines Containers mithilfe dieser Konfigurationseinstellungen finden Sie unter [Abrechnung](computer-vision-how-to-install-containers.md#billing).

Cognitive Services-Container werden unter [Ihrer Vereinbarung](https://go.microsoft.com/fwlink/?linkid=2018657) lizenziert und legen Ihre Nutzung von Azure fest. Wenn Sie über keine Vereinbarung zur Nutzung von Azure verfügen, bestätigen Sie, dass Ihre Vereinbarung zur Nutzung von Azure der [Microsoft Online-Abonnementvertrag](https://go.microsoft.com/fwlink/?linkid=2018755) ist, der die [Nutzungsbedingungen für Onlinedienste](https://go.microsoft.com/fwlink/?linkid=2018760) umfasst. Für Vorschauversionen stimmen Sie auch den [ergänzenden Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://go.microsoft.com/fwlink/?linkid=2018815) zu. Durch die Nutzung von Containern stimmen Sie diesen Bedingungen zu.

## <a name="fluentd-configuration-settings"></a>Konfigurationseinstellungen für Fluentd

Der Abschnitt `Fluentd` dient zur Verwaltung von Konfigurationseinstellungen für [Fluentd](https://www.fluentd.org) (ein Open-Source-Datensammler für eine vereinheitlichte Protokollierung). Maschinelles Sehen-Container enthalten einen Fluentd-Protokollierungsanbieter, der es Ihrem Container ermöglicht, Protokolldaten (und optional auch Metrikdaten) auf einen Fluentd-Server zu schreiben.

In der folgenden Tabelle werden die Konfigurationseinstellungen beschrieben, die unter dem Abschnitt `Fluentd` unterstützt werden.

| NAME | Datentyp | BESCHREIBUNG |
|------|-----------|-------------|
| `Host` | Zeichenfolge | Die IP-Adresse oder der DNS-Hostname des Fluentd-Servers. |
| `Port` | Ganze Zahl  | Der Port des Fluentd-Servers.<br/> Standardwert: 24224 |
| `HeartbeatMs` | Ganze Zahl  | Das Heartbeatintervall in Millisekunden. Wurde bis zum Ablauf dieses Intervalls kein Ereignisdatenverkehr gesendet, wird ein Heartbeat an den Fluentd-Server gesendet. Standardwert: 60.000 Millisekunden (eine Minute) |
| `SendBufferSize` | Ganze Zahl  | Der für Sendevorgänge zugeordnete Netzwerkpufferspeicher (in Byte). Standardwert: 32.768 Byte (32 KB) |
| `TlsConnectionEstablishmentTimeoutMs` | Ganze Zahl  | Das Timeout (in Millisekunden) für die Herstellung einer SSL/TLS-Verbindung mit dem Fluentd-Server. Der Standardwert beträgt 10.000 Millisekunden (zehn Sekunden).<br/> Wenn `UseTLS` auf FALSE festgelegt ist, wird dieser Wert ignoriert. |
| `UseTLS` | Boolescher Wert | Gibt an, ob der Container für die Kommunikation mit dem Fluentd-Server SSL/TLS verwenden soll. Der Standardwert ist „false“. |


## <a name="http-proxy-credentials-settings"></a>Anmeldeinformationseinstellungen für HTTP-Proxy

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-http-proxy.md)]

## <a name="logging-configuration-settings"></a>Konfigurationseinstellungen für die Protokollierung

Die `Logging`-Konfigurationseinstellungen dienen zur Verwaltung der ASP.NET Core-Protokollierungsunterstützung für Ihren Container. Sie können für Ihren Container die gleichen Konfigurationseinstellungen und Werte verwenden wie für eine ASP.NET Core-Anwendung. Von Maschinelles Sehen-Containern werden folgende Protokollanbieter unterstützt:

* [Console](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#console-provider)  
  Der ASP.NET Core-Protokollierungsanbieter `Console`. Alle ASP.NET Core-Konfigurationseinstellungen und Standardwerte für diesen Protokollanbieter werden unterstützt.
* [Debuggen](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#debug-provider)  
  Der ASP.NET Core-Protokollierungsanbieter `Debug`. Alle ASP.NET Core-Konfigurationseinstellungen und Standardwerte für diesen Protokollanbieter werden unterstützt.
* Datenträger  
  Der JSON-Protokollanbieter. Dieser Protokollanbieter schreibt Protokolldaten in die Ausgabeeinbindung.  
  Der Protokollanbieter `Disk` unterstützt folgende Konfigurationseinstellungen:  

  | NAME | Datentyp | BESCHREIBUNG |
  |------|-----------|-------------|
  | `Format` | Zeichenfolge | Das Ausgabeformat für Protokolldateien.<br/> **Hinweis:** Dieser Wert muss auf `json` festgelegt werden, um den Protokollanbieter zu aktivieren. Wenn dieser Wert bei der Containerinstanziierung angegeben wird, ohne eine Ausgabeeinbindung anzugeben, tritt ein Fehler auf. |
  | `MaxFileSize` | Ganze Zahl  | Die maximale Größe einer Protokolldatei (in MB). Wenn die Größe der aktuellen Protokolldatei diesen Wert erreicht oder übersteigt, wird vom Protokollanbieter eine neue Protokolldatei erstellt. Bei Angabe von „-1“ wird die Größe der Protokolldatei nur durch die maximal zulässige Dateigröße für die Ausgabeeinbindung begrenzt (sofern vorhanden). Der Standardwert ist 1. |

Weitere Informationen zum Konfigurieren der ASP.NET Core-Protokollierungsunterstützung finden Sie unter [Protokollierung in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#configuration).

## <a name="mounts-configuration-settings"></a>Konfigurationseinstellungen für Einbindungen

Die von Maschinelles Sehen bereitgestellten Docker-Container sind sowohl zustandslos als auch unveränderlich. In einem Container erstellte Dateien werden also auf einer schreibbaren Containerebene gespeichert, die nur während der Ausführung des Containers vorhanden ist und auf die nicht ohne Weiteres zugegriffen werden kann. Wird der Container beendet oder entfernt, gehen auch die darin erstellten Dateien verloren.

Da es sich jedoch um Docker-Container handelt, können Sie Docker-Speicheroptionen wie Volumes und Bindungseinbindungen verwenden, um gespeicherte Daten außerhalb des Containers zu lesen und zu schreiben, sofern der Container dies unterstützt. Weitere Informationen zum Angeben und Verwalten von Docker-Speicheroptionen finden Sie unter [Manage data in Docker](https://docs.docker.com/storage/) (Verwalten von Daten in Docker).

> [!NOTE]
> Die Werte für diese Konfigurationseinstellungen müssen in der Regel nicht geändert werden. Verwenden Sie die in diesen Konfigurationseinstellungen angegebenen Werte stattdessen als Ziele, wenn Sie Eingabe- und Ausgabeeinbindungen für Ihren Container angeben. Weitere Informationen zum Angeben von Eingabe- und Ausgabeeinbindungen finden Sie unter [Eingabe- und Ausgabeeinbindungen](#input-and-output-mounts).

In der folgenden Tabelle werden die Konfigurationseinstellungen beschrieben, die unter dem Abschnitt `Mounts` unterstützt werden.

| NAME | Datentyp | BESCHREIBUNG |
|------|-----------|-------------|
| `Input` | Zeichenfolge | Das Ziel der Eingabeeinbindung. Standardwert: `/input`. |
| `Output` | Zeichenfolge | Das Ziel der Ausgabeeinbindung. Standardwert: `/output`. |

### <a name="input-and-output-mounts"></a>Eingabe- und Ausgabeeinbindungen

Standardmäßig kann jeder Container eine *Eingabeeinbindung* unterstützen, aus der der Container Daten lesen kann, und eine *Ausgabeeinbindung*, an die der Container Daten schreiben kann. Container müssen allerdings nicht zwingend Eingabe- oder Ausgabeeinbindungen unterstützen, und jeder Container kann zusätzlich zu den von Maschinelles Sehen-Containern unterstützten Protokollierungsoptionen Eingabe- und Ausgabeeinbindungen für containerspezifische Zwecke verwenden.

Der Texterkennungscontainer unterstützt keine Eingabeeinbindungen; Ausgabeeinbindungen werden optional unterstützt.

Sie können eine Eingabe- oder Ausgabeeinbindung mithilfe der Option `--mount` im Befehl [docker run](https://docs.docker.com/engine/reference/commandline/run/) angeben, mit dem ein Container auf der Grundlage eines heruntergeladenen Containerimages instanziiert wird. Standardmäßig verwendet die Eingabeeinbindung das Ziel `/input` und die Ausgabeeinbindung das Ziel `/output`. In der Option `--mount` kann jede beliebige Docker-Speicheroption angegeben werden, die für den Docker-Containerhost verfügbar ist.

Der folgende Befehl definiert beispielsweise eine Docker-Bindungseinbindung für den Ordner `D:\Output` auf dem Hostcomputer als Ausgabeeinbindung und instanziiert anschließend einen Container auf der Grundlage des Images für den Texterkennungscontainer. Protokolldateien werden dabei im JSON-Format in der Ausgabeeinbindung gespeichert.

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 --mount type=bind,source=D:\Output,destination=/output containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/vision/v1.0 ApiKey=0123456789 Logging:Disk:Format=json
  ```

## <a name="next-steps"></a>Nächste Schritte

* Verwenden weiterer [Cognitive Services-Container](../cognitive-services-container-support.md)