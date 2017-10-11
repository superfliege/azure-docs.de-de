Aufgrund der ständigen Entwicklung entsprechen in Android Studio installierte Android-SDK-Version möglicherweise nicht die Version im Code. Android SDK, auf die verwiesen wird in diesem Lernprogramm wird die Version 23, die neueste zum Zeitpunkt der Verfassung. Die Versionsnummer erhöhen, da neue Versionen des SDK angezeigt, und es wird empfohlen, die neueste verfügbare Version.

Versionskonflikt zwei Symptome sind:

- Wenn Sie erstellen oder das Projekt neu erstellen, erhalten Sie möglicherweise Fehlermeldungen wie Gradle "**Fehler beim Suchen des Ziel Google Inc.:Google APIs:n**".
- Standard-Android-Objekte im Code, der aufgelöst werden soll, basierend auf `import` Anweisungen möglicherweise Fehlermeldungen generiert werden.

Wenn eine dieser Optionen angezeigt wird, entsprechen die Version von Android SDK in Android Studio installiert das SDK-Ziel des heruntergeladenen Projekt möglicherweise nicht. Um die Version zu überprüfen, nehmen Sie die folgenden Änderungen:

1. Klicken Sie in Android Studio **Tools** > **Android** > **SDK-Manager**. Wenn Sie nicht die neueste Version der Plattform SDK installiert haben, klicken Sie dann auf um es zu installieren. Notieren Sie die Versionsnummer.
2. Auf der **Projektexplorer** Registerkarte **Gradle-Skripts**, öffnen Sie die Datei **build.gradle (Modeule: app)**. Sicherstellen, dass die **CompileSdkVersion** und **BuildToolsVersion** festgelegt sind, auf die neueste SDK-Version installiert. Die Tags können wie folgt aussehen:

             compileSdkVersion 'Google Inc.:Google APIs:23'
            buildToolsVersion "23.0.2"
3. Android Studio-Projekt-Explorer mit der Maustaste des Projektknotens, wählen Sie **Eigenschaften**, und wählen Sie in der linken Spalte **Android**. Sicherstellen, dass die **Projektziel erstellen** festgelegt ist, auf die gleiche SDK-Version als die **TargetSdkVersion**.

In Android Studio ist die Manifestdatei nicht mehr an das Ziel SDK und die minimale SDK-Version, anders als bei Eclipse verwendet.
