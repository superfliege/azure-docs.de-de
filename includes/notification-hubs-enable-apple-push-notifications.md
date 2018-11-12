---
title: Includedatei
description: Includedatei
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 08/28/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 8c8f3cd67186450fdcf65c177ea0353d297a3b01
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51263991"
---
## <a name="generate-the-certificate-signing-request-file"></a>Erstellen der Zertifikatsignierungsanforderungs-Datei

Der Apple Push Notification Service (APNS) verwendet Zertifikate zur Authentifizierung Ihrer Pushbenachrichtigungen. Befolgen Sie diese Anweisungen, um das erforderliche Pushzertifikat zum Senden und Empfangen von Benachrichtigungen zu erstellen. Weitere Informationen zu diesen Konzepten finden Sie in der offiziellen Dokumentation zum [Apple Push Notification Service](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html).

Erstellen Sie die Zertifikatsignieranforderungsdatei (CSR-Datei), die von Apple zur Generierung eines signierten Pushzertifikats verwendet wird.

1. Führen Sie auf Ihrem Mac das Tool **Schlüsselbundverwaltung** aus. Es kann im Ordner **Dienstprogramme** oder im Ordner **Andere** auf dem Launchpad geöffnet werden.
2. Klicken Sie auf **Schlüsselbundverwaltung**, erweitern Sie **Zertifikatsassisten**t, und klicken Sie dann auf **Zertifikat einer Zertifizierungsinstanz anfordern …**.

    ![Anfordern eines neuen Zertifikats mithilfe der Schlüsselbundverwaltung](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)
3. Machen Sie entsprechende Angaben unter **E-Mail des Benutzers** sowie **Allgemeiner Name**, vergewissern Sie sich, dass **Auf der Festplatte sichern** aktiviert ist, und klicken Sie dann auf **Weiter**. Lassen Sie das Feld **E-Mail der Zert.-Instanz** leer, da hier keine Eingabe benötigt wird.

    ![Erforderliche Zertifikatinformationen](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-csr-info.png)

4. Geben Sie einen Namen für die Zertifikatsignieranforderungsdatei (CSR-Datei) in **Sichern unter** ein, wählen Sie den Speicherort in **Ort**, und klicken Sie dann auf **Sichern**.

    ![Auswählen eines Dateinamens für das Zertifikat](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-save-csr.png)

    Hierdurch wird die CSR-Datei am ausgewählten Ort gespeichert, wobei „Desktop“ der Standardspeicherort ist. Merken Sie sich den für diese Datei festgelegten Speicherort.

Als Nächstes registrieren Sie Ihre App bei Apple, aktivieren Pushbenachrichtigungen und laden diese exportierte CSR hoch, um ein Pushzertifikat zu erstellen.

## <a name="register-your-app-for-push-notifications"></a>Registrieren der App für Pushbenachrichtigungen

Um Pushbenachrichtigungen an eine iOS-App senden zu können, muss diese bei Apple registriert und auch für Pushbenachrichtigungen angemeldet werden.  

