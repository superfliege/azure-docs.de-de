---
title: "Bereitstellen von Azure Log Analytics Nozzle zur Überwachung von Cloud Foundry | Microsoft-Dokumentation"
description: "Enthält eine Schritt-für-Schritt-Anleitung zur Bereitstellung von Cloud Foundry Loggregator Nozzle für Azure Log Analytics. Verwenden Sie Nozzle zum Überwachen der Systemintegrität und Leistungsmetriken von Cloud Foundry."
services: virtual-machines-linux
documentationcenter: 
author: ningk
manager: timlt
editor: 
tags: Cloud-Foundry
ms.assetid: 00c76c49-3738-494b-b70d-344d8efc0853
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/22/2017
ms.author: ningk
ms.openlocfilehash: 0d13d39d2921c51c537534a5b000564a9df91880
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-azure-log-analytics-nozzle-for-cloud-foundry-system-monitoring"></a>Bereitstellen von Azure Log Analytics Nozzle zur Überwachung des Cloud Foundry-Systems

[Azure Log Analytics](https://azure.microsoft.com/services/log-analytics/) ist ein Dienst aus der [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/) (OMS) von Microsoft. Er dient zum Erfassen und Analysieren von Daten, die von Ihren cloudbasierten und lokalen Umgebungen generiert werden.

Log Analytics Nozzle (die Nozzle-Komponente) ist eine Cloud Foundry-Komponente (CF), die Metriken aus der Firehose-Komponente von [Cloud Foundry Loggregator](https://docs.cloudfoundry.org/loggregator/architecture.html) an Log Analytics weiterleitet. Mithilfe der Nozzle-Komponente können Sie Integritäts- und Leistungsmetriken für Ihr CF-System über mehrere Bereitstellungen hinweg sammeln, anzeigen und analysieren.

In diesem Dokument erfahren Sie, wie Sie die Nozzle-Komponente in Ihrer CF-Umgebung bereitstellen und dann über die Log Analytics-OMS-Konsole auf die Daten zugreifen.

## <a name="prerequisites"></a>Voraussetzungen

Die folgenden Schritte sind Voraussetzungen für die Bereitstellung der Nozzle-Komponente:

### <a name="1-deploy-a-cf-or-pivotal-cloud-foundry-environment-in-azure"></a>1. Bereitstellen einer CF- oder Pivotal Cloud Foundry-Umgebung in Azure

Sie können die Nozzle-Komponente entweder mit einer Open-Source-CF-Bereitstellung (Cloud Foundry) oder mit einer PCF-Bereitstellung (Pivotal Cloud Foundry) verwenden.

* [Deploy Cloud Foundry on Azure](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/blob/master/docs/guidance.md) (Bereitstellen von Cloud Foundry in Azure)

* [Installing Pivotal Cloud Foundry on Azure](https://docs.pivotal.io/pivotalcf/1-11/customizing/azure.html) (Installieren von Pivotal Cloud Foundry in Azure)

### <a name="2-install-the-cf-command-line-tools-for-deploying-the-nozzle"></a>2. Installieren der CF-Befehlszeilentools zum Bereitstellen der Nozzle-Komponente

Die Nozzle-Komponente wird als Anwendung in der CF-Umgebung ausgeführt. Sie benötigen die CF-Befehlszeilenschnittstelle, um die Anwendung bereitzustellen.

Außerdem ist für die Nozzle-Komponente eine Zugriffsberechtigung für Loggregator Firehose und den Cloud Controller erforderlich. Zum Erstellen und Konfigurieren des Benutzers benötigen Sie den UAA-Dienst (User Account and Authentication, Benutzerkonto und Authentifizierung).

* [Installing the cf CLI](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html) (Installieren der CF-Befehlszeilenschnittstelle)

* [CloudFoundry UAA Command Line Client](https://github.com/cloudfoundry/cf-uaac/blob/master/README.md) (Cloud Foundry-UAA-Befehlszeilenclient)

Stellen Sie vor dem Einrichten des UAA-Befehlszeilenclients sicher, dass Rubygems installiert ist.

### <a name="3-create-an-oms-workspace-in-azure"></a>3. Erstellen eines OMS-Arbeitsbereichs in Azure

Sie können den OMS-Arbeitsbereich manuell oder mit einer Vorlage erstellen. Laden Sie die vorkonfigurierten OMS-Ansichten und Warnungen, nachdem Sie die Nozzle-Bereitstellung abgeschlossen haben.

Gehen Sie wie folgt vor, um den Arbeitsbereich manuell zu erstellen:

1. Suchen Sie im Azure-Portal in der Marketplace-Dienstliste nach „Log Analytics“, und wählen Sie den Eintrag aus.
2. Wählen Sie die Option **Erstellen** und anschließend Optionen für die folgenden Elemente aus:

   * **OMS-Arbeitsbereich**: Geben Sie einen Namen für Ihren Arbeitsbereich ein.
   * **Abonnement**: Wählen Sie das Abonnement aus, das Ihrer CF-Bereitstellung entspricht, falls Sie über mehrere Abonnements verfügen.
   * **Ressourcengruppe**: Sie können eine neue Ressourcengruppe erstellen oder die gleiche wie für Ihre CF-Bereitstellung verwenden.
   * **Standort**: Geben Sie den Standort ein.
   * **Tarif**: Wählen Sie **OK**, um den Vorgang abzuschließen.

Weitere Informationen finden Sie unter [Erste Schritte mit Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-get-started).

Alternativ hierzu können Sie den OMS-Arbeitsbereich auch mit der OMS-Vorlage erstellen. Bei dieser Methode lädt die Vorlage die vorkonfigurierten OMS-Ansichten und -Warnungen automatisch. Weitere Informationen finden Sie unter [Azure OMS Log Analytics solution for Cloud Foundry](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-cloudfoundry-solution) (Azure OMS Log Analytics-Lösung für Cloud Foundry).

## <a name="deploy-the-nozzle"></a>Bereitstellen der Nozzle-Komponente

Sie haben verschiedene Möglichkeiten, um die Nozzle-Komponente bereitzustellen: als PCF-Datei oder als CF-Anwendung.

### <a name="deploy-the-nozzle-as-a-pcf-ops-manager-tile"></a>Bereitstellen der Nozzle-Komponente als PCF Operations Manager-Kachel

Falls Sie PCF mit Ops Manager bereitgestellt haben, sollten Sie die Schritte zum [Installieren und Konfigurieren der Nozzle-Komponente für PCF](http://docs.pivotal.io/partners/azure-log-analytics-nozzle/installing.html) ausführen. Die Nozzle-Komponente wird mit Ops Manager als Kachel installiert.

### <a name="deploy-the-nozzle-as-a-cf-application"></a>Bereitstellen der Nozzle-Komponente als CF-Anwendung

Wenn Sie PCF Ops Manager nicht verwenden, stellen Sie die Nozzle-Komponente als Anwendung bereit. In den folgenden Abschnitten wird dieser Prozess beschrieben.

#### <a name="sign-in-to-your-cf-deployment-as-an-admin-through-cf-cli"></a>Anmelden an Ihrer CF-Bereitstellung über die CF-Befehlszeilenschnittstelle als Administrator

Führen Sie den folgenden Befehl aus:
```
cf login -a https://api.${SYSTEM_DOMAIN} -u ${CF_USER} --skip-ssl-validation
```

„SYSTEM_DOMAIN“ steht für Ihren CF-Domänennamen. Diesen können Sie ermitteln, indem Sie in Ihrer CF-Bereitstellungsmanifestdatei nach „SYSTEM_DOMAIN“ suchen. 

„CF_User“ ist der CF-Administratorname. Zum Ermitteln des Namens und Kennworts können Sie den Abschnitt „scim“ in Ihrer CF-Bereitstellungsmanifestdatei nach dem Namen und nach „cf_admin_password“ durchsuchen.

#### <a name="create-a-cf-user-and-grant-required-privileges"></a>Erstellen Sie einen CF-Benutzer, und gewähren Sie die erforderlichen Berechtigungen.

Führen Sie die folgenden Befehle aus:
```
uaac target https://uaa.${SYSTEM_DOMAIN} --skip-ssl-validation
uaac token client get admin
cf create-user ${FIREHOSE_USER} ${FIREHOSE_USER_PASSWORD}
uaac member add cloud_controller.admin ${FIREHOSE_USER}
uaac member add doppler.firehose ${FIREHOSE_USER}
```

„SYSTEM_DOMAIN“ steht für Ihren CF-Domänennamen. Diesen können Sie ermitteln, indem Sie in Ihrer CF-Bereitstellungsmanifestdatei nach „SYSTEM_DOMAIN“ suchen.

#### <a name="download-the-latest-log-analytics-nozzle-release"></a>Herunterladen der neuesten Version von Log Analytics Nozzle

Führen Sie den folgenden Befehl aus:
```
git clone https://github.com/Azure/oms-log-analytics-firehose-nozzle.git
cd oms-log-analytics-firehose-nozzle
```

#### <a name="set-environment-variables"></a>Festlegen von Umgebungsvariablen

Nun können Sie die Umgebungsvariablen in der Datei „manifest.yml“ in Ihrem aktuellen Verzeichnis festlegen. Unten ist das App-Manifest für die Nozzle-Komponente angegeben. Ersetzen Sie die entsprechenden Werte durch Ihre Informationen für den OMS-Arbeitsbereich.

```
OMS_WORKSPACE             : OMS workspace ID: open OMS portal from your OMS workspace, select Settings, and select connected sources.
OMS_KEY                   : OMS key: open OMS portal from your OMS workspace, select Settings, and select connected sources.
OMS_POST_TIMEOUT          : HTTP post timeout for sending events to OMS Log Analytics. The default is 10 seconds.
OMS_BATCH_TIME            : Interval for posting a batch to OMS Log Analytics. The default is 10 seconds.
OMS_MAX_MSG_NUM_PER_BATCH : The maximum number of messages in a batch to OMS Log Analytics. The default is 1000.
API_ADDR                  : The API URL of the CF environment. For more information, see the preceding section, "Sign in to your CF deployment as an admin through CF CLI."
DOPPLER_ADDR              : Loggregator's traffic controller URL. For more information, see the preceding section, "Sign in to your CF deployment as an admin through CF CLI."
FIREHOSE_USER             : CF user you created in the preceding section, "Create a CF user and grant required privileges." This user has firehose and Cloud Controller admin access.
FIREHOSE_USER_PASSWORD    : Password of the CF user above.
EVENT_FILTER              : Event types to be filtered out. The format is a comma-separated list. Valid event types are METRIC, LOG, and HTTP.
SKIP_SSL_VALIDATION       : If true, allows insecure connections to the UAA and the traffic controller.
CF_ENVIRONMENT            : Enter any string value for identifying logs and metrics from different CF environments.
IDLE_TIMEOUT              : The Keep Alive duration for the firehose consumer. The default is 60 seconds.
LOG_LEVEL                 : The logging level of the Nozzle. Valid levels are DEBUG, INFO, and ERROR.
LOG_EVENT_COUNT           : If true, the total count of events that the Nozzle has received and sent are logged to OMS Log Analytics as CounterEvents.
LOG_EVENT_COUNT_INTERVAL  : The time interval of the logging event count to OMS Log Analytics. The default is 60 seconds.
```

### <a name="push-the-application-from-your-development-computer"></a>Übertragen der Anwendung von Ihrem Entwicklungscomputer per Pushvorgang

Vergewissern Sie sich, dass Sie sich im Ordner „oms-log-analytics-firehose-nozzle“ befinden. Führen Sie den folgenden Befehl aus:
```
cf push
```

## <a name="validate-the-nozzle-installation"></a>Überprüfen der Nozzle-Installation

### <a name="from-apps-manager-for-pcf"></a>Über Apps Manager (für PCF)

1. Melden Sie sich bei Operations Manager an, und vergewissern Sie sich, dass die Kachel auf dem Installationsdashboard angezeigt wird.
2. Melden Sie sich bei Apps Manager an, und vergewissern Sie sich, dass der für die Nozzle-Komponente erstellte Bereich im Nutzungsbericht aufgeführt wird und der Status normal ist.

### <a name="from-your-development-computer"></a>Über Ihren Entwicklungscomputer

Geben Sie im Fenster der CF-Befehlszeilenschnittstelle Folgendes ein:
```
cf apps
```
Vergewissern Sie sich, dass die OMS-Nozzle-Anwendung ausgeführt wird.

## <a name="view-the-data-in-the-oms-portal"></a>Anzeigen der Daten im OMS-Portal

### <a name="1-import-the-oms-view"></a>1. Importieren der OMS-Ansicht

Navigieren Sie über das OMS-Portal zu **Ansicht-Designer** > **Importieren** > **Durchsuchen**, und wählen Sie einige OMSVIEW-Dateien aus. Wählen Sie beispielsweise *Cloud Foundry.omsview* aus, und speichern Sie die Ansicht. Daraufhin wird auf der OMS-Seite mit der **Übersicht** eine Kachel angezeigt. Wählen Sie diese Kachel aus, um visualisierte Metriken anzuzeigen.

Sie können diese Ansichten über den **Ansicht-Designer** anpassen oder neue Ansichten erstellen.

*„Cloud Foundry.omsview“* ist eine Vorschauversion der Cloud Foundry-OMS-Ansichtsvorlage. Dies ist eine vollständig konfigurierte Standardvorlage. Wenn Sie Vorschläge oder Feedback für die Vorlage haben, können Sie diese im [Abschnitt zu den Problemen](https://github.com/Azure/oms-log-analytics-firehose-nozzle/issues) hinterlassen.

### <a name="2-create-alert-rules"></a>2. Erstellen von Warnungsregeln

Sie können [die Warnungen erstellen](https://docs.microsoft.com/azure/log-analytics/log-analytics-alerts) und die Abfragen und Schwellenwerte nach Bedarf anpassen. Hier sind empfohlene Warnungen angegeben:

| Suchabfrage                                                                  | Warnung generieren basierend auf | Beschreibung                                                                       |
| ----------------------------------------------------------------------------- | ----------------------- | --------------------------------------------------------------------------------- |
| Type=CF_ValueMetric_CL Origin_s=bbs Name_s="Domain.cf-apps"                   | Anzahl von Ergebnissen < 1   | **bbs.Domain.cf-apps** gibt an, ob die Domäne „cf-apps“ aktuell ist. Dies bedeutet, dass CF-App-Anforderungen von Cloud Controller für die Ausführung mit „bbs.LRPsDesired“ (von Diego gewünschte KIs) synchronisiert werden. Wenn keine Daten empfangen werden, bedeutet dies, dass die Domäne „cf-apps“ im angegebenen Zeitraum nicht auf dem neuesten Stand ist. |
| Type=CF_ValueMetric_CL Origin_s=rep Name_s=UnhealthyCell Value_d>1            | Anzahl von Ergebnissen > 0   | Für Diego-Zellen bedeutet der Wert 0 fehlerfrei und der Wert 1 fehlerhaft. Legen Sie die Warnung fest, wenn im angegebenen Zeitfenster mehrere fehlerhafte Diego-Zellen erkannt werden. |
| Type=CF_ValueMetric_CL Origin_s="bosh-hm-forwarder" Name_s="system.healthy" Value_d=0 | Anzahl von Ergebnissen > 0 | Bei 1 ist das System fehlerfrei, bei 0 nicht. |
| Type=CF_ValueMetric_CL Origin_s=route_emitter Name_s=ConsulDownMode Value_d>0 | Anzahl von Ergebnissen > 0   | Consul gibt in regelmäßigen Abständen seinen Integritätsstatus aus. Bei 0 ist das System fehlerfrei. Bei 1 hat der Routen-Emitter den Ausfall von Consul festgestellt. |
| Type=CF_CounterEvent_CL Origin_s=DopplerServer (Name_s="TruncatingBuffer.DroppedMessages" or Name_s="doppler.shedEnvelopes") Delta_d>0 | Anzahl von Ergebnissen > 0 | Die Deltaanzahl von Nachrichten, die von Doppler aufgrund eines Rückstaus vorsätzlich gelöscht wurden. |
| Type=CF_LogMessage_CL SourceType_s=LGR MessageType_s=ERR                      | Anzahl von Ergebnissen > 0   | Loggregator gibt **LGR** aus, um auf Probleme mit dem Protokollierungsprozess hinzuweisen. Ein Beispiel für diese Art von Problem ist eine zu hohe Ausgabe von Protokollmeldungen. |
| Type=CF_ValueMetric_CL Name_s=slowConsumerAlert                               | Anzahl von Ergebnissen > 0   | Wenn die Nozzle-Komponente von Loggregator eine Warnung wegen eines langsamen Consumers empfängt, sendet sie das ValueMetric-Element **SlowConsumerAlert** an OMS. |
| Type=CF_CounterEvent_CL Job_s=nozzle Name_s=eventsLost Delta_d>0              | Anzahl von Ergebnissen > 0   | Wenn die Deltaanzahl verloren gegangener Ereignisse einen Schwellenwert erreicht, liegt unter Umständen ein Problem mit der Nozzle-Ausführung vor. |

## <a name="scale"></a>Skalieren

Sie können die Nozzle-Komponente und den Loggregator skalieren.

### <a name="scale-the-nozzle"></a>Skalieren der Nozzle-Komponente

Sie sollten mindestens mit zwei Instanzen der Nozzle-Komponente beginnen. Die Firehose-Komponente verteilt die Workload auf alle Instanzen der Nozzle-Komponente.
Richten Sie die Warnung **slowConsumerAlert** (im vorherigen Abschnitt „Erstellen von Warnungsregeln“ aufgeführt) ein, um sicherzustellen, dass die Nozzle-Komponente den Datenverkehr von der Firehose-Komponente bewältigen kann. Befolgen Sie nach Erhalt der Warnung den [Leitfaden für eine langsame Nozzle-Komponente](https://docs.pivotal.io/pivotalcf/1-11/loggregator/log-ops-guide.html#slow-noz), um zu ermitteln, ob eine Skalierung erforderlich ist.
Verwenden Sie zum Skalieren der Nozzle-Komponente entweder Apps Manager oder die CF-Befehlszeilenschnittstelle, um die Instanzanzahl oder die Arbeitsspeicher- bzw. Datenträgerressourcen für die Nozzle-Komponente zu erhöhen.

### <a name="scale-the-loggregator"></a>Skalieren von Loggregator

Loggregator sendet eine Protokollmeldung **LGR**, um auf Probleme mit dem Protokollierungsprozess hinzuweisen. Sie können die Warnung überwachen, um zu ermitteln, ob Loggregator zentral hochskaliert werden muss.
Erhöhen Sie zum zentralen Hochskalieren von Loggregator entweder die Doppler-Puffergröße, oder fügen Sie im CF-Manifest zusätzliche Doppler-Serverinstanzen hinzu. Weitere Informationen finden Sie im [Leitfaden zum Skalieren von Loggregator](https://docs.cloudfoundry.org/running/managing-cf/logging-config.html#scaling).

## <a name="update"></a>Aktualisieren

Wenn Sie die Nozzle-Komponente mit einer neueren Version aktualisieren möchten, laden Sie die neue Nozzle-Version herunter, führen die Schritte im vorherigen Abschnitt „Bereitstellen der Nozzle-Komponente“ aus und übermitteln die Anwendung per Pushvorgang erneut.

### <a name="remove-the-nozzle-from-ops-manager"></a>Entfernen der Nozzle-Komponente aus Ops Manager

1. Melden Sie sich bei Ops Manager an.
2. Suchen Sie nach der Kachel **Microsoft Azure Log Analytics Nozzle für PCF**.
3. Wählen Sie das Papierkorbsymbol, und bestätigen Sie den Löschvorgang.

### <a name="remove-the-nozzle-from-your-development-computer"></a>Entfernen der Nozzle-Komponente vom Entwicklungscomputer

Geben Sie im Fenster der CF-Befehlszeilenschnittstelle Folgendes ein:
```
cf delete <App Name> -r
```

Wenn Sie die Nozzle-Komponente entfernen, werden die Daten im OMS-Portal nicht automatisch entfernt. Sie laufen basierend auf Ihrer Aufbewahrungseinstellung für OMS Log Analytics ab.

## <a name="support-and-feedback"></a>Support und Feedback

Azure Log Analytics Nozzle ist eine Open-Source-Komponente. Senden Sie Ihre Fragen und Ihr Feedback über den [GitHub-Abschnitt](https://github.com/Azure/oms-log-analytics-firehose-nozzle/issues). Wählen Sie „Virtueller Computer mit Cloud Foundry“ als Dienstkategorie, wenn Sie eine Azure-Supportanfrage erstellen möchten. 

## <a name="next-step"></a>Nächster Schritt

Zusätzlich zu den Cloud Foundry-Metriken, die mit der Nozzle-Komponente abgedeckt werden, können Sie den OMS-Agent nutzen, um Einblicke in die Betriebsdaten auf VM-Ebene (z.B. Syslog, Leistung, Warnungen, Bestand) zu erhalten. Der OMS-Agent wird als Bosh-Add-On für Ihre CF-VMs installiert.

Ausführliche Informationen finden Sie unter [Deploy OMS agent to your Cloud Foundry deployment](https://github.com/Azure/oms-agent-for-linux-boshrelease) (Bereitstellen des OMS-Agents für Ihre Cloud Foundry-Bereitstellung).
