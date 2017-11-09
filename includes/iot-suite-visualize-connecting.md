## <a name="view-device-telemetry"></a>Anzeigen der Gerätetelemetrie

Sie können die von Ihrem Gerät gesendeten Telemetriedaten in der Lösung auf der Seite **Geräte** sehen.

1. Wählen Sie auf der Seite **Geräte** in der Liste der Geräte das von Ihnen bereitgestellte Gerät aus. In einem Bereich werden Informationen über Ihr Gerät angezeigt, einschließlich eines Diagramms der Gerätetelemetrie:

    ![Gerätedetails anzeigen](media/iot-suite-visualize-connecting/devicesdetail.png)

1. Wählen Sie **Druck**, um die Anzeige der Telemetriedaten zu ändern:

    ![Telemetriedaten zum Druck anzeigen](media/iot-suite-visualize-connecting/devicespressure.png)

1. Um Diagnoseinformationen zu Ihrem Gerät anzuzeigen, scrollen Sie nach unten zu **Diagnose**:

    ![Gerätediagnose anzeigen](media/iot-suite-visualize-connecting/devicesdiagnostics.png)

## <a name="act-on-your-device"></a>Aktionen auf Ihrem Gerät

Um Methoden auf Ihren Geräten aufzurufen, verwenden Sie in der Lösung für die Remoteüberwachung die Seite **Geräte**. In der Lösung für die Remoteüberwachung implementieren **Kühlgeräte** beispielsweise eine **Reboot**-Methode.

1. Wählen Sie **Geräte**, um zur Seite **Geräte** in der Lösung zu navigieren.

1. Wählen Sie auf der Seite **Geräte** in der Liste der Geräte das von Ihnen bereitgestellte Gerät aus:

    ![Auswählen des physischen Geräts](media/iot-suite-visualize-connecting/devicesselect.png)

1. Um eine Liste der Methoden anzuzeigen, können Sie diese auf Ihrem Gerät aufrufen und **Planen** wählen. Um eine Methode für die Ausführung auf mehreren Geräten zu planen, können Sie mehrere Geräte in der Liste auswählen. Der Bereich **Planen** zeigt die für alle ausgewählten Geräte gemeinsamen Methodentypen an.

1. Wählen Sie **Reboot**, legen den Auftragsnamen auf **RebootPhysicalChiller** fest, und klicken Sie auf **Übernehmen**:

    ![Planen des Neustarts](media/iot-suite-visualize-connecting/deviceschedule.png)

1. In der Konsole wird bei der Ausführung Ihres Gerätecodes eine Meldung angezeigt, wenn das Gerät die Methode verarbeitet.

> [!NOTE]
> Um den Status des Auftrags in der Lösung nachzuverfolgen, wählen Sie **Anzeigen**.

## <a name="next-steps"></a>Nächste Schritte

Im Artikel [Anpassen der vorkonfigurierten Lösung für die Remoteüberwachung](../articles/iot-suite/iot-suite-remote-monitoring-customize.md) werden einige Möglichkeiten zur Anpassung der vorkonfigurierten Lösung beschrieben.