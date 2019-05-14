---
title: Erstellen einer Azure AD-App-Registrierung und eines Benutzers für Azure Red Hat OpenShift | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie einen Dienstprinzipal erstellen, ein Clientgeheimnis und eine Rückruf-URL für die Authentifizierung generieren sowie einen neuen Active Directory-Benutzer zum Testen von Apps in Ihrem Microsoft Azure Red Hat OpenShift-Cluster erstellen.
author: tylermsft
ms.author: twhitney
ms.service: openshift
manager: jeconnoc
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/06/2019
ms.openlocfilehash: de3f3c30848d26ea399bcccc29a6149a149f6a55
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65080584"
---
# <a name="create-an-azure-ad-app-registration-and-user-for-azure-red-hat-openshift"></a>Erstellen einer Azure AD-App-Registrierung und eines Benutzers für Azure Red Hat OpenShift

Microsoft Azure Red Hat OpenShift benötigt Berechtigungen zum Ausführen von Tasks für Ihren Cluster. Wenn Ihre Organisation noch keine Azure AD-App-Registrierung (Azure Active Directory) zur Verwendung als Dienstprinzipal besitzt, befolgen Sie zu deren Erstellung die folgenden Anweisungen.

Dieses Thema endet mit Anweisungen zum Erstellen eines neuen Azure AD-Benutzers, den Sie für den Zugriff auf Apps benötigen, die in Ihrem Azure Red Hat OpenShift-Cluster ausgeführt werden.

Wenn Sie noch keinen Azure AD-Mandanten erstellt haben, befolgen Sie die Anweisungen unter [Erstellen von Azure AD-Mandanten für Azure Red Hat OpenShift](howto-create-tenant.md), bevor Sie mit dieser Anleitung fortfahren.

## <a name="create-a-new-app-registration"></a>Erstellen einer neuen App-Registrierung

Eine Anwendung muss zunächst in einem Azure AD-Mandanten registriert werden, um die Funktionen von Azure AD nutzen zu können. Während des Registrierungsvorgangs müssen Details zu Ihrer Anwendung in Azure AD angegeben werden, darunter die URL für den Speicherort der App, die URL, an die nach der Authentifizierung eines Benutzers Antworten gesendet werden sollen, der URI zum Identifizieren der App usw.

