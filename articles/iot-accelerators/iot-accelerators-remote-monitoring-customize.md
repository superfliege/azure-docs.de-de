---
title: Anpassen der Benutzeroberfläche der Remoteüberwachungslösung – Azure | Microsoft-Dokumentation
description: Dieser Artikel enthält Informationen dazu, wie Sie auf den Quellcode für die Benutzeroberfläche des Solution Accelerators für Remoteüberwachung zugreifen können.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/17/2018
ms.topic: conceptual
ms.openlocfilehash: 36e63d26bf7ada2d23fa3cd9fddbb5ba90494527
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2018
ms.locfileid: "43126021"
---
# <a name="customize-the-remote-monitoring-solution-accelerator"></a>Anpassen des Solution Accelerators für Remoteüberwachung

Dieser Artikel enthält Informationen dazu, wie Sie auf den Quellcode zugreifen und die Benutzeroberfläche des Solution Accelerators für Remoteüberwachung anpassen können. Der Artikel beschreibt Folgendes:

## <a name="prepare-a-local-development-environment-for-the-ui"></a>Vorbereiten einer lokalen Entwicklungsumgebung für die Benutzeroberfläche

Der Code der Benutzeroberfläche des Solution Accelerators für Remoteüberwachung wird mithilfe des Frameworks React.js implementiert. Sie finden den Quellcode im [azure-iot-pcs-remote-monitoring-webui](https://github.com/Azure/azure-iot-pcs-remote-monitoring-webui)-GitHub-Repository.

Um Änderungen an der Benutzeroberfläche vorzunehmen, können Sie eine Kopie davon lokal ausführen. Die lokale Kopie stellt eine Verbindung mit einer bereitgestellten Instanz der Lösung her, um Aktionen wie das Abrufen von Telemetriedaten durchzuführen.

Die folgenden Schritte beschreiben den Prozess zum Einrichten einer lokalen Umgebung für die Entwicklung von Benutzeroberflächen:

1. Bereitstellen einer **grundlegenden** Instanz des Solution Accelerators mit der **pcs**-CLI. Notieren Sie den Namen der Bereitstellung und die Anmeldeinformationen, die Sie für die VM angegeben haben. Weitere Informationen finden Sie unter [Bereitstellen der vorkonfigurierten Remoteüberwachungslösung über die Befehlszeilenschnittstelle](iot-accelerators-remote-monitoring-deploy-cli.md).

1. Aktivieren Sie über das Azure-Portal oder mit [az CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) den SSH-Zugriff auf den virtuellen Computer, der die Microservices in der Lösung hostet. Beispiel: 

    ```sh
    az network nsg rule update --name SSH --nsg-name {your solution name}-nsg --resource-group {your solution name} --access Allow
    ```

    Sie sollten nur während der Test- und Entwicklungsphase SSH-Zugriff aktivieren. Wenn Sie SSH aktivieren, [sollten Sie es so bald wie möglich wieder deaktivieren](../security/azure-security-network-security-best-practices.md#disable-rdpssh-access-to-azure-virtual-machines).

1. Suchen Sie über das Azure-Portal oder mit [az CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) den Namen und die öffentliche IP-Adresse des virtuellen Computers. Beispiel: 

    ```sh
    az resource list --resource-group {your solution name} -o table
    az vm list-ip-addresses --name {your vm name from previous command} --resource-group {your solution name} -o table
    ```

1. Stellen Sie mit SSH die Verbindung mit dem virtuellen Computer mithilfe der IP-Adresse aus dem vorherigen Schritt und den Anmeldeinformationen her, die Sie bei der Ausführung von **pcs** zum Bereitstellen der Lösung angegeben haben.

1. Damit der lokale Computer eine Verbindung herstellen kann, führen Sie die folgenden Befehle in der Bash-Shell des virtuellen Computers aus:

    ```sh
    cd /app
    sudo ./start.sh --unsafe
    ```

1. Sobald der Befehl abgeschlossen ist und die Website gestartet wird, können Sie den virtuellen Computer trennen.

1. Bearbeiten Sie in Ihrer lokalen Kopie des [azure-iot-pcs-remote-monitoring-webui](https://github.com/Azure/azure-iot-pcs-remote-monitoring-webui)-Repositorys die **ENV**-Datei, um die URL für die bereitgestellte Lösung hinzuzufügen:

    ```config
    NODE_PATH = src/
    REACT_APP_BASE_SERVICE_URL=https://{your solution name}.azurewebsites.net/
    ```

1. Führen Sie an der Eingabeaufforderung in der lokalen Kopie des `azure-iot-pcs-remote-monitoring-webui`-Ordners die folgenden Befehle zum Installieren der erforderlichen Bibliotheken und lokalen Ausführen der Benutzeroberfläche aus:

    ```cmd/sh
    npm install
    npm start
    ```

1. Der vorherige Befehl führt die Benutzeroberfläche lokal unter http://localhost:3000/dashboard aus. Sie können den Code bearbeiten, während die Website ausgeführt wird, und ihre dynamische Aktualisierung beobachten.

## <a name="customize-the-layout"></a>Anpassen des Layouts

Jede Seite in der Remoteüberwachungslösung besteht aus einem Satz von Steuerelementen, den sogenannten *Bereichen* im Quellcode. Die **Dashboard**-Seite besteht z.B. aus fünf Bereichen: Übersicht, Zuordnung, Alarme, Telemetrie und KPIs. Sie finden den Quellcode, der jede Seite und ihre Bereiche definiert, im [pcs-remote-monitoring-webui](https://github.com/Azure/pcs-remote-monitoring-webui)-GitHub-Repository. Beispielsweise befindet sich der Code, der die **Dashboard**-Seite, ihr Layout und die Bereiche auf der Seite definiert, im [src/components/pages/dashboard](https://github.com/Azure/pcs-remote-monitoring-webui/tree/master/src/components/pages/dashboard)-Ordner.

Da die Bereiche ihr Layout und ihre Größe selbst verwalten, können Sie einfach das Layout einer Seite ändern. Mit den folgenden Änderungen des **PageContent**-Elements in der `src/components/pages/dashboard/dashboard.js`-Datei tauschen Sie z.B. die Positionen des Zuordnungs- und Telemetriebereichs und ändern die relative Breite des Zuordnungs- und KPI-Bereichs:

```nodejs
<PageContent className="dashboard-container" key="page-content">
  <Grid>
    <Cell className="col-1 devices-overview-cell">
      <OverviewPanel
        openWarningCount={openWarningCount}
        openCriticalCount={openCriticalCount}
        onlineDeviceCount={onlineDeviceCount}
        offlineDeviceCount={offlineDeviceCount}
        isPending={kpisIsPending || devicesIsPending}
        error={devicesError || kpisError}
        t={t} />
    </Cell>
    <Cell className="col-5">
      <TelemetryPanel
        telemetry={telemetry}
        isPending={telemetryIsPending}
        error={telemetryError}
        colors={chartColorObjects}
        t={t} />
    </Cell>
    <Cell className="col-3">
      <CustAlarmsPanel
        alarms={currentActiveAlarmsWithName}
        isPending={kpisIsPending || rulesIsPending}
        error={rulesError || kpisError}
        t={t} />
    </Cell>
    <Cell className="col-4">
    <PanelErrorBoundary msg={t('dashboard.panels.map.runtimeError')}>
        <MapPanel
          azureMapsKey={azureMapsKey}
          devices={devices}
          devicesInAlarm={devicesInAlarm}
          mapKeyIsPending={azureMapsKeyIsPending}
          isPending={devicesIsPending || kpisIsPending}
          error={azureMapsKeyError || devicesError || kpisError}
          t={t} />
      </PanelErrorBoundary>
    </Cell>
    <Cell className="col-6">
      <KpisPanel
        topAlarms={topAlarmsWithName}
        alarmsPerDeviceId={alarmsPerDeviceType}
        criticalAlarmsChange={criticalAlarmsChange}
        warningAlarmsChange={warningAlarmsChange}
        isPending={kpisIsPending || rulesIsPending || devicesIsPending}
        error={devicesError || rulesError || kpisError}
        colors={chartColorObjects}
        t={t} />
    </Cell>
  </Grid>
</PageContent>
```

![Bereichslayout ändern](./media/iot-accelerators-remote-monitoring-customize/layout.png)

> [!NOTE]
> Die Zuordnung ist in der lokalen Bereitstellung nicht konfiguriert.

Sie können auch mehrere Instanzen des gleichen Bereichs oder mehrere Versionen hinzufügen, wenn Sie [einen Bereich duplizieren und anpassen](#duplicate-and-customize-an-existing-control). Im folgenden Beispiel wird das Hinzufügen von zwei Instanzen des Telemetriebereichs durch Bearbeiten der `src/components/pages/dashboard/dashboard.js`-Datei gezeigt:

```nodejs
<PageContent className="dashboard-container" key="page-content">
  <Grid>
    <Cell className="col-1 devices-overview-cell">
      <OverviewPanel
        openWarningCount={openWarningCount}
        openCriticalCount={openCriticalCount}
        onlineDeviceCount={onlineDeviceCount}
        offlineDeviceCount={offlineDeviceCount}
        isPending={kpisIsPending || devicesIsPending}
        error={devicesError || kpisError}
        t={t} />
    </Cell>
    <Cell className="col-3">
      <TelemetryPanel
        telemetry={telemetry}
        isPending={telemetryIsPending}
        error={telemetryError}
        colors={chartColorObjects}
        t={t} />
    </Cell>
    <Cell className="col-3">
      <TelemetryPanel
        telemetry={telemetry}
        isPending={telemetryIsPending}
        error={telemetryError}
        colors={chartColorObjects}
        t={t} />
    </Cell>
    <Cell className="col-2">
      <CustAlarmsPanel
        alarms={currentActiveAlarmsWithName}
        isPending={kpisIsPending || rulesIsPending}
        error={rulesError || kpisError}
        t={t} />
    </Cell>
    <Cell className="col-4">
    <PanelErrorBoundary msg={t('dashboard.panels.map.runtimeError')}>
        <MapPanel
          azureMapsKey={azureMapsKey}
          devices={devices}
          devicesInAlarm={devicesInAlarm}
          mapKeyIsPending={azureMapsKeyIsPending}
          isPending={devicesIsPending || kpisIsPending}
          error={azureMapsKeyError || devicesError || kpisError}
          t={t} />
      </PanelErrorBoundary>
    </Cell>
    <Cell className="col-6">
      <KpisPanel
        topAlarms={topAlarmsWithName}
        alarmsPerDeviceId={alarmsPerDeviceType}
        criticalAlarmsChange={criticalAlarmsChange}
        warningAlarmsChange={warningAlarmsChange}
        isPending={kpisIsPending || rulesIsPending || devicesIsPending}
        error={devicesError || rulesError || kpisError}
        colors={chartColorObjects}
        t={t} />
    </Cell>
  </Grid>
</PageContent>
```

Sie können dann in den einzelnen Bereichen verschiedene Telemetriedaten anzeigen:

![Mehrere Telemetriedatenbereiche](./media/iot-accelerators-remote-monitoring-customize/multiple-telemetry.png)

> [!NOTE]
> Die Zuordnung ist in der lokalen Bereitstellung nicht konfiguriert.

## <a name="duplicate-and-customize-an-existing-control"></a>Duplizieren und Anpassen eines vorhandenen Steuerelements

Die folgenden Schritte beschreiben die Verwendung des **Alarmbereichs** als Beispiel für das Duplizieren eines vorhandenen Bereichs, seine Änderung und die Verwendung der geänderten Version:

1. Erstellen Sie in der lokalen Kopie des Repositorys eine Kopie des **alarms**-Ordners im `src/components/pages/dashboard/panels`-Ordner. Benennen Sie die neue Kopie **cust_alarms**.

1. Nennen Sie in der Datei **alarmsPanel.js** im **cust_alarms**-Ordner die Klasse in **CustAlarmsPanel** um:

    ```nodejs
    export class CustAlarmsPanel extends Component {
    ```

1. Fügen Sie der Datei `src/components/pages/dashboard/panels/index.js` die folgende Zeile hinzu:

    ```nodejs
    export * from './cust_alarms';
    ```

1. Ersetzen Sie in der `src/components/pages/dashboard/dashboard.js`-Datei `AlarmsPanel` durch `CustAlarmsPanel`:

    ```nodejs
    import {
      OverviewPanel,
      CustAlarmsPanel,
      TelemetryPanel,
      KpisPanel,
      MapPanel,
      transformTelemetryResponse,
      chartColors
    } from './panels';

    ...

    <Cell className="col-3">
      <CustAlarmsPanel
        alarms={currentActiveAlarmsWithName}
        isPending={kpisIsPending || rulesIsPending}
        error={rulesError || kpisError}
        t={t} />
    </Cell>
    ```

Sie haben jetzt den ursprünglichen Bereich **Alarme** durch eine Kopie namens **CustAlarms** ersetzt. Diese Kopie ist mit dem Original identisch. Sie können jetzt die Kopie ändern. Ändern Sie z.B. die Spaltenreihenfolge im **Alarme**-Bereich:

1. Öffnen Sie die Datei `src/components/pages/dashboard/panels/cust_alarms/alarmsPanel.js` .

1. Ändern Sie die Spaltendefinitionen, wie im folgenden Codeausschnitt gezeigt:

    ```nodejs
    this.columnDefs = [
      rulesColumnDefs.severity,
      {
        headerName: 'rules.grid.count',
        field: 'count'
      },
      {
        ...rulesColumnDefs.ruleName,
        minWidth: 200
      },
      rulesColumnDefs.explore
    ];
    ```

Der folgende Screenshot zeigt die neue Version des **Alarme**-Bereichs:

![Aktualisierter Bereich „Alarme“](./media/iot-accelerators-remote-monitoring-customize/reorder-columns.png)

## <a name="customize-the-telemetry-chart"></a>Anpassen des Telemetriediagramms

Das Telemetriediagramm auf der **Dashboard**-Seite wird durch die Dateien im `src/components/pages/dashboard/panels/telemtry`-Ordner definiert. Die Benutzeroberfläche ruft die Telemetriedaten aus dem Lösungs-Back-End in der `src/services/telemetryService.js`-Datei ab. Die folgenden Schritte zeigen Ihnen, wie Sie den im Telemetriediagramm angezeigten Zeitraum von 15 Minuten auf 5 Minuten ändern:

1. In der `src/services/telemetryService.js`-Datei finden Sie die aufgerufene Funktion **getTelemetryByDeviceIdP15M**. Erstellen Sie eine Kopie dieser Funktion, und ändern Sie die Kopie wie folgt:

    ```nodejs
    static getTelemetryByDeviceIdP5M(devices = []) {
      return TelemetryService.getTelemetryByMessages({
        from: 'NOW-PT5M',
        to: 'NOW',
        order: 'desc',
        devices
      });
    }
    ```

1. Um mit dieser neuen Funktion das Telemetriediagramm aufzufüllen, öffnen Sie die `src/components/pages/dashboard/dashboard.js`-Datei. Ändern Sie dort die Zeile, die den Telemetriedatenstrom initialisiert, wie folgt:

    ```node.js
    const getTelemetryStream = ({ deviceIds = [] }) => TelemetryService.getTelemetryByDeviceIdP5M(deviceIds)
    ```

Das Telemetriediagramm zeigt jetzt die Telemetriedaten von fünf Minuten an:

![Telemetriediagramm für einen Tag](./media/iot-accelerators-remote-monitoring-customize/telemetry-period.png)

## <a name="add-a-new-kpi"></a>Hinzufügen eines neuen KPI

Auf der Seite **Dashboard** werden KPIs im Bereich **System-KPIs** angezeigt. Diese KPIs werden in der `src/components/pages/dashboard/dashboard.js`-Datei berechnet. Die KPIs werden durch die `src/components/pages/dashboard/panels/kpis/kpisPanel.js`-Datei gerendert. Die folgenden Schritte beschreiben das Berechnen und Rendern eines neuen KPI-Werts auf der **Dashboard**-Seite. Im Beispiel wird eine neue prozentuale Änderung im Alarme-KPI hinzugefügt:

1. Öffnen Sie die Datei `src/components/pages/dashboard/dashboard.js` . Ändern des **initialState**-Objekts, sodass eine **warningAlarmsChange**-Eigenschaft wie folgt einbezogen wird:

    ```nodejs
    const initialState = {
      ...

      // Kpis data
      currentActiveAlarms: [],
      topAlarms: [],
      alarmsPerDeviceId: {},
      criticalAlarmsChange: 0,
      warningAlarmsChange: 0,
      kpisIsPending: true,
      kpisError: null,

      ...
    };
    ```

1. Ändern des **currentAlarmsStats**-Objekts, sodass **totalWarningCount** als Eigenschaft einbezogen wird:

    ```nodejs
    return {
      openWarningCount: (acc.openWarningCount || 0) + (isWarning && isOpen ? 1 : 0),
      openCriticalCount: (acc.openCriticalCount || 0) + (isCritical && isOpen ? 1 : 0),
      totalWarningCount: (acc.totalWarningCount || 0) + (isWarning ? 1 : 0),
      totalCriticalCount: (acc.totalCriticalCount || 0) + (isCritical ? 1 : 0),
      alarmsPerDeviceId: updatedAlarmsPerDeviceId
    };
    ```

1. Berechnen Sie den neuen KPI. Finden Sie die Berechnung für die kritische Anzahl von Alarmen. Kopieren Sie den Code, und ändern Sie die Kopie wie folgt:

    ```nodejs
    // ================== Warning Alarms Count - START
    const currentWarningAlarms = currentAlarmsStats.totalWarningCount;
    const previousWarningAlarms = previousAlarms.reduce(
      (cnt, { severity }) => severity === 'warning' ? cnt + 1 : cnt,
      0
    );
    const warningAlarmsChange = ((currentWarningAlarms - previousWarningAlarms) / currentWarningAlarms * 100).toFixed(2);
    // ================== Warning Alarms Count - END
    ```

1. Beziehen Sie den neuen KPI **warningAlarmsChange** in den KPI-Datenstrom ein:

    ```nodejs
    return ({
      kpisIsPending: false,

      // Kpis data
      currentActiveAlarms,
      topAlarms,
      criticalAlarmsChange,
      warningAlarmsChange,
      alarmsPerDeviceId: currentAlarmsStats.alarmsPerDeviceId,

      ...
    });
    ```

1. Beziehen Sie den neuen KPI **warningAlarmsChange** in die Zustandsdaten ein, die zum Rendern der Benutzeroberfläche verwendet werden:

    ```nodejs
    const {
      ...

      currentActiveAlarms,
      topAlarms,
      alarmsPerDeviceId,
      criticalAlarmsChange,
      warningAlarmsChange,
      kpisIsPending,
      kpisError,

      ...
    } = this.state;
    ```

1. Aktualisieren Sie die Daten, die dem Bereich „KPIs“ übergeben werden:

    ```node.js
    <KpisPanel
      topAlarms={topAlarmsWithName}
      alarmsPerDeviceId={alarmsPerDeviceType}
      criticalAlarmsChange={criticalAlarmsChange}
      warningAlarmsChange={warningAlarmsChange}
      isPending={kpisIsPending || rulesIsPending || devicesIsPending}
      error={devicesError || rulesError || kpisError}
      colors={chartColorObjects}
      t={t} />
    ```

Sie haben jetzt die Änderungen in der `src/components/pages/dashboard/dashboard.js`-Datei fertig gestellt. Die folgenden Schritte beschreiben, welche Änderungen in der `src/components/pages/dashboard/panels/kpis/kpisPanel.js`-Datei zum Anzeigen der neuen KPI erforderlich sind:

1. Ändern Sie die folgende Codezeile wie folgt zum Abrufen des neuen KPI-Werts:

    ```nodejs
    const { t, isPending, criticalAlarmsChange, warningAlarmsChange, error } = this.props;
    ```

1. Ändern Sie das Markup zum Anzeigen des neuen KPI-Werts wie folgt:

    ```nodejs
    <div className="kpi-cell">
      <div className="kpi-header">{t('dashboard.panels.kpis.criticalAlarms')}</div>
      <div className="critical-alarms">
        {
          criticalAlarmsChange !== 0 &&
            <div className="kpi-percentage-container">
              <div className="kpi-value">{ criticalAlarmsChange }</div>
              <div className="kpi-percentage-sign">%</div>
            </div>
        }
      </div>
      <div className="kpi-header">{t('Warning alarms')}</div>
      <div className="critical-alarms">
        {
          warningAlarmsChange !== 0 &&
            <div className="kpi-percentage-container">
              <div className="kpi-value">{ warningAlarmsChange }</div>
              <div className="kpi-percentage-sign">%</div>
            </div>
        }
      </div>
    </div>
    ```

Auf der **Dashboard**-Seite wird jetzt der neue KPI-Wert angezeigt:

![Warnungs-KPI](./media/iot-accelerators-remote-monitoring-customize/new-kpi.png)

## <a name="customize-the-map"></a>Anpassen der Karte

Ausführliche Informationen zu den Kartenkomponenten in der Lösung finden Sie auf der Seite [Customize map](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#upgrade-map-key-to-see-devices-on-a-dynamic-map) (Anpassungsübersicht) in GitHub.

<!--
### Connect an external visualization tool

See the [Connect an external visualization tool](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) page in GitHub for details of how to connect an external visualization tool.

-->

## <a name="other-customization-options"></a>Andere Optionen zur Anpassung

Um die Darstellungs- und Visualisierungsschicht in der Remoteüberwachungslösung weiter zu ändern, können Sie den Code bearbeiten. Relevante GitHub-Repositorys:

* [The configuration microservice for Azure IoT Solutions (.NET) (Der Konfigurations-Microservice für Azure IoT-Lösungen [.NET])](https://github.com/Azure/pcs-ui-config-dotnet/)
* [The configuration microservice for Azure IoT Solutions (Java) (Der Konfigurations-Microservice für Azure IoT-Lösungen [Java])](https://github.com/Azure/pcs-ui-config-java/)
* [Azure IoT PCS Remote Monitoring Web UI (Azure IoT PCS-Remoteüberwachungs-Webbenutzeroberfläche)](https://github.com/Azure/pcs-remote-monitoring-webui)

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie Informationen zu den verfügbaren Ressourcen erhalten, mit denen Sie die Webbenutzeroberfläche im Solution Accelerator für Remoteüberwachung anpassen können.

Weitere konzeptuelle Informationen zum Solution Accelerator für Remoteüberwachung finden Sie unter [Remoteüberwachungsarchitektur](iot-accelerators-remote-monitoring-sample-walkthrough.md).

Weitere Informationen zum Anpassen der Remoteüberwachungslösung finden Sie unter [Anpassen und erneutes Bereitstellen eines Microservice](iot-accelerators-microservices-example.md)
<!-- Next tutorials in the sequence -->.
