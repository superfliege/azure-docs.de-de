---
title: Erstellen von Cloud Foundry in Azure
description: Es wird beschrieben, wie Sie die Parameter einrichten, die zum Bereitstellen eines PCF-Clusters (Pivotal Cloud Foundry) in Azure benötigt werden.
services: Cloud Foundry
documentationcenter: CloudFoundry
author: ruyakubu
manager: brunoborges
editor: ruyakubu
ms.assetid: ''
ms.author: ruyakubu
ms.date: 09/13/2018
ms.devlang: ''
ms.service: Cloud Foundry
ms.tgt_pltfrm: multiple
ms.topic: tutorial
ms.workload: web
ms.openlocfilehash: a0a3379a8a2579080d9b686917395feec9cf8f3d
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/03/2018
ms.locfileid: "48250577"
---
# <a name="create-cloud-foundry-on-azure"></a>Erstellen von Cloud Foundry in Azure

Dieses Tutorial enthält die Schritte zum schnellen Erstellen und Generieren von Parametern, die zum Bereitstellen eines PCF-Clusters (Pivotal Cloud Foundry) in Azure benötigt werden.  Die Pivotal Cloud Foundry-Lösung finden Sie, indem Sie auf dem Azure [Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/pivotal.pivotal-cloud-foundry) eine Suche durchführen.

![Alternativer Bildtext](media/deploy/pcf-marketplace.png "Suchen nach Pivotal Cloud Foundry in Azure")


## <a name="generate-an-ssh-public-key"></a>Generieren eines öffentlichen SSH-Schlüssels

Es gibt mehrere Möglichkeiten, unter Windows, Mac oder Linux einen öffentlichen SSH-Schlüssel zu generieren.

```Bash
ssh-keygen -t rsa -b 2048
```
- Klicken Sie hier, um eine [Anleitung]( https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows) für Ihre Umgebung anzuzeigen.

## <a name="create-a-service-principal"></a>Erstellen eines Dienstprinzipals

> [!NOTE]
>
> Zum Erstellen eines Dienstprinzipals ist die Berechtigung „Kontobesitzer“ erforderlich.  Darüber hinaus können Sie ein Skript für die Automatisierung der Dienstprinzipalerstellung schreiben. Sie können beispielsweise den Azure CLI-Befehl [az ad sp create-for-rbac](https://docs.microsoft.com/cli/azure/ad/sp?view=azure-cli-latest) verwenden.

1. Melden Sie sich bei Ihrem Azure-Konto an.

    `az login`

    ![Alternativer Bildtext](media/deploy/az-login-output.png "Azure CLI-Anmeldung")
 
    Kopieren Sie den „id“-Wert als **Abonnement-ID** und den Wert von **tenantId** zur späteren Verwendung.

2. Legen Sie Ihr Standardabonnement für diese Konfiguration fest.

    `az account set -s {id}`

3. Erstellen Sie eine AAD-Anwendung für Ihre PCF-Lösung, und geben Sie ein eindeutiges alphanumerisches Kennwort an.  Speichern Sie das Kennwort als **clientSecret** zur späteren Verwendung.

    `az ad app create --display-name "Svc Prinicipal for OpsManager" --password {enter-your-password} --homepage "{enter-your-homepage}" --identifier-uris {enter-your-homepage}`

    Kopieren Sie den Wert von „appId“ in der Ausgabe als **ClientID** zur späteren Verwendung.

    > [!NOTE]
    >
    > Wählen Sie Ihre eigene Anwendungsstartseite und den Bezeichner-URI aus.  Beispiel: http://www.contoso.com.

4. Erstellen Sie einen Dienstprinzipal mit Ihrer neuen „appId“.

    `az ad sp create --id {appId}`

5. Legen Sie als Berechtigung für Ihren Dienstprinzipal die Rolle **Mitwirkender** fest.

    `az role assignment create --assignee “{enter-your-homepage}” --role “Contributor” `

    Sie können auch Folgendes verwenden:

    `az role assignment create --assignee {service-princ-name} --role “Contributor” `

    ![Alternativer Bildtext](media/deploy/svc-princ.png "Rollenzuweisung für Dienstprinzipal")

6. Vergewissern Sie sich, dass Sie sich erfolgreich an Ihrem Dienstprinzipal anmelden können, indem Sie die Elemente „appId“, „password“ und „tenantId“ verwenden.

    `az login --service-principal -u {appId} -p {your-passward}  --tenant {tenantId}`

7. Erstellen Sie eine JSON-Datei im folgenden Format, indem Sie die oben kopierten Werte für **Abonnement-ID**, **tenantId**, **clientID** und **clientSecret** verwenden.  Speichern Sie die Datei.

    ```json
    {
        "subscriptionID": "{enter-your-subscription-Id-here}",
        "tenantID": "{enter-your-tenant-Id-here}",
        "clientID": "{enter-your-app-Id-here}",
        "clientSecret": "{enter-your-key-here}"
    }
    ```

## <a name="get-the-pivotal-network-token"></a>Abrufen des Tokens für das Pivotal-Netzwerk

1. Melden Sie sich an Ihrem Konto für das [Pivotal-Netzwerk](https://network.pivotal.io) an, oder führen Sie die Registrierung dafür durch.
2. Klicken Sie oben rechts auf der Seite auf Ihren Profilnamen, und wählen Sie dann „Profil bearbeiten“.
3. Scrollen Sie zum unteren Rand der Seite, und kopieren Sie den Wert für **LEGACY API TOKEN** (LEGACY-API-TOKEN).  Dies ist der Wert für das **Token für das Pivotal-Netzwerk**, der später verwendet wird.

## <a name="provision-your-cloud-foundry-on-azure"></a>Bereitstellen von Cloud Foundry in Azure

1. Sie verfügen jetzt über alle Parameter für die Bereitstellung Ihres Clusters vom Typ [Pivotal Cloud Foundry in Azure](https://azuremarketplace.microsoft.com/marketplace/apps/pivotal.pivotal-cloud-foundry).
2. Geben Sie die Parameter ein, und erstellen Sie Ihren PCF-Cluster.

## <a name="verify-the-deployment-and-log-into-the-pivotal-ops-manager"></a>Überprüfen der Bereitstellung und Anmelden am Pivotal Operations Manager

1. In Ihrem PCF-Cluster sollte nun ein Bereitstellungsstatus angezeigt werden.

    ![Alternativer Bildtext](media/deploy/deployment.png "Bereitstellungsstatus von Azure")

2. Klicken Sie im Navigationsbereich auf der linken Seite auf den Link **Bereitstellungen**, um die Anmeldeinformationen für Ihren PCF Operations Manager zu erhalten, und klicken Sie auf der nächsten Seite dann auf **Bereitstellungsname**.
3. Klicken Sie im linken Navigationsbereich auf den Link **Ausgaben**, um die URL, den Benutzernamen und das Kennwort für den PCF Operations Manager anzuzeigen.  Der Wert „OPSMAN-FQDN“ ist die URL.
 
    ![Alternativer Bildtext](media/deploy/deploy-outputs.png "Ausgabe der Cloud Foundry-Bereitstellung")
 
4. Navigieren Sie in einem Webbrowser zur URL, und geben Sie die Anmeldeinformationen aus dem vorherigen Schritt ein, um sich anzumelden.

    ![Alternativer Bildtext](media/deploy/pivotal-login.png "Anmeldeseite für Pivotal")
         
    > [!NOTE]
    >
    > Wenn im Internet Explorer-Browser ein Fehler aufgrund der Warnmeldung „Website ist nicht sicher“ auftritt, können Sie auf „Weitere Informationen“ und „Webseite trotzdem laden“ klicken.  Klicken Sie im Firefox-Browser auf „Erweitert“, und fügen Sie die Zertifizierung hinzu, um fortzufahren.

5. Im PCF Operations Manager sollten die bereitgestellten Azure-Instanzen angezeigt werden. Sie können hier jetzt mit dem Bereitstellen und Verwalten Ihrer Anwendungen beginnen!
               
    ![Alternativer Bildtext](media/deploy/ops-mgr.png "Bereitgestellte Azure-Instanz in Pivotal")
 
