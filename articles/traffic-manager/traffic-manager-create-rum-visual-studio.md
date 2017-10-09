---
title: Senden von Benutzer-Realmessungen an Azure Traffic Manager mit Visual Studio Mobile Center | Microsoft-Dokumentation
description: Richten Sie Ihre mobile Anwendung, die mithilfe von Visual Studio Mobile Center entwickelt wurde, so ein, dass Benutzer-Realmessungen an Traffic Manager gesendet werden.
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
ms.date: 09/29/2017
ms.author: kumud
ms.custom: 
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: fcefa16112e29c897d72ade80bc612b7765c578d
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="how-to-send-real-user-measurements-to-traffic-manager-with-visual-studio-mobile-center"></a>Senden von Benutzer-Realmessungen an Azure Traffic Manager mit Visual Studio Mobile Center

>[!NOTE]
>Das Feature „Benutzer-Realmessungen“ in Traffic Manager befindet sich in der öffentlichen Vorschauphase (Public Preview) und ist unter Umständen nicht so verfügbar und zuverlässig wie Features in Versionen mit allgemeiner Verfügbarkeit. Das Feature wird nicht unterstützt, bietet möglicherweise eingeschränkte Funktionen und ist vielleicht nicht an allen Azure-Standorten verfügbar. Aktuelle Hinweise zur Verfügbarkeit und zum Status dieses Features finden Sie auf der Seite mit den [Azure Traffic Manager-Updates](https://azure.microsoft.com/updates/?product=traffic-manager).

Sie können Ihre mobile Anwendung, die mithilfe von Visual Studio Mobile Center entwickelt wurde, so einrichten, dass Benutzer-Realmessungen an Traffic Manager gesendet werden. Gehen Sie dazu so vor:

>[!NOTE]
> Derzeit wird das Senden von Benutzer-Realmessungen an Traffic Manager nur für Android unterstützt.

Zum Konfigurieren von Benutzer-Realmessungen müssen Sie einen Schlüssel abrufen und Ihre App mit dem RUM-Paket instrumentieren.

## <a name="step-1-obtain-a-key"></a>Schritt 1: Abrufen eines Schlüssels
    
Die von Ihrer Clientanwendung erfassten und an Traffic Manager gesendeten Messungen werden vom Dienst mithilfe einer eindeutigen Zeichenfolge bestimmt, die „RUM-Schlüssel“ (Real User Measurements, Benutzer-Realmessungen) genannt wird. Sie können einen RUM-Schlüssel über das Azure-Portal, eine REST-API oder mithilfe von PowerShell oder Azure CLI abrufen.

Gehen Sie wie folgt vor, um den RUM-Schlüssel über das Azure-Portal abzurufen:
   1. Melden Sie sich im Browser beim Azure-Portal an. Falls Sie noch nicht über ein Konto verfügen, können Sie sich für eine kostenlose einmonatige Testversion registrieren.
   2. Suchen Sie über die Suchleiste des Portals nach dem Namen des Traffic Manager-Profils, das Sie ändern möchten, und klicken Sie in den angezeigten Ergebnissen auf das Traffic Manager-Profil.
   3. Klicken Sie auf der Seite „Traffic Manager-Profil“ unter **Einstellungen** auf **Benutzer-Realmessungen**.
   4. Klicken Sie auf **Schlüssel generieren**, um einen neuen RUM-Schlüssel zu generieren.
        
   ![Erstellen eines Schlüssels für Benutzer-Realmessungen (RUM)](./media/traffic-manager-create-rum-visual-studio/generate-rum-key.png)

   **Abbildung 1: Erstellung eines Schlüssels für Benutzer-Realmessungen**

   5.   Die Seite zeigt nun den generierten RUM Key und einen JavaScript-Codeausschnitt, der in Ihre HTML-Seite eingebettet werden muss.
 
   ![JavaScript-Code für den Schlüssel für Benutzer-Realmessungen](./media/traffic-manager-create-rum-visual-studio/rum-key.png)

   **Abbildung 2: Schlüssel für Benutzer-Realmessungen und JavaScript für Messung**
 
   6. Klicken Sie auf die Schaltfläche **Kopieren**, um den RUM-Schlüssel zu kopieren. 

## <a name="step-2-instrument-your-app-with-the-rum-package-of-mobile-center-sdk"></a>Schritt 2: Instrumentieren Ihrer App mit dem RUM-Paket im Mobile Center SDK

Wenn Sie noch nicht mit Visual Studio Mobile Center vertraut sind, besuchen Sie die [Website](https://mobile.azure.com). Ausführliche Anweisungen zur SDK-Integration finden Sie unter [Erste Schritte mit dem Android SDK](https://docs.microsoft.com/mobile-center/sdk/getting-started/Android).

Um Benutzer-Realmessungen zu verwenden, führen Sie die folgenden Schritte aus:

1.  Hinzufügen des SDK zum Projekt

    In der Vorschauversion des ATM RUM SDK müssen Sie explizit auf das Paketrepository verweisen.

    Fügen Sie der Datei **app/build.gradle** die folgenden Zeilen hinzu:

    ```groovy
    repositories {
        maven {
            url "https://dl.bintray.com/mobile-center/mobile-center-snapshot"
        }
    }
    ```
    Fügen Sie der Datei **app/build.gradle** die folgenden Zeilen hinzu:

    ```groovy
    dependencies {   
     
        def mobileCenterSdkVersion = '0.12.1-16+3fe5b08'
        compile "com.microsoft.azure.mobile:mobile-center-rum:${mobileCenterSdkVersion}"
    }
    ```

2. Starten des SDK

    Öffnen Sie die Hauptaktivitätsklasse Ihrer App, und fügen Sie die folgenden „import“-Anweisungen hinzu:

    ```java
    import com.microsoft.azure.mobile.MobileCenter;
    import com.microsoft.azure.mobile.rum.RealUserMeasurements;
    ```

    Suchen Sie nach dem `onCreate`-Rückruf in derselben Datei, und fügen Sie den folgenden Code hinzu:

    ```java
    RealUserMeasurements.setRumKey("<Your RUM Key>");
    MobileCenter.start(getApplication(), "<Your Mobile Center AppSecret>", RealUserMeasurements.class);
    ```

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über [Benutzer-Realmessungen](traffic-manager-rum-overview.md)
- Informieren Sie sich über die [Funktionsweise von Traffic Manager](traffic-manager-overview.md)
- Erfahren Sie mehr zu [Mobile Center](https://docs.microsoft.com/mobile-center/)
- [Registrieren](https://mobile.azure.com) für Mobile Center
- Informieren Sie sich über die von Traffic Manager unterstützten [Methoden für das Datenverkehrsrouting](traffic-manager-routing-methods.md) .
- Informationen zum [Erstellen eines Traffic Manager-Profils](traffic-manager-create-profile.md)


