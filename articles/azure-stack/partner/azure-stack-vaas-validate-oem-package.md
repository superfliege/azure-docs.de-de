---
title: Überprüfen von OEM-Paketen mit Validation-as-a-Service in Azure Stack | Microsoft-Dokumentation
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
ms.date: 02/19/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 02/19/2019
ROBOTS: NOINDEX
ms.openlocfilehash: f9ed10c84be86304722020606873b0c7866df1e8
ms.sourcegitcommit: a8948ddcbaaa22bccbb6f187b20720eba7a17edc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2019
ms.locfileid: "56594048"
---
# <a name="validate-oem-packages"></a>Überprüfen von OEM-Paketen

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Sie können ein neues OEM-Paket testen, wenn für eine abgeschlossene Lösungsvalidierung eine Änderung an der Firmware oder an Treibern vorgenommen wurde. Wenn Ihr Paket den Test bestanden hat, wird es von Microsoft signiert. Der Test muss das aktualisierte OEM-Erweiterungspaket mit den Treibern und der Firmware enthalten, die die Windows Server-Logo- und PCS-Tests bestanden haben.

[!INCLUDE [azure-stack-vaas-workflow-validation-completion](includes/azure-stack-vaas-workflow-validation-completion.md)]

> [!IMPORTANT]
> Informieren Sie sich vor dem Hochladen oder Übermitteln von Paketen unter [Erstellen eines OEM-Pakets](azure-stack-vaas-create-oem-package.md) über das erwartete Paketformat und die erwarteten Inhalte.

## <a name="managing-packages-for-validation"></a>Verwalten von Paketen für die Überprüfung

Wenn Sie ein Paket unter Verwendung des Workflows **Paketvalidierung** überprüfen, müssen Sie eine URL zu einem **Azure Storage-Blob** angeben. Dieses Blob ist das signierte OEM-Testpaket, das im Rahmen des Updateprozesses installiert wird. Erstellen Sie das Blob unter Verwendung des Azure Storage-Kontos, das Sie im Rahmen der Einrichtung erstellt haben. (Weitere Informationen finden Sie unter [Tutorial: Einrichten von Ressourcen für Validation-as-a-Service](azure-stack-vaas-set-up-resources.md).)

### <a name="prerequisite-provision-a-storage-container"></a>Voraussetzung: Bereitstellen eines Speichercontainers

Erstellen Sie in Ihrem Speicherkonto einen Container für Paketblobs. Dieser Container kann für alle Paketvalidierungsdurchläufe verwendet werden.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu dem Speicherkonto, das Sie in [Tutorial: Einrichten von Ressourcen für Validation-as-a-Service](azure-stack-vaas-set-up-resources.md) erstellt haben.
2. Klicken Sie auf dem linken Blatt unter **Blobdienst** auf **Container**.
3. Klicken Sie auf der Menüleiste auf **+ Container**, und geben Sie einen Namen für den Container an (beispielsweise `vaaspackages`).

### <a name="upload-package-to-storage-account"></a>Hochladen des Pakets in das Speicherkonto

1. Bereiten Sie das Paket vor, das Sie überprüfen möchten. Falls Ihr Paket mehrere Dateien umfasst, komprimieren Sie sie zu einer ZIP-Datei (`.zip`).
2. Wählen Sie im [Azure-Portal](https://portal.azure.com) den Paketcontainer aus, und laden Sie das Paket hoch, indem Sie auf der Menüleiste auf **Hochladen** klicken.
3. Wählen Sie die hochzuladende Paketdatei (`.zip`) aus. Behalten Sie die Standardwerte für **BLOB-Typ** (**Blockblob**) und **Blockgröße** bei.

> [!NOTE]
> Wichtig: Die Inhalte der ZIP-Datei (`.zip`) müssen im Stammverzeichnis der Datei platziert `.zip` werden. Das Paket darf keine Unterordner enthalten.

### <a name="generate-package-blob-url-for-vaas"></a>Generieren der Paketblob-URL für VaaS

Wenn Sie im VaaS-Portal einen Workflow vom Typ **Paketvalidierung** erstellen, müssen Sie eine URL für das Azure Storage-Blob angeben, das Ihr Paket enthält.

#### <a name="option-1-generating-a-blob-sas-url"></a>Option 1: Generieren einer Blob-SAS-URL

Verwenden Sie diese Option, wenn Sie den öffentlichen Lesezugriff auf Ihren Speichercontainer oder Ihre Blobs nicht aktivieren möchten.

1. Wechseln Sie im [Azure-Portal](https://portal.azure.com/) zu Ihrem Speicherkonto, und navigieren Sie zu der ZIP-Datei, die das Paket enthält.

2. Wählen Sie im Kontextmenü **SAS generieren** aus.

3. Wählen Sie unter **Berechtigungen** die Option **Lesen** aus.

4. Legen Sie die **Startzeit** auf die aktuelle Zeit und die **Endzeit** auf mindestens 48 Stunden nach der **Startzeit** fest. Falls Sie andere Tests mit dem gleichen Paket ausführen, können Sie die **Endzeit** ggf. für die Dauer des Tests erhöhen. Bei allen Tests, die über VaaS nach der **Endzeit** geplant werden, tritt ein Fehler auf, und es muss eine neue SAS generiert werden.

5. Wählen Sie **Blob-SAS-Token und URL generieren**.

Verwenden Sie die **Blob-SAS-URL**, wenn Sie Paket-Blob-URLs im Portal bereitstellen.

#### <a name="option-2-grant-public-read-access"></a>Option 2: Gewähren des öffentlichen Lesezugriffs

> [!CAUTION]
> Diese Option ermöglicht anonymen Lesezugriff auf Ihre Blobs.

1. Gehen Sie wie im Abschnitt [Erteilen von anonymen Benutzerberechtigungen für Container und Blobs](https://docs.microsoft.com/azure/storage/storage-manage-access-to-resources#grant-anonymous-users-permissions-to-containers-and-blobs) beschrieben vor, um dem Paketcontainer **öffentlichen Lesezugriff nur für Blobs** zu gewähren.

> [!NOTE]
> Wenn Sie eine Paket-URL für einen *interaktiven Test* bereitstellen (z. B. Monatliche Azure Stack-Updateüberprüfung oder Überprüfung des OEM-Erweiterungspakets), müssen Sie **vollständigen öffentlichen Lesezugriff gewähren**, um mit dem Testen fortfahren zu können.

2. Wählen Sie im Paketcontainer das Paketblob aus, um den Eigenschaftenbereich zu öffnen.

3. Kopieren Sie die **URL**. Verwenden Sie diesen Wert, wenn Sie im Portal Paketblob-URLs bereitstellen.

## <a name="apply-monthly-update"></a>Anwenden des monatlichen Updates

[!INCLUDE [azure-stack-vaas-workflow-section_update-azs](includes/azure-stack-vaas-workflow-section_update-azs.md)]

## <a name="create-a-package-validation-workflow"></a>Erstellen eines Paketvalidierungsworkflows

1. Melden Sie sich beim [VaaS-Portal](https://azurestackvalidation.com) an.

2. [!INCLUDE [azure-stack-vaas-workflow-step_select-solution](includes/azure-stack-vaas-workflow-step_select-solution.md)]

3. Klicken Sie auf der Kachel **Paketvalidierung** auf **Starten**.

    ![Kachel des Paketvalidierungsworkflows](media/tile_validation-package.png)

4. [!INCLUDE [azure-stack-vaas-workflow-step_naming](includes/azure-stack-vaas-workflow-step_naming.md)]

5. Geben Sie die Azure Storage-Blob-URL für das signierte OEM-Testpaket ein, für das eine Signatur von Microsoft benötigt wird. Eine entsprechende Anleitung finden Sie unter [Generieren der Paketblob-URL für VaaS](#generate-package-blob-url-for-vaas).

6. [!INCLUDE [azure-stack-vaas-workflow-step_upload-stampinfo](includes/azure-stack-vaas-workflow-step_upload-stampinfo.md)]

7. [!INCLUDE [azure-stack-vaas-workflow-step_test-params](includes/azure-stack-vaas-workflow-step_test-params.md)]

    > [!NOTE]
    > Umgebungsparameter können nach der Workflowerstellung nicht mehr geändert werden.

8. [!INCLUDE [azure-stack-vaas-workflow-step_tags](includes/azure-stack-vaas-workflow-step_tags.md)]

9. [!INCLUDE [azure-stack-vaas-workflow-step_submit](includes/azure-stack-vaas-workflow-step_submit.md)]
    Sie werden auf die Seite mit der Testzusammenfassung umgeleitet.

## <a name="required-tests"></a>Erforderliche Tests

Die folgenden Tests sind für die OEM-Paketvalidierung erforderlich:

- Überprüfung des OEM-Erweiterungspakets
- Cloud-Simulationsmodul

## <a name="run-package-validation-tests"></a>Ausführen von Paketvalidierungstests

1. Auf der Seite mit der **Package Validation tests summary** (Zusammenfassung der Paketvalidierungstests) führen Sie eine Teilmenge der aufgelisteten Tests durch, die für Ihr Szenario geeignet sind.

    In den Validierungsworkflows werden beim **Planen** eines Tests die allgemeinen Parameter auf der Workflowebene verwendet, die Sie bei der Workflowerstellung angegeben haben (siehe [Allgemeine Workflowparameter für Validation-as-a-Service in Azure Stack](azure-stack-vaas-parameters.md)). Sollt einer der Testparameterwerte ungültig werden, muss er gemäß den Anweisungen unter [Ändern von Workflowparametern](azure-stack-vaas-monitor-test.md#change-workflow-parameters) erneut angegeben werden.

    > [!NOTE]
    > Wenn Sie einen Validierungstest für eine bereits vorhandene Instanz planen, wird die alte Instanz im Portal durch eine neu erstellte Instanz ersetzt. Die Protokolle für die alte Instanz bleiben erhalten, können aber nicht über das Portal verwendet werden.  
    Nach erfolgreicher Ausführung eines Tests wird die **Planungsoption** deaktiviert.

2. Wählen Sie den Agent aus, der den Test ausführt. Informationen zum Hinzufügen lokaler Agents für die Ausführung von Tests finden Sie unter [Bereitstellen des lokalen Agents](azure-stack-vaas-local-agent.md).

3. Wählen Sie zum Durchführen der Überprüfung des OEM-Erweiterungspakets im Kontextmenü die Option **Schedule** (Planen), um eine Eingabeaufforderung zum Planen der Testinstanz zu öffnen.

4. Überprüfen Sie die Testparameter, und wählen Sie dann **Submit** (Absenden), um die Ausführung der Überprüfung des OEM-Erweiterungspakets zu planen.

5. Sehen Sie sich das Ergebnis der Überprüfung des OEM-Erweiterungspakets an. Wenn der Test erfolgreich war, können Sie die Ausführung des Cloud-Simulationsmoduls planen.

Wenn alle Tests erfolgreich abgeschlossen wurden, senden Sie den Namen Ihrer VaaS-Lösung und der Paketvalidierung an [vaashelp@microsoft.com](mailto:vaashelp@microsoft.com), um die Paketsignierung anzufordern.

## <a name="next-steps"></a>Nächste Schritte

- [Überwachen und Verwalten von Tests im VaaS-Portal](azure-stack-vaas-monitor-test.md)