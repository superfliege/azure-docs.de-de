---
title: Bereitstellen einer cloudbasierten IoT-Remoteüberwachungslösung in Azure | Microsoft-Dokumentation
description: In dieser Schnellstartanleitung stellen Sie den Azure IoT Solution Accelerator für die Remoteüberwachung bereit und melden sich am Lösungsdashboard an, um es zu nutzen.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: quickstart
ms.custom: mvc
ms.date: 06/07/2018
ms.author: dobett
ms.openlocfilehash: e3eff46299ecfbfe39b57bc2cf5ed4a655a6d7f1
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2018
ms.locfileid: "37087947"
---
# <a name="quickstart-deploy-a-cloud-based-remote-monitoring-solution"></a>Schnellstart: Bereitstellen einer cloudbasierten Lösung für die Remoteüberwachung

In dieser Schnellstartanleitung wird veranschaulicht, wie Sie den Azure IoT Solution Accelerator für die Remoteüberwachung bereitstellen, um für Ihre IoT-Geräte eine cloudbasierte Lösung für die Remoteüberwachung zu erhalten. Nach der Bereitstellung des Solution Accelerators verwenden Sie die Seite mit dem **Lösungsdashboard**, um simulierte Geräte auf einer Karte zu visualisieren. Auf der Seite **Wartung** reagieren Sie auf eine Druckwarnung eines simulierten Kühlgeräts.

In der Standardbereitstellung wird der Solution Accelerator für die Remoteüberwachung für das Unternehmen mit dem Namen Contoso konfiguriert. Contoso verwaltet eine Auswahl von unterschiedlichen Gerätetypen, z.B. Kältemaschinen, die in verschiedenen physischen Umgebungen bereitgestellt wurden. Eine Kältemaschine sendet Telemetriedaten zu Temperatur, Luftfeuchtigkeit und Druck an den Solution Accelerator für die Remoteüberwachung.

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung benötigen Sie ein aktives Azure-Abonnement.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="deploy-the-solution"></a>Bereitstellen der Lösung

Beim Bereitstellen des Solution Accelerators in Ihrem Azure-Abonnement müssen Sie einige Konfigurationsoptionen festlegen.

