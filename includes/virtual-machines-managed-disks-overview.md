---
title: Includedatei
description: Includedatei
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 01/11/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: dfd91caf67592b349bd16bab673a3e45397ad282
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2019
ms.locfileid: "58807633"
---
## <a name="benefits-of-managed-disks"></a>Vorteile von verwalteten Datenträgern

Hier werden einige Vorteile beschrieben, in deren Genuss Sie bei der Verwendung von verwalteten Datenträgern kommen.

### <a name="highly-durable-and-available"></a>Hoch verfügbar und stabil

Verwaltete Datenträger sind auf eine Verfügbarkeit von 99,999% ausgelegt. Hierzu stellen verwaltete Datenträger drei Replikate Ihrer Daten für Sie bereit, um eine hohe Dauerhaftigkeit zu garantieren. Wenn bei einem oder sogar bei zwei Ihrer Replikate Probleme auftreten, stellen Sie mit den übrigen Replikaten die Persistenz Ihrer Daten und eine hohe Fehlertoleranz sicher. Mit dieser Architektur konnte Azure für Infrastructure-as-a-Service-Datenträger (IaaS) durchgängig eine Dauerhaftigkeit auf Unternehmensniveau mit einer branchenweit führenden auf das Jahr umgerechneten Fehlerrate von 0% bereitstellen.

### <a name="simple-and-scalable-vm-deployment"></a>Einfache und skalierbare VM-Bereitstellung

Mit verwalteten Datenträgern können Sie für ein Abonnement pro Region bis zu 50.000VM-**Datenträger** eines Typs erstellen – Sie können also Tausende von **VMs** in einem einzigen Abonnement erstellen. Außerdem wird mit diesem Feature die Skalierbarkeit von [VM-Skalierungsgruppen](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) weiter erhöht, sodass Sie mit einem Marketplace-Image bis zu 1.000VMs in einer VM-Skalierungsgruppe erstellen können.

### <a name="integration-with-availability-sets"></a>Integration in Verfügbarkeitsgruppen

Verwaltete Datenträger werden in Verfügbarkeitsgruppen integriert, um sicherzustellen, dass die Datenträger von [VMs in einer Verfügbarkeitsgruppe](../articles/virtual-machines/windows/manage-availability.md#use-managed-disks-for-vms-in-an-availability-set) ausreichend voneinander isoliert sind, um einzelne Fehlerquellen zu vermeiden. Datenträger werden automatisch in unterschiedliche Speicherskalierungseinheiten („Stamps“) platziert. Wenn ein Stamp aufgrund eines Hardware- oder Softwarefehlers ausfällt, treten nur für die VM-Instanzen mit Datenträgern auf diesen Stamps Fehler auf. Angenommen, Sie führen eine Anwendung auf fünf VMs aus, und die VMs sind in einer Verfügbarkeitsgruppe enthalten. Die Datenträger für diese VMs werden nicht alle auf demselben Stamp gespeichert, damit die anderen Instanzen der Anwendung weiter ausgeführt werden, wenn ein Stamp ausfällt.

### <a name="azure-backup-support"></a>Azure Backup-Unterstützung

Zum Schutz vor regionalen Katastrophen kann mit [Azure Backup](../articles/backup/backup-introduction-to-azure-backup.md) ein Sicherungsauftrag mit zeitbasierten Sicherungen und Richtlinien zur Sicherungsaufbewahrung erstellt werden. So können Sie einfach nach Belieben VM-Wiederherstellungen ausführen. Azure Backup unterstützt derzeit Datenträgergrößen von bis zu vier Tebibyte (TiB). Weitere Informationen finden Sie unter [Verwendung von virtuellen Computern auf verwalteten Datenträgern](../articles/backup/backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup).

### <a name="granular-access-control"></a>Genau abgestimmte Zugriffssteuerung

Sie können die [Rollenbasierte Zugriffssteuerung in Azure (RBAC)](../articles/role-based-access-control/overview.md) verwenden, um die spezifischen Berechtigungen für einen verwalteten Datenträger einem oder mehreren Benutzern zuzuweisen. Verwaltete Datenträger bieten viele verschiedene Vorgänge, z.B. Lesen, Schreiben (Erstellen/Aktualisieren), Löschen und Abrufen eines [SAS-URI (Shared Access Signature)](../articles/storage/common/storage-dotnet-shared-access-signature-part-1.md) für den Datenträger. Sie haben die Möglichkeit, Personen nur Zugriff auf die Vorgänge zu gewähren, die sie jeweils benötigen, um ihre Aufgaben zu erledigen. Wenn Sie es für eine Person beispielsweise nicht zulassen möchten, dass sie einen verwalteten Datenträger auf ein Speicherkonto kopiert, können Sie festlegen, dass der Zugriff auf die Exportaktion für diesen verwalteten Datenträger nicht gewährt wird. Wenn Sie nicht möchten, dass eine Person einen SAS-URI zum Kopieren eines verwalteten Datenträgers verwendet, können Sie auch festlegen, dass diese Berechtigung für den verwalteten Datenträger nicht gewährt wird.

## <a name="disk-roles"></a>Datenträgerrollen

### <a name="data-disks"></a>Datenträger

Ein Datenträger für Daten ist ein verwalteter Datenträger, der zum Speichern von Anwendungsdaten oder anderen Daten, die Sie aufbewahren müssen, an einen virtuellen Computer angebunden ist. Datenträger werden als SCSI-Laufwerke registriert und mit einem von Ihnen ausgewählten Buchstaben gekennzeichnet. Jeder Datenträger weist eine maximale Kapazität von 32.767 GiB (Gibibyte) auf. Die Größe des virtuellen Computers bestimmt die Anzahl der Datenträger, die Sie anfügen können, und den Typ des Speichers, den Sie zum Hosten der Datenträger verwenden können.

### <a name="os-disks"></a>Betriebssystem-Datenträger

Jedem virtuellen Computer ist ein Betriebssystem-Datenträger zugeordnet. Dieser Betriebssystem-Datenträger verfügt über ein vorinstalliertes Betriebssystem, das beim Erstellen des virtuellen Computers ausgewählt wurde.

Dieser Datenträger weist eine maximale Kapazität von 2.048 GiB auf.

### <a name="temporary-disk"></a>Temporärer Datenträger

Jeder virtuelle Computer enthält einen temporären Datenträger, der kein verwalteter Datenträger ist. Der temporäre Datenträger bietet kurzfristigen Speicher für Anwendungen und Prozesse und ist ausschließlich dafür ausgelegt, Daten wie z.B. Seiten-oder Auslagerungsdateien zu speichern. Daten auf dem temporären Datenträger können während eines [Wartungsereignisses](../articles/virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) verloren gehen oder wenn Sie eine [VM erneut bereitstellen](../articles/virtual-machines/troubleshooting/redeploy-to-new-node-windows.md?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json). Während eines erfolgreichen standardmäßigen Neustarts der VM bleiben die Daten auf dem virtuellen Datenträger erhalten.

## <a name="managed-disk-snapshots"></a>Momentaufnahmen eines verwalteten Datenträgers

Bei einer Momentaufnahme eines verwalteten Datenträgers handelt es sich um eine schreibgeschützte vollständige Kopie eines verwalteten Datenträgers, die standardmäßig als verwalteter Standarddatenträger gespeichert wird. Mit Momentaufnahmen können Sie Ihre verwalteten Datenträger jederzeit sichern. Diese Momentaufnahmen existieren unabhängig vom Quelldatenträger und können zum Erstellen neuer verwalteter Datenträger verwendet werden. Sie werden auf Basis der verwendeten Größe in Rechnung gestellt. Wenn Sie beispielsweise eine Momentaufnahme eines verwalteten Datenträgers mit einer bereitgestellten Kapazität von 64GiB und einer tatsächlichen Datengröße von 10GiB erstellen, wird die Momentaufnahme nur für die in Anspruch genommene Datengröße von 10GiB in Rechnung gestellt.  

Weitere Informationen dazu, wie Sie Momentaufnahmen mit verwalteten Datenträgern erstellen, finden Sie in den folgenden Ressourcen:

* [Erstellen einer Kopie einer als verwalteter Datenträger gespeicherten virtuellen Festplatte mithilfe von Momentaufnahmen unter Windows](../articles/virtual-machines/windows/snapshot-copy-managed-disk.md)
* [Erstellen einer Kopie einer als verwalteter Datenträger gespeicherten virtuellen Festplatte mithilfe von Momentaufnahmen unter Linux](../articles/virtual-machines/linux/snapshot-copy-managed-disk.md)

### <a name="images"></a>Bilder

Für verwaltete Datenträger wird auch die Erstellung eines verwalteten benutzerdefinierten Image unterstützt. Sie können ein Image aus Ihrer benutzerdefinierten VHD in einem Speicherkonto oder direkt von einer generalisierten VM (mit Systemvorbereitung) erstellen. Dieser Prozess erfasst ein einzelnes Image. Dieses Image enthält alle verwalteten Datenträger, die einer VM zugeordnet sind, einschließlich der Datenträger für das Betriebssystem und für die Daten. Dieses verwaltete benutzerdefinierte Image ermöglicht die Erstellung von Hunderten von VMs mit Ihrem benutzerdefinierten Image, ohne dass Sie Speicherkonten kopieren oder verwalten müssen.

Informationen zum Erstellen von Images finden Sie in den folgenden Artikeln:

* [How to capture a managed image of a generalized VM in Azure](../articles/virtual-machines/windows/capture-image-resource.md) (Erstellen eines verwalteten Image eines generalisierten virtuellen Computers in Azure)
* [Generalisieren und Erfassen eines virtuellen Linux-Computers mithilfe der Azure CLI](../articles/virtual-machines/linux/capture-image.md)

#### <a name="images-versus-snapshots"></a>Vergleich von Images und Momentaufnahmen

Es ist wichtig, den Unterschied zwischen Images und Momentaufnahmen zu verstehen. Bei verwalteten Datenträgern können Sie ein Image einer generalisierten VM erstellen, deren Zuordnung aufgehoben wurde. Dieses Image umfasst alle Datenträger, die der VM angefügt sind. Sie können dieses Image verwenden, um eine VM mit allen Datenträgern zu erstellen.

Eine Momentaufnahme ist eine Kopie eines Datenträgers zum Zeitpunkt der Erstellung der Momentaufnahme. Sie gilt nur für einen einzigen Datenträger. Wenn Sie eine VM mit einem einzigen Datenträger (Betriebssystem-Datenträger) verwenden, können Sie eine Momentaufnahme oder ein Image davon erstellen und dann entweder aus der Momentaufnahme oder dem Image eine VM erstellen.

Eine Momentaufnahme bezieht sich nur auf den Datenträger, von dem sie erstellt wurde. Darum ist ihre Verwendung problematisch in Szenarien, die die Koordination mehrerer Datenträger erfordern, z.B. Striping. Momentaufnahmen müssten sich miteinander abstimmen, und dies wird derzeit nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie in unserem Artikel über Datenträgertypen mehr über die einzelnen Datenträgertypen, die Azure bietet, und welcher Typ sich gut für Ihre Anforderungen eignet.