1. Stellen Sie im [Azure-Portal](https://portal.azure.com) sicher, dass Ihr Mandant unter Ihrem Benutzernamen oben rechts im Portal angezeigt wird:

    ![Screenshot des Portals mit Anzeige des Mandanten oben rechts][tenantcallout] Wenn der falsche Mandant angezeigt wird, klicken Sie oben rechts auf Ihren Benutzernamen. Klicken Sie dann auf **Verzeichnis wechseln**, und wählen Sie den richtigen Mandanten aus der Liste **Alle Verzeichnisse** aus.

2. Öffnen Sie das Blatt [App-Registrierungen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps), und klicken Sie auf **Registrierung einer neuen Anwendung**.
3. Geben Sie im Bereich **Erstellen** einen Anzeigenamen für Ihr Anwendungsobjekt ein.
4. Stellen Sie sicher, dass **Anwendungstyp** auf *Web-App/API* festgelegt ist.
5. Erstellen Sie eine **Anmelde-URL** nach folgendem Muster:

    ```
    https://<cluster-name>.<azure-region>.cloudapp.azure.com
    ```

    . . . Hierbei steht `<cluster-name>` für den beabsichtigten Name Ihres Azure Red Hat OpenShift-Clusters und `<azure-region>` für die [Azure-Region, die Ihren Azure Red Hat OpenShift-Cluster hostet](supported-resources.md#azure-regions). Wenn Ihr Cluster beispielsweise `contoso` heißen und in der Region `eastus` erstellt werden soll, geben Sie als vollqualifizierten Domänennamen (FQDN) für die **Anmelde-URL** Folgendes ein: `https://contoso.eastus.cloudapp.azure.com`.

    > [!IMPORTANT]
    > Der Clustername darf nur Kleinbuchstaben enthalten, und die FQDN-URL muss eindeutig sein.
    > Stellen Sie sicher, dass Sie einen ausreichend eindeutigen Namen für Ihren Cluster auswählen.

    Notieren Sie den Namen Ihres Clusters und die Anmelde-URL – Sie benötigen diese Angaben später für den Zugriff auf Apps, die in Ihrem Cluster ausgeführt werden. Im Tutorial [Erstellen eines Azure Red Hat OpenShift-Clusters](tutorial-create-cluster.md) bezeichnen wir den Clusternamen als `CLUSTER_NAME` und diese Anmelde-URL als `FQDN`.

6. Stellen Sie sicher, dass der Wert für die **Anmelde-URL** nach Überprüfung mit einem grünen Häkchen markiert wird. (Drücken Sie die TAB-TASTE, um den Fokus aus dem Feld *Anmelde-URL* zu verschieben, und führen Sie die Überprüfung durch.)
7. Klicken Sie auf die Schaltfläche **Erstellen**, um das Azure AD-Anwendungsobjekt zu erstellen.
8. Notieren Sie sich von der angezeigten Seite **Registrierte App** die **Anwendungs-ID**. Im Tutorial [Erstellen eines Azure Red Hat OpenShift-Clusters](tutorial-create-cluster.md) bezeichnen wir diesen Wert als `APPID`.

    ![Screenshot des Textfelds mit der Anwendungs-ID][appidimage]
    
Weitere Informationen zu Azure-Anwendungsobjekten finden Sie unter [Anwendungs- und Dienstprinzipalobjekte in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals).

Details zur Erstellung einer neuen Azure AD-Anwendung finden Sie unter [Registrieren einer App mit dem Azure AD v1.0-Endpunkt](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-add-azure-ad-app).

### <a name="create-a-client-secret"></a>Erstellen eines Clientgeheimnisses

Jetzt sind Sie bereit, ein Clientgeheimnis zur Authentifizierung Ihrer App bei Azure Active Directory zu generieren.

1. Klicken Sie auf der Seite **Registrierte App** auf **Einstellungen**, um die Einstellungen für Ihre registrierte App zu öffnen.
2. Klicken Sie im daraufhin angezeigten Bereich **Einstellungen** auf **Schlüssel**.  Der Bereich **Schlüssel** wird angezeigt.
![Screenshot der Seite zum Erstellen von Schlüsseln im Portal][createkeyimage]
3. Geben Sie eine **Beschreibung** für den Schlüssel ein.
4. Legen Sie einen Wert für **Läuft ab** fest, z.B. *In 2 Jahren*.
5. Klicken Sie auf **Speichern**. Der Schlüsselwert wird angezeigt.
6. Notieren Sie sich den Schlüsselwert. Im Tutorial [Erstellen eines Azure Red Hat OpenShift-Clusters](tutorial-create-cluster.md) bezeichnen wir diesen Wert als `SECRET`.

### <a name="create-a-reply-url"></a>Erstellen einer Antwort-URL

Mithilfe des App-Objekts *Antwort-URL* gibt Azure AD den Rückruf an, der beim Autorisieren der App verwendet werden soll. Im Fall einer Web-API oder Webanwendung ist die Antwort-URL der Ort, an den Azure AD die Authentifizierungsantwort sendet (einschließlich eines Tokens, sofern die Authentifizierung erfolgreich war).

Selbst der geringste Unterschied (nachgestellter Schrägstrich fehlt, unterschiedliche Groß-/Kleinschreibung) führt dazu, dass der Vorgang zur Tokenausstellung nicht durchgeführt wird und Sie sich nicht anmelden können.

1. Wechseln Sie zurück zum Bereich **Einstellungen** (klicken Sie in der Breadcrumb-Leiste am oberen Rand des Portals auf **Einstellungen**), und klicken Sie rechts auf **Antwort-URLs**.  Der Bereich **Antwort-URLs** wird angezeigt.
2. Die erste Antwort-URL in der Liste ist der `FQDN`-Wert aus Schritt 6 unter [Erstellen einer neuen App-Registrierung](#create-a-new-app-registration). Bearbeiten Sie ihn, und fügen Sie `/oauth2callback/Azure%20AD` an.  Ihre Antwort-URL sollte in etwa folgendermaßen aussehen: `https://mycluster.eastus.cloudapp.azure.com/oauth2callback/Azure%20AD`.
3. Klicken Sie auf **Speichern**, um die Antwort-URL zu speichern.

## <a name="create-a-new-active-directory-user"></a>Erstellen eines neuen Active Directory-Benutzers

Erstellen Sie einen neuen Benutzer in Active Directory, mit dem Sie sich bei der App anmelden möchten, die in Ihrem Azure Red Hat OpenShift-Cluster ausgeführt wird.

1. Wechseln Sie zum Blatt [Benutzer – Alle Benutzer](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UsersManagementMenuBlade/AllUsers).
2. Klicken Sie auf **+ Neuer Benutzer**. Der Bereich **Benutzer** wird geöffnet.
3. Geben Sie einen **Namen** ein, den Sie für diesen Benutzer verwenden möchten.
4. Erstellen Sie einen **Benutzernamen** basierend auf dem Namen des erstellten Mandanten, und fügen Sie am Ende `.onmicrosoft.com` an. Beispiel: `yourUserName@yourTenantName.onmicrosoft.com`. Notieren Sie sich diesen Benutzernamen. Sie benötigen ihn zur Anmeldung, um die App in Ihrem Cluster zu verwenden.
5. Klicken Sie auf **Verzeichnisrolle**, und wählen Sie **Globaler Administrator** aus. Klicken Sie dann am unteren Rand des Bereichs auf **OK**.
6. Klicken Sie in der Mitte des Bereichs **Benutzer** auf **Kennwort anzeigen**, und notieren Sie das temporäre Kennwort. Nachdem Sie sich zum ersten Mal angemeldet haben, werden Sie zum Zurücksetzen des Kennworts aufgefordert.
7. Klicken Sie im unteren Bereich auf **Erstellen**, um den Benutzer zu erstellen.

## <a name="resources"></a>Ressourcen

* [Anwendungs- und Dienstprinzipalobjekte in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals)  
* [Schnellstart: Registrieren einer App mit dem Azure AD v1.0-Endpunkt](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-add-azure-ad-app)  

[appidimage]: ./media/howto-create-tenant/get-app-id.png
[createkeyimage]: ./media/howto-create-tenant/create-key.png
[tenantcallout]: ./media/howto-create-tenant/tenant-callout.png

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie alle [Azure Red Hat OpenShift-Voraussetzungen](howto-setup-environment.md) erfüllt haben, können Sie Ihren ersten Cluster erstellen!

Lesen Sie dieses Tutorial:
> [!div class="nextstepaction"]
> [Erstellen eines Azure Red Hat OpenShift-Clusters](tutorial-create-cluster.md)
