---
title: Aktualisieren eines bestehenden Angebots für Azure Marketplace | Microsoft-Dokumentation
description: Erläutert, wie ein vorhandenes Azure Marketplace-Angebot über das Cloud-Partnerportal aktualisiert wird.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: pbutlerm
ms.openlocfilehash: bfe2b0c70c8b912f6489ed461f5bcf6f233ed60d
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48806144"
---
<a name="update-an-existing-offer-for-azure-marketplace"></a>Aktualisieren eines bestehenden Angebots für Azure Marketplace
==============================================

Es gibt verschiedene Arten von Aktualisierungen, die Sie auf Ihr Angebot anwenden sollten, nachdem es freigeschaltet wurde.   Das [Cloud-Partnerportal](https://cloudpartner.azure.com/) hat mehrere Funktionen, die Ihnen bei der Bearbeitung eines Angebots helfen, um sicherzustellen, dass die Änderungen korrekt sind. Dazu gehören:

-  Hinzufügen einer neuen VM-Imageversion zu einer vorhandenen SKU
-  Ändern von Regionen, in denen eine SKU verfügbar ist
-  Hinzufügen von neuen SKUs
-  Aktualisieren von Marketplace-Metadaten für Angebote oder SKUs 
-  Aktualisieren von Preisen für Angebote mit nutzungsbasierter Bezahlung
-  Vergleichen von Funktionen
-  Versionsgeschichte der Funktionen

Nachdem Sie ein Angebot oder eine SKU geändert haben, muss es oder sie erneut veröffentlicht werden, bevor die Änderungen freigeschaltet werden.
In diesem Artikel werden die verschiedenen Aspekte zur Aktualisierung Ihres VM-Angebots beleuchtet.


<a name="unpermitted-changes-to-vm-offersku"></a>Nicht zugelassene Änderungen an VM-Angeboten/-SKUs
-----------------------------------

Einige Attribute von VM-Angeboten oder -SKUs können nicht geändert werden, sobald das Angebot im Azure Marketplace freigeschaltet ist.

-  Angebots-ID und Herausgeber-ID des Angebots
-  SKU-ID von vorhandenen SKUs
-  Anzahl von Datenträgern vorhandener SKUs
-  Abrechnungs-/Lizenzmodelländerungen an vorhandenen SKUs


<a name="updating-the-vm-image-version-for-a-sku"></a>Aktualisieren der VM-Imageversion für eine SKU
---------------------------------------

Das VM-Image für eine SKU Ihres Angebots wurde möglicherweise mit zusätzlichen Funktionen, Sicherheitspatches usw. aktualisiert. In solchen Szenarien empfiehlt es sich, das VM-Image zu aktualisieren, das Ihre SKU referenziert. Gehen Sie wie folgt vor, um ein VM-Image zu aktualisieren. 

1.  Melden Sie sich beim [Cloudpartnerportal](https://cloudpartner.azure.com/) an.
2.  Suchen Sie unter **Alle Angebote** nach dem Angebot, das aktualisiert werden soll.
3.  Klicken Sie im Formular **SKUs** auf die SKU, deren VM-Image aktualisiert werden soll.
4.  Klicken Sie unter **Datenträgerversion** auf **+Neue Datenträgerversion**, um ein neues VM-Image hinzuzufügen.
5.  Geben Sie die **Datenträgerversion** der neuen VM-Images an. Die Datenträgerversion muss dem Format der [semantischen Version](http://semver.org/) entsprechen. Versionen sollten im Format „X.Y.Z“ angegeben werden. „X“, „Y“ und „Z“ sind dabei ganze Zahlen. Stellen Sie sicher, dass die neue Version, die Sie angeben, höher ist als die vorherigen Versionen, da die neue Versionsnummer anderenfalls bei einer erneuten Veröffentlichung nicht im Azure-Portal oder Azure Marketplace angezeigt wird.
6.  Geben Sie für **URL der Betriebssystem-VHD** den Shared Access Signature-URI (SAS-URI) ein, der für die Betriebssystem-VHD erstellt wurde. Beachten Sie, dass die Anzahl von Datenträgern zwischen den verschiedenen Versionen der SKU nicht geändert werden kann. Wenn in vorherigen Versionen Datenträger konfiguriert wurden, müssen die Datenträger bei dieser neuen Version ebenfalls konfiguriert werden.
7.  Klicken Sie auf **Veröffentlichen**, um den Veröffentlichungsworkflow zu initiieren und Ihre neue VM-Version zur Live-Schaltung im Azure Marketplace und Azure-Portal abzurufen.


<a name="changing-regions-a-sku-is-available-in"></a>Ändern von Regionen, in denen eine SKU verfügbar ist
--------------------------------------

Im Laufe der Zeit sollten Sie Ihr Angebot bzw. Ihre SKU in weiteren Regionen zur Verfügung stellen.
Alternativ können Sie die Unterstützung für das Angebot bzw. die SKU in einer bestimmten Region beenden.
Führen Sie die folgenden Schritte aus, um diese Änderungen zu implementieren.

1.  Melden Sie sich beim [Cloudpartnerportal](https://cloudpartner.azure.com/) an.
2.  Suchen Sie unter **Alle Angebote** nach dem Angebot, das aktualisiert werden soll.
3.  Klicken Sie im Formular **SKUs** auf die SKU, deren regionale Verfügbarkeit aktualisiert werden soll.
4.  Klicken Sie im Feld **Verfügbare Länder/Regionen** auf die Schaltfläche **Länder auswählen**.
5.  Fügen Sie im Popupfenster „Verfügbare Regionen“ die für diese SKU gewünschten Regionen hinzu, oder entfernen Sie sie.
6.  Klicken Sie auf **Veröffentlichen**, um den Veröffentlichungsworkflow zu initiieren und die verfügbaren Regionen für Ihre SKUs zu aktualisieren.

Wenn eine SKU in einer neuen Region zur Verfügung gestellt wird, haben Sie die Möglichkeit, mithilfe der Funktion **Preisdaten exportieren** die Preise für diese bestimmte Region anzugeben. Wenn Sie eine Region erneut hinzufügen, die bereits zuvor einmal verfügbar war, können Sie deren Preise nicht aktualisieren, da Preisänderungen nicht zulässig sind.


<a name="adding-a-new-sku"></a>Hinzufügen einer neuen SKU
----------------

Führen Sie die folgenden Schritte aus, um eine neue SKU für Ihr vorhandenes Angebot verfügbar zu machen.

1.  Melden Sie sich beim [Cloudpartnerportal](https://cloudpartner.azure.com/) an.
2.  Suchen Sie unter **Alle Angebote** nach dem Angebot, das aktualisiert werden soll.
3.  Klicken Sie im Formular **SKUs** auf **Neue SKU hinzufügen** und geben Sie im Popupfenster eine **SKU-ID** an.
4.  Führen Sie die restlichen Schritte aus, die unter [Veröffentlichen eines virtuellen Computers in Azure Marketplace](./cloud-partner-portal-publish-virtual-machine.md) aufgeführt sind.
5.  Klicken Sie auf **Veröffentlichen**, um den Veröffentlichungsworkflow zu initiieren und Ihre neue SKU live zu schalten.


<a name="updating-offer-marketplace-metadata"></a>Aktualisieren von Marketplace-Metadaten eines Angebots
------------------------------------

In bestimmten Szenarien müssen Sie möglicherweise die mit Ihrem Angebot verknüpften Marketplace-Metadaten aktualisieren, wie etwa bei der Aktualisierung Ihrer Unternehmenslogos. Führen Sie die folgenden Schritte aus, um die Metadaten Ihres Angebots zu aktualisieren.

1.  Melden Sie sich beim [Cloudpartnerportal](https://cloudpartner.azure.com/) an.
2.  Suchen Sie unter **Alle Angebote** nach dem Angebot, das aktualisiert werden soll.
3.  Navigieren Sie zum Formular **Marketplace** und befolgen Sie die hier angegebenen Anweisungen, um Änderungen vorzunehmen.
4.  Klicken Sie auf **Veröffentlichen**, um den Veröffentlichungsworkflow zu initiieren und Ihre Änderungen live zu schalten.


<a name="updating-pricing-on-published-offers"></a>Aktualisieren der Preise für veröffentlichte Angebote
------------------------------------

Sobald Ihr Angebot mit nutzungsbasierter Bezahlung veröffentlicht ist, können Sie den Preis einer vorhandenen SKU nicht erhöhen, aber Sie können eine neue SKU unter demselben Angebot erstellen, die alte SKU löschen und Ihr Angebot dann erneut veröffentlichen. Außerdem können Sie den Preis für ein bereits veröffentlichtes Angebot verringern. So verringern Sie den Preis eines Angebots:

1.  Klicken Sie auf die SKU, für die Sie die Preise verringern möchten.
2.  Wenn Sie die Preise in der 1x1-GUI festgelegt haben, können Sie sie direkt in der Benutzeroberfläche ändern. Wenn Sie die Preise über Import/Export einer Kalkulationstabelle festlegen, können Sie die Preise nur über die Import/Export-Funktion verringern.
3.  Klicken Sie auf **Speichern**.
4.  Veröffentlichen Sie Ihr Angebot erneut, und schalten Sie es frei.

Die Preise sind für neue Kunden sichtbar, sobald sie auf der Website vollständig freigeschaltet sind, und alle neuen Kunden zahlen den jeweils niedrigeren Preis.

Für Bestandskunden wird die Preissenkung rückwirkend ab dem Anfang des Abrechnungszyklus berücksichtigt, in dem die Preissenkung wirksam wurde.
Bestandskunden, für die bereits in dem Zyklus abgerechnet wurde, in dem eine Preissenkung stattgefunden hat, erhalten während ihres nächsten Abrechnungszyklus eine Rückerstattung, die dem niedrigeren Preis entspricht.


<a name="simplified-currency-pricing"></a>Vereinfachte Währungspreise
---------------------------

Ab September 2018 können Sie einen neuen Abschnitt namens **Vereinfacht Währungspreise** sehen. Microsoft optimiert das Azure Marketplace-Geschäft dadurch, dass besser prognostizierbare Preise und Auflistungen von Ihren Kunden auf der ganzen Welt ermöglicht werden. Diese Optimierung wird dadurch erreicht, dass weniger Währungen verwendet werden, in denen Microsoft Ihren Kunden Rechnungen stellt.

Für den neuen Abschnitt werden die Preise in diesen neuen Währungen gestaltet. Sobald alle Kunden zu diesen neuen Abrechnungswährungen migriert wurden, wird der ursprüngliche Preisabschnitt entfernt, und es wird nur noch der Abschnitt "Vereinfachte Währungspreise" vorhanden sein.

Sie haben bis zum 1. November 2018 Zeit, neue Preise für die Regionen festzulegen, für die sich die Abrechnungswährung ändert. Für Regionen, für die sich die Abrechnungswährung nicht ändert, können Sie die Preise nicht erhöhen. Die folgenden Regionen sind diejenigen, für die sich die Währung ändert:

| Country                  | ISO2-Code | Vereinfachte Rechnungswährung |
|--------------------------|-----------|-----------------------------|
| **Algerien**              | DZ        | USD                         |
| **Argentinien**            | AR        | USD                         |
| **Bahrain**              | BH        | USD                         |
| **Belarus**              | BY        | USD                         |
| **Brasilien**               | BR        | BRL                         |
| **Bulgarien**             | BG        | EUR                         |
| **Chile**                | CL        | USD                         |
| **Kolumbien**             | CO        | USD                         |
| **Costa Rica**           | CR        | USD                         |
| **Kroatien**              | HR        | EUR                         |
| **Tschechische Republik**       | CZ        | EUR                         |
| **Ägypten**                | EG        | USD                         |
| **Guatemala**            | GT        | USD                         |
| **Hongkong**            | HK        | USD                         |
| **Ungarn**              | HU        | EUR                         |
| **Island**              | IS        | EUR                         |
| **Indonesien**            | ID        | USD                         |
| **Israel**               | IL        | USD                         |
| **Jordanien**               | JO        | USD                         |
| **Kasachstan**           | KZ        | USD                         |
| **Kenia**                | KE        | USD                         |
| **Kuwait**               | KW        | USD                         |
| **Liechtenstein**        | LI        | EUR                         |
| **Mazedonien (ehem. jugoslawische Republik Mazedonien)**     | MK        | USD                         |
| **Malaysia**             | MY        | USD                         |
| **Mexiko**               | MX        | USD                         |
| **Montenegro**           | ME        | USD                         |
| **Marokko**              | MA        | USD                         |
| **Nigeria**              | NG        | USD                         |
| **Oman**                 | OM        | USD                         |
| **Pakistan**             | PK        | USD                         |
| **Paraguay**             | PY        | USD                         |
| **Peru**                 | PE        | USD                         |
| **Philippinen**          | PH        | USD                         |
| **Polen\***             | PL        | EUR                         |
| **Katar**                | QA        | USD                         |
| **Rumänien**              | RO        | EUR                         |
| **Saudi-Arabien**         | SA        | USD                         |
| **Serbien**               | RS        | USD                         |
| **Singapur**            | SG        | USD                         |
| **Südafrika**         | ZA        | USD                         |
| **Thailand**             | TH        | USD                         |
| **Trinidad und Tobago**  | TT        | USD                         |
| **Tunesien**              | TN        | USD                         |
| **Türkei**               | TR        | USD                         |
| **Ukraine**              | UA        | USD                         |
| **Vereinigte Arabische Emirate** | AE        | USD                         |
| **Uruguay**              | UY        | USD                         |
|  |  |  |

> [!NOTE]
> Wenn Sie APIs verwenden, um Ihr Angebot zu veröffentlichen, können Sie im Angebot einen neuen Abschnitt namens **JSON** sehen. Dieser wird mit `virtualMachinePricingV2` oder `monthlyPricingV2` kommentiert, je nach dem Typ des Angebots.

Wenn Sie Fragen zu dieser Änderung haben, wenden Sie sich an den Azure Marketplace-Support.


<a name="compare-feature"></a>Vergleichen-Funktion
---------------

Wenn Sie Änderungen an einem bereits veröffentlichten Angebot vorgenommen haben, können Sie die *Vergleichen*-Funktion verwenden, um die vorgenommenen Änderungen zu prüfen. So nutzen Sie „Vergleichen“:

1.  Zu jedem Zeitpunkt des Bearbeitungsprozesses können Sie für Ihr Angebot auf die Schaltfläche **Vergleichen** klicken.

2.  Zeigen Sie nebeneinander Versionen von Marketingressourcen und Metadaten an.


<a name="history-of-publishing-actions"></a>Verlauf der Veröffentlichungsaktionen
-----------------------------

Um bereits ausgeführte Veröffentlichungsaktivitäten anzuzeigen, klicken Sie auf die Registerkarte **Verlauf** im linken Navigationsbereich im Cloud-Partnerportal. Auf dieser Registerkarte werden mit Zeitstempeln versehene Aktionen angezeigt, die während der Lebensdauer Ihrer Azure Marketplace-Angebote ausgeführt wurden.
