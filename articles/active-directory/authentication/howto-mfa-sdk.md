---
title: Azure MFA Software Development Kit für benutzerdefinierte Apps – Azure Active Directory
description: Dieser Artikel erläutert, wie Sie das Azure MFA SDK herunterladen und verwenden, um die zweistufige Überprüfung für Ihre benutzerdefinierten Apps zu aktivieren.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8b6f5def70dcb2564e92c04e53b5d2ef5f0631fd
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2019
ms.locfileid: "58310862"
---
# <a name="building-multi-factor-authentication-into-custom-apps-sdk"></a>Erstellen von Multi-Factor Authentication in benutzerdefinierten Apps (SDK)

> [!IMPORTANT]
> Die Einstellung des Azure Multi-Factor Authentication Software Development Kit (SDK) wurde angekündigt. Dieses Feature wird für neue Kunden nicht mehr unterstützt. Aktuelle Kunden können das SDK bis zum 14. November 2018 weiterhin nutzen. Nach diesem Zeitpunkt führen Aufrufe an das SDK zu Fehlern. 

Mit dem Software Development Kit (SDK) für Azure Multi-Factor Authentication können Sie die zweistufige Überprüfung direkt in den Anmelde- oder Transaktionsprozessen von Anwendungen in Ihrem Azure AD-Mandanten erstellen.

Das Multi-Factor Authentication SDK ist für C#, Visual Basic (.NET), Java, Perl, PHP und Ruby verfügbar. Das SDK bietet einen einfachen Wrapper für die zweistufige Überprüfung. Es enthält alles, was Sie zum Schreiben des Codes benötigen, einschließlich kommentierter Quellcodedateien, Beispieldateien und einer detaillierten Infodatei. Jedes SDK enthält auch ein Zertifikat und einen privaten Schlüssel zum Verschlüsseln von Transaktionen, das/der für Ihren Multi-Factor Authentication-Anbieter eindeutig ist. Solange Sie einen Anbieter haben, können Sie das SDK in so vielen Sprachen und Formaten herunterladen, wie Sie benötigen.

Die Struktur der APIs im Multi-Factor Authentication SDK ist einfach. Sie führen einen einzelnen Funktionsaufruf an eine API aus und übergeben die Parameter für die Multi-Factor-Option (z.B. den Überprüfungsmodus) und die Benutzerdaten (z.B. die Telefonnummer, die angerufen werden soll, oder die PIN-Nummer, die überprüft werden soll). Die APIs übersetzen den Funktionsaufruf in Webdiensteanforderungen an den cloudbasierten Azure Multi-Factor Authentication-Dienst. Alle Aufrufe müssen einen Verweis auf das private Zertifikat enthalten, das in jedem SDK enthalten ist.

Da die APIs keinen Zugriff auf in Azure Active Directory registrierte Benutzer haben, müssen Sie Benutzerinformationen in einer Datei oder Datenbank bereitstellen. Darüber hinaus bieten die APIs keine Registrierungs- oder Benutzerverwaltungsfunktionen, weshalb Sie diese Prozesse in Ihre Anwendung integrieren müssen.

> [!IMPORTANT]
> Um das SDK herunterzuladen, müssen Sie einen Azure Multi-Factor Authentication-Anbieter erstellen, auch wenn Sie über Azure MFA-, AAD Premium- oder EMS-Lizenzen verfügen. Wenn Sie zu diesem Zweck einen Azure Multi-Factor Authentication-Anbieter erstellen und bereits über Lizenzen verfügen, erstellen Sie den Anbieter mit dem Modell **Pro aktiviertem Benutzer**. Verknüpfen Sie den Anbieter anschließend mit dem Verzeichnis, in dem die Lizenzen für Azure MFA, Azure AD Premium oder EMS enthalten sind. Diese Konfiguration stellt sicher, dass nur Kosten berechnet werden, wenn die Anzahl eindeutiger Benutzer, die das SDK verwenden, die Anzahl Ihrer Lizenzen übersteigt.


