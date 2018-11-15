---
title: Einrichten von Azure Active Directory für Service Fabric-Clientauthentifizierung | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Azure Active Directory (Azure AD) für die Authentifizierung von Clients für Service Fabric-Cluster eingerichtet wird.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: chackdan
ms.assetid: 15d0ab67-fc66-4108-8038-3584eeebabaa
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/15/2018
ms.author: aljo
ms.openlocfilehash: 75ba2ee378e9eddfeaeb2346b4d5bb584844afe2
ms.sourcegitcommit: 542964c196a08b83dd18efe2e0cbfb21a34558aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2018
ms.locfileid: "51636663"
---
# <a name="set-up-azure-active-directory-for-client-authentication"></a>Einrichten von Azure Active Directory für die Clientauthentifizierung

Für in Azure ausgeführte Cluster wird Azure Active Directory (Azure AD) zum Sichern des Zugriffs auf Verwaltungsendpunkte empfohlen.  In diesem Artikel wird die Einrichtung von Azure AD zum Authentifizieren von Clients für einen Service Fabric-Cluster beschrieben. Diese muss vor dem [Erstellen des Clusters](service-fabric-cluster-creation-via-arm.md) erfolgen.  Mit Azure AD können Organisationen (so genannte Mandanten) den Benutzerzugriff auf Anwendungen verwalten. Bei den Anwendungen wird zwischen Anwendungen mit webbasierter Anmeldebenutzeroberfläche und Anwendungen mit nativer Clientumgebung unterschieden. In diesem Artikel wird davon ausgegangen, dass Sie bereits einen Mandanten erstellt haben. Falls nicht, sollten Sie sich zunächst mit dem Artikel [Einrichten eines Azure Active Directory-Mandanten][active-directory-howto-tenant] befassen.

## <a name="create-azure-ad-applications"></a>Erstellen von Azure AD-Anwendungen
Service Fabric-Cluster bieten unterschiedliche Einstiegspunkte für ihre Verwaltungsfunktionen. Hierzu zählen etwa der webbasierte [Service Fabric Explorer][service-fabric-visualizing-your-cluster] und [Visual Studio][service-fabric-manage-application-in-visual-studio]. Daher erstellen Sie zwei Azure AD-Anwendungen, um den Zugriff auf den Cluster zu steuern: eine Webanwendung und eine native Anwendung.  Nachdem die Anwendungen erstellt wurden, weisen Sie Benutzer zu schreibgeschützten Rollen und Administratorrollen zu.

Wir haben eine Reihe von Windows PowerShell-Skripts erstellt, um einige Schritte der Konfiguration von Azure AD mit einem Service Fabric-Cluster zu vereinfachen.

> [!NOTE]
> Vor der Clustererstellung müssen folgende Schritte ausgeführt werden. Da in den Skripts Clusternamen und Endpunkte erwartet werden, sollte es sich bei den Werten nicht um bereits erstellte, sondern um geplante Werte handeln.

1. [Laden Sie die Skripts auf Ihren Computer herunter][sf-aad-ps-script-download].
2. Klicken Sie mit der rechten Maustaste auf die ZIP-Datei, wählen Sie **Eigenschaften** aus, aktivieren Sie das Kontrollkästchen **Entsperren**, und klicken Sie anschließend auf **Übernehmen**.
3. Extrahieren Sie die ZIP-Datei.
4. Führen Sie `SetupApplications.ps1` aus, und geben Sie „TenantId“, „ClusterName“ und „WebApplicationReplyUrl“ als Parameter an. Beispiel: 

```PowerShell
.\SetupApplications.ps1 -TenantId '690ec069-8200-4068-9d01-5aaf188e557a' -ClusterName 'mycluster' -WebApplicationReplyUrl 'https://mycluster.westus.cloudapp.azure.com:19080/Explorer/index.html' -AddResourceAccess
```

> [!NOTE]
> Für nationale Clouds (Azure Government, Azure China, Azure Deutschland) sollten Sie auch den `-Location`-Parameter angeben.

Die Mandanten-ID (TenantId) können Sie mithilfe des PowerShell-Befehls `Get-AzureSubscription` ermitteln. Wenn Sie diesen Befehl ausführen, wird die Mandanten-ID für jedes Abonnement angezeigt.

„ClusterName“ wird als Präfix für durch das Skript erstellte Azure AD-Anwendungen verwendet. Der Wert muss nicht exakt dem tatsächlichen Clusternamen entsprechen. Er dient lediglich zur Vereinfachung der Zuordnung von Azure AD-Artefakten zu dem Service Fabric-Cluster, mit dem sie verwendet werden.

„WebApplicationReplyUrl“ ist der Standardendpunkt, den Azure AD nach Abschluss des Anmeldeprozesses an Ihre Benutzer zurückgibt. Legen Sie diesen Endpunkt auf den Service Fabric Explorer-Endpunkt für Ihren Cluster fest. Dies ist standardmäßig folgender Wert:

https://&lt;Cluster-Domäne&gt;:19080/Explorer

