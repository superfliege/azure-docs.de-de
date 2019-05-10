---
title: 'Tutorial: Erstellen eines Azure Red Hat OpenShift-Clusters | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie einen Microsoft Azure Red Hat OpenShift-Cluster mithilfe der Azure-Befehlszeilenschnittstelle erstellen
services: container-service
author: TylerMSFT
ms.author: twhitney
manager: jeconnoc
ms.topic: tutorial
ms.service: openshift
ms.date: 05/06/2019
ms.openlocfilehash: 5bc71a2d0f29fed163fb5c93ebd27df7f66a1325
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65079475"
---
# <a name="tutorial-create-an-azure-red-hat-openshift-cluster"></a>Tutorial: Erstellen eines Azure Red Hat OpenShift-Clusters

Dieses Tutorial ist der erste Teil einer Serie. Sie erfahren, wie Sie einen Microsoft Azure Red Hat OpenShift-Cluster mithilfe der Azure-Befehlszeilenschnittstelle erstellen, skalieren und anschließend löschen, um die Ressourcen zu bereinigen.

Im ersten Teil der Serie lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines Azure Red Hat OpenShift-Clusters

In dieser Tutorialserie lernen Sie Folgendes:
> [!div class="checklist"]
> * Erstellen eines Azure Red Hat OpenShift-Clusters
> * [Skalieren eines Azure Red Hat OpenShift-Clusters](tutorial-scale-cluster.md)
> * [Löschen eines Azure Red Hat OpenShift-Clusters](tutorial-delete-cluster.md)

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit diesem Tutorial beginnen können, müssen Sie Folgendes tun:

Vergewissern Sie sich, dass [Ihre Entwicklungsumgebung eingerichtet](howto-setup-environment.md) ist. Dies umfasst:
- Installieren der neuesten Befehlszeilenschnittstelle
- Erstellen eines Mandanten
- Erstellen eines Azure-Anwendungsobjekts
- Erstellen eines Active Directory-Benutzers, der für die Anmeldung bei Apps verwendet wird, die auf dem Cluster ausgeführt werden.

## <a name="step-1-sign-in-to-azure"></a>Schritt 1: Anmelden bei Azure

Wenn Sie die Azure-Befehlszeilenschnittstelle lokal ausführen, öffnen Sie eine Bash-Befehlsshell, und führen Sie `az login` aus, um sich bei Azure anzumelden.

```bash
az login
```

 Wenn Sie Zugriff auf mehrere Abonnements haben, führen Sie `az account set -s {subscription ID}` aus, und ersetzen Sie `{subscription ID}` durch das zu verwendende Abonnement.

## <a name="step-2-create-an-azure-red-hat-openshift-cluster"></a>Schritt 2: Erstellen eines Azure Red Hat OpenShift-Clusters

Legen Sie in Ihrem Bash-Befehlsfenster die folgenden Variablen fest:

> [!IMPORTANT]
> Der Name Ihres Clusters darf nur aus Kleinbuchstaben bestehen, andernfalls tritt bei der Clustererstellung ein Fehler auf.

