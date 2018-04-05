---
title: Azure Stack-PKI-Zertifikatanforderungen für in Azure Stack integrierte Systeme | Microsoft-Dokumentation
description: Beschreibt die Azure Stack-PKI-Zertifikatanforderungen für in Azure Stack integrierte Systeme.
services: azure-stack
documentationcenter: ''
author: mabriggs
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: 455c74ca808f71258a12166c2e36bdd73d9a3e20
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="azure-stack-public-key-infrastructure-certificate-requirements"></a>Azure Stack-PKI-Zertifikatanforderungen
Azure Stack verfügt über ein öffentliches Infrastrukturnetz mit extern zugänglichen öffentlichen IP-Adressen, die einer kleinen Gruppe von Azure Stack-Diensten und möglicherweise Mandanten-VMs zugewiesen sind. PKI-Zertifikate (Public Key-Infrastruktur) mit den entsprechenden DNS-Namen für diese Endpunkte der öffentlichen Infrastruktur von Azure Stack werden während der Bereitstellung von Azure Stack benötigt. Dieser Artikel enthält Informationen zu Folgendem:

- Den zum Bereitstellen von Azure Stack erforderlichen Zertifikaten
- Der Prozess des Beziehens der Zertifikate, die diesen Vorgaben entsprechen
- Vorbereiten, Überprüfen und Verwenden dieser Zertifikate während der Bereitstellung

> [!NOTE]
> Während der Bereitstellung müssen Sie Zertifikate in den Bereitstellungsordner kopieren, der zu dem Identitätsanbieter gehört, für den Sie die Bereitstellung ausführen (Azure AD oder AD FS). Wenn Sie ein einzelnes Zertifikat für alle Endpunkte verwenden, müssen Sie diese Zertifikatdatei, wie in den folgenden Tabellen aufgeführt, in jeden Bereitstellungsordner kopieren. Die Ordnerstruktur ist bereits im virtuellen Computer für die Bereitstellung unter folgendem Pfad festgelegt: C:\CloudDeployment\Setup\Certificates. 

## <a name="certificate-requirements"></a>Zertifikatanforderungen
Die folgende Liste beschreibt die Zertifikatsanforderungen, die für die Bereitstellung von Azure Stack erfüllt sein müssen: 
- Zertifikate müssen von einer internen oder öffentlichen Zertifizierungsstelle ausgestellt werden. Wenn eine öffentliche Zertifizierungsstelle verwendet wird, muss diese im Rahmen des Microsoft Trusted Root Authority Program in das Basisbetriebssystem-Image aufgenommen werden. Die vollständige Liste finden Sie hier: https://gallery.technet.microsoft.com/Trusted-Root-Certificate-123665ca 
- Das Zertifikat kann ein einzelnes Platzhalterzertifikat sein, das alle Namespaces im Feld „Alternativer Antragstellername“ abdeckt. Alternativ können Sie auch einzelne Zertifikate mit Platzhaltern für Endpunkte wie ACS und Key Vault verwenden, wo sie benötigt werden. 
- Der Zertifikatsignaturalgorithmus darf nicht SHA1, sondern muss sicherer sein. 
- Das Zertifikatsformat muss PFX sein, da sowohl der öffentliche als auch der private Schlüssel für die Installation von Azure Stack benötigt werden. 
- Die PFX-Zertifikatdateien müssen im Feld „Schlüsselverwendung“ einen Wert in „Digitale Signatur“ und „Schlüsselchiffrierung“ enthalten.
- Die PFX-Zertifikatdateien müssen die Werte „Serverauthentifizierung (1.3.6.1.5.5.7.3.1)“ und „Clientauthentifizierung (1.3.6.1.5.5.7.3.2)“ im Feld „Erweiterte Schlüsselverwendung“ aufweisen.
- Das Feld „Ausgestellt für:“ des Zertifikats darf nicht mit dem Feld „Ausgestellt von:“ identisch sein.
- Die Kennwörter aller PFX-Zertifikatdateien müssen zum Zeitpunkt der Bereitstellung identisch sein.
- Stellen Sie sicher, dass die Antragstellernamen und alternativen Antragstellernamen aller Zertifikate mit den in diesem Artikel beschriebenen Vorgaben übereinstimmen, um fehlerhafte Implementierungen zu vermeiden.

> [!NOTE]
> Das Vorhandensein von Zwischenzertifizierungsstellen in der Vertrauenskette eines Zertifikats wird ausdrücklich unterstützt. 