Sie werden aufgefordert, sich bei einem Konto anzumelden, das über Administratorrechte für den Azure AD-Mandanten verfügt. Nach der Anmeldung erstellt das Skript die Webanwendung und die native Anwendung, die Ihren Service Fabric-Cluster darstellen. Wenn Sie sich die Anwendungen des Mandanten im [Azure-Portal][azure-portal] ansehen, sollten zwei neue Einträge angezeigt werden:

   * *Clustername*\_Cluster
   * *Clustername*\_Client

Das Skript gibt den für die Azure Resource Manager-Vorlage erforderlichen JSON-Code aus, wenn Sie im nächsten Abschnitt den Cluster erstellen. Es empfiehlt sich daher, das PowerShell-Fenster geöffnet zu lassen.

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

<a name="assign-roles"></a>

## <a name="assign-users-to-roles"></a>Zuweisen von Benutzern zu Rollen
Nachdem Sie die Anwendungen für Ihren Cluster erstellt haben, müssen Ihre Benutzer den von Service Fabric unterstützten Rollen zugewiesen werden: „read-only“ (schreibgeschützt) und „admin“ (Administrator). Für die Rollenzuweisung können Sie das [Azure-Portal][azure-portal] verwenden.

1. Wählen Sie im Azure-Portal rechts oben Ihren Mandanten aus.

    ![Schaltfläche zur Auswahl des Mandanten][select-tenant-button]
2. Wählen Sie auf der Registerkarte links **Azure Active Directory** aus, und klicken Sie dann auf „Unternehmensanwendungen“.
3. Wählen Sie „Alle Anwendungen“ aus, und suchen Sie dann nach einer Webanwendung mit einem Namen wie `myTestCluster_Cluster`. Wählen Sie die Webanwendung aus.
4. Klicken Sie auf die Registerkarte **Benutzer und Gruppen**.

    ![Registerkarte „Benutzer und Gruppen“][users-and-groups-tab]
5. Klicken Sie auf der neuen Seite auf die Schaltfläche **Benutzer hinzufügen**, wählen Sie einen Benutzer und die zuzuweisende Rolle aus, und klicken Sie dann unten auf der Seite auf die Schaltfläche **Auswählen**.

    ![Seite zum Zuweisen von Benutzern zu Rollen][assign-users-to-roles-page]
6. Klicken Sie unten auf der Seite auf die Schaltfläche **Zuweisen**.

    ![Bestätigung der Zuweisung][assign-users-to-roles-confirm]

> [!NOTE]
> Weitere Informationen zu Rollen in Service Fabric finden Sie unter [Rollenbasierte Zugriffssteuerung für Service Fabric-Clients](service-fabric-cluster-security-roles.md).
>
>

## <a name="troubleshooting-help-in-setting-up-azure-active-directory"></a>Problembehandlung beim Einrichten von Azure Active Directory
Das Einrichten und Verwenden von Azure AD ist unter Umständen recht schwierig. Im Folgenden finden Sie daher einige Hinweise, wie Sie Probleme beheben können.

### <a name="service-fabric-explorer-prompts-you-to-select-a-certificate"></a>Service Fabric Explorer fordert Sie auf, ein Zertifikat auszuwählen.
#### <a name="problem"></a>Problem
Nach erfolgreicher Anmeldung bei Azure AD über Service Fabric Explorer zeigt der Browser wieder die Startseite an und fordert Sie auf, ein Zertifikat auszuwählen.

![SFX-Zertifikatdialogfeld][sfx-select-certificate-dialog]

#### <a name="reason"></a>Grund
Dem Benutzer ist in der Azure AD-Clusteranwendung keine Rolle zugewiesen. Daher tritt bei der Azure AD-Authentifizierung im Service Fabric-Cluster ein Fehler auf. Service Fabric Explorer greift auf die Zertifikatauthentifizierung zurück.

#### <a name="solution"></a>Lösung
Führen Sie die Schritte zum Einrichten von Azure AD aus, und weisen Sie Benutzerrollen zu. Darüber hinaus empfiehlt es sich, „Benutzerzuweisung für den Zugriff auf die App erforderlich“ zu aktivieren, wie dies bei `SetupApplications.ps1` der Fall ist.

### <a name="connection-with-powershell-fails-with-an-error-the-specified-credentials-are-invalid"></a>Beim Herstellen der Verbindung mit PowerShell tritt der Fehler „Die angegebenen Anmeldeinformationen sind ungültig.“ auf.
#### <a name="problem"></a>Problem
Wenn Sie die Verbindung mit dem Cluster über PowerShell und unter Verwendung des Sicherheitsmodus „AzureActiveDirectory“ herstellen, tritt nach erfolgreicher Anmeldung bei Azure AD folgender Verbindungsfehler auf: „Die angegebenen Anmeldeinformationen sind ungültig.“

#### <a name="solution"></a>Lösung
Siehe vorherige Lösung.

