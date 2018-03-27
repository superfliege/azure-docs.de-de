---
title: Bereitstellen des Azure Stack Development Kit | Microsoft-Dokumentation
description: In diesem Tutorial installieren Sie das ASDK unter Verwendung der Installationsskripts.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: de31ed75b79ddb3832e32fad141ea7aef806d4d3
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/17/2018
---
# <a name="tutorial-deploy-the-asdk-using-the-installer"></a>Tutorial: Bereitstellen des ASDKs mithilfe des Installationsprogramms
In diesem Tutorial stellen Sie das Azure Stack Development Kit (ASDK) in einer produktionsfremden Umgebung bereit. 

Das ASDK ist eine Test- und Entwicklungsumgebung, die sie zum Evaluieren und Demonstrieren von Azure Stack-Funktionen und -Diensten bereitstellen können. Um das ASDK verwenden zu können, müssen Sie zunächst die Hardware des Hostcomputers vorbereiten und anschließend einige Skripts ausführen. (Die Installation dauert mehrere Stunden.) Anschließend können Sie sich beim Administrator- und Benutzerportal anmelden und Azure Stack verwenden.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Herunterladen und Extrahieren des Bereitstellungspakets
> * Vorbereiten des Development Kit-Hostcomputers 
> * Installieren des ASDKs auf dem Hostcomputer
> * Durchführen der Konfigurationsschritte nach der Bereitstellung
> * Registrieren bei Azure

## <a name="prerequisites"></a>Voraussetzungen 
Vor der Installation des ASDKs müssen Sie zunächst den Computer vorbereiten, der das Development Kit hostet (der Development Kit-Host). Der Development Kit-Host muss die Mindestanforderungen für Hardware, Software und Netzwerk erfüllen. 

Außerdem müssen Sie sich entscheiden, ob Sie Azure Active Directory (Azure AD) oder Active Directory-Verbunddienste (AD FS) als Identitätsanbieter für Ihre Bereitstellung verwenden möchten. 

Wichtig: Die Hardware des Development Kit-Hosts muss die Mindestanforderungen erfüllen, und die Entscheidung hinsichtlich der Identitätslösung muss vor der Bereitstellung getroffen werden, um eine reibungslose Installation zu gewährleisten. 

Die Überlegungen zur Bereitstellungsplanung für das ASDK finden Sie **[hier](asdk-deploy-considerations.md)**.

> [!TIP]
> Mit dem [Tool zur Überprüfung von Bereitstellungsanforderungen für Azure Stack](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) können Sie sich nach der Installation des Betriebssystems auf dem Development Kit-Hostcomputer vergewissern, dass die Hardware die Anforderungen erfüllt.

## <a name="download-and-extract-the-deployment-package"></a>Herunterladen und Extrahieren des Bereitstellungspakets
Nachdem Sie sich vergewissert haben, dass Ihr Development Kit-Hostcomputer die Mindestanforderungen für die ASDK-Installation erfüllt, können Sie im nächsten Schritt das ASDK-Bereitstellungspaket herunterladen und extrahieren. Das Bereitstellungspaket enthält die Datei „Cloudbuilder.vhdx“. Hierbei handelt es sich um eine virtuelle Festplatte mit startbarem Betriebssystem und den Azure Stack-Installationsdateien.

Sie können das Bereitstellungspaket auf den Development Kit-Host oder auf einen anderen Computer herunterladen. Die extrahierten Bereitstellungsdateien belegen bis zu 60 GB freien Speicherplatz auf dem Datenträger, durch die Verwendung eines anderen Computers lassen sich also die Hardwareanforderungen an den Development Kit-Host reduzieren.

**[Download and extract the Azure Stack Development Kit (ASDK)](asdk-download.md)** (Herunterladen und Extrahieren des Azure Stack Development Kits)

## <a name="prepare-the-development-kit-host-computer"></a>Vorbereiten des Development Kit-Hostcomputers
Bevor Sie das ASDK auf dem Hostcomputer installieren können, muss zunächst die Umgebung vorbereitet und das System für den Start von einer VHD konfiguriert werden. Nach der Vorbereitung des Development Kit-Hostcomputers startet der Computer von der Festplatte des virtuellen Computers (CloudBuilder.vhdx), und Sie können mit der ASDK Bereitstellung beginnen.

