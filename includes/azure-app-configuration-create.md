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
ms.openlocfilehash: 71e63de247e05a4712687354ed548219b36e8f2f
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2019
ms.locfileid: "56885123"
---
1. Melden Sie sich zunächst am [Azure-Portal](https://aka.ms/azconfig/portal) an, um einen neuen App-Konfigurationsspeicher zu erstellen. Klicken Sie oben links auf der Seite auf **+ Create a resource** (+ Ressource erstellen). Geben Sie im Textfeld **Marketplace durchsuchen** den Suchbegriff **App Configuration** ein, und drücken Sie die **EINGABETASTE**.

    ![Suchen nach App Configuration](../articles/azure-app-configuration/media/quickstarts/azure-app-configuration-new.png)

2. Klicken Sie in den Suchergebnissen auf **App Configuration** und dann auf **Erstellen**.

3. Geben Sie auf der Seite **App Configuration** > **Erstellen** die folgenden Einstellungen ein:

    | Einstellung | Empfohlener Wert | BESCHREIBUNG |
    |---|---|---|
    | **Ressourcenname** | Global eindeutiger Name | Geben Sie einen eindeutigen Ressourcennamen ein, der für die Ressource des App-Konfigurationsspeichers verwendet werden soll. Der Name muss zwischen 1 und 63 Zeichen lang sein und darf nur Zahlen, Buchstaben und das Zeichen `-` enthalten. Der Name darf weder mit dem Zeichen `-` beginnen oder enden, noch mehrere aufeinanderfolgende Zeichen vom Typ `-` enthalten.  |
    | **Abonnement** | Ihr Abonnement | Wählen Sie das Azure-Abonnement aus, das Sie zum Testen von App Configuration verwenden möchten. Falls das Konto nur über ein einzelnes Abonnement verfügt, wird automatisch dieses Abonnement ausgewählt, und die Dropdownliste **Abonnement** wird nicht angezeigt. |
    | **Ressourcengruppe** | *AppConfigTestResources* | Wählen oder erstellen Sie eine Ressourcengruppe für Ihre App-Konfigurationsspeicher-Ressource. Diese Gruppe ist beim Organisieren mehrerer Ressourcen hilfreich, die Sie möglicherweise zur gleichen Zeit löschen möchten, indem Sie Ressourcengruppe löschen. Weitere Informationen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen](/azure/azure-resource-manager/resource-group-overview). |
    | **Location** | *USA (Mitte)* | Verwenden Sie **Standort**, um den geografischen Standort anzugeben, an dem Ihr App-Konfigurationsspeicher gehostet wird. Für die beste Leistung wird empfohlen, dass Sie die Ressource in derselben Region wie andere Komponenten Ihrer Anwendung erstellen. |

    ![Erstellen einer App-Konfigurationsspeicherressource](../articles/azure-app-configuration/media/quickstarts/azure-app-configuration-create.png)

4. Klicken Sie auf **Create**. Die Bereitstellung kann einige Minuten dauern.

5. Klicken Sie nach Abschluss der Bereitstellung auf **Einstellungen** > **Zugriffsschlüssel**. Notieren Sie sich entweder die Primärschlüssel-Verbindungszeichenfolge vom Typ „Schreibgeschützt“ oder „Lesen/Schreiben“. Sie verwenden sie später zum Konfigurieren Ihrer Anwendung für die Kommunikation mit dem eben erstellten App-Konfigurationsspeicher.

6. Klicken Sie auf **Schlüssel/Wert-Explorer** und **+ Erstellen**, um die folgenden Schlüssel-Wert-Paare hinzuzufügen:

    | Schlüssel | Wert |
    |---|---|
    | TestApp:Settings:BackgroundColor | white |
    | TestApp:Settings:FontSize | 24 |
    | TestApp:Settings:FontColor | black |
    | TestApp:Settings:Message | Daten aus Azure App Configuration |

    Lassen Sie **Bezeichnung** und **Inhaltstyp** vorerst leer.