## <a name="mandatory-certificates"></a>Erforderliche Zertifikate
Die Tabelle in diesem Abschnitt beschreibt die PKI-Zertifikate für öffentliche Azure Stack-Endpunkte, die sowohl für Azure AD- als auch für AD FS-gestützte Azure Stack-Installationen erforderlich sind. Zertifikatsanforderungen werden nach Bereichen gruppiert, ebenso wie die verwendeten Namespaces und die Zertifikate, die für jeden Namespace benötigt werden. Die Tabelle beschreibt auch den Ordner, in den Ihr Lösungsanbieter die verschiedenen Zertifikate für jeden öffentlichen Endpunkt kopiert. 

Zertifikate mit den entsprechenden DNS-Namen für diese Endpunkte der öffentlichen Infrastruktur von Azure Stack sind erforderlich. Der DNS-Name von Endpunkten wird im folgenden Format angegeben: *&lt;Präfix>.&lt;Region>.&lt;FQDN>*. 

Für Ihre Bereitstellung müssen die Werte [region] und [externalfqdn] mit der Region und den externen Domänennamen übereinstimmen, die Sie für Ihr Azure Stack-System ausgewählt haben. Beispiel: Wenn der Name der Region *Redmond* und der externe Domänenname *contoso.com* lautet, haben die DNS-Namen das Format *&lt;Präfix>.redmond.contoso.com*. Die *&lt;Präfix>*-Werte werden von Microsoft vordefiniert, um den durch das Zertifikat geschützten Endpunkt zu beschreiben. Darüber hinaus hängen die *&lt;Präfix>*-Werte der externen Infrastrukturendpunkte vom Azure Stack-Dienst ab, der den spezifischen Endpunkt verwendet. 

|Bereitstellungsordner|Erforderlicher Zertifikatantragsteller und alternative Antragstellernamen|Bereich (pro Region)|Namespace der Unterdomäne|
|-----|-----|-----|-----|
|Öffentliches Portal|portal.*&lt;region>.&lt;fqdn>*|Portale|*&lt;region>.&lt;fqdn>*|
|Verwaltungsportal|adminportal.*&lt;region>.&lt;fqdn>*|Portale|*&lt;region>.&lt;fqdn>*|
|Azure Resource Manager (Öffentlich)|management.*&lt;region>.&lt;fqdn>*|Azure Resource Manager|*&lt;region>.&lt;fqdn>*|
|Azure Resource Manager (Verwaltung)|adminmanagement.*&lt;region>.&lt;fqdn>*|Azure Resource Manager|*&lt;region>.&lt;fqdn>*|
|ACS<sup>1</sup>|Ein Platzhalterzertifikat für mehrere Unterdomänen mit alternativen Antragstellernamen für:<br>&#42;.blob.*&lt;region>.&lt;fqdn>*<br>&#42;.queue.*&lt;region>.&lt;fqdn>*<br>&#42;.table.*&lt;region>.&lt;fqdn>*|Speicher|blob.*&lt;region>.&lt;fqdn>*<br>table.*&lt;region>.&lt;fqdn>*<br>queue.*&lt;region>.&lt;fqdn>*|
|KeyVault|&#42;.vault.*&lt;region>.&lt;fqdn>*<br>(SSL-Platzhalterzertifikat)|Schlüsseltresor|vault.*&lt;region>.&lt;fqdn>*|
|KeyVaultInternal|&#42;.adminvault.*&lt;region>.&lt;fqdn>*<br>(SSL-Platzhalterzertifikat)|Interner Schlüsseltresor|adminvault.*&lt;region>.&lt;fqdn>*|
|
<sup>1</sup> Das ACS-Zertifikat erfordert drei Platzhalter für alternative Antragstellernamen auf einem einzigen Zertifikat. Mehrere Platzhalter für alternative Antragstellernamen auf einem einzigen Zertifikat werden ggf. nicht von allen öffentlichen Zertifizierungsstellen unterstützt. 

Wenn Sie Azure Stack im Azure AD-Bereitstellungsmodus bereitstellen, müssen Sie nur die in der vorigen Tabelle aufgeführten Zertifikate anfordern. Wenn Sie jedoch Azure Stack mit dem AD FS-Bereitstellungsmodus bereitstellen, müssen Sie auch die in der folgenden Tabelle beschriebenen Zertifikate anfordern:

|Bereitstellungsordner|Erforderlicher Zertifikatantragsteller und alternative Antragstellernamen|Bereich (pro Region)|Namespace der Unterdomäne|
|-----|-----|-----|-----|
|ADFS|adfs.*&lt;region>.&lt;fqdn>*<br>(SSL-Zertifikat)|ADFS|*&lt;region>.&lt;fqdn>*|
|Graph|graph.*&lt;region>.&lt;fqdn>*<br>(SSL-Zertifikat)|Graph|*&lt;region>.&lt;fqdn>*|
|

> [!IMPORTANT]
> Alle Zertifikate, die in diesem Abschnitt aufgeführt sind, müssen das gleiche Kennwort haben. 

## <a name="optional-paas-certificates"></a>Optionale PaaS-Zertifikate
Wenn Sie die zusätzlichen Azure Stack-PaaS-Dienste (SQL, MySQL und App Service) bereitstellen möchten, nachdem Azure Stack bereitgestellt und konfiguriert wurde, müssen Sie zusätzliche Zertifikate anfordern, um die Endpunkte der PaaS-Dienste abzudecken. 

> [!IMPORTANT]
> Die Zertifikate, die Sie für App Service-, SQL- und MySQL-Ressourcenanbieter verwenden, müssen die gleiche Stammzertifizierungsstelle haben wie die Zertifikate, die für die globalen Azure Stack-Endpunkte verwendet werden. 

In der folgenden Tabelle werden die Endpunkte und Zertifikate beschrieben, die für die SQL- und MySQL-Adapter sowie für App Service erforderlich sind. Sie müssen diese Zertifikate nicht in den Azure Stack-Bereitstellungsordner kopieren. Stattdessen stellen Sie diese Zertifikate bei der Installation der zusätzlichen Ressourcenanbieter bereit. 

|Bereich (pro Region)|Zertifikat|Erforderlicher Zertifikatantragsteller und alternative Antragstellernamen|Namespace der Unterdomäne|
|-----|-----|-----|-----|
|SQL, MySQL|SQL und MySQL|&#42;.dbadapter.*&lt;region>.&lt;fqdn>*<br>(SSL-Platzhalterzertifikat)|dbadapter.*&lt;region>.&lt;fqdn>*|
|App Service|Webdatenverkehr: SSL-Standardzertifikat|&#42;.appservice.*&lt;region>.&lt;fqdn>*<br>&#42;.scm.appservice.*&lt;region>.&lt;fqdn>*<br>(SSL-Platzhalterzertifikat für mehrere Domänen<sup>1</sup>)|appservice.*&lt;region>.&lt;fqdn>*<br>scm.appservice.*&lt;region>.&lt;fqdn>*|
|App Service|API|api.appservice.*&lt;region>.&lt;fqdn>*<br>(SSL-Zertifikat<sup>2</sup>)|appservice.*&lt;region>.&lt;fqdn>*<br>scm.appservice.*&lt;region>.&lt;fqdn>*|
|App Service|FTP|ftp.appservice.*&lt;region>.&lt;fqdn>*<br>(SSL-Zertifikat<sup>2</sup>)|appservice.*&lt;region>.&lt;fqdn>*<br>scm.appservice.*&lt;region>.&lt;fqdn>*|
|App Service|SSO|sso.appservice.*&lt;region>.&lt;fqdn>*<br>(SSL-Zertifikat<sup>2</sup>)|appservice.*&lt;region>.&lt;fqdn>*<br>scm.appservice.*&lt;region>.&lt;fqdn>*|

<sup>1</sup> Erfordert ein Zertifikat mit Platzhaltern für mehrere alternative Antragstellernamen. Mehrere Platzhalter für alternative Antragstellernamen auf einem einzigen Zertifikat werden ggf. nicht von allen öffentlichen Zertifizierungsstellen unterstützt. 

<sup>2</sup> Ein Platzhalterzertifikat des Typs &#42;.appservice.*&lt;region>.&lt;fqdn>* kann nicht anstelle dieser drei Zertifikate (api.appservice.*&lt;region>.&lt;fqdn>*, ftp.appservice.*&lt;region>.&lt;fqdn>* und sso.appservice.*&lt;region>.&lt;fqdn>*) verwendet werden. App Service erfordert explizit die Verwendung separater Zertifikate für diese Endpunkte. 

## <a name="learn-more"></a>Weitere Informationen
Erfahren Sie mehr zum [Generieren von PKI-Zertifikaten für die Azure Stack-Bereitstellung](azure-stack-get-pki-certs.md). 

## <a name="next-steps"></a>Nächste Schritte
[Identitätsintegration](azure-stack-integrate-identity.md)