**[Prepare the ASDK host computer](asdk-prepare-host.md)** (Vorbereiten des ASDK-Hostcomputers)

## <a name="install-the-asdk-on-the-host-computer"></a>Installieren des ASDKs auf dem Hostcomputer
Nach der Vorbereitung des Development Kit-Hostcomputers kann das ASDK im Image „CloudBuilder.vhdx“ bereitgestellt werden. Das ASDK kann über eine grafische Benutzeroberfläche (GUI) bereitgestellt werden. Laden Sie hierzu das PowerShell-Skript „asdk-installer.ps1“ herunter, und führen Sie es aus. Die Bereitstellung kann aber auch vollständig über die [Befehlszeile](asdk-deploy-powershell.md) erfolgen. 

> [!NOTE]
> Optional: Nachdem der Hostcomputer mit „CloudBuilder.vhdx“ gestartet wurde, können Sie *vor* der ASDK-Installation [Azure Stack-Telemetrieeinstellungen](asdk-telemetry.md#set-telemetry-level-in-the-windows-registry) konfigurieren.


**[Install the Azure Stack Development Kit (ASDK)](asdk-install.md)** (Installieren des Azure Stack Development Kits (ASDK))

## <a name="perform-post-deployment-configurations"></a>Durchführen der Konfigurationsschritte nach der Bereitstellung
Nach der ASDK-Installation empfiehlt es sich, einige Überprüfungen und Konfigurationsänderungen durchzuführen. Sie können mit dem Cmdlet „test-AzureStack“ überprüfen, ob die Installation erfolgreich war, und Azure Stack PowerShell sowie GitHub-Tools installieren. 

Nach Bereitstellungen, für die Azure AD verwendet wird, müssen Sie sowohl das Azure Stack-Administratorportal als auch das Mandantenportal aktivieren. Mit dieser Aktivierung wird die Zustimmung erteilt, dass dem Azure Stack-Portal und Azure Resource Manager die richtigen Berechtigungen (auf der Zustimmungsseite aufgeführt) für alle Benutzer des Verzeichnisses gewährt werden können.

Darüber hinaus sollten Sie die Kennwortablaufrichtlinie zurücksetzen, um sicherzustellen, dass das Kennwort für den Development Kit-Host nicht vor dem Ende des Evaluierungszeitraums abläuft.

> [!NOTE]
> Optional können Sie [Azure Stack-Telemetrieeinstellungen](asdk-telemetry.md#enable-or-disable-telemetry-after-deployment) auch *nach* der ASDK-Installation konfigurieren.

**[Post ASDK installation configuration tasks](asdk-post-deploy.md)** (Aufgaben nach der ASDK-Bereitstellung)

## <a name="register-with-azure"></a>Registrieren bei Azure
Sie müssen Azure Stack bei Azure registrieren, um [Azure Marketplace-Elemente in Azure Stack herunterladen](asdk-marketplace-item.md) zu können.

**[Register Azure Stack with Azure](asdk-register.md)** (Registrieren von Azure Stack bei Azure)

## <a name="next-steps"></a>Nächste Schritte
Glückwunsch! Nach Abschluss dieser Schritte verfügen Sie über eine Development Kit-Umgebung mit [Administratorportal](https://adminportal.local.azurestack.external) und [Benutzerportal](https://portal.local.azurestack.external). 

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Herunterladen und Extrahieren des Bereitstellungspakets
> * Vorbereiten des Development Kit-Hostcomputers 
> * Installieren des ASDKs auf dem Hostcomputer
> * Durchführen der Konfigurationsschritte nach der Bereitstellung
> * Registrieren bei Azure

Im nächsten Tutorial erfahren Sie, wie Sie ein Azure Stack-Marketplace-Element hinzufügen.

> [!div class="nextstepaction"]
> [Hinzufügen eines Azure Stack-Marketplace-Elements](asdk-marketplace-item.md)




