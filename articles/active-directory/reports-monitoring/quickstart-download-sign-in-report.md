---
title: 'Schnellstart: Herunterladen eines Anmeldeberichts über das Azure-Portal | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie einen Anmeldebericht über das Azure-Portal herunterladen.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 9131f208-1f90-4cc1-9c29-085cacd69317
ms.service: active-directory
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 3e20af1c90f0e8a7a582d2d01dc4218a14496c40
ms.sourcegitcommit: e68df5b9c04b11c8f24d616f4e687fe4e773253c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/20/2018
ms.locfileid: "53653287"
---
# <a name="quickstart-download-a-sign-in-report-using-the-azure-portal"></a>Schnellstart: Herunterladen eines Anmeldeberichts über das Azure-Portal

In diesem Schnellstart erfahren Sie, wie Sie die Anmeldedaten Ihres Mandanten für die letzten 24 Stunden herunterladen.

## <a name="prerequisites"></a>Voraussetzungen

Erforderlich:

* Ein Azure Active Directory-Mandant mit einer Premium-Lizenz zum Anzeigen des Berichts zu Anmeldeaktivitäten. Unter [Erste Schritte mit Azure Active Directory Premium](../fundamentals/active-directory-get-started-premium.md) erfahren Sie, wie Sie ein Upgrade für Ihre Azure Active Directory-Edition durchführen.
* Ein Benutzer, der über die Rolle **Sicherheitsadministrator**, **Benutzer mit Leseberechtigung für Sicherheitsfunktionen**, **Benutzer mit Leseberechtigung für Berichte** oder **globaler Administrator** für den Mandanten verfügt. Darüber hinaus kann jeder Benutzer im Mandanten auf die eigenen Anmeldungen zugreifen.

## <a name="quickstart-download-a-sign-in-report"></a>Schnellstart: Herunterladen eines Anmeldeberichts

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com).
2. Wählen Sie im linken Navigationsbereich die Option **Azure Active Directory** aus, und klicken Sie auf die Schaltfläche **Verzeichnis wechseln**, um Ihr Active Directory-Verzeichnis auszuwählen.
3. Wählen Sie im Dashboard **Azure Active Directory** und dann **Anmeldungen** aus. 
4. Wählen Sie im Dropdownmenü **Datum** den Eintrag **Letzte 24 Stunden** und dann **Anwenden** aus, um die Anmeldungen der letzten 24 Stunden anzuzeigen. 
5. Wählen Sie die Schaltfläche **Herunterladen** aus, um eine CSV-Datei mit den gefilterten Datensätzen herunterzuladen. 

![Berichterstellung](./media/quickstart-download-sign-in-report/download-sign-ins.png)

## <a name="next-steps"></a>Nächste Schritte

* [Berichte zu Anmeldeaktivitäten im Azure Active Directory-Portal](concept-sign-ins.md)
* [Vermerkdauer für Azure Active Directory-Berichte](reference-reports-data-retention.md)
* [Latenzen bei Azure Active Directory-Berichten – Vorschau](reference-reports-latencies.md)
