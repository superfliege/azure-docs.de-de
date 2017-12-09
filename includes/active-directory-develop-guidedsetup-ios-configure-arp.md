
## <a name="add-the-applications-registration-information-to-your-app"></a>Registrierungsinformationen für die Anwendung der app hinzufügen

In diesem Schritt müssen Sie die Anwendungs-Id zu Ihrem Projekt hinzufügen:

1.  In `ViewController.swift`, ersetzen Sie die Zeile, beginnend mit "`let kClientID`" mit:
```swift
let kClientID = "[Enter the application Id here]"
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
STRG + Klicken Sie auf <code>Info.plist</code> im Kontextmenü, und klicken Sie dann auf: <code>Open As</code>> <code>Source Code</code>
</li>
<li>
Klicken Sie unter der <code>dict</code> Stammknoten, fügen Sie Folgendes hinzu:
</li>
</ol>

```xml
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>$(PRODUCT_BUNDLE_IDENTIFIER)</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>msal[Enter the application Id here]</string>
            <string>auth</string>
        </array>
    </dict>
</array>
```
