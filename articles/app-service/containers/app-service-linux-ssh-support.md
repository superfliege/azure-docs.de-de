---
title: SSH-Unterstützung für App Service unter Linux – Azure | Microsoft-Dokumentation
description: Erfahren Sie etwas über die Verwendung von SSH mit Azure App Service unter Linux.
keywords: Azure App Service, Web-App, Linux, OSS
services: app-service
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.assetid: 66f9988f-8ffa-414a-9137-3a9b15a5573c
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2017
ms.author: wesmc
ms.custom: seodec18
ms.openlocfilehash: 9a05769b4cfd4bcaca0df9e1af1816d99f78bc62
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/09/2019
ms.locfileid: "55984466"
---
# <a name="ssh-support-for-azure-app-service-on-linux"></a>SSH-Unterstützung bei Azure App Service unter Linux

[Secure Shell (SSH)](https://wikipedia.org/wiki/Secure_Shell) wird häufig für die Remoteausführung von Verwaltungsbefehlen über ein Befehlszeilenterminal verwendet. App Service unter Linux unterstützt SSH in den App-Container für jedes der integrierten Docker-Images, die für den Laufzeitstapel neuer Web-Apps verwendet werden. 

![Laufzeitstapel](./media/app-service-linux-ssh-support/app-service-linux-runtime-stack.png)

Für benutzerdefinierte Docker-Images, indem ein SSH-Server in Ihrem benutzerdefinierten Image konfiguriert wird.

Sie können auch direkt von Ihrem lokalen Entwicklungscomputer per SSH und SFTP eine Verbindung herstellen.

## <a name="open-ssh-session-in-browser"></a>Öffnen einer SSH-Sitzung im Browser

Zum Herstellen einer SSH-Clientverbindung mit Ihrem Container sollte Ihre App ausgeführt werden.

Fügen Sie die folgende URL in Ihren Browser ein, und ersetzen Sie \<app_name> durch den Namen Ihrer App:

```
https://<app_name>.scm.azurewebsites.net/webssh/host
```

Wenn Sie nicht bereits authentifiziert sind, müssen Sie sich mit Ihrem Azure-Abonnement für die Verbindung authentifizieren. Nach der Authentifizierung wird im Browser eine Shell angezeigt, über die Sie Befehle innerhalb Ihres Containers ausführen können.

![SSH-Verbindung](./media/app-service-linux-ssh-support/app-service-linux-ssh-connection.png)

## <a name="use-ssh-support-with-custom-docker-images"></a>Verwenden der SSH-Unterstützung mit benutzerdefinierten Docker-Images

Damit ein benutzerdefiniertes Docker-Image die SSH-Kommunikation zwischen dem Container und dem Client unterstützt, führen Sie im Azure-Portal die folgenden Schritte für Ihr Docker-Image aus.

Diese Schritte werden im Azure App Service-Repository [als Beispiel](https://github.com/Azure-App-Service/node/blob/master/6.9.3/) gezeigt.

1. Fügen Sie die `openssh-server`-Installation in die [`RUN`-Anweisung](https://docs.docker.com/engine/reference/builder/#run) in der Dockerfile-Datei für Ihr Image ein, und legen Sie als Kennwort für das Root-Konto `"Docker!"` fest.

    > [!NOTE]
    > Diese Konfiguration erlaubt keine externen Verbindungen zum Container. Der SSH-Zugriff ist nur über die Kudu-/SCM-Website möglich, die über die Veröffentlichungsanmeldeinformationen authentifiziert wird.

    ```Dockerfile
    # ------------------------
    # SSH Server support
    # ------------------------
    RUN apt-get update \
        && apt-get install -y --no-install-recommends openssh-server \
        && echo "root:Docker!" | chpasswd
    ```

2. Fügen Sie der Dockerfile-Datei eine [`COPY`-Anweisung](https://docs.docker.com/engine/reference/builder/#copy) hinzu, mit der eine [sshd_config](https://man.openbsd.org/sshd_config)-Datei in das Verzeichnis */etc/ssh/* kopiert wird. Die Konfigurationsdatei sollte auf der Datei [sshd_config](https://github.com/Azure-App-Service/node/blob/master/8.2.1/sshd_config) im Azure App Service-GitHub-Repository basieren.

    > [!NOTE]
    > Die Datei *sshd_config* muss Folgendes enthalten, da andernfalls ein Verbindungsfehler auftritt: 
    > * `Ciphers` muss mindestens einen der folgenden Werte enthalten: `aes128-cbc,3des-cbc,aes256-cbc`.
    > * `MACs` muss mindestens einen der folgenden Werte enthalten: `hmac-sha1,hmac-sha1-96`.

    ```Dockerfile
    COPY sshd_config /etc/ssh/
    ```

3. Fügen Sie Port 2222 in die [`EXPOSE`-Anweisung](https://docs.docker.com/engine/reference/builder/#expose) für die Dockerfile-Datei ein. Selbst wenn das Root-Kennwort bekannt ist, kann auf Port 2222 nicht aus dem Internet zugegriffen werden. Es handelt sich um einen internen Port, auf den nur von Containern innerhalb des Brückennetzwerks eines privaten virtuellen Netzwerks zugegriffen werden kann.

    ```Dockerfile
    EXPOSE 2222 80
    ```

4. Sie sollten zum Starten des SSH-Diensts unbedingt ein Shell-Skript verwenden (siehe Beispiel unter [init_container.sh](https://github.com/Azure-App-Service/node/blob/master/6.9.3/startup/init_container.sh)).

    ```bash
    #!/bin/bash
    service ssh start
    ```

Die Dockerfile-Datei verwendet die [`ENTRYPOINT`-Anweisung](https://docs.docker.com/engine/reference/builder/#entrypoint) zum Ausführen des Skripts.

    ```Dockerfile
    COPY init_container.sh /opt/startup
    ...
    RUN chmod 755 /opt/startup/init_container.sh
    ...
    ENTRYPOINT ["/opt/startup/init_container.sh"]
    ```

## <a name="open-ssh-session-from-remote-shell"></a>Öffnen einer SSH-Sitzung per Remote-Shell

> [!NOTE]
> Diese Funktion steht derzeit als Vorschau zur Verfügung.
>

Mithilfe von TCP-Tunneln können Sie eine Netzwerkverbindung zwischen Ihrem Entwicklungscomputer und Web-App für Container herstellen, indem Sie eine authentifizierte WebSocket-Verbindung verwenden. Auf diese Weise können Sie eine SSH-Sitzung mit Ihrem Container, der unter App Service ausgeführt wird, auf dem Client Ihrer Wahl öffnen.

Zunächst müssen Sie die [Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli?view=azure-cli-latest) (Azure CLI) installieren. Öffnen Sie [Azure Cloud Shell](../../cloud-shell/overview.md), wenn Sie sich die Vorgehensweise ohne Installation der Azure CLI ansehen möchten. 

Fügen Sie die neueste App Service-Erweiterung hinzu, indem Sie [az extension add](/cli/azure/extension?view=azure-cli-latest#az-extension-add) ausführen:

```azurecli-interactive
az extension add --name webapp
```

Falls Sie `az extension add` bereits ausgeführt haben, können Sie stattdessen [az extension update](/cli/azure/extension?view=azure-cli-latest#az-extension-update) ausführen:

```azurecli-interactive
az extension update --name webapp
```

Öffnen Sie eine Remoteverbindung mit Ihrer App, indem Sie den Befehl [az webapp remote-connection create](/cli/azure/ext/webapp/webapp/remote-connection?view=azure-cli-latest#ext-webapp-az-webapp-remote-connection-create) verwenden. Geben Sie _\<Abonnement\_ID>_, _\<Gruppen\_Name>_ und \_<App\_Name>_ für Ihre App an.

```azurecli-interactive
az webapp remote-connection create --subscription <subscription_id> --resource-group <group_name> -n <app_name> &
```

> [!TIP]
> `&` am Ende des Befehls ist der Vollständigkeit halber angegeben, falls Sie Cloud Shell verwenden. Hierbei wird der Prozess im Hintergrund ausgeführt, damit Sie den nächsten Befehl in derselben Shell ausführen können.

Mit der Befehlsausgabe erhalten Sie die Informationen, die Sie zum Öffnen einer SSH-Sitzung benötigen.

```
Port 21382 is open
SSH is available { username: root, password: Docker! }
Start your favorite client and connect to port 21382
```

Öffnen Sie eine SSH-Sitzung mit Ihrem Container mit dem Client Ihrer Wahl, indem Sie den lokalen Port verwenden. Im folgenden Beispiel wird der Standardbefehl [ssh](https://ss64.com/bash/ssh.html) verwendet:

```azurecli-interactive
ssh root@127.0.0.1 -p <port>
```

Geben Sie nach Aufforderung `yes` ein, um mit der Verbindungsherstellung fortzufahren. Sie werden dann aufgefordert, das Kennwort einzugeben. Verwenden Sie `Docker!`, wie oben gezeigt.

```
Warning: Permanently added '[127.0.0.1]:21382' (ECDSA) to the list of known hosts.
root@127.0.0.1's password:
```

Nachdem Sie authentifiziert wurden, sollte der Begrüßungsbildschirm der Sitzung angezeigt werden.

```
  _____
  /  _  \ __________ _________   ____
 /  /_\  \___   /  |  \_  __ \_/ __ \
/    |    \/    /|  |  /|  | \/\  ___/
\____|__  /_____ \____/ |__|    \___  >
        \/      \/                  \/
A P P   S E R V I C E   O N   L I N U X

0e690efa93e2:~#
```

Sie sind jetzt mit Ihrem Connector verbunden. 

Versuchen Sie, den Befehl [top](https://ss64.com/bash/top.html) auszuführen. Sie sollten den Prozess Ihrer App in der Prozessliste verfolgen können. In der folgenden Beispielausgabe ist dies der Prozess mit `PID 263`.

```
Mem: 1578756K used, 127032K free, 8744K shrd, 201592K buff, 341348K cached
CPU:   3% usr   3% sys   0% nic  92% idle   0% io   0% irq   0% sirq
Load average: 0.07 0.04 0.08 4/765 45738
  PID  PPID USER     STAT   VSZ %VSZ CPU %CPU COMMAND
    1     0 root     S     1528   0%   0   0% /sbin/init
  235     1 root     S     632m  38%   0   0% PM2 v2.10.3: God Daemon (/root/.pm2)
  263   235 root     S     630m  38%   0   0% node /home/site/wwwroot/app.js
  482   291 root     S     7368   0%   0   0% sshd: root@pts/0
45513   291 root     S     7356   0%   0   0% sshd: root@pts/1
  291     1 root     S     7324   0%   0   0% /usr/sbin/sshd
  490   482 root     S     1540   0%   0   0% -ash
45539 45513 root     S     1540   0%   0   0% -ash
45678 45539 root     R     1536   0%   0   0% top
45733     1 root     Z        0   0%   0   0% [init]
45734     1 root     Z        0   0%   0   0% [init]
45735     1 root     Z        0   0%   0   0% [init]
45736     1 root     Z        0   0%   0   0% [init]
45737     1 root     Z        0   0%   0   0% [init]
45738     1 root     Z        0   0%   0   0% [init]
```

## <a name="next-steps"></a>Nächste Schritte

Im [Azure-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview) können Sie Fragen stellen und Antworten auf Probleme erhalten.

Weitere Informationen zu Web-App für Container finden Sie in den folgenden Artikeln:

* [Introducing remote debugging of Node.js apps on Azure App Service from VS Code](https://medium.com/@auchenberg/introducing-remote-debugging-of-node-js-apps-on-azure-app-service-from-vs-code-in-public-preview-9b8d83a6e1f0) (Einführung in das Remotedebuggen von Node.js-Apps unter Azure App Service über VS Code)
* [Verwenden eines benutzerdefinierten Docker-Images für Web-App für Container](quickstart-docker-go.md)
* [Verwenden von .NET Core in Azure App Service unter Linux](quickstart-dotnetcore.md)
* [Verwenden von Ruby in Azure App Service unter Linux](quickstart-ruby.md)
* [Azure App Service-Web-App für Container – FAQs](app-service-linux-faq.md)
