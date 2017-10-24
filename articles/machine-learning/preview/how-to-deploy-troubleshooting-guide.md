---
title: Handbuch zur Problembehandlung bei der Bereitstellung von Azure Machine Learning | Microsoft-Dokumentation
description: Handbuch zur Problembehandlung bei der Bereitstellung und Diensterstellung
services: machine-learning
author: raymondl
ms.author: raymondl
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 10/05/2017
ms.openlocfilehash: 066a6a223692055c7855abc63667e345ee8dc2d4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="model-management-troubleshooting"></a>Problembehandlung bei der Modellverwaltung
Die folgenden Informationen sollen Sie beim Ermitteln der Ursache von Fehlern beim Bereitstellen oder beim Aufrufen des Webdiensts unterstützen.
 
## <a name="1-service-logs"></a>1. Dienstprotokolle
Die Option `logs` der Dienst-Befehlszeilenschnittstelle liefert Protokolldaten von Docker und Kubernetes.

```
az ml service logs realtime -i <web service id>
```

Weitere Protokolleinstellungen erhalten Sie mithilfe der Option `--help` (oder `-h`).

```
az ml service logs realtime -h
```

## <a name="2-debug-and-verbose-options"></a>2. Optionen „debug“ und „verbose“
Verwenden Sie das Flag `--debug`, um Debugprotokolle anzuzeigen, während der Dienst bereitgestellt wird.

```
az ml service create realtime -m <modelfile>.pkl -f score.py -n <service name> -r python --debug
```

Verwenden Sie das Flag `--verbose`, um zusätzliche Details anzuzeigen, während der Dienst bereitgestellt wird.

```
az ml service create realtime -m <modelfile>.pkl -f score.py -n <service name> -r python --verbose
```

## <a name="3-app-insights"></a>3. App Insights
Legen Sie das Flag `-l` beim Erstellen eines Webdiensts auf „true“ fest, um die Protokollierung auf der Anforderungsebene zu aktivieren. Die Anforderungsprotokolle werden in die App Insights-Instanz für Ihre Umgebung in Azure geschrieben. Verwenden Sie für die Suche nach dieser Instanz den Umgebungsnamen, den Sie bei Verwendung des Befehls `az ml env setup` verwendet haben.

- Legen Sie `-l` auf „true“ fest, wenn Sie den Dienst erstellen.
- Öffnen Sie App Insights im Azure-Portal. Suchen Sie anhand Ihres Umgebungsnamens nach der App Insights-Instanz.
- Klicken Sie in App Insights im oberen Menü auf „Suchen“, um die Ergebnisse anzuzeigen.
- Alternativ können Sie auch zu `Analytics` > `Exceptions` > `exceptions take | 10` navigieren.


## <a name="4-error-handling-in-script"></a>4. Fehlerbehandlung im Skript
Verwenden Sie die Ausnahmebehandlung der Funktion **run** Ihres Skripts `scoring.py`, um die Fehlermeldung als Teil Ihrer Webdienstausgabe zurückzugeben.

Beispiel für Python:
```
    try:
        <code to load model and score>
   except Exception as e:
        return(str(e))
```

## <a name="5-other-common-problems"></a>5. Andere allgemeine Probleme
- Sollte der Befehl `env setup` nicht erfolgreich sein, vergewissern Sie sich, dass in Ihrem Abonnement genügend Kerne verfügbar sind.
- Verwenden Sie im Webdienstnamen keinen Unterstrich (wie in *my_webservice*).
- Sollte beim Aufrufen des Webdiensts der Fehler **502 - Ungültiges Gateway** auftreten, wiederholen Sie den Vorgang. Der Fehler bedeutet in der Regel, dass der Container noch nicht im Cluster bereitgestellt wurde.
- Falls beim Erstellen eines Diensts der Fehler **CrashLoopBackOff** auftritt, überprüfen Sie Ihre Protokolle. Dieser Fehler ist in der Regel auf fehlende Abhängigkeiten in der Funktion **Init** zurückzuführen.