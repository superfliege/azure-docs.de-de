---
title: Konfigurieren von Containern
titlesuffix: Face - Azure Cognitive Services
description: Konfigurationseinstellungen für Container
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: conceptual
ms.date: 11/14/2018
ms.author: diberry
ms.openlocfilehash: 30546d31e96d7d7fa1009f16a50fe8fda12ead67
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2018
ms.locfileid: "53105103"
---
# <a name="configure-containers"></a>Konfigurieren von Containern

Der Container für die Gesichtserkennung nutzt ein allgemeines Konfigurationsframework, sodass Sie ganz einfach Aspekte wie Speicher, Protokollierung und Telemetrie sowie Sicherheitseinstellungen für Ihre Container konfigurieren und verwalten können.

## <a name="configuration-settings"></a>Konfigurationseinstellungen

Die Konfigurationseinstellungen im Container für die Gesichtserkennung sind hierarchisch, und alle Container verwenden eine Hierarchie mit folgender Hauptstruktur:

* [ApiKey](#apikey-configuration-setting)
* [ApplicationInsights](#applicationinsights-configuration-settings)
* [Authentifizierung](#authentication-configuration-settings)
* [Abrechnung](#billing-configuration-setting)
* [CloudAI](#cloudai-configuration-settings)
* [Eula](#eula-configuration-setting)
* [Fluentd](#fluentd-configuration-settings)
* [Protokollierung](#logging-configuration-settings)
* [Mounts](#mounts-configuration-settings)

Verwenden Sie entweder [Umgebungsvariablen](#configuration-settings-as-environment-variables) oder [Befehlszeilenargumente](#configuration-settings-as-command-line-arguments), um Konfigurationseinstellungen anzugeben, wenn Sie einen Container für die Gesichtserkennung instanziieren.

Die Werte für Umgebungsvariablen haben Vorrang vor den Werten für Befehlszeilenargumente, die wiederum Vorrang vor den Standardwerten für das Containerimage haben. Wenn Sie also in einer Umgebungsvariable und einem Befehlszeilenargument unterschiedliche Werte für die gleiche Konfigurationseinstellung (beispielsweise `Logging:Disk:LogLevel`) angeben und anschließend einen Container instanziieren, wird für diesen der Wert aus der Umgebungsvariable verwendet.

### <a name="configuration-settings-as-environment-variables"></a>Konfigurationseinstellungen als Umgebungsvariablen

Sie können die [ASP.NET Core-Syntax für Umgebungsvariablen](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.1&tabs=basicconfiguration#configuration-by-environment) verwenden, um Konfigurationseinstellungen anzugeben.

Der Container liest Benutzerumgebungsvariablen, wenn er instanziiert wird. Ist eine Umgebungsvariable vorhanden, setzt deren Wert den Standardwert für die angegebene Konfigurationseinstellung außer Kraft. Die Verwendung von Umgebungsvariablen hat den Vorteil, dass vor dem Instanziieren von Containern mehrere Konfigurationseinstellungen festgelegt werden und mehrere Container automatisch den gleichen Satz von Konfigurationseinstellungen verwenden können.

Die folgenden Befehle verwenden beispielsweise eine Umgebungsvariable, um die Protokollierungsstufe der Konsole auf [LogLevel.Information](https://msdn.microsoft.com) festzulegen und anschließend einen Container auf der Grundlage des Containerimages für die Gesichtserkennung zu instanziieren. Der Wert der Umgebungsvariable überschreibt die Standardkonfigurationseinstellung.

  ```Docker
  SET Logging:Console:LogLevel=Information
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 containerpreview.azurecr.io/microsoft/cognitive-services-face Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/face/v1.0 ApiKey=0123456789
  ```

### <a name="configuration-settings-as-command-line-arguments"></a>Konfigurationseinstellungen als Befehlszeilenargumente

Sie können die [ASP.NET Core-Syntax für Befehlszeilenargumente](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.1&tabs=basicconfiguration#arguments) verwenden, um Konfigurationseinstellungen anzugeben.

Sie können Konfigurationseinstellungen im optionalen Parameter `ARGS` des Befehls [docker run](https://docs.docker.com/engine/reference/commandline/run/) angeben, mit dem ein Container auf der Grundlage eines heruntergeladenen Containerimages instanziiert wird. Die Verwendung von Befehlszeilenargumenten hat den Vorteil, dass jeder Container einen anderen benutzerdefinierten Satz von Konfigurationseinstellungen verwenden kann.

Der folgende Befehl instanziiert beispielsweise einen Container auf der Grundlage des Containerimages für die Gesichtserkennung und legt die Protokollierungsstufe der Konsole auf „LogLevel.Information“ fest, sodass die Standardkonfigurationseinstellung überschrieben wird.

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 containerpreview.azurecr.io/microsoft/cognitive-services-face Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/face/v1.0 ApiKey=0123456789 Logging:Console:LogLevel=Information
  ```

## <a name="apikey-configuration-setting"></a>Konfigurationseinstellung „ApiKey“

Die Konfigurationseinstellung `ApiKey` gibt den Konfigurationsschlüssel der Gesichtserkennungsressource in Azure an, der zum Nachverfolgen der Abrechnungsinformationen für den Container verwendet wird. Sie müssen einen Wert für diese Konfigurationseinstellung angeben, und bei dem Wert muss es sich um einen gültigen Konfigurationsschlüssel für die Gesichtserkennungsressource handeln, die für die Konfigurationseinstellung [`Billing`](#billing-configuration-setting) angegeben wurde.

> [!IMPORTANT]
> Die Konfigurationseinstellungen [`ApiKey`](#apikey-configuration-setting), [`Billing`](#billing-configuration-setting) und [`Eula`](#eula-configuration-setting) werden gemeinsam verwendet, und Sie müssen gültige Werte für alle drei angeben, da der Container andernfalls nicht startet. Weitere Informationen zum Instanziieren eines Containers mithilfe dieser Konfigurationseinstellungen finden Sie unter [Abrechnung](face-how-to-install-containers.md#billing).

## <a name="applicationinsights-configuration-settings"></a>Konfigurationseinstellungen für Application Insights

Mithilfe der Konfigurationseinstellungen im Abschnitt `ApplicationInsights` können Sie Ihrem Container Unterstützung für [Azure Application Insights](https://docs.microsoft.com/azure/application-insights)-Telemetriedaten hinzufügen. Application Insights ermöglicht eine eingehende Überwachung Ihrer Container bis hinunter zur Codeebene. Sie können ganz einfach die Verfügbarkeit, Leistung und Nutzung Ihrer Container überwachen. Außerdem können Sie schnell Fehler in Ihrem Container erkennen und diagnostizieren, ohne darauf warten zu müssen, bis diese von Benutzern gemeldet werden.

In der folgenden Tabelle werden die Konfigurationseinstellungen beschrieben, die unter dem Abschnitt `ApplicationInsights` unterstützt werden.

| NAME | Datentyp | BESCHREIBUNG |
|------|-----------|-------------|
| `InstrumentationKey` | Zeichenfolge | Der Instrumentierungsschlüssel der Application Insights-Instanz, an die Telemetriedaten für den Container gesendet werden. Weitere Informationen finden Sie unter [Application Insights für ASP.NET Core](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core). |

## <a name="authentication-configuration-settings"></a>Konfigurationseinstellungen für die Authentifizierung

Die `Authentication`-Konfigurationseinstellungen stellen Azure-Sicherheitsoptionen für Ihren Container bereit. Die Konfigurationseinstellungen in diesem Abschnitt sind zwar verfügbar, der Abschnitt wird vom Container für die Gesichtserkennung jedoch nicht verwendet.

| NAME | Datentyp | BESCHREIBUNG |
|------|-----------|-------------|
| `Storage` | Zeichenfolge | Der Instrumentierungsschlüssel der Application Insights-Instanz, an die Telemetriedaten für den Container gesendet werden. Weitere Informationen finden Sie unter [Application Insights für ASP.NET Core](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core). |

## <a name="billing-configuration-setting"></a>Konfigurationseinstellung „Billing“

Die Konfigurationseinstellung `Billing` gibt den Endpunkt-URI der Gesichtserkennungsressource in Azure an, der zum Messen der Abrechnungsinformationen für den Container verwendet wird. Sie müssen einen Wert für diese Konfigurationseinstellung angeben, und bei dem Wert muss es sich um einen gültigen URI-Endpunkt für eine Gesichtserkennungsressource in Azure handeln.

> [!IMPORTANT]
> Die Konfigurationseinstellungen [`ApiKey`](#apikey-configuration-setting), [`Billing`](#billing-configuration-setting) und [`Eula`](#eula-configuration-setting) werden gemeinsam verwendet, und Sie müssen gültige Werte für alle drei angeben, da der Container andernfalls nicht startet. Weitere Informationen zum Instanziieren eines Containers mithilfe dieser Konfigurationseinstellungen finden Sie unter [Abrechnung](face-how-to-install-containers.md#billing).

## <a name="cloudai-configuration-settings"></a>Konfigurationseinstellungen unter „CloudAI“

Die Konfigurationseinstellungen im Abschnitt `CloudAI` umfassen containerspezifische Optionen für Ihren Container. Die folgenden Einstellungen und Objekte werden beim Container für die Gesichtserkennung im Abschnitt `CloudAI` unterstützt:

| NAME | Datentyp | BESCHREIBUNG |
|------|-----------|-------------|
| `Storage` | Objekt | Das vom Container für die Gesichtserkennung verwendete Speicherszenario. Weitere Informationen zu Speicherszenarien und den zugehörigen Einstellungen für das `Storage`-Objekt finden Sie unter [Einstellungen für das Speicherszenario](#storage-scenario-settings). |

### <a name="storage-scenario-settings"></a>Einstellungen für das Speicherszenario

Der Container für die Gesichtserkennung kann Blob-, Cache-, Meta- und Warteschlangendaten speichern. So werden beispielsweise Trainingsindizes und -ergebnisse für eine große Personengruppe als Blobdaten gespeichert. Der Container für die Gesichtserkennung bietet zwei unterschiedliche Speicherszenarien für die Interaktion mit diesen Datentypen und deren Speicherung:

* Arbeitsspeicher  
  Alle vier Datentypen werden im Arbeitsspeicher gespeichert. Sie sind weder verteilt noch persistent. Wenn der Container für die Gesichtserkennung beendet oder entfernt wird, gehen sämtliche Daten für den betreffenden Container im Speicher verloren.  
  Dies ist das Standardspeicherszenario für den Container für die Gesichtserkennung.
* Azure  
  Der Container für die Gesichtserkennung verwendet Azure Storage und Azure Cosmos DB, um diese vier Datentypen auf permanente Speicher zu verteilen. Blob- und Warteschlangendaten werden durch Azure Storage verarbeitet. Meta- und Cachedaten werden von Azure Cosmos DB mithilfe der MongoDB-API verarbeitet. Wenn der Container für die Gesichtserkennung beendet oder entfernt wird, werden sämtliche gespeicherte Daten für den betreffenden Container in Azure Storage und Azure Cosmos DB gespeichert.  
  Für die Ressourcen, die beim Azure-Speicherszenario verwendet werden, gelten die folgenden zusätzlichen Anforderungen:
  * Die Azure Storage-Ressource muss den Kontotyp StorageV2 verwenden.
  * Die Azure Cosmos DB-Ressource muss die MongoDB-API verwenden.

Die Speicherszenarien und die zugehörigen Konfigurationseinstellungen werden vom `Storage`-Objekt unter dem Konfigurationsabschnitt `CloudAI` verwaltet. Die folgenden Konfigurationseinstellungen sind im `Storage`-Objekt verfügbar:

| NAME | Datentyp | BESCHREIBUNG |
|------|-----------|-------------|
| `StorageScenario` | Zeichenfolge | Das vom Container unterstützte Speicherszenario. Folgende Werte sind verfügbar:<br/>`Memory`: Standardwert. Der Container verwendet nicht persistenten und nicht verteilten Speicher im Arbeitsspeicher für die temporäre Nutzung durch einen einzelnen Knoten. Wenn der Container beendet oder entfernt wird, wird der Speicher für diesen Container zerstört.<br/>`Azure`: Der Container verwendet Azure-Ressourcen für die Speicherung. Wenn der Container beendet oder entfernt wird, wird der Speicher für diesen Container beibehalten.|
| `ConnectionStringOfAzureStorage` | Zeichenfolge | Die Verbindungszeichenfolge für die Azure Storage-Ressource, die vom Container verwendet wird.<br/>Diese Einstellung gilt nur, wenn `Azure` für die Konfigurationseinstellung `StorageScenario` angegeben wurde. |
| `ConnectionStringOfCosmosMongo` | Zeichenfolge | Die MongoDB-Verbindungszeichenfolge für die Azure Cosmos DB-Ressource, die vom Container verwendet wird.<br/>Diese Einstellung gilt nur, wenn `Azure` für die Konfigurationseinstellung `StorageScenario` angegeben wurde. |

Der folgende Befehl gibt beispielsweise das Azure-Speicherszenario an und stellt Beispielverbindungszeichenfolgen für die Azure Storage- und Cosmos DB-Ressourcen bereit, die zum Speichern von Daten für Container für die Gesichtserkennung verwendet werden.

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 containerpreview.azurecr.io/microsoft/cognitive-services-face Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/face/v1.0 ApiKey=0123456789 CloudAI:Storage:StorageScenario=Azure CloudAI:Storage:ConnectionStringOfCosmosMongo="mongodb://samplecosmosdb:0123456789@samplecosmosdb.documents.azure.com:10255/?ssl=true&replicaSet=globaldb" CloudAI:Storage:ConnectionStringOfAzureStorage="DefaultEndpointsProtocol=https;AccountName=sampleazurestorage;AccountKey=0123456789;EndpointSuffix=core.windows.net"
  ```

Das Speicherszenario wird getrennt von den Ein- und-Ausgabeeinbindungen verarbeitet. Sie können eine Kombination dieser Features für einen einzelnen Container angeben. Der folgende Befehl definiert beispielsweise eine Docker-Bindungseinbindung für den Ordner `D:\Output` auf dem Hostcomputer als Ausgabeeinbindung und instanziiert anschließend einen Container auf der Grundlage des Containerimages für die Gesichtserkennung. Protokolldateien werden dabei im JSON-Format in der Ausgabeeinbindung gespeichert. Der Befehl gibt auch das Azure-Speicherszenario an und stellt Beispielverbindungszeichenfolgen für die Azure Storage- und Cosmos DB-Ressourcen bereit, die zum Speichern von Daten für Container für die Gesichtserkennung verwendet werden.

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 --mount type=bind,source=D:\Output,destination=/output containerpreview.azurecr.io/microsoft/cognitive-services-face Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/face/v1.0 ApiKey=0123456789 Logging:Disk:Format=json CloudAI:Storage:StorageScenario=Azure CloudAI:Storage:ConnectionStringOfCosmosMongo="mongodb://samplecosmosdb:0123456789@samplecosmosdb.documents.azure.com:10255/?ssl=true&replicaSet=globaldb" CloudAI:Storage:ConnectionStringOfAzureStorage="DefaultEndpointsProtocol=https;AccountName=sampleazurestorage;AccountKey=0123456789;EndpointSuffix=core.windows.net"
  ```

## <a name="eula-configuration-setting"></a>Konfigurationseinstellung „Eula“

Die Konfigurationseinstellung `Eula` gibt an, dass Sie die Lizenz für den Container akzeptiert haben. Sie müssen einen Wert für diese Konfigurationseinstellung angeben, und der Wert muss auf `accept` festgelegt werden.

> [!IMPORTANT]
> Die Konfigurationseinstellungen [`ApiKey`](#apikey-configuration-setting), [`Billing`](#billing-configuration-setting) und [`Eula`](#eula-configuration-setting) werden gemeinsam verwendet, und Sie müssen gültige Werte für alle drei angeben, da der Container andernfalls nicht startet. Weitere Informationen zum Instanziieren eines Containers mithilfe dieser Konfigurationseinstellungen finden Sie unter [Abrechnung](face-how-to-install-containers.md#billing).

Cognitive Services-Container werden unter [Ihrer Vereinbarung](https://go.microsoft.com/fwlink/?linkid=2018657) lizenziert und legen Ihre Nutzung von Azure fest. Wenn Sie über keine Vereinbarung zur Nutzung von Azure verfügen, bestätigen Sie, dass Ihre Vereinbarung zur Nutzung von Azure der [Microsoft Online-Abonnementvertrag](https://go.microsoft.com/fwlink/?linkid=2018755) ist (der die [Nutzungsbedingungen für Onlinedienste](https://go.microsoft.com/fwlink/?linkid=2018760) umfasst). Für Vorschauversionen stimmen Sie auch den [ergänzenden Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://go.microsoft.com/fwlink/?linkid=2018815) zu. Durch die Nutzung von Containern stimmen Sie diesen Bedingungen zu.

## <a name="fluentd-configuration-settings"></a>Konfigurationseinstellungen für Fluentd

Der Abschnitt `Fluentd` dient zur Verwaltung von Konfigurationseinstellungen für [Fluentd](https://www.fluentd.org) (ein Open-Source-Datensammler für eine vereinheitlichte Protokollierung). Der Container für die Gesichtserkennung enthält einen Fluentd-Protokollanbieter, der es Ihrem Container ermöglicht, Protokolldaten (und optional auch Metrikdaten) auf einen Fluentd-Server zu schreiben.

In der folgenden Tabelle werden die Konfigurationseinstellungen beschrieben, die unter dem Abschnitt `Fluentd` unterstützt werden.

| NAME | Datentyp | BESCHREIBUNG |
|------|-----------|-------------|
| `Host` | Zeichenfolge | Die IP-Adresse oder der DNS-Hostname des Fluentd-Servers. |
| `Port` | Ganze Zahl  | Der Port des Fluentd-Servers.<br/> Standardwert: 24224 |
| `HeartbeatMs` | Ganze Zahl  | Das Heartbeatintervall in Millisekunden. Wurde bis zum Ablauf dieses Intervalls kein Ereignisdatenverkehr gesendet, wird ein Heartbeat an den Fluentd-Server gesendet. Standardwert: 60.000 Millisekunden (eine Minute) |
| `SendBufferSize` | Ganze Zahl  | Der für Sendevorgänge zugeordnete Netzwerkpufferspeicher (in Byte). Standardwert: 32.768 Byte (32 KB) |
| `TlsConnectionEstablishmentTimeoutMs` | Ganze Zahl  | Das Timeout (in Millisekunden) für die Herstellung einer SSL/TLS-Verbindung mit dem Fluentd-Server. Der Standardwert beträgt 10.000 Millisekunden (zehn Sekunden).<br/> Wenn `UseTLS` auf FALSE festgelegt ist, wird dieser Wert ignoriert. |
| `UseTLS` | Boolescher Wert | Gibt an, ob der Container für die Kommunikation mit dem Fluentd-Server SSL/TLS verwenden soll. Der Standardwert ist „false“. |

## <a name="logging-configuration-settings"></a>Konfigurationseinstellungen für die Protokollierung

Die `Logging`-Konfigurationseinstellungen dienen zur Verwaltung der ASP.NET Core-Protokollierungsunterstützung für Ihren Container. Sie können für Ihren Container die gleichen Konfigurationseinstellungen und Werte verwenden wie für eine ASP.NET Core-Anwendung. Der Container für die Gesichtserkennung unterstützt folgende Protokollanbieter:

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

Weitere Informationen zum Konfigurieren der ASP.NET Core-Protokollierungsunterstützung finden Sie unter [Protokollierung in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#settings-file-configuration).

## <a name="mounts-configuration-settings"></a>Konfigurationseinstellungen für Einbindungen

Die von der Gesichtserkennung bereitgestellten Docker-Container sind sowohl zustandslos als auch unveränderlich. In einem Container erstellte Dateien werden also auf einer schreibbaren Containerebene gespeichert, die nur während der Ausführung des Containers vorhanden ist und auf die nicht ohne Weiteres zugegriffen werden kann. Wird der Container beendet oder entfernt, gehen auch die darin erstellten Dateien verloren.

Da es sich jedoch um Docker-Container handelt, können Sie Docker-Speicheroptionen wie Volumes und Bindungseinbindungen verwenden, um gespeicherte Daten außerhalb des Containers zu lesen und zu schreiben, sofern der Container dies unterstützt. Weitere Informationen zum Angeben und Verwalten von Docker-Speicheroptionen finden Sie unter [Manage data in Docker](https://docs.docker.com/storage/) (Verwalten von Daten in Docker).

> [!NOTE]
> Die Werte für diese Konfigurationseinstellungen müssen in der Regel nicht geändert werden. Verwenden Sie die in diesen Konfigurationseinstellungen angegebenen Werte stattdessen als Ziele, wenn Sie Eingabe- und Ausgabeeinbindungen für Ihren Container angeben. Weitere Informationen zum Angeben von Eingabe- und Ausgabeeinbindungen finden Sie unter [Eingabe- und Ausgabeeinbindungen](#input-and-output-mounts).

In der folgenden Tabelle werden die Konfigurationseinstellungen beschrieben, die unter dem Abschnitt `Mounts` unterstützt werden.

| NAME | Datentyp | BESCHREIBUNG |
|------|-----------|-------------|
| `Input` | Zeichenfolge | Das Ziel der Eingabeeinbindung. Standardwert: `/input`. |
| `Output` | Zeichenfolge | Das Ziel der Ausgabeeinbindung. Standardwert: `/output`. |

### <a name="input-and-output-mounts"></a>Eingabe- und Ausgabeeinbindungen

Standardmäßig kann jeder Container eine *Eingabeeinbindung* unterstützen, aus der der Container Daten lesen kann, und eine *Ausgabeeinbindung*, in die der Container Daten schreiben kann. Container müssen allerdings nicht zwingend Eingabe- oder Ausgabeeinbindungen unterstützen, und jeder Container kann zusätzlich zu den vom Container für die Gesichtserkennung unterstützten Protokollierungsoptionen Eingabe- und Ausgabeeinbindungen für containerspezifische Zwecke verwenden.

Der Container für die Gesichtserkennung unterstützt keine Eingabeeinbindungen, Ausgabeeinbindungen werden jedoch optional unterstützt.

Sie können eine Eingabe- oder Ausgabeeinbindung mithilfe der Option `--mount` im Befehl [docker run](https://docs.docker.com/engine/reference/commandline/run/) angeben, mit dem ein Container auf der Grundlage eines heruntergeladenen Containerimages instanziiert wird. Standardmäßig verwendet die Eingabeeinbindung das Ziel `/input` und die Ausgabeeinbindung das Ziel `/output`. In der Option `--mount` kann jede beliebige Docker-Speicheroption angegeben werden, die für den Docker-Containerhost verfügbar ist.

Der folgende Befehl definiert beispielsweise eine Docker-Bindungseinbindung für den Ordner `D:\Output` auf dem Hostcomputer als Ausgabeeinbindung und instanziiert anschließend einen Container auf der Grundlage des Containerimages für die Gesichtserkennung. Protokolldateien werden dabei im JSON-Format in der Ausgabeeinbindung gespeichert.

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 --mount type=bind,source=D:\Output,destination=/output containerpreview.azurecr.io/microsoft/cognitive-services-face Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/face/v1.0 ApiKey=0123456789 Logging:Disk:Format=json
  ```

Der Container für die Gesichtserkennung verwendet die Eingabe- oder Ausgabeeinbindungen nicht zum Speichern von Trainings- oder Datenbankdaten. Stattdessen stellt der Container für die Gesichtserkennung Speicherszenarien für die Verwaltung von Trainings-und Datenbankdaten bereit. Weitere Informationen zur Verwendung von Speicherszenarien finden Sie unter [Einstellungen für Speicherszenarien](#storage-scenario-settings).
