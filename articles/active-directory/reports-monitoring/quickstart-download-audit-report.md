---
title: 'Schnellstart: Herunterladen eines Überwachungsberichts über das Azure-Portal | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie einen Überwachungsbericht über das Azure-Portal herunterladen.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 4de121ea-f4aa-4c8a-aae4-700c2c5e97a2
ms.service: active-directory
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 09/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: d6ecf62b46c25ec7cf4ab80219d1bc220e4eb4cc
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2018
ms.locfileid: "46367405"
---
# <a name="quickstart-download-an-audit-report-using-the-azure-portal"></a>Schnellstart: Herunterladen eines Überwachungsberichts über das Azure-Portal

In dieser Schnellstartanleitung erfahren Sie, wie Sie die Überwachungsprotokolle der letzten 24 Stunden für Ihren Mandanten herunterladen.

## <a name="prerequisites"></a>Voraussetzungen

Erforderlich:

* Ein Azure Active Directory-Mandant. 
* Ein Benutzer, der über eine der Rollen „Sicherheitsadministrator“, „Leseberechtigung für Sicherheitsfunktionen“ oder „globaler Administrator“ für den Mandanten verfügt. Darüber hinaus kann jeder Benutzer im Mandanten auf die eigenen Überwachungsprotokolle zugreifen.

## <a name="quickstart-download-an-audit-report"></a>Schnellstart: Herunterladen eines Überwachungsberichts

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com).
2. Wählen Sie im linken Navigationsbereich die Option **Azure Active Directory** aus, klicken Sie auf die Schaltfläche **Verzeichnis wechseln**, um Ihr Active Directory auszuwählen.
3. Wählen Sie im Dashboard **Azure Active Directory** und dann **Überwachungsprotokolle** aus. 
4. Wählen Sie in der Dropdownliste **Datumsbereich** den Eintrag **Letzte 24 Stunden** aus, und klicken Sie auf **Anwenden**, um die Überwachungsprotokolle für die letzten 24 Stunden anzuzeigen. 
5. Klicken Sie auf die Schaltfläche **Herunterladen**, um eine CSV-Datei mit den gefilterten Datensätzen herunterzuladen. 

![Berichterstellung](./media/quickstart-download-audit-report/download-audit-logs.png)

## <a name="next-steps"></a>Nächste Schritte

* [Berichte zu Anmeldeaktivitäten im Azure Active Directory-Portal](concept-sign-ins.md)
* [Vermerkdauer für Azure Active Directory-Berichte](reference-reports-data-retention.md)
* [Latenzen bei Azure Active Directory-Berichten – Vorschau](reference-reports-latencies.md)
