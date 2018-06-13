---
title: Includedatei
description: Includedatei
services: container-registry
author: mmacy
ms.service: container-registry
ms.topic: include
ms.date: 04/23/2018
ms.author: marsma
ms.custom: include file
ms.openlocfilehash: 6ed114ea6162c9d4888b6f86998cfb422a3944e8
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
ms.locfileid: "32198225"
---
## <a name="create-a-service-principal"></a>Erstellen eines Dienstprinzipals

Zum Erstellen eines Dienstprinzipals mit Zugriff auf Ihre Containerregistrierung können Sie das folgende Skript verwenden. Aktualisieren Sie die Variable `ACR_NAME` mit dem Namen Ihrer Containerregistrierung und optional den Wert `--role` im Befehl [az ad sp create-for-rbac][az-ad-sp-create-for-rbac], um verschiedene Berechtigungen zu gewähren. [Azure CLI](/cli/azure/install-azure-cli) muss installiert sein, damit Sie dieses Skript verwenden können.

Nachdem Sie das Skript ausgeführt haben, notieren Sie die **ID** und das **Kennwort** des Dienstprinzipals. Sobald Sie über dessen Anmeldeinformationen verfügen, können Sie Ihre Anwendungen und Dienste so konfigurieren, dass diese bei Ihrer Containerregistrierung als Dienstprinzipal authentifiziert werden.

[!code-azurecli-interactive[acr-sp-create](~/cli_scripts/container-registry/service-principal-create/service-principal-create.sh)]

## <a name="use-an-existing-service-principal"></a>Verwenden eines vorhandenen Dienstprinzipals

Damit Sie einem vorhandenen Dienstprinzipal den Zugriff auf die Registrierung gewähren können, müssen Sie dem Dienstprinzipal eine neue Rolle zuweisen. Wie bei der Erstellung eines neuen Dienstprinzipals können Sie Pull-, Push- und Pull- sowie Besitzerzugriff gewähren.

Das folgende Skript verwendet den Befehl [az role assignment create][az-role-assignment-create], um einem Dienstprinzipal *Pull*-Berechtigungen zuzuweisen, den Sie in der Variablen `SERVICE_PRINCIPAL_ID` festlegen. Passen Sie den Wert `--role` an, wenn Sie eine andere Zugriffsebene zuweisen möchten.

[!code-azurecli-interactive[acr-sp-role-assign](~/cli_scripts/container-registry/service-principal-assign-role/service-principal-assign-role.sh)]

<!-- LINKS - Internal -->
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az_ad_sp_create_for_rbac
[az-role-assignment-create]: /cli/azure/role/assignment#az_role_assignment_create
