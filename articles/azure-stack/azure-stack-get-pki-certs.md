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
ms.date: 05/17/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: 69a4529e009d9fdd7081bc4d4b53c468befd8e6d
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/18/2018
ms.locfileid: "34304357"
---
# <a name="azure-stack-certificates-signing-request-generation"></a>Generieren von Signieranforderungen für Azure Stack-Zertifikate

Das in diesem Artikel beschriebene Tool Azure Stack Readiness Checker steht im [PowerShell-Katalog](https://aka.ms/AzsReadinessChecker) zur Verfügung. Das Tool erstellt Zertifikatsignieranforderung, die für eine Azure Stack-Bereitstellung geeignet sind. Zertifikate müssen mit genügend Zeit zum Testen vor dem Einsatz angefordert, generiert und validiert werden.

Das Tool Azure Stack Readiness Checker (AzsReadinessChecker) führt die folgenden Zertifikatanforderung aus:

 - **Standardzertifikatanforderungen**  
    Stellen Sie eine Anforderung zum [Generieren von PKI-Zertifikaten für die Azure Stack-Bereitstellung](azure-stack-get-pki-certs.md).
 - **Anforderungstyp**  
    Gibt an, ob die Zertifikatsignieranforderung eine einzelne Anforderung oder mehrere Anforderungen sein soll.
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
 
  > [!NOTE]
  > Wenn Sie Ihre Zertifikate von Ihrer Zertifizierungsstelle erhalten haben, müssen Sie auf demselben System die Schritte in [Vorbereiten von Azure Stack-PKI-Zertifikaten](azure-stack-prepare-pki-certs.md) ausführen!

## <a name="generate-certificate-signing-requests"></a>Generieren der Zertifikatsignieranforderungen

Gehen Sie wie folgt vor, um die Azure Stack-PKI-Zertifikate vorzubereiten und zu überprüfen: 

1.  Führen Sie an einer PowerShell-Eingabeaufforderung (5.1 oder höher) das folgende Cmdlet aus, um AzsReadinessChecker zu installieren:

    ````PowerShell  
        Install-Module Microsoft.AzureStack.ReadinessChecker
    ````

2.  Deklarieren Sie den **Antragsteller** als sortiertes Wörterbuch. Beispiel:  

    ````PowerShell  
    $subjectHash = [ordered]@{"OU"="AzureStack";"O"="Microsoft";"L"="Redmond";"ST"="Washington";"C"="US"} 
    ````
    > [!note]  
    > Wenn ein allgemeiner Name (CN) angegeben ist, wird dieser vom ersten DNS-Namen der Zertifikatanforderung überschrieben.

3.  Deklarieren Sie ein Ausgabeverzeichnis, das bereits vorhanden ist. Beispiel: 

    ````PowerShell  
    $outputDirectory = "$ENV:USERPROFILE\Documents\AzureStackCSR"
    ````
4.  Deklarieren Sie das Identitätssystem.

    Azure Active Directory

    ```PowerShell
    $IdentitySystem = "AAD"
    ````

    Active Directory-Verbunddienste (AD FS)

    ```PowerShell
    $IdentitySystem = "ADFS"
    ````

5. Deklarieren Sie einen **Regionsnamen** und **externen FQDN** für die Azure Stack-Bereitstellung.

    ```PowerShell
    $regionName = 'east'
    $externalFQDN = 'azurestack.contoso.com'
    ````

    > [!note]  
    > `<regionName>.<externalFQDN>` bildet die Grundlage für die Erstellung aller externen DNS-Namen in Azure Stack. In diesem Beispiel heißt das Portal `portal.east.azurestack.contoso.com`.  

6. So generieren Sie eine einzige Zertifikatsanforderung mit mehreren alternativen Antragstellernamen:

    ```PowerShell  
    Start-AzsReadinessChecker -RegionName $regionName -FQDN $externalFQDN -subject $subjectHash -RequestType SingleCSR -OutputRequestPath $OutputDirectory -IdentitySystem $IdentitySystem
    ````

    Sollen PaaS-Dienste einbezogen werden, geben Sie den Schalter ```-IncludePaaS``` an.

7. So generieren Sie einzelne Zertifikatsignieranforderungen für jeden DNS-Namen:

    ```PowerShell  
    Start-AzsReadinessChecker -RegionName $regionName -FQDN $externalFQDN -subject $subjectHash -RequestType MultipleCSR -OutputRequestPath $OutputDirectory -IdentitySystem $IdentitySystem
    ````

    Sollen PaaS-Dienste einbezogen werden, geben Sie den Schalter ```-IncludePaaS``` an.

8. Überprüfen Sie die Ausgabe:

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

9.  Übermitteln Sie die generierte **REQ**-Datei an Ihre (interne oder öffentliche) Zertifizierungsstelle.  Das Ausgabeverzeichnis von **Start-AzsReadinessChecker** enthält die Zertifikatsignieranforderungen, die für die Übermittlung an eine Zertifizierungsstelle erforderlich sind.  Es enthält auch ein Unterverzeichnis, das die INF-Dateien, die bei der Generierung von Zertifikatanforderungen verwendet werden, als Referenz enthält. Vergewissern Sie sich, dass Ihre Zertifizierungsstelle Zertifikate mit Ihrer generierten Anforderung erstellt, die die [Azure Stack-PKI-Voraussetzungen](azure-stack-pki-certs.md) erfüllen.

## <a name="next-steps"></a>Nächste Schritte

[Vorbereiten von Azure Stack-PKI-Zertifikaten](azure-stack-prepare-pki-certs.md)
