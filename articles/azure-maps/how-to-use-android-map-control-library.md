---
title: Verwenden des Android Maps-Kartensteuerelements in Azure Maps | Microsoft-Dokumentation
description: Verwenden des Android-Kartensteuerelements in Azure Maps
author: walsehgal
ms.author: v-musehg
ms.date: 02/12/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: e3f7579324e1218cc2e2c3594889db776da6e529
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2019
ms.locfileid: "56119067"
---
# <a name="how-to-use-azure-maps-android-sdk"></a>Verwenden des Azure Maps Android SDK

Das Azure Maps Android SDK ist eine Vektorenzuordnungsbibliothek für Android. Dieser Artikel führt Sie durch den Prozess der Installation des Azure Maps Android SDK, das Laden einer Karte und das Platzieren eines Pins.

## <a name="prerequisites-to-get-started"></a>Voraussetzungen für den Einstieg

### <a name="create-an-azure-maps-account"></a>Erstellen eines Azure Maps-Kontos 

Um die Schritte in diesem Leitfaden auszuführen, müssen Sie zunächst [Verwalten von Konten und Schlüsseln](how-to-manage-account-keys.md) lesen, um Ihr Kontoabonnement mit S1-Tarif zu erstellen und zu verwalten.

### <a name="download-android-studio"></a>Herunterladen von Android Studio