1. Falls Sie Ihre App noch nicht registriert haben, navigieren Sie im Apple Developer Center zum [iOS-Bereitstellungsportal](https://go.microsoft.com/fwlink/p/?LinkId=272456). Melden Sie sich mit Ihrer Apple-ID an, klicken Sie auf **Identifiers**, dann auf **App IDs** und schließlich auf das Symbol **+**, um eine neue App zu registrieren.

    ![iOS-Bereitstellungsportal: Seite mit App-IDs](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids.png)

2. Aktualisieren Sie die folgenden drei Felder für Ihre neue App, und klicken Sie dann auf **Weiter**:

    * **Name**: Geben Sie im Abschnitt **App ID Description** in das Feld **Name** einen beschreibenden Namen für Ihre App ein.
    * **Bundle ID**: Geben Sie im Abschnitt **Explicit App ID** eine **Bundle ID** im Format `<Organization Identifier>.<Product Name>` (entsprechend den Angaben im [App Distribution Guide](https://help.apple.com/xcode/mac/current/#/dev91fe7130a)) ein. Die unter *Organization Identifier* und *Product Name* eingegebenen Werte müssen der Organisationskennung und dem Produktnamen entsprechen, die Sie beim Erstellen des XCode-Projekts verwenden. Im folgenden Screenshot werden *NotificationHubs* als Organisationskennung und *GetStarted* als Produktname verwendet. Wenn Sie sicherstellen, dass diese Werte mit den Werten übereinstimmen, die Sie in Ihrem XCode-Projekt verwenden, können Sie das richtige Veröffentlichungsprofil mit XCode verwenden.
    * **Pushbenachrichtigungen**: Aktivieren Sie die Option **Push Notifications** im Bereich **App Services**.

    ![Formular zum Registrieren einer neuen App-ID](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-appid-info.png)

    Hierdurch wird Ihre App-ID generiert, und Sie werden zur Bestätigung der Daten aufgefordert. Klicken Sie auf **Registrieren** , um die neue App-ID zu bestätigen.

    Nachdem Sie auf **Register** geklickt haben, wird der Bildschirm **Registration complete** geöffnet, wie in der folgenden Abbildung gezeigt. Klicken Sie auf **Fertig**.

    ![Abgeschlossene App-ID-Registrierung mit Berechtigungen](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-registration-complete.png)

3. Suchen Sie im Developer Center unter „App IDs“ nach der soeben erstellten App-ID, und klicken Sie auf die entsprechende Zeile.

    ![App-ID-Liste](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids2.png)

    Durch Klicken auf die App-ID werden Einzelheiten zur App angezeigt. Klicken Sie unten auf dem Bildschirm auf **Bearbeiten** .

    ![Bearbeiten der Seite für App-IDs](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-edit-appid.png)

4. Scrollen Sie bis zur Unterseite des Bildschirms und klicken Sie auf die Schaltfläche **Create Certificate...** unter dem Abschnitt **Development Push SSL Certificate**.

    ![Schaltfläche zum Erstellen eines Zertifikats für die App-ID](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)

    Der Assistent **Add iOS Certificate** wird angezeigt.

    > [!NOTE]
    > In diesem Lernprogramm wird ein Entwicklungszertifikat verwendet. Derselbe Prozess wird auch zum Registrieren eines Produktionszertifikats durchgeführt. Achten Sie darauf, dass Sie denselben Zertifikattyp beim Senden von Benachrichtigungen verwenden.

5. Klicken Sie auf **Choose File**, wechseln Sie zum Speicherort der in der ersten Aufgabe erstellten CSR-Datei und klicken Sie dann auf **Generate**.

    ![CSR-Uploadseite für das generierte Zertifikat](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-cert-choose-csr.png)

6. Nach Erstellung des Zertifikats vom Portal klicken Sie auf die Schaltfläche **Download** und dann auf **Done**.

    ![Downloadseite für das generierte Zertifikat](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)

    Hierdurch wird das Zertifikat heruntergeladen und auf Ihrem Computer im Ordner **Downloads** gespeichert.

    ![Suchen der Zertifikatdatei im Ordner „Downloads“](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)

    > [!NOTE]
    > Standardmäßig ist die heruntergeladene Datei ein Entwicklungszertifikat namens **aps_development.cer**.

7. Doppelklicken Sie auf dem heruntergeladenen Pushzertifikat **aps_development.cer**.

    Das neue Zertifikat wird im Schlüsselbund installiert, wie in der folgenden Abbildung gezeigt:

    ![Zertifikatliste der Schlüsselbundverwaltung mit neuem Zertifikat](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)

    > [!NOTE]
    > Der Name in Ihrem Zertifikat kann anders lauten, enthält jedoch das Präfix **Apple Development iOS Push Services**.

8. Klicken Sie in der Kategorie **Zertifikate** in der Schlüsselbundverwaltung mit der rechten Maustaste auf das neu erstellte Pushzertifikat. Klicken Sie auf **Exportieren**, benennen Sie die Datei, wählen Sie das Format **.p12** aus, und klicken Sie dann auf **Speichern**.

    ![Exportieren des Zertifikats im p12-Format](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-export-cert-p12.png)

    Notieren Sie sich den Dateinamen und den Speicherort des exportierten P12-Zertifikats, das zum Aktivieren der Authentifizierung mit APNS verwendet wird.

    > [!NOTE]
    > In diesem Lernprogramm wird eine QuickStart.p12-Datei erstellt. Name und Ort Ihrer Datei können verschieden sein.

## <a name="create-a-provisioning-profile-for-the-app"></a>Erstellen eines Bereitstellungsprofils für die App

1. Klicken Sie im [iOS-Bereitstellungsportal](https://go.microsoft.com/fwlink/p/?LinkId=272456) auf **Provisioning Profiles** > **All**, und klicken Sie schließlich auf die Plusschaltfläche (**+**), um ein neues Profil zu erstellen. Der Assistent **Add iOS Provisioning Profile** wird angezeigt:

    ![Bereitstellungsprofilliste](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)

2. Wählen Sie unter **Development** die Option **iOS App Development** als Bereitstellungsprofiltyp aus, und klicken Sie dann auf **Continue**.

3. Wählen Sie anschließend die soeben erstellte App-ID in der Dropdownliste **App ID** aus, und klicken Sie auf **Continue**.

    ![Auswählen der App-ID](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)

4. Wählen Sie im Bildschirm **Select certificates** das normalerweise für die Codesignatur verwendete Entwicklungszertifikat aus, und klicken Sie auf **Continue**. Dabei handelt es sich nicht um das erstellte Pushzertifikat.

    ![Auswählen des Zertifikats](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-select-cert.png)

5. Anschließend wählen Sie die zum Testen zu verwendenden **Devices**, und klicken Sie auf **Continue**.

    ![Auswählen der Geräte](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-select-devices.png)

6. Geben Sie schließlich einen Namen für das Profil im Feld **Profile Name** ein, und klicken Sie auf **Generate**.

    ![Auswählen eines Bereitstellungsprofilnamens](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)

7. Nachdem das neue Bereitstellungsprofil erstellt wurde, klicken Sie darauf, um es herunterzuladen und auf Ihrem Xcode-Entwicklungscomputer zu installieren. Klicken Sie anschließend auf **Fertig**.

    ![Herunterladen des Bereitstellungsprofils](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-profile-ready.png)
