---
title: 'Verwenden der Sprachanpassung: Azure AD B2C | Microsoft-Dokumentation'
description: Erfahren Sie mehr über die Sprachanpassung.
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 02/26/2018
ms.author: davidmu
ms.openlocfilehash: 7c72c1d43d9a5fa541c72a8ba7a5ccedeafdaaff
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="azure-active-directory-b2c-using-language-customization"></a>Azure Active Directory B2C: Verwenden der Sprachanpassung

>[!NOTE]
>Dieses Feature befindet sich in der Phase der öffentlichen Vorschau.
>

Durch die Sprachanpassung können Sie mithilfe Ihrer Richtlinie Unterstützung für verschiedene Sprachen bereitstellen, um etwaige Kundenanforderungen zu erfüllen.  Microsoft bietet Übersetzungen für 36 Sprachen (siehe [Zusätzliche Informationen](#additional-information)), Sie können jedoch auch eigene Übersetzungen für beliebige Sprachen bereitstellen.  Auch wenn Ihre Benutzeroberfläche nur für eine einzelne Sprache bestimmt ist, können Sie beliebigen Text auf den Seiten anpassen.  

## <a name="how-does-language-customization-work"></a>Wie funktioniert die Sprachanpassung?
Mit der Sprachanpassung können Sie auswählen, in welchen Sprachen Ihre User Journey verfügbar ist.  Nach der Aktivierung des Features können Sie den Abfragezeichenfolgen-Parameter „ui_locales“ aus Ihrer Anwendung bereitstellen.  Wenn Sie Azure AD B2C aufrufen, übersetzen wir Ihre Seite in das von Ihnen angegebene Gebietsschema.  Mit dieser Art von Konfiguration haben Sie die vollständige Kontrolle über die Sprachen Ihrer User Journey, und die Spracheinstellungen des Kundenbrowsers können ignoriert werden. Es kann aber auch sein, dass Sie nicht so genau steuern müssen, welche Sprachen Kunden angezeigt werden.  Wenn Sie den Parameter „ui_locales“ nicht angeben, wird die Benutzeroberfläche des Kunden durch die Einstellungen des Browsers vorgegeben.  Sie können weiterhin steuern, in welche Sprachen Ihre User Journey übersetzt wird, indem Sie eine unterstützte Sprache hinzufügen.  Wenn für den Browser eines Kunden das Anzeigen einer Sprache festgelegt ist, die Sie nicht unterstützen möchten, wird stattdessen die Sprache angezeigt, die Sie als Standardeinstellung für die unterstützte Kultur ausgewählt haben.

1. **Für „ui-locales“ angegebene Sprache**: Nachdem Sie die Sprachanpassung aktiviert haben, wird Ihre User Journey in die hier angegebene Sprache übersetzt.
2. **Vom Browser angeforderte Sprache**: Wenn keine Angaben für „ui-locales“ vorhanden sind, wird eine Übersetzung in die vom Browser angeforderte Sprache durchgeführt, **sofern sie in den unterstützten Sprachen enthalten ist**.
3. **Standardsprache für Richtlinie**: Wenn vom Browser keine Sprache oder eine nicht unterstützte Sprache angegeben wird, wird eine Übersetzung in die Standardsprache der Richtlinie durchgeführt.

>[!NOTE]
>Falls Sie benutzerdefinierte Benutzerattribute verwenden, müssen Sie Ihre eigenen Übersetzungen angeben. Ausführliche Informationen hierzu finden Sie unter [Anpassen von Zeichenfolgen](#customize-your-strings).
>

## <a name="support-uilocales-requested-languages"></a>Unterstützen von angeforderten Sprachen für „ui_locales“ 
Bei Richtlinien, die vor der Veröffentlichung der allgemein verfügbaren Version der Sprachanpassung erstellt wurden, muss zuerst diese Funktion aktiviert werden.  Bei Richtlinien, die im Anschluss erstellt wurden, ist die Sprachanpassung standardmäßig aktiviert.  Indem Sie die „Sprachanpassung“ für eine Richtlinie aktivieren, können Sie die Sprache der User Journey jetzt steuern. Fügen Sie hierzu den Parameter „ui_locales“ hinzu.
1. [Führen Sie diese Schritte aus, um im Azure-Portal zur Seite „B2C-Features“ zu navigieren.](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-app-registration#navigate-to-b2c-settings)
2. Navigieren Sie zu einer Richtlinie, die Sie für Übersetzungen aktivieren möchten.
3. Klicken Sie auf **Sprachanpassung**.  
4. Klicken Sie oben auf **Sprachanpassung aktivieren**.
5. Lesen Sie das Dialogfeld, und klicken Sie auf „Ja“.

## <a name="select-which-languages-in-your-user-journey-are-enabled"></a>Auswählen der in der User Journey zu aktivierenden Sprachen 
Aktivieren Sie eine Reihe von Sprachen für Ihre User Journey, in die die Übersetzung erfolgen soll, wenn der Parameter „ui_locales“ nicht angegeben ist.
1. Stellen Sie anhand der obigen Anleitung sicher, ob für Ihre Richtlinie die „Sprachanpassung“ aktiviert ist.
2. Wählen Sie auf der Seite **Richtlinie bearbeiten** die Option **Sprachanpassung** aus.
3. Wählen Sie eine Sprache aus, die unterstützt werden soll.
4. Schalten Sie im Bereich „Eigenschaften“ die Option **Aktiviert** auf „Ja“ um.  
5. Klicken Sie oben im Bereich „Eigenschaften“ auf **Speichern**.

>[!NOTE]
>Wenn der Parameter „ui_locales“ nicht angegeben wird, wird die Seite nur in die Browsersprache des Kunden übersetzt, wenn sie aktiviert ist.
>

## <a name="customize-your-strings"></a>Anpassen von Zeichenfolgen
Mit der Sprachanpassung können Sie alle Zeichenfolgen Ihrer User Journey anpassen.
1. Stellen Sie anhand der obigen Anleitung sicher, ob für Ihre Richtlinie die „Sprachanpassung“ aktiviert ist.
2. Wählen Sie auf der Seite **Richtlinie bearbeiten** die Option **Sprachanpassung** aus.
3. Wählen Sie die Sprache aus, die Sie anpassen möchten.
4. Wählen Sie die Seite aus, die Sie bearbeiten möchten.
5. Klicken Sie auf **Standardeinstellungen herunterladen** (oder **Außerkraftsetzungen herunterladen**, wenn Sie diese Sprache zuvor bearbeitet haben). 

Bei diesen Schritten erhalten Sie eine JSON-Datei, die Sie nutzen können, um mit dem Bearbeiten Ihrer Zeichenfolgen zu beginnen.

### <a name="changing-any-string-on-the-page"></a>Ändern einer beliebigen Zeichenfolge auf der Seite
1. Öffnen Sie die JSON-Datei, die Sie im Rahmen der obigen Anleitung heruntergeladen haben, in einem JSON-Editor.
2. Suchen Sie nach dem Element, das Sie ändern möchten.  Sie können nach der gewünschten `StringId` der Zeichenfolge oder nach dem `Value` suchen, den Sie ändern möchten.
3. Aktualisieren Sie das `Value`-Attribut mit den Daten, die angezeigt werden sollen.
4. Denken Sie daran, bei jeder zu ändernden Zeichenfolge `Override` auf **True** umzuschalten.
5. Speichern Sie die Datei, und laden Sie Ihre Änderungen hoch (Sie finden das Steuerelement zum Hochladen an derselben Stelle, an der Sie die JSON-Datei heruntergeladen haben). 

>[!IMPORTANT]
>Stellen Sie beim Überschreiben einer Zeichenfolge sicher, dass Sie den Wert für `Override` auf `true` festlegen.  Wenn der Wert nicht geändert wird, wird der Eintrag ignoriert. 
>

### <a name="changing-extension-attributes"></a>Ändern von Erweiterungsattributen
Wenn Sie die Zeichenfolge für ein benutzerdefiniertes Benutzerattribut ändern oder eine Zeichenfolge dem JSON-Code hinzufügen möchten, hat diese das folgende Format:
```JSON
{
  "LocalizedStrings": [
    {
      "ElementType": "ClaimType",
      "ElementId": "extension_<ExtensionAttribute>",
      "StringId": "DisplayName",
      "Override": true,
      "Value": "<ExtensionAttributeValue>"
    }
    [...]
}
```

Ersetzen Sie `<ExtensionAttribute>` durch den Namen Ihres benutzerdefinierten Benutzerattributs.  

Ersetzen Sie `<ExtensionAttributeValue>` durch die neue Zeichenfolge, die angezeigt werden soll.

### <a name="using-localizedcollections"></a>Verwenden von LocalizedCollections
Wenn Sie eine vorgegebene Liste mit Werten für Antworten bereitstellen möchten, müssen Sie ein `LocalizedCollections`-Element erstellen.  Ein `LocalizedCollections`-Element ist ein Array mit Paaren aus `Name` und `Value`.  `Name` wird angezeigt, und `Value` ist der Wert, der im Anspruch zurückgegeben wird.  Für das Hinzufügen eines `LocalizedCollections`-Elements gilt das folgende Format:

```JSON
{
  "LocalizedStrings": [...],
  "LocalizedCollections": [{
      "ElementType":"ClaimType", 
      "ElementId":"<UserAttribute>",
      "TargetCollection":"Restriction",
      "Override": true,
      "Items":[
           {
                "Name":"<Response1>",
                "Value":"<Value1>"
           },
           {
                "Name":"<Response2>",
                "Value":"<Value2>"
           }
     ]
  }]
}
```

* `ElementId` ist das Benutzerattribut, für das `LocalizedCollections` als Antwort gilt.
* `Name` ist der Wert, der dem Benutzer angezeigt wird.
* `Value` wird im Anspruch zurückgegeben, wenn diese Option ausgewählt wird.

### <a name="upload-your-changes"></a>Hochladen von Änderungen
1. Navigieren Sie nach dem Eingeben der Änderungen in die JSON-Datei zurück zu Ihrem B2C-Mandanten.
2. Wählen Sie auf der Seite **Richtlinie bearbeiten** die Option **Sprachanpassung** aus.
3. Wählen Sie die Sprache aus, für die Sie Übersetzungen bereitstellen möchten.
4. Wählen Sie die Seite aus, für die Sie Übersetzungen bereitstellen möchten.
5. Klicken Sie auf das Ordnersymbol, und wählen Sie die hochzuladende JSON-Datei aus.
6. Diese Änderung wird automatisch in Ihrer Richtlinie gespeichert.

## <a name="using-page-ui-customization-with-language-customization"></a>Verwenden der „Seite für die Benutzeroberflächenanpassung“ mit der Sprachanpassung

Es gibt zwei Möglichkeiten zum Lokalisieren Ihrer HTML-Inhalte.  Durch Aktivieren von [Sprachanpassung](active-directory-b2c-reference-language-customization.md).  Wenn Sie dieses Feature aktivieren, kann Azure AD B2C den Open ID Connect-Parameter `ui-locales` an Ihren Endpunkt weiterleiten.  Ihr Inhaltsserver kann diesen Parameter verwenden, um benutzerdefinierte HTML-Seiten bereitzustellen, die sprachspezifisch sind.

Alternativ können wir abhängig vom verwendeten Gebietsschema Inhalte aus unterschiedlichen Quellen abrufen.  In Ihrem CORS-fähigen Endpunkt können Sie eine Ordnerstruktur zum Hosten des Inhalts für bestimmte Sprachen einrichten. Wenn Sie den Platzhalterwert `{Culture:RFC5646}` einfügen, rufen wir den richtigen Inhalt ab.  Nehmen Sie beispielsweise an, das Folgende ist Ihr benutzerdefinierter Seiten-URI:

```
https://wingtiptoysb2c.blob.core.windows.net/{Culture:RFC5646}/wingtip/unified.html
```
Sie können Ihre Seite in `fr` laden. HTML- und CSS-Inhalte werden von der folgenden Seite abgerufen:
```
https://wingtiptoysb2c.blob.core.windows.net/fr/wingtip/unified.html
```

## <a name="custom-locales"></a>Benutzerdefinierte Gebietsschemas

Sie können auch Sprachen hinzufügen, für die Microsoft aktuell keine Übersetzungen bereitstellt.  Sie müssen die Übersetzungen für alle Zeichenfolgen in der Richtlinie bereitstellen.

1. Wählen Sie auf der Seite **Richtlinie bearbeiten** die Option **Sprachanpassung** aus.
2. Wählen Sie im oberen Bereich der Seite **Benutzerdefinierte Sprache hinzuzufügen** aus.
3. Legen Sie im Kontextbereich fest, für welche Sprache Sie Übersetzungen bereitstellen, indem Sie einen gültigen Gebietsschemacode eingeben.
4. Für jede Seite können Sie eine Reihe von Außerkraftsetzungen für Englisch herunterladen und Übersetzungen anfertigen.
5. Wenn Sie mit der Bearbeitung der JSON-Dateien fertig sind, können Sie sie für jede Seite hochladen.
6. Wählen Sie **Aktivieren** aus. Ihre Richtlinie kann diese Sprache nun für Ihren Benutzer anzeigen.
7. Denken Sie daran, Ihre Sprache zu speichern, nachdem Sie sie aktiviert haben.

## <a name="additional-information"></a>Zusätzliche Informationen

### <a name="page-ui-customization-labels-are-persisted-as-your-first-set-of-overrides-once-language-customization-is-enabled"></a>Sobald die „Sprachanpassung“ aktiviert ist, werden Bezeichnungen zur „Seite für die Benutzeroberflächenanpassung“ als erste Gruppe von Außerkraftsetzungen beibehalten.
Bei Aktivierung der „Sprachanpassung“ werden Ihre vorherigen Bearbeitungen für Bezeichnungen anhand von „Seite für die Benutzeroberflächenanpassung“ in einer JSON-Datei für Englisch (en) beibehalten.  Sie können Ihre Bezeichnungen und anderen Zeichenfolgen weiterhin ändern, indem Sie Sprachressourcen in „Sprachanpassung“ hochladen.
### <a name="microsoft-is-committed-to-provide-the-most-up-to-date-translations-for-your-use"></a>Microsoft versucht, Ihnen jeweils die aktuellsten Übersetzungen zur Verfügung zu stellen.
Wir arbeiten ständig an der Verbesserung der Übersetzungen und sorgen für deren Konformität.  Wir identifizieren Fehler und Änderungen der globalen Terminologie und nehmen die Aktualisierungen vor, die für Ihre User Journey am besten geeignet sind.
### <a name="support-for-right-to-left-languages"></a>Unterstützung für von rechts nach links geschriebene Sprachen
Zurzeit bieten wir keine Unterstützung für von rechts nach links geschriebene Sprachen. Wenn Sie dieses Feature benötigen, stimmen Sie auf [Azure-Feedback](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/19393000-provide-language-support-for-right-to-left-languag) dafür.
### <a name="social-identity-provider-translations"></a>Übersetzungen für den Fall „Soziales Netzwerk als Identitätsanbieter“
Wir stellen den OIDC-Parameter „ui_locales“ für die Anmeldung an sozialen Netzwerken bereit, aber er wird von einigen Identitätsanbietern für soziale Netzwerke, z.B. Facebook und Google, nicht respektiert. 
### <a name="browser-behavior"></a>Browserverhalten
Für Chrome und Firefox wird jeweils die festgelegte Sprache angefordert, und wenn es sich um eine unterstützte Sprache handelt, wird sie anstelle der Standardsprache angezeigt.  Für Edge wird derzeit keine Sprache angefordert, sondern es wird gleich die Standardsprache verwendet.

### <a name="what-languages-are-supported"></a>Welche Sprachen werden unterstützt?

| Sprache              | Sprachcode |
|-----------------------|---------------|
| Bengalisch                | bn            |
| Tschechisch                 | cs            |
| Dänisch                | da            |
| Deutsch                | de            |
| Griechisch                 | el            |
| Englisch               | en            |
| Spanisch               | es            |
| Finnisch               | fi            |
| Französisch                | fr            |
| Gujarati              | gu            |
| Hindi                 | hi            |
| Kroatisch              | hr            |
| Ungarisch             | hu            |
| Italienisch               | it            |
| Japanisch              | ja            |
| Kannada               | kn            |
| Koreanisch                | ko            |
| Malayalam             | ml            |
| Marathi               | mr            |
| Malaiisch                 | ms            |
| Norwegisch (Bokmål)      | nb            |
| Niederländisch                 | nl            |
| Pandschabi               | pa            |
| Polnisch                | pl            |
| Portugiesisch (Brasilien)   | pt-br         |
| Portugiesisch (Portugal) | pt-pt         |
| Rumänisch              | ro            |
| Russisch               | ru            |
| Slowakisch                | sk            |
| Schwedisch               | sv            |
| Tamilisch                 | ta            |
| Telugu                | te            |
| Thailändisch                  | th            |
| Türkisch               | tr            |
| Chinesisch (vereinfacht)  | zh-hans       |
| Chinesisch (traditionell) | zh-hant       |
