---
title: Includedatei
description: Includedatei
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/19/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: 0ed42e4ace17db1e681152589cc46d82c26dddff
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
ms.locfileid: "32202668"
---
## <a name="set-up-your-project"></a>Einrichten des Projekts

Möchten Sie stattdessen das Android Studio-Projekt dieses Beispiels herunterladen? [Laden Sie ein Projekt herunter](https://github.com/Azure-Samples/active-directory-android-native-v2/archive/master.zip), und fahren Sie mit dem [Konfigurationsschritt](#register-your-application) fort, um das Codebeispiel vor der Ausführung zu konfigurieren.

### <a name="create-a-new-project"></a>Erstellen eines neuen Projekts 
1.  Öffnen Sie Android Studio, und klicken Sie auf **File** > **New** > **New Project** (Datei > Neu > Neues Projekt).
2.  Geben Sie der Anwendung einen Namen, und klicken Sie dann auf **Next** (Weiter).
3.  Wählen Sie die **API 21 oder höher (Android 5.0)** aus, und klicken Sie auf **Next** (Weiter).
4.  Lassen Sie **Empty Activity** (Leere Aktivität) unverändert, und klicken Sie auf **Next** (Weiter) und anschließend auf **Finish** (Fertig stellen).


### <a name="add-msal-to-your-project"></a>Hinzufügen von MSAL zu Ihrem Projekt
1.  Wählen Sie in Android Studio **Gradle Scripts** > **build.gradle (Module: app)** (Gradle-Skripts > build.gradle (Module: app)).
2.  Fügen Sie unter **Dependencies** (Abhängigkeiten) den folgenden Code ein:

    ```ruby  
    compile ('com.microsoft.identity.client:msal:0.1.+') {
        exclude group: 'com.android.support', module: 'appcompat-v7'
    }
    compile 'com.android.volley:volley:1.0.0'
    ```

<!--start-collapse-->
### <a name="about-this-package"></a>Informationen zu diesem Paket

Das Paket im obigen Code installiert Microsoft Authentication Library (MSAL). MSAL übernimmt die Erfassung, Zwischenspeicherung und Aktualisierung von Benutzertoken für den Zugriff auf die durch den Azure Active Directory v2-Endpunkt geschützten APIs.
<!--end-collapse-->

## <a name="create-the-application-ui"></a>Erstellen der Anwendungsbenutzeroberfläche

1. Navigieren Sie zu **res** > **Layout**, und öffnen Sie die Datei **activity_main.xml**. 
2. Ändern Sie das Layout der Aktivität von `android.support.constraint.ConstraintLayout` o.ä. in `LinearLayout`.
3. Fügen Sie die Eigenschaft `android:orientation="vertical"` zum Knoten `LinearLayout` hinzu.
4. Fügen Sie den folgenden Code in den Knoten `LinearLayout` ein, wobei Sie den aktuellen Inhalt ersetzen:

    ```xml
    <TextView
        android:text="Welcome, "
        android:textColor="#3f3f3f"
        android:textSize="50px"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginLeft="10dp"
        android:layout_marginTop="15dp"
        android:id="@+id/welcome"
        android:visibility="invisible"/>

    <Button
        android:id="@+id/callGraph"
        android:text="Call Microsoft Graph"
        android:textColor="#FFFFFF"
        android:background="#00a1f1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="200dp"
        android:textAllCaps="false" />

    <TextView
        android:text="Getting Graph Data..."
        android:textColor="#3f3f3f"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginLeft="5dp"
        android:id="@+id/graphData"
        android:visibility="invisible"/>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="0dip"
        android:layout_weight="1"
        android:gravity="center|bottom"
        android:orientation="vertical" >

        <Button
            android:text="Sign Out"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginBottom="15dp"
            android:textColor="#FFFFFF"
            android:background="#00a1f1"
            android:textAllCaps="false"
            android:id="@+id/clearCache"
            android:visibility="invisible" />
    </LinearLayout>
    ```
