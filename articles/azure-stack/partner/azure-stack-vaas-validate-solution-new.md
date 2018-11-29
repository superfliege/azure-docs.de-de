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
ms.date: 11/26/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 7949e764baa7a4e20eb988c78817b6b4f0045593
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/27/2018
ms.locfileid: "52333767"
---
# <a name="validate-a-new-azure-stack-solution"></a>Überprüfen einer neuen Azure Stack-Lösung

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Hier erfahren Sie, wie Sie den Workflow **Lösungsvalidierung** verwenden, um neue Azure Stack-Lösungen zu zertifizieren.

Eine Azure Stack-Lösung ist eine Hardwareliste, auf die sich Microsoft und der Partner nach Erfüllung der Anforderungen für die Windows Server-Logo-Zertifizierung geeinigt haben. Nach einer Änderung der Hardwareliste muss die Lösung erneut zertifiziert werden. Sollten Sie weitere Fragen zur erneuten Zertifizierung von Lösungen haben, wenden Sie sich unter [vaashelp@microsoft.com](mailto:vaashelp@microsoft.com) an das zuständige Team.

Führen Sie den Lösungsvalidierungsworkflow zweimal aus, um Ihre Lösung zu zertifizieren. Führen Sie ihn einmal für die unterstützte *minimale* Konfiguration aus. Führen Sie ihn ein zweites Mal für die *maximale* unterstützte Konfiguration aus. Microsoft zertifiziert die Lösung, wenn beide Konfigurationen alle Tests bestehen.

[!INCLUDE [azure-stack-vaas-workflow-validation-completion](includes/azure-stack-vaas-workflow-validation-completion.md)]

## <a name="create-a-solution-validation-workflow"></a>Erstellen eines Lösungsvalidierungsworkflows

1. [!INCLUDE [azure-stack-vaas-workflow-step_select-solution](includes/azure-stack-vaas-workflow-step_select-solution.md)]
2. Klicken Sie auf der Kachel **Solution Validations** (Lösungsvalidierungen) auf **Starten**.

    ![Kachel des Lösungsvalidierungsworkflows](media/tile_validation-solution.png)

3. [!INCLUDE [azure-stack-vaas-workflow-step_naming](includes/azure-stack-vaas-workflow-step_naming.md)]
4. Wählen Sie die **Lösungskonfiguration** aus.
    - **Minimum**: Die Lösung wird mit der unterstützten Mindestanzahl von Knoten konfiguriert.
    - **Maximum**: Die Lösung wird mit der maximal unterstützten Anzahl von Knoten konfiguriert.
5. [!INCLUDE [azure-stack-vaas-workflow-step_upload-stampinfo](includes/azure-stack-vaas-workflow-step_upload-stampinfo.md)]

    ![Informationen zur Lösungsvalidierung](media/workflow_validation-solution_info.png)

6. [!INCLUDE [azure-stack-vaas-workflow-step_test-params](includes/azure-stack-vaas-workflow-step_test-params.md)]

    > [!NOTE]
    > Umgebungsparameter können nach der Workflowerstellung nicht mehr geändert werden.

7. [!INCLUDE [azure-stack-vaas-workflow-step_tags](includes/azure-stack-vaas-workflow-step_tags.md)]
8. [!INCLUDE [azure-stack-vaas-workflow-step_submit](includes/azure-stack-vaas-workflow-step_submit.md)]
    Sie werden auf die Seite mit der Testzusammenfassung umgeleitet.

## <a name="execute-solution-validation-tests"></a>Ausführen von Lösungsvalidierungstests

Auf der Zusammenfassungsseite der **Lösungsvalidierungstests** wird eine Liste mit den erforderlichen Tests für die Überprüfung angezeigt.

[!INCLUDE [azure-stack-vaas-workflow-validation-section_schedule](includes/azure-stack-vaas-workflow-validation-section_schedule.md)]

![Planen eines Lösungsvalidierungstests](media/workflow_validation-solution_schedule-test.png)

## <a name="next-steps"></a>Nächste Schritte

- [Überwachen und Verwalten von Tests im VaaS-Portal](azure-stack-vaas-monitor-test.md)