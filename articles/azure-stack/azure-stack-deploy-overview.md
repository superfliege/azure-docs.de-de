---
title: Evaluieren des Azure Stack Development Kit | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie das Azure Stack Development Kit zu Evaluierungszwecken bereitstellen.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 10/15/2018
ms.author: jeffgilb
ms.custom: mvc
ms.openlocfilehash: bf07fe56c65e53f5485b9927e0d704f80842cf3a
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2018
ms.locfileid: "49338699"
---
# <a name="quickstart-evaluate-the-azure-stack-development-kit"></a>Schnellstart: Evaluieren des Azure Stack Development Kit

Das [Azure Stack Development Kit (ASDK)](.\asdk\asdk-what-is.md) ist eine Test- und Entwicklungsumgebung, die sie zum Evaluieren und Demonstrieren von Azure Stack-Funktionen und -Diensten bereitstellen können. Um das ASDK verwenden zu können, müssen Sie zunächst die Hardware des Hostcomputers vorbereiten und anschließend einige Skripts ausführen. (Die Installation dauert mehrere Stunden.) Anschließend können Sie sich beim Administrator- oder Benutzerportal anmelden und Azure Stack verwenden.

## <a name="prerequisites"></a>Voraussetzungen

### <a name="asdk-host-computer-requirements"></a>Anforderungen für ASDK-Hostcomputer

Vor der Installation des ASDK müssen Sie zunächst den Computer vorbereiten, der das Development Kit hostet. Der Development Kit-Hostcomputer muss die unter **[Überlegungen zur Planung der Azure Stack-Bereitstellung](.\asdk\asdk-deploy-considerations.md)** beschriebenen Hardware-, Software- und Netzwerkanforderungen erfüllen.

> [!TIP]
> Mit dem [Tool zur Überprüfung von Bereitstellungsanforderungen für Azure Stack](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) können Sie sich nach der Installation des Betriebssystems auf dem Development Kit-Hostcomputer vergewissern, dass die Hardware die Anforderungen erfüllt.

### <a name="account-requirements"></a>Kontoanforderungen

Außerdem müssen Sie sich entscheiden, ob Sie Azure Active Directory (Azure AD) oder Active Directory-Verbunddienste (AD FS) als Identitätsanbieter für Ihre Bereitstellung verwenden möchten. Überprüfen Sie die Kontoanforderungen in den **[Überlegungen zur Planung der Azure Stack-Bereitstellung](.\asdk\asdk-deploy-considerations.md#account-requirements)**.

## <a name="download-and-extract-the-deployment-package"></a>Herunterladen und Extrahieren des Bereitstellungspakets

Laden Sie nach der Vorbereitung Ihres Development Kit-Hostcomputers das ASDK-Bereitstellungspaket herunter, und extrahieren Sie es. Das Bereitstellungspaket enthält die Datei „Cloudbuilder.vhdx“. Hierbei handelt es sich um eine virtuelle Festplatte (virtual hard drive, VHD) mit startbarem Betriebssystem und den Azure Stack-Installationsdateien.

Sie können das Bereitstellungspaket auf den Development Kit-Host oder auf einen anderen Computer herunterladen. Die extrahierten Bereitstellungsdateien belegen bis zu 60 GB freien Speicherplatz auf dem Datenträger, durch die Verwendung eines anderen Computers lassen sich also die Speicheranforderungen für den Development Kit-Host reduzieren.

**[Download and extract the Azure Stack Development Kit (ASDK)](.\asdk\asdk-download.md)** (Herunterladen und Extrahieren des Azure Stack Development Kits)

## <a name="prepare-the-host-computer"></a>Vorbereiten des Hostcomputers

Um das ASDK installieren zu können, muss zunächst die Hostumgebung vorbereitet und das System für den Start von der Development Kit-VHD konfiguriert werden. Wenn Sie den Host neu starten, startet er über „CloudBuilder.vhdx“, und Sie können mit der Bereitstellung des ASDK beginnen.

**[Prepare the ASDK host computer](.\asdk\asdk-prepare-host.md)** (Vorbereiten des ASDK-Hostcomputers)

## <a name="install-the-asdk-on-the-host-computer"></a>Installieren des ASDKs auf dem Hostcomputer

Nachdem der Hostcomputer über die virtuelle Festplatte gestartet wurde, können Sie das Development Kit in der virtuellen Cloudbuilder-Umgebung bereitstellen. Sie können das ASDK entweder über die grafische Benutzeroberfläche (graphical user interface, GUI) bereitstellen, die durch Ausführen des PowerShell-Skripts „asdk-installer.ps1“ bereitgestellt wird, oder über die [PowerShell-Befehlszeile](.\asdk\asdk-deploy-powershell.md).

> [!NOTE]
> Nachdem der Host über das Image „CloudBuilder.vhdx“ gestartet wurde, können Sie *vor* der ASDK-Installation optional [Azure Stack-Telemetrieeinstellungen](.\asdk\asdk-telemetry.md#set-telemetry-level-in-the-windows-registry) konfigurieren.

**[Install the Azure Stack Development Kit (ASDK)](.\asdk\asdk-install.md)** (Installieren des Azure Stack Development Kits (ASDK))

## <a name="perform-post-deployment-configurations"></a>Durchführen der Konfigurationsschritte nach der Bereitstellung

Nach der ASDK-Installation empfiehlt es sich, einige Überprüfungen und Konfigurationsänderungen durchzuführen.

**Tools**

Installieren Sie Azure Stack PowerShell- und GitHub-Tools, und überprüfen Sie mithilfe des Cmdlets „test-AzureStack“, ob die Installation erfolgreich war.

**Identitätslösung**

Bei Bereitstellungen, für die Azure AD verwendet wird, müssen Sie sowohl das Azure Stack-Administratorportal als auch das Mandantenportal aktivieren. Mit dieser Aktivierung wird die Zustimmung erteilt, dass dem Azure Stack-Portal und Azure Resource Manager die richtigen Berechtigungen (auf der Zustimmungsseite aufgeführt) für alle Benutzer des Verzeichnisses gewährt werden können.

**Kennwortablauf**

Es empfiehlt sich, die Kennwortablaufrichtlinie zurückzusetzen, um sicherzustellen, dass das Kennwort für den Development Kit-Host nicht vor dem Ende des Evaluierungszeitraums abläuft.

> [!NOTE]
> [Azure Stack-Telemetrieeinstellungen](.\asdk\asdk-telemetry.md#enable-or-disable-telemetry-after-deployment) können optional auch *nach* der ASDK-Installation konfiguriert werden.

**[Post ASDK installation configuration tasks](.\asdk\asdk-post-deploy.md)** (Aufgaben nach der ASDK-Bereitstellung)

## <a name="register-with-azure"></a>Registrieren bei Azure

Sie müssen Azure Stack bei Azure registrieren, um [Azure Marketplace-Elemente in Azure Stack herunterladen](.\asdk\asdk-marketplace-item.md) zu können.

**[Register Azure Stack with Azure](.\asdk\asdk-register.md)** (Registrieren von Azure Stack bei Azure)

## <a name="next-steps"></a>Nächste Schritte

Glückwunsch! Mithilfe der Schritte in dieser Schnellstartanleitung haben Sie eine ASDK-Umgebung mit einem [Administratorportal](https://adminportal.local.azurestack.external) und einem [Benutzerportal](https://portal.local.azurestack.external) eingerichtet.
