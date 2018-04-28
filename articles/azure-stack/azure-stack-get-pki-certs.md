---
title: Generieren von PKI-Zertifikaten für Azure Stack für die Bereitstellung von in Azure Stack integrierten Systemen | Microsoft-Dokumentation
description: Beschreibt den Bereitstellungsprozess für Azure Stack-PKI-Zertifikate für in Azure Stack integrierte Systeme.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/11/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: 349661352d17b015d4c605b39f1e42aa482949ac
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2018
---
# <a name="azure-stack-certificates-signing-request-generation"></a>Generieren von Signieranforderungen für Azure Stack-Zertifikate

Das in diesem Artikel beschriebene Tool Azure Stack Readiness Checker steht im [PowerShell-Katalog](https://aka.ms/AzsReadinessChecker) zur Verfügung. Das Tool erstellt Zertifikatsignieranforderung, die für eine Azure Stack-Bereitstellung geeignet sind. Zertifikate müssen mit genügend Zeit zum Testen vor dem Einsatz angefordert, generiert und validiert werden. 

Das Tool Azure Stack Readiness Checker (AzsReadinessChecker) führt die folgenden Zertifikatanforderung aus:

 - **Standardzertifikatanforderungen**  
    Stellen Sie eine Anforderung zum [Generieren von PKI-Zertifikaten für die Azure Stack-Bereitstellung](azure-stack-get-pki-certs.md). 
 - **Anforderungstyp**  
    Sie können mehrere Platzhalter-SAN-, mehrere Domänen- bzw. einzelne Platzhalterzertifikate anfordern.
 - **Platform-as-a-Service**  
    Fordern Sie optional PaaS-Namen (Platform-as-a-Service ) für Zertifikate an, wie unter [Azure Stack-PKI-Zertifikatanforderungen: Optionale PaaS-Zertifikate](azure-stack-pki-certs.md#optional-paas-certificates) angegeben.

## <a name="prerequisites"></a>Voraussetzungen

Ihr System muss die folgenden Voraussetzungen erfüllen, bevor Sie die Zertifikatsignieranforderungen für PKI-Zertifikate für eine Azure Stack-Bereitstellung generieren:

 - Microsoft Azure Stack Readiness Checker
 - Zertifikatattribute:
    - Regionsname
    - Externer vollqualifizierter Domänenname (FQDN)
    - Antragsteller
 - Windows 10 oder Windows Server 2016

## <a name="generate-certificate-signing-requests"></a>Generieren der Zertifikatsignieranforderungen

Gehen Sie wie folgt vor, um die Azure Stack-PKI-Zertifikate vorzubereiten und zu überprüfen: 

1.  Installieren Sie AzsReadinessChecker an einer PowerShell-Eingabeaufforderung (5.1 oder höher), indem Sie das folgende Cmdlet ausführen:

    ````PowerShell  
        Install-Module Microsoft.AzureStack.ReadinessChecker
    ````

2.  Deklarieren Sie den **Antragsteller** als sortiertes Wörterbuch. Beispiel:  

    ````PowerShell  
    $subjectHash = [ordered]@{"OU"="AzureStack";"O"="Microsoft";"L"="Redmond";"ST"="Washington";"C"="US"} 
    ````
    > [!note]  
    > Wenn ein allgemeiner Name (CN) angegeben ist, wird dieser vom ersten DNS-Namen der Zertifikatanforderung überschrieben.

3.  Deklarieren Sie ein Ausgabeverzeichnis, das bereits vorhanden ist:

    ````PowerShell  
    $outputDirectory = "$ENV:USERNAME\Documents\AzureStackCSR" 
    ````

4. Deklarieren Sie einen **Regionsnamen** und **externen FQDN** für die Azure Stack-Bereitstellung.

    ```PowerShell  
    $regionName = 'east'
    $externalFQDN = 'azurestack.contoso.com'
    ````

    > [!note]  
    > `<regionName>.<externalFQDN>` bildet die Grundlage für die Erstellung aller externen DNS-Namen in Azure Stack. In diesem Beispiel heißt das Portal `portal.east.azurestack.contoso.com`.

5. So generieren Sie eine einzige Zertifikatsanforderung mit mehreren alternativen Antragstellernamen, einschließlich derjenigen, die für PaaS-Dienste benötigt werden

    ```PowerShell  
    Start-AzsReadinessChecker -RegionName $regionName -FQDN $externalFQDN -subject $subjectHash -RequestType MultipleSAN -OutputRequestPath $OutputDirectory -IncludePaaS
    ````

6. So generieren Sie individuelle Zertifikatsignieranforderungen für jeden DNS-Namen ohne PaaS-Dienste

    ```PowerShell  
    Start-AzsReadinessChecker -RegionName $regionName -FQDN $externalFQDN -subject $subjectHash -RequestType SingleSAN -OutputRequestPath $OutputDirectory
    ````

7. Überprüfen Sie die Ausgabe:

    ````PowerShell  
    AzsReadinessChecker v1.1803.405.3 started
    Starting Certificate Request Generation

    CSR generating for following SAN(s): dns=*.east.azurestack.contoso.com&dns=*.blob.east.azurestack.contoso.com&dns=*.queue.east.azurestack.contoso.com&dns=*.table.east.azurestack.cont
    oso.com&dns=*.vault.east.azurestack.contoso.com&dns=*.adminvault.east.azurestack.contoso.com&dns=portal.east.azurestack.contoso.com&dns=adminportal.east.azurestack.contoso.com&dns=ma
    nagement.east.azurestack.contoso.com&dns=adminmanagement.east.azurestack.contoso.com
    Present this CSR to your Certificate Authority for Certificate Generation: C:\Users\username\Documents\AzureStackCSR\wildcard_east_azurestack_contoso_com_CertRequest_20180405233530.req
    Certreq.exe output: CertReq: Request Created

    Finished Certificate Request Generation

    AzsReadinessChecker Log location: C:\Program Files\WindowsPowerShell\Modules\Microsoft.AzureStack.ReadinessChecker\1.1803.405.3\AzsReadinessChecker.log
    AzsReadinessChecker Completed
    ````

8.  Übermitteln Sie die generierte **REQ**-Datei an Ihre (interne oder öffentliche) Zertifizierungsstelle.  Das Ausgabeverzeichnis von **Start-AzsReadinessChecker** enthält die Zertifikatsignieranforderungen, die für die Übermittlung an eine Zertifizierungsstelle erforderlich sind.  Es enthält auch ein Unterverzeichnis, das die INF-Dateien, die bei der Generierung von Zertifikatanforderungen verwendet werden, als Referenz enthält. Vergewissern Sie sich, dass Ihre Zertifizierungsstelle Zertifikate mit Ihrer generierten Anforderung erstellt, die die [Azure Stack-PKI-Voraussetzungen](azure-stack-pki-certs.md) erfüllen.

## <a name="next-steps"></a>Nächste Schritte

[Vorbereiten von Azure Stack-PKI-Zertifikaten](azure-stack-prepare-pki-certs.md)

