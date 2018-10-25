---
title: Einstellen von Runtimeversionen von Azure Functions als Ziel
description: Azure Functions unterstützt mehrere Versionen der Runtime. Erfahren Sie, wie Sie die Runtimeversion einer in Azure gehosteten Funktions-App angeben.
services: functions
documentationcenter: ''
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 10/05/2018
ms.author: glenga
ms.openlocfilehash: 6d89746c0a2d4642e5025789d352803195c0a3b9
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2018
ms.locfileid: "48886806"
---
# <a name="how-to-target-azure-functions-runtime-versions"></a>Einstellen von Runtimeversionen von Azure Functions als Ziel

Eine Funktionen-App wird für eine bestimmte Version der Azure Functions-Runtime ausgeführt. Es gibt zwei Hauptversionen: [1.x und 2.x](functions-versions.md). Standardmäßig werden Funktions-Apps für Version 2.x der Runtime erstellt. In diesem Artikel wird erläutert, wie Sie eine Funktionen-App in Azure so konfigurieren, dass sie mit der von Ihnen ausgewählten Version ausgeführt wird. Informationen zum Konfigurieren einer lokalen Entwicklungsumgebung für eine bestimmte Version finden Sie unter [Codieren und lokales Testen von Azure Functions](functions-run-local.md).

> [!NOTE]
> Sie können die Runtimeversion für eine Funktions-App, die über Funktionen verfügt, nicht ändern. Sie sollten die Runtimeversion im Azure-Portal ändern.

## <a name="automatic-and-manual-version-updates"></a>Automatische und manuelle Versionsupdates

Mit Functions können Sie eine bestimmte Version der Runtime als Ziel festlegen, indem Sie die Anwendungseinstellung `FUNCTIONS_EXTENSION_VERSION` in einer Funktionen-App verwenden. Die Funktionen-App verwendet weiterhin die angegebene Version der Runtime, bis Sie ausdrücklich die Verwendung einer neuen Version auswählen.

Wenn Sie nur die Hauptversion („~2“ für 2.x oder „~1“ für 1.x) angeben, wird die Funktions-App automatisch auf neue Nebenversionen der Runtime aktualisiert, sobald diese verfügbar werden. Neue Nebenversion enthalten keine grundlegenden Änderungen. Wenn Sie eine Nebenversion (z.B. „2.0.12345“) angeben, wird die Funktions-App an diese spezifische Version angeheftet, bis Sie sie explizit ändern.

Wenn eine neue Version öffentlich verfügbar ist, bietet eine Eingabeaufforderung im Portal Ihnen die Möglichkeit, auf diese Version umzusteigen. Nach dem Wechsel zu einer neuen Version können Sie jederzeit mit der Anwendungseinstellung `FUNCTIONS_EXTENSION_VERSION` zu einer früheren Version zurückkehren.

Eine Änderung an der Runtimeversion bewirkt, dass eine Funktionen-App neu gestartet wird.

Die Werte, die Sie in der App-Einstellung `FUNCTIONS_EXTENSION_VERSION` festlegen können, um automatische Updates zu aktivieren, lauten zurzeit „~1“ für die 1.x-Runtime und „~2“ für 2.x.

## <a name="view-and-update-the-current-runtime-version"></a>Anzeigen und Aktualisieren der aktuellen Runtimeversion

Gehen Sie folgendermaßen vor, um die zurzeit von einer Funktionen-App verwendete Runtimeversion anzuzeigen.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrer Funktions-App.

1. Wählen Sie unter **Konfigurierte Features** die Option **Funktionen-App-Einstellungen** aus.

    ![„Funktionen-App-Einstellungen“ auswählen](./media/set-runtime-version/add-update-app-setting.png)

1. Suchen Sie auf der Registerkarte **Funktionen-App-Einstellungen** nach der **Runtimeversion**. Sie sehen dort die spezifische Runtimeversion und die angeforderte Hauptversion. Im nachstehenden Beispiel ist die Version auf `~2` festgelegt.

   ![„Funktionen-App-Einstellungen“ auswählen](./media/set-runtime-version/function-app-view-version.png)

