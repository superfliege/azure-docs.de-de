---
title: Includedatei
description: Includedatei
services: functions
author: ggailey777
ms.service: functions
ms.topic: include
ms.date: 05/22/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: a8be74558fb71ceb1c28c27004b27a5a5f477a8f
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2018
ms.locfileid: "49650700"
---
Mit der Azure Functions-Projektvorlage in Visual Studio wird ein Projekt erstellt, das in einer Funktions-App in Azure veröffentlicht werden kann. Sie können mit einer Funktionen-App Funktionen zu logischen Einheiten gruppieren, um die Verwaltung, Bereitstellung und Freigabe von Ressourcen zu ermöglichen.

1. Wählen Sie in Visual Studio im Menü **Datei** die Optionen **Neu** > **Projekt**.

2. Wählen Sie im Dialogfeld **Neues Projekt** die Option **Installiert**, erweitern Sie **Visual C#** > **Cloud**, und wählen Sie **Azure Functions** aus. Geben Sie unter **Name** einen Namen für Ihr Projekt ein, und klicken Sie auf **OK**. Der Name der Funktions-App muss als C#-Namespace gültig sein, verwenden Sie daher keine Unterstriche, Bindestriche oder andere nicht alphanumerische Zeichen.

    ![Dialogfeld „Neues Projekt“ zum Erstellen einer Funktion in Visual Studio](./media/functions-vstools-create/functions-vs-new-project.png)

3. Verwenden Sie die Einstellungen, die in der Tabelle unter der Abbildung angegeben sind.

    ![Dialogfeld „Neue Funktion“ in Visual Studio](./media/functions-vstools-create/functions-vs-new-function.png) 

    | Einstellung      | Empfohlener Wert  | BESCHREIBUNG                      |
    | ------------ |  ------- |----------------------------------------- |
    | **Version** | Azure Functions 2.x <br />(.NET Core) | Hierdurch wird ein Funktionsprojekt mit Version 2.x der Runtime von Azure Functions erstellt, die .NET Core unterstützt. Azure Functions 1.x unterstützt .NET Framework. Weitere Informationen finden Sie unter [Einstellen von Runtimeversionen von Azure Functions als Ziel](../articles/azure-functions/functions-versions.md).   |
    | **Vorlage** | HTTP-Trigger | Es wird eine Funktion erstellt, die mit einer HTTP-Anforderung ausgelöst wird. |
    | **Speicherkonto**  | Speicheremulator | Für einen HTTP-Trigger wird die Storage-Kontoverbindung nicht verwendet. Für alle anderen Triggertypen ist eine gültige Speicherkonto-Verbindungszeichenfolge erforderlich. |
    | **Zugriffsrechte** | Anonym | Die erstellte Funktion kann von jedem Client ausgelöst werden, ohne dass ein Schlüssel angegeben werden muss. Diese Autorisierungseinstellung erleichtert Ihnen das Testen Ihrer neuen Funktion. Weitere Informationen zu Schlüsseln und zur Autorisierung finden Sie unter [Autorisierungsschlüssel](../articles/azure-functions/functions-bindings-http-webhook.md#authorization-keys) im Artikel [HTTP- und Webhookbindungen in Azure Functions](../articles/azure-functions/functions-bindings-http-webhook.md). |
4. Klicken Sie auf **OK**, um das Funktionsprojekt und die per HTTP ausgelöste Funktion zu erstellen.
