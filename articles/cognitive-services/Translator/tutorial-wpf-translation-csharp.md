---
title: 'Tutorial: Schreiben einer WPF-Anwendung für die Textübersetzung mit C#'
titleSuffix: Azure Cognitive Services
description: In diesem Tutorial wird beschrieben, wie Sie die Textübersetzungs-API zum Übersetzen von Text, Abrufen einer lokalisierten Liste mit unterstützten Sprachen und für weitere Zwecke verwenden, indem Sie per C# eine WPF-Anwendung erstellen.
services: cognitive-services
author: noellelacharite
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: tutorial
ms.date: 07/20/2018
ms.author: nolachar
ms.openlocfilehash: 97660985b275bbe4384acb3fc92be8aaa0b57881
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "46123962"
---
# <a name="tutorial-write-a-wpf-application-for-translator-text-using-c35"></a>Tutorial: Schreiben einer WPF-Anwendung für die Textübersetzung per C&#35;

In diesem Tutorial erstellen Sie mit der Textübersetzungs-API (V3) – einer Microsoft Cognitive Services-Komponente in Azure – ein interaktives Textübersetzungstool. Sie lernen Folgendes:

> [!div class="checklist"]
> * Abrufen einer Liste mit den vom Dienst unterstützten Sprachen
> * Durchführen der Übersetzung eines vom Benutzer eingegebenen Texts aus einer Sprache in eine andere

In diese Anwendung sind noch zwei andere Komponenten von Microsoft Cognitive Services integriert:

