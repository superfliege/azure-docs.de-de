---
title: storage-files-create-storage-account-portal
description: Es wird beschrieben, wie Sie ein Speicherkonto für Azure Files erstellen.
services: storage
author: wmgries
ms.service: storage
ms.topic: include
ms.date: 03/28/2018
ms.author: wgries
ms.custom: include file
ms.openlocfilehash: 82e141eb4941dbd58be95e19dae186acd18caf94
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2018
---
Ein Speicherkonto ist ein gemeinsam genutzter Pool mit Speicherplatz, den Sie zum Bereitstellen einer Azure-Dateifreigabe oder von anderen Speicherressourcen wie Blobs oder Warteschlangen verwenden können. Ein Speicherkonto kann eine unbegrenzte Anzahl von Freigaben enthalten, und eine Freigabe kann eine unbegrenzte Anzahl von Dateien speichern, bis die Kapazitätsgrenze des Speicherkontos erreicht ist.

Erstellen Sie ein Speicherkonto wie folgt:

1. Klicken Sie im Menü auf der linken Seite auf das Pluszeichen (**+**), um eine Ressource zu erstellen.
2. Geben Sie im Suchfeld (1) den Text **Speicherkonto** ein, und wählen Sie **Speicherkonto – Blob, Datei, Tabelle, Warteschlange** (2) aus. Klicken Sie anschließend auf **Erstellen**.
    ![Screenshot: Speicherkontoeintrag im Dialogfeld für die Ressourcensuche](../articles/storage/files/media/storage-how-to-use-files-portal/create-storage-account-1.png)

3. Geben Sie unter **Name** den Namen *mystorageacct* ein, gefolgt von einigen zufällig ausgewählten Zahlen, bis durch das grüne Häkchen angezeigt wird, dass der Name eindeutig ist. Der Name des Speicherkontos darf nur Kleinbuchstaben enthalten und muss global eindeutig sein. Notieren Sie sich den Namen des Speicherkontos, da Sie ihn später noch benötigen. 
4. Übernehmen Sie unter **Bereitstellungsmodell** den Standardwert **Resource Manager**. Weitere Informationen zu den Unterschieden zwischen der Azure Resource Manager-Bereitstellung und der klassischen Bereitstellung finden Sie unter [Grundlegendes zu Bereitstellungsmodellen und zum Status von Ressourcen](../articles/azure-resource-manager/resource-manager-deployment-model.md).
5. Wählen Sie unter **Kontoart** die Option **StorageV2**. Weitere Informationen zu den unterschiedlichen Arten von Speicherkonten finden Sie unter [Optionen für Azure Storage-Konten](../articles/storage/common/storage-account-options.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).
6. Übernehmen Sie unter **Leistung** den Standardwert **Standardspeicher**. Für Azure Files wird derzeit nur Standardspeicher unterstützt. Auch wenn Sie Storage Premium wählen, wird Ihre Dateifreigabe im Standardspeicher gespeichert.
7. Wählen Sie unter **Replikation** die Option *Lokal redundanter Speicher (LRS)*. 
8. Wir empfehlen Ihnen, unter **Sichere Übertragung erforderlich** immer die Option **Aktiviert** zu wählen. Weitere Informationen zu dieser Option finden Sie unter [Vorschreiben einer sicheren Übertragung in Azure Storage](../articles/storage/common/storage-require-secure-transfer.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).
9. Wählen Sie unter **Abonnement** das Abonnement aus, unter dem das Speicherkonto erstellt werden soll. Falls Sie nur über ein Abonnement verfügen, sollte es das Standardabonnement sein.
10. Wählen Sie unter **Ressourcengruppe** die Option **Neu erstellen**, und geben Sie *myResourceGroup* als Namen ein.
11. Wählen Sie unter **Standort** die Option **USA, Osten**.
12. Übernehmen Sie unter **Virtuelle Netzwerke** die Standardoption *Deaktiviert*. 
13. Wählen Sie **An Dashboard anheften**, damit Sie das Speicherkonto leichter finden.
14. Wenn Sie fertig sind, können Sie auf **Erstellen** klicken, um die Bereitstellung zu starten.