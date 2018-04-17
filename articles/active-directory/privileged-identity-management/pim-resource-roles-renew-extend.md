---
title: Privileged Identity Management für Azure-Ressourcen – Verlängern und Erneuern von Rollen | Microsoft-Dokumentation
description: In diesem Dokument wird beschrieben, wie Sie die Azure-Ressourcenrollen für PIM-Ressourcen verlängern und erneuern.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: mwahl
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 2547b3793688eb51a4114f30bfcf61a9402f2cd2
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="privileged-identity-management---resource-roles---extend-or-renew"></a>Privileged Identity Management für Azure-Ressourcen – Ressourcenrollen – Verlängern und Erneuern

Mit PIM für Azure-Ressourcen werden neue Steuerelemente zum Verwalten des Zugriffs und des Zuweisungslebenszyklus für Azure-Ressourcen eingeführt. Administratoren können die Mitgliedschaft zuweisen, indem sie Eigenschaften für die Start- und Endzeit verwenden. Wenn das Ende der Zuweisung bevorsteht, sendet PIM E-Mail-Benachrichtigungen an die betroffenen Mitglieder (Benutzer oder Gruppe) und die Administratoren der Ressource, um sicherzustellen, dass der gewünschte Zugriff weiter möglich ist. Falls der Zugriff aufgrund einer Unterlassung nicht verlängert wird, können Zuweisungen auch erneuert werden. Sie bleiben im abgelaufenen Zustand noch 30 Tage lang sichtbar.

## <a name="who-can-extend-and-renew"></a>Wer kann das Verlängern und Erneuern durchführen?

Nur Administratoren der Ressource können Rollenzuweisungen verlängern oder erneuern. Das betroffene Mitglied kann anfordern, dass in Kürze ablaufende Rollen verlängert und dass bereits abgelaufene Rollen erneuert werden.

## <a name="when-are-notifications-sent"></a>Wann werden die Benachrichtigungen gesendet?

PIM sendet E-Mail-Benachrichtigungen an Administratoren und betroffene Mitglieder von Rollen, die innerhalb der nächsten 14 Tage ablaufen. Einen Tag vor dem Ablauf wird eine weitere Benachrichtigung gesendet. Wenn eine Zuweisung offiziell abgelaufen ist, wird ebenfalls eine E-Mail gesendet. 

Administratoren erhalten Benachrichtigungen, wenn ein Mitglied einer ablaufenden oder abgelaufenen Rolle die Verlängerung bzw. Erneuerung anfordert. Wenn ein bestimmter Administrator die Anforderung bearbeitet, werden alle anderen Administratoren über die getroffene Entscheidung (Genehmigung oder Ablehnung) informiert, und das anfordernde Mitglied erhält ebenfalls eine Benachrichtigung über die Entscheidung. 

## <a name="extend-role-assignments"></a>Verlängern von Rollenzuweisungen

Unten ist beschrieben, welche Schritte und Benutzeroberflächenelemente für das Anfordern, Bearbeiten bzw. Verwalten einer Verlängerung oder Erneuerung einer Rollenzuweisung erforderlich sind. 

### <a name="member-extend"></a>Verlängerung für Mitglied

Mitglieder einer Rollenzuweisung können die Verlängerung von ablaufenden Rollenzuweisungen direkt über die Registerkarte „Berechtigt“ oder „Aktiv“ auf der Seite „Meine Rollen“ einer Ressource und über die oberste Ebene von „Meine Rollen“ im PIM-Portal durchführen. Mitglieder können die Verlängerung von berechtigten und aktiven (zugewiesenen) Rollen anfordern, die innerhalb der nächsten 14 Tage ablaufen.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_ui.png)

Wenn der Endzeitpunkt innerhalb von 14 Tagen liegt, wird die Schaltfläche „Verlängern“ auf der Benutzeroberfläche zu einem aktiven Link. Im folgenden Beispiel wird angenommen, dass der 27. März das aktuelle Datum ist.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_within_14.png)

Klicken Sie auf „Verlängern“, um das Anforderungsformular zu öffnen und eine Verlängerung dieser Rollenzuweisung anzufordern.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_role_assignment_request.png)

Erweitern Sie die „Zuweisungsdetails“, um Informationen zur ursprünglichen Zuweisung anzuzeigen. Geben Sie einen Grund für die Verlängerungsanforderung an, und klicken Sie auf „Verlängern“.

>[!Note]
>Wir empfehlen Ihnen, die Details zum Grund der Verlängerung einzufügen und den Verlängerungszeitraum anzugeben (falls bekannt).

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_form_complete.png)

Die Ressourcenadministratoren erhalten nach kurzer Zeit eine E-Mail-Benachrichtigung mit der Aufforderung, die Verlängerungsanforderung zu überprüfen. Falls bereits eine Verlängerungsanforderung übermittelt wurde, wird oben im Azure-Portal eine Popupbenachrichtigung angezeigt, in der der Fehler erklärt wird.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_failed_existing_request.png)

Navigieren Sie oben links im Menü zur Registerkarte „Ausstehende Anforderungen“, um den Status der Anforderung anzuzeigen oder sie zu stornieren.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_cancel_request.png)

### <a name="admin-approve"></a>Genehmigung durch Administrator

Wenn ein Mitglied eine Anforderung zur Verlängerung einer Rollenzuweisung übermittelt, erhalten die Ressourcenadministratoren eine E-Mail-Benachrichtigung mit den Details der ursprünglichen Zuweisung und dem Grund, der von der anfordernden Person angegeben wurde. Die Benachrichtigung enthält einen direkten Link zu der Anforderung, damit der Administrator diese genehmigen oder ablehnen kann. 

Administratoren können Anforderungen nicht nur genehmigen oder ablehnen, indem sie den Link in der E-Mail verwenden, sondern hierfür auch im PIM-Verwaltungsportal im linken Navigationsmenü die Option „Anforderungen genehmigen“ wählen.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_admin_approve_grid.png)

Wenn ein Administrator die Option „Genehmigen“ oder „Ablehnen“ wählt, werden die Details der Anforderung zusammen mit einem Feld zum Angeben der Begründung für die Überwachungsprotokolle angezeigt.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_admin_approve_blade.png)

Beim Genehmigen einer Anforderung zur Verlängerung der Rollenzuweisung können Ressourcenadministratoren einen neuen Start- und Endzeitpunkt und einen neuen Zuweisungstyp wählen. Das Ändern des Zuweisungstyps kann erforderlich sein, wenn der Administrator begrenzten Zugriff gewähren möchte (z.B. nur für einen Tag), damit eine bestimmte Aufgabe erfüllt werden kann. In diesem Beispiel kann der Administrator die Zuweisung von „Berechtigt“ in „Aktiv“ ändern, sodass die anfordernde Person Zugriff erhält, ohne die Aktivierung durchführen zu müssen.

### <a name="admin-extend"></a>Verlängerung durch Administrator

Wenn ein Rollenmitglied vergisst, eine Verlängerung der Rollenmitgliedschaft anzufordern, oder diesen Vorgang nicht durchführen kann, kann ein Administrator eine Zuweisung auch im Namen des Mitglieds verlängern. Für Verlängerungen von Rollenmitgliedschaften durch Administratoren sind keine Genehmigungen erforderlich, aber nach Abschluss der Rollenverlängerung werden Benachrichtigungen an alle anderen Administratoren gesendet.

Navigieren Sie in PIM zur Ressourcenrolle oder zur Ansicht für ein Mitglied, um eine Rollenmitgliedschaft zu verlängern. Suchen Sie nach dem Mitglied, für das die Verlängerung durchgeführt werden soll, und klicken Sie in der Spalte mit den Aktionen auf „Verlängern“.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_admin_extend.png)

## <a name="renew-role-assignments"></a>Erneuern von Rollenzuweisungen

Der Prozess zum Erneuern einer abgelaufenen Rollenzuweisung ist vom Konzept her zwar ähnlich, aber es bestehen sowohl für Mitglieder als auch für Administratoren Unterschiede gegenüber dem Prozess für die Verlängerung. Mit den unten angegebenen Schritten können Mitglieder und Administratoren den Zugriff auf abgelaufene Rollen bei Bedarf erneuern.

### <a name="member-renew"></a>Erneuerung durch Mitglieder

Mitglieder, für die der Zugriff auf Ressourcen nicht mehr möglich ist, können bis zu 30 Tage lang auf den Verlauf der abgelaufenen Zuweisung zugreifen, indem sie in PIM im linken Navigationsbereich zu „Meine Rollen“ navigieren und im Abschnitt mit den Azure-Ressourcenrollen die Registerkarte „Abgelaufene Rollen“ wählen.

![](media/azure-pim-resource-rbac/aadpim_rbac_renew_from_myroles.png)

In der Liste mit den Rollen werden die Standardeinstellungen für berechtigte Zuweisungen angezeigt. Verwenden Sie die Dropdownliste, um zwischen berechtigten und aktiven zugewiesenen Rollen umzuschalten.

Wählen Sie die Aktion „Erneuern“, und geben Sie einen Grund für die Anforderung an, um für Rollenzuweisungen in der Liste eine Erneuerung anzufordern. Es ist hilfreich, zusätzlich zum weiteren Kontext einen Gültigkeitszeitraum anzugeben, um dem Ressourcenadministrator die Entscheidung über die Genehmigung oder Ablehnung zu erleichtern.

![](media/azure-pim-resource-rbac/aadpim_rbac_renew_request_form.png)

Beim Übermitteln der Anforderung werden die Ressourcenadministratoren über eine ausstehende Anforderung zur Erneuerung einer Rollenzuweisung informiert.

### <a name="admin-approves"></a>Genehmigung durch Administrator

Ressourcenadministratoren können über den Link in der E-Mail-Benachrichtigung auf die Erneuerungsanforderung zugreifen, oder sie können über das Azure-Portal auf PIM zugreifen und im linken Navigationsmenü die Option „Anforderungen genehmigen“ wählen.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_admin_approve_grid.png)

Wenn ein Administrator die Option „Genehmigen“ oder „Ablehnen“ wählt, werden die Details der Anforderung zusammen mit einem Feld zum Angeben der Begründung für die Überwachungsprotokolle angezeigt.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_admin_approve_blade.png)

Beim Genehmigen einer Anforderung zur Erneuerung der Rollenzuweisung müssen Ressourcenadministratoren einen neuen Start- und Endzeitpunkt und einen neuen Zuweisungstyp eingeben. 

### <a name="admin-renew"></a>Erneuerung durch Administrator

Ressourcenadministratoren können abgelaufene Rollenzuweisungen im linken Navigationsmenü einer Ressource über die Registerkarte „Mitglieder“ oder über die Registerkarte „Abgelaufen“ einer Ressourcenrolle erneuern.

Wählen Sie auf dem Bildschirm „Mitglieder“ die Option „Abgelaufene Rollen“, um eine Liste mit allen abgelaufenen Rollenzuweisungen anzuzeigen.

![](media/azure-pim-resource-rbac/aadpim_rbac_renew_from_member_blade.png)

## <a name="next-steps"></a>Nächste Schritte

[Erzwingen einer Genehmigung für die Aktivierung](pim-resource-roles-approval-workflow.md)

[Aktivieren einer Rolle](pim-resource-roles-use-the-audit-log.md)