### <a name="service-fabric-explorer-returns-a-failure-when-you-sign-in-aadsts50011"></a>Service Fabric Explorer gibt bei der Anmeldung den Fehler „AADSTS50011“ zurück.
#### <a name="problem"></a>Problem
Wenn Sie versuchen, sich in Service Fabric Explorer bei Azure AD anzumelden, wird der Fehler „AADSTS50011“ mit dem Hinweis zurückgegeben, dass die Antwortadresse &lt;URL&gt; nicht den für die Anwendung &lt;GUID&gt; konfigurierten Antwortadressen entspricht.

![Die SFX-Antwortadresse stimmt nicht überein.][sfx-reply-address-not-match]

#### <a name="reason"></a>Grund
Die Cluster(web)anwendung für Service Fabric Explorer versucht, sich bei Azure AD zu authentifizieren, und gibt im Rahmen der Anforderung die Rückgabe-URL für die Umleitung an. Die URL ist jedoch nicht in der Liste **ANTWORT-URL** der Azure AD-Anwendung enthalten.

#### <a name="solution"></a>Lösung
Klicken Sie auf der AAD-Seite auf „App-Registrierungen“, wählen Sie Ihre Clusteranwendung aus, und klicken Sie dann auf die Schaltfläche **Antwort-URLs**. Fügen Sie auf der Seite „Antwort-URLs“ die URL von Service Fabric Explorer zur Liste hinzu, oder ersetzen Sie eines der Elemente in der Liste. Speichern Sie anschließend Ihre Änderung.

![Antwort-URL für Webanwendung][web-application-reply-url]

### <a name="connect-the-cluster-by-using-azure-ad-authentication-via-powershell"></a>Herstellen der Clusterverbindung unter Verwendung der Azure AD-Authentifizierung über PowerShell
Verwenden Sie das folgende PowerShell-Befehlsbeispiel, um eine Service Fabric-Clusterverbindung herzustellen:

```PowerShell
Connect-ServiceFabricCluster -ConnectionEndpoint <endpoint> -KeepAliveIntervalInSec 10 -AzureActiveDirectory -ServerCertThumbprint <thumbprint>
```

Weitere Informationen finden Sie unter [Connect-ServiceFabricCluster](https://docs.microsoft.com/powershell/module/servicefabric/connect-servicefabriccluster).

### <a name="can-i-reuse-the-same-azure-ad-tenant-in-multiple-clusters"></a>Kann ich den gleichen Azure AD-Mandanten in mehreren Clustern verwenden?
Ja. Denken Sie jedoch daran, die URL von Service Fabric Explorer Ihrer Cluster(web)anwendung hinzuzufügen. Andernfalls funktioniert Service Fabric Explorer nicht.

### <a name="why-do-i-still-need-a-server-certificate-while-azure-ad-is-enabled"></a>Warum benötige ich ein Serverzertifikat, auch wenn Azure AD aktiviert ist?
„FabricClient“ und „FabricGateway“ führen eine wechselseitige Authentifizierung durch. Bei der Azure AD-Authentifizierung stellt die Azure AD-Integration die Identität des Clients für den Server bereit, und das Serverzertifikat dient zum Überprüfen der Serveridentität. Weitere Informationen zu Service Fabric-Zertifikaten finden Sie unter [X.509-Zertifikate und Service Fabric][x509-certificates-and-service-fabric].

## <a name="next-steps"></a>Nächste Schritte
Nach dem Einrichten von Azure Active Directory-Anwendungen und dem Festlegen von Rollen für Benutzer [konfigurieren Sie einen Cluster und stellen ihn bereit](service-fabric-cluster-creation-via-arm.md).


<!-- Links -->
[azure-CLI]:https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest
[azure-portal]: https://portal.azure.com/
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[active-directory-howto-tenant]:../active-directory/develop/quickstart-create-new-tenant.md
[service-fabric-visualizing-your-cluster]: service-fabric-visualizing-your-cluster.md
[service-fabric-manage-application-in-visual-studio]: service-fabric-manage-application-in-visual-studio.md
[sf-aad-ps-script-download]:http://servicefabricsdkstorage.blob.core.windows.net/publicrelease/MicrosoftAzureServiceFabric-AADHelpers.zip
[x509-certificates-and-service-fabric]: service-fabric-cluster-security.md#x509-certificates-and-service-fabric

<!-- Images -->
[select-tenant-button]: ./media/service-fabric-cluster-creation-setup-aad/select-tenant-button.png
[users-and-groups-tab]: ./media/service-fabric-cluster-creation-setup-aad/users-and-groups-tab.png
[assign-users-to-roles-page]: ./media/service-fabric-cluster-creation-setup-aad/assign-users-to-roles-page.png
[assign-users-to-roles-confirm]: ./media/service-fabric-cluster-creation-setup-aad/assign-users-to-roles-confirm.png
[sfx-select-certificate-dialog]: ./media/service-fabric-cluster-creation-setup-aad/sfx-select-certificate-dialog.png
[sfx-reply-address-not-match]: ./media/service-fabric-cluster-creation-setup-aad/sfx-reply-address-not-match.png
[web-application-reply-url]: ./media/service-fabric-cluster-creation-setup-aad/web-application-reply-url.png
