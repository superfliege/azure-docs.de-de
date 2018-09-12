---
title: Überprüfen einer neuen Azure Stack-Lösung | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie eine neue Azure Stack-Lösung mit Validation-as-a-Service überprüfen.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 1e908a8cf5576ce3bc3d58d1ef6f29d596ebc58b
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44158176"
---
# <a name="validate-a-new-azure-stack-solution"></a>Überprüfen einer neuen Azure Stack-Lösung

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Lesen Sie, wie Sie mithilfe des Workflows für die Lösungsvalidierung neue Azure Stack-Lösungen zertifizieren.

Eine Azure Stack-Lösung ist eine mit Microsoft vereinbarte Hardwareliste, die den Zertifizierungsanforderungen für das Windows Server-Logo entspricht. Sie können den Workflow für die Lösungsvalidierung auch verwenden, wenn eine Lösung aufgrund einer Änderung an der Hardwareliste als *neu* klassifiziert wird. Sollten Sie Fragen zu den Auslösern für eine Klassifizierung als **neu** bzw. für eine **erneute Zertifizierung** haben, können Sie sich an [vaashelp@microsoft.com](mailto:vaashelp@microsoft.com) wenden.

Führen Sie den Workflow zweimal aus, um Ihre Lösung zu zertifizieren. Führen Sie ihn einmal für die unterstützte *minimale* Konfiguration aus. Und führen Sie ihn ein zweites Mal für die *maximale* Konfiguration aus. Microsoft zertifiziert die Lösung, wenn beide Konfigurationen alle Tests bestehen.

In dieser Schnellstartanleitung erhalten Sie eine Einführung in den Prozess zum Hinzufügen Ihrer Lösung und zum Ausführen der Tests.

## <a name="add-a-new-solution"></a>Hinzufügen einer neuen Lösung

