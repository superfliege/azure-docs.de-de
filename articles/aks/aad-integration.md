---
title: Integrieren von Azure Active Directory in Azure Kubernetes Service
description: Erstellen von Azure Active Directory-fähigen AKS-Clustern (Azure Kubernetes Service)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 08/09/2018
ms.author: iainfou
ms.openlocfilehash: 0cf83180647c142c9db2a1229674de96fec6a6bb
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58087532"
---
# <a name="integrate-azure-active-directory-with-azure-kubernetes-service"></a>Integrieren von Azure Active Directory in Azure Kubernetes Service

Azure Kubernetes Service (AKS) kann für die Verwendung von Azure Active Directory (AD) für die Benutzerauthentifizierung konfiguriert werden. In dieser Konfiguration können Sie sich mit Ihrem Azure Active Directory-Authentifizierungstoken bei einem AKS-Cluster anmelden. Zusätzlich können Clusteradministratoren die rollenbasierte Zugriffssteuerung (RBAC) von Kubernetes basierend auf einer Benutzeridentität oder einer Verzeichnisgruppenzugehörigkeit konfigurieren.

Dieser Artikel zeigt Ihnen, wie Sie die Voraussetzungen für AKS und Azure AD bereitstellen, dann einen Azure AD-fähigen Cluster bereitstellen und eine einfache RBAC-Rolle im AKS-Cluster erstellen.

Es gelten die folgenden Einschränkungen:

- Azure AD kann nur aktiviert werden, wenn Sie einen neuen, RBAC-fähigen Cluster erstellen. Es ist nicht möglich, Azure AD für einen vorhandenen AKS-Cluster zu aktivieren.
- *Gast*benutzer in Azure AD (z.B. bei Verwendung einer Verbundanmeldung aus einem anderen Verzeichnis) werden nicht unterstützt.

## <a name="authentication-details"></a>Authentifizierungsdetails

Die Azure AD-Authentifizierung wird für AKS-Cluster mit OpenID Connect bereitgestellt. OpenID Connect ist eine Identitätsebene, die auf dem OAuth 2.0-Protokoll aufbaut. Weitere Informationen zu OpenID Connect finden Sie in der [OpenID Connect-Dokumentation][open-id-connect].

Innerhalb des Kubernetes-Clusters werden Authentifizierungstoken mithilfe der Webhooktokenauthentifizierung überprüft. Die Webhooktokenauthentifizierung wird als Teil des AKS-Clusters konfiguriert und verwaltet. Weitere Informationen zur Webhooktokenauthentifizierung finden Sie in der [Dokumentation zur Webhookauthentifizierung][kubernetes-webhook].

> [!NOTE]
> Bei der Konfiguration von Azure AD für die AKS-Authentifizierung werden zwei Azure AD-Anwendungen konfiguriert. Dieser Vorgang muss von einem Azure-Mandantenadministrator ausgeführt werden.

## <a name="create-server-application"></a>Erstellen einer Serveranwendung

Die erste Azure AD-Anwendung wird verwendet, um eine Azure AD-Gruppenmitgliedschaft zu erhalten.

1. Wählen Sie **Azure Active Directory** > **App-Registrierungen** > **Registrierung einer neuen Anwendung** aus.

   Geben Sie der Anwendung einen Namen, wählen Sie **Web-App / API** für den Anwendungstyp aus, und geben Sie einen beliebigen Wert im URI-Format für die **Anmelde-URL** ein. Wählen Sie **Erstellen** aus, wenn Sie fertig sind.

   ![Erstellen der Azure AD-Registrierung](media/aad-integration/app-registration.png)

2. Wählen Sie **Manifest** aus, und setzen Sie den Wert von `groupMembershipClaims` auf `"All"`.

   Speichern Sie die Änderungen, wenn Sie fertig sein.

   ![Aktualisieren der Gruppenmitgliedschaft auf „All“](media/aad-integration/edit-manifest.png)

3. Wechseln Sie zurück zur Azure AD-Anwendung, und wählen Sie **Einstellungen** > **Schlüssel** aus.

   Fügen Sie eine Schlüsselbeschreibung hinzu, wählen Sie eine Ablaufzeit aus, und klicken Sie auf **Speichern**. Notieren Sie sich den Schlüsselwert. Beim Bereitstellen eines Azure AD-fähigen AKS-Clusters wird dieser Wert als `Server application secret` bezeichnet.

   ![Abrufen des privaten Schlüssels der Anwendung](media/aad-integration/application-key.png)

4. Wechseln Sie zurück zur Azure AD-Anwendung, und wählen Sie **Einstellungen** > **Erforderliche Berechtigungen** > **Hinzufügen** > **API auswählen** > **Microsoft Graph** > **Auswählen**.

   ![Auswählen der Graph-API](media/aad-integration/graph-api.png)

5. Aktivieren Sie unter **ANWENDUNGSBERECHTIGUNGEN** die Option **Verzeichnisdaten lesen**.

   ![Festlegen von Anwendungsberechtigungen für Graph](media/aad-integration/read-directory.png)

