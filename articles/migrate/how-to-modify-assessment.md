---
title: Anpassen der Einstellungen einer Azure Migrate-Bewertung | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie eine Bewertung für die Migration von virtuellen VMware-Computern mithilfe von Azure Migration Planner einrichten und ausführen.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 10/30/2018
ms.author: raynew
ms.openlocfilehash: 2423c4fde177ab50552af580a60c7a15550e5586
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52840434"
---
# <a name="customize-an-assessment"></a>Anpassen einer Bewertung

[Azure Migrate](migrate-overview.md) erstellt Bewertungen mit Standardeigenschaften. Nach dem Erstellen einer Bewertung können Sie die Standardeigenschaften entsprechend den Anweisungen in diesem Artikel ändern.


## <a name="edit-assessment-properties"></a>Bearbeiten von Bewertungseigenschaften

1. Wählen Sie auf der Seite **Bewertungen** des Migrationsprojekts die Bewertung aus, und klicken Sie auf **Eigenschaften bearbeiten**.
2. Passen Sie die Bewertungseigenschaften anhand der unten angegebenen Details an:

    **Einstellung** | **Details** | **Standard**
    --- | --- | ---
    **Zielstandort** | Der Azure-Standort, zu dem die Migration durchgeführt werden soll.<br/><br/> Azure Migrate unterstützt derzeit 30 Regionen, einschließlich „Australien, Osten“, „Australien, Südosten“, „Brasilien, Süden“, „Kanada, Mitte“, „Kanada, Osten“, „Indien, Mitte“, „USA, Mitte“, „China, Osten“, „China, Norden“, „Asien, Osten“, „USA, Osten“, „Deutschland, Mitte“, „Deutschland, Nordosten“, „USA, Osten 2“, „Japan, Osten“, „Japan, Westen“, „Südkorea, Mitte“, „Südkorea, Süden“, „USA, Norden-Mitte“, „Europa, Norden“, „USA, Süden-Mitte“, „Asien, Südosten“, „Indien, Süden“, „Vereinigtes Königreich, Süden“, „Vereinigtes Königreich, Westen“, „US Gov Arizona“, „US Gov Texas“, „US Gov Virginia“, „USA, Westen-Mitte“, „Europa, Westen“, „Indien, Westen“, „USA, Westen“ und „USA, Westen 2“. |  „USA, Westen 2“ ist der Standardstandort.
    **Speichertyp** | Sie können diese Eigenschaft nutzen, um die Art der Datenträger anzugeben, die Sie in Azure zuordnen möchten. Für die Größenanpassung für die lokale Umgebung können Sie den Zieldatenträgertyp als „Managed Disks Premium“ oder „Managed Disks Standard“ angeben. Für die leistungsbasierte Größenanpassung können Sie den Zieldatenträgertyp als „Automatisch“, „Managed Disks Premium“ oder „Managed Disks Standard“ angeben. Beim Angeben des Speichertyps als „Automatisch“ erfolgt die Datenträgerempfehlung basierend auf den Leistungsdaten der Datenträger (IOPS und Durchsatz). Wenn Sie beispielsweise eine [SLA von 99,9% für Einzelinstanz-VMs](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/) erzielen möchten, können Sie den Speichertyp als „Managed Disks Premium“ angeben. So wird sichergestellt, dass alle Datenträger der Bewertung als Datenträger vom Typ „Managed Disks Premium“ empfohlen werden. Hinweis: Azure Migrate unterstützt nur verwaltete Datenträger für die Migrationsbewertung. | Standardmäßig wird „Managed Disks Premium“ verwendet (mit dem Größenkriterium *Wie lokal*).
    **Reservierte Instanzen** |  Sie können auch angeben, ob Sie über [reservierte Instanzen](https://azure.microsoft.com/pricing/reserved-vm-instances/) in Azure verfügen. Dies wird dann von Azure Migrate bei der Kostenschätzung entsprechend berücksichtigt. Reservierte Instanzen gelten nicht für unabhängige Regionen (Azure Government, Deutschland und China), und sie gelten nur für nutzungsbasierte Angebote in Azure Migrate. | Der Standardwert für diese Eigenschaft sind für 3 Jahre reservierte Instanzen.
    **Größenkriterium** | Das Kriterium, das in Azure Migrate zur Größenanpassung virtueller Computer für Azure verwendet werden soll. Sie können eine *leistungsbasierte* Größenanpassung vornehmen oder die Größe der virtuellen Computer *lokal* ändern, ohne den Leistungsverlauf zu berücksichtigen. | Die leistungsbasierte Größenanpassung ist die Standardoption.
    **Leistungsverlauf** | Die zur Bewertung der Leistung der virtuellen Computer zu berücksichtigende Dauer. Diese Eigenschaft ist nur anwendbar, wenn als Größenkriterium die *leistungsbasierte Größenanpassung* festgelegt ist. | Standardmäßig ist ein Tag festgelegt.
    **Perzentilwert der Nutzung** | Der für die Größenanpassung zu berücksichtigende Perzentilwert des festgelegten Leistungsbeispiels. Diese Eigenschaft ist nur anwendbar, wenn als Größenkriterium die *leistungsbasierte Größenanpassung* festgelegt ist.  | Der Standardwert ist das 95. Perzentil.
    **VM-Serie** | Sie können die VM-Serie angeben, die Sie für die Größenanpassung berücksichtigen möchten. Wenn Sie beispielsweise eine Produktionsumgebung haben, die Sie nicht auf VMs der A-Serie in Azure migrieren möchten, können Sie die A-Serie von der Liste oder aus der Serie ausschließen, sodass die Größenanpassung nur in der ausgewählten Serie erfolgt. | Standardmäßig sind alle VM-Serien ausgewählt.
    **Komfortfaktor** | Bei Azure Migrate wird während der Bewertung ein Puffer (Komfortfaktor) berücksichtigt. Dieser Puffer wird zusätzlich zu den Daten zur Computernutzung für VMs (CPU, Arbeitsspeicher, Datenträger und Netzwerk) angewendet. Beim Komfortfaktor geht es um Bereiche wie saisonale Nutzung, kurzer Leistungsverlauf und voraussichtliche zukünftige Zunahme der Nutzung.<br/><br/> Beispiel: Für eine VM mit zehn Kernen und 20% Auslastung ergibt sich normalerweise eine VM mit zwei Kernen. Bei einem Komfortfaktor von 2.0x ist das Ergebnis dagegen eine VM mit vier Kernen. | Die Standardeinstellung ist 1.3x.
    **Angebot** | Das [Azure-Angebot](https://azure.microsoft.com/support/legal/offer-details/), für das Sie registriert sind. | [Nutzungsbasierte Bezahlung](https://azure.microsoft.com/offers/ms-azr-0003p/) ist die Standardeinstellung.
    **Währung** | Rechnungswährung. | Der Standardwert ist US-Dollar.
    **Rabatt (%)** | Alle abonnementspezifischen Rabatte, die Sie zusätzlich zum Azure-Angebot erhalten. | Die Standardeinstellung ist 0 %.
    **VM-Betriebszeit** | Wenn Ihre virtuellen Computer nicht rund um die Uhr in Azure aktiv sind, können Sie den Zeitraum angeben (Anzahl der Tage pro Monat und Anzahl der Stunden pro Tag), für den sie voraussichtlich ausgeführt werden, um eine entsprechende Kostenschätzung zu erhalten. | Der Standardwert sind 31 Tage pro Monat und 24 Stunden pro Tag.
    **Azure-Hybridvorteil** | Geben Sie an, ob Sie über die Software Assurance verfügen und den [Azure-Hybridvorteil](https://azure.microsoft.com/pricing/hybrid-use-benefit/) nutzen können. Wenn diese Einstellung auf „Ja“ festgelegt ist, werden für virtuelle Windows-Computer Nicht-Windows-Azure-Preise veranschlagt. | Die Standardeinstellung ist „Ja“.

3. Klicken Sie auf **Speichern**, um die Bewertung zu aktualisieren.


## <a name="next-steps"></a>Nächste Schritte

[Weitere Informationen](concepts-assessment-calculation.md) zur Berechnung von Bewertungen
