---
title: Authentifizieren mit einer Azure-Containerregistrierung
description: "Authentifizierungsoptionen für eine Azure-Containerregistrierung einschließlich direkter und Registrierungsanmeldung von Azure Active Directory-Dienstprinzipalen."
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: mmacy
tags: 
keywords: 
ms.assetid: 128a937a-766a-415b-b9fc-35a6c2f27417
ms.service: container-registry
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/05/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 37514e7b90afe1162aa4bbd2869326a691f75c4e
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2017
---
# <a name="authenticate-with-a-private-docker-container-registry"></a>Authentifizieren mit einer privaten Docker-Containerregistrierung

Es gibt mehrere Möglichkeiten, sich mit einer Azure-Containerregistrierung zu authentifizieren, von denen jede für eine oder mehrere Registrierungsverwendungsszenarien gilt.

Sie können sich direkt mittels einer [individuellen Anmeldung](#individual-login-with-azure-ad) bei einer Registrierung anmelden, und Ihre Anwendungen und Containerorchestratoren können eine unbeaufsichtigte oder „monitorlose“ Authentifizierung mithilfe eines Azure Active Directory-[Dienstprinzipals](#service-principal) (Azure AD) durchführen.

Azure Container Registry unterstützt keine nicht authentifizierten Docker-Vorgänge oder anonymen Zugriff. Für öffentliche Images können Sie [Docker Hub](https://docs.docker.com/docker-hub/) verwenden.

## <a name="individual-login-with-azure-ad"></a>Individuelle Anmeldung bei Azure AD

Authentifizieren Sie sich bei der direkten Arbeit mit der Registrierung wie der Pullübertragung von Images auf Ihre und der Pushübertragung von Images von Ihrer Entwicklungsarbeitsstation mithilfe des [az acr login](/cli/azure/acr?view=azure-cli-latest#az_acr_login) -Befehls in der [Azure CLI](/cli/azure/install-azure-cli):

```azurecli
az acr login --name <acrName>
```

Bei der Anmeldung mit `az acr login` verwendet die CLI das Token, das erstellt wurde, als Sie `az login` ausgeführt haben, zur nahtlosen Authentifizierung Ihrer Sitzung mit Ihrer Registrierung. Sobald Sie sich auf diese Weise angemeldet haben, werden Ihre Anmeldeinformationen zwischengespeichert, und nachfolgende `docker`-Befehle benötigen weder einen Benutzernamen noch das Kennwort. Wenn das Token abgelaufen ist, können Sie es aktualisieren, indem Sie den `az acr login`-Befehl erneut zur Authentifizierung verwenden. Die Verwendung von `az acr login` mit Azure-Identitäten ermöglicht [rollenbasierten Zugriff](../active-directory/role-based-access-control-configure.md).

## <a name="service-principal"></a>Dienstprinzipal

Sie können Ihrer Registrierung einen [Dienstprinzipal](../active-directory/develop/active-directory-application-objects.md) zuweisen, und Ihre Anwendung oder Ihr Dienst kann ihn für die monitorlose Authentifizierung verwenden. Dienstprinzipale ermöglichen [rollenbasierten Zugriff](../active-directory/role-based-access-control-configure.md) auf eine Registrierung, und Sie können einer Registrierung mehrere Dienstprinzipale zuweisen. Mit mehreren Dienstprinzipalen können Sie unterschiedliche Zugriffsberechtigungen für verschiedene Anwendungen definieren.

Folgende Rollen sind verfügbar:

  * **Leser**: Pull
  * **Mitwirkender**: Pull und Push
  * **Besitzer**: Pull, Push und Zuweisen von Rollen an andere Benutzer

Dienstprinzipale ermöglichen auch monitorlose Verbindungen mit einer Registrierung in Pull- und Pushszenarien, z.B.:

  * *Leser*: Containerbereitstellungen aus einer Registrierung für Orchestrierungssysteme, z.B. Kubernetes, DC/OS und Docker Swarm. Sie können auch Pullvorgänge aus Containerregistrierungen in andere Azure-Dienste durchführen, z.B. [AKS](../aks/index.yml), [App Service](../app-service/index.yml), [Batch](../batch/index.md), [Service Fabric](/azure/service-fabric/) und weitere Dienste.

  * *Mitwirkender*: Lösungen für Continuous Integration und Continuous Deployment wie Visual Studio Team Services (VSTS) oder Jenkins, mit denen Containerimages erstellt und per Pushvorgang in eine Registrierung übertragen werden.

> [!TIP]
> Sie können das Kennwort eines Dienstprinzipals neu generieren, indem Sie den Befehl [az ad sp reset-credentials](/cli/azure/ad/sp?view=azure-cli-latest#az_ad_sp_reset_credentials) ausführen.
>

Sie können sich auch direkt mit einem Dienstprinzipal anmelden. Geben Sie die App-ID und das Kennwort des Dienstprinzipals für den Befehl `docker login` ein:

```
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

Nach der Anmeldung speichert Docker die Anmeldeinformationen zwischen, sodass Sie die App-ID nicht im Kopf behalten müssen.

Abhängig von der Docker-Version, die Sie installiert haben, wird möglicherweise eine Sicherheitswarnung angezeigt, die die Verwendung des `--password-stdin`-Parameters empfiehlt. Obwohl in diesem Artikel nicht auf dessen Verwendung eingegangen werden kann, wird empfohlen, diese bewährte Methode anzuwenden. Weitere Informationen finden Sie in der [docker login](https://docs.docker.com/engine/reference/commandline/login/)-Befehlsreferenz.

## <a name="admin-account"></a>Administratorkonto

Jede Containerregistrierung enthält ein Administratorbenutzerkonto, das standardmäßig deaktiviert ist. Sie können den Administratorbenutzer aktivieren und seine Anmeldeinformationen im [Azure-Portal](container-registry-get-started-portal.md#create-a-container-registry) oder mithilfe der Azure CLI verwalten.

> [!IMPORTANT]
> Das Administratorkonto ist dafür ausgelegt, dass ein einzelner Benutzer auf die Registrierung zugreift (hauptsächlich für Testzwecke). Sie sollten die Administratorkonto-Anmeldeinformationen nicht für mehrere Benutzer freigeben. Alle Benutzer, die sich mit dem Administratorkonto authentifizieren, werden als ein einzelner Benutzer mit Push- und Pullzugriff auf die Registrierung angezeigt. Wenn dieses Konto geändert oder deaktiviert wird, wird der Zugriff auf die Registrierung für alle Benutzer deaktiviert, die dessen Anmeldeinformationen verwenden. Für Benutzer und Dienstprinzipale wird für monitorlose Szenarien einzelne Identität empfohlen.
>

Das Administratorkonto erhält zwei Kennwörter, die beide erneut generiert werden können. Die beiden Kennwörter ermöglichen Ihnen, Verbindungen mit der Registrierung aufrechtzuerhalten, indem Sie ein Kennwort verwenden, während Sie das andere Kennwort neu generieren. Wenn das Administratorkonto aktiviert ist, können Sie den Benutzernamen und eines der Kennwörter an den Befehl `docker login` übergeben, um die Standardauthentifizierung für die Registrierung zu erhalten. Beispiel:

```
docker login myregistry.azurecr.io -u myAdminName -p myPassword1
```

Auch hier empfiehlt Docker die Verwendung des `--password-stdin`-Parameters, anstatt es in der Befehlszeile für die erhöhte Sicherheit bereitzustellen. Sie können auch nur Ihren Benutzernamen ohne `-p` angeben und nach Aufforderung Ihr Kennwort eingeben.

Um den Administratorbenutzer für eine vorhandene Registrierung zu aktivieren, können Sie den `--admin-enabled`-Parameter des [az acr update](/cli/azure/acr?view=azure-cli-latest#az_acr_update)-Befehls in der Azure CLI verwenden:

```azurecli
az acr update -n <acrName> --admin-enabled true
```

Sie können den Administratorbenutzer im Azure-Portal aktivieren, indem Sie zu Ihrer Registrierung navigieren, **Zugriffsschlüssel** unter **EINSTELLUNGEN** und dann **Aktivieren** unter **Administratorbenutzer** auswählen.

![Aktivieren der Administratorbenutzeroberfläche im Azure-Portal][auth-portal-01]

## <a name="next-steps"></a>Nächste Schritte

* [Freigeben Ihres ersten Image mit der Azure CLI](container-registry-get-started-azure-cli.md)

<!-- IMAGES -->
[auth-portal-01]: ./media/container-registry-authentication/auth-portal-01.png
