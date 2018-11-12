---
title: Microsoft Azure Data Box Heavy – Übersicht | Microsoft-Dokumentation
description: In diesem Artikel wird Azure Data Box beschrieben, eine Hybridlösung, die zum Übertragen sehr großer Datenmengen in Azure dient.
services: databox
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: overview
ms.custom: ''
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/24/2018
ms.author: alkohli
ms.openlocfilehash: 0a5b7f93f9ac6cc5b1076881727a42fd5b95ff4b
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51235155"
---
# <a name="what-is-azure-data-box-heavy-preview"></a>Was ist Azure Data Box Heavy? (Vorschau)

Mit der Hybridlösung Microsoft Azure Data Box können Sie Datenmengen von Hunderten von Terabyte schnell, kostengünstig und zuverlässig in Azure übertragen. Die sichere Datenübertragung wird beschleunigt, indem Sie ein spezielles Speichermedium mit 1 PB Speicherkapazität per Kurier erhalten. Das Gerät verfügt über ein widerstandsfähiges Gehäuse zum Schützen und Absichern von Daten während des Transports.

Data Box Heavy ist derzeit in der Vorschauphase. Sie können sich über das Azure-Portal für die Bestellung eines Geräts registrieren. Nach Empfang des Geräts in Ihrem Rechenzentrum können Sie es auf der lokalen Webbenutzeroberfläche schnell einrichten. Kopieren Sie die Daten von Ihren Servern auf das Gerät, und senden Sie es zurück an Azure. Im Azure-Rechenzentrum werden Ihre Daten dann automatisch vom Gerät in Azure hochgeladen. Der gesamte Prozess wird im Azure-Portal vom Data Box-Dienst von Anfang bis Ende nachverfolgt.


