---
title: Abrufen von Daten per Azure AD Reporting-API mit Zertifikaten | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie die Azure AD Reporting-API mit Zertifikatanmeldeinformationen verwenden, um Daten ohne Benutzereingriff aus Verzeichnissen abzurufen.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: report-monitor
ms.date: 05/07/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: c1f5b88dc8cd6d7f8ff1cd1269d4791d5654e133
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/18/2018
ms.locfileid: "42139959"
---
# <a name="get-data-using-the-azure-active-directory-reporting-api-with-certificates"></a>Abrufen von Daten per Berichtserstellungs-API von Azure Active Directory mit Zertifikaten

Die [Berichtserstellungs-APIs von Azure Active Directory (Azure AD)](concept-reporting-api.md) bieten Ihnen über eine Gruppe von REST-basierten APIs programmgesteuerten Zugriff auf die Daten. Sie können diese APIs über verschiedene Programmiersprachen und Tools aufrufen. Wenn Sie ohne Benutzereingriff auf die Azure AD-Berichtserstellungs-API zugreifen möchten, können Sie für Ihren Zugriff die Verwendung von Zertifikaten konfigurieren.

Dieser Vorgang umfasst die folgenden Schritte:

1. [Installieren der erforderlichen Komponenten](#install-prerequisites)
2. [Registrieren des Zertifikats in Ihrer App](#register-the-certificate-in-your-app)
3. [Abrufen eines Zugriffstokens für die MS Graph-API](#get-an-access-token-for-ms-graph-api)
4. [Abfragen der MS Graph-API-Endpunkte](#query-the-ms-graph-api-endpoints)


## <a name="install-prerequisites"></a>Installieren der erforderlichen Komponenten

1. Stellen Sie zuerst sicher, dass Sie die [Voraussetzungen für den Zugriff auf die Azure Active Directory-Berichtserstellungs-API](howto-configure-prerequisites-for-reporting-api.md) erfüllen. 

2. Laden Sie Azure AD Powershell V2 herunter, und führen Sie die Installation durch, indem Sie die Anleitung unter [Azure Active Directory PowerShell](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure AD Cmdlets/AzureAD/index.md) befolgen.

3. Installieren Sie die MSCloudIDUtils über [PowerShell-Katalog: MSCloudIdUtils](https://www.powershellgallery.com/packages/MSCloudIdUtils/). Dieses Modul enthält mehrere Hilfsprogramm-Cmdlets, z.B.:
    - Die für die Authentifizierung benötigten ADAL-Bibliotheken
    - Zugriffstoken für Benutzer, Anwendungsschlüssel und Zertifikate mit Verwendung von ADAL
    - Graph-API zur Verarbeitung von auf Seiten aufgeteilten Ergebnissen

4. Wenn Sie das Modul erstmalig verwenden, führen Sie **Install-MSCloudIdUtilsModule** aus, um das Setup abzuschließen. Ansonsten können Sie es einfach mit dem PowerShell-Befehl **Import-Module** importieren.

Ihre Sitzung sollte etwa wie folgt aussehen:

  ![Windows PowerShell](./media/tutorial-access-api-with-certificates/module-install.png)

## <a name="register-the-certificate-in-your-app"></a>Registrieren des Zertifikats in Ihrer App

1. Navigieren Sie zunächst zur Anwendungsregistrierungsseite. Wechseln Sie hierzu zum [Azure-Portal](https://portal.azure.com). Klicken Sie auf **Azure Active Directory**, dann auf **App-Registrierungen**, und wählen Sie Ihre Anwendung in der Liste aus. 

2. Befolgen Sie dann die Anweisungen zum [Registrieren Ihres Zertifikats bei Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-certificate-credentials#register-your-certificate-with-azure-ad) für die Anwendung. 

3. Notieren Sie die Anwendungs-ID und den Fingerabdruck des Zertifikats, das Sie gerade in Ihrer Anwendung registriert haben. Um den Fingerabdruck zu finden, wechseln Sie auf Ihrer Anwendungsseite im Portal zu **Einstellungen**, und klicken Sie auf **Schlüssel**. Der Fingerabdruck befindet sich unter der Liste **Öffentliche Schlüssel**.

  
## <a name="get-an-access-token-for-ms-graph-api"></a>Abrufen eines Zugriffstokens für die MS Graph-API

Verwenden Sie zum Abrufen eines Zugriffstokens für die MS Graph-API das Cmdlet **Get-MSCloudIdMSGraphAccessTokenFromCert** im PowerShell-Modul MSCloudIdUtils. 

>[!NOTE]
>Sie müssen die Anwendungs-ID (auch ClientID genannt) und den Fingerabdruck des Zertifikats mit dem privaten Schlüssel verwenden, der im Zertifikatsspeicher Ihres Computers installiert ist (im Zertifikatspeicher „CurrentUser“ oder „LocalMachine“).
>

 ![Azure-Portal](./media/tutorial-access-api-with-certificates/getaccesstoken.png)

## <a name="use-the-access-token-to-call-the-graph-api"></a>Verwenden des Zugriffstokens zum Aufrufen der Graph-API

Nun können Sie das Zugriffstoken in Ihrem PowerShell-Skript zum Abfragen der Graph-API verwenden. Es folgt ein Beispiel, bei dem das Cmdlet **Invoke-MSCloudIdMSGraphQuery** aus den MSCloudIDUtils verwendet wird, um den Endpunkt „signIns“ oder „directoryAudits“ aufzuzählen. Mit diesem Cmdlet werden mehrseitige Ergebnisse verarbeitet, die dann an die PowerShell-Pipeline gesendet werden.

### <a name="query-the-directoryaudits-endpoint"></a>Abfragen des Endpunkts „directoryAudits“
 ![Azure-Portal](./media/tutorial-access-api-with-certificates/query-directoryAudits.png)

 ### <a name="query-the-signins-endpoint"></a>Abfrage des Endpunkts „SignIns“
 ![Azure-Portal](./media/tutorial-access-api-with-certificates/query-signins.png)

Sie können nun diese Daten in eine CSV-Datei exportieren und Datei in einem SIEM-System speichern. Außerdem können Sie Ihr Skript mit einer geplanten Aufgabe umschließen, mit der Azure AD-Daten regelmäßig aus Ihrem Mandanten abgerufen werden, ohne dass Anwendungsschlüssel im Quellcode gespeichert werden müssen. 


## <a name="next-steps"></a>Nächste Schritte

* [Verschaffen eines ersten Eindrucks über die Berichterstellungs-APIs](concept-reporting-api.md)
* [Referenz zur Überwachungs-API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 
* [Referenz zur Anmeldeaktivitätsbericht-API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)



