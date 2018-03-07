---
title: "Erstellen eines tabellarischen Beispielmodells für Ihren Azure Analysis Services-Server | Microsoft-Dokumentation"
description: Erfahren Sie, wie Sie ein Beispielmodell in Azure Analysis Services erstellen.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 02/27/2018
ms.author: owend
ms.openlocfilehash: 1cc36ebf4c410d4764eb28ab89d7f2a5e7749ae5
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="tutorial-create-a-sample-model"></a>Tutorial: Erstellen eines Beispielmodells

In diesem Tutorial erstellen Sie ein Adventure Works-Beispielmodell. Das Beispielmodell ist eine abgeschlossene Version des Datenmodellierungstutorials für Internetverkäufe in Adventure Works (1200). Ein Beispielmodell eignet sich zum Testen der Modellverwaltung, zum Herstellen einer Verbindung mit Tools und Clientanwendungen und zum Abfragen von Modelldaten.

## <a name="before-you-begin"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

- Einen Azure Analysis Services-Server
- Serveradministratorberechtigungen

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="create-a-sample-model"></a>Erstellen einer Beispieltabelle

1. Klicken Sie auf dem Server unter **Übersicht** auf **Neues Modell**.

    ![Erstellen einer Beispieltabelle](./media/analysis-services-create-sample-model/aas-create-sample-new-model.png)

2. Vergewissern Sie sich unter **Neues Modell** > **Datenquelle auswählen**, dass **Beispieldaten** ausgewählt ist, und klicken Sie dann auf **Hinzufügen**.

    ![Auswählen der Beispieldaten](./media/analysis-services-create-sample-model/aas-create-sample-data.png)

3. Vergewissern Sie sich unter **Übersicht**, dass das Beispiel `adventureworks` erstellt wurde.

    ![Auswählen der Beispieldaten](./media/analysis-services-create-sample-model/aas-create-sample-verify.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Ihr Beispielmodell verwendet Cachespeicherressourcen. Wenn Sie Ihr Beispielmodell nicht zum Testen verwenden, sollten Sie es von Ihrem Server entfernen.

> [!NOTE]
> In diesen Schritten wird beschrieben, wie Sie ein Modell mithilfe von SSMS auf einem Server löschen. Sie können ein Modell auch über das Vorschaufeature des Web-Designers löschen.

1. Klicken Sie in SSMS im **Objekt-Explorer** auf **Verbinden** > **Analysis Services**.

2. Fügen Sie unter **Mit Server verbinden** den Servernamen ein, wählen Sie unter **Authentifizierung** die Option **Active Directory – universell mit MFA-Unterstützung** aus, geben Sie Ihren Benutzernamen ein, und klicken Sie dann auf **Verbinden**.

    ![Anmelden](./media/analysis-services-create-sample-model/aas-create-sample-cleanup-signin.png)

3. Klicken Sie im **Objekt-Explorer** mit der rechten Maustaste auf die Beispieldatenbank `adventureworks`, und klicken Sie dann auf **Löschen**.

    ![Löschen der Beispieldatenbank](./media/analysis-services-create-sample-model/aas-create-sample-cleanup-delete.png)

## <a name="next-steps"></a>Nächste Schritte 

[Herstellen einer Verbindung in Power BI Desktop](analysis-services-connect-pbi.md)   
[Verwalten von Datenbankrollen und Benutzern](analysis-services-database-users.md)


