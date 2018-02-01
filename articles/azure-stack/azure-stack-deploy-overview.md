---
title: Evaluieren des Azure Stack Development Kit | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie das Azure Stack Development Kit zu Evaluierungszwecken bereitstellen.
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 01/22/2018
ms.author: jeffgilb
ms.custom: mvc
ms.openlocfilehash: 4ad2e0a91e2fd5023417722fc0a1a6fae93960d0
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2018
---
# <a name="quickstart-evaluate-the-azure-stack-development-kit"></a>Schnellstart: Evaluieren des Azure Stack Development Kit

*Gilt für: Azure Stack Development Kit*

Das [Azure Stack Development Kit](azure-stack-poc.md) ist eine Test- und Entwicklungsumgebung, die sie zum Evaluieren und Demonstrieren von Azure Stack-Funktionen und -Diensten bereitstellen können. Um sie einsatzbereit zu machen, müssen Sie die Umgebungshardware vorbereiten und einige Skripts ausführen (dies nimmt mehrere Stunden in Anspruch). Anschließend können Sie sich bei den Administrator- und Benutzerportalen anmelden, um Azure Stack zu verwalten und Angebote zu testen. 

1. [**Planen Sie die Hardware, Software und das Netzwerk**](azure-stack-deploy.md). Der Computer, der das Development Kit hostet (der Development Kit-Host) muss die Hardware-, Software- und Netzwerkanforderungen erfüllen. Sie müssen auch zwischen der Verwendung von Azure Active Directory oder Active Directory-Verbunddienste (AD FS) wählen. Achten Sie darauf, diese Voraussetzungen vor dem Beginn der Bereitstellung zu erfüllen, damit der Installationsvorgang reibungslos ausgeführt wird. 

2. [**Laden Sie das Bereitstellungspaket herunter und extrahieren Sie es**](azure-stack-run-powershell-script.md#download-and-extract-the-development-kit). Sie können das Bereitstellungspaket auf den Development Kit-Host oder einen anderen Computer herunterladen. Die extrahierten Bereitstellungsdateien belegen bis zu 60 GB freien Speicherplatz auf dem Datenträger, durch die Verwendung eines anderen Computers lassen sich also die Hardwareanforderungen an den Development Kit-Host reduzieren.

3. [**Bereiten Sie den Development Kit Host**](azure-stack-run-powershell-script.md) mithilfe des Installationsprogramms vor. Nach diesem Schritt startet der Development Kit-Host „Cloudbuilder.vhdx“ (eine virtuelle Festplatte mit startbarem Betriebssystem und den Azure Stack-Installationsdateien).

4. [**Stellen Sie das Development Kit**](azure-stack-run-powershell-script.md) auf dem Development Kit-Host bereit.

5. Wenn Azure Active Directory für Ihre Azure Stack-Bereitstellung verwendet wird, müssen Sie [Azure Stack bei Azure registrieren](azure-stack-register.md), damit Sie [Azure Marketplace-Elemente](azure-stack-download-azure-marketplace-item.md) in Azure Stack herunterladen können.

Nach Abschluss dieser Schritte verfügen Sie über eine Development Kit-Umgebung mit Administrator- und Benutzerportalen. Als Nächstes können Sie [eine Verbindung mit dem Portal herstellen und sich anmelden](azure-stack-connect-azure-stack.md). Dann können Sie mit der Bereitstellung von Ressourcenanbietern, dem Erstellen von [Angeboten](azure-stack-key-features.md#regions-services-plans-offers-and-subscriptions) und dem Auffüllen von Azure Stack [Marketplace](azure-stack-marketplace.md) beginnen.
