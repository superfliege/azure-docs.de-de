---
title: Überprüfen von OEM-Paketen mit Azure Stack Validation-as-a-Service | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie OEM-Pakete (Original Equipment Manufacturer, Originalgerätehersteller) mit Validation-as-a-Service überprüfen.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: cefa32c35df4e87d4d2b983ee8c4a16dc065e774
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44160454"
---
# <a name="validate-oem-packages"></a>Überprüfen von OEM-Paketen

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Sie können ein neues OEM-Paket testen, wenn für eine abgeschlossene Lösungsvalidierung eine Änderung an der Firmware oder an Treibern vorgenommen wurde. Wenn Ihr Paket den Test bestanden hat, wird es von Microsoft signiert. Der Test muss das aktualisierte OEM-Erweiterungspaket mit den Treibern und der Firmware enthalten, die die Windows Server-Logo- und PCS-Tests bestanden haben.

Alle Tests werden innerhalb von 24 Stunden mit dem Ergebnis **Erfolgreich** abgeschlossen. Wenn einer der Tests das Ergebnis **Fehlerhaft** hat, melden Sie einen Fehler in [Microsoft Collaborate](https://aka.ms/collaborate), und benachrichtigen Sie Microsoft per E-Mail an [vaashelp@microsoft.com](mailto:vaashelp@microsoft.com).

## <a name="get-your-oem-package-signed"></a>Signieren des OEM-Pakets

1. Stellen Sie sicher, dass das aktuelle monatliche Update angewendet wurde. Die neuste Version finden Sie in der [Dokumentation für Azure Stack-Bediener unter „Übersicht“ > „Versionshinweise“](https://docs.microsoft.com/azure/azure-stack/).

    Den Namen von Microsoft-Softwareupdates für Azure Stack liegt eine Benennungskonvention zugrunde. „1803“ bedeutet beispielsweise, dass es sich um ein Update für März 2018 handelt. Informationen zur Richtlinie, zum Rhythmus und zu Versionshinweisen von Azure Stack-Updates finden Sie unter [Azure Stack-Wartungsrichtlinie](https://docs.microsoft.com/azure/azure-stack/azure-stack-servicing-policy).

1. Überprüfen Sie den Systemintegritätsstatus, indem Sie **Test-AzureStack** ausführen wie unter „Ausführen eines Validierungstests für Azure Stack“ beschrieben. Beheben Sie vor dem Starten von Tests alle Warnungen und Fehler.

2. Wählen Sie im [Validierungsportal](https://azurestackvalidation.com) eine vorhandene Lösung aus. Falls Sie Ihre Lösung nicht hinzugefügt haben, lesen Sie die Informationen zum [Hinzufügen einer neuen Lösung](azure-stack-vaas-validate-solution-new.md#add-a-new-solution).

3. Klicken Sie auf der Kachel **Package Validations** (Paketvalidierungen) auf **Starten**, um einen neuen Workflow zu starten.

    ![Paketvalidierungen](media/image3.png)

4.  Geben Sie eine Diagnoseverbindungszeichenfolge an. Anweisungen hierzu finden Sie unter [Set up your validation as a service account](azure-stack-vaas-set-up-account.md) (Einrichten eines Validation-as-a-Service-Kontos).

    Für jede Paketvalidierungsausführung muss ein OEM-Erweiterungspaket angegeben werden. Geben Sie das OEM-Paket an, das bei der Azure Stack-Bereitstellung für die Lösung installiert wurde. Anweisungen finden Sie unter [Set up your validation as a service account](azure-stack-vaas-set-up-account.md#create-an-azure-storage-blob-to-store-logs) (Einrichten eines Validation-as-a-Service-Kontos).

    Sie müssen eine JSON-Datei mit den Umgebungsvariablen verwenden, um die Eingabe für erforderliche Felder der Ausführung abzuschließen und Fehler in der Dateneingabe zu vermeiden. Anweisungen finden Sie unter [Workflow common parameters for Azure Stack validation as a service](azure-stack-vaas-parameters.md) (Allgemeine Workflowparameter für Azure Stack Validation-as-a-Service).

5. Führen Sie die Tests aus.

6. Wenn alle Tests erfolgreich abgeschlossen wurden, senden Sie den Namen Ihrer Lösung und Paketvalidierung an [vaashelp@microsoft.com](mailto:vaashelp@microsoft.com), um die Paketsignierung anzufordern.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr zu [Validation-as-a-Service in Azure Stack](https://docs.microsoft.com/azure/azure-stack/partner).