> [!IMPORTANT]
> - Data Box Heavy ist in der Vorschauphase. Lesen Sie die [Azure-Vertragsbedingungen für Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/), bevor Sie diese Lösung bereitstellen. 
> - Registrieren Sie sich im [Vorschauportal](https://aka.ms/), um ein Gerät zu bestellen.
> - In der Vorschauphase kann Data Box Heavy an Kunden in den USA und Europa versendet werden. Weitere Informationen finden Sie unter [Regionale Verfügbarkeit](#region-availability).

## <a name="use-cases"></a>Anwendungsfälle

Data Box Heavy eignet sich ideal für die Übertragung von Datenmengen von mehr als 500 TB in Szenarien ohne oder mit eingeschränkter Netzwerkkonnektivität. Die Datenverschiebung kann eine einmalige, eine periodische oder eine erste Massenübertragung von Daten sein, auf die regelmäßige Übertragungen folgen. Es folgen die verschiedenen Szenarien, in denen Data Box Heavy für die Datenübertragung verwendet werden kann.

 - **Einmalige Migration**: Wird verwendet, wenn eine große Menge von lokalen Daten in Azure verschoben wird. 
     - Verschieben einer Medienbibliothek von Offlinebändern in Azure, um eine Onlinemedienbibliothek zu erstellen
     - Migrieren Ihrer VM-Farm, SQL-Server und Anwendungen in Azure
     - Verschieben von Verlaufsdaten in Azure für eine detaillierte Analyse und Berichterstellung mithilfe von HDInsight

 - **Erste Massenübertragung**: Die Ausführung einer ersten Massenübertragung mithilfe von Data Box Heavy (Seeding) gefolgt von inkrementellen Übertragungen über das Netzwerk. 
     - Beispielsweise werden Partner im Bereich Sicherungslösungen wie Commvault und Data Box Heavy herangezogen, um die erste große Sicherung von Verlaufsdaten in Azure zu verschieben. Im Anschluss werden die inkrementellen Daten über das Netzwerk in Azure Storage übertragen.

 - **Periodische Uploads**: Werden verwendet, wenn regelmäßig große Datenmengen generiert werden und in Azure verschoben werden müssen. Ein Beispiel hierfür ist die Exploration in der Energiebranche, bei der Videodaten auf Bohrinseln und für Windfarmen generiert werden.      

## <a name="benefits"></a>Vorteile

Data Box Heavy ist so ausgelegt, dass sehr große Datenmengen ohne bzw. mit geringfügigen negativen Auswirkungen auf das Netzwerk in Azure verschoben werden können. Diese Lösung hat die folgenden Vorteile:

- **Geschwindigkeit**: Data Box Heavy verwendet sehr leistungsfähige Netzwerkschnittstellen mit 40 Gbit/s.

- **Sicherheit**: Data Box Heavy bietet einen integrierten Sicherheitsschutz für Geräte, Daten und den Dienst.
    - Das Gerät verfügt über ein widerstandsfähiges Gehäuse, das durch manipulationssichere Schrauben und Etiketten geschützt ist. 
    - Die Daten auf dem Gerät sind jederzeit durch AES-256-Bit-Verschlüsselung geschützt.
    - Das Gerät kann nur mit einem Kennwort entsperrt werden, das über das Azure-Portal bereitgestellt wird.
    - Der Dienst ist durch Azure-Sicherheitsfunktionen geschützt.
    - Nachdem Ihre Daten in Azure hochgeladen wurden, werden die Datenträger auf dem Gerät gemäß NIST-Standards (800-88r1) vollständig bereinigt.


<!--## Features and specifications

The Data Box Heavy device has the following features in this release.

| Specifications                                          | Description              |
|---------------------------------------------------------|--------------------------|
| Weight                                                  | < 50 lbs.                |
| Dimensions                                              | Device - Width: 309.0 mm Height: 430.4 mm Depth: 502.0 mm |            
| Rack space                                              | 7 U when placed in the rack on its side (cannot be rack-mounted)|
| Cables required                                         | 1 X power cable (included) <br> 2 RJ45 cables <br> 2 X SFP+ Twinax copper cables|
| Storage capacity                                        | 100 TB <br> 80 TB usable capacity after RAID 5 protection|
| Network interfaces                                      | 2 X 1 GbE interface - MGMT, DATA 3. <br> MGMT - for management, not user configurable, used for initial setup <br> DATA3 - for data, user configurable, and is dynamic by default <br> MGMT and DATA 3 can also work as 10 GbE <br> 2 X 10 GbE interface - DATA 1, DATA 2 <br> Both are for data, can be configured as dynamic (default) or static |
| Data transfer media                                     | RJ45, SFP+ copper 10 GbE Ethernet  |
| Security                                                | Rugged device casing with tamper-proof custom screws <br> Tamper-evident stickers placed at the bottom of the device|
| Data transfer rate                                      | Up to 80 TB in a day over 10 GbE network interface        |
| Management                                              | Local web UI - one-time initial setup and configuration <br> Azure portal - day-to-day device management        |-->

## <a name="components"></a>Komponenten

Data Box Heavy umfasst die folgenden Komponenten:

* **Data Box Heavy-Gerät**: Ein physisches Gerät mit einem widerstandsfähigen Gehäuse, in dem Daten sicher gespeichert werden. Das Data Box-Gerät bietet eine nutzbare Speicherkapazität von 800 TB. 

    
* **Data Box-Dienst:** Eine Erweiterung des Azure-Portals, mit der Sie ein Data Box Heavy-Gerät auf einer zentralen Webbenutzeroberfläche verwalten können, auf die an verschiedenen geografischen Standorten zugegriffen werden kann. Sie verwenden den Data Box-Dienst für die tägliche Verwaltung Ihres Data Box Heavy-Geräts. Zu den Aufgaben des Diensts gehören das Erstellen und Verwalten von Aufträgen, das Anzeigen und Verwalten von Warnmeldungen und das Verwalten von Freigaben.  

* **Lokale Webbenutzeroberfläche:** Eine webbasierte Benutzeroberfläche zum Konfigurieren des Geräts für das Herstellen der Verbindung mit dem lokalen Netzwerk und anschließende Registrieren des Geräts beim Data Box-Dienst. Auf der lokalen Webbenutzeroberfläche können Sie das auch Gerät herunterfahren und neu starten, Kopierprotokolle anzeigen und den Microsoft-Support kontaktieren, um eine Serviceanfrage zu stellen.


## <a name="the-workflow"></a>Workflow

Der Workflow umfasst üblicherweise die folgenden Schritte:

1. **Auftrag**: Erstellen Sie im Azure-Portal einen Auftrag, und geben Sie die Versandinformationen und das Azure-Zielspeicherkonto für Ihre Daten an. Wenn das Gerät verfügbar ist, wird es von Azure vorbereitet und mit einer Sendungsverfolgungs-ID versendet.

2. **Empfang**: Nach Erhalt des Geräts verbinden Sie es mit den vorgesehenen Kabeln mit dem Netzwerk und der Stromversorgung. Schalten Sie das Gerät ein, und verbinden Sie sich damit. Konfigurieren Sie die Netzwerk- und Bereitstellungsfreigaben auf dem Hostcomputer, von dem aus Sie die Daten kopieren möchten.

3. **Kopieren von Daten**: Kopieren Sie Daten auf die Data Box Heavy-Freigaben.

4. **Rücksendung**: Bereiten Sie das Gerät vor, schalten Sie es aus, und senden Sie es an das Azure-Rechenzentrum zurück.

5. **Hochladen**: Daten werden automatisch vom Gerät in Azure kopiert. Die Datenträger des Geräts werden gemäß NIST-Richtlinien (National Institute of Standards and Technology) auf sichere Weise gelöscht.

Bei diesem Prozess werden Sie per E-Mail über alle Statusänderungen informiert. 

## <a name="region-availability"></a>Regionale Verfügbarkeit

Data Box Heavy kann Daten basierend auf der Region, in der der Dienst bereitgestellt wird, dem Land, in das das Gerät versendet wird, und dem Azure Storage-Zielkonto übertragen, in das Sie die Daten übertragen. 

- **Verfügbarkeit des Diensts**: Bei dieser Version ist Data Box Heavy in den folgenden Regionen verfügbar:
    - Alle Public Cloud-Regionen in den USA: USA, Westen-Mitte; USA, Westen 2; USA, Westen; USA, Süden-Mitte; USA, Mitte; USA, Norden-Mitte; USA, Osten 2.
    - Europa: Europa, Westen und Europa, Norden.
    - Vereinigtes Königreich: Vereinigtes Königreich, Süden und Vereinigtes Königreich, Westen.
    - Frankreich: Frankreich, Mitte und Frankreich, Süden.

- **Zielspeicherkonten**: Die Speicherkonten, in denen die Daten gespeichert werden, sind in allen Azure-Regionen verfügbar, in denen der Dienst verfügbar ist. 

## <a name="sign-up"></a>Registrieren

Data Box Heavy ist in der Vorschauphase. Sie müssen sich für den Dienst registrieren. Führen Sie die folgenden Schritte aus, um sich für Data Box-Heavy zu registrieren:

1. Melden Sie sich unter https://aka.ms/azuredatabox beim Azure-Portal an.
2. Klicken Sie auf **+**, um eine neue Ressource zu erstellen. Suchen Sie nach **Azure Data Box**. Wählen Sie den Dienst **Azure Data Box** aus.

    <!--![The Data Box Heavy sign up 1]()-->

3. Klicken Sie auf **Create**.

    <!--![The Data Box Heavy sign up 2]()-->

4. Wählen Sie das Abonnement aus, das Sie für die Vorschauversion von Data Box Heavy verwenden möchten. Wählen Sie die Region aus, in der Sie die Data Box Heavy-Ressource bereitstellen möchten. Klicken Sie bei der Option **Data Box Heavy** auf **Registrieren**.

   <!--![The Data Box Heavy sign up 3]()-->

5. Beantworten Sie die Fragen nach dem Land, in dem sich die Daten befinden, Zeitrahmen, Azure-Zieldienst für die Datenübertragung, der Netzwerkbandbreite und Häufigkeit der Datenübertragung. Überprüfen Sie „Datenschutz und Nutzungsrichtlinien“, und aktivieren Sie das Kontrollkästchen „Microsoft darf Ihre E-Mail-Adresse für die Kontaktaufnahme verwenden“.

    <!--![The Data Box Heavy sign up 4]()-->

Nachdem Sie registriert und für die Vorschauversion aktiviert wurden, können Sie ein Data Box Heavy-Gerät bestellen.




