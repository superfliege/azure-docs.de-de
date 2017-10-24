---
title: "Umstieg vom klassischen Portal auf das Azure-Portal – häufig gestellte Fragen | Microsoft-Dokumentation"
description: "Enthält Antworten auf häufig gestellte Fragen zum Verschieben von StorSimple-Geräten vom klassischen Portal in das Azure-Portal."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/28/2017
ms.author: alkohli
ms.openlocfilehash: 06e8d99aa2ad4eb11e594a729c6dab39d5cd1eb6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="move-storsimple-device-manager-service-from-classic-to-azure-portal-frequently-asked-questions-faq"></a>Verschieben des StorSimple Device Manager-Diensts vom klassischen Portal in das Azure-Portal: häufig gestellte Fragen (FAQ)

## <a name="overview"></a>Übersicht

Hier finden Sie Fragen, die sich möglicherweise ergeben, wenn Sie Ihren StorSimple Device Manager-Dienst, der im klassischen Azure-Portal ausgeführt wird, in das Azure-Portal verschieben, mit den zugehörigen Antworten.

Die Fragen und Antworten sind in folgende Kategorien unterteilt:

* Verschieben des StorSimple Device Manager-Diensts
* Verschieben von Speicherkonten
* Verwenden von Azure Resource Manager-basierten Cmdlets
* Verwenden des StorSimple Data Manager-Diensts
* Verschiedenes

## <a name="moving-storsimple-device-manager-service"></a>Verschieben des StorSimple Device Manager-Diensts

### <a name="once-i-have-moved-to-azure-portal-can-i-still-create-a-storsimple-manager-service-in-the-classic-portal"></a>Kann ich nach dem Umstieg auf das Azure-Portal weiterhin einen StorSimple Manager-Dienst im klassischen Portal erstellen?