|||
|-|-|
|[Textanalyse](https://azure.microsoft.com/services/cognitive-services/text-analytics/)|Ermöglicht optional die automatische Erkennung der Ausgangssprache des zu übersetzenden Texts.|
|[Bing-Rechtschreibprüfung](https://azure.microsoft.com/services/cognitive-services/spell-check/)|Dient zum Korrigieren von Schreibfehlern in englischem Ausgangstext, um das Übersetzungsergebnis zu verbessern.

![[Das ausgeführte Tutorialprogramm]](media/translator-text-csharp-session.png)

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen [Visual Studio 2017](https://www.visualstudio.com/downloads/), um diesen Code unter Windows ausführen zu können. (Die kostenlose Community Edition ist hierfür geeignet.)

Außerdem benötigen Sie Abonnementschlüssel für die drei Azure-Dienste, die in der Anwendung zum Einsatz kommen. Über das Azure-Dashboard können Sie einen Schlüssel für den Textübersetzungsdienst abrufen. Im Rahmen des verfügbaren kostenlosen Tarifs können Sie pro Monat bis zu zwei Millionen Zeichen kostenlos übersetzen lassen.

Für die Dienste „Textanalyse“ und „Bing-Rechtschreibprüfung“ stehen ebenfalls kostenlose Testversionen zur Verfügung, für die Sie sich auf der Seite [Cognitive Services ausprobieren](https://azure.microsoft.com/try/cognitive-services/) registrieren können. Über das Azure-Dashboard können Sie auch ein Abonnement für den jeweiligen Dienst erstellen. Für die Textanalyse steht ein kostenloser Tarif zur Verfügung.

Den Quellcode für dieses Tutorial finden Sie weiter unten. Fügen Sie Ihre Abonnementschlüssel in `MainWindow.xaml.cs` als Variable `TEXT_TRANSLATION_API_SUBSCRIPTION_KEY` usw. in den Quellcode ein.

> [!IMPORTANT]
> Der Textanalysedienst ist in verschiedenen Regionen verfügbar. Der URI im Quellcode dieses Tutorials befindet sich in der Region `westus`. Dies ist die Region für kostenlose Testversionen. Sollten Sie über ein Abonnement in einer anderen Region verfügen, passen Sie diesen URI entsprechend an.

## <a name="source-code"></a>Quellcode

Hier finden Sie den Quellcode für die Microsoft- Textübersetzungs-API. Kopieren Sie den Quellcode in die entsprechende Datei eines neuen WPF-Projekts in Visual Studio, um die App auszuführen.

### <a name="mainwindowxamlcs"></a>MainWindow.xaml.cs

Hierbei handelt es sich um die Code-Behind-Datei mit den Anwendungsfunktionen.

```csharp
using System;
using System.Windows;
using System.Net;
using System.Net.Http;
using System.IO;
using System.Collections.Generic;
using System.Linq;
using System.Text;

// NOTE: Add assembly references to System.Runtime.Serialization, System.Web, and System.Web.Extensions.

// NOTE: Install the Newtonsoft.Json NuGet package.
using Newtonsoft.Json;

namespace MSTranslatorTextDemo
{
    /// <summary>
    /// This WPF application demonstrates the use of the Microsoft Translator Text API to translate a brief text string from
    /// one language to another. The languages are selected from a drop-down menu. The text of the translation is displayed.
    /// The source language may optionally be automatically detected. English text is spell-checked.
    /// </summary>
    public partial class MainWindow : Window
    {
        // Translator text subscription key from Microsoft Azure dashboard
        const string TEXT_TRANSLATION_API_SUBSCRIPTION_KEY = "ENTER KEY HERE";
        const string TEXT_ANALYTICS_API_SUBSCRIPTION_KEY = "ENTER KEY HERE";
        const string BING_SPELL_CHECK_API_SUBSCRIPTION_KEY = "ENTER KEY HERE";

        public static readonly string TEXT_TRANSLATION_API_ENDPOINT = "https://api.cognitive.microsofttranslator.com/{0}?api-version=3.0";
        const string TEXT_ANALYTICS_API_ENDPOINT = "https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/";
        const string BING_SPELL_CHECK_API_ENDPOINT = "https://api.cognitive.microsoft.com/bing/v7.0/spellcheck/";

        private string[] languageCodes;     // array of language codes

        // Dictionary to map language code from friendly name (sorted case-insensitively on language name)
        private SortedDictionary<string, string> languageCodesAndTitles =
            new SortedDictionary<string, string>(Comparer<string>.Create((a, b) => string.Compare(a, b, true)));

        public MainWindow()
        {
            // at least show an error dialog if there's an unexpected error
            AppDomain.CurrentDomain.UnhandledException += new UnhandledExceptionEventHandler(HandleExceptions);

            if (TEXT_TRANSLATION_API_SUBSCRIPTION_KEY.Length != 32
                || TEXT_ANALYTICS_API_SUBSCRIPTION_KEY.Length != 32
                || BING_SPELL_CHECK_API_SUBSCRIPTION_KEY.Length != 32)
            {
                MessageBox.Show("One or more invalid API subscription keys.\n\n" +
                    "Put your keys in the *_API_SUBSCRIPTION_KEY variables in MainWindow.xaml.cs.",
                    "Invalid Subscription Key(s)", MessageBoxButton.OK, MessageBoxImage.Error);
                System.Windows.Application.Current.Shutdown();
            }
            else
            {
                InitializeComponent();          // start the GUI
                GetLanguagesForTranslate();     // get codes and friendly names of languages that can be translated
                PopulateLanguageMenus();        // fill the drop-down language lists
            }
        }

        // Global exception handler to display error message and exit
        private static void HandleExceptions(object sender, UnhandledExceptionEventArgs args)
        {
            Exception e = (Exception)args.ExceptionObject;
            MessageBox.Show("Caught " + e.Message, "Error", MessageBoxButton.OK, MessageBoxImage.Error);
            System.Windows.Application.Current.Shutdown();
        }

        // ***** POPULATE LANGUAGE MENUS
        private void PopulateLanguageMenus()
        {
            // Add option to automatically detect the source language
            FromLanguageComboBox.Items.Add("Detect");

            int count = languageCodesAndTitles.Count;
            foreach (string menuItem in languageCodesAndTitles.Keys)
            {
                FromLanguageComboBox.Items.Add(menuItem);
                ToLanguageComboBox.Items.Add(menuItem);
            }

            // set default languages
            FromLanguageComboBox.SelectedItem = "Detect";
            ToLanguageComboBox.SelectedItem = "English";
        }

        // ***** DETECT LANGUAGE OF TEXT TO BE TRANSLATED
        private string DetectLanguage(string text)
        {
            string uri = TEXT_ANALYTICS_API_ENDPOINT + "languages?numberOfLanguagesToDetect=1";

            // create request to Text Analytics API
            HttpWebRequest detectLanguageWebRequest = (HttpWebRequest)WebRequest.Create(uri);
            detectLanguageWebRequest.Headers.Add("Ocp-Apim-Subscription-Key", TEXT_ANALYTICS_API_SUBSCRIPTION_KEY);
            detectLanguageWebRequest.Method = "POST";

            // create and send body of request
            var serializer = new System.Web.Script.Serialization.JavaScriptSerializer();
            string jsonText = serializer.Serialize(text);

            string body = "{ \"documents\": [ { \"id\": \"0\", \"text\": " + jsonText + "} ] }";
            byte[] data = Encoding.UTF8.GetBytes(body);
            detectLanguageWebRequest.ContentLength = data.Length;

            using (var requestStream = detectLanguageWebRequest.GetRequestStream())
                requestStream.Write(data, 0, data.Length);

            HttpWebResponse response = (HttpWebResponse)detectLanguageWebRequest.GetResponse();

            // read and parse JSON response
            var responseStream = response.GetResponseStream();
            var jsonString = new StreamReader(responseStream, Encoding.GetEncoding("utf-8")).ReadToEnd();
            dynamic jsonResponse = serializer.DeserializeObject(jsonString);

            // fish out the detected language code
            var languageInfo = jsonResponse["documents"][0]["detectedLanguages"][0];
            if (languageInfo["score"] > (decimal)0.5)
                return languageInfo["iso6391Name"];
            else
                return "";
        }

        // ***** CORRECT SPELLING OF TEXT TO BE TRANSLATED
        private string CorrectSpelling(string text)
        {
            string uri = BING_SPELL_CHECK_API_ENDPOINT + "?mode=spell&mkt=en-US";

            // create request to Bing Spell Check API
            HttpWebRequest spellCheckWebRequest = (HttpWebRequest)WebRequest.Create(uri);
            spellCheckWebRequest.Headers.Add("Ocp-Apim-Subscription-Key", BING_SPELL_CHECK_API_SUBSCRIPTION_KEY);
            spellCheckWebRequest.Method = "POST";
            spellCheckWebRequest.ContentType = "application/x-www-form-urlencoded"; // doesn't work without this

            // create and send body of request
            string body = "text=" + System.Web.HttpUtility.UrlEncode(text);
            byte[] data = Encoding.UTF8.GetBytes(body);
            spellCheckWebRequest.ContentLength = data.Length;
            using (var requestStream = spellCheckWebRequest.GetRequestStream())
                requestStream.Write(data, 0, data.Length);
            HttpWebResponse response = (HttpWebResponse)spellCheckWebRequest.GetResponse();

            // read and parse JSON response and get spelling corrections
            var serializer = new System.Web.Script.Serialization.JavaScriptSerializer();
            var responseStream = response.GetResponseStream();
            var jsonString = new StreamReader(responseStream, Encoding.GetEncoding("utf-8")).ReadToEnd();
            dynamic jsonResponse = serializer.DeserializeObject(jsonString);
            var flaggedTokens = jsonResponse["flaggedTokens"];

            // construct sorted dictionary of corrections in reverse order in string (right to left)
            // so that making a correction can't affect later indexes
            var corrections = new SortedDictionary<int, string[]>(Comparer<int>.Create((a, b) => b.CompareTo(a)));
            for (int i = 0; i < flaggedTokens.Length; i++)
            {
                var correction = flaggedTokens[i];
                var suggestion = correction["suggestions"][0];  // consider only first suggestion
                if (suggestion["score"] > (decimal)0.7)         // take it only if highly confident
                    corrections[(int)correction["offset"]] = new string[]   // dict key   = offset
                        { correction["token"], suggestion["suggestion"] };  // dict value = {error, correction}
            }

            // apply the corrections in order from right to left
            foreach (int i in corrections.Keys)
            {
                var oldtext = corrections[i][0];
                var newtext = corrections[i][1];

                // apply capitalization from original text to correction - all caps or initial caps
                if (text.Substring(i, oldtext.Length).All(char.IsUpper)) newtext = newtext.ToUpper();
                else if (char.IsUpper(text[i])) newtext = newtext[0].ToString().ToUpper() + newtext.Substring(1);

                text = text.Substring(0, i) + newtext + text.Substring(i + oldtext.Length);
            }

            return text;
        }

        // ***** GET TRANSLATABLE LANGUAGE CODES
        private void GetLanguagesForTranslate()
        {
            // send request to get supported language codes
            string uri = String.Format(TEXT_TRANSLATION_API_ENDPOINT, "languages") + "&scope=translation";
            WebRequest WebRequest = WebRequest.Create(uri);
            WebRequest.Headers.Add("Ocp-Apim-Subscription-Key", TEXT_TRANSLATION_API_SUBSCRIPTION_KEY);
            WebRequest.Headers.Add("Accept-Language", "en");
            WebResponse response = null;
            // read and parse the JSON response
            response = WebRequest.GetResponse();
            using (var reader = new StreamReader(response.GetResponseStream(), UnicodeEncoding.UTF8))
            {
                var result = JsonConvert.DeserializeObject<Dictionary<string, Dictionary<string, Dictionary<string, string>>>>(reader.ReadToEnd());
                var languages = result["translation"];

                languageCodes = languages.Keys.ToArray();
                foreach (var kv in languages)
                {
                    languageCodesAndTitles.Add(kv.Value["name"], kv.Key);
                }
            }
        }

        // ***** PERFORM TRANSLATION ON BUTTON CLICK
        private async void TranslateButton_Click(object sender, EventArgs e)
        {
            string textToTranslate = TextToTranslate.Text.Trim();

            string fromLanguage = FromLanguageComboBox.SelectedValue.ToString();
            string fromLanguageCode;

            // auto-detect source language if requested
            if (fromLanguage == "Detect")
            {
                fromLanguageCode = DetectLanguage(textToTranslate);
                if (!languageCodes.Contains(fromLanguageCode))
                {
                    MessageBox.Show("The source language could not be detected automatically " +
                        "or is not supported for translation.", "Language detection failed",
                        MessageBoxButton.OK, MessageBoxImage.Error);
                    return;
                }
            }
            else
                fromLanguageCode = languageCodesAndTitles[fromLanguage];

            string toLanguageCode = languageCodesAndTitles[ToLanguageComboBox.SelectedValue.ToString()];

            // spell-check the source text if the source language is English
            if (fromLanguageCode == "en")
            {
                if (textToTranslate.StartsWith("-"))    // don't spell check in this case
                    textToTranslate = textToTranslate.Substring(1);
                else
                {
                    textToTranslate = CorrectSpelling(textToTranslate);
                    TextToTranslate.Text = textToTranslate;     // put corrected text into input field
                }
            }

            // handle null operations: no text or same source/target languages
            if (textToTranslate == "" || fromLanguageCode == toLanguageCode)
            {
                TranslatedTextLabel.Content = textToTranslate;
                return;
            }

            // send HTTP request to perform the translation
            string endpoint = string.Format(TEXT_TRANSLATION_API_ENDPOINT, "translate");
            string uri = string.Format(endpoint + "&from={0}&to={1}", fromLanguageCode, toLanguageCode);

            System.Object[] body = new System.Object[] { new { Text = textToTranslate } };
            var requestBody = JsonConvert.SerializeObject(body);

            using (var client = new HttpClient())
            using (var request = new HttpRequestMessage())
            {
                request.Method = HttpMethod.Post;
                request.RequestUri = new Uri(uri);
                request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");
                request.Headers.Add("Ocp-Apim-Subscription-Key", TEXT_TRANSLATION_API_SUBSCRIPTION_KEY);
                request.Headers.Add("X-ClientTraceId", Guid.NewGuid().ToString());

                var response = await client.SendAsync(request);
                var responseBody = await response.Content.ReadAsStringAsync();

                var result = JsonConvert.DeserializeObject<List<Dictionary<string, List<Dictionary<string, string>>>>>(responseBody);
                var translations = result[0]["translations"];
                var translation = translations[0]["text"];

                // Update the translation field
                TranslatedTextLabel.Content = translation;
            }
        }
    }
}
```

### <a name="mainwindowxaml"></a>MainWindow.xaml

Diese Datei definiert die Benutzeroberfläche für die Anwendung (ein WPF-Formular). Wenn Sie eine eigene Version des Formulars entwerfen möchten, benötigen Sie diesen XAML-Code nicht.

```xml
<Window x:Class="MSTranslatorTextDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:MSTranslatorTextDemo"
        mc:Ignorable="d"
        Title="Microsoft Translator" Height="400" Width="700" BorderThickness="0">
    <Grid>
        <Label x:Name="label" Content="Microsoft Translator" HorizontalAlignment="Left" Margin="39,6,0,0" VerticalAlignment="Top" Height="49" FontSize="26.667"/>
        <TextBox x:Name="TextToTranslate" HorizontalAlignment="Left" Height="23" Margin="42,160,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="600" FontSize="14" TabIndex="3"/>
        <Label x:Name="EnterTextLabel" Content="Text to translate:" HorizontalAlignment="Left" Margin="40,129,0,0" VerticalAlignment="Top" FontSize="14"/>
        <Label x:Name="toLabel" Content="Translate to:" HorizontalAlignment="Left" Margin="304,58,0,0" VerticalAlignment="Top" FontSize="14"/>

        <Button x:Name="TranslateButton" Content="Translate" HorizontalAlignment="Left" Margin="39,206,0,0" VerticalAlignment="Top" Width="114" Height="31" Click="TranslateButton_Click" FontSize="14" TabIndex="4" IsDefault="True"/>
        <ComboBox x:Name="ToLanguageComboBox"
                HorizontalAlignment="Left"
                Margin="306,88,0,0"
                VerticalAlignment="Top"
                Width="175" FontSize="14" TabIndex="2">

        </ComboBox>
        <Label x:Name="fromLabel" Content="Translate from:" HorizontalAlignment="Left" Margin="40,58,0,0" VerticalAlignment="Top" FontSize="14"/>
        <ComboBox x:Name="FromLanguageComboBox"
            HorizontalAlignment="Left"
            Margin="42,88,0,0"
            VerticalAlignment="Top"
            Width="175" FontSize="14" TabIndex="1"/>
        <Label x:Name="TranslatedTextLabel" Content="Translation appears here" HorizontalAlignment="Left" Margin="39,255,0,0" VerticalAlignment="Top" Width="620" FontSize="14" Height="85" BorderThickness="0"/>
    </Grid>
</Window>
```

## <a name="service-endpoints"></a>Dienstendpunkte

Der Microsoft Translator-Dienst verfügt über verschiedene Endpunkte für unterschiedliche Übersetzungsfunktionen. In diesem Tutorial werden folgende verwendet:

|||
|-|-|
|`Languages`|Gibt die Sprachen zurück, die derzeit von anderen Vorgängen der Textübersetzungs-API unterstützt werden.|
|`Translate`|Gibt auf der Grundlage des angegebenen Ausgangstexts, eines Ausgangssprachcodes und eines Zielsprachcodes eine Übersetzung des Ausgangstexts in der Zielsprache zurück.|

## <a name="the-translation-app"></a>Die Übersetzungs-App

Die Benutzeroberfläche der Übersetzungsanwendung basiert auf Windows Presentation Foundation (WPF). Gehen Sie wie folgt vor, um in Visual Studio ein neues WPF-Projekt zu erstellen.

* Klicken Sie im Menü **Datei** auf **Neu > Projekt**.
* Navigieren Sie im Fenster „Neues Projekt“ zu **Installiert > Vorlagen > Visual C#**. In der Mitte des Dialogfelds wird eine Liste mit den verfügbaren Projektvorlagen angezeigt.
* Vergewissern Sie sich, dass im Dropdownmenü über der Projektvorlagenliste die Option **.NET Framework 4.5.2** ausgewählt ist.
* Klicken Sie in der Projektvorlagenliste auf **WPF-App (.NET Framework)**.
* Verwenden Sie die Felder im unteren Bereich des Dialogfelds, um das neue Projekt und die enthaltende Projektmappe zu benennen.
* Klicken Sie auf **OK**, um das neue Projekt und die Projektmappe zu erstellen.

![[Erstellen einer neuen WPF-App in Visual Studio]](media/translator-text-csharp-new-project.png)

Fügen Sie Ihrem Projekt Verweise auf die folgenden .NET Framework-Assemblys hinzu:

* System.Runtime.Serialization
* System.Web
* System.Web.Extensions

Installieren Sie außerdem das NuGet-Paket `Newtonsoft.Json` in Ihrem Projekt.

Suchen Sie als Nächstes im Projektmappen-Explorer nach der Datei `MainWindow.xaml`, und öffnen Sie sie. Sie ist zunächst leer. Die folgende Abbildung zeigt die fertige Benutzeroberfläche mit den Namen der Steuerelemente in Blau. Verwenden Sie für die Steuerelemente Ihrer Benutzeroberfläche die gleichen Namen, da sie auch im Code erscheinen.

![[Mit Anmerkungen versehene Ansicht des Hauptfensters im Visual Studio-Designer]](media/translator-text-csharp-xaml.png)

> [!NOTE]
> Der Quellcode für dieses Tutorial enthält den XAML-Quellcode für dieses Formular. Diesen können Sie in Ihr Projekt einfügen, anstatt das Formular in Visual Studio zu erstellen.

* `FromLanguageComboBox` *(Kombinationsfeld)*: Zeigt eine Liste mit den Sprachen an, die von Microsoft Translator für Textübersetzungen unterstützt werden. Der Benutzer wählt die Ausgangssprache für die Übersetzung aus.
* `ToLanguageComboBox` *(Kombinationsfeld)*: Dient zum Auswählen der Zielsprache für die Übersetzung und zeigt die gleiche Sprachenliste an wie `FromComboBox`.
* `TextToTranslate` *(Textfeld)*: Das Feld, in das der Benutzer den zu übersetzenden Text eingibt.
* `TranslateButton` *(Schaltfläche)*: Die Schaltfläche, auf die der Benutzer klickt, um den Text zu übersetzen. (Alternativ kann der Benutzer auch die EINGABETASTE drücken.)
* `TranslatedTextLabel` *(Bezeichnung)*: Hier wird die Übersetzung des Texts angezeigt, den der Benutzer eingegeben hat.

Wenn Sie eine eigene Version dieses Formulars erstellen, muss diese nicht *exakt* wie die hier verwendete Version aussehen. Achten Sie jedoch darauf, dass die Dropdownmenüs für die Sprache breit genug sind, um den vollständigen Sprachnamen anzuzeigen.

## <a name="the-mainwindow-class"></a>Die MainWindow-Klasse

Die Code-Behind-Datei `MainWindow.xaml.cs` enthält den Code für die Funktionen des Programms. Das Programm wird zweimal tätig:

* Wenn das Programm gestartet und `MainWindow` instanziiert wird, wird die Sprachenliste über Translator und die APIs abgerufen, um die Dropdownmenüs damit zu füllen. Diese Aufgabe wird einmalig zu Beginn einer Sitzung ausgeführt.

* Wenn der Benutzer auf die Schaltfläche **Translate** (Übersetzen) klickt, werden die Sprachauswahl des Benutzers und der eingegebene Text abgerufen, und anschließend wird die `Translate`-API aufgerufen, um die Übersetzung durchzuführen. Es können auch noch andere Funktionen aufgerufen werden, um die Sprache des Texts zu bestimmen und mögliche Schreibfehler vor der Übersetzung zu korrigieren.

Sehen Sie sich den Anfang der Klasse an:

```csharp
public partial class MainWindow : Window
{
    // Translator text subscription key from Microsoft Azure dashboard
    const string TEXT_TRANSLATION_API_SUBSCRIPTION_KEY = "ENTER KEY HERE";
    const string TEXT_ANALYTICS_API_SUBSCRIPTION_KEY = "ENTER KEY HERE";
    const string BING_SPELL_CHECK_API_SUBSCRIPTION_KEY = "ENTER KEY HERE";

    public static readonly string TEXT_TRANSLATION_API_ENDPOINT = "https://api.cognitive.microsofttranslator.com/{0}?api-version=3.0";
    const string TEXT_ANALYTICS_API_ENDPOINT = "https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/";
    const string BING_SPELL_CHECK_API_ENDPOINT = "https://api.cognitive.microsoft.com/bing/v7.0/spellcheck/";

    private string[] languageCodes;     // array of language codes

    // Dictionary to map language code from friendly name (sorted case-insensitively on language name)
    private SortedDictionary<string, string> languageCodesAndTitles =
        new SortedDictionary<string, string>(Comparer<string>.Create((a, b) => string.Compare(a, b, true)));

    public MainWindow()
    {
        // at least show an error dialog if there's an unexpected error
        AppDomain.CurrentDomain.UnhandledException += new UnhandledExceptionEventHandler(HandleExceptions);

        if (TEXT_TRANSLATION_API_SUBSCRIPTION_KEY.Length != 32
            || TEXT_ANALYTICS_API_SUBSCRIPTION_KEY.Length != 32
            || BING_SPELL_CHECK_API_SUBSCRIPTION_KEY.Length != 32)
        {
            MessageBox.Show("One or more invalid API subscription keys.\n\n" +
                "Put your keys in the *_API_SUBSCRIPTION_KEY variables in MainWindow.xaml.cs.",
                "Invalid Subscription Key(s)", MessageBoxButton.OK, MessageBoxImage.Error);
            System.Windows.Application.Current.Shutdown();
        }
        else
        {
            InitializeComponent();          // start the GUI
            GetLanguagesForTranslate();     // get codes and friendly names of languages that can be translated
            PopulateLanguageMenus();        // fill the drop-down language lists
        }
    }
// more to come
}
```

Zwei hier deklarierte Membervariablen enthalten Informationen zu unseren Sprachen:

|||
|-|-|
|`languageCodes`<br>Array von Zeichenfolgen|Dient zum Zwischenspeichern der Sprachcodes. Der Translator-Dienst identifiziert Sprachen mithilfe von Kurzcodes (etwa `en` für Englisch).|
|`languageCodesAndTitles`<br>SortedDictionary|Ordnet die Anzeigenamen auf der Benutzeroberfläche den in der API verwendeten Kurzcodes zu. Dabei wird die alphabetische Sortierung (ohne Berücksichtigung der Groß-/Kleinschreibung) eingehalten.|

Der erste Code, der von der Anwendung ausgeführt wird, ist der Konstruktor `MainWindow`. Richten Sie zuerst die `HandleExceptions`-Methode als globalen Fehlerhandler ein. Auf diese Weise ist im Falle einer unbehandelten Ausnahme zumindest eine Fehlermeldung vorhanden.

Vergewissern Sie sich als Nächstes, dass alle API-Abonnementschlüssel genau 32 Zeichen lang sind. Andernfalls hat wahrscheinlich *jemand* vergessen, seine API-Schlüssel einzufügen. Gehen Sie in diesem Fall so vor, dass der Vorgang mit einer Fehlermeldung abgebrochen wird. (Diese Schlüsselüberprüfung sagt aber natürlich noch nichts über die Gültigkeit der Schlüssel aus.)

Wenn Schlüssel vorhanden sind, die zumindest die richtige Länge haben, wird `InitializeComponent()` aufgerufen, um die Benutzeroberfläche zu initiieren. Hierzu wird die XAML-Beschreibung des Hauptanwendungsfensters gesucht, geladen und instanziiert.

Richten Sie abschließend noch die Dropdownmenüs für die Sprache ein. Für diese Aufgabe sind drei separate Methodenaufrufe erforderlich, die in den folgenden Abschnitten ausführlich beschrieben werden.

## <a name="get-supported-languages"></a>Abrufen der unterstützten Sprachen

Der Microsoft Translator-Dienst unterstützt zum Zeitpunkt der Artikelerstellung insgesamt 61 Sprachen, und es kommen nach und nach weitere Sprachen hinzu. Die unterstützten Sprachen sollten daher nicht im Programm hartcodiert werden. Fragen Sie stattdessen die unterstützten Sprachen beim Translator-Dienst ab. Jede unterstützte Sprache kann in eine andere unterstützte Sprache übersetzt werden.

Rufen Sie die API `Languages` auf, um die Liste der unterstützten Sprachen abzurufen.

Die API `Languages` akzeptiert den optionalen GET-Abfrageparameter *scope*. *scope* kann einen der drei folgenden Werte haben: `translation`, `transliteration` und `dictionary`. Für diesen Code wird der Wert `translation` verwendet.

Die API `Languages` akzeptiert auch einen optionalen HTTP-Header: `Accept-Language`. Der Wert dieses Headers bestimmt die Sprache, in der die Namen der unterstützten Sprachen zurückgegeben werden. Bei dem Wert muss es sich um ein wohlgeformtes BCP-47-Sprachtag handeln. Für diesen Code wird der Wert `en` verwendet, um die Namen der Sprachen auf Englisch abzurufen.

Die API `Languages` gibt eine JSON-Antwort zurück, die in etwa wie folgt aussieht:

```json
{
  "translation": {
    "af": {
      "name": "Afrikaans",
      "nativeName": "Afrikaans",
      "dir": "ltr"
    },
    "ar": {
      "name": "Arabic",
      "nativeName": "العربية",
      "dir": "rtl"
    },
...
}
```

Damit Sprachcodes (z.B. `af`) und Namen von Sprachen (z.B. `Afrikaans`) extrahiert werden können, wird für diesen Code die NewtonSoft.Json-Methode [JsonConvert.DeserializeObject](https://www.newtonsoft.com/json/help/html/M_Newtonsoft_Json_JsonConvert_DeserializeObject__1.htm) verwendet.

Erstellen Sie mit diesem Hintergrundwissen die folgende Methode, um die Sprachcodes und die Namen der Sprachen abzurufen.

```csharp
private void GetLanguagesForTranslate()
{
    // send request to get supported language codes
    string uri = String.Format(TEXT_TRANSLATION_API_ENDPOINT, "languages") + "&scope=translation";
    WebRequest WebRequest = WebRequest.Create(uri);
    WebRequest.Headers.Add("Ocp-Apim-Subscription-Key", TEXT_TRANSLATION_API_SUBSCRIPTION_KEY);
    WebRequest.Headers.Add("Accept-Language", "en");
    WebResponse response = null;
    // read and parse the JSON response
    response = WebRequest.GetResponse();
    using (var reader = new StreamReader(response.GetResponseStream(), UnicodeEncoding.UTF8))
    {
        var result = JsonConvert.DeserializeObject<Dictionary<string, Dictionary<string, Dictionary<string, string>>>>(reader.ReadToEnd());
        var languages = result["translation"];

        languageCodes = languages.Keys.ToArray();
        foreach (var kv in languages)
        {
            languageCodesAndTitles.Add(kv.Value["name"], kv.Key);
        }
    }
}
```

`GetLanguagesForTranslate()` erstellt zunächst die HTTP-Anforderung. Mit dem Abfragezeichenfolgen-Parameter `scope=translation` werden nur die Sprachen angefordert, die für die Textübersetzung unterstützt werden. Der Abonnementschlüssel für die Textübersetzungs-API wird den Anforderungsheadern hinzugefügt. Der `Accept-Language`-Header mit dem Wert `en` wird hinzugefügt, damit die unterstützten Sprachen auf Englisch zurückgegeben werden.

Nach Abschluss der Anforderung wird die JSON-Antwort analysiert und in ein Wörterbuch konvertiert, und anschließend werden die Sprachcodes der Membervariablen `languageCodes` hinzugefügt. Die Schlüssel-Wert-Paare, die die Sprachcodes und die Anzeigenamen der Sprachen enthalten, werden in einer Schleife durchlaufen und der Membervariablen `languageCodesAndTitles` hinzugefügt. (Die Dropdownmenüs im Formular enthalten zwar die Anzeigenamen, aber zum Anfordern der Übersetzung werden die Codes benötigt.)

## <a name="populate-the-language-menus"></a>Auffüllen der Sprachmenüs

Der Großteil der Benutzeroberfläche ist in XAML definiert, sodass Sie für die Einrichtung nicht viel mehr tun müssen, als `InitializeComponent()` aufzurufen. Der einzige weitere Schritt ist das Hinzufügen der Anzeigenamen der Sprachen zu den Dropdownmenüs **Translate from** (Übersetzen aus) und **Translate to** (Übersetzen in). Dies erfolgt unter `PopulateLanguageMenus()`.

```csharp
private void PopulateLanguageMenus()
{
    // Add option to automatically detect the source language
    FromLanguageComboBox.Items.Add("Detect");

    int count = languageCodesAndTitles.Count;
    foreach (string menuItem in languageCodesAndTitles.Keys)
    {
        FromLanguageComboBox.Items.Add(menuItem);
        ToLanguageComboBox.Items.Add(menuItem);
    }

    // set default languages
    FromLanguageComboBox.SelectedItem = "Detect";
    ToLanguageComboBox.SelectedItem = "English";
}
```

Zum Auffüllen der Menüs müssen wir lediglich das Wörterbuch `languageCodesAndTitles` durchlaufen und beiden Menüs die einzelnen Schlüssel (die Anzeigenamen) hinzufügen. Nach dem Auffüllen der Menüs werden die Standardwerte für die Ausgangs- und Zielsprache auf **Detect** (automatische Spracherkennung) bzw. auf **English** (Englisch) festgelegt.

> [!TIP]
> Ohne Standardauswahl für die Menüs kann der Benutzer auf **Translate** (Übersetzen) klicken, ohne zuerst eine Ausgangs- oder Zielsprache auszuwählen. Durch Festlegen von Standardwerten entsteht dieses Problem gar nicht erst.

Nachdem `MainWindow` initialisiert und die Benutzeroberfläche erstellt wurde, wartet der Code darauf, dass der Benutzer auf die Schaltfläche **Translate** (Übersetzen) klickt.

## <a name="perform-translation"></a>Durchführen der Übersetzung

Wenn der Benutzer auf **Translate** (Übersetzen) klickt, ruft WPF den Ereignishandler `TranslateButton_Click()` auf, wie hier zu sehen.

```csharp
private async void TranslateButton_Click(object sender, EventArgs e)
{
    string textToTranslate = TextToTranslate.Text.Trim();

    string fromLanguage = FromLanguageComboBox.SelectedValue.ToString();
    string fromLanguageCode;

    // auto-detect source language if requested
    if (fromLanguage == "Detect")
    {
        fromLanguageCode = DetectLanguage(textToTranslate);
        if (!languageCodes.Contains(fromLanguageCode))
        {
            MessageBox.Show("The source language could not be detected automatically " +
                "or is not supported for translation.", "Language detection failed",
                MessageBoxButton.OK, MessageBoxImage.Error);
            return;
        }
    }
    else
        fromLanguageCode = languageCodesAndTitles[fromLanguage];

    string toLanguageCode = languageCodesAndTitles[ToLanguageComboBox.SelectedValue.ToString()];

    // spell-check the source text if the source language is English
    if (fromLanguageCode == "en")
    {
        if (textToTranslate.StartsWith("-"))    // don't spell check in this case
            textToTranslate = textToTranslate.Substring(1);
        else
        {
            textToTranslate = CorrectSpelling(textToTranslate);
            TextToTranslate.Text = textToTranslate;     // put corrected text into input field
        }
    }

    // handle null operations: no text or same source/target languages
    if (textToTranslate == "" || fromLanguageCode == toLanguageCode)
    {
        TranslatedTextLabel.Content = textToTranslate;
        return;
    }

    // send HTTP request to perform the translation
    string endpoint = string.Format(TEXT_TRANSLATION_API_ENDPOINT, "translate");
    string uri = string.Format(endpoint + "&from={0}&to={1}", fromLanguageCode, toLanguageCode);

    System.Object[] body = new System.Object[] { new { Text = textToTranslate } };
    var requestBody = JsonConvert.SerializeObject(body);

    using (var client = new HttpClient())
    using (var request = new HttpRequestMessage())
    {
        request.Method = HttpMethod.Post;
        request.RequestUri = new Uri(uri);
        request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");
        request.Headers.Add("Ocp-Apim-Subscription-Key", TEXT_TRANSLATION_API_SUBSCRIPTION_KEY);
        request.Headers.Add("X-ClientTraceId", Guid.NewGuid().ToString());

        var response = await client.SendAsync(request);
        var responseBody = await response.Content.ReadAsStringAsync();

        var result = JsonConvert.DeserializeObject<List<Dictionary<string, List<Dictionary<string, string>>>>>(responseBody);
        var translations = result[0]["translations"];
        var translation = translations[0]["text"];

        // Update the translation field
        TranslatedTextLabel.Content = translation;
    }
}
```

Der erste Schritt ist das Abrufen der Ausgangs- und Zielsprache sowie des vom Benutzer eingegebenen Texts aus dem Formular.

Wenn die Ausgangssprache auf **Detect** (Erkennen) festgelegt ist, rufen Sie `DetectLanguage()` auf, um die Sprache des Texts zu bestimmen. Der Text liegt möglicherweise in einer Sprache vor, die von den Translator-APIs nicht unterstützt wird (die Anzahl erkennbarer Sprachen ist deutlich höher als die Anzahl übersetzbarer Sprachen), oder die Sprache kann unter Umständen von der Textanalyse-API nicht erkannt werden. In diesem Fall sollte der Benutzer eine entsprechende Meldung erhalten und der Vorgang ohne Übersetzung beendet werden.

Wenn als Ausgangssprache Englisch angegeben oder erkannt wird, überprüfen Sie mithilfe von `CorrectSpelling()` die Rechtschreibung des Texts und wenden ggf. erforderliche Korrekturen an. Der korrigierte Text wird wieder in das Eingabefeld eingefügt, damit der Benutzer weiß, dass eine Korrektur durchgeführt wurde. (Der Benutzer kann dem zu übersetzenden Text einen Bindestrich voranstellen, um die Rechtschreibkorrektur zu unterbinden.)

Falls der Benutzer keinen Text eingegeben hat oder Ausgangs- und Zielsprache identisch sind, ist keine Übersetzung erforderlich, und die Anforderung kann vermieden werden.

Der Code zum Durchführen der Übersetzungsanforderung sollte Ihnen vertraut vorkommen: Erstellen des URI, Erstellen einer Anforderung, Senden der Anforderung und Analysieren der Antwort. Senden Sie den Text an das `TranslatedTextLabel`-Steuerelement, um ihn anzuzeigen.

Übergeben Sie den Text als Nächstes in einem serialisierten JSON-Array im Hauptteil einer POST-Anforderung an die `Translate`-API. Das JSON-Array kann mehrere zu übersetzende Texte enthalten, aber hier ist nur einer erforderlich.

Die HTTP-Header `X-ClientTraceId` ist optional. Der Wert muss eine GUID sein. Anhand der vom Client angegebenen Ablaufverfolgungs-ID können Sie Anforderungen nachverfolgen, falls etwas nicht wie erwartet funktioniert. Sie ist allerdings nur hilfreich, wenn der Client den Wert von „X-ClientTraceID“ erfasst. Auf der Grundlage der Clientablaufverfolgungs-ID und des Datums der Anforderungen kann Microsoft mögliche Probleme diagnostizieren.

> [!NOTE]
> Da in diesem Tutorial der Microsoft Translator-Dienst im Mittelpunkt steht, gehen wir hier nicht näher auf die Methoden `DetectLanguage()` und `CorrectSpelling()` ein.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Referenz für die Textübersetzungs-API von Microsoft](https://docs.microsoft.com/azure/cognitive-services/Translator/reference/v3-0-reference)