6. Aktivieren Sie unter **DELEGIERTE BERECHTIGUNGEN** die Optionen **Anmelden und Benutzerprofil lesen** und **Verzeichnisdaten lesen**. Speichern Sie die Änderungen, wenn Sie fertig sein.

   ![Festlegen von Anwendungsberechtigungen für Graph](media/aad-integration/delegated-permissions.png)

   Wählen Sie **Fertig**aus.

7. Wählen Sie in der Liste der APIs *Microsoft Graph* aus, und wählen Sie dann **Berechtigungen erteilen** aus. Dieser Schritt schlägt fehl, wenn das aktuelle Konto keinem Mandantenadministrator gehört.

   ![Festlegen von Anwendungsberechtigungen für Graph](media/aad-integration/grant-permissions.png)

   Wenn die Berechtigungen erfolgreich gewährt wurden, wird die folgende Benachrichtigung im Portal angezeigt:

   ![Benachrichtigung über erfolgreich erteilte Berechtigungen](media/aad-integration/permissions-granted.png)

8. Wechseln Sie zurück zur neuen Anwendung, und notieren Sie sich die **Anwendungs-ID**. Beim Bereitstellen eines Azure AD-fähigen AKS-Clusters wird dieser Wert als `Server application ID` bezeichnet.

   ![Abrufen der Anwendungs-ID](media/aad-integration/application-id.png)

## <a name="create-client-application"></a>Erstellen einer Clientanwendung

Die zweite Azure AD-Anwendung wird beim Anmelden mit der Kubernetes-CLI (kubectl) verwendet.

1. Wählen Sie **Azure Active Directory** > **App-Registrierungen** > **Registrierung einer neuen Anwendung** aus.

   Geben Sie der Anwendung einen Namen, wählen Sie **Nativ** für den Anwendungstyp aus, und geben Sie einen beliebigen Wert im URI-Format für **Umleitungs-URI** ein. Wählen Sie **Erstellen** aus, wenn Sie fertig sind.

   ![Erstellen einer AAD-Registrierung](media/aad-integration/app-registration-client.png)

2. Wählen Sie in der Azure AD-Anwendung **Einstellungen** > **Erforderliche Berechtigungen** > **Hinzufügen** > **API auswählen** aus, und suchen Sie nach dem Namen der im letzten Schritt dieses Dokuments erstellten Serveranwendung.

   ![Konfigurieren der Anwendungsberechtigungen](media/aad-integration/select-api.png)

3. Aktivieren Sie die Anwendung in der Liste, und klicken Sie auf **Auswählen**.

   ![Auswählen des Endpunkts für die AKS-AAD-Serveranwendung](media/aad-integration/select-server-app.png)

   Wählen Sie **Fertig**aus.

4. Wählen Sie Ihre Server-API aus der Liste aus, und wählen Sie dann **Berechtigungen erteilen** aus:

   ![Erteilen von Berechtigungen](media/aad-integration/grant-permissions-client.png)

5. Wechseln Sie zurück zur AD-Anwendung, und notieren Sie sich die **Anwendungs-ID**. Beim Bereitstellen eines Azure AD-fähigen AKS-Clusters wird dieser Wert als `Client application ID` bezeichnet.

   ![Abrufen der Anwendungs-ID](media/aad-integration/application-id-client.png)

## <a name="get-tenant-id"></a>Abrufen der Mandanten-ID

Abschließend rufen Sie die ID von Ihrem Azure-Mandanten ab. Dieser Wert wird auch bei der Bereitstellung des AKS-Clusters verwendet.

Wählen Sie im Azure-Portal **Azure Active Directory** > **Eigenschaften** aus, und notieren Sie die **Verzeichnis-ID**. Beim Bereitstellen eines Azure AD-fähigen AKS-Clusters wird dieser Wert als `Tenant ID` bezeichnet.

![Abrufen der Azure-Mandanten-ID](media/aad-integration/tenant-id.png)

## <a name="deploy-cluster"></a>Bereitstellen eines Clusters

Verwenden Sie zum Erstellen einer Ressourcengruppe für den AKS-Cluster den Befehl [az group create][az-group-create].

```azurecli
az group create --name myResourceGroup --location eastus
```

Stellen Sie den Cluster mit dem Befehl [az aks create][az-aks-create] bereit. Ersetzen Sie die Werte im Beispielbefehl unten durch die Werte, die beim Erstellen der Azure AD-Anwendungen gesammelt wurden.

```azurecli
az aks create \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --generate-ssh-keys \
  --aad-server-app-id b1536b67-29ab-4b63-b60f-9444d0c15df1 \
  --aad-server-app-secret wHYomLe2i1mHR2B3/d4sFrooHwADZccKwfoQwK2QHg= \
  --aad-client-app-id 8aaf8bd5-1bdd-4822-99ad-02bfaa63eea7 \
  --aad-tenant-id 72f988bf-0000-0000-0000-2d7cd011db47
```