1. Melden Sie sich beim [Validierungsportal](https://azurestackvalidation.com) an.
2. Klicken Sie auf **Neue Lösung**.
3. Geben Sie einen Namen für die Lösung ein, und wählen Sie **Speichern**.

## <a name="create-a-solution-validation-workflow"></a>Erstellen eines Lösungsvalidierungsworkflows

1. Wählen Sie den Namen der Lösung aus.
2. Klicken Sie auf der Kachel **Solution Validations** (Lösungsvalidierungen) auf **Verwalten**.

    ![Lösungsvalidierungen](media/image2.png)

## <a name="create-a-solution-workflow"></a>Erstellen eines Lösungsworkflows

1. Klicken Sie auf **New solution validation** (Neue Lösungsvalidierung).
2. Geben Sie den Namen für die Validierung ein.
3. Wählen Sie **Minimum** oder **Maximum** aus.  
    - **Mindestanforderungen**  
    Die Lösung wird mit der unterstützten Mindestanzahl von Knoten konfiguriert.  
    - **Maximum**  
    Die Lösung wird mit der unterstützten maximalen Anzahl von Knoten konfiguriert.
4. Klicken Sie auf **Hochladen**, und fügen Sie dann Ihre Bereitstellungskonfigurationsdatei hinzu. Dieser Schritt ist optional. Ihre Testparameter können auch mithilfe der Schritte im nächsten Abschnitt hinzugefügt werden.

    > [!note]  
    > Sie können die Konfigurationsdatei erstellen, indem Sie die Parameter auf der Oberfläche in den Abschnitten für Umgebungsparameter und allgemeine Testparameter hinzufügen. Sie können die Datei abrufen, die vom Dienst der gerade überprüften Azure Stack-Bereitstellung generiert wird. Anweisungen hierzu finden Sie unter [Workflow common parameters for Azure Stack validation as a service](azure-stack-vaas-parameters.md) (Allgemeine Workflowparameter für Azure Stack Validation-as-a-Service).

5. Fügen Sie Ihre Umgebungsparameter hinzu. Weitere Informationen finden Sie unter [Hinzufügen von Umgebungsparametern](#add-environmental-parameters).
6. Fügen Sie Ihre allgemeinen Testparameter hinzu. Weitere Informationen finden Sie unter [Hinzufügen allgemeiner Testparameter](#add-common-test-parameters).

    Je nach Testdefinition müssen Sie für den Test unter Umständen unabhängig von den allgemeinen Parametern einen Wert eingeben. Ggf. ist es auch möglich, den Wert des allgemeinen Parameters zu überschreiben.

7. Klicken Sie auf **Übermitteln**, um den Test zu planen.

## <a name="add-environmental-parameters"></a>Hinzufügen von Umgebungsparametern

Fügen Sie die folgenden Umgebungsparameter hinzu:

| Informationen zum Testdurchlauf | Erforderlich | BESCHREIBUNG |
| --- | --- | --- | --- |
| Azure Stack-Build | Erforderlich | Azure Stack-Buildnummer (z.B. 20170501.1). Der Wert muss eine gültige Azure Stack-Buildnummer oder -Version sein, etwa 1.0.170330.9. |
| Mandanten-ID | Erforderlich | Active Directory-Mandanten-ID. Hier muss eine GUID (z.B. ECA23256-6BA0-4F27-8E4D-AFB02F088363) angegeben werden. |
| Region | Erforderlich | Azure Stack-Bereitstellungsregion |
| Resource Manager-Endpunkt des Mandanten | Erforderlich | Endpunkt für Azure Resource Manager-Vorgänge des Mandanten (z.B. https://management.loc-ext.domain.com) |
| Resource Manager-Endpunkt des Administrators | Nicht erforderlich | Endpunkt für Azure Resource Manager-Vorgänge des Mandanten (z.B. https://management.loc-ext.domain.com) |
| Externer FQDN | Nicht erforderlich | Externer vollqualifizierter Domänenname, der als Suffix für Endpunkte verwendet wird (Beispiel: local.azurestack.external oder redmond.contoso.com) |
| Anzahl von Knoten | Erforderlich | Die Anzahl von Knoten in Ihrer Lösung |

## <a name="add-common-test-parameters"></a>Hinzufügen allgemeiner Testparameter

Fügen Sie die folgenden allgemeinen Testparameter hinzu:

| Informationen zum Testdurchlauf | Erforderlich | BESCHREIBUNG |
| --- | --- | --- |
| Mandantenbenutzername | Erforderlich | Mandantenbenutzername (z.B. tenant@contoso.onmicrosoft.com) |
| Mandantenkennwort | Erforderlich | Das Kennwort für den Mandanten |
| Benutzername des Dienstadministrators | Nicht erforderlich | Mandantenbenutzername (z.B. tenant@contoso.onmicrosoft.com) |
| Kennwort des Dienstadministrators | Nicht erforderlich | Benutzername des Dienstadministrators (z.B. serviceadmin@contoso.onmicrosoft.com) |
| Benutzername des Cloudadministrators | Nicht erforderlich | Konto des Azure Stack-Domänenadministrators (z.B. „contoso\cloudadmin“) |
| Kennwort des Cloudadministrators | Nicht erforderlich | |
|  Diagnoseverbindungszeichenfolge | Nicht erforderlich | SAS-URI zu einem Azure Storage-Konto, in das während der Testausführung Diagnoseprotokolle kopiert werden. Siehe [Set up your validation as a service account](azure-stack-vaas-set-up-account.md#create-an-azure-storage-blob-to-store-logs) (Einrichten eines Validation-as-a-Service-Kontos) <br><br>Der Wert des allgemeinen Parameters **Diagnoseverbindungszeichenfolge** wird vom Dienst gespeichert und zum geplanten Zeitpunkt für alle Tests im Workflow bereitgestellt, die diesen Parameter nutzen. Wenn die SAS-URL innerhalb der nächsten 30 Tage abläuft, werden Sie auf der Seite für allgemeine Parameter zur Eingabe einer neuen SAS-URL aufgefordert. |
| Tag – Name | Nicht erforderlich |  Zum Bezeichnen des Workflows können beschreibende Tags eingegeben werden. Dies ist der Name des Tags. |
| Tag – Wert | Nicht erforderlich | Zum Bezeichnen des Workflows können beschreibende Tags eingegeben werden. Dies ist der Wert des Tags. |

## <a name="next-steps"></a>Nächste Schritte

- [Neuplanen oder Abbrechen eines Tests](azure-stack-vaas-monitor-test.md#reschedule-a-test)
- Lesen Sie mehr über [Azure Stack Validation-as-a-Service](https://docs.microsoft.com/azure/azure-stack/partner).