<!--author=alkohli last changed: 03/17/16-->

## <a name="troubleshooting-update-failures"></a>Problembehandlung bei Fehler beim update
**Was geschieht, wenn Sie eine Benachrichtigung angezeigt, die vor dem upgrade Überprüfungen fehlerhaft waren?**

Wenn bei der Vorprüfung ein Fehler auftritt, stellen Sie sicher, dass Sie in der detaillierte Benachrichtigungsleiste am unteren Rand der Seite besprochen haben. Dies bietet Anleitungen darüber, welche bei der Vorprüfung fehlgeschlagen ist. Die folgende Abbildung zeigt eine Instanz, in der diese eine Benachrichtigung angezeigt wird. In diesem Fall sind die controllerintegritätsprüfung und Hardware Komponente integritätsprüfung fehlgeschlagen. Klicken Sie unter der **Hardwarestatus** Abschnitt sehen Sie, die beide **Controller 0** und **Controller 1** Komponenten Aufmerksamkeit.

  ![Fehler bei der Vorprüfung](./media/storsimple-install-troubleshooting/HCS_PreUpdateCheckFailed-include.png)

Sie benötigen, um sicherzustellen, dass beide Controller fehlerfrei und online sind. Sie müssen auch sicherstellen, dass alle Hardwarekomponenten in der StorSimple-Gerät angezeigt werden, auf der Seite "Wartung" fehlerfrei sind. Sie können dann versuchen, Updates zu installieren. Wenn Sie nicht die Komponente Hardwareprobleme beheben können, müssen Sie für die nächsten Schritte von Microsoft Support Services.

**Was geschieht, wenn Sie eine Fehlermeldung "Updates konnten nicht installiert werden.", und die Empfehlung erhalten, ist zum Verweisen auf das Update, das zum Bestimmen der Ursache des Fehlers Handbuch zur Problembehandlung?**

Eine wahrscheinliche Ursache dafür ist möglicherweise, dass Sie nicht über Konnektivität mit Microsoft Update-Servern verfügen. Dies ist eine manuelle Überprüfung, die ausgeführt werden muss. Wenn Sie die Konnektivität mit dem Updateserver verlieren, würde der updateauftrag fehlschlagen. Die Konnektivität können Sie überprüfen, indem Sie das folgende Cmdlet ausführen, von der Windows PowerShell-Schnittstelle Ihres StorSimple-Geräts:

 `Test-Connection -Source <Fixed IP of your device controller> -Destination <Any IP or computer name outside of datacenter>`

Führen Sie das Cmdlet auf beiden Controllern.

Wenn Sie überprüft haben, die Konnektivität vorhanden ist und Sie den Vorgang fortsetzen, um dieses Problem angezeigt wird, wenden Sie sich an den Microsoft Support, für die nächsten Schritte.

**Was geschieht, wenn einen Fehler angezeigt werden, wenn Ihr Gerät zu Update 4 und beide Controller zu aktualisieren sind Update 4 ausgeführt?**

Starten Update 4, wenn beide Controller dieselbe Softwareversion ausgeführt werden und es ein Fehler beim Aktualisieren wird, gehen die Controller im Wiederherstellungsmodus nicht. Diese Situation kann auftreten, wenn das Gerät Software-Hotfix (1. Reihenfolge) Updates für beide Controller erfolgreich jedoch andere Hotfixes (2. Ordnung und 3. Reihenfolge) sind noch angewendet werden. Starten Update 4, geht die Controller in den Wiederherstellungsmodus nur, wenn die beiden Controller unterschiedliche Softwareversionen ausgeführt werden. 

Wenn dem Benutzer einen Fehler angezeigt wird, wenn beide Controller Update 4 ausgeführt werden, wird empfohlen, dass sie warten Sie einige Minuten, und wiederholen Sie dann auf aktualisieren. Wenn die Wiederholung nicht erfolgreich ist, sollten sie Microsoft Support Services.
