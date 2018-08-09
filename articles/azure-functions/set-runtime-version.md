---
title: Einstellen von Runtimeversionen von Azure Functions als Ziel
description: Azure Functions unterstützt mehrere Versionen der Runtime. Erfahren Sie, wie Sie die Runtimeversion einer in Azure gehosteten Funktions-App angeben.
services: functions
documentationcenter: ''
author: ggailey777
manager: cfowler
editor: ''
ms.service: functions
ms.workload: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2018
ms.author: glenga
ms.openlocfilehash: 3efc09f93cf8b3e65d595c87d0cf25691386d6e2
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39434763"
---
# <a name="how-to-target-azure-functions-runtime-versions"></a>Einstellen von Runtimeversionen von Azure Functions als Ziel

Eine Funktionen-App wird für eine bestimmte Version der Azure Functions-Runtime ausgeführt. Es gibt zwei Hauptversionen: [1.x und 2.x](functions-versions.md). In diesem Artikel wird erläutert, wie Sie eine Funktionen-App in Azure so konfigurieren, dass sie mit der von Ihnen ausgewählten Version ausgeführt wird. Informationen zum Konfigurieren einer lokalen Entwicklungsumgebung für eine bestimmte Version finden Sie unter [Codieren und lokales Testen von Azure Functions](functions-run-local.md).

>[!IMPORTANT]   
> Die Azure Functions-Laufzeit 2.0 ist eine Vorschauversion und derzeit werden nicht alle Features von Azure Functions unterstützt. Weitere Informationen finden Sie unter [Einstellen von Runtimeversionen von Azure Functions als Ziel](functions-versions.md).

## <a name="automatic-and-manual-version-updates"></a>Automatische und manuelle Versionsupdates

Mit Functions können Sie eine bestimmte Version der Runtime als Ziel festlegen, indem Sie die Anwendungseinstellung `FUNCTIONS_EXTENSION_VERSION` in einer Funktionen-App verwenden. Die Funktionen-App verwendet weiterhin die angegebene Version der Runtime, bis Sie ausdrücklich die Verwendung einer neuen Version auswählen.

Wenn Sie nur die Hauptversion („~1“ für 1.x oder „beta“ für 2.x) angeben, wird die Funktionen-App automatisch auf neue Nebenversionen der Runtime aktualisiert, sobald diese verfügbar werden. Neue Nebenversion enthalten keine grundlegenden Änderungen. Wenn Sie eine Nebenversion (z.B. „1.0.11360“) angeben, wird für die Funktionen-App diese Version beibehalten, bis Sie sie explizit ändern. 

Wenn eine neue Version öffentlich verfügbar ist, bietet eine Eingabeaufforderung im Portal Ihnen die Möglichkeit, auf diese Version umzusteigen. Nach dem Wechsel zu einer neuen Version können Sie jederzeit mit der Anwendungseinstellung `FUNCTIONS_EXTENSION_VERSION` zu einer früheren Version zurückkehren.

Eine Änderung an der Runtimeversion bewirkt, dass eine Funktionen-App neu gestartet wird.

Die Werte, die Sie in der App-Einstellung `FUNCTIONS_EXTENSION_VERSION` festlegen können, um automatische Updates zu aktivieren, lauten zurzeit „~1“ für die 1.x-Runtime und „beta“ für 2.x.

## <a name="view-the-current-runtime-version"></a>Anzeigen der aktuellen Runtimeversion

Gehen Sie folgendermaßen vor, um die zurzeit von einer Funktionen-App verwendete Runtimeversion anzuzeigen. 

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zur Funktionen-App, und wählen Sie dann unter **Konfigurierte Features** die Option **Funktionen-App-Einstellungen** aus. 

    ![„Funktionen-App-Einstellungen“ auswählen](./media/functions-versions/add-update-app-setting.png)

2. Suchen Sie auf der Registerkarte **Funktionen-App-Einstellungen** nach der **Runtimeversion**. Sie sehen dort die spezifische Runtimeversion und die angeforderte Hauptversion. Im folgenden Beispiel ist die App-Einstellung FUNCTIONS\_EXTENSION\_VERSION auf `~1` festgelegt.
 
   ![„Funktionen-App-Einstellungen“ auswählen](./media/functions-versions/function-app-view-version.png)

## <a name="target-a-version-using-the-portal"></a>Verwenden einer Version als Ziel mithilfe des Portals

Wenn Sie eine andere Version als die aktuelle Hauptversion oder Version 2.0 als Ziel verwenden möchten, legen Sie die Anwendungseinstellung `FUNCTIONS_EXTENSION_VERSION` fest.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrer Funktions-App, und wählen Sie unter **Konfigurierte Features** die Option **Anwendungseinstellungen** aus.

    ![„Funktionen-App-Einstellungen“ auswählen](./media/functions-versions/add-update-app-setting1a.png)

2. Suchen Sie auf der Registerkarte **Anwendungseinstellungen** die Einstellung `FUNCTIONS_EXTENSION_VERSION`, und ändern Sie den Wert in eine gültige Version der 1.x-Runtime oder in `beta` für Version 2.0. Eine Tilde mit Hauptversion bedeutet, dass die neueste Version dieser Hauptversion (z. B. „~1“) verwendet wird. 

    ![Funktions-App-Einstellung festlegen](./media/functions-versions/add-update-app-setting2.png)

3. Klicken Sie auf **Speichern**, um die Aktualisierung der Anwendungseinstellung zu speichern. 

## <a name="target-a-version-using-azure-cli"></a>Verwenden einer Version als Ziel mit der Azure CLI

 Sie können `FUNCTIONS_EXTENSION_VERSION` auch über die Azure-Befehlszeilenschnittstelle festlegen. Aktualisieren Sie über die Azure-Befehlszeilenschnittstelle die Anwendungseinstellungen in der Funktions-App mit dem Befehl [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#set).

```azurecli-interactive
az functionapp config appsettings set --name <function_app> \
--resource-group <my_resource_group> \
--settings FUNCTIONS_EXTENSION_VERSION=<version>
```
Ersetzen Sie in diesem Code `<function_app>` durch den Namen der Funktions-App. Ersetzen Sie außerdem `<my_resource_group>` durch den Namen der Ressourcengruppe für Ihre Funktions-App. Ersetzen Sie `<version>` durch eine gültige Version der Runtime 1.x oder durch `beta` für Version 2.0. 

Sie können diesen Befehl an der [Azure Cloud Shell](../cloud-shell/overview.md) ausführen, indem Sie im vorangehenden Codebeispiel **Ausprobieren** auswählen. Sie können auch die [Azure-Befehlszeilenschnittstelle lokal](/cli/azure/install-azure-cli) zum Ausführen dieses Befehls verwenden, nachdem Sie sich mit [az login](/cli/azure/reference-index#az-login) angemeldet haben.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Verwenden der 2.0-Runtime als Ziel in Ihrer lokalen Entwicklungsumgebung](functions-run-local.md)

> [!div class="nextstepaction"]
> [Siehe Anmerkungen zu dieser Version für Runtimeversionen](https://github.com/Azure/azure-webjobs-sdk-script/releases)