---
title: Includedatei
description: Includedatei
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/13/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: 5518036d34a493558458673202e52e4559e49573
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48843494"
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

    ```gradle  
    compile ('com.microsoft.identity.client:msal:0.1.+') {
        exclude group: 'com.android.support', module: 'appcompat-v7'
    }
    compile 'com.android.volley:volley:1.0.0'
    ```

<!--start-collapse-->
### <a name="about-this-package"></a>Informationen zu diesem Paket

Das Paket im obigen Code installiert Microsoft Authentication Library (MSAL). Mit der MSAL werden alle Tokenvorgänge durchgeführt, z.B. Beschaffen, Zwischenspeichern, Aktualisieren und Löschen.  Die Token sind für den Zugriff auf die APIs erforderlich, die mit der Microsoft Identity Platform geschützt sind.
<!--end-collapse-->

## <a name="create-the-apps-ui"></a>Erstellen der App-Benutzeroberfläche

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
