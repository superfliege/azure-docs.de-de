---
title: Überprüfen von Softwareupdates von Microsoft mit Azure Stack Validation-as-a-Service | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Softwareupdates von Microsoft mit Validation-as-a-Service überprüfen.
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
ms.openlocfilehash: 6ef8c0486a694ac44c53375b24893812b10343e4
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44158482"
---
# <a name="validate-software-updates-from-microsoft"></a>Überprüfen der Softwareupdates von Microsoft

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Microsoft veröffentlicht in regelmäßigen Abständen Updates für die Azure Stack-Software. Diese Updates werden Azure Stack-Partnern vor der öffentlichen Bereitstellung zur Verfügung gestellt, damit diese die Updates zusammen mit ihren Lösungen überprüfen und Feedback an Microsoft senden können.

## <a name="test-an-existing-solution"></a>Testen einer vorhandenen Lösung

1. Melden Sie sich beim [Validierungsportal](https://azurestackvalidation.com) an.

2. Wählen Sie eine vorhandene Lösung aus, für die das Microsoft-Update angewendet wurde, und klicken Sie auf der Kachel **Paketvalidierung** auf **Starten**.

    ![Paketvalidierung](media/image3.png)

3. Geben Sie den Validierungsnamen ein.

4. Geben Sie die URL zum OEM-Paket an, das bei der Bereitstellung für die Lösung installiert wurde. Verwenden Sie die URL für das im Azure-Blobdienst gespeicherte Paket. Weitere Informationen finden Sie unter [Set up your validation as a service account](azure-stack-vaas-set-up-account.md#create-an-azure-storage-blob-to-store-logs) (Einrichten eines Validation-as-a-Service-Kontos).

5. Klicken Sie auf **Hochladen**, um Ihre Bereitstellungskonfigurationsdatei hinzuzufügen. Informationen zum Hochladen der Bereitstellungskonfigurationsdatei finden Sie unter [Überprüfen einer neuen Azure Stack-Lösung](azure-stack-vaas-validate-solution-new.md).

6. Die Bereitstellungskonfigurationsdatei muss anschließend mit der richtigen Umgebungsparameterdatei angepasst werden. Weitere Einzelheiten finden Sie unter [Workflow common parameters for Azure Stack validation as a service](azure-stack-vaas-parameters.md#environment-parameters) (Allgemeine Workflowparameter für Azure Stack Validation-as-a-Service).

    > [!Note]   
    > Die Bereitstellungskonfigurationsdatei kann durch Hinzufügen allgemeiner Testparameter weiter angepasst werden. Weitere Informationen finden Sie unter [Workflow common parameters for Azure Stack validation as a service](azure-stack-vaas-parameters.md) (Allgemeine Workflowparameter für Azure Stack Validation-as-a-Service).

7. Die Benutzernamen und die Kennwörter für den Mandantenbenutzer, den Dienstadministrator und den Cloudadministrator müssen manuell eingegeben werden.

8. Geben Sie die URL zu Azure Storage Blob ein, um die Diagnoseprotokolle zu speichern. Weitere Informationen finden Sie unter [Set up your validation as a service account](azure-stack-vaas-set-up-account.md#create-an-azure-storage-blob-to-store-logs) (Einrichten eines Validation-as-a-Service-Kontos).

    > [!Note]  
    > Zum Bezeichnen des Workflows können beschreibende Tags eingegeben werden.

10. Klicken Sie auf **Übermitteln**, um den Workflow zu speichern.

Der Lösungsworkflow wird ungefähr 24 Stunden lang ausgeführt. Fügen Sie einen Link oder eine Anweisung zum Planen der Tests hinzu. Löschen Sie die Angaben im Tool.

Weitere Informationen zum Überwachen des Status einer Validierungsausführung finden Sie unter [Monitor a test with Azure Stack validation as a service](azure-stack-vaas-monitor-test.md) (Überwachen eines Tests mit Azure Stack Validation-as-a-Service).

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr zu [Validation-as-a-Service in Azure Stack](https://docs.microsoft.com/azure/azure-stack/partner).