Melden Sie sich mit Ihren Azure-Kontoanmeldeinformationen an [azureiotsolutions.com](https://www.azureiotsolutions.com/Accelerators) an.

Klicken Sie auf der Kachel **Remoteüberwachung** auf **Jetzt testen**.

![Klicken auf „Remoteüberwachung“](./media/quickstart-remote-monitoring-deploy/remotemonitoring.png)

Wählen Sie auf der Seite **Create Remote Monitoring solution** (Remoteüberwachungslösung erstellen) eine Bereitstellung vom Typ **Basic** aus. Wenn Sie den Solution Accelerator bereitstellen, um sich damit vertraut zu machen oder ihn zu Demonstrationszwecken auszuführen, wählen Sie zur Kostenminimierung die Option **Basic**.

Wählen Sie als Sprache die Option **.NET**. Die Java- und .NET-Implementierungen verfügen über identische Features.

Geben Sie einen eindeutigen **Lösungsnamen** für den Solution Accelerator für die Remoteüberwachung ein.

Wählen Sie die gewünschten Angaben für **Abonnement** und **Region**, um den Solution Accelerator bereitzustellen. Normalerweise wählen Sie die Region, die Ihnen am nächsten liegt. Sie müssen ein [globaler Administrator oder Benutzer](iot-accelerators-permissions.md) des Abonnements sein.

Klicken Sie auf **Lösung erstellen**, um mit der Bereitstellung zu beginnen. Dieser Prozess dauert mindestens fünf Minuten:

![Details zur Remoteüberwachungslösung](./media/quickstart-remote-monitoring-deploy/createform.png)

## <a name="sign-in-to-the-solution"></a>Anmelden bei der Lösung

Nachdem die Bereitstellung für Ihr Azure-Abonnement abgeschlossen ist, können Sie sich am Dashboard des Solution Accelerators für die Remoteüberwachung anmelden.

Klicken Sie auf der Seite **Bereitgestellte Lösungen** auf Ihren neuen Solution Accelerator für die Remoteüberwachung:

![Auswählen der neuen Lösung](./media/quickstart-remote-monitoring-deploy/choosenew.png)

Im daraufhin angezeigten Bereich können Sie Informationen zum Solution Accelerator für die Remoteüberwachung einsehen. Wählen Sie **Lösungsdashboard**, um Ihren Solution Accelerator für die Remoteüberwachung anzuzeigen:

![Lösungsbereich](./media/quickstart-remote-monitoring-deploy/solutionpanel.png)

Klicken Sie auf **Akzeptieren**, um die Zustimmungsanforderung zu akzeptieren, die im Dashboard für die Remoteüberwachungslösung im Browser angezeigt wird:

[![Lösungsdashboard](./media/quickstart-remote-monitoring-deploy/solutiondashboard-inline.png)](./media/quickstart-remote-monitoring-deploy/solutiondashboard-expanded.png#lightbox)

## <a name="view-your-devices"></a>Anzeigen von Geräten

Im Lösungsdashboard werden die folgenden Informationen zu den Geräten von Contoso angezeigt:

* Unter **Device statistics** (Gerätestatistik) werden zusammenfassende Informationen zu Warnungen und die Gesamtzahl von Geräten angezeigt. In der Standardbereitstellung verfügt Contoso über zehn simulierte Geräte unterschiedlichen Typs.

* Unter **Device locations** (Gerätestandorte) wird angezeigt, wo Ihre Geräte physisch angeordnet sind. Über die Farbe der Nadel wird angegeben, wenn Warnungen für das Gerät vorliegen.

* Unter **Alerts** (Warnungen) sind die Details für die Warnungen Ihrer Geräte angegeben.

* Unter **Telemetry** (Telemetrie) werden Telemetriedaten für Ihre Geräte angezeigt. Sie können unterschiedliche Telemetriedatenströme anzeigen, indem Sie oben auf einen Telemetrietyp klicken.

* Unter **Analytics** (Analyse) werden kombinierte Informationen zu den Warnungen der Geräte angezeigt.

## <a name="respond-to-an-alert"></a>Reagieren auf eine Warnung

Als Bediener bei Contoso können Sie Ihre Geräte über das Lösungsdashboard überwachen. Im Panel **Device statistics** (Gerätestatistik) wird angezeigt, dass einige kritische Warnungen angefallen sind, und im Panel **Alerts** (Warnungen) ist zu sehen, dass die meisten von einer Kältemaschine stammen. Für die Kältemaschinen von Contoso zeigt ein interner Druck von mehr als 250 PSI an, dass das Gerät nicht richtig funktioniert.

### <a name="identify-the-issue"></a>Identifizieren des Problems

Auf der Seite **Dashboard** im Panel **Alerts** (Warnungen) wird die Warnung **Chiller pressure too high** (Druck der Kältemaschine zu hoch) angezeigt. Für die Kältemaschine ist in der Karte eine rote Nadel vorhanden (ggf. müssen Sie die Karte schwenken oder vergrößern):

[![Dashboard: Druckwarnung und Gerät in der Karte](./media/quickstart-remote-monitoring-deploy/dashboardalarm-inline.png)](./media/quickstart-remote-monitoring-deploy/dashboardalarm-expanded.png#lightbox)

Klicken Sie im Panel **Alerts** (Warnungen) in der Spalte **Explore** (Durchsuchen) neben der Regel **Chiller pressure too high** (Druck der Kältemaschine zu hoch) auf **...**. Sie gelangen auf die Seite **Maintenance** (Wartung), auf der Sie die Details der Regel anzeigen können, die die Warnung ausgelöst hat.

Auf der Wartungsseite **Chiller pressure too high** (Druck der Kältemaschine zu hoch) werden die Details zu der Regel angezeigt, die die Warnungen ausgelöst hat. Außerdem ist hier angegeben, wann die Warnungen aufgetreten sind und von welchem Gerät sie ausgelöst wurden:

[![Seite „Wartung“ mit Liste der ausgelösten Warnungen](./media/quickstart-remote-monitoring-deploy/maintenancealarmlist-inline.png)](./media/quickstart-remote-monitoring-deploy/maintenancealarmlist-expanded.png#lightbox)

Sie haben nun das Problem und das zugeordnete Gerät identifiziert, für das die Warnung ausgelöst wurde. Für den Bediener sind die nächsten Schritte das Bestätigen der Warnung und das Beheben des Problems.

### <a name="fix-the-issue"></a>Beheben des Problems

Um für andere Bediener anzugeben, dass Sie nun an der Warnung arbeiten, können Sie sie auswählen und den **Warnungsstatus** in **Bestätigt** ändern:

[![Auswählen und Bestätigen der Warnung](./media/quickstart-remote-monitoring-deploy/maintenanceacknowledge-inline.png)](./media/quickstart-remote-monitoring-deploy/maintenanceacknowledge-expanded.png#lightbox)

Der Wert in der Statusspalte ändert sich in **Bestätigt**.

Scrollen Sie nach unten zu **Verwandte Informationen**, wählen Sie die Kältemaschine in der Liste **Alerted devices** (Geräte mit Warnungen) aus, und wählen Sie anschließend **Aufträge**, um mit der Behebung für die Kältemaschine zu beginnen:

[![Auswählen des Geräts und Planen einer Aktion](./media/quickstart-remote-monitoring-deploy/maintenanceschedule-inline.png)](./media/quickstart-remote-monitoring-deploy/maintenanceschedule-expanded.png#lightbox)

Wählen Sie im Panel **Aufträge** die Option **Run method** (Methode ausführen) und dann die **EmergencyValveRelease**-Methode, fügen Sie den Auftragsnamen **ChillerPressureRelease** hinzu, und klicken Sie auf **Übernehmen**. Mit diesen Einstellungen wird ein Auftrag erstellt, der sofort ausgeführt wird.

Kehren Sie zum Anzeigen des Auftragsstatus auf die Seite **Wartung** zurück, und zeigen Sie die Liste mit den Aufträgen in der Ansicht **Aufträge** an. Unter Umständen müssen Sie einige Sekunden warten, bevor Sie sehen, dass der Auftrag ausgeführt und der Ventildruck für die Kältemaschine verringert wurde:

[![Status der Aufträge in der Ansicht „Aufträge“](./media/quickstart-remote-monitoring-deploy/maintenancerunningjob-inline.png)](./media/quickstart-remote-monitoring-deploy/maintenancerunningjob-expanded.png#lightbox)

### <a name="check-the-pressure-is-back-to-normal"></a>Sicherstellen des normalen Drucks

Navigieren Sie zum Anzeigen Drucktelemetriedaten für die Kältemaschine zur Seite **Dashboard**, wählen Sie im Telemetriepanel die Option **Druck**, und vergewissern Sie sich, dass der Druck für **chiller-02.0** wieder normal ist:

[![Druck ist wieder normal](./media/quickstart-remote-monitoring-deploy/pressurenormal-inline.png)](./media/quickstart-remote-monitoring-deploy/pressurenormal-expanded.png#lightbox)

Navigieren Sie zum Schließen des Vorfalls zur Seite **Maintenance** (Wartung), wählen Sie die Warnung aus, und legen Sie den Status auf **Closed** (Geschlossen) fest:

[![Auswählen und Schließen der Warnung](./media/quickstart-remote-monitoring-deploy/maintenanceclose-inline.png)](./media/quickstart-remote-monitoring-deploy/maintenanceclose-expanded.png#lightbox)

Der Wert in der Statusspalte ändert sich in **Geschlossen**.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mit den Tutorials fortfahren möchten, können Sie die Bereitstellung des Solution Accelerators für die Remoteüberwachung beibehalten.

Falls Sie den Solution Accelerator nicht mehr benötigen, können Sie ihn auf der Seite [Bereitgestellte Lösungen](https://www.azureiotsolutions.com/Accelerators#dashboard) löschen:

![Löschen der Lösung](media/quickstart-remote-monitoring-deploy/deletesolution.png)

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie den Solution Accelerator für die Remoteüberwachung bereitgestellt und eine Überwachungsaufgabe durchgeführt, indem Sie die simulierten Geräte in der Contoso-Standardbereitstellung verwendet haben.

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie die Firmware auf Ihren verbundenen Geräten aktualisieren und Ihre Assets auf dem Solution Accelerator organisieren.

> [!div class="nextstepaction"]
> [Tutorial: Überwachen Ihrer IoT-Geräte](iot-accelerators-remote-monitoring-monitor.md)