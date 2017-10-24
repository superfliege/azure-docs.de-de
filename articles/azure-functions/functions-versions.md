---
title: Einstellen von Runtimeversionen von Azure Functions als Ziel | Microsoft-Dokumentation
description: "Functions unterstützt mehrere Versionen der Runtime. Erfahren Sie, wie Sie die Runtimeversion einer in Azure gehosteten Funktions-App angeben."
services: functions
documentationcenter: 
author: ggailey777
manager: cfowler
editor: 
ms.service: functions
ms.workload: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2017
ms.author: glenga
ms.openlocfilehash: 26d4276a0a550d78a9c7657c464bd3320c956fb0
ms.sourcegitcommit: 54fd091c82a71fbc663b2220b27bc0b691a39b5b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/12/2017
---
# <a name="how-to-target-azure-functions-runtime-versions"></a>Einstellen von Runtimeversionen von Azure Functions als Ziel

Die Azure Functions-Runtime implementiert die serverlose Ausführung Ihres Codes in Azure. Sie finden diese Runtime in verschiedenen Umgebungen, die nicht in Azure gehostet werden. In den [Azure Functions Core-Tools](functions-run-local.md) wird diese Runtime auf Ihrem Entwicklungscomputer implementiert. Die [Azure Functions-Runtime](functions-runtime-overview.md) ermöglicht Ihnen, Functions in lokalen Umgebungen zu verwenden. 

Functions unterstützt mehrere Hauptversionen der Runtime. Ein Update der Hauptversion kann Fehler verursachende Änderungen nach sich ziehen. In diesem Thema wird beschrieben, wie Funktions-Apps auf eine bestimmte Runtimeversion eingestellt werden können, wenn sie in Azure gehostet werden. 

Mit Functions können Sie eine bestimmte Hauptversion der Runtime als Ziel festlegen, indem Sie die Anwendungseinstellung `FUNCTIONS_EXTENSION_VERSION` in Ihrer Funktions-App verwenden. Dies gilt für öffentliche und Vorschauversionen. Ihre Funktions-App verwendet weiterhin die angegebene Hauptversion der Runtime, bis Sie ausdrücklich auswählen, zu einer neuen Version zu wechseln. Ihre Funktions-App wird auf neue Nebenversionen der Runtime aktualisiert, sobald diese verfügbar werden. Updates der Nebenversion enthalten keine Fehler verursachenden Änderungen.  

Wenn eine neue Hauptversion öffentlich verfügbar ist, erhalten Sie die Möglichkeit, zu dieser Version zu wechseln, wenn Sie Ihre Funktions-App im Portal anzeigen. Nach dem Wechsel zu einer neuen Version können Sie jederzeit mit der Anwendungseinstellung `FUNCTIONS_EXTENSION_VERSION` zu einer früheren Runtimeversion zurückwechseln.

Jede Änderung an der Runtimeversion bewirkt, dass Ihre Funktions-App neu gestartet wird. Versionshinweise für alle Common Language Runtime-Versionen (Haupt- und Nebenversionsnummern) werden im [GitHub-Repository](https://github.com/Azure/azure-webjobs-sdk-script/releases) veröffentlicht.   
## <a name="view-the-current-runtime-version"></a>Anzeigen der aktuellen Runtimeversion

Gehen Sie folgendermaßen vor, um die genaue derzeit von Ihrer Funktions-App verwendete Runtimeversion anzuzeigen. 

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrer Funktions-App, und wählen Sie unter **Konfigurierte Features** die Option **Funktionen-App-Einstellungen** aus. 

    ![„Funktionen-App-Einstellungen“ auswählen](./media/functions-versions/add-update-app-setting.png)

2. Suchen Sie auf der Registerkarte **Funktionen-App-Einstellungen** nach der **Runtimeversion**. Sie sehen dort die spezifische Runtimeversion und die angeforderte Hauptversion. Im nachstehenden Beispiel ist die Hauptversion auf `~1.0` festgelegt.
 
   ![„Funktionen-App-Einstellungen“ auswählen](./media/functions-versions/function-app-view-version.png)

## <a name="target-the-functions-version-20-runtime"></a>Einstellen der Functions-Runtime, Version 2.0

>[!IMPORTANT]   
> Die Azure Functions-Runtime 2.0 ist eine Vorschauversion, und derzeit werden nicht alle Features von Azure Functions unterstützt. Weitere Informationen finden Sie unter [Bekannte Probleme der Azure Functions-Laufzeit 2.0](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Azure-Functions-runtime-2.0-known-issues).  

<!-- Add a table comparing the 1.x and 2.x runtime features-->

[!INCLUDE [functions-set-runtime-version](../../includes/functions-set-runtime-version.md)]

Sie können im Azure-Portal Ihre Funktions-App auf die Runtimeversion 2.0 Preview umstellen. Wählen Sie auf der Registerkarte **Funktionen-App-Einstellungen** unter **Runtimeversion** die Option **Beta** aus.  

   ![„Funktionen-App-Einstellungen“ auswählen](./media/functions-versions/function-app-view-version.png)

Diese Einstellung entspricht dem Festlegen der Anwendungseinstellung `FUNCTIONS_EXTENSION_VERSION` auf `beta`. Wählen Sie die Schaltfläche **~1** aus, um zurück zur aktuellen öffentlich unterstützten Hauptversion zu wechseln. Sie können diese Anwendungseinstellung auch mit der Azure-Befehlszeilenschnittstelle aktualisieren. 

## <a name="target-a-specific-runtime-version-from-the-portal"></a>Einstellen einer bestimmten Runtimeversion als Ziel über das Portal

Wenn Sie eine andere Hauptversion als die aktuelle Hauptversion oder Version 2.0 einstellen möchten, müssen Sie die Anwendungseinstellung `FUNCTIONS_EXTENSION_VERSION` festlegen.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrer Funktions-App, und wählen Sie unter **Konfigurierte Features** die Option **Anwendungseinstellungen** aus.

    ![„Funktionen-App-Einstellungen“ auswählen](./media/functions-versions/add-update-app-setting1a.png)

2. Suchen Sie auf der Registerkarte **Anwendungseinstellungen** die Einstellung `FUNCTIONS_EXTENSION_VERSION`, und ändern Sie den Wert in eine gültige Hauptversion der 1.x-Runtime oder in `beta` für Version 2.0. 

    ![Funktions-App-Einstellung festlegen](./media/functions-versions/add-update-app-setting2.png)

3. Klicken Sie auf **Speichern**, um die Aktualisierung der Anwendungseinstellung zu speichern. 

## <a name="target-a-specific-version-using-azure-cli"></a>Einstellen einer bestimmten Version mit der Azure-Befehlszeilenschnittstelle

 Sie können `FUNCTIONS_EXTENSION_VERSION` auch über die Azure-Befehlszeilenschnittstelle festlegen. Aktualisieren Sie über die Azure-Befehlszeilenschnittstelle die Anwendungseinstellungen in der Funktions-App mit dem Befehl [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#set).

```azurecli-interactive
az functionapp config appsettings set --name <function_app> \
--resource-group <my_resource_group> \
--settings FUNCTIONS_EXTENSION_VERSION=<version>
```
Ersetzen Sie in diesem Code `<function_app>` durch den Namen der Funktions-App. Ersetzen Sie außerdem `<my_resource_group>` durch den Namen der Ressourcengruppe für Ihre Funktions-App. Ersetzen Sie `<version>` durch eine gültige Hauptversion der Runtime 1.x oder durch `beta` für Version 2.0. 

Sie können diesen Befehl an der [Azure Cloud Shell](../cloud-shell/overview.md) ausführen, indem Sie im vorangehenden Codebeispiel **Ausprobieren** auswählen. Sie können auch die [Azure-Befehlszeilenschnittstelle lokal](/cli/azure/install-azure-cli) zum Ausführen dieses Befehls verwenden, nachdem Sie sich mit [az login](/cli/azure#az_login) angemeldet haben.
