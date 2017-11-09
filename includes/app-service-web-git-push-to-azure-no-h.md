Fügen Sie Ihrem lokalen Git-Repository im lokalen Terminalfenster einen Azure-Remotespeicherort hinzu. Dieser Azure-Remotespeicherort wurde für Sie in [Web-App erstellen](#create-a-web-app) erstellt.

```bash
git remote add azure <URI from previous step>
```

Führen Sie einen Pushvorgang zum Azure-Remotespeicherort durch, um Ihre App mit dem folgenden Befehl bereitzustellen. Stellen Sie sicher, dass Sie bei der entsprechenden Aufforderung das Kennwort eingeben, das Sie unter [Konfigurieren eines Bereitstellungsbenutzers](#configure-a-deployment-user) erstellt haben, und nicht das Kennwort für die Anmeldung am Azure-Portal.

```bash
git push azure master
```

Mit dem vorherigen Befehl werden Informationen angezeigt, die den Informationen im folgenden Beispiel ähneln:
