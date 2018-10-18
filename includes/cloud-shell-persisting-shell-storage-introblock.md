---
author: jluk
ms.service: cloud-shell
ms.topic: persist-storage
ms.date: 9/7/2018
ms.author: juluk
ms.openlocfilehash: 6055b70c7df2704a334b7f14c9365863ddafbd5a
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44164541"
---
# <a name="persist-files-in-azure-cloud-shell"></a>Beibehalten von Dateien in Azure Cloud Shell
Cloud Shell nutzt Azure-Dateispeicher, um Dateien sitzungsübergreifend beizubehalten. Beim ersten Start fordert Cloud Shell Sie auf, eine neue oder vorhandene Dateifreigabe zu verknüpfen, um Dateien sitzungsübergreifend beizubehalten.

> [!NOTE]
> Für Bash und PowerShell wird dieselbe Dateifreigabe verwendet. In Cloud Shell kann der automatischen Bereitstellung nur eine Dateifreigabe zugeordnet werden.

## <a name="create-new-storage"></a>Erstellen von neuem Speicher

Wenn Sie die grundlegenden Einstellungen verwenden und nur ein Abonnement auswählen, werden von Cloud Shell in der für Sie unterstützten nächstgelegenen Region drei Ressourcen erstellt:
* Ressourcengruppe: `cloud-shell-storage-<region>`
* Speicherkonto: `cs<uniqueGuid>`
* Dateifreigabe: `cs-<user>-<domain>-com-<uniqueGuid>`

![Einstellung „Abonnement“](../articles/cloud-shell/media/persisting-shell-storage/basic-storage.png)

Die Dateifreigabe wird in Ihrem Verzeichnis `$Home` als `clouddrive` bereitgestellt. Dies ist eine einmalige Aktion, und die Dateifreigabe wird in den nachfolgenden Sitzungen automatisch bereitgestellt. 

> [!NOTE]
> Aus Sicherheitsgründen sollte jeder Benutzer ein eigenes Speicherkonto bereitstellen.  Für die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) benötigen Benutzer zumindest die Berechtigungen eines Mitwirkenden auf Speicherkontoebene.

Die Dateifreigabe enthält auch ein 5-GB-Image, das für Sie erstellt wird und mit dem Daten automatisch in Ihrem Verzeichnis `$Home` beibehalten werden. Dies gilt für Bash sowie für PowerShell.

## <a name="use-existing-resources"></a>Verwenden vorhandener Ressourcen

Sie können vorhandene Ressourcen zuordnen, indem Sie die erweiterte Option verwenden. Wählen Sie bei der Anzeige mit der Aufforderung zum Einrichten des Speichers die Option **Erweiterte Einstellungen anzeigen**, um weitere Optionen anzuzeigen. Die ausgefüllten Speicheroptionen werden nach Konten mit lokal redundantem Speicher (LRS), georedundantem Speicher (GRS) und zonenredundantem Speicher (ZRS) gefiltert. [Hier](https://docs.microsoft.com/azure/storage/common/storage-redundancy#choosing-a-replication-option) erhalten Sie weitere Informationen zu Replikationsoptionen für Azure Storage-Konten.

![Einstellung „Ressourcengruppe“](../articles/cloud-shell/media/persisting-shell-storage/advanced-storage.png)

Wenn Sie eine Cloud Shell-Region auswählen, müssen Sie auch auswählen, dass ein unterstützendes Speicherkonto in dieser Region eingebunden wird.

### <a name="supported-storage-regions"></a>Unterstützte Speicherregionen
Zugeordnete Azure-Speicherkonten müssen sich in derselben Region wie der Cloud Shell-Computer befinden, auf dem Sie diese bereitstellen. Um die aktuelle Region zu suchen, können Sie `env` in Bash ausführen und die Variable `ACC_LOCATION` suchen. Dateifreigaben erhalten ein für Sie erstelltes 5-GB-Image zum Beibehalten Ihres Verzeichnisses `$Home`.

Cloud Shell-Computer sind in folgenden Regionen vorhanden:
|Bereich|Region|
|---|---|
|Amerika|USA, Osten; USA, Süden-Mitte; USA, Westen|
|Europa|„Europa, Norden“, „Europa, Westen“|
|Asien-Pazifik|Indien, Mitte; Asien, Südosten|

## <a name="restrict-resource-creation-with-an-azure-resource-policy"></a>Beschränken der Ressourcenerstellung mit einer Azure-Ressourcenrichtlinie
Speicherkonten, die Sie in Cloud Shell erstellen, erhalten das Tag `ms-resource-usage:azure-cloud-shell`. Wenn Sie nicht möchten, dass Benutzer Speicherkonten in Cloud Shell erstellen, können Sie eine [Azure-Ressourcenrichtlinie für Tags](../articles/azure-policy/json-samples.md) erstellen, die durch das jeweilige Tag ausgelöst werden.