## <a name="download-the-sdk"></a>Herunterladen des SDK
Das Herunterladen des Azure Multi-Factor SDK erfordert einen [Azure MFA-Anbieter](concept-mfa-authprovider.md).  Dies erfordert ein vollständiges Azure-Abonnement, auch wenn Sie über Azure MFA-, Azure AD Premium- oder Enterprise Mobility Suite-Lizenzen verfügen. Die öffentlichen Methoden zum Herunterladen des SDKs wurden deaktiviert, weil das SDK eingestellt wurde. Sie müssen einen Supportfall bei Microsoft öffnen, wenn Sie das SDK herunterladen möchten. Das SDK wird nur für Kunden bereitgestellt, die das SDK bereits verwenden. Neue Kunden werden nicht eingebunden.

## <a name="whats-in-the-sdk"></a>Was bietet das SDK?
Das SDK enthält Folgendes:

* **README**. Erläutert, wie die Multi-Factor Authentication-APIs in einer neuen oder vorhandenen Anwendung verwendet werden.
* **Quelldateien** für Multi-Factor Authentication.
* **Clientzertifikat** für die Kommunikation mit dem Multi-Factor Authentication-Dienst.
* **Privater Schlüssel** für das Zertifikat.
* **Aurufergebnisse.**  Eine Liste mit Aurufergebniscodes. Um diese Datei zu öffnen, verwenden Sie eine Anwendung mit Textformatierung wie WordPad. Verwenden Sie die Aufrufergebniscodes, um die Implementierung der Multi-Factor Authentication in Ihrer Anwendung zu testen und Probleme zu behandeln. Dies sind keine Authentifizierungsstatuscodes.
* **Beispiele.**  Beispielcode für eine grundlegende, funktionsfähige Implementierung von Multi-Factor Authentication.

> [!WARNING]
> Das Client-Zertifikat ist ein eindeutiges, privates Zertifikat, das speziell für Sie generiert wurde. Geben Sie diese Datei nicht frei und verlieren Sie sie nicht. Sie ist der Schlüssel zum Gewährleisten der Sicherheit der Kommunikation mit dem Multi-Factor Authentication-Dienst.

## <a name="code-sample"></a>Codebeispiel
Dieses Codebeispiel veranschaulicht, wie in Ihrer Anwendung mithilfe der APIs im Azure Multi-Factor Authentication-SDK ein Sprachanruf im Standardmodus zur Überprüfung hinzugefügt werden kann. Der Standardmodus besteht aus einem Telefonanruf, auf den der Benutzer durch Drücken der #-Taste reagiert.

Dieses Beispiel verwendet das C# .NET 2.0 Multi-Factor Authentication SDK in einer einfachen ASP.NET-Anwendung mit serverseitiger C#-Logik, aber der Prozess ist in anderen Sprachen sehr ähnlich. Da das SDK Quelldateien und keine ausführbaren Dateien enthält, können Sie die Dateien direkt in der Anwendung erstellen, darauf verweisen oder sie dort einbinden.

> [!NOTE]
> Wenn die Multi-Factor Authentication implementiert wird, verwenden Sie die zusätzlichen Methoden (Telefonanruf oder SMS) als sekundäre oder tertiäre Überprüfungsmöglichkeiten, um Ihre primäre Authentifizierungsmethode (Benutzername und Kennwort) zu ergänzen. Diese Methoden sind nicht als primäre Authentifizierungsmethoden konzipiert.

### <a name="code-sample-overview"></a>Übersicht über das Codebeispiel
Dieser Beispielcode für eine einfache Webdemoanwendung verwendet einen Telefonanruf mit einer #-Tastenantwort, um die Authentifizierung des Benutzers zu überprüfen. Dieser Telefonanruffaktor wird in Multi-Factor Authentication Standardmodus genannt.

Der clientseitige Code enthält keine Elemente, die für Multi-Factor Authentication spezifisch sind. Da die zusätzlichen Authentifizierungsfaktoren nicht von der primären Authentifizierung abhängen, können Sie sie ohne Änderung der vorhandenen Anmeldeschnittstelle hinzufügen. Mit den APIs im Multi-Factor SDK können Sie die Benutzererfahrung anpassen. Möglicherweise müssen Sie dafür gar nichts ändern.

Der serverseitige Code fügt den Standard-Authentifizierungsmodus in Schritt 2 hinzu. Er erstellt ein PfAuthParams-Objekt mit den Parametern, die für die Überprüfung im Standardmodus erforderlich sind: Benutzername, Telefonnummer, Modus und Pfad zum Clientzertifikat (CertFilePath), der bei jedem Aufruf erforderlich ist. Eine Demonstration aller Parameter in PfAuthParams finden Sie in der Beispieldatei im SDK.

Als Nächstes übergibt der Code das PfAuthParams-Objekt an die pf_authenticate()-Funktion. Der Rückgabewert gibt den Erfolg oder Misserfolg der Authentifizierung an. Die Ausgangsparameter „callStatus“ und „errorID“ enthalten zusätzliche Informationen zu den Anrufergebnissen. Die Anrufergebniscodes sind in der Anrufergebnisdatei im SDK dokumentiert.

Diese minimale Implementierung lässt sich in einigen wenigen Zeilen schreiben. Der Produktionscode enthält jedoch normalerweise eine komplexere Fehlerbehandlung, zusätzlichen Datenbankcode und ein optimiertes Bedienungserlebnis.

### <a name="web-client-code"></a>Webclientcode
Es folgt Webclientcode für eine Demoseite.

    <%@ Page Language="C#" AutoEventWireup="true" CodeFile="Default.aspx.cs" Inherits="\_Default" %>

    <!DOCTYPE html>

    <html xmlns="https://www.w3.org/1999/xhtml">
    <head runat="server">
    <title>Multi-Factor Authentication Demo</title>
    </head>
    <body>
    <h1>Azure Multi-Factor Authentication Demo</h1>
    <form id="form1" runat="server">

    <div style="width:auto; float:left">
    Username:&nbsp;<br />
    Password:&nbsp;<br />
    </div>

    <div">
    <asp:TextBox id="username" runat="server" width="100px"/><br />
    <asp:Textbox id="password" runat="server" width="100px" TextMode="password" /><br />
    </div>

    <asp:Button id="btnSubmit" runat="server" Text="Log in" onClick="btnSubmit_Click"/>

    <p><asp:Label ID="lblResult" runat="server"></asp:Label></p>

    </form>
    </body>
    </html>


### <a name="server-side-code"></a>Serverseitiger Code
Im folgenden serverseitigen Code wird Multi-Factor Authentication in Schritt 2 konfiguriert und ausgeführt. Der Standardmodus (MODE_STANDARD) besteht aus einem Telefonanruf, auf den der Benutzer durch Drücken der #-Taste reagiert.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.UI;
    using System.Web.UI.WebControls;

    public partial class \_Default : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {
        }

        protected void btnSubmit_Click(object sender, EventArgs e)
        {
            // Step 1: Validate the username and password
            if (username.Text != "Contoso" || password.Text != "password")
            {
                lblResult.ForeColor = System.Drawing.Color.Red;
                lblResult.Text = "Username or password incorrect.";
            }
            else
            {
                // Step 2: Perform multi-factor authentication

                // Add call details from the user database.
                PfAuthParams pfAuthParams = new PfAuthParams();
                pfAuthParams.Username = username.Text;
                pfAuthParams.Phone = "5555555555";
                pfAuthParams.Mode = pf_auth.MODE_STANDARD;

                // Specify a client certificate
                // NOTE: This file contains the private key for the client
                // certificate. It must be stored with appropriate file
                // permissions.
                pfAuthParams.CertFilePath = "c:\\cert_key.p12";

                // Perform phone-based authentication
                int callStatus;
                int errorId;

                if(pf_auth.pf_authenticate(pfAuthParams, out callStatus, out errorId))
                {
                    lblResult.ForeColor = System.Drawing.Color.Green;
                    lblResult.Text = "Multi-Factor Authentication succeeded.";
                }
                else
                {
                    lblResult.ForeColor = System.Drawing.Color.Red;
                    lblResult.Text = "Multi-Factor Authentication failed.";
                }
            }

        }
    }
