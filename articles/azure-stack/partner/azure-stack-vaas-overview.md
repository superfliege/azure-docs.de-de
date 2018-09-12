---
title: Übersicht über Validation-as-a-Service für Azure Stack| Microsoft-Dokumentation
description: Eine Übersicht über bekannte Probleme von Validation-as-a-Service für Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 56251245a23df031f3bc8fe3d36de43e194fbcc7
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44159672"
---
# <a name="what-is-validation-as-a-service-for-azure-stack"></a>Was ist Validation-as-a-Service für Azure Stack?

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Validation-as-a-Service (VaaS) ist ein nativer Azure-Dienst, der für Lösungspartner konzipiert wurde, die zusammen mit Microsoft Azure Stack-Angebote entwickeln. Lösungspartner können mithilfe dieses Diensts überprüfen, ob ihre Lösungen die Anforderungen von Microsoft erfüllen und in Azure Stack wie erwartet funktionieren.

VaaS wird vorrangig für folgende Zwecke eingesetzt:

- Überprüfen neuer Azure Stack-Lösungen
- Überprüfen von Änderungen an der Azure Stack-Software
- Abrufen digital signierter Pakete von Lösungspartnern, die während der Bereitstellung verwendet wurden
- Vorschau des Begleitmaterials zur Überprüfung von Azure Stack

## <a name="validate-new-azure-stack-solution"></a>Überprüfen einer neuen Azure Stack-Lösung

Partner überprüfen mithilfe des Workflows für die Lösungsvalidierung neue Azure Stack-Lösungen. Die Lösung muss die erforderlichen HKL-Komponententests (HKL = Hardware Lab Kit) von Azure Stack bestehen. Sie müssen den Workflow für jede neue Lösung nur zweimal ausführen: einmal für die Mindestkonfiguration und einmal für die Maximalkonfiguration.

Weitere Informationen finden Sie unter [Überprüfen einer neuen Azure Stack-Lösung](azure-stack-vaas-validate-solution-new.md).

## <a name="validate-changes-to-the-azure-stack-software"></a>Überprüfen von Änderungen an der Azure Stack-Software

Partner überprüfen mithilfe des Workflows für die Paketvalidierung, ob ihre Lösungen mit dem aktuellen Update der Azure Stack-Software ausgeführt werden können. Der Workflow für die Paketvalidierung muss zumindest in der von Microsoft empfohlenen Hardwareumgebung und in einer Lösung ausgeführt werden, in der das Patch und Update (P&U) zur Anwendung des Updates verwendet wurden. Sie sollten die Paketvalidierung im Baselinebuild ausführen.

Weitere Informationen finden Sie unter [Überprüfen der Softwareupdates von Microsoft](azure-stack-vaas-validate-microsoft-updates.md).

## <a name="get-digitally-signed-solution-partner-packages"></a>Abrufen digital signierter Pakete von Lösungspartnern

Neben der Validierung von Azure Stack-Updates können Sie mithilfe des Workflows für die Paketvalidierung Updates für benutzerdefinierte OEM-Pakete überprüfen. Diese umfassen partnerspezifische Azure Stack-Treiber, Firmware und weitere Software, die während der Bereitstellung von Azure Stack-Software verwendet wird. Stellen Sie das Paket, das Sie in der aktuellen Version der Azure Stack-Software überprüfen, zumindest mit der Lösung in der unterstützten Mindestgröße bereit. Das aktualisierte Paket muss vor Beginn des Tests in dem Dienst hochgeladen werden. Benachrichtigen Sie vaashelp@microsoft.com, wenn die Tests erfolgreich waren. Teilen Sie dem Azure Stack-Partner mit, dass die Tests für das Paket abgeschlossen wurden, und dass das Paket mit der digitalen Signatur von Azure Stack digital signiert werden sollte. Das Paket wird von Microsoft signiert. Zudem benachrichtigt Microsoft den Azure Stack-Partner darüber, dass das Paket zum Herunterladen im Portal verfügbar ist.

Weitere Informationen finden Sie unter [Überprüfen von OEM-Paketen](azure-stack-vaas-validate-oem-package.md).

## <a name="preview-azure-stack-validation-collateral"></a>Vorschau des Begleitmaterials zur Überprüfung von Azure Stack

Microsoft stellt in Azure Stack regelmäßig neue Features zur Verfügung. Im Rahmen des Entwicklungsprozesses für die Bereitstellung dieser Features auf dem Markt wird im Workflow für den Testdurchlauf neues Begleitmaterial für Tests zur Verfügung gestellt. Der Workflow für den Testdurchlauf umfasst Begleitmaterial für Tests aus den anderen Workflows, damit Tests inoffiziell ausgeführt werden können. Verwenden Sie den Workflow für den Testdurchlauf nicht, um Ergebnisse zur Genehmigung zu senden. Verwenden Sie den Workflow für die Lösungsvalidierung und den Workflow für die Paketvalidierung, um eine offizielle Genehmigung für Ihre Lösung zu erhalten.

## <a name="next-steps"></a>Nächste Schritte

- Erste Schritte und [Einrichten Ihres Validation-as-a-Service-Kontos](azure-stack-vaas-validate-solution-new.md)
