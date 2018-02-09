## <a name="test-your-code"></a>Testen Ihres Codes

1. Stellen Sie Ihren Code Ihrem Gerät/Geräteemulator bereit.

2. Wenn Sie Ihre Anwendung testen möchten, verwenden Sie für die Anmeldung ein Azure Active Directory-Konto (Geschäfts-, Schul- oder Unikonto) oder ein Microsoft-Konto (live.com, outlook.com). 

    ![Testen Ihrer Anwendung](media/active-directory-develop-guidedsetup-android-test/mainwindow.png)
    <br/><br/>
    ![Eingeben von Benutzername und Kennwort](media/active-directory-develop-guidedsetup-android-test/usernameandpassword.png)

### <a name="provide-consent-for-application-access"></a>Zustimmen zum Anwendungszugriff
Bei der ersten Anmeldung bei Ihrer Anwendung werden Sie zudem aufgefordert, Ihre Zustimmung zu geben, dass die Anwendung auf Ihr Profil zugreifen und Sie anmelden darf, wie hier gezeigt: 

![Zustimmen zum Anwendungszugriff](media/active-directory-develop-guidedsetup-android-test/androidconsent.png)


### <a name="view-application-results"></a>Anzeigen von Anwendungsergebnissen
Nachdem Sie sich angemeldet haben, sollten die Ergebnisse angezeigt werden, die durch den Aufruf der Microsoft Graph-API zurückgegeben werden. Der Aufruf des **me**-Endpunkt der Microsoft Graph-API gibt das [Benutzerprofil](https://graph.microsoft.com/v1.0/me) zurück. Eine Liste der allgemeinen Microsoft Graph-Endpunkte finden Sie in der Entwicklerdokumentation mit einer [Übersicht über Microsoft Graph](https://developer.microsoft.com/graph/docs#common-microsoft-graph-queries).

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Weitere Informationen zu Bereichen und delegierten Berechtigungen

Die Microsoft Graph-API benötigt den Bereich *user.read*, um das Benutzerprofil zu lesen. Dieser Bereich wird standardmäßig jeder Anwendung automatisch hinzugefügt, die im Anwendungsregistrierungsportal registriert wird. Andere Microsoft Graph-APIs sowie benutzerdefinierte APIs für Ihren Back-End-Server erfordern unter Umständen zusätzliche Bereiche. Die Microsoft Graph-API benötigt den Bereich *Calendars.Read*, um die Kalender des Benutzers aufzuführen. 

Um auf die Kalender des Benutzers im Kontext einer Anwendung zugreifen zu können, müssen Sie den Informationen für die Anwendungsregistrierung die delegierte Berechtigung *Calendars.Read* hinzufügen. Fügen Sie dann dem Aufruf von `acquireTokenSilent` den Bereich *Calendars.Read* hinzu. 

>[!NOTE]
>Wenn Sie die Anzahl der Bereiche erhöhen, werden Benutzer ggf. zu weiteren Genehmigungen aufgefordert.

<!--end-collapse-->

[!INCLUDE [Help and support](active-directory-develop-help-support-include.md)]
