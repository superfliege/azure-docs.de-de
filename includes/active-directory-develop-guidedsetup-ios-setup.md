---
title: Includedatei
description: Includedatei
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: ios
ms.workload: identity
ms.date: 09/19/2018
ms.author: dadobali
ms.custom: include file
ms.openlocfilehash: d8e249a788d80108d46a61f316b795ab04e541a2
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58203516"
---
## <a name="setting-up-your-ios-application"></a>Einrichten Ihrer iOS-Anwendung

Dieser Abschnitt enthält detaillierte Anleitungen zum Erstellen eines neuen Projekts, um zu veranschaulichen, wie Sie eine iOS (Swift)-Anwendung mit *Bei Microsoft anmelden* so integrieren, dass Sie Web-APIs abfragen kann, die ein Token erfordern.

> Möchten Sie stattdessen das XCode-Projekt dieses Beispiels herunterladen? [Laden Sie ein Projekt herunter](https://github.com/Azure-Samples/active-directory-ios-swift-native-v2/archive/master.zip), und fahren Sie mit dem Schritt [Konfiguration](#register-your-application) fort, um das Codebeispiel vor der Ausführung zu konfigurieren.

## <a name="install-carthage-to-download-and-build-msal"></a>Installieren von Carthage, um MSAL herunterzuladen und aufzubauen

Der Carthage-Paket-Manager wird während der Vorschauphase von MSAL verwendet – er kann mit XCode integriert werden, während Microsoft gleichzeitig die Möglichkeit behält, Änderungen an der Bibliothek vorzunehmen.

- Laden Sie [hier](https://github.com/Carthage/Carthage/releases "Carthage-Download-URL") die neueste Version von Carthage herunter, und installieren Sie sie.

## <a name="creating-your-application"></a>Erstellen Ihrer Anwendung

1. Starten Sie Xcode, und wählen Sie **Create a new Xcode project** (Neues Xcode-Projekt erstellen) aus.
2. Wählen Sie **iOS > Single view Application** (iOS > Einzelansicht-App) und dann **Next** (Weiter) aus.
3. Geben Sie einen Produktnamen ein, und wählen Sie **Next** (Weiter) aus.
4. Wählen Sie einen Ordner aus, um Ihre App zu erstellen, und klicken Sie auf *Erstellen*.

## <a name="build-the-msal-framework"></a>Erstellen des MSAL-Frameworks

Gehen Sie folgendermaßen vor, um die neueste Version der MSAL-Bibliotheken mit Carthage mithilfe von Pull zu übertragen und zu erstellen:

1. Öffnen Sie das Bashterminal, und gehen Sie zum Stammordner der App.
2. Kopieren Sie den nachstehenden Text, und fügen Sie ihn in den Bash-Terminal ein, um eine „Cartfile“-Datei zu erstellen:

```bash
echo "github \"AzureAD/microsoft-authentication-library-for-objc\" \"master\"" > Cartfile
```
<!-- Workaround for Docs conversion bug -->
<ol start="3">
<li>
Kopieren den nachstehenden Text, und fügen Sie diesen ein. Dieser Befehl ruft Abhängigkeiten in einem Carthage-/Auscheckordner ab, und erstellt dann die MSAL-Bibliothek:
</li>
</ol>

```bash
carthage update
```

> Der oben beschriebene Prozess dient zum Herunterladen und Erstellen der Microsoft Authentication Library (MSAL). MSAL kümmert sich um das Erfassen, Zwischenspeichern und Aktualisieren von Benutzertoken für den Zugriff auf APIs, die von Azure Active Directory v2.0 geschützt werden.

## <a name="add-the-msal-framework-to-your-application"></a>Hinzufügen des MSAL-Frameworks zu Ihrer Anwendung

1. Öffnen Sie in Xcode die Registerkarte **General** (Allgemein).
2. Navigieren Sie zum Abschnitt **Linked Frameworks and Libraries** (Verknüpfte Frameworks und Bibliotheken) und wählen Sie **+** aus.
3. Wählen Sie **Add other...** (Sonstiges hinzufügen) aus.
4. Wählen Sie **Carthage > Build > iOS > MSAL.framework** (Carthage > Erstellen > iOS > MSAL.framework) und dann **Open** (Öffnen) aus. Daraufhin sollten Sie sehen, dass `MSAL.framework` zur Liste hinzugefügt wurde.
5. Wechseln Sie zur Registerkarte **Build Phases** (Phasen erstellen), wählen Sie das **+**-Symbol und dann **New Run Script Phase** (Neue Skriptausführungsphase) aus.
6. Fügen Sie die folgenden Inhalte in den *Skriptbereich* ein:

```text
/usr/local/bin/carthage copy-frameworks
```

<!-- Workaround for Docs conversion bug -->
<ol start="7">
<li>
Fügen Sie den folgenden Text durch Klicken auf <code>+</code> zu <code>Input Files</code> hinzu:
</li>
</ol>

```text
$(SRCROOT)/Carthage/Build/iOS/MSAL.framework
```

## <a name="creating-your-applications-ui"></a>Erstellen der Benutzeroberfläche Ihrer Anwendung

Eine Datei Main.storyboard-Datei sollte als Teil Ihrer Projektvorlage automatisch erstellt werden. Befolgen Sie die nachstehenden Anweisungen, um die Benutzeroberfläche der App zu erstellen:

1.  Klicken Sie bei gedrückter STRG-TASTE auf `Main.storyboard`, um das Kontextmenü aufzurufen, und klicken Sie dann auf `Open As` > `Source Code`
2.  Ersetzen Sie die `<scenes>`-Knoten mit dem folgenden Code:

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
                            <state key="normal" title="Sign Out"/>
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
