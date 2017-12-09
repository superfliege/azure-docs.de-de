
## <a name="setting-up-your-ios-application"></a>Einrichten der iOS-Anwendung

Dieser Abschnitt enthält schrittweise Anweisungen zum Erstellen eines neuen Projekts veranschaulicht, wie Sie eine iOS-Anwendung (Swift) integrieren mit *melden Sie sich mit Microsoft* damit Web-APIs abgefragt werden kann, die ein Token erfordern.

> Möchten Sie stattdessen XCode-Projekt für dieses Beispiel herunterladen? [Herunterladen ein Projekts](https://github.com/Azure-Samples/active-directory-ios-swift-native-v2/archive/master.zip) und fahren Sie mit der [Konfigurationsschritt](#create-an-application-express) im Codebeispiel wird vor der Ausführung zu konfigurieren.


## <a name="install-carthage-to-download-and-build-msal"></a>Installieren Sie zum Herunterladen und Aufbauen von MSAL Carthage
Carthage-Paket-Manager wird verwendet, während des Vorschauzeitraums von MSAL – integriert mit XCode, und gleichzeitig die Möglichkeit, Microsoft, um die Bibliothek zu ändern.

- Herunterladen und installieren Sie die neueste Version von Carthage [hier](https://github.com/Carthage/Carthage/releases "Carthage Download-URL")

## <a name="creating-your-application"></a>Erstellen einer Anwendung

1.  Öffnen Sie Xcode, und wählen Sie`Create a new Xcode project`
2.  Wählen Sie `iOS`  >  `Single view Application` , und klicken Sie auf *weiter*
3.  Geben Sie einen Produktnamen, und klicken Sie auf *weiter*
4.  Wählen Sie einen Ordner Ihrer app erstellen, und klicken Sie auf *erstellen*

## <a name="build-the-msal-framework"></a>Das Framework MSAL erstellen

Gehen Sie folgendermaßen vor, zum Abrufen und erstellen Sie dann auf die neueste Version der MSAL-Bibliotheken, die mithilfe von Carthage:

1.  Öffnen Sie das Bash-Terminal und wechseln Sie zum Stammordner der App
2.  Kopieren der unten, und fügen Sie in der Bash-Terminaldienste zum Erstellen einer Datei "Cartfile":

```bash
echo "github \"AzureAD/microsoft-authentication-library-for-objc\" \"master\"" > Cartfile
```
<!-- Workaround for Docs conversion bug -->
<ol start="3">
<li>
Kopieren Sie die unten. Dieser Befehl ruft Abhängigkeiten in einem Ordner Carthage/Auschecken, dann erstellt die Bibliothek MSAL:
</li>
</ol>

```bash
carthage update
```

> Die oben beschriebenen Prozess dient zum Herunterladen und die Microsoft Authentication Library (MSAL) zu erstellen. MSAL handles erfassen, Zwischenspeichern und Aktualisieren von Benutzertoken verwendet, um die APIs, die von der Azure Active Directory-v2 geschützt zuzugreifen.

## <a name="add-the-msal-framework-to-your-application"></a>Das Framework MSAL zur Anwendung hinzufügen
1.  Öffnen Sie in Xcode den `General` Registerkarte
2.  Wechseln Sie zu der `Linked Frameworks and Libraries` Abschnitt, und klicken Sie auf`+`
3.  Wählen Sie`Add other…`
4.  Select: `Carthage` > `Build` > `iOS` > `MSAL.framework` and click *Open*. Daraufhin sollte `MSAL.framework` zur Liste hinzugefügt.
5.  Wechseln Sie zu `Build Phases` Registerkarte, und klicken Sie auf `+` Symbol auswählen`New Run Script Phase`
6.  Fügen Sie die folgenden Inhalte, die *Skript Bereich*:

```text
/usr/local/bin/carthage copy-frameworks
```

<!-- Workaround for Docs conversion bug -->
<ol start="7">
<li>
Fügen Sie die folgenden auf <code>Input Files</code> durch Klicken auf <code>+</code>:
</li>
</ol>

```text
$(SRCROOT)/Carthage/Build/iOS/MSAL.framework
```

## <a name="creating-your-applications-ui"></a>Erstellen die Benutzeroberfläche Ihrer Anwendung
Eine Datei Main.storyboard sollte automatisch als Teil der Projektvorlage erstellt werden. Führen Sie die nachstehenden Anweisungen, um die Benutzeroberfläche der app zu erstellen:

1.  STRG + Klicken Sie auf `Main.storyboard` im Kontextmenü, und klicken Sie dann auf:`Open As` > `Source Code`
2.  Ersetzen Sie die `<scenes>` Knoten mit den folgenden Code:

```xml
 <scenes>
    <scene sceneID="tne-QT-ifu">
        <objects>
            <viewController id="BYZ-38-t0r" customClass="ViewController" customModule="MSALiOS" customModuleProvider="target" sceneMemberID="viewController">
                <layoutGuides>
                    <viewControllerLayoutGuide type="top" id="y3c-jy-aDJ"/>
                    <viewControllerLayoutGuide type="bottom" id="wfy-db-euE"/>
                </layoutGuides>
                <view key="view" contentMode="scaleToFill" id="8bC-Xf-vdC">
                    <rect key="frame" x="0.0" y="0.0" width="375" height="667"/>
                    <autoresizingMask key="autoresizingMask" widthSizable="YES" heightSizable="YES"/>
                    <subviews>
                        <label opaque="NO" userInteractionEnabled="NO" contentMode="left" horizontalHuggingPriority="251" verticalHuggingPriority="251" fixedFrame="YES" text="Microsoft Authentication Library" textAlignment="natural" lineBreakMode="tailTruncation" baselineAdjustment="alignBaselines" adjustsFontSizeToFit="NO" translatesAutoresizingMaskIntoConstraints="NO" id="ifd-fu-zjm">
                            <rect key="frame" x="64" y="28" width="246" height="21"/>
                            <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                            <fontDescription key="fontDescription" type="system" pointSize="17"/>
                            <nil key="textColor"/>
                            <nil key="highlightedColor"/>
                        </label>
                        <label opaque="NO" userInteractionEnabled="NO" contentMode="left" horizontalHuggingPriority="251" verticalHuggingPriority="251" fixedFrame="YES" text="Logging" textAlignment="natural" lineBreakMode="tailTruncation" baselineAdjustment="alignBaselines" adjustsFontSizeToFit="NO" translatesAutoresizingMaskIntoConstraints="NO" id="98g-dc-BPL">
                            <rect key="frame" x="16" y="277" width="62" height="21"/>
                            <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                            <fontDescription key="fontDescription" type="system" pointSize="17"/>
                            <nil key="textColor"/>
                            <nil key="highlightedColor"/>
                        </label>
                        <button opaque="NO" contentMode="scaleToFill" fixedFrame="YES" contentHorizontalAlignment="center" contentVerticalAlignment="center" buttonType="roundedRect" lineBreakMode="middleTruncation" translatesAutoresizingMaskIntoConstraints="NO" id="2rX-Vv-T1i">
                            <rect key="frame" x="87" y="100" width="200" height="30"/>
                            <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                            <state key="normal" title="Call Microsoft Graph API"/>
                            <connections>
                                <action selector="callGraphButton:" destination="BYZ-38-t0r" eventType="touchUpInside" id="Kx0-JL-Bv9"/>
                            </connections>
                        </button>
                        <textView clipsSubviews="YES" multipleTouchEnabled="YES" contentMode="scaleToFill" fixedFrame="YES" editable="NO" textAlignment="natural" selectable="NO" translatesAutoresizingMaskIntoConstraints="NO" id="qXW-2z-J7K">
                            <rect key="frame" x="16" y="324" width="343" height="291"/>
                            <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                            <color key="backgroundColor" white="1" alpha="1" colorSpace="calibratedWhite"/>
                            <accessibility key="accessibilityConfiguration">
                                <accessibilityTraits key="traits" updatesFrequently="YES"/>
                            </accessibility>
                            <fontDescription key="fontDescription" type="system" pointSize="14"/>
                            <textInputTraits key="textInputTraits" autocapitalizationType="sentences"/>
                        </textView>
                        <button opaque="NO" contentMode="scaleToFill" fixedFrame="YES" contentHorizontalAlignment="center" contentVerticalAlignment="center" buttonType="roundedRect" lineBreakMode="middleTruncation" translatesAutoresizingMaskIntoConstraints="NO" id="9u4-b8-vmX">
                            <rect key="frame" x="137" y="138" width="100" height="30"/>
                            <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                            <state key="normal" title="Sign-Out"/>
                            <connections>
                                <action selector="signoutButton:" destination="BYZ-38-t0r" eventType="touchUpInside" id="kZT-P8-0Zy"/>
                            </connections>
                        </button>
                    </subviews>
                    <color key="backgroundColor" red="1" green="1" blue="1" alpha="1" colorSpace="custom" customColorSpace="sRGB"/>
                </view>
                <connections>
                    <outlet property="loggingText" destination="qXW-2z-J7K" id="uqO-Yw-AsK"/>
                    <outlet property="signoutButton" destination="9u4-b8-vmX" id="OCh-qk-ldv"/>
                </connections>
            </viewController>
            <placeholder placeholderIdentifier="IBFirstResponder" id="dkx-z0-nzr" sceneMemberID="firstResponder"/>
        </objects>
        <point key="canvasLocation" x="140" y="137.18140929535232"/>
    </scene>
</scenes>
```
