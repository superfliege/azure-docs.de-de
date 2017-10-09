---
title: Benutzer-Realmessungen in Azure Traffic Manager mit Webseiten | Microsoft-Dokumentation
description: "Informationen zum Einrichten von Webseiten für das Senden von Benutzer-Realmessungen an Traffic Manager"
services: traffic-manager
documentationcenter: traffic-manager
author: KumudD
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 09/19/2017
ms.author: kumud
ms.custom: 
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: ce5d333ad5d339b64d1fc1d081651052f71a8b67
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="how-to-send-real-user-measurements-to-azure-traffic-manager-using-web-pages"></a>Senden von Benutzer-Realmessungen an Azure Traffic Manager mithilfe von Webseiten

>[!NOTE]
>Das Feature „Benutzer-Realmessungen“ in Traffic Manager befindet sich in der öffentlichen Vorschauphase (Public Preview) und ist unter Umständen nicht so verfügbar und zuverlässig wie Features in Versionen mit allgemeiner Verfügbarkeit. Das Feature wird nicht unterstützt, bietet möglicherweise eingeschränkte Funktionen und ist vielleicht nicht an allen Azure-Standorten verfügbar. Aktuelle Hinweise zur Verfügbarkeit und zum Status dieses Features finden Sie auf der Seite mit den [Azure Traffic Manager-Updates](https://azure.microsoft.com/updates/?product=traffic-manager).

Sie können Ihre Webseiten so konfigurieren, dass Benutzer-Realmessungen an Traffic Manager gesendet werden. Dazu müssen Sie einen entsprechenden Schlüssel abrufen und den generierten Code in eine Webseite einbetten.

## <a name="obtain-a-real-user-measurements-key"></a>Abrufen eines Schlüssels für Benutzer-Realmessungen (RUM)

Die von Ihrer Clientanwendung erfassten und an Traffic Manager gesendeten Messungen werden vom Dienst mithilfe einer eindeutigen Zeichenfolge bestimmt, die **RUM-Schlüssel** (Real User Measurements, Benutzer-Realmessungen) genannt wird. Sie können einen RUM-Schlüssel über das Azure-Portal, eine REST-API oder mithilfe von PowerShell oder Azure CLI abrufen.

So rufen Sie den RUM-Schlüssel im Azure-Portal ab
1. Melden Sie sich im Browser beim Azure-Portal an. Falls Sie noch nicht über ein Konto verfügen, können Sie sich für eine kostenlose einmonatige Testversion registrieren.
2. Suchen Sie über die Suchleiste des Portals nach dem Namen des Traffic Manager-Profils, das Sie ändern möchten, und klicken Sie in den angezeigten Ergebnissen auf das Traffic Manager-Profil.
3. Klicken Sie auf dem Blatt „Traffic Manager-Profil“ unter **Einstellungen** auf **Benutzer-Realmessungen**.
4. Klicken Sie auf **Schlüssel generieren**, um einen neuen RUM-Schlüssel zu generieren.
 
  ![Erstellen eines Schlüssels für Benutzer-Realmessungen (RUM)](./media/traffic-manager-create-rum-visual-studio/generate-rum-key.png)

   **Abbildung 1: Erstellung eines Schlüssels für Benutzer-Realmessungen**

5. Das Blatt zeigt nun den generierten RUM-Schlüssel und einen JavaScript-Codeausschnitt, der in Ihre HTML-Seite eingebettet werden muss.
 
    ![JavaScript-Code für den Schlüssel für Benutzer-Realmessungen](./media/traffic-manager-create-rum-web-pages/rum-javascript-code.png)

    **Abbildung 2: Schlüssel für Benutzer-Realmessungen und JavaScript für Messung**
 
6.  Klicken Sie auf die Schaltfläche **Kopieren**, um den JavaScript-Code zu kopieren. 

>[!IMPORTANT]
> Verwenden Sie das generierte JavaScript, damit Benutzer-Realmessungen ordnungsgemäß funktionieren. Änderungen an diesem Skript oder den Skripts, die von Benutzer-Realmessungen verwendet werden, können zu einem unvorhersehbarem Verhalten führen.

## <a name="embed-the-code-to-an-html-web-page"></a>Einbetten des Codes in eine HTML-Webseite

Nachdem Sie den RUM-Schlüssel erhalten haben, müssen Sie dieses kopierte JavaScript in eine HTML-Seite einbetten, die Ihre Endbenutzer besuchen. Das Editieren von HTML kann auf viele verschiedene Arten und mit verschiedenen Tools und Workflows erfolgen. Dieses Beispiel zeigt, wie Sie eine HTML-Seite aktualisieren, um dieses Skript hinzuzufügen. Sie können diese Anleitung an Ihren Workflow zur Verwaltung von HTML-Quellcode anpassen.

1.  Öffnen Sie die HTML-Seite in einem Texteditor.
2.  Fügen Sie den JavaScript-Code, den Sie im vorherigen Schritt kopiert haben, in den Abschnitt BODY des HTML-Codes ein (der kopierte Code befindet sich in den Zeile 8 und 9, siehe Abbildung 3).
 
    ![Einbetten des JavaScript-Codes in die Webseite für Benutzer-Realmessungen](./media/traffic-manager-create-rum-web-pages/rum-javascript-code.png)  

    **Abbildung 3: Einfacher HTML-Code mit eingebettetem JavaScript für Benutzer-Realmessungen**

3.  Speichern Sie die HTML-Datei.
4. Wenn diese Seite das nächste Mal in einem Webbrowser gerendert wird, wird das referenzierte JavaScript heruntergeladen. Das Skript führt die Mess- und Berichterstellungsvorgänge durch.


## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über [Benutzer-Realmessungen](traffic-manager-rum-overview.md)
- Informieren Sie sich über die [Funktionsweise von Traffic Manager](traffic-manager-overview.md)
- Informieren Sie sich über die von Traffic Manager unterstützten [Methoden für das Datenverkehrsrouting](traffic-manager-routing-methods.md) .
- Informationen zum [Erstellen eines Traffic Manager-Profils](traffic-manager-create-profile.md)


