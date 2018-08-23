---
title: Beispiele zur Azure Active Directory-Anmeldeaktivitätsbericht-API | Microsoft Docs
description: Vorgehensweise zum Einstieg in die Azure Active Directory Berichterstellungs-API
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: c41c1489-726b-4d3f-81d6-83beb932df9c
ms.service: active-directory
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 05/31/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: b02c0ac482c13868606674a097a84976780b7ef6
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2018
ms.locfileid: "41917547"
---
# <a name="azure-active-directory-sign-in-activity-report-api-samples"></a>Beispiele zur Azure Active Directory-Anmeldeaktivitätsbericht-API
Dieser Artikel ist Teil einer Sammlung von Artikeln zur Azure Active Directory-Berichterstellungs-API.  
Die Azure AD-Berichterstellung bietet eine API, mit der Sie unter Verwendung von Code oder zugehörigen Tools auf Daten zur Anmeldeaktivität zugreifen können.  
In diesem Artikel werden Codebeispiele für die **Anmeldeaktivitäts-API** bereitgestellt.

Siehe:

* [Überwachungsprotokolle](overview-reports.md#activity-reports) erhalten Sie konzeptionelle Informationen.
* [Erste Schritte mit der Berichterstellungs-API von Azure Active Directory](concept-reporting-api.md) finden Sie weitere Informationen zur Berichterstellungs-API.


## <a name="prerequisites"></a>Voraussetzungen
Damit Sie die Beispiele in diesem Artikel verwenden können, müssen die [Voraussetzungen für den Zugriff auf die Azure AD-Berichterstellungs-API](howto-configure-prerequisites-for-reporting-api.md) erfüllt sein.  

## <a name="powershell-script"></a>PowerShell-Skript

```powershell

# This script will require the Web Application and permissions setup in Azure Active Directory
$clientID       = "<appid>"             # ApplicationId
$clientSecret   = "<key>"         # Should be a ~44 character string insert your info here
$loginURL       = "https://login.windows.net/"
$tenantdomain   = "<domain>"            # For example, contoso.onmicrosoft.com
$msgraphEndpoint = "https://graph.microsoft.com"
$countOfSignInDocsToBeSavedInAFile = 2000
    
# Get an Oauth 2 access token based on client id, secret and tenant domain
$body       = @{grant_type="client_credentials";resource=$msgraphEndpoint;client_id=$clientID;client_secret=$clientSecret}
$oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body
    
if ($oauth.access_token -ne $null) {
    $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}
    
    $url = "$msgraphEndpoint/beta/auditLogs/signIns"
    Write-Output "Fetching data using Uri: $url"
    $i=0
    $docCount=0
    Do{
        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
        $jsonReport = ($myReport.Content | ConvertFrom-Json).value
        $fetchedRecordCount = $jsonReport.Count
        $docCount = $docCount + $fetchedRecordCount
        $totalFetchedRecordCount = $totalFetchedRecordCount + $fetchedRecordCount
        Write-Output "Fetched $fetchedRecordCount records and saved into SignIns$i.json"
        if($docCount -le $countOfSignInDocsToBeSavedInAFile)
        {
            $myReport.Content | Out-File -FilePath SignIns$i.json -append  -Force       
        }
        else
        {           
            $docCount=0
            $i = $i+1
        }
            
        #Get url from next link
        $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'         
    }while($url -ne $null)
    Write-Output "Total Fetched record count is : $totalFetchedRecordCount"
                
} else {
    Write-Host "ERROR: No Access Token"
}

```




## <a name="executing-the-script"></a>Ausführen des Skripts
Wenn Sie die Bearbeitung des Skripts abgeschlossen haben, führen Sie es aus, und prüfen Sie, ob die erwarteten Daten aus dem Bericht zu den Anmeldeprotokollen zurückgegeben werden.

Das Skript gibt die Ausgabe des Anmeldeberichts im JSON-Format zurück. Darüber hinaus wird eine Datei `SignIns.json` mit derselben Ausgabe erstellt. Sie können damit experimentieren, indem Sie das Skript so verändern, dass es Daten aus anderen Berichten zurückgibt, und nicht benötigte Ausgabeformate auskommentieren.

## <a name="next-steps"></a>Nächste Schritte
* Möchten Sie die Beispiele in diesem Artikel anpassen? Dann sehen Sie sich die [Referenz zur Azure Active Directory-Anmeldeaktivitätsbericht-API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)an. 
* Eine vollständige Übersicht zur Verwendung der Azure Active Directory-Berichterstellungs-API finden Sie im Artikel [Erste Schritte mit der Azure Active Directory-Berichterstellungs-API](concept-reporting-api.md).
* Wenn Sie weitere Informationen zur Azure Active Directory-Berichterstellung benötigen, finden Sie diese im [Leitfaden zur Azure Active Directory-Berichterstellung](overview-reports.md).  