1. Um Ihre Funktions-App an die Runtime der Version 1.x anzuheften, wählen Sie **~1** unter **Laufzeitversion** aus. Dieser Schalter ist deaktiviert, wenn Ihre App Funktionen enthält.

1. Wenn Sie die Runtimeversion geändert haben, wechseln Sie zurück zur Registerkarte **Übersicht**, und wählen Sie **Neu starten** aus, um die App neu zu starten.  Die Funktions-App wird neu gestartet und mit der Runtime der Version 1.x ausgeführt. Außerdem werden die Vorlagen für Version 1.x verwendet, wenn Sie Funktionen erstellen.

## <a name="view-and-update-the-runtime-version-using-azure-cli"></a>Anzeigen und Aktualisieren der Runtimeversion mithilfe der Azure-Befehlszeilenschnittstelle

Sie können `FUNCTIONS_EXTENSION_VERSION` auch über die Azure-Befehlszeilenschnittstelle anzeigen und festlegen.

>[!NOTE]
>Da auch andere Einstellungen von der Runtimeversion beeinträchtigt werden können, sollten Sie die Version im Portal ändern. Das Portal nimmt automatisch die anderen erforderlichen Updates vor, wenn Sie die Runtimeversionen ändern, z.B. Node.js-Version und Runtimestapel.  

Zeigen Sie in der Azure-Befehlszeilenschnittstelle die aktuelle Version der Runtime mit dem Befehl [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#set) an.

```azurecli-interactive
az functionapp config appsettings list --name <function_app> \
--resource-group <my_resource_group>
```

Ersetzen Sie in diesem Code `<function_app>` durch den Namen der Funktions-App. Ersetzen Sie außerdem `<my_resource_group>` durch den Namen der Ressourcengruppe für Ihre Funktions-App. 

Sie sehen `FUNCTIONS_EXTENSION_VERSION` in der folgenden Ausgabe, die zur besseren Lesbarkeit beschnitten wurde:

```output
[
  {
    "name": "FUNCTIONS_EXTENSION_VERSION",
    "slotSetting": false,
    "value": "~2"
  },
  {
    "name": "FUNCTIONS_WORKER_RUNTIME",
    "slotSetting": false,
    "value": "dotnet"
  },
  
  ...
  
  {
    "name": "WEBSITE_NODE_DEFAULT_VERSION",
    "slotSetting": false,
    "value": "8.11.1"
  }
]
```

Sie können die Einstellung `FUNCTIONS_EXTENSION_VERSION` in der Funktions-App mit dem Befehl [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#set) aktualisieren.

```azurecli-interactive
az functionapp config appsettings set --name <function_app> \
--resource-group <my_resource_group> \
--settings FUNCTIONS_EXTENSION_VERSION=<version>
```

Ersetzen Sie `<function_app>` durch den Namen der Funktions-App. Ersetzen Sie außerdem `<my_resource_group>` durch den Namen der Ressourcengruppe für Ihre Funktions-App. Ersetzen Sie außerdem `<version>` durch eine gültige Version der Runtime 1.x oder durch `~2` für Version 2.x.

Sie können diesen Befehl an der [Azure Cloud Shell](../cloud-shell/overview.md) ausführen, indem Sie im vorangehenden Codebeispiel **Ausprobieren** auswählen. Sie können auch die [Azure-Befehlszeilenschnittstelle lokal](/cli/azure/install-azure-cli) zum Ausführen dieses Befehls verwenden, nachdem Sie sich mit [az login](/cli/azure/reference-index#az-login) angemeldet haben.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Verwenden der 2.0-Runtime als Ziel in Ihrer lokalen Entwicklungsumgebung](functions-run-local.md)

> [!div class="nextstepaction"]
> [Siehe Anmerkungen zu dieser Version für Runtimeversionen](https://github.com/Azure/azure-webjobs-sdk-script/releases)
