---
title: 'Tutorial: Erkennen und Umranden von Gesichtern in einem Bild – Gesichtserkennungs-API, Java für Android'
titleSuffix: Azure Cognitive Services
description: In diesem Tutorial erstellen Sie eine einfache Android-App, die mithilfe der Gesichtserkennungs-API Gesichter in einem Bild erkennt und umrandet.
services: cognitive-services
author: noellelacharite
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: tutorial
ms.date: 07/12/2018
ms.author: nolachar
ms.openlocfilehash: 50185588b77f01bced9462d5fd1ad67bb5de6e08
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "46129725"
---
# <a name="tutorial-create-an-android-app-to-detect-and-frame-faces-in-an-image"></a>Tutorial: Erstellen einer Android-App zum Erkennen und Umranden von Gesichtern in einem Bild

In diesem Tutorial erstellen Sie eine einfache Android-Anwendung, bei der die Java-Klassenbibliothek für die Gesichtserkennung genutzt wird, um in einem Bild Gesichter von Menschen zu erkennen. In der Anwendung wird ein ausgewähltes Bild angezeigt, und jedes erkannte Gesicht ist mit einem Rechteck umrandet. Der vollständige Beispielcode ist auf GitHub unter [Detect and frame faces in an image on Android](https://github.com/Azure-Samples/cognitive-services-face-android-sample) (Erkennen und Umranden von Gesichtern in einem Bild unter Android) verfügbar.

![Android-Screenshot: Foto mit Gesichtern, die mit einem roten Rechteck umrandet sind](../Images/android_getstarted2.1.PNG)

Dieses Tutorial veranschaulicht folgende Vorgehensweisen:

> [!div class="checklist"]
> - Erstellen einer Android-Anwendung
> - Installieren der Clientbibliothek für den Gesichtserkennungsdienst
> - Verwenden der Clientbibliothek zum Erkennen von Gesichtern in einem Bild
> - Zeichnen eines Rahmens um jedes erkannte Gesicht

## <a name="prerequisites"></a>Voraussetzungen

- Zum Ausführen des Beispiels benötigen Sie einen Abonnementschlüssel. Über die Seite [Cognitive Services ausprobieren](https://azure.microsoft.com/try/cognitive-services/?api=face-api) können Sie Abonnementschlüssel für eine kostenlose Testversion abrufen.
- [Android Studio](https://developer.android.com/studio/), mindestens SDK 22 (für die Clientbibliothek zur Gesichtserkennung erforderlich).
- Die Clientbibliothek [com.microsoft.projectoxford:face:1.4.3](http://search.maven.org/#search%7Cga%7C1%7Cg%3A%22com.microsoft.projectoxford%22) zur Gesichtserkennung von Maven. Es ist nicht erforderlich, das Paket herunterzuladen. Unten ist die Installationsanleitung angegeben.

## <a name="create-the-project"></a>Erstellen des Projekts

Erstellen Sie Ihr Android-Anwendungsprojekt, indem Sie die folgenden Schritte ausführen:

1. Öffnen Sie Android Studio. In diesem Tutorial wird Android Studio 3.1 verwendet.
1. Wählen Sie **Start a new Android Studio project** (Neues Android Studio-Projekt starten).
1. Ändern Sie unter **Create Android Project** (Android-Projekt erstellen) die Standardfelder, falls dies erforderlich ist, und klicken Sie dann auf **Next** (Weiter).
1. Wählen Sie auf dem Bildschirm **Target Android Devices** (Android-Zielgeräte) im Dropdownmenü die Option **API 22** oder höher, und klicken Sie dann auf **Next** (Weiter).
1. Wählen Sie **Empty Activity** (Leere Aktivität), und klicken Sie auf **Next** (Weiter).
1. Deaktivieren Sie die Option **Backwards Compatibility** (Abwärtskompatibilität), und klicken Sie auf **Finish** (Fertig stellen).

## <a name="create-the-ui-for-selecting-and-displaying-the-image"></a>Erstellen der Benutzeroberfläche für die Auswahl und Anzeige des Bilds

Wenn Sie *activity_main.xml* öffnen, sollte der Layout-Editor angezeigt werden. Wählen Sie die Registerkarte **Text**, und ersetzen Sie den Inhalt durch den folgenden Code.

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <ImageView
        android:layout_width="match_parent"
        android:layout_height="fill_parent"
        android:id="@+id/imageView1"
        android:layout_above="@+id/button1"
        android:contentDescription="Image with faces to analyze"/>

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Browse for face image"
        android:id="@+id/button1"
        android:layout_alignParentBottom="true"/>
</RelativeLayout>
```

Öffnen Sie *MainActivity.java*, und ersetzen Sie bis auf die erste `package`-Anweisung alles durch den folgenden Code.

Mit dem Code wird ein Ereignishandler für `Button` festgelegt, über den eine neue Aktivität gestartet wird, damit der Benutzer ein Bild auswählen kann. Nach der Auswahl wird das Bild in `ImageView` angezeigt.

```java
import java.io.*;
import android.app.*;
import android.content.*;
import android.net.*;
import android.os.*;
import android.view.*;
import android.graphics.*;
import android.widget.*;
import android.provider.*;

public class MainActivity extends Activity {
    private final int PICK_IMAGE = 1;
    private ProgressDialog detectionProgressDialog;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            Button button1 = (Button)findViewById(R.id.button1);
            button1.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                Intent intent = new Intent(Intent.ACTION_GET_CONTENT);
                intent.setType("image/*");
                startActivityForResult(Intent.createChooser(
                        intent, "Select Picture"), PICK_IMAGE);
            }
        });

        detectionProgressDialog = new ProgressDialog(this);
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (requestCode == PICK_IMAGE && resultCode == RESULT_OK &&
                data != null && data.getData() != null) {
            Uri uri = data.getData();
            try {
                Bitmap bitmap = MediaStore.Images.Media.getBitmap(
                        getContentResolver(), uri);
                ImageView imageView = (ImageView) findViewById(R.id.imageView1);
                imageView.setImageBitmap(bitmap);

                // Uncomment
                //detectAndFrame(bitmap);
                } catch (IOException e) {
                    e.printStackTrace();
                }
        }
    }
}
```

Nun kann Ihre App nach einem Foto suchen und es in einem Fenster anzeigen (z.B. wie in der folgenden Abbildung).

![Android-Screenshot: Foto mit Gesichtern](../Images/android_getstarted1.1.PNG)

## <a name="configure-the-face-client-library"></a>Konfigurieren der Clientbibliothek für die Gesichtserkennung

Die Gesichtserkennungs-API ist eine Cloud-API, die Sie mithilfe von HTTP-Anforderungen aufrufen können. Um Ihnen die Arbeit zu erleichtern, wird in diesem Tutorial die Clientbibliothek für die Gesichtserkennung verwendet, die diese Webanforderungen kapselt.

Verwenden Sie im Bereich **Projekt** das Dropdownmenü, um **Android** auszuwählen. Erweitern Sie **Gradle Scripts** (Gradle-Skripts), und öffnen Sie anschließend *build.gradle (Module: app)* (build.gradle (Modul: App)).

Fügen Sie eine Abhängigkeit für die Clientbibliothek für die Gesichtserkennung (`com.microsoft.projectoxford:face:1.4.3`) hinzu, wie im Screenshot unten gezeigt. Klicken Sie anschließend auf **Sync Now** (Jetzt synchronisieren).

![Android Studio-Screenshot: App-Datei „build.gradle“](../Images/face-tut-java-gradle.png)

Öffnen Sie **MainActivity.java**, und fügen Sie die folgenden Import-Direktiven an:

```java
import com.microsoft.projectoxford.face.*;
import com.microsoft.projectoxford.face.contract.*;
```

## <a name="add-the-face-client-library-code"></a>Hinzufügen des Codes für die Clientbibliothek zur Gesichtserkennung

Fügen Sie in der `MainActivity`-Klasse oberhalb der `onCreate`-Methode den folgenden Code ein:

```java
private final String apiEndpoint = "<API endpoint>";
private final String subscriptionKey = "<Subscription Key>";

private final FaceServiceClient faceServiceClient =
        new FaceServiceRestClient(apiEndpoint, subscriptionKey);
```

Ersetzen Sie `<API endpoint>` durch den API-Endpunkt, der Ihrem Schlüssel zugewiesen wurde. Abonnementschlüssel für kostenlose Testversionen werden in der Region **westcentralus** generiert. Wenn Sie einen Abonnementschlüssel für kostenlose Testversionen nutzen, lautet die Anweisung also wie folgt:

```java
apiEndpoint = "https://westcentralus.api.cognitive.microsoft.com/face/v1.0";
```

Ersetzen Sie `<Subscription Key>` durch Ihren Abonnementschlüssel. Beispiel: 

```java
subscriptionKey = "0123456789abcdef0123456789ABCDEF"
```

Erweitern Sie im Bereich **Project** (Projekt) die Option **app** (App) und dann **manifests** (Manifeste), und öffnen Sie *AndroidManifest.xml*.

Fügen Sie das folgende Element als direkt untergeordnetes Element von `manifest` ein:

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

Führen Sie den Buildvorgang für Ihr Projekt durch, um es auf Fehler zu prüfen. Sie sind jetzt zum Aufrufen des Diensts für die Gesichtserkennung bereit.

## <a name="upload-an-image-to-detect-faces"></a>Hochladen eines Bilds zur Erkennung von Gesichtern

Die einfachste Möglichkeit zur Erkennung von Gesichtern ist das Aufrufen der `FaceServiceClient.detect`-Methode. Diese Methode umschließt die [Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)-API-Methode und gibt ein Array mit `Face`-Elementen zurück.

Die Position jedes zurückgegebenen `Face`-Elements wird mit einem Rechteck angegeben, und außerdem werden einige optionale Gesichtsmerkmale angezeigt. In diesem Beispiel sind nur die Positionen der Gesichter erforderlich.

Wenn ein Fehler auftritt, wird per `AlertDialog` die zugrunde liegende Ursache angezeigt.

Fügen Sie die folgenden Methoden in die `MainActivity`-Klasse ein.

```java
// Detect faces by uploading a face image.
// Frame faces after detection.
private void detectAndFrame(final Bitmap imageBitmap) {
    ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
    imageBitmap.compress(Bitmap.CompressFormat.JPEG, 100, outputStream);
    ByteArrayInputStream inputStream =
            new ByteArrayInputStream(outputStream.toByteArray());

    AsyncTask<InputStream, String, Face[]> detectTask =
            new AsyncTask<InputStream, String, Face[]>() {
                String exceptionMessage = "";

                @Override
                protected Face[] doInBackground(InputStream... params) {
                    try {
                        publishProgress("Detecting...");
                        Face[] result = faceServiceClient.detect(
                                params[0],
                                true,         // returnFaceId
                                false,        // returnFaceLandmarks
                                null          // returnFaceAttributes:
                                /* new FaceServiceClient.FaceAttributeType[] {
                                    FaceServiceClient.FaceAttributeType.Age,
                                    FaceServiceClient.FaceAttributeType.Gender }
                                */
                        );
                        if (result == null){
                            publishProgress(
                                    "Detection Finished. Nothing detected");
                            return null;
                        }
                        publishProgress(String.format(
                                "Detection Finished. %d face(s) detected",
                                result.length));
                        return result;
                    } catch (Exception e) {
                        exceptionMessage = String.format(
                                "Detection failed: %s", e.getMessage());
                        return null;
                    }
                }

                @Override
                protected void onPreExecute() {
                    //TODO: show progress dialog
                }
                @Override
                protected void onProgressUpdate(String... progress) {
                    //TODO: update progress
                }
                @Override
                protected void onPostExecute(Face[] result) {
                    //TODO: update face frames
                }
            };

    detectTask.execute(inputStream);
}

private void showError(String message) {
    new AlertDialog.Builder(this)
    .setTitle("Error")
    .setMessage(message)
    .setPositiveButton("OK", new DialogInterface.OnClickListener() {
            public void onClick(DialogInterface dialog, int id) {
        }})
    .create().show();
}
```

## <a name="frame-faces-in-the-image"></a>Umranden von Gesichtern im Bild

Fügen Sie die folgende Hilfsmethode in die `MainActivity`-Klasse ein. Mit dieser Methode wird um jedes erkannte Gesicht ein Rechteck gezeichnet.

```java
private static Bitmap drawFaceRectanglesOnBitmap(
        Bitmap originalBitmap, Face[] faces) {
    Bitmap bitmap = originalBitmap.copy(Bitmap.Config.ARGB_8888, true);
    Canvas canvas = new Canvas(bitmap);
    Paint paint = new Paint();
    paint.setAntiAlias(true);
    paint.setStyle(Paint.Style.STROKE);
    paint.setColor(Color.RED);
    paint.setStrokeWidth(10);
    if (faces != null) {
        for (Face face : faces) {
            FaceRectangle faceRectangle = face.faceRectangle;
            canvas.drawRect(
                    faceRectangle.left,
                    faceRectangle.top,
                    faceRectangle.left + faceRectangle.width,
                    faceRectangle.top + faceRectangle.height,
                    paint);
        }
    }
    return bitmap;
}
```

Führen Sie die `AsyncTask`-Methoden durch, die über die `TODO`-Kommentare in der `detectAndFrame`-Methode angegeben werden. Im Erfolgsfall wird das ausgewählte Bild mit den umrandeten Gesichtern unter `ImageView` angezeigt.

```java
@Override
protected void onPreExecute() {
    detectionProgressDialog.show();
}
@Override
protected void onProgressUpdate(String... progress) {
    detectionProgressDialog.setMessage(progress[0]);
}
@Override
protected void onPostExecute(Face[] result) {
    detectionProgressDialog.dismiss();
    if(!exceptionMessage.equals("")){
        showError(exceptionMessage);
    }
    if (result == null) return;
    ImageView imageView = findViewById(R.id.imageView1);
    imageView.setImageBitmap(
            drawFaceRectanglesOnBitmap(imageBitmap, result));
    imageBitmap.recycle();
}
```

Heben Sie abschließend in der `onActivityResult`-Methode für den Aufruf der `detectAndFrame`-Methode die Auskommentierung auf, wie unten gezeigt.

```java
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);

    if (requestCode == PICK_IMAGE && resultCode == RESULT_OK &&
                data != null && data.getData() != null) {
        Uri uri = data.getData();
        try {
            Bitmap bitmap = MediaStore.Images.Media.getBitmap(
                    getContentResolver(), uri);
            ImageView imageView = findViewById(R.id.imageView1);
            imageView.setImageBitmap(bitmap);

            // Uncomment
            detectAndFrame(bitmap);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

## <a name="run-the-app"></a>Ausführen der App

Führen Sie die Anwendung aus, und suchen Sie nach einem Bild mit einem Gesicht. Warten Sie einige Sekunden, damit der Gesichtserkennungsdienst reagieren kann. Das anschließende Ergebnis ähnelt der folgenden Abbildung:

![GettingStartAndroid](../Images/android_getstarted2.1.PNG)

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial wurden die Grundlagen zur Verwendung des Gesichtserkennungsdiensts beschrieben, und Sie haben eine Anwendung erstellt, mit der umrandete Gesichter in einem Bild angezeigt wurden.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über das Erkennen und Verwenden von Gesichtszügen.

> [!div class="nextstepaction"]
> [Gewusst wie: Gesichtserkennung in Bildern](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md)

Erkunden Sie die Gesichtserkennungs-APIs, die zum Erkennen von Gesichtern und der zugehörigen Attribute, z.B. Haltung, Geschlecht, Alter, Kopfhaltung, Gesichtsbehaarung und Brille, verwendet werden.

> [!div class="nextstepaction"]
> [Referenz zur Gesichtserkennungs-API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).