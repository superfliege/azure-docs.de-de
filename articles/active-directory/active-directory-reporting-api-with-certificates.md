---
title: Abrufen von Daten per Azure AD Reporting-API mit Zertifikaten | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie die Azure AD Reporting-API mit Zertifikatanmeldeinformationen verwenden, um Daten ohne Benutzereingriff aus Verzeichnissen abzurufen.
services: active-directory
documentationcenter: ''
author: ramical
writer: v-lorisc
manager: kannar
ms.assetid: ''
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.component: compliance-reports
ms.date: 05/07/2018
ms.author: ramical
ms.openlocfilehash: 5ceb3d1ef99189e5b0cfcc48b38906ed28c07730
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34588956"
---
# <a name="get-data-using-the-azure-active-directory-reporting-api-with-certificates"></a>Abrufen von Daten per Berichtserstellungs-API von Azure Active Directory mit Zertifikaten

Die [Berichtserstellungs-APIs von Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) bieten Ihnen über eine Gruppe von REST-basierten APIs programmgesteuerten Zugriff auf die Daten. Sie können diese APIs über verschiedene Programmiersprachen und Tools aufrufen.

Wenn Sie ohne Benutzereingriff auf die Azure AD-Berichtserstellungs-API zugreifen möchten, können Sie Ihren Zugriff auf die Verwendung von Zertifikaten konfigurieren.

Dieser Artikel:

- Bietet Ihnen die erforderlichen Schritte, um über Zertifikate auf die Azure AD-Berichtserstellungs-API zuzugreifen.
- Setzt voraus, dass Sie die [Voraussetzungen für den Zugriff auf die Azure Active Directory-Berichtserstellungs-API](active-directory-reporting-api-prerequisites-azure-portal.md) erfüllt haben. 


Für den Zugriff auf die Berichtserstellungs-API über Zertifikate ist Folgendes erforderlich:

1. Installieren der erforderlichen Komponenten
2. Festlegen des Zertifikats in Ihrer App 
3. Erteilen von Berechtigungen
4. Abrufen eines Zugriffstokens




Informationen zum Quellcode finden Sie unter [Leverage Report API Module](https://github.com/AzureAD/azure-activedirectory-powershell/tree/gh-pages/Modules/AzureADUtils) (Nutzen des Reporting-API-Moduls). 

## <a name="install-prerequisites"></a>Installieren der erforderlichen Komponenten

Sie müssen Azure AD PowerShell V2 und das AzureADUtils-Modul installieren.

1. Laden Sie Azure AD Powershell V2 herunter, und führen Sie die Installation durch, indem Sie die Anleitung unter [Azure Active Directory PowerShell](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure AD Cmdlets/AzureAD/index.md) befolgen.

2. Laden Sie das Azure AD Utils-Modul unter [AzureAD/azure-activedirectory-powershell](https://github.com/AzureAD/azure-activedirectory-powershell/blob/gh-pages/Modules/AzureADUtils/AzureADUtils.psm1) herunter. 
  Dieses Modul enthält mehrere Hilfsprogramm-Cmdlets, z.B.:
    - Aktuelle Version von ADAL mit Verwendung von Nuget
    - Zugriffstoken für Benutzer, Anwendungsschlüssel und Zertifikate mit Verwendung von ADAL
    - Graph-API zur Verarbeitung von auf Seiten aufgeteilten Ergebnissen

**Gehen Sie wie folgt vor, um das Azure AD Utils-Modul zu installieren:**

1. Erstellen Sie ein Verzeichnis, um das Hilfsprogrammmodul zu speichern (z.B. „c:\azureAD“), und laden Sie das Modul von GitHub herunter.
2. Öffnen Sie eine PowerShell-Sitzung, und navigieren Sie zum gerade erstellten Verzeichnis. 
3. Importieren Sie das Modul, und installieren Sie es unter dem PowerShell-Modulpfad, indem Sie das Install-AzureADUtilsModule-Cmdlet verwenden. 

Die Sitzung sollte etwa wie folgt aussehen:

  ![Windows PowerShell](./media/active-directory-report-api-with-certificates/windows-powershell.png)

## <a name="set-the-certificate-in-your-app"></a>Festlegen des Zertifikats in Ihrer App

**So legen Sie das Zertifikat in Ihrer App fest**

1. [Rufen Sie die Objekt-ID](active-directory-reporting-api-prerequisites-azure-portal.md#get-your-applications-client-id) Ihrer App aus dem Azure-Portal ab. 

  ![Azure-Portal](./media/active-directory-report-api-with-certificates/azure-portal.png)

2. Öffnen Sie eine PowerShell-Sitzung, und stellen Sie mit dem Connect-AzureAD-Cmdlet eine Verbindung mit Azure AD her.

  ![Azure-Portal](./media/active-directory-report-api-with-certificates/connect-azuaread-cmdlet.png)

3. Verwenden Sie das New-AzureADApplicationCertificateCredential-Cmdlet von AzureADUtils, um Zertifikatanmeldeinformationen hinzuzufügen. 

>[!Note]
>Sie müssen die Objekt-ID der Anwendung, die Sie zuvor erfasst haben, und das Zertifikatobjekt angeben (über das Cert:-Laufwerk).
>


  ![Azure-Portal](./media/active-directory-report-api-with-certificates/add-certificate-credential.png)
  
## <a name="get-an-access-token"></a>Abrufen eines Zugriffstokens

Verwenden Sie zum Abrufen eines Zugriffstokens das **Get-AzureADGraphAPIAccessTokenFromCert**-Cmdlet von AzureADUtils. 

>[!NOTE]
>Sie müssen anstelle der Objekt-ID, die Sie im letzten Abschnitt verwendet haben, die Anwendungs-ID verwenden.
>

 ![Azure-Portal](./media/active-directory-report-api-with-certificates/application-id.png)

## <a name="use-the-access-token-to-call-the-graph-api"></a>Verwenden des Zugriffstokens zum Aufrufen der Graph-API

Nun können Sie das Skript erstellen. Unten ist ein Beispiel für die Verwendung des **Invoke-AzureADGraphAPIQuery**-Cmdlets von AzureADUtils angegeben. Mit diesem Cmdlet werden mehrseitige Ergebnisse verarbeitet, die dann an die PowerShell-Pipeline gesendet werden. 

 ![Azure-Portal](./media/active-directory-report-api-with-certificates/script-completed.png)

Nun können Sie den Export in eine CSV-Datei durchführen und die Daten in einem SIEM-System speichern. Außerdem können Sie Ihr Skript mit einer geplanten Aufgabe umschließen, mit der Azure AD-Daten regelmäßig aus Ihrem Mandanten abgerufen werden, ohne dass Anwendungsschlüssel im Quellcode gespeichert werden müssen. 

## <a name="next-steps"></a>Nächste Schritte

- [Verschaffen eines ersten Eindrucks über die Berichterstellungs-APIs](active-directory-reporting-api-getting-started-azure-portal.md#explore)

- [Erstellen Ihrer eigenen Lösung](active-directory-reporting-api-getting-started-azure-portal.md#customize)