## <a name="create-rbac-binding"></a>Erstellen einer RBAC-Bindung

Bevor ein Azure Active Directory-Konto mit dem AKS-Cluster verwendet werden kann, muss eine Rollenbindung oder eine Clusterrollenbindung erstellt werden. *Rollen* definieren die zu erteilenden Berechtigungen, und *Bindungen* wenden diese auf die gewünschten Benutzer an. Diese Zuweisungen können auf einen bestimmten Namespace oder im gesamten Cluster angewendet werden. Weitere Informationen finden Sie unter [Verwenden der RBAC-Autorisierung][rbac-authorization].

Verwenden Sie zunächst den Befehl [az aks get-credentials][az-aks-get-credentials] mit dem Argument `--admin`, um sich beim Cluster mit Administratorzugriff anzumelden.

```azurecli
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster --admin
```

Verwenden Sie anschließend das folgende Manifest, um ein ClusterRoleBinding-Element für ein Azure AD-Konto zu erstellen. In diesem Beispiel wird dem Konto Vollzugriff auf alle Namespaces des Clusters gewährt. Erstellen Sie eine Datei, z. B. *rbac-aad-user.yaml*, und fügen Sie den folgenden Inhalt ein. Aktualisieren Sie den Benutzernamen mit einem Namen aus Ihrem Azure AD-Mandanten:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: contoso-cluster-admins
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: User
  name: "user@contoso.com"
```

Wenden Sie die Bindung wie im folgenden Beispiel gezeigt mit dem Befehl [kubectl apply][kubectl-apply] an:

```console
kubectl apply -f rbac-aad-user.yaml
```

Für alle Mitglieder einer Azure AD-Gruppe kann auch eine Rollenbindung erstellt werden. Azure AD-Gruppen werden wie im folgenden Beispiel gezeigt anhand der Gruppenobjekt-ID angegeben. Erstellen Sie eine Datei, z. B. *rbac-aad-group.yaml*, und fügen Sie den folgenden Inhalt ein. Aktualisieren Sie den Benutzernamen mit einem Namen aus Ihrem Azure AD-Mandanten:

 ```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: contoso-cluster-admins
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- apiGroup: rbac.authorization.k8s.io
   kind: Group
   name: "894656e1-39f8-4bfe-b16a-510f61af6f41"
```

Wenden Sie die Bindung wie im folgenden Beispiel gezeigt mit dem Befehl [kubectl apply][kubectl-apply] an:

```console
kubectl apply -f rbac-aad-group.yaml
```

Weitere Informationen zum Sichern eines Kubernetes-Clusters mit RBAC finden Sie in der Dokumentation zur [Verwendung der RBAC-Autorisierung][rbac-authorization].

## <a name="access-cluster-with-azure-ad"></a>Zugriff auf Cluster mit Azure AD

Als Nächstes pullen Sie den Kontext für den Nicht-Administratorbenutzer mit dem Befehl [az aks get-credentials][az-aks-get-credentials].

```azurecli
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Nach dem Ausführen eines beliebigen kubectl-Befehls werden Sie aufgefordert, sich gegenüber Azure zu authentifizieren. Befolgen Sie die Anweisungen auf dem Bildschirm.

```console
$ kubectl get nodes

To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code BUJHWDGNL to authenticate.

NAME                       STATUS    ROLES     AGE       VERSION
aks-nodepool1-79590246-0   Ready     agent     1h        v1.9.9
aks-nodepool1-79590246-1   Ready     agent     1h        v1.9.9
aks-nodepool1-79590246-2   Ready     agent     1h        v1.9.9
```

Anschließend wird das Authentifizierungstoken zwischengespeichert. Erst, wenn das Token abgelaufen ist oder die Kubernetes-Konfigurationsdatei neu erstellt wurde, werden Sie aufgefordert, sich anzumelden.

Wenn nach erfolgreicher Anmeldung eine Fehlermeldung bezüglich der Autorisierung angezeigt wird, überprüfen Sie Folgendes:
1. Der Benutzer, den Sie anmelden, ist in der Azure AD-Instanz kein Gastbenutzer (dies ist häufig der Fall, wenn Sie eine Verbundanmeldung aus einem anderen Verzeichnis verwenden).
2. Der Benutzer ist kein Mitglied der über 200 Gruppen.

```console
error: You must be logged in to the server (Unauthorized)
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Sichern von Kubernetes-Clustern mit RBAC finden Sie in der Dokumentation zur [Verwendung der RBAC-Autorisierung][rbac-authorization].

<!-- LINKS - external -->
[kubernetes-webhook]:https://kubernetes.io/docs/reference/access-authn-authz/authentication/#webhook-token-authentication
[rbac-authorization]: https://kubernetes.io/docs/reference/access-authn-authz/rbac/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply

<!-- LINKS - internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-group-create]: /cli/azure/group#az-group-create
[open-id-connect]:../active-directory/develop/v1-protocols-openid-connect-code.md