```bash
CLUSTER_NAME=<cluster name in lowercase>
```

 Verwenden Sie den gleichen Namen für den Cluster, den Sie in Schritt 6 von [Erstellen einer neuen App-Registrierung](howto-aad-app-configuration.md#create-a-new-app-registration) gewählt haben.

```bash
LOCATION=<location>
```

Wählen Sie einen Speicherort zum Erstellen des Clusters aus. Eine Liste der Azure-Regionen, die OpenShift in Azure unterstützen, finden Sie unter [Unterstützte Regionen](supported-resources.md#azure-regions). Beispiel: `LOCATION=eastus`.

Legen Sie `FQDN` auf den vollqualifizierten Namen Ihres Clusters fest. Dieser Name setzt sich aus dem Clusternamen, dem Speicherort und `.cloudapp.azure.com` (am Ende angefügt) zusammen. Es handelt sich um den gleichen Wert wie die Anmelde-URL, die Sie in Schritt 6 von [Erstellen einer neuen App-Registrierung](howto-aad-app-configuration.md#create-a-new-app-registration) erstellt haben. Beispiel:   

```bash
FQDN=$CLUSTER_NAME.$LOCATION.cloudapp.azure.com
```

Legen Sie `APPID` auf den Wert fest, den Sie in Schritt 9 von [Erstellen einer neuen App-Registrierung](howto-aad-app-configuration.md#create-a-new-app-registration) gespeichert haben.  

```bash
APPID=<app ID value>
```

Legen Sie `SECRET` auf den Wert fest, den Sie in Schritt 6 von [Erstellen eines geheimen Clientschlüssels](howto-aad-app-configuration.md#create-a-client-secret) gespeichert haben.  

```bash
SECRET=<secret value>
```

Legen Sie `TENANT` auf den Wert der Mandanten-ID fest, den Sie in Schritt 7 von [Erstellen eines neuen Mandanten](howto-create-tenant.md#create-a-new-azure-ad-tenant) gespeichert haben.  

```bash
TENANT=<tenant ID>
```

Erstellen Sie die Ressourcengruppe für den Cluster. Führen Sie in der Bash-Shell den folgenden Befehl aus, den Sie oben zum Definieren der Variablen verwendet haben:

```bash
az group create --name $CLUSTER_NAME --location $LOCATION
```

### <a name="optional-connect-the-clusters-virtual-network-to-an-existing-virtual-network"></a>Optional: Verbinden des virtuellen Netzwerks des Clusters mit einem vorhandenen virtuellen Netzwerk

Wenn Sie keine Verbindung zwischen dem virtuellen Netzwerk (VNET) des Clusters, den Sie erstellen, und einem vorhandenen VNET herstellen müssen, überspringen Sie diesen Schritt.

Rufen Sie zuerst die ID des vorhandenen VNETs ab. Die ID weist diese Form auf: `/subscriptions/{subscription id}/resourceGroups/{resource group of VNET}/providers/Microsoft.Network/virtualNetworks/{VNET name}`.

Wenn Sie den Netzwerknamen oder die Ressourcengruppe des vorhandenen VNETs nicht kennen, wechseln Sie zum Blatt [Virtuelle Netzwerke](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Network%2FvirtualNetworks), und klicken Sie auf Ihr virtuelles Netzwerk. Die Seite mit den virtuellen Netzwerken wird angezeigt und listet den Namen des Netzwerks und die Ressourcengruppe auf, zu der es gehört.

Definieren Sie eine VNET_ID-Variable mithilfe des folgenden CLI-Befehls in einer BASH-Shell:

```bash
VNET_ID=$(az network vnet show -n {VNET name} -g {VNET resource group} --query id -o tsv)
```

Beispiel: `VNET_ID=$(az network vnet show -n MyVirtualNetwork -g MyResourceGroup --query id -o tsv`

### <a name="create-the-cluster"></a>Erstellen des Clusters

Sie sind jetzt bereit, einen Cluster zu erstellen.

 Wenn Sie das virtuelle Netzwerk des Clusters nicht mit einem vorhandenen virtuellen Netzwerk verbinden, lassen Sie den schließenden Parameter `--vnet-peer-id $VNET_ID` im folgenden Beispiel fort.

```bash
az openshift create --resource-group $CLUSTER_NAME --name $CLUSTER_NAME -l $LOCATION --fqdn $FQDN --aad-client-app-id $APPID --aad-client-app-secret $SECRET --aad-tenant-id $TENANT --vnet-peer-id $VNET_ID
```

Nach einigen Minuten wird `az openshift create` erfolgreich abgeschlossen und gibt eine JSON-Antwort zurück, die Ihre Clusterdetails enthält.

> [!NOTE]
> Wenn Sie eine Fehlermeldung erhalten, dass der Hostname nicht verfügbar ist, liegt das möglicherweise daran, dass der Name Ihres Clusters nicht eindeutig ist. Versuchen Sie, Ihre ursprüngliche App-Registrierung zu löschen und die Schritte in [Erstellen einer neuen App-Registrierung] (howto-aad-app-configuration.md#create-a-new-app-registration) mit einem anderen Clusternamen erneut auszuführen (wobei Sie den abschließenden Schritt des Erstellens eines neuen Benutzers auslassen, da Sie bereits einen erstellt haben).

## <a name="step-3-sign-in-to-the-openshift-console"></a>Schritt 3: Anmelden bei der OpenShift-Konsole

Sie sind jetzt bereit, sich bei der OpenShift-Konsole für Ihren neuen Cluster anzumelden. Die [OpenShift-Webkonsole](https://docs.openshift.com/dedicated/architecture/infrastructure_components/web_console.html) ermöglicht Ihnen das Visualisieren, Durchsuchen und Verwalten der Inhalte Ihrer OpenShift-Projekte.

Sie melden sich als [neuer Azure AD-Benutzer](howto-aad-app-configuration.md#create-a-new-active-directory-user) an – den hatten Sie zum Testen erstellt. Dazu benötigen Sie eine neue Browserinstanz, in der die Identität, die Sie normalerweise für die Anmeldung beim Azure-Portal verwenden, nicht gecacht ist.

1. Öffnen Sie ein *Incognito*-Fenster (Chrome) oder ein *InPrivate*-Fenster (Microsoft Edge).
2. Navigieren Sie zu der Anmelde-URL, die Sie in Schritt 6 von [Erstellen einer neuen App-Registrierung](howto-aad-app-configuration.md#create-a-new-app-registration) erstellt haben. Zum Beispiel, https://constoso.eastus.cloudapp.azure.com

> [!NOTE]
> Die OpenShift-Konsole verwendet ein selbstsigniertes Zertifikat.
> Wenn Sie im Browser dazu aufgefordert werden, umgehen Sie die Warnung, und akzeptieren Sie das „nicht vertrauenswürdige“ Zertifikat.

Melden Sie sich mit dem Benutzernamen und dem Kennwort an, die Sie in [Erstellen eines neuen Active Directory-Benutzers](howto-aad-app-configuration.md#create-a-new-active-directory-user) erstellt haben. Wenn das Dialogfeld **Angeforderte Berechtigungen** angezeigt wird, wählen Sie **Zustimmung im Namen Ihrer Organisation** und dann **Akzeptieren** aus.

Sie werden jetzt bei der Clusterkonsole angemeldet.

[Screenshot der OpenShift-Clusterkonsole](./media/aro-console.png)

 Mehr über das [Verwenden der OpenShift-Konsole](https://docs.openshift.com/dedicated/getting_started/developers_console.html) zum Erstellen von Images können Sie in der [Red Hat OpenShift](https://docs.openshift.com/dedicated/welcome/index.html)-Dokumentation erfahren.

## <a name="step-4-install-the-openshift-cli"></a>Schritt 4: Installieren der OpenShift-CLI

Die [OpenShift-Befehlszeilenschnittstelle](https://docs.openshift.com/dedicated/cli_reference/get_started_cli.html) (oder *OC Tools*) stellt Befehle zum Verwalten Ihrer Anwendungen und allgemeineren Hilfsprogramme für die Interaktion mit den verschiedenen Komponenten des OpenShift-Clusters bereit.

Klicken Sie in der OpenShift-Konsole auf das Fragezeichen in der oberen rechten Ecke neben Ihrem Anmeldenamen, und wählen Sie **Befehlszeilentools** aus.  Folgen Sie dem Link **Neueste Version**, um die unterstützte oc-Befehlszeilenschnittstelle für Linux, MacOS oder Windows herunterzuladen.

> [!NOTE]
> Wenn Sie das Fragezeichensymbol in der oberen rechten Ecke nicht sehen, wählen Sie in der oberen linken Dropdownliste *Dienstkatalog* oder *Anwendungskonsole* aus.
>
> Alternativ können Sie die [oc-CLI direkt herunterladen](https://www.okd.io/download.html).

Auf der Seite **Befehlszeilentools** steht ein Befehl in der Form `oc login https://<your cluster name>.<azure region>.cloudapp.azure.com --token=<token value>` zur Verfügung.  Klicken Sie auf die Schaltfläche *In die Zwischenablage kopieren*, um diesen Befehl zu kopieren.  In einem Terminalfenster können Sie [Ihren Pfad festlegen](https://docs.okd.io/latest/cli_reference/get_started_cli.html#installing-the-cli), um Ihre lokale Installation der oc-Tools einzubinden. Melden Sie sich anschließend mithilfe des kopierten oc-CLI-Befehls beim Cluster an.

Wenn Sie den Tokenwert nicht mithilfe der oben beschriebenen Schritte abrufen konnten, rufen Sie den Tokenwert hier ab: `https://<your cluster name>.<azure region>.cloudapp.azure.com/oauth/token/request`.

## <a name="next-steps"></a>Nächste Schritte

In diesem Teil des Tutorials haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines Azure Red Hat OpenShift-Clusters

Fahren Sie mit dem nächsten Tutorial fort:
> [!div class="nextstepaction"]
> [Skalieren eines Azure Red Hat OpenShift-Clusters](tutorial-scale-cluster.md)