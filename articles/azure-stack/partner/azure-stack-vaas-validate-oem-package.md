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
ms.date: 10/19/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: bcfc4cb65c94e34e9f6056ada53726f88489fefb
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2018
ms.locfileid: "49646650"
---
# <a name="validate-oem-packages"></a>Überprüfen von OEM-Paketen

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Sie können ein neues OEM-Paket testen, wenn für eine abgeschlossene Lösungsvalidierung eine Änderung an der Firmware oder an Treibern vorgenommen wurde. Wenn Ihr Paket den Test bestanden hat, wird es von Microsoft signiert. Der Test muss das aktualisierte OEM-Erweiterungspaket mit den Treibern und der Firmware enthalten, die die Windows Server-Logo- und PCS-Tests bestanden haben.

[!INCLUDE [azure-stack-vaas-workflow-validation-completion](includes/azure-stack-vaas-workflow-validation-completion.md)]

> [!IMPORTANT]
> Informieren Sie sich vor dem Hochladen oder Übermitteln von Paketen unter [Erstellen eines OEM-Pakets](azure-stack-vaas-create-oem-package.md) über das erwartete Paketformat und die erwarteten Inhalte.

## <a name="managing-packages-for-validation"></a>Verwalten von Paketen für die Überprüfung

Wenn Sie ein Paket unter Verwendung des Workflows **Paketvalidierung** überprüfen, müssen Sie eine URL zu einem **Azure Storage-Blob** angeben. Bei diesem Blob handelt es sich um das OEM-Paket, das beim Bereitstellen für die Lösung installiert wurde. Erstellen Sie das Blob unter Verwendung des Azure Storage-Kontos, das Sie im Rahmen der Einrichtung erstellt haben. (Weitere Informationen finden Sie unter [Tutorial: Einrichten von Ressourcen für Validation-as-a-Service](azure-stack-vaas-set-up-resources.md).)

### <a name="prerequisite-provision-a-storage-container"></a>Voraussetzung: Erstellen eines Speichercontainers

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

#### <a name="option-1-generating-an-account-sas-url"></a>Option 1: Generieren einer Konto-SAS-URL

1. [!INCLUDE [azure-stack-vaas-sas-step_navigate](includes/azure-stack-vaas-sas-step_navigate.md)]

1. Aktivieren Sie die Option **Blob** unter **Zulässige Dienste**. Deaktivieren Sie alle anderen Optionen.

1. Aktivieren Sie unter **Zugelassene Ressourcentypen** die Optionen **Container** und **Objekt**. Deaktivieren Sie alle anderen Optionen.

1. Aktivieren Sie unter **Zugelassene Berechtigungen** die Optionen **Lesen** und **Auflisten**. Deaktivieren Sie alle anderen Optionen.

1. Legen Sie die **Startzeit** auf die aktuelle Zeit und die **Endzeit** auf eine Stunde nach der aktuellen Zeit fest.

1. [!INCLUDE [azure-stack-vaas-sas-step_generate](includes/azure-stack-vaas-sas-step_generate.md)]
    Das Format sollte wie folgt aussehen: `https://storageaccountname.blob.core.windows.net/?sv=2016-05-31&ss=b&srt=co&sp=rl&se=2017-05-11T21:41:05Z&st=2017-05-11T13:41:05Z&spr=https`

1. Ändern Sie die generierte SAS-URL, um den Paketcontainer (`{containername}`) und den Namen Ihres Paketblobs (`{mypackage.zip}`) einzuschließen: `https://storageaccountname.blob.core.windows.net/{containername}/{mypackage.zip}?sv=2016-05-31&ss=b&srt=co&sp=rl&se=2017-05-11T21:41:05Z&st=2017-05-11T13:41:05Z&spr=https`

    Verwenden Sie diesen Wert, wenn Sie im VaaS-Portal einen neuen Workflow vom Typ **Paketvalidierung** starten.

#### <a name="option-2-using-public-read-container"></a>Option 2: Verwenden eines Containers mit öffentlichem Lesezugriff

> [!CAUTION]
> Diese Option ermöglicht anonymen schreibgeschützten Zugriff auf Ihren Container.

1. Gehen Sie wie im Abschnitt [Erteilen von anonymen Benutzerberechtigungen für Container und Blobs](https://docs.microsoft.com/azure/storage/storage-manage-access-to-resources#grant-anonymous-users-permissions-to-containers-and-blobs) beschrieben vor, um dem Paketcontainer **öffentlichen Lesezugriff nur für Blobs** zu gewähren.

2. Wählen Sie im Paketcontainer das im Container enthaltene Paketblob aus, um den Eigenschaftenbereich zu öffnen.

3. Kopieren Sie die **URL**. Verwenden Sie diesen Wert, wenn Sie im VaaS-Portal einen neuen Workflow vom Typ **Paketvalidierung** starten.

## <a name="apply-monthly-update"></a>Anwenden des monatlichen Updates

[!INCLUDE [azure-stack-vaas-workflow-section_update-azs](includes/azure-stack-vaas-workflow-section_update-azs.md)]

## <a name="create-a-package-validation-workflow"></a>Erstellen eines Paketvalidierungsworkflows

1. Melden Sie sich beim [VaaS-Portal](https://azurestackvalidation.com) an.

2. [!INCLUDE [azure-stack-vaas-workflow-step_select-solution](includes/azure-stack-vaas-workflow-step_select-solution.md)]

3. Klicken Sie auf der Kachel **Paketvalidierung** auf **Starten**.

    ![Kachel des Paketvalidierungsworkflows](media/tile_validation-package.png)

4. [!INCLUDE [azure-stack-vaas-workflow-step_naming](includes/azure-stack-vaas-workflow-step_naming.md)]

5. Geben Sie die Azure Storage-Blob-URL zu dem OEM-Paket an, das beim Bereitstellen für die Lösung installiert wurde. Eine entsprechende Anleitung finden Sie unter [Generieren der Paketblob-URL für VaaS](#generate-package-blob-url-for-vaas).

6. [!INCLUDE [azure-stack-vaas-workflow-step_upload-stampinfo](includes/azure-stack-vaas-workflow-step_upload-stampinfo.md)]

7. [!INCLUDE [azure-stack-vaas-workflow-step_test-params](includes/azure-stack-vaas-workflow-step_test-params.md)]

    > [!NOTE]
    > Umgebungsparameter können nach der Workflowerstellung nicht mehr geändert werden.

8. [!INCLUDE [azure-stack-vaas-workflow-step_tags](includes/azure-stack-vaas-workflow-step_tags.md)]

9. [!INCLUDE [azure-stack-vaas-workflow-step_submit](includes/azure-stack-vaas-workflow-step_submit.md)]
    Sie werden auf die Seite mit der Testzusammenfassung umgeleitet.

## <a name="run-package-validation-tests"></a>Ausführen von Paketvalidierungstests

Auf der Zusammenfassungsseite der **Paketvalidierungstests**wird eine Liste mit den erforderlichen Tests für die Überprüfung angezeigt. In diesem Workflow enthaltene Tests werden ungefähr 24 Stunden lang ausgeführt.

[!INCLUDE [azure-stack-vaas-workflow-validation-section_schedule](includes/azure-stack-vaas-workflow-validation-section_schedule.md)]

Wenn alle Tests erfolgreich abgeschlossen wurden, senden Sie den Namen Ihrer VaaS-Lösung und der Paketvalidierung an [vaashelp@microsoft.com](mailto:vaashelp@microsoft.com), um die Paketsignierung anzufordern.

## <a name="next-steps"></a>Nächste Schritte

- [Überwachen und Verwalten von Tests im VaaS-Portal](azure-stack-vaas-monitor-test.md)