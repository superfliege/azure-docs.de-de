---
title: Handbuch zur Problembehandlung bei der Bereitstellung von Azure Machine Learning | Microsoft-Dokumentation
description: Handbuch zur Problembehandlung bei der Bereitstellung und Diensterstellung
services: machine-learning
author: aashishb
ms.author: aashishb
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 11/16/2017
ms.openlocfilehash: 4cf86466d5fca4f5095c67a8400643bff29bb56c
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2018
ms.locfileid: "35637425"
---
# <a name="troubleshooting-service-deployment-and-environment-setup"></a>Problembehandlung bei der Dienstbereitstellung und Umgebungseinrichtung
Folgende Informationen können dabei helfen, beim Einrichten der Modellverwaltungsumgebung die Ursache von Fehlern zu finden.

## <a name="model-management-environment"></a>Modellverwaltungsumgebung
### <a name="contributor-permission-required"></a>Berechtigung „Mitwirkender“ erforderlich
Sie benötigen den Zugriffstyp „Mitwirkender“ für das Abonnement oder die Ressourcengruppe, um einen Cluster für die Bereitstellung Ihrer Webdienste einzurichten.

### <a name="resource-availability"></a>Ressourcenverfügbarkeit
In Ihrem Abonnement müssen genügend Ressourcen verfügbar sein, damit Sie die Umgebungsressourcen bereitstellen können.

### <a name="subscription-caps"></a>Abonnementobergrenzen
Möglicherweise wurde für Ihr Abonnement eine Obergrenze in Bezug auf die Abrechnung festgelegt, die verhindert, dass Sie die Umgebungsressourcen bereitstellen können. Entfernen Sie diese Obergrenze, um die Bereitstellung zu ermöglichen.

### <a name="enable-debug-and-verbose-options"></a>Aktivieren der Optionen „debug“ und „verbose“
Verwenden Sie die Flags `--debug` und `--verbose` im setup-Befehl, um Debug- und Ablaufverfolgungsinformationen anzuzeigen, während die Umgebung bereitgestellt wird.

```
az ml env setup -l <location> -n <name> -c --debug --verbose 
```

## <a name="service-deployment"></a>Dienstbereitstellung
Die folgenden Informationen sollen Sie beim Ermitteln der Ursache von Fehlern beim Bereitstellen oder beim Aufrufen des Webdiensts unterstützen.

### <a name="service-logs"></a>Dienstprotokolle
Die Option `logs` der Dienst-Befehlszeilenschnittstelle liefert Protokolldaten von Docker und Kubernetes.

```
az ml service logs realtime -i <web service id>
```

Weitere Protokolleinstellungen erhalten Sie mithilfe der Option `--help` (oder `-h`).

```
az ml service logs realtime -h
```

### <a name="debug-and-verbose-options"></a>Optionen „debug“ und „verbose“
Verwenden Sie das Flag `--debug`, um Debugprotokolle anzuzeigen, während der Dienst bereitgestellt wird.

```
az ml service create realtime -m <modelfile>.pkl -f score.py -n <service name> -r python --debug
```

Verwenden Sie das Flag `--verbose`, um zusätzliche Details anzuzeigen, während der Dienst bereitgestellt wird.

```
az ml service create realtime -m <modelfile>.pkl -f score.py -n <service name> -r python --verbose
```

### <a name="enable-request-logging-in-app-insights"></a>Aktivieren der Anforderungsprotokollierung in App Insights
Legen Sie das Flag `-l` beim Erstellen eines Webdiensts auf „true“ fest, um die Protokollierung auf der Anforderungsebene zu aktivieren. Die Anforderungsprotokolle werden in die App Insights-Instanz für Ihre Umgebung in Azure geschrieben. Verwenden Sie für die Suche nach dieser Instanz den Umgebungsnamen, den Sie bei Verwendung des Befehls `az ml env setup` verwendet haben.

- Legen Sie `-l` auf „true“ fest, wenn Sie den Dienst erstellen.
- Öffnen Sie App Insights im Azure-Portal. Suchen Sie anhand Ihres Umgebungsnamens nach der App Insights-Instanz.
- Klicken Sie in App Insights im oberen Menü auf „Suchen“, um die Ergebnisse anzuzeigen.
- Alternativ können Sie auch zu `Analytics` > `Exceptions` > `exceptions take | 10` navigieren.


### <a name="add-error-handling-in-scoring-script"></a>Hinzufügen der Fehlerbehandlung im Bewertungsskript
Verwenden Sie die Ausnahmebehandlung der Funktion **run** Ihres Skripts `scoring.py`, um die Fehlermeldung als Teil Ihrer Webdienstausgabe zurückzugeben.

Beispiel für Python:
```
    try:
        <code to load model and score>
   except Exception as e:
        return(str(e))
```

## <a name="other-common-problems"></a>Andere allgemeine Probleme
- Falls auf einem Windows-Computer bei der pip-Installation von „azure-cli-ml“ der Fehler `cannot find the path specified` auftritt, müssen Sie die Unterstützung langer Pfade aktivieren. Siehe https://blogs.msdn.microsoft.com/jeremykuhne/2016/07/30/net-4-6-2-and-long-paths-on-windows-10/. 
- Wenn der Befehl `env setup` den Fehler `LocationNotAvailableForResourceType` ausgibt, verwenden Sie wahrscheinlich den falschen Standort (Region) für die Ressourcen für maschinelles Lernen. Stellen Sie sicher, dass der mit dem Parameter `-l` angegebene Standort `eastus2`, `westcentralus` oder `australiaeast` lautet.
- Wenn der Befehl `env setup` den Fehler `Resource quota limit exceeded` ausgibt, stellen Sie sicher, dass Sie über ausreichende verfügbare Kerne in Ihrem Abonnement verfügen und dass Ihre Ressourcen nicht in anderen Vorgängen aufgebraucht werden.
- Wenn der Befehl `env setup` den Fehler `Invalid environment name. Name must only contain lowercase alphanumeric characters` ausgibt, stellen Sie sicher, dass der Dienstname keine Großbuchstaben, Symbole oder Unterstriche (_) enthält (wie in *myenvironment*).
- Wenn der Befehl `service create` den Fehler `Service Name: [service_name] is invalid. The name of a service must consist of lower case alphanumeric characters (etc.)` ausgibt, stellen Sie sicher, dass der Dienstname zwischen 3 und 32 Zeichen lang ist, mit alphanumerischen Zeichen beginnt und endet und keine Großbuchstaben sowie keine Symbole außer Bindestrich (-), Punkt (. ) oder Unterstrich (_) enthält (wie in *my_webservice*).
- Sollte beim Aufrufen des Webdiensts der Fehler `502 Bad Gateway` auftreten, wiederholen Sie den Vorgang. Der Fehler bedeutet in der Regel, dass der Container noch nicht im Cluster bereitgestellt wurde.
- Wenn beim Erstellen eines Diensts der Fehler `CrashLoopBackOff` auftritt, überprüfen Sie Ihre Protokolle. Dieser Fehler ist in der Regel auf fehlende Abhängigkeiten in der Funktion **Init** zurückzuführen.
