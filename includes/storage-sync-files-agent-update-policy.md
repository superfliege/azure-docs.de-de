Der Azure File Sync-Agent wird regelmäßig aktualisiert, um neue Funktionalität hinzuzufügen und Probleme zu beheben. Es wird empfohlen, Microsoft Update zu konfigurieren, um Updates für den Azure File Sync-Agent zu erhalten, wenn diese verfügbar sind.

#### <a name="major-vs-minor-agent-versions"></a>Unterschiede zwischen Haupt- und Nebenversionen des Agents
* Hauptversionen des Agents enthalten oft neue Features und haben eine aufsteigende Zahl als ersten Teil der Versionsnummer. Beispiel: *2.\*.\**
* Nebenversionen des Agents werden auch als „Patches“ bezeichnet und werden häufiger veröffentlicht als Hauptversionen. Sie enthalten oft Fehlerbehebungen und kleinere Verbesserungen, aber keine neuen Features. Beispiel: *\*.3.\**

#### <a name="upgrade-paths"></a>Upgradepfade
Es gibt drei bewährte und getestete Wege, um die Updates des Azure File Sync-Agents zu installieren. Diese Updatepfade funktionieren sowohl für Haupt- als auch für Nebenversionen.
1. **(Bevorzugt) Konfigurieren Sie Microsoft Update zum automatischen Herunterladen und Installieren von Agent-Updates.**  
    Es wird stets empfohlen, jedes Azure File Sync-Update auszuführen, um sicherzustellen, dass Sie Zugriff auf die neuesten Fehlerbehebungen für den Server-Agent haben. Mit Microsoft Update ist dies ein nahtloser Prozess, da Updates automatisch für Sie heruntergeladen und installiert werden.
2. **Patchen Sie einen vorhandenen Azure File Sync-Agent mithilfe einer Microsoft Update-Patchdatei oder einer ausführbaren MSP-Datei. Das neueste Azure File Sync-Updatepaket kann aus dem [Microsoft Update-Katalog](https://www.catalog.update.microsoft.com/Search.aspx?q=Azure%20File%20Sync) heruntergeladen werden.**  
    Wenn Sie eine ausführbare MSP-Datei verwenden, wird Ihre Azure File Sync-Installation mit der gleichen Methode aktualisiert, die auch von Microsoft Update automatisch im vorherigen Upgradepfad verwendet wird. Das Anwenden eines Microsoft Update-Patches führt ein direktes Upgrade einer Azure File Sync-Installation aus.
3. **Laden Sie den neuesten Azure File Sync-Agent aus dem [Microsoft Download Center](https://go.microsoft.com/fwlink/?linkid=858257) herunter. Als Download steht ein Microsoft-Installer-Paket oder eine ausführbare MSI-Datei zur Verfügung.**  
    Um eine bestehende Azure File Sync-Agent-Installation zu aktualisieren, deinstallieren Sie die ältere Version und installieren Sie dann die neueste Version mit dem heruntergeladenen Installationsprogramm. Die Serverregistrierung, Synchronisierungsgruppen und alle anderen Einstellungen werden vom Azure File Sync-Installationsprogramm beibehalten.

#### <a name="agent-lifecycle-and-change-management-guarantees"></a>Garantien zu Agent-Lebenszyklus und Änderungsverwaltung
Azure File Sync ist ein Clouddienst, der die kontinuierliche Einführung neuer Features und Funktionen ermöglicht. Dies bedeutet, dass eine bestimmte Azure File Sync-Agent-Version nur für eine begrenzte Zeit unterstützt werden kann. Für eine einfachere Bereitstellung gelten die folgenden Regeln, damit sichergestellt ist, dass Sie genügend Zeit haben und benachrichtigt werden, um Updates/Upgrades von Agents in Ihrem Änderungsverwaltungsprozess zu berücksichtigen:

- Die Hauptversionen des Agents werden für mindestens sechs Monate ab dem Datum der Erstveröffentlichung unterstützt.
- Eine Überlappung von mindestens drei Monaten bei der Unterstützung der Agent-Hauptversionen wird garantiert. 
- Warnungen werden für registrierte Server ausgegeben, die einen bald ablaufenden Agent mindestens drei Monate vor Ablauf verwenden. Ob ein registrierter Server eine ältere Version des Agents verwendet, können Sie im Abschnitt „Registrierte Server“ eines Speichersynchronisierungsdiensts überprüfen.
- Die Lebensdauer einer Nebenversion des Agents hängt von der zugehörigen Hauptversion ab. Wenn beispielsweise die Agent-Version 3.0 freigegeben wird, werden alle Agent-Versionen 2.\* gleichzeitig ablaufen.

> [!Note]
> Bei der Installation einer Agent-Version mit einer Ablaufmeldung wird zwar eine Warnung angezeigt, sie ist aber dennoch erfolgreich. Der Versuch einer Installation oder Verbindung mit einer abgelaufenen Agent-Version wird nicht unterstützt und wird daher blockiert.