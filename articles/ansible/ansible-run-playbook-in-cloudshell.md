---
title: Ausführen von Ansible mit Bash in Azure Cloud Shell
description: Hier erfahren Sie, wie Sie verschiedene Ansible-Aufgaben mit Bash in Azure Cloud Shell ausführen.
ms.service: ansible
keywords: Ansible, Azure, DevOps, Bash, CloudShell, Playbook, Bash
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 08/07/2018
ms.topic: article
ms.openlocfilehash: 6bfac47e4afa41b4c75a8d33b4eea1ff5103296d
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2019
ms.locfileid: "54050896"
---
# <a name="run-ansible-with-bash-in-azure-cloud-shell"></a>Ausführen von Ansible mit Bash in Azure Cloud Shell

In diesem Tutorial erfahren Sie, wie Sie Bash in Cloud Shell verwenden, um ein Azure-Abonnement als Ihren Ansible-Arbeitsbereich zu konfigurieren. 

## <a name="prerequisites"></a>Voraussetzungen

- **Azure-Abonnement:** Falls Sie nicht über ein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) erstellen.

- **Konfigurieren von Azure Cloud Shell:** Sollten Sie noch nicht mit Azure Cloud Shell vertraut sein, finden Sie im Artikel [Schnellstart für Bash in Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart) Informationen zu den ersten Schritten mit Cloud Shell sowie zur Konfiguration. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="automatic-credential-configuration"></a>Automatische Konfiguration von Anmeldeinformationen

Wenn Sie bei Cloud Shell angemeldet sind, führt Ansible eine Authentifizierung bei Azure durch, um die Infrastruktur ohne zusätzliche Konfiguration verwalten zu können. Wenn Sie über mehrere Abonnements verfügen, können Sie auswählen, welches Abonnement Ansible verwenden soll, indem Sie die Umgebungsvariable `AZURE_SUBSCRIPTION_ID` exportieren. Zum Auflisten all Ihrer Azure-Abonnements führen Sie den folgenden Befehl aus:

```azurecli-interactive
az account list
```

Setzen Sie anhand der **ID** des Abonnements, mit dem Sie arbeiten möchten, die **AZURE_SUBSCRIPTION_ID** wie folgt fest:

```azurecli-interactive
export AZURE_SUBSCRIPTION_ID=<your-subscription-id>
```

## <a name="verify-the-configuration"></a>Überprüfen der Konfiguration
Erstellen Sie mithilfe von Ansible eine Ressourcengruppe, um zu überprüfen, ob die Konfiguration erfolgreich war.

[!INCLUDE [create-resource-group-with-ansible.md](../../includes/ansible-create-resource-group.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"] 
> [Erstellen eines einfachen virtuellen Computers in Azure mit Ansible](/azure/virtual-machines/linux/ansible-create-vm)