---
title: Marketo | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Leadverwaltung für Marketo konfigurieren.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: pbutlerm
ms.openlocfilehash: 482d7a9662a79503bb2b197d5a6c63c9fa3c1c96
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58113111"
---
# <a name="configure-lead-management-in-marketo"></a>Konfigurieren der Leadverwaltung in Marketo

Dieser Artikel beschreibt, wie Sie Marketo zum Verwenden von Microsoft Sales Leads einrichten.

1. Melden Sie sich bei Marketo an.
2. Wählen Sie **Design Studio** (Designstudio) aus.
    ![„Design Studio“ in Marketo](./media/cloud-partner-portal-lead-management-instructions-marketo/marketo1.png)

3.  Wählen Sie **New Form** (Neues Formular) aus.
    ![Neues Formular in Marketo](./media/cloud-partner-portal-lead-management-instructions-marketo/marketo2.png)

4.  Füllen Sie im Fenster „New Form“ (Neues Formular) die Pflichtfelder aus, und wählen Sie **Erstellen** aus.
    ![Erstellen eines neuen Formulars in Marketo](./media/cloud-partner-portal-lead-management-instructions-marketo/marketo3.png)

4.  Wählen Sie im Abschnitt „Field Details“ (Felddetails) **Fertig stellen** aus.
    ![Fertigstellen eines Formulars in Marketo](./media/cloud-partner-portal-lead-management-instructions-marketo/marketo4.png)

5.  Bestätigen Sie die Eingaben, und schließen Sie sie.

6.  Wählen Sie auf der Registerkarte „MarketplaceLeadBacked“ **Embed Code** (Code einbetten) aus.
    ![Option zum Einbetten von Code in Marketo](./media/cloud-partner-portal-lead-management-instructions-marketo/marketo5.png)

7.  Der Marketo-Einbettungscode wird etwa folgendermaßen angezeigt:

`<script src="//app-ys12.marketo.com/js/forms2/js/forms2.min.js"></script>`

    <form id="mktoForm_1179"></form>
    <script>MktoForms2.loadForm("("//app-ys12.marketo.com", "123-PQR-789", 1179);</script>

1. Kopieren Sie die angezeigten Werte, um im Cloud-Partnerportal die **Server-ID**, **Munchkin-ID** und **Formular-ID** in den Marketo-Feldern zu konfigurieren.

Das nächste Beispiel dient als Leitfaden zum Abrufen der benötigten IDs aus dem Marketo-Einbettungscode.

- Server-ID: **ys12**
- Munchkin-ID: **123-PQR-789**
- Formular-ID: **1179**\