Nein. Nachdem Sie den StorSimple Manager-Dienst zum Azure-Portal migriert haben, können Sie im klassischen Portal keinen neuen Dienst erstellen. Zudem können Sie Ihr Gerät nicht über das klassische Portal verwalten. Weitere Informationen finden Sie unter [Verschieben eines Diensts in das Azure-Portal](storsimple-8000-manage-service.md#move-a-service-to-azure-portal).

### <a name="i-have-multiple-storsimple-managers-running-in-the-classic-portal-can-i-choose-which-ones-to-move-to-the-azure-portal"></a>Ich führe mehrere StorSimple Manager im klassischen Portal aus. Kann ich auswählen, welche davon in das Azure-Portal verschoben werden?

Nein. Sie können nicht auswählen, welche StorSimple Manager in das Azure-Portal verschoben werden. Alle StorSimple Manager in Ihrem Abonnement werden verschoben.


### <a name="i-initiated-the-migration-of-my-service-to-the-new-azure-portal-should-i-delete-the-storsimple-manager-from-the-classic-portal"></a>Ich habe mit der Migration meines Diensts zum neuen Azure-Portal begonnen. Muss ich den StorSimple Manager-Dienst im klassischen Portal löschen? 

Nein. Versuchen Sie nicht, Dienste zu löschen, die Sie vom klassischen Portal verschoben haben. Warten Sie, bis die Migration abgeschlossen ist. Nachdem alle StorSimple Manager in das neue Portal verschoben wurden, müssen keine Dienste im klassischen Portal gelöscht werden. Zu einem bestimmten Zeitpunkt ist das klassische Portal veraltet.

### <a name="can-i-rename-my-storsimple-device-manager-service-in-the-azure-portal"></a>Kann ich meinen StorSimple Device Manager-Dienst im Azure-Portal umbenennen?

Nein. Der Dienstname kann nicht mehr geändert werden, nachdem der Dienst im Azure-Portal erstellt wurde. Dies gilt auch für andere Entitäten wie z.B. Geräte, Volumes, Volumecontainer und Sicherungsrichtlinien.

### <a name="can-i-create-the-80108020-storsimple-cloud-appliances-with-azure-resource-manager-deployment-model"></a>Kann ich die 8010/8020 StorSimple Cloud Appliances mit dem Azure Resource Manager-Bereitstellungsmodell erstellen?

Ja. Sie können neue 8010/8020 StorSimple Cloud Appliances im Azure Resource Manager-Bereitstellungsmodell erstellen. Die zugrunde liegenden virtuellen Computer für diese Cloud Appliances sind auch im Resource Manager-Bereitstellungsmodell enthalten.

### <a name="when-we-migrate-subscriptions-to-the-azure-portal-will-the-underlying-vms-for-the-storsimple-cloud-appliances-also-migrate-to-azure-resource-management-deployment-model"></a>Werden die zugrunde liegenden virtuellen Computer für die StorSimple Cloud Appliances beim Migrieren von Abonnements zum Azure-Portal auch zum Azure Resource Manager-Bereitstellungsmodell migriert?

Das Verschieben des StorSimple-Diensts erfolgt unabhängig von der Verwaltung der virtuellen Computer für die StorSimple Cloud Appliances. Sie können die virtuellen Computer für StorSimple Cloud Appliances im klassischen Portal und im Azure-Portal direkt vollständig verwalten.

### <a name="can-i-manage-existing-classic-80108020-storsimple-cloud-appliance-from-the-azure-portal"></a>Kann ich vorhandene klassische 8010/8020 StorSimple Cloud Appliances über das Azure-Portal verwalten?

Ja. Die mit vorhandenen 8010/8020 Cloud Appliances verbundenen virtuellen Computer können über das Azure-Portal verwaltet werden.

Wenn Sie StorSimple Cloud Appliances für das Modell 8010/8020 mit ausgeführtem Update 3.0 oder höher erstellt haben, hat das Verschieben Ihres Diensts in das neue Azure-Portal darauf keine Auswirkungen. Sie sollten die Cloud Appliances ohne Probleme vollständig verwalten können. 

Bei Cloud Appliances mit ausgeführten Versionen vor Update 3.0 im klassischen Portal steht nur eingeschränkte Funktionalität zur Verfügung. Weitere Informationen finden Sie in der [Aufstellung der nicht unterstützten Vorgänge für Geräte, auf denen Versionen vor Update 3 ausgeführt werden](storsimple-8000-manage-service.md#move-a-service-to-azure-portal).

Sie können für ein Cloudgerät keine Updates durchführen. Verwenden Sie die neueste Version der Software, um eine neue Cloud Appliance zu erstellen und dann ein Failover der vorhandenen Volumecontainer für die neu erstellte Cloud Appliance auszuführen. Weitere Informationen finden Sie unter [Durchführen eines Failovers zum Cloudgerät](storsimple-8000-cloud-appliance-u2.md#fail-over-to-the-cloud-appliance).


### <a name="my-storsimple-8000-series-device-is-running-update-20-i-migrated-my-service-to-new-azure-portal-my-device-connected-successfully-but-it-seems-that-i-am-not-able-to-fully-manage-my-device-how-do-i-resolve-this-behavior"></a>Auf meinem Gerät der StorSimple 8000-Serie wird Update 2.0 ausgeführt. Ich habe meinen Dienst zum neuen Azure-Portal migriert. Die Verbindung mit meinem Gerät wurde erfolgreich hergestellt, allerdings kann ich mein Gerät nicht vollständig verwalten. Wie kann ich dieses Problem lösen?

Im neuen Azure-Portal werden nur StorSimple-Geräte mit ausgeführtem Update 3.0 und höher unterstützt. Wenn auf Ihrem Gerät Update 2.0 ausgeführt wird, steht für das Gerät nur eingeschränkte Funktionalität zur Verfügung. Weitere Informationen finden Sie in der [Aufstellung der nicht unterstützten Vorgänge für Geräte, auf denen Versionen vor Update 3 ausgeführt werden](storsimple-8000-manage-service.md#move-a-service-to-azure-portal).

Installieren Sie das neueste Update auf Ihrem Gerät, um es vollständig verwalten zu können. Weitere Informationen finden Sie unter [Installieren von Update 5](storsimple-8000-install-update-5.md).

### <a name="i-just-moved-my-storsimple-manager-service-to-the-azure-portal-i-am-seeing-some-alerts-related-to-my-device-is-this-behavior-related-to-the-move"></a>Ich habe gerade meinen StorSimple Manager-Dienst in das Azure-Portal verschoben. Nun werden einige Warnungen zu meinem Gerät angezeigt. Ist dies auf das Verschieben zurückzuführen?

Nein. Das Verschieben selbst sollte nicht zu Fehlern oder Warnungen führen. Befolgen Sie die Empfehlungen in den Warnungen, um diese zu beheben.

### <a name="i-am-using-a-storsimple-50007000-series-device-are-these-also-supported-in-the-azure-portal"></a>Ich verwende ein Gerät der StorSimple 5000/7000-Serie. Werden diese Geräte auch im Azure-Portal unterstützt?

Nein. Die Geräte der StorSimple 5000/7000-Serie werden im Azure-Portal nicht unterstützt.

### <a name="i-am-planning-to-migrate-data-from-storsimple-50007000-series-device-to-a-storsimple-8000-series-device-how-does-moving-a-service-to-azure-portal-impact-my-data-migration"></a>Ich möchte Daten von einem Gerät der StorSimple 5000/7000-Serie zu einem Gerät der StorSimple 8000-Serie migrieren. Wie wirkt sich das Verschieben eines Diensts in das Azure-Portal auf die Datenmigration aus? 

Sie können Daten von Ihrem Gerät der StorSimple 5000/7000-Serie zu einem Gerät der StorSimple 8000-Serie migrieren, das im Azure-Portal ausgeführt wird. Damit Sie die Datenmigration von einem Gerät der 5000/7000-Serie zu einem Gerät der 8000-Serie durchführen können, müssen Sie [eine Supportanfrage beim Microsoft-Support erstellen](storsimple-8000-contact-microsoft-support.md).


## <a name="moving-subscriptions-storage-accounts-resource-groups"></a>Verschieben von Abonnements, Speicherkonten und Ressourcengruppen

### <a name="can-i-move-storsimple-device-manager-from-one-subscription-to-another-subscription-in-the-azure-portal"></a>Kann ich StorSimple Device Manager zwischen Abonnements im Azure-Portal verschieben?

Nein. Das Verschieben eines StorSimple Device Manager-Diensts zwischen verschiedenen Abonnements ist nicht möglich. Sie können dies in einem manuellen Vorgang mit folgenden Schritten durchführen:

* Migrieren Sie die Daten aus dem StorSimple-Gerät.
* Führen Sie eine Zurücksetzung des Geräts auf die Werkseinstellungen durch. Damit werden alle lokalen Daten auf dem Gerät gelöscht.
* Registrieren Sie das Gerät mit dem neuen Abonnement bei einem StorSimple-Geräte-Manager-Dienst.
* Migrieren Sie die Daten zurück zum Gerät.

### <a name="do-i-have-to-migrate-the-storage-account-to-azure-resource-manager-deployment-model"></a>Muss ich das Speicherkonto zum Azure Resource Manager-Bereitstellungsmodell migrieren?

Nein. Ihre klassischen Speicherkonten können vollständig über das Azure-Portal verwaltet werden. Wenn Sie Ihren StorSimple-Dienst in das Azure-Portal verschieben, wird Ihr Speicherkonto weiterhin wie zuvor ausgeführt.

### <a name="what-happens-to-the-storage-account-after-the-service-is-migrated-to-the-azure-portal"></a>Was geschieht mit dem Speicherkonto, nachdem der Dienst zum Azure-Portal migriert wurde?

Das Verschieben des Diensts hängt nicht mit der Verwaltung des Speicherkontos zusammen. Sowohl klassische als auch Azure Resource Manager-Speicherkonten können im Azure-Portal vollständig verwaltet werden. Nachdem Sie Ihren Dienst in das Azure-Portal verschoben haben, sollte Ihr Gerät weiterhin mit dem vorhandenen Speicherkonto ausgeführt werden, und auch Ihre Daten sollten davon nicht beeinträchtigt werden.

### <a name="can-i-migrate-storsimple-device-manager-from-one-resource-group-to-another"></a>Kann ich StorSimple Device Manager von einer zu einer anderen Ressourcengruppe migrieren?

Nein. Sie können einen mit einer Ressourcengruppe erstellten StorSimple Device Manager nicht in eine andere Ressourcengruppe verschieben.

## <a name="using-azure-resource-manager-based-cmdlets"></a>Verwenden von Azure Resource Manager-basierten Cmdlets

### <a name="i-moved-to-the-new-azure-portal-now-my-scripts-based-on-azure-classic-deployment-model-powershell-cmdlets-are-not-working-what-do-i-need-to-do"></a>Ich bin auf das neue Azure-Portal umgestiegen. Nun werden meine Skripts, die auf PowerShell-Cmdlets des klassischen Azure-Bereitstellungsmodells basieren, nicht ausgeführt. Was muss ich tun?

Die vorhandenen PowerShell-Cmdlets des klassischen Azure-Bereitstellungsmodells werden im Azure-Portal nicht unterstützt. Aktualisieren Sie die Skripts so, dass Ihrer Geräte mit Azure Resource Manager verwaltet werden. Weitere Informationen zum Aktualisieren von Skripts finden Sie in den [Beispielen für das neue Azure-Portal](https://github.com/anoobbacker/storsimpledevicemgmttools).

### <a name="i-just-moved-to-the-azure-portal-and-needed-to-roll-over-the-service-data-encryption-key-where-is-this-option-in-the-azure-portal"></a>Ich bin gerade auf das Azure-Portal umgestiegen und muss ein Rollover des Dienstdatenverschlüsselungs-Schlüssels ausführen. Wo befindet sich diese Option im Azure-Portal?

Die Option zum Ausführen des Rollovers des Dienstdatenverschlüsselungs-Schlüssels befindet sich nicht im Azure-Portal. Weitere Informationen zu diesem Rollover im Azure-Portal finden Sie unter [Ändern des Dienstdatenverschlüsselungs-Schlüssels](storsimple-8000-manage-service.md#change-the-service-data-encryption-key).

### <a name="i-am-using-windows-powershell-for-storsimple-cmdlets-on-the-storsimple-device-to-perform-operations-such-extract-a-support-package-are-these-cmdlets-affected-when-i-move-to-the-new-azure-portal"></a>Ich verwende Windows PowerShell für StorSimple-Cmdlets auf dem StorSimple-Gerät zum Durchführen von Vorgängen, z.B. zum Extrahieren eines Unterstützungspakets. Sind diese Cmdlets betroffen, wenn ich auf das neue Azure-Portal umsteige?

Nein. Das Verschieben Ihres Diensts in das neue Azure-Portal sollte keine Auswirkungen auf die Windows PowerShell für StorSimple-Cmdlets haben, die mit dem lokalen StorSimple-Gerät verknüpft sind (das Gerät selbst ist von der Verschiebung nicht betroffen). Sie können diese Cmdlets weiterhin verwenden und ein Unterstützungspaket auch im Azure-Portal ohne Probleme erstellen. Von der Verschiebung sind nur die PowerShell-Cmdlets des klassischen Azure-Bereitstellungsmodells betroffen.

## <a name="moving-storsimple-data-manager-service"></a>Verschieben des StorSimple Data Manager-Diensts

### <a name="i-am-using-storsimple-data-manager-service-how-should-i-proceed-with-this-move"></a>Ich verwende den StorSimple Data Manager-Dienst. Wie muss ich beim Verschieben vorgehen?

Wenn Sie den StorSimple Data Manager-Dienst verwenden, müssen Sie zunächst Ihre StorSimple Device Manager in das Azure-Portal verschieben. Erstellen Sie nach Abschluss der Verschiebung neue StorSimple Data Manager im Azure-Portal. StorSimple Data Manager, die vor der Verschiebung erstellt werden, werden nicht ausgeführt.

Weitere Informationen zur Migration des StorSimple Device Manager-Diensts finden Sie unter [Verschieben eines Diensts in das Azure-Portal](storsimple-8000-manage-service.md#move-a-service-to-azure-portal). Weitere Informationen zum Erstellen von StorSimple Data Manager finden Sie unter [Erstellen eines StorSimple Data Manager-Diensts](storsimple-data-manager-ui.md).

## <a name="miscellaneous"></a>Verschiedenes

### <a name="i-am-running-storsimple-snapshot-manager-for-my-8000-series-device-is-there-any-impact-on-storsimple-snapshot-manager-when-i-move-to-azure-portal"></a>Ich führe StorSimple Snapshot Manager für mein Gerät der 8000-Serie aus. Ergeben sich beim Verschieben in das Azure-Portal Auswirkungen auf StorSimple Snapshot Manager?

Nein. Das Verschieben Ihres Diensts in das Azure-Portal wirkt sich nicht auf StorSimple Snapshot Manager aus. Ihr Gerät und StorSimple Snapshot Manager werden weiterhin wie zuvor ausgeführt.

### <a name="can-i-rename-my-storsimple-device-volume-containers-or-volumes"></a>Kann ich das StorSimple-Gerät, Volumecontainer oder Volumes umbenennen?

Nein. Sie können Geräte, Volumes, Volumecontainer oder Sicherungsrichtlinien im Azure-Portal nicht umbenennen.

## <a name="next-steps"></a>Nächste Schritte

Ausführliche Anweisungen zum [Verschieben des StorSimple Device Manager-Diensts in das Azure-Portal](storsimple-8000-manage-service.md#move-a-service-to-azure-portal).



