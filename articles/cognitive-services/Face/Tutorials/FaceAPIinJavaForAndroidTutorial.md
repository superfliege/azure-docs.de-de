---
title: Gesichtserkennungs-API – Java für Android-Tutorial | Microsoft-Dokumentation
titleSuffix: Microsoft Cognitive Services
description: Erstellen Sie eine einfache Android-App, die die Gesichtserkennungs-API von Cognitive Services verwendet, um menschliche Gesichter in einem Bild zu erkennen und zu umranden.
services: cognitive-services
author: SteveMSFT
manager: corncar
ms.service: cognitive-services
ms.component: face-api
ms.topic: article
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 5164a261d482d0cca3842a973d2109b17999bd25
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35377811"
---
# <a name="getting-started-with-face-api-in-java-for-android-tutorial"></a>Tutorial für erste Schritte mit der Gesichtserkennungs-API in Java für Android

In diesem Tutorial erfahren Sie, wie Sie eine einfache Android-App erstellen und entwickeln, die die Gesichtserkennungs-API enthält, um menschliche Gesichter in einem Bild zu erkennen. Die App stellt die Ergebnisse dar, indem sie die erkannten Gesichter umrandet.

![GettingStartedAndroid](../Images/android_getstarted2.1.PNG)

## <a name="preparation"></a> Vorbereitung

Für dieses Tutorial sind folgende Voraussetzungen erforderlich:

- Android Studio und SDK müssen installiert sein.
- Ein Android-Gerät (optional für Testzwecke).

## <a name="step1"></a>Schritt 1: Abonnieren der Gesichtserkennungs-API und Abrufen des Abonnementschlüssels

Bevor Sie eine Gesichtserkennungs-API verwenden, müssen Sie sich bei der Gesichtserkennungs-API im Microsoft Cognitive Services-Portal registrieren. Weitere Informationen finden Sie unter [Abonnements](https://azure.microsoft.com/try/cognitive-services/). Der Primär- und der Sekundärschlüssel können in diesem Tutorial verwendet werden.

## <a name="step2"></a>Schritt 2: Erstellen des Anwendungsframeworks

In diesem Schritt erstellen Sie ein Android-Anwendungsprojekt zum Implementieren der Standardbenutzeroberfläche zum Erfassen und Anzeigen des Bilds. Befolgen Sie die nachstehenden Anweisungen: 

1. Öffnen Sie Android Studio.
2. Klicken Sie im Menü „Datei“ auf **Neues Projekt...**.
3. Nennen Sie die Anwendung **MyFirstApp**, und klicken Sie anschließend auf „Weiter“. 

    ![GettingStartAndroidNewProject](../Images/AndroidNewProject.png)

4. Wählen Sie die gewünschte Zielplattform, und klicken Sie auf „Weiter“. 

    ![GettingStartAndroidNewProject2](../Images/AndroidNewProject2.png)

5. Wählen Sie eine **Basic Activity** (Standardaktivität), und klicken Sie dann auf „Weiter“.
6. Benennen Sie die Aktivität folgendermaßen, und klicken Sie auf „Fertig stellen“. 

    ![GettingStartAndroidNewProject4](../Images/AndroidNewProject4.png)

7. Wenn Sie **activity_main.xml** öffnen, sollte der Layout-Editor dieser Aktivität angezeigt werden.
8. Zeigen Sie den Text der Quelldatei an, und bearbeiten Sie das Aktivitätslayout folgendermaßen:

    ```xml
    <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:tools="http://schemas.android.com/tools" android:layout_width="match_parent"
        android:layout_height="match_parent" android:paddingLeft="@dimen/activity_horizontal_margin"
        android:paddingRight="@dimen/activity_horizontal_margin"
        android:paddingTop="@dimen/activity_vertical_margin"
        android:paddingBottom="@dimen/activity_vertical_margin" tools:context=".MainActivity">
     
        <ImageView
            android:layout_width="match_parent"
            android:layout_height="fill_parent"
            android:id="@+id/imageView1"
            android:layout_above="@+id/button1" />
    
        <Button
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Browse"
            android:id="@+id/button1"
            android:layout_alignParentBottom="true" />
    </RelativeLayout>
    ```  

9. Öffnen Sie **MainActivity.java**, und fügen Sie folgende import-Anweisungen am Anfang der Datei hinzu:

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
    ```
      
    Ändern Sie anschließend die Klassen folgendermaßen:  
    
    ```java
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
                Intent gallIntent = new Intent(Intent.ACTION_GET_CONTENT);
                gallIntent.setType("image/*");
                startActivityForResult(Intent.createChooser(gallIntent, "Select Picture"), PICK_IMAGE);
            }
        });
         
        detectionProgressDialog = new ProgressDialog(this);
    }
    
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (requestCode == PICK_IMAGE && resultCode == RESULT_OK && data != null && data.getData() != null) {
            Uri uri = data.getData();
            try {
                Bitmap bitmap = MediaStore.Images.Media.getBitmap(getContentResolver(), uri);
                ImageView imageView = (ImageView) findViewById(R.id.imageView1);
                imageView.setImageBitmap(bitmap);
                } catch (IOException e) {
                e.printStackTrace();
                }
        }
    }
    ```

Nun kann Ihre App ein Foto in der Galerie suchen und es in einem Fenster anzeigen, das folgendem Bild ähnelt:

![GettingStartAndroidUI](../Images/android_getstarted1.1.PNG)

## <a name="step3"></a>Schritt 3: Konfigurieren der Clientbibliothek der Gesichtserkennungs-API

Die Gesichtserkennungs-API ist eine Cloud-API, die Sie mithilfe von HTTP-Anforderungen aufrufen können. Damit Sie die Gesichtserkennungs-API einfacher in Anwendungen für die .NET-Plattform verwenden können, wird eine Clientbibliothek bereitgestellt, um die Webanforderungen zu kapseln. In diesem Beispiel wird die Clientbibliothek verwendet, um die Arbeit zu vereinfachen. 

Führen Sie folgende Anweisungen durch, um die Clientbibliothek zu konfigurieren: 

1. Suchen Sie die **build.gradle**-Datei auf oberster Ebene Ihres Projekts im Bereich „Projekt“, der in diesem Beispiel dargestellt wird. Beachten Sie, dass es mehrere andere **build.gradle**-Dateien in der Projektstruktur gibt, und dass Sie zuerst die **build.gradle** auf oberster Ebene öffnen müssen.
2. Fügen Sie **mavenCentral()** den Repositorys Ihres Projekts hinzu. Sie können ebenfalls jcenter() verwenden. Dies ist das Standardrepository von Android Studio, da jcenter() mavenCentral() übergeordnet ist.  

```
    allprojects {
        repositories {
            ...
            mavenCentral()
        }
    }
```

3. Öffnen Sie die Datei **build.gradle** im Projekt „app“.
4. Fügen Sie eine Abhängigkeit für die Clientbibliothek hinzu, die im zentralen Maven-Repository gespeichert ist:

```
    dependencies {  
        ...  
        implementation 'com.microsoft.projectoxford:face:1.4.3'  
    }
```

5. Öffnen Sie **MainActivity.java** im Projekt „app“, und fügen Sie folgende import-Anweisungen hinzu: 
    
    ```java
    import com.microsoft.projectoxford.face.*;  
    import com.microsoft.projectoxford.face.contract.*;  
    ```
    
   Fügen Sie anschließend folgenden Code zur Klasse hinzu:

    ```java
    private FaceServiceClient faceServiceClient = new FaceServiceRestClient("your API endpoint", "<Subscription Key>");
    ```

   Ersetzen Sie den ersten Parameter oben durch den API-Endpunkt, der Ihrem Schlüssel in Schritt 1 zugewiesen wurde. Beispiel: 
   
        https://eastus2.api.cognitive.microsoft.com/face/v1.0
   
   Ersetzen Sie den zweiten Parameter durch den Abonnementschlüssel, den Sie in Schritt 1 abgerufen haben.
   
6. Öffnen Sie die Datei **AndroidManifest.xml** im Projekt „app“. Fügen Sie folgendes Element als untergeordnetes Element von **manifest** hinzu:  

    ```xml
    <uses-permission android:name="android.permission.INTERNET" />  
    ```

7. Sie können nun die Gesichtserkennungs-API über Ihre App aufrufen. 

## <a name="step4"></a>Schritt 4: Hochladen von Bildern für die Gesichtserkennung

Die einfachste Möglichkeit zum Erkennen von Gesichtern ist das Aufrufen der [Gesichtserkennungs-API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), indem die Bilddatei direkt hochgeladen wird. Wenn Sie die Clientbibliothek verwenden, kann dies mithilfe der asynchronen Methode **DetectAsync** der Klasse **FaceServiceClient** erfolgen. Die Position jedes erkannten Gesichts wird mit einem Rechteck angegeben, außerdem werden einige optionale Gesichtsmerkmale angezeigt. In diesem Beispiel muss nur die Position des Gesichts abgerufen werden. Für die Gesichtserkennung muss eine Methode zur **MainActivity**-Klasse hinzugefügt werden: 

```java

    // Detect faces by uploading face images
    // Frame faces after detection
    
    private void detectAndFrame(final Bitmap imageBitmap)
    {
        ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
        imageBitmap.compress(Bitmap.CompressFormat.JPEG, 100, outputStream);
        ByteArrayInputStream inputStream = 
            new ByteArrayInputStream(outputStream.toByteArray());
        AsyncTask<InputStream, String, Face[]> detectTask =
            new AsyncTask<InputStream, String, Face[]>() {
                @Override
                protected Face[] doInBackground(InputStream... params) {
                    try {
                        publishProgress("Detecting...");
                        Face[] result = faceServiceClient.detect(
                                params[0], 
                                true,         // returnFaceId
                                false,        // returnFaceLandmarks
                                null           // returnFaceAttributes: a string like "age, gender"
                /* If you want value of FaceAttributes, try adding 4th argument like below.
                            new FaceServiceClient.FaceAttributeType[] {
                    FaceServiceClient.FaceAttributeType.Age,
                    FaceServiceClient.FaceAttributeType.Gender }
                */              
                        );
                        if (result == null)
                        {
                            publishProgress("Detection Finished. Nothing detected");
                            return null;
                        }
                        publishProgress(
                                String.format("Detection Finished. %d face(s) detected",
                                        result.length));
                        return result;
                    } catch (Exception e) {
                        publishProgress("Detection failed");
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
```

## <a name="step5"></a>Schritt 5: Markieren von Gesichtern im Bild

In diesem letzten Schritt werden alle oben genannten Schritte kombiniert, und die erkannten Gesichter werden mit Rahmen im Bild markiert. Öffnen Sie zunächst **MainActivity.java**, und fügen Sie eine Hilfsmethode hinzu, die Rechtecke zeichnet: 

```java
    private static Bitmap drawFaceRectanglesOnBitmap(Bitmap originalBitmap, Face[] faces) {
        Bitmap bitmap = originalBitmap.copy(Bitmap.Config.ARGB_8888, true);
        Canvas canvas = new Canvas(bitmap);
        Paint paint = new Paint();
        paint.setAntiAlias(true);
        paint.setStyle(Paint.Style.STROKE);
        paint.setColor(Color.RED);
        int stokeWidth = 2;
        paint.setStrokeWidth(stokeWidth);
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

Vervollständigen sie nun die TODO-Abschnitte in der Methode **detectAndFrame**, um Gesichter zu umranden und den Status zu melden.

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
        if (result == null) return;
        ImageView imageView = (ImageView)findViewById(R.id.imageView1);
        imageView.setImageBitmap(drawFaceRectanglesOnBitmap(imageBitmap, result));
        imageBitmap.recycle();
    }
```
 
Fügen Sie zuletzt wie im Folgenden dargestellt einen Aufruf von der **onActivityResult**-Methode zur **detectAndFrame**-Methode hinzu. 

```java
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (requestCode == PICK_IMAGE && resultCode == RESULT_OK && data != null && data.getData() != null) {
            Uri uri = data.getData();
            try {
                Bitmap bitmap = MediaStore.Images.Media.getBitmap(getContentResolver(), uri);
                ImageView imageView = (ImageView) findViewById(R.id.imageView1);
                imageView.setImageBitmap(bitmap);
     
                // This is the new addition.
                // detectAndFrame(bitmap);
     
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
```

Führen Sie diese App aus, und suchen Sie nach einem Bild mit einem Gesicht. Warten Sie einige Sekunden, damit die Cloud-API reagieren kann. Das anschließende Ergebnis ähnelt folgendem Bild: 

![GettingStartAndroid](../Images/android_getstarted2.1.PNG)

## <a name="summary"></a> Zusammenfassung

In diesem Tutorial haben Sie die Grundlagen zur Verwendung der Gesichtserkennungs-API gelernt und eine App erstellt, die die Markierungen von Gesichtern in Bildern anzeigt. Weitere Informationen zur Gesichtserkennungs-API finden Sie in den exemplarischen Vorgehensweisen und in der [API-Referenz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236). 

## <a name="related"></a> Zugehörige Tutorials

- [Tutorial für erste Schritte mit der Gesichtserkennungs-API in C#](FaceAPIinCSharpTutorial.md)
- [Tutorial für erste Schritte mit der Gesichtserkennungs-API in Python](FaceAPIinPythonTutorial.md)
