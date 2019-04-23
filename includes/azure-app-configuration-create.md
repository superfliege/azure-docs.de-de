---
title: Includedatei
description: Includedatei
services: azure-app-configuration
author: yegu
ms.service: azure-app-configuration
ms.topic: include
ms.date: 01/22/2019
ms.author: yegu
ms.custom: include file
ms.openlocfilehash: 9b86f2e05e2cb42470061bd6398b4200607f2418
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2019
ms.locfileid: "60012284"
---
1. Melden Sie sich beim [Azure-Portal](https://aka.ms/azconfig/portal) an, um einen neuen App-Konfigurationsspeicher zu erstellen. Wählen Sie links oben auf der Seite die Option **+ Ressource erstellen** aus. Geben Sie im Feld **Marketplace durchsuchen** den Suchbegriff **App Configuration** ein, und drücken Sie die EINGABETASTE.

    ![Suchen nach App Configuration](../articles/azure-app-configuration/media/quickstarts/azure-app-configuration-new.png)

2. Wählen Sie **App Configuration** in den Suchergebnissen und dann **Erstellen** aus.

3. Geben Sie auf der Seite **App Configuration** > **Erstellen** die folgenden Einstellungen ein.

    | Einstellung | Empfohlener Wert | BESCHREIBUNG |
    |---|---|---|
    | **Ressourcenname** | Global eindeutiger Name | Geben Sie einen eindeutigen Ressourcennamen ein, der für die Ressource des App-Konfigurationsspeichers verwendet werden soll. Der Name muss zwischen 1 und 63 Zeichen lang sein und darf nur Zahlen, Buchstaben und das Zeichen `-` enthalten. Der Name darf weder mit dem Zeichen `-` beginnen oder enden noch mehrere aufeinanderfolgende Zeichen vom Typ `-` enthalten.  |
    | **Abonnement** | Ihr Abonnement | Wählen Sie das Azure-Abonnement aus, das Sie zum Testen von App Configuration verwenden möchten. Wenn das Konto nur über ein einzelnes Abonnement verfügt, wird automatisch dieses Abonnement ausgewählt, und die Dropdownliste **Abonnement** wird nicht angezeigt. |
    | **Ressourcengruppe** | *AppConfigTestResources* | Wählen oder erstellen Sie eine Ressourcengruppe für Ihre App-Konfigurationsspeicher-Ressource. Diese Gruppe ist beim Organisieren mehrerer Ressourcen hilfreich, die Sie möglicherweise zur gleichen Zeit löschen möchten, indem Sie die Ressourcengruppe löschen. Weitere Informationen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen](/azure/azure-resource-manager/resource-group-overview). |
    | **Location** | *USA (Mitte)* | Verwenden Sie **Standort**, um den geografischen Standort anzugeben, an dem Ihr App-Konfigurationsspeicher gehostet wird. Erstellen Sie die Ressource in derselben Region wie andere Komponenten Ihrer Anwendung, um eine optimale Leistung zu erzielen. |

    ![Erstellen einer App-Konfigurationsspeicherressource](../articles/azure-app-configuration/media/quickstarts/azure-app-configuration-create.png)

4. Klicken Sie auf **Erstellen**. Die Bereitstellung kann einige Minuten dauern.

5. Wählen Sie nach Abschluss der Bereitstellung die Optionen **Einstellungen** > **Zugriffsschlüssel** aus. Notieren Sie sich entweder die Primärschlüssel-Verbindungszeichenfolge vom Typ „Schreibgeschützt“ oder „Lesen/Schreiben“. Diese Verbindungszeichenfolge verwenden Sie später zum Konfigurieren Ihrer Anwendung für die Kommunikation mit dem erstellten App-Konfigurationsspeicher.
