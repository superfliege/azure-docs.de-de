---
title: "Generieren von PKI-Zertifikaten für Azure Stack für die Bereitstellung von in Azure Stack integrierten Systemen | Microsoft-Dokumentation"
description: "Beschreibt den Bereitstellungsprozess für Azure Stack-PKI-Zertifikate für in Azure Stack integrierte Systeme."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2018
ms.author: jeffgilb
ms.reviewer: ppacent
ms.openlocfilehash: 991a94e4ca41bad438a3c8d06e4e1f691cff91bc
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/24/2018
---
# <a name="generate-pki-certificates-for-azure-stack-deployment"></a>Generieren von PKI-Zertifikaten für die Azure Stack-Bereitstellung
Nachdem Sie nun [die PKI-Zertifikatanforderungen](azure-stack-pki-certs.md) für Azure Stack-Bereitstellungen kennen, müssen Sie diese Zertifikate von der Zertifizierungsstelle Ihrer Wahl beziehen. 

## <a name="request-certificates-using-an-inf-file"></a>Anfordern von Zertifikaten mithilfe einer INF-Datei
Eine Möglichkeit zum Anfordern von Zertifikaten von einer öffentlichen oder internen Zertifizierungsstelle ist die Verwendung einer INF-Datei. Das in Windows integrierte Hilfsprogramm CERTREQ.EXE kann eine INF-Datei mit detaillierten Angaben zum Zertifikat verwenden, um eine Anforderungsdatei zu erzeugen, was in diesem Abschnitt beschrieben wird. 

### <a name="sample-inf-file"></a>Beispiel einer INF-Datei 
Die INF-Beispieldatei zum Anfordern von Zertifikaten kann verwendet werden, um eine Offline-Zertifikatsanforderungsdatei zur Übermittlung an eine Zertifizierungsstelle (intern oder öffentlich) zu erstellen. Die INF-Datei deckt alle erforderlichen Endpunkte (einschließlich der optionalen PaaS-Dienste) in einem einzigen Platzhalterzertifikat ab. 

Die INF-Beispieldatei geht davon aus, dass die Region **sea** und der externe FQDN-Wert **sea&#46;contoso&#46;com** ist. Ändern Sie diese Werte entsprechend Ihrer Umgebung, bevor Sie für Ihre Bereitstellung eine INF-Datei generieren. 

    
    [Version] 
    Signature="$Windows NT$"

    [NewRequest] 
    Subject = "C=US, O=Microsoft, L=Redmond, ST=Washington, CN=portal.sea.contoso.com"

    Exportable = TRUE                   ; Private key is not exportable 
    KeyLength = 2048                    ; Common key sizes: 512, 1024, 2048, 4096, 8192, 16384 
    KeySpec = 1                         ; AT_KEYEXCHANGE 
    KeyUsage = 0xA0                     ; Digital Signature, Key Encipherment 
    MachineKeySet = True                ; The key belongs to the local computer account 
    ProviderName = "Microsoft RSA SChannel Cryptographic Provider" 
    ProviderType = 12 
    SMIME = FALSE 
    RequestType = PKCS10
    HashAlgorithm = SHA256

    ; At least certreq.exe shipping with Windows Vista/Server 2008 is required to interpret the [Strings] and [Extensions] sections below

    [Strings] 
    szOID_SUBJECT_ALT_NAME2 = "2.5.29.17" 
    szOID_ENHANCED_KEY_USAGE = "2.5.29.37" 
    szOID_PKIX_KP_SERVER_AUTH = "1.3.6.1.5.5.7.3.1" 
    szOID_PKIX_KP_CLIENT_AUTH = "1.3.6.1.5.5.7.3.2"

    [Extensions] 
    %szOID_SUBJECT_ALT_NAME2% = "{text}dns=*.sea.contoso.com&dns=*.blob.sea.contoso.com&dns=*.queue.sea.contoso.com&dns=*.table.sea.contoso.com&dns=*.vault.sea.contoso.com&dns=*.adminvault.sea.contoso.com&dns=*.dbadapter.sea.contoso.com&dns=*.appservice.sea.contoso.com&dns=*.scm.appservice.sea.contoso.com&dns=api.appservice.sea.contoso.com&dns=ftp.appservice.sea.contoso.com&dns=sso.appservice.sea.contoso.com&dns=adminportal.sea.contoso.com&dns=management.sea.contoso.com&dns=adminmanagement.sea.contoso.com" 
    %szOID_ENHANCED_KEY_USAGE% = "{text}%szOID_PKIX_KP_SERVER_AUTH%,%szOID_PKIX_KP_CLIENT_AUTH%"

    [RequestAttributes]
    

## <a name="generate-and-submit-request-to-the-ca"></a>Generieren und Übermitteln der Anforderung an die Zertifizierungsstelle
Der folgende Workflow beschreibt, wie Sie die zuvor erzeugte INF-Beispieldatei anpassen und verwenden können, um ein Zertifikat von einer Zertifizierungsstelle anzufordern:

1. **Bearbeiten und speichern Sie die INF-Datei**. Kopieren Sie das bereitgestellte Beispiel, und speichern Sie es in einer neuen Textdatei. Ersetzen Sie den Antragstellernamen und den externen FQDN mit den Werten, die Ihrer Bereitstellung entsprechen, und speichern Sie die Datei als INF-Datei.
2. **Generieren Sie eine Anforderung mithilfe von CERTREQ**. Öffnen Sie auf einem Windows-Computer eine Eingabeaufforderung als Administrator, und führen Sie den folgenden Befehl aus, um eine Anforderungsdatei (.req) zu erzeugen: `certreq -new <yourinffile>.inf <yourreqfilename>.req`.
3. **Übermitteln Sie die Datei an die Zertifizierungsstelle**. Senden Sie die generierte REQ-Datei an Ihre (interne oder öffentliche) Zertifizierungsstelle.
4. **Importieren Sie die CER-Datei**. Die Zertifizierungsstelle gibt eine CER-Datei zurück. Importieren Sie auf dem Windows-Computer, auf dem Sie die Anforderungsdatei erstellt haben, die CER-Datei, die an den Computer bzw. in den persönlichen Speicher zurückgegeben wurde. 
5. **Exportieren und kopieren Sie die PFX-Datei in die Bereitstellungsordner**. Exportieren Sie das Zertifikat (einschließlich des privaten Schlüssels) als PFX-Datei, und kopieren Sie die PFX-Datei in die Bereitstellungsordner, die unter [PKI-Anforderungen für die Azure Stack-Bereitstellung](azure-stack-pki-certs.md) angegeben sind.

## <a name="next-steps"></a>Nächste Schritte
[Vorbereiten von Azure Stack-PKI-Zertifikaten](prepare-pki-certs.md)