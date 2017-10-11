## <a name="prerequisites"></a>Voraussetzungen
Bevor wir CDN Management Code schreiben können, müssen wir eine Vorbereitung des getesteten Codes für die Interaktion mit der Azure-Ressourcen-Manager aktivieren möchten.  Zu diesem Zweck müssen Sie Folgendes ausführen:

* Erstellen Sie eine Ressourcengruppe aus, um den CDN-Profil enthalten, das wir in diesem Lernprogramm erstellen
* Konfigurieren von Azure Active Directory-Authentifizierung für die Anwendung bereitstellen
* Anwenden von Berechtigungen für die Ressourcengruppe, sodass unsere CDN-Profil aus unserem Azure AD-Mandanten nur autorisierte Benutzer interagieren können

### <a name="creating-the-resource-group"></a>Erstellen der Ressourcengruppe
1. Melden Sie sich bei der [Azure-Portal](https://portal.azure.com).
2. Klicken Sie auf die **neu** Schaltfläche in der oberen linken Ecke und dann **Management**, und **Ressourcengruppe**.

    ![Eine neue Ressourcengruppe erstellen](./media/cdn-app-dev-prep/cdn-new-rg-1-include.png)
3. Rufen Sie die Ressourcengruppe *CdnConsoleTutorial*.  Wählen Sie Ihr Abonnement, und wählen Sie einen Speicherort in Ihrer Nähe.  Wenn Sie möchten, können Sie klicken die **an Dashboard anheften** Kontrollkästchen, um die im Portal Ressourcengruppe an das Dashboard anheften.  Dadurch wird es zu einem späteren Zeitpunkt vereinfacht.  Nachdem Sie Ihre Auswahl getroffen haben, klicken Sie auf **erstellen**.

    ![Benennen die Ressourcengruppe](./media/cdn-app-dev-prep/cdn-new-rg-2-include.png)
4. Nachdem die Ressourcengruppe erstellt wird, wenn Sie es an Ihr Dashboard anheften haben nicht, können Sie ihn durch Klicken auf finden **Durchsuchen**, klicken Sie dann **Ressourcengruppen**.  Klicken Sie auf die Ressourcengruppe, um ihn zu öffnen.  Notieren Sie sich Ihre **Abonnement-ID**.  Wir benötigen sie später.

    ![Benennen die Ressourcengruppe](./media/cdn-app-dev-prep/cdn-subscription-id-include.png)

### <a name="creating-the-azure-ad-application-and-applying-permissions"></a>Die Azure AD-Anwendung erstellen und Anwenden von Berechtigungen
Es gibt zwei Ansätze zur Authentifizierung der app bei Azure Active Directory: Einzelbenutzer oder einem Dienstprinzipal. Ein Dienstprinzipal ähnelt ein Dienstkonto in Windows.  Anstelle von erteilen, einen bestimmten Benutzer Berechtigungen für die Interaktion mit dem CDN-Profilen, erteilen Sie stattdessen über die Berechtigungen für den Dienstprinzipal.  Dienstprinzipale werden im Allgemeinen für automatisierte, nicht interaktiven Prozessen verwendet.  Obwohl dieses Lernprogramm ist eine interaktive Konsole app schreiben konzentrieren wir uns auf die Service principal-Ansatz.

Erstellen einen Dienstprinzipal umfasst mehrere Schritte, einschließlich dem Erstellen einer Azure Active Directory-Anwendung.  Zu diesem Zweck wir werden [führen Sie dieses Lernprogramm](../articles/resource-group-create-service-principal-portal.md).

> [!IMPORTANT]
> Achten Sie darauf, dass Sie alle Schritte befolgen die [verknüpften Lernprogramm](../articles/resource-group-create-service-principal-portal.md).  Es ist *äußerst wichtig* , dass Sie genau wie beschrieben ausführen.  Beachten Sie unbedingt Ihre **-Mandanten-ID**, **name_der_mandantendomäne** (häufig eine *. "onmicrosoft.com"* Domäne, sofern Sie eine benutzerdefinierte Domäne angegeben haben), **Client-ID**, und **Client Authentifizierungsschlüssel**, wie wir diese später benötigen.  Seien Sie vorsichtig, zum Schutz Ihrer **Clientkennung** und **Client Authentifizierungsschlüssel**, wie diese Anmeldeinformationen von beliebigen Benutzern verwendet werden können, Ausführen von Vorgängen als den Dienstprinzipal.
>
> Wenn Sie mit dem Schritt, mit dem Namen mehrinstanzenfähige Anwendung konfigurieren erhalten, wählen Sie **keine**.
>
> Wenn erhalten Sie mit dem Schritt [Anwendung Rolle zuweisen](../articles/azure-resource-manager/resource-group-create-service-principal-portal.md#assign-application-to-role), verwenden Sie die zuvor erstellte Ressourcengruppe *CdnConsoleTutorial*, jedoch anstelle von der **Reader** Rolle zuweisen der **CDN-Profil Contributor** Rolle.  Nachdem Sie die Anwendung zuweisen der **CDN-Profil Contributor** -Rolle in der Ressourcengruppe, zu diesem Lernprogramm zurück. 
>
>

Nachdem Sie den Dienstprinzipal erstellt und zugewiesen haben die **CDN-Profil Contributor** Rolle, die **Benutzer** Blatt für Ihre Ressourcengruppe sollte etwa wie folgt aussehen.

![Blatt "Benutzer"](./media/cdn-app-dev-prep/cdn-service-principal-include.png)

### <a name="interactive-user-authentication"></a>Interaktive Benutzerauthentifizierung
Wenn anstelle eines dienstprinzipals Sie stattdessen interaktive Benutzerauthentifizierung für einzelne müssten, ist das sehr ähnlich, die für einen Dienstprinzipal.  Tatsächlich müssen Sie dasselbe Verfahren, aber einige geringfügige Änderungen daran vornehmen.

> [!IMPORTANT]
> Führen Sie nur die folgenden Schritte, wenn Sie einzelne Benutzerauthentifizierung statt einem Dienstprinzipal verwendet werden.
>
>

1. Beim Erstellen Ihrer Anwendung anstelle von **Webanwendung**, wählen Sie **systemeigene Anwendung**.

    ![Systemeigene Anwendung](./media/cdn-app-dev-prep/cdn-native-application-include.png)
2. Auf der nächsten Seite werden Sie aufgefordert für eine **umleitungs-URI**.  Der URI wird nicht überprüft werden, aber beachten Sie, was Sie eingegeben haben.  Sie benötigen ihn später.
3. Besteht keine Notwendigkeit zum Erstellen einer **Client Authentifizierungsschlüssel**.
4. Statt einen Dienstprinzipal zum Zuweisen der **CDN-Profil Contributor** Rolle, wir werden einzelne Benutzer oder Gruppen zuweisen.  In diesem Beispiel können Sie sehen, dass ich zugewiesen habe *CDN Demo Benutzer* auf die **CDN-Profil Contributor** Rolle.  

    ![Einzelne Benutzerzugriff](./media/cdn-app-dev-prep/cdn-aad-user-include.png)
