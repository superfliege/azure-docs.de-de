---
title: Konfigurieren der Replikation für ADE-fähige (Azure Disk Encryption) virtuelle Computer in Azure Site Recovery | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie die Replikation virtueller ADE-Computer zwischen Azure-Regionen mithilfe von Site Recovery konfigurieren.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: sutalasi
ms.openlocfilehash: 5d992d13a67c7b01f82b615e7131a20b84dec9e8
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52851015"
---
# <a name="replicate-azure-disk-encryption-ade-enabled-virtual-machines-to-another-azure-region"></a>Replizieren von ADE-fähigen (Azure Disk Encryption) virtuellen Computern in einer anderen Azure-Region

In diesem Artikel erfahren Sie, wie Sie die Replikation ADE-fähiger (Azure Disk Encryption) virtueller Computer zwischen Azure-Regionen aktivieren.

>[!NOTE]
>Von Azure Site Recovery werden derzeit nur Azure-VMs unterstützt, auf denen das Windows-Betriebssystem ausgeführt wird und die [für die Verschlüsselung mit der Azure AD-App aktiviert sind](https://aka.ms/ade-aad-app).
>

## <a name="required-user-permissions"></a>Erforderliche Benutzerberechtigungen

Zum Aktivieren der Replikation von ADE-VMs über das Portal muss der Benutzer über die unten aufgeführten Berechtigungen verfügen.
- Schlüsseltresorberechtigungen
    - list
    - Erstellen
    - Get

-   Berechtigungen für Schlüsseltresorgeheimnisse
    - Auflisten
    - Erstellen
    - Get

- Berechtigungen für Schlüsseltresorschlüssel (nur erforderlich, wenn die virtuellen Computer den Schlüssel für die Schlüsselverschlüsselung verwenden, um Datenträger-Verschlüsselungsschlüssel zu verschlüsseln)
    - Auflisten
    - Get
    - Erstellen
    - Verschlüsseln
    - Entschlüsseln

Sie können die Berechtigungen verwalten, indem Sie im Portal zur Schlüsseltresorressource navigieren und die erforderlichen Berechtigungen für den Benutzer hinzufügen.

![keyvaultpermissions](./media/azure-to-azure-how-to-enable-replication-ade-vms/keyvaultpermissions.png)

Wenn der Benutzer, der die Notfallwiederherstellung (DR) aktiviert, nicht über die erforderlichen Berechtigungen zum Kopieren des Schlüssels verfügt, kann das folgende Skript dem Sicherheitsadministrator mit entsprechenden Berechtigungen übergeben werden, um die Verschlüsselungsgeheimnisse und Schlüssel in die Zielregion zu kopieren.

>[!NOTE]
>Zum Aktivieren der Replikation der ADE-VM aus dem Portal benötigen Sie mindestens die Berechtigungen „Liste“ für die Schlüsseltresore, Schlüssel und Geheimnisse.
>

## <a name="copy-ade-keys-to-dr-region-using-powershell-script"></a>Kopieren der ADE-Schlüssel in die DR-Region mithilfe eines PowerShell-Skripts

1. Öffnen Sie den Code des Rohskripts „CopyKeys“ in einem Browserfenster, indem Sie auf [diesen Link](https://aka.ms/ade-asr-copy-keys-code) klicken.
2. Kopieren Sie das Skript in eine Datei, und benennen Sie diese mit „Copy-keys.ps1“.
2. Öffnen Sie die Windows PowerShell-Anwendung, und wechseln Sie zum Speicherort des Ordners, in dem sich die Datei befindet.
3. Starten Sie „Copy-keys.ps1“.
4. Geben Sie die Anmeldeinformationen für die Azure-Anmeldung an.
5. Wählen Sie das **Azure-Abonnement** Ihrer virtuellen Computer aus.
6. Warten Sie, bis die Ressourcengruppen geladen wurden, und wählen Sie dann die **Ressourcengruppe** Ihrer virtuellen Computer aus.
7. Wählen Sie die VM in der angezeigten Liste der VMs aus. Nur virtuelle Computer, für die Azure Disk Encryption aktiviert wurde, werden in der Liste angezeigt.
8. Wählen Sie den **Zielspeicherort** aus.
9. **Schlüsseltresore für Datenträgerverschlüsselung**: Standardmäßig erstellt Azure Site Recovery einen neuen Schlüsseltresor in der Zielregion mit dem Namenssuffix „asr“, der auf den Verschlüsselungsschlüsseln für die Datenträger des virtuellen Quellcomputers basiert. Falls bereits ein von Azure Site Recovery erstellter Schlüsseltresor vorhanden ist, wird er wiederverwendet. Sie können bei Bedarf einen anderen Schlüsseltresor in der Liste auswählen.
10. **Schlüsseltresore für Schlüsselverschlüsselung**: Standardmäßig erstellt Azure Site Recovery einen neuen Schlüsseltresor in der Zielregion mit dem Namenssuffix „asr“, der auf den Verschlüsselungsschlüsseln für die Schlüssel des virtuellen Quellcomputers basiert. Falls bereits ein von Azure Site Recovery erstellter Schlüsseltresor vorhanden ist, wird er wiederverwendet. Sie können bei Bedarf einen anderen Schlüsseltresor in der Liste auswählen.

## <a name="enable-replication"></a>Aktivieren der Replikation

In diesem Verfahren wird davon ausgegangen, dass als primäre Azure-Region „Asien, Osten“ und als sekundäre Region „Asien, Südosten“ verwendet wird.

1. Klicken Sie im Tresor auf **+Replizieren**.
2. Beachten Sie die folgenden Felder:
    - **Quelle**: Der Ursprung der virtuellen Computer, in diesem Fall **Azure**.
    - **Quellstandort**: Die Azure-Region, in der Sie Ihre virtuellen Computer schützen möchten. In diesem Beispiel ist der Quellspeicherort „Asien, Osten“.
    - **Bereitstellungsmodell**: Das Azure-Bereitstellungsmodell der Quellcomputer.
    - **Quellabonnement**: Das Abonnement, dem Ihre virtuellen Quellcomputer angehören. Dies kann ein beliebiges Abonnement im gleichen Azure Active Directory-Mandanten sein, in dem sich auch Ihr Recovery Services-Tresor befindet.
    - **Ressourcengruppe**: Die Ressourcengruppe, der Ihre virtuellen Quellcomputer angehören. Alle virtuellen Computer der ausgewählten Ressourcengruppe werden im nächsten Schritt für den Schutz aufgeführt.

3. Wählen Sie unter **Virtuelle Computer > Virtuelle Computer auswählen** die virtuellen Computer aus, die Sie replizieren möchten. Sie können nur Computer auswählen, für die die Replikation aktiviert werden kann. Klicken Sie dann auf **OK**.

4. Unter **Einstellungen** können Sie optional Einstellungen für den Zielstandort konfigurieren:

    - **Zielstandort**: Der Standort, an den die Daten Ihrer virtuellen Quellcomputer repliziert werden. Abhängig vom ausgewählten Computerstandort, stellt Site Recovery eine Liste der geeigneten Zielregionen bereit. Es empfiehlt sich, als Zielstandort den gleichen Standort zu verwenden wie für den Recovery Services-Tresor.
    - **Zielabonnement**: Das Zielabonnement für die Notfallwiederherstellung. Zielabonnement und Quellabonnement sind standardmäßig identisch.
    - **Zielressourcengruppe**: Die Ressourcengruppe, der all Ihre replizierten virtuellen Computer angehören. Azure Site Recovery erstellt standardmäßig in der Zielregion eine neue Ressourcengruppe, deren Name das Suffix „asr“ aufweist. Falls bereits eine von Azure Site Recovery erstellte Ressourcengruppe vorhanden ist, wird diese wiederverwendet. Sie können die Gruppe auch anpassen, wie im Abschnitt unten gezeigt. Der Speicherort der Zielressourcengruppe kann eine beliebige Azure-Region sein, mit Ausnahme der Region, in der die virtuellen Quellcomputer gehostet werden.
    - **Virtuelles Zielnetzwerk**: Site Recovery erstellt standardmäßig in der Zielregion ein neues virtuelles Netzwerk mit dem Namenssuffix „asr“. Dieses wird Ihrem Quellnetzwerk zugeordnet und für alle zukünftigen Schutzaktivitäten verwendet werden. Informationen zur Netzwerkzuordnung finden Sie [hier](site-recovery-network-mapping-azure-to-azure.md).
    - **Zielspeicherkonten (wenn die Quell-VM keine verwalteten Datenträger verwendet)**: Standardmäßig erstellt Site Recovery ein neues Zielspeicherkonto und übernimmt dabei die Speicherkonfiguration Ihrer Quell-VM. Sollte bereits ein Speicherkonto vorhanden sein, wird dieses wiederverwendet.
    - **Verwaltete Replikatdatenträger (wenn die Quell-VM verwaltete Datenträger verwendet)**: Site Recovery erstellt neue verwaltete Replikatdatenträger in der Zielregion, um die verwalteten Datenträger der Quell-VM zu spiegeln. Dabei wird der gleiche Speichertyp (Standard oder Premium) verwendet wie für die verwalteten Datenträger der Quell-VM.
    - **Cachespeicherkonten**: Site Recovery benötigt als zusätzliches Speicherkonto in der Quellregion ein so genanntes Cachespeicherkonto. Alle Änderungen an den virtuellen Quellcomputern werden nachverfolgt und vor der Replikation dieser Computer am Zielspeicherort an das Cachespeicherkonto gesendet.
    - **Verfügbarkeitsgruppe**: Standardmäßig erstellt Azure Site Recovery in der Zielregion eine neue Verfügbarkeitsgruppe mit dem Namenssuffix „asr“. Falls bereits eine von Azure Site Recovery erstellte Verfügbarkeitsgruppe vorhanden ist, wird diese wiederverwendet.
    - **Schlüsseltresore für Datenträgerverschlüsselung**: Standardmäßig erstellt Azure Site Recovery einen neuen Schlüsseltresor in der Zielregion mit dem Namenssuffix „asr“, der auf den Verschlüsselungsschlüsseln für die Datenträger des virtuellen Quellcomputers basiert. Falls bereits ein von Azure Site Recovery erstellter Schlüsseltresor vorhanden ist, wird er wiederverwendet.
    - **Schlüsseltresore für Schlüsselverschlüsselung**: Standardmäßig erstellt Azure Site Recovery einen neuen Schlüsseltresor in der Zielregion mit dem Namenssuffix „asr“, der auf den Verschlüsselungsschlüsseln für die Schlüssel des virtuellen Quellcomputers basiert. Falls bereits ein von Azure Site Recovery erstellter Schlüsseltresor vorhanden ist, wird er wiederverwendet.
    - **Replikationsrichtlinie**: Site Recovery definiert die Einstellungen für den Aufbewahrungsverlauf des Wiederherstellungspunkts und die Häufigkeit von App-konsistenten Momentaufnahmen. Standardmäßig erstellt Azure Site Recovery eine neue Replikationsrichtlinie mit der Standardeinstellung „24 Stunden“ für den Aufbewahrungszeitraum des Wiederherstellungspunkts und „60 Minuten“ für die App-konsistente Momentaufnahmenhäufigkeit.



## <a name="customize-target-resources"></a>Anpassen der Zielressourcen

Sie können die von Site Recovery verwendeten Standardzieleinstellungen ändern.


1. Klicken Sie neben „Zielabonnement“ auf **Anpassen:**, um das Standardzielabonnement zu ändern. Wählen Sie das Abonnement aus der Liste aller Abonnements aus, die im gleichen Azure Active Directory-Mandanten (AAD) verfügbar sind.

2. Klicken Sie neben „Ressourcengruppe“, „Netzwerk“, „Speicher“ und „Verfügbarkeitsgruppen“ auf **Anpassen**, um die folgenden Standardeinstellungen zu ändern:
    - Wählen Sie unter **Zielressourcengruppe** aus der Liste mit allen Ressourcengruppen, die innerhalb des Abonnements am Zielspeicherort vorhanden sind, die gewünschte Ressourcengruppe aus.
    - Wählen Sie unter **Virtuelles Zielnetzwerk** in der Liste mit allen virtuellen Netzwerken am Zielspeicherort das gewünschte Netzwerk aus.
    - Unter **Verfügbarkeitsgruppe** können Sie dem virtuellen Computer Verfügbarkeitsgruppeneinstellungen hinzufügen, sofern dieser einer Verfügbarkeitsgruppe in der Quellregion angehört.
    - Wählen Sie unter **Zielspeicherkonten** das Konto aus, das Sie verwenden möchten.


2. Klicken Sie neben „Verschlüsselungseinstellungen“ auf **Anpassen**, um die nachstehenden Standardeinstellungen zu ändern:
    - Wählen Sie unter **Schlüsseltresor für Zieldatenträgerverschlüsselung** den Schlüsseltresor für die Zieldatenträgerverschlüsselung in der Liste aller Schlüsseltresore am Zielspeicherort des Abonnements aus.
  - Wählen Sie unter **Schlüsseltresor für Zielschlüsselverschlüsselung** den Schlüsseltresor für die Zielschlüsselverschlüsselung in der Liste aller Schlüsseltresore am Zielspeicherort des Abonnements aus.

3. Klicken Sie auf **Zielressource erstellen** > **Replikation aktivieren**.
4. Nachdem Sie die Replikation für die virtuellen Computer aktiviert haben, können Sie unter **Replizierte Elemente** den VM-Integritätsstatus überprüfen.

>[!NOTE]
>Die Aktualisierung des Status kann während der Erstreplikation einige Zeit dauern, und zu Beginn wird unter Umständen kein Fortschritt angezeigt. Klicken Sie auf die Schaltfläche **Aktualisieren**, um den aktuellen Status abzurufen.
>

## <a name="update-target-vm-encryption-settings"></a>Aktualisieren der Verschlüsselungseinstellungen für die Ziel-VM
In den nachstehenden Szenarien müssen Sie die Verschlüsselungseinstellungen für den virtuellen Zielcomputer aktualisieren.
  - Sie haben die Site Recovery-Replikation auf dem virtuellen Computer aktiviert und Azure Disk Encryption (ADE) später auf dem virtuellen Quellcomputer aktiviert.
  - Sie haben die Site Recovery-Replikation auf dem virtuellen Computer aktiviert und den Datenträger-Verschlüsselungsschlüssel bzw. den Schlüsselverschlüsselungsschlüssel später auf dem virtuellen Quellcomputer aktiviert.

Sie können mit dem [Skript](#copy-ade-keys-to-dr-region-using-powershell-script) die Verschlüsselungsschlüssel in die Zielregion kopieren und dann die Zielverschlüsselungseinstellungen unter **Recovery Services-Tresor -> repliziertes Element -> Eigenschaften > Compute und Netzwerk** aktualisieren.

![update-ade-settings](./media/azure-to-azure-how-to-enable-replication-ade-vms/update-ade-settings.png)

## <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie mehr](site-recovery-test-failover-to-azure.md) über die Ausführung von Testfailovervorgängen.
