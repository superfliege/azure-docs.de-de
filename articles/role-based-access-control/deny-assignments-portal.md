---
title: Anzeigen von Ablehnungszuweisungen für Azure-Ressourcen mit dem Azure-Portal | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mithilfe des Azure-Portals die Benutzer, Gruppen, Dienstprinzipale und verwalteten Identitäten anzeigen können, denen der Zugriff auf bestimmte Aktionen von Azure-Ressourcen untersagt wurde.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 8078f366-a2c4-4fbb-a44b-fc39fd89df81
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/30/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: ec5e3daf1d4d799aab043f241548a3b4177f567c
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/18/2019
ms.locfileid: "56343237"
---
# <a name="view-deny-assignments-for-azure-resources-using-the-azure-portal"></a>Anzeigen von Ablehnungszuweisungen für Azure-Ressourcen mit dem Azure-Portal

[Ablehnungszuweisungen](deny-assignments.md) blockieren Aktionen von Azure-Ressourcen für bestimmte Benutzer, selbst wenn diesen durch eine Rollenzuweisung Zugriff erteilt wurde. Auch wenn Sie keine eigenen Ablehnungszuweisungen erstellen können, müssen Sie dennoch in der Lage sein, Ablehnungszuweisungen anzuzeigen, da diese Ihre allgemeinen Berechtigungen beeinflussen können. Um Informationen zu einer Ablehnungszuweisung zu erhalten, benötigen Sie die Berechtigung `Microsoft.Authorization/denyAssignments/read`, die in den meisten [integrierten Rollen](built-in-roles.md) für Azure-Ressourcen enthalten ist.

In diesem Artikel wird beschrieben, wie Sie mit dem Azure-Portal Ablehnungszuweisungen anzeigen.

> [!NOTE]
> Aktuell sind Ablehnungszuweisungen schreibgeschützt und können nur von Azure festgelegt werden.

## <a name="view-deny-assignments"></a>Anzeigen von Ablehnungszuweisungen

Führen Sie die folgenden Schritte aus, um Ablehnungszuweisungen für ein Abonnement oder eine Verwaltungsgruppe anzuzeigen.

1. Klicken Sie im Azure-Portal auf **Alle Dienste** und dann auf **Verwaltungsgruppen** oder **Abonnements**.

1. Klicken Sie auf die Verwaltungsgruppe oder das Abonnement, die bzw. das Sie anzeigen möchten.

1. Klicken Sie auf **Zugriffssteuerung (IAM)**.

1. Klicken Sie auf die Registerkarte **Ablehnungszuweisungen** (oder klicken Sie auf der Kachel „Ablehnungszuweisungen anzeigen“ auf **Anzeigen**).

    Wenn Ablehnungszuweisungen für diesen Bereich oder an diesen Bereich vererbte Ablehnungszuweisungen vorhanden sind, werden diese angezeigt.

    ![Zugriffssteuerung: Registerkarte „Ablehnungszuweisungen“](./media/deny-assignments-portal/access-control-deny-assignments.png)

1. Um zusätzliche Spalten anzuzeigen, klicken Sie auf **Spalten bearbeiten**.

    ![Ablehnungszuweisungen: Spalten](./media/deny-assignments-portal/deny-assignments-columns.png)

    |  |  |
    | --- | --- |
    | **Name** | Der Name der Ablehnungszuweisung. |
    | **Prinzipaltyp** | Benutzer, Gruppe, vom System definierte Gruppe oder Dienstprinzipal. |
    | **Verweigert**  | Der Name des Sicherheitsprinzipals, der in der Ablehnungszuweisung enthalten ist. |
    | **Id** | Eindeutiger Bezeichner für die Ablehnungszuweisung. |
    | **Ausgeschlossene Prinzipale** | Gibt an, ob Sicherheitsprinzipale von der Ablehnungszuweisung ausgeschlossen sind. |
    | **Gilt nicht für untergeordnete Elemente** | Gibt an, ob die Ablehnungszuweisung an Unterbereiche vererbt wird. |
    | **Durch System geschützt** | Gibt an, ob die Ablehnungszuweisung durch Azure verwaltet wird. Aktuell immer „Ja“. |
    | **Umfang** | Eine Verwaltungsgruppe, ein Abonnement, eine Ressourcengruppe oder eine Ressource. |

1. Aktivieren Sie das Kontrollkästchen für eines der aktivierten Elemente, und klicken Sie auf **OK**, um die ausgewählten Spalten anzuzeigen.

## <a name="view-details-about-a-deny-assignment"></a>Anzeigen von Details zu einer Ablehnungszuweisung

Führen Sie die folgenden Schritte aus, um zusätzliche Details zu einer Ablehnungszuweisung anzuzeigen.

1. Öffnen Sie den Bereich **Ablehnungszuweisungen**, wie im vorherigen Abschnitt beschrieben.

1. Klicken Sie auf den Namen der Ablehnungszuweisung, um das Blatt **Benutzer** zu öffnen.

    ![Ablehnungszuweisung: Benutzer](./media/deny-assignments-portal/deny-assignment-users.png)

    Das Blatt **Benutzer** umfasst die folgenden zwei Abschnitte.

    |  |  |
    | --- | --- |
    | **Die Ablehnungszuweisung gilt für**  | Sicherheitsprinzipale, auf die die Ablehnungszuweisung angewendet wird. |
    | **Die Ablehnungszuweisung schließt Folgendes aus** | Gibt Sicherheitsprinzipale an, die von der Ablehnungszuweisung ausgeschlossen sind. |

    **Vom System definierter Prinzipal** repräsentiert alle Benutzer, Gruppen, Dienstprinzipale und verwalteten Identitäten in einem Azure AD-Verzeichnis.

1. Um eine Liste der Berechtigungen anzuzeigen, die verweigert wurden, klicken Sie auf **Verweigerte Berechtigungen**.

    ![Ablehnungszuweisung: verweigerte Berechtigungen](./media/deny-assignments-portal/deny-assignment-denied-permissions.png)

    | Aktionstyp | BESCHREIBUNG |
    | --- | --- |
    | **Aktionen**  | Verweigerte Verwaltungsvorgänge. |
    | **NotActions** | Verwaltungsvorgänge, die von den verweigerten Verwaltungsvorgängen ausgeschlossen wurden. |
    | **DataActions**  | Verweigerte Datenvorgänge. |
    | **NotDataActions** | Datenvorgänge, die von den verweigerten Datenvorgängen ausgeschlossen wurden. |

    Für das im vorstehenden Screenshot gezeigte Beispiel gelten die folgenden effektiven Berechtigungen:

    - Alle Speichervorgänge für die Datenebene bis auf Computevorgänge wurden verweigert.

1. Um die Eigenschaften für eine Ablehnungszuweisung anzuzeigen, klicken Sie auf **Eigenschaften**.

    ![Ablehnungszuweisung: Eigenschaften](./media/deny-assignments-portal/deny-assignment-properties.png)

    Auf dem Blatt **Eigenschaften** können Sie den Namen, die ID, die Beschreibung und den Bereich für eine Ablehnungszuweisung anzeigen. Die Option **Gilt nicht für untergeordnete Elemente** gibt an, ob die Ablehnungszuweisung an Unterbereiche vererbt wird. Die Option **Vom System definiert** gibt an, ob diese Ablehnungszuweisung durch Azure verwaltet wird. Aktuell lautet die Einstellung immer **Ja**.

## <a name="next-steps"></a>Nächste Schritte

* [Verstehen von Ablehnungszuweisungen für Azure-Ressourcen](deny-assignments.md)
* [Auflisten von Ablehnungszuweisungen für Azure-Ressourcen mithilfe der REST-API](deny-assignments-rest.md)
