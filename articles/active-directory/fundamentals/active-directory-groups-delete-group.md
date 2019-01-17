---
title: Löschen einer Gruppe – Azure Active Directory | Microsoft-Dokumentation
description: Anleitung zum Löschen einer Gruppe mit Azure Active Directory.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: lizross
ms.reviewer: krbain
ms.custom: it-pro, seodec18
ms.openlocfilehash: 405e86506f13472886878eb18557121c752e9464
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/07/2019
ms.locfileid: "54062297"
---
# <a name="delete-a-group-using-azure-active-directory"></a>Löschen einer Gruppe mithilfe von Azure Active Directory
Für das Löschen einer Azure Active Directory-Gruppe (Azure AD) kann es verschiedene Gründe geben. Häufig liegt jedoch einer der folgenden Gründe vor:

- Der **Gruppentyp** wurde auf die falsche Option festgelegt.

- Es wurde versehentlich eine falsche oder eine bereits vorhandene Gruppe hinzugefügt. 

- Die Gruppe wird nicht mehr benötigt.

## <a name="to-delete-a-group"></a>So löschen Sie eine Gruppe
1. Melden Sie sich mit dem Konto eines globalen Administrators für das Verzeichnis beim [Azure-Portal](https://portal.azure.com) an.

2. Klicken Sie auf **Azure Active Directory** und anschließend auf **Gruppen**.

3. Suchen Sie auf der Seite **Gruppen – Alle Gruppen** nach der Gruppe, die Sie löschen möchten, und wählen Sie sie aus. Für diese Schritte verwenden wir **MDM policy - East**.

    ![Seite „Gruppen – Alle Gruppen“ mit hervorgehobenem Gruppennamen](media/active-directory-groups-delete-group/group-all-groups-screen.png)

4. Klicken Sie auf der Übersichtsseite für **MDM policy - East** auf **Löschen**.

    Die Gruppe wird aus dem Azure Active Directory-Mandanten gelöscht.

    ![Übersichtsseite für „MDM policy - East“ mit hervorgehobener Option „Löschen“](media/active-directory-groups-delete-group/group-overview-blade.png)

## <a name="next-steps"></a>Nächste Schritte

- Wenn Sie eine Gruppe versehentlich gelöscht haben, können Sie sie erneut erstellen. Weitere Informationen finden Sie unter [Gewusst wie: Erstellen einer Basisgruppe und Hinzufügen von Mitgliedern mit Azure Active Directory](active-directory-groups-create-azure-portal.md).

- Wenn Sie eine Office 365-Gruppe versehentlich gelöscht haben, können Sie sie möglicherweise wiederherstellen. Weitere Informationen finden Sie unter [Wiederherstellen einer gelöschten Office 365-Gruppe in der Azure Active Directory-Vorschau](../users-groups-roles/groups-restore-deleted.md).