Sie können [Android Studio](https://developer.android.com/studio/) kostenlos bei Google herunterladen. Um das Azure Maps Android SDK zu installieren, müssen Sie zunächst Android Studio herunterladen und ein Projekt mit einer leeren Aktivität erstellen.

## <a name="create-a-project-in-android-studio"></a>Erstellen eines Projekts in Android Studio

Sie müssen ein neues Projekt mit einer leeren Aktivität erstellen. Führen Sie die folgenden Schritte aus, um ein neues Android Studio-Projekt zu erstellen:

1. Aktivieren Sie *Ihr Projekt auswählen* „Telefon und Tablet“ als Formfaktor, auf dem Ihre Anwendung ausgeführt werden soll.
2. Klicken Sie auf *Leere Aktivität* unter „Formfaktoren“ und klicken Sie auf **Weiter**.
3. Wählen Sie unter *Ihr Projekt konfigurieren* die Option `API 21: Android 5.0.0 (Lollipop)` als das minimale SDK aus. Dies ist die niedrigste Version, die von Azure Maps Android SDK unterstützt wird.
4. Übernehmen Sie den Standardwert `Activity Name` und `Layout Name`, und klicken Sie auf **Fertig stellen**.

Weitere Informationen zur Installation von Android Studio und zur Erstellung eines neuen Projekts finden Sie in der Dokumentation zu [Android Studio](https://developer.android.com/studio/intro/).

![Ein neues Projekt erstellen](./media/how-to-use-android-map-control-library/form-factor-android.png)

## <a name="set-up-a-virtual-device"></a>Einrichten eines virtuellen Geräts

Mit Android Studio können Sie ein virtuelles Android-Gerät auf Ihrem Computer einrichten. Damit können Sie Ihre Anwendung während der Entwicklung testen. Um ein virtuelles Gerät einzurichten, klicken Sie auf das Symbol „Android Virtual Device (AVD)-Manager“ oben rechts auf Ihrem Projektbildschirm. Klicken Sie dann auf die Schaltfläche **Virtuelles Gerät erstellen**. Außerdem können Sie den Manager über „Tools > Android > AVD-Manager“ in der Symbolleiste aufrufen. Wählen Sie in der **Smartphones** die Option **Nexus 5 X**, und klicken Sie auf **Weiter**.

Weitere Informationen zum Einrichten eines AVD finden Sie in der Dokumentation zu [Android Studio](https://developer.android.com/studio/run/managing-avds).

![Android-Emulator](./media/how-to-use-android-map-control-library/android-emulator.png)

## <a name="install-azure-maps-android-sdk"></a>Installieren des Azure Maps Android SDK

Bevor Sie mit der Erstellung Ihrer Anwendung fortfahren, führen Sie die folgenden Schritte aus, um das Azure Maps Android SDK zu installieren. 

1. Fügen Sie Folgendes zum Repositoryblock **allprojects** in Ihrer Datei **build.gradle** hinzu.

    ```
    maven {
            url "https://atlas.microsoft.com/sdk/android"
    }
    ```

2. Aktualisieren Sie **app/build.gradle**, und fügen Sie Folgendes hinzu:

    1. Fügen Sie Folgendes zum Android-Block hinzu:

        ```
        compileOptions {
            sourceCompatibility JavaVersion.VERSION_1_8
            targetCompatibility JavaVersion.VERSION_1_8
        }
        ```
    2. Aktualisieren Sie Ihren Abhängigkeitsblock und fügen Sie Folgendes hinzu:

        ```
        implementation "com.microsoft.azure.maps:mapcontrol:0.1"
        ```

3. Richten Sie Berechtigungen ein, indem Sie Folgendes zu Ihrer AndroidManifest.xml hinzufügen.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <manifest>
        ...
        <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
        ...
    </manifest>
    ```

4. Bearbeiten Sie **Res > Layout > „activity_main.xml“**, sodass es wie der folgende XML-Code aussieht:
    
    ```XML
    <?xml version="1.0" encoding="utf-8"?>
    <FrameLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        >

        <com.microsoft.azure.maps.mapcontrol.MapControl
            android:id="@+id/mapcontrol"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:mapcontrol_cameraTargetLat="47.64"
            app:mapcontrol_cameraTargetLng="-122.33"
            app:mapcontrol_cameraZoom="12"
            />

    </FrameLayout>
    ```

5. Bearbeiten Sie **MainActivity.java**, um eine Aktivitätsklasse für die Kartenansicht zu erstellen. Nach dem Bearbeiten sollte die Ansicht wie unten dargestellt aussehen:

    ```java
    package com.example.myapplication;

    import android.support.v7.app.AppCompatActivity;
    import android.os.Bundle;
    import com.microsoft.azure.maps.mapcontrol.AzureMaps;
    import com.microsoft.azure.maps.mapcontrol.MapControl;
    import com.microsoft.azure.maps.mapcontrol.layer.SymbolLayer;
    import com.microsoft.azure.maps.mapcontrol.options.MapStyle;
    import com.microsoft.azure.maps.mapcontrol.source.DataSource;

    public class MainActivity extends AppCompatActivity {
        
        static{
            AzureMaps.setSubscriptionKey("{subscription-key}");
        }

        MapControl mapControl;

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);

            mapControl = findViewById(R.id.mapcontrol);

            mapControl.onCreate(savedInstanceState);

        }

        @Override
        public void onResume() {
            super.onResume();
            mapControl.onResume();
        }

        @Override
        public void onPause() {
            super.onPause();
            mapControl.onPause();
        }

        @Override
        public void onStop() {
            super.onStop();
            mapControl.onStop();
        }

        @Override
        public void onLowMemory() {
            super.onLowMemory();
            mapControl.onLowMemory();
        }

        @Override
        protected void onDestroy() {
            super.onDestroy();
            mapControl.onDestroy();
        }

        @Override
        protected void onSaveInstanceState(Bundle outState) {
            super.onSaveInstanceState(outState);
            mapControl.onSaveInstanceState(outState);
        }

    }

    ```

## <a name="import-classes"></a>Importieren von Klassen

Nachdem Sie die obigen Schritte ausgeführt haben, werden Sie höchstwahrscheinlich Warnungen von Android Studio zu einem Teil des Textes im Code erhalten. Um diese zu bearbeiten, müssen Sie einige der Klassen importieren, die in `MainActivity.java` referenziert werden.

Sie können diese Klassen automatisch importieren, indem Sie `Alt`+`Enter` (`Option`+`Return` auf Mac) drücken. 

Klicken Sie auf die Schaltfläche **App ausführen** (oder `Control`+`R` auf einem Mac), um Ihre Anwendung zu erstellen.

![Klicken Sie auf „Ausführen“.](./media/how-to-use-android-map-control-library/run-app.png)

Es dauert ein paar Sekunden, bis Android Studio die Anwendung erstellt hat. Nach Abschluss des Builds können Sie Ihre Anwendung auf dem emulierten Android-Gerät testen. Es wird eine Zuordnung wie die folgende angezeigt.

![Android-Zuordnung](./media/how-to-use-android-map-control-library/android-map.png)

## <a name="add-a-marker-to-the-map"></a>Hinzufügen eines Markers zur Karte

Um einen Marker zu Ihrer Karte hinzuzufügen, fügen Sie der Funktion `mapView.getMapAsync()` zur `MainActivity.java` hinzu. Die finale `MainActivity.java` sollte folgendermaßen aussehen:

```java
package com.example.myapplication;

import android.app.Activity;
import android.os.Bundle;
import com.mapbox.geojson.Feature;
import com.mapbox.geojson.Point;
import com.microsoft.azure.maps.mapcontrol.AzureMaps;
import com.microsoft.azure.maps.mapcontrol.MapControl;
import com.microsoft.azure.maps.mapcontrol.layer.SymbolLayer;
import com.microsoft.azure.maps.mapcontrol.source.DataSource;
import static com.microsoft.azure.maps.mapcontrol.options.SymbolLayerOptions.iconImage;
public class MainActivity extends AppCompatActivity {
    
    static{
            AzureMaps.setSubscriptionKey("{subscription-key}");
        }

    MapControl mapControl;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        mapControl = findViewById(R.id.mapcontrol);

        mapControl.onCreate(savedInstanceState);

        mapControl.getMapAsync(map -> {
            DataSource dataSource = new DataSource();
            dataSource.add(Feature.fromGeometry(Point.fromLngLat(-122.33, 47.64)));

            SymbolLayer symbolLayer = new SymbolLayer(dataSource);
            symbolLayer.setOptions(iconImage("my-icon"));

            map.images.add("my-icon", R.drawable.mapcontrol_marker_red);
            map.sources.add(dataSource);
            map.layers.add(symbolLayer);
        });
    }

    @Override
    public void onStart() {
        super.onStart();
        mapControl.onStart();
    }

    @Override
    public void onResume() {
        super.onResume();
        mapControl.onResume();
    }

    @Override
    public void onPause() {
        super.onPause();
        mapControl.onPause();
    }

    @Override
    public void onStop() {
        super.onStop();
        mapControl.onStop();
    }

    @Override
    public void onLowMemory() {
        super.onLowMemory();
        mapControl.onLowMemory();
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        mapControl.onDestroy();
    }

    @Override
    protected void onSaveInstanceState(Bundle outState) {
        super.onSaveInstanceState(outState);
        mapControl.onSaveInstanceState(outState);
    }
}
```

Führen Sie die Anwendung erneut aus. Sie sollten jetzt einen Marker wie den folgenden auf der Karte sehen.

![Android-Kartenpin](./media/how-to-use-android-map-control-library/android-map-pin.png)