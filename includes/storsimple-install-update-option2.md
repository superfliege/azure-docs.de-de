<!--author=SharS last changed: 03/17/2016-->

#### <a name="to-install-update-12-from-the-azure-classic-portal"></a>So installieren Sie Update 1.2 über das klassische Azure-portal
1. Wechseln Sie im klassischen Azure-Portal, um die **Geräte** Seite und wählen Sie Ihr Gerät.
2. Navigieren Sie zu **Geräte** > **konfigurieren**.
3. Klicken Sie unter **Netzwerkschnittstellen**, stellen Sie zuerst sicher, dass Sie mindestens eine Netzwerkschnittstelle verfügen, der iSCSI-aktiviert ist. Suchen Sie die Netzwerkschnittstelle (als DATA 0), die ein Gateway zugewiesen wurde.
4. Deaktivieren Sie die Netzwerkschnittstelle, die ein Gateway zugewiesen wurde, und speichern Sie die geänderte Konfiguration. Beachten Sie die netzwerkschnittstelleneinstellungen beibehalten werden und daher, wenn Sie diese Netzwerkschnittstelle später wieder aktivieren, das Portal zurückversetzt auf die ursprünglichen Einstellungen.
5. Sie können jetzt [klassische Azure-Portal So installieren Sie Update 1.2 verwenden](#install-update-12-via-the-azure-classic-portal). Befolgen Sie die Anweisungen, die beginnend mit Schritt 3 dieses Verfahrens fort. Nachdem Sie alle Updates installiert haben, können Sie die Netzwerkschnittstelle wieder aktivieren, die deaktiviert.

