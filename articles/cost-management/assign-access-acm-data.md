---
title: Zuweisen des Zugriffs auf Daten in Azure Cost Management | Microsoft-Dokumentation
description: Dieser Artikel führt Sie durch das Zuweisen von Berechtigungen für Daten in Azure Cost Management für verschiedene Zugriffsbereiche.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 01/04/2019
ms.topic: conceptual
ms.service: cost-management
manager: vitavor
ms.custom: secdec18
ms.openlocfilehash: 50f048dc36b0248cdbbd85d91c00b7947f9ddc1f
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2019
ms.locfileid: "54052137"
---
# <a name="assign-access-to-cost-management-data"></a>Zuweisen des Zugriffs auf Daten in Cost Management

Für die meisten Benutzer wird die Zugriffsebene auf Daten in Azure Cost Management durch eine Kombination aus im Azure-Portal und im Enterprise-Portal (EA) gewährten Berechtigungen definiert. Dieser Artikel führt Sie durch das Zuweisen des Zugriffs auf Daten in Cost Management. Nachdem die Kombination von Berechtigungen zugewiesen wurde, kann der Benutzer anhand des Bereichs, auf den er Zugriff hat, und des im Azure-Portal ausgewählten Bereichs auf Daten zugreifen.

Der von einem Benutzer ausgewählte Bereich wird im gesamten Cost Management verwendet, um Daten zu konsolidieren und den Zugriff auf Kosteninformationen zu steuern. Bei Verwendung von Bereichen findet keine Mehrfachauswahl durch Benutzer statt. Stattdessen wählen sie einen größeren Bereich aus, auf den untergeordnete Bereiche erweitert werden, und filtern diese dann nach den gewünschten Elementen für die Anzeige. Diese Datenkonsolidierung sollten Sie verstehen, da einige Personen keinen Zugriff auf einen übergeordneten Bereich haben, in den die untergeordneten Bereiche zusammengefasst werden.

## <a name="cost-management-scopes"></a>Bereiche in Cost Management

Zum Anzeigen von Kostendaten benötigen Sie für einen oder mehrere der folgenden Bereiche mindestens Lesezugriff.

| **Umfang** | **Definiert unter** | **Erforderlicher Zugriff zum Anzeigen von Daten** | **Erforderliche EA-Einstellung** | **Konsolidierung von Daten** |
| --- | --- | --- | --- | --- |
| Abrechnungskonto<sup>1</sup> | [https://ea.azure.com](https://ea.azure.com/) | Unternehmensadministrator | Keine | Alle Abonnements aus dem Enterprise Agreement |
| Department | [https://ea.azure.com](https://ea.azure.com/) | Abteilungsadministrator | **Gebühren anzeigen** für Abteilungsadministratoren aktiviert | Alle Abonnements, die zu einem Registrierungskonto gehören, das mit der Abteilung verknüpft ist |
| Registrierungskonto<sup>2</sup> | [https://ea.azure.com](https://ea.azure.com/) | Kontobesitzer | **AO view charges** (Gebühren anzeigen für Kontobesitzer) aktiviert | Alle Abonnements aus dem Registrierungskonto |
| Verwaltungsgruppe | [https://portal.azure.com](https://portal.azure.com/) | Cost Management-Leser (oder Leser) | **AO view charges** (Gebühren anzeigen für Kontobesitzer) aktiviert | Alle Abonnements unter der Verwaltungsgruppe |
| Abonnement | [https://portal.azure.com](https://portal.azure.com/) | Cost Management-Leser (oder Leser) | **AO view charges** (Gebühren anzeigen für Kontobesitzer) aktiviert | Alle Ressourcen/Ressourcengruppen im Abonnement |
| Ressourcengruppe | [https://portal.azure.com](https://portal.azure.com/) | Cost Management-Leser (oder Leser) | **AO view charges** (Gebühren anzeigen für Kontobesitzer) aktiviert | Alle Ressourcen in der Ressourcengruppe |

<sup>1</sup> Das Abrechnungskonto wird auch als Enterprise Agreement oder Registrierung bezeichnet.

<sup>2</sup> Das Registrierungskonto wird auch als Kontobesitzer bezeichnet.

Das folgende Diagramm veranschaulicht die Beziehung zwischen Cost Management-Bereichen mit Rollen und EA-Portal-Einstellungen.

![Das Diagramm zeigt die Beziehung zwischen Cost Management-Bereichen mit Rollen und EA-Portal-Einstellungen.](./media/assign-access-acm-data/scope-access-relationship-diagram.png)

## <a name="enable-access-to-costs-in-the-ea-portal"></a>Aktivieren des Zugriffs auf Kosten im EA-Portal

Für den Abteilungsbereich muss die Option **Abteilungsadministratoren können Gebühren anzeigen** im EA-Portal **Aktiviert** sein. Für alle anderen Bereiche muss die Option **AO view charges** (Gebühren anzeigen für Kontobesitzer) im EA-Portal **aktiviert** sein.

So aktivieren Sie eine Option

1. Melden Sie sich beim EA-Portal unter [https://ea.azure.com](https://ea.azure.com) mit einem Enterprise-Administratorkonto an.
2. Wählen Sie im linken Bereich **Verwalten** aus.
3. Aktivieren Sie für die Kostenverwaltungsbereiche, auf die Sie Zugriff gewähren möchten, nach Bedarf die Optionen **DA view charges** (Gebühren anzeigen für Abteilungsadministrator) oder **AO view charges** (Gebühren anzeigen für Kontobesitzer).  
    ![Registerkarte „Registrierung“ mit Optionen zum Anzeigen der Gebühren für Abteilungsadministratoren und Kontobesitzer](./media/assign-access-acm-data/ea-portal-enrollment-tab.png)

Nachdem die Optionen zum Anzeigen von Gebühren aktiviert wurden, müssen für die meisten Bereiche auch Berechtigungen für die rollenbasierte Zugriffssteuerung (RBAC) im Azure-Portal konfiguriert werden.

## <a name="enterprise-administrator-role"></a>Unternehmensadministratorrolle

Standardmäßig hat ein Unternehmensadministrator Zugriff auf das Abrechnungskonto (Enterprise Agreement/Registrierung) und alle anderen untergeordneten Bereiche. Der Unternehmensadministrator weist anderen Benutzern den Zugriff auf Bereiche zu. Als bewährte Methode für Geschäftskontinuität sollten immer zwei Benutzer den Zugriff eines Unternehmensadministrators haben. Die folgenden Abschnitte sind detaillierte Beispiele für die Zuweisung des Zugriffs auf Bereiche für andere Benutzer durch den Unternehmensadministrator.

## <a name="assign-billing-account-scope-access"></a>Zuweisen des Zugriffs auf den Abrechnungskontenbereich

Für den Zugriff auf den Abrechnungskontenbereich muss im EA-Portal die Berechtigung eines Unternehmensadministrators vorliegen. Der Unternehmensadministrator hat Zugriff auf die Anzeige von Kosten in der gesamten EA-Registrierung oder in mehreren Registrierungen. Es ist keine Aktion im Azure-Portal für den Abrechnungskontenbereich erforderlich.

1. Melden Sie sich beim EA-Portal unter [https://ea.azure.com](https://ea.azure.com) mit einem Enterprise-Administratorkonto an.
2. Wählen Sie im linken Bereich **Verwalten** aus.
3. Wählen Sie auf der Registerkarte **Registrierung** die Registrierung aus, die Sie verwalten möchten.  
    ![Auswählen Ihrer Registrierung im EA-Portal](./media/assign-access-acm-data/ea-portal.png)
4. Klicken Sie auf **+ Administrator hinzufügen**.
5. Wählen Sie im Feld „Administrator hinzufügen“ den Authentifizierungstyp aus, und geben Sie die E-Mail-Adresse des Benutzers ein.
6. Wenn der Benutzer schreibgeschützten Zugriff auf Kosten- und Nutzungsdaten erhalten soll, wählen Sie unter **Schreibgeschützt** die Option **Ja** aus.  Klicken Sie andernfalls auf **Nein**.
7. Klicken Sie auf **Hinzufügen**, um das Konto zu erstellen.  
    ![Beispielinformationen im Feld „Administrator hinzufügen“](./media/assign-access-acm-data/add-admin.png)

Es kann bis zu 30 Minuten dauern, bevor der neue Benutzer in Cost Management auf Daten zugreifen kann.

### <a name="assign-department-scope-access"></a>Zuweisen des Zugriffs auf den Abteilungsbereich

Für den Zugriff auf den Abteilungsbereich muss im EA-Portal der Zugriff eines Abteilungsadministrators („DA view charges“) gewährt werden. Der Abteilungsadministrator hat Zugriff auf die Anzeige der Kosten- und Nutzungsdaten für eine oder mehrere Abteilungen. Die Daten für die Abteilung schließen alle Abonnements ein, die zu einem Registrierungskonto gehören und mit der Abteilung verknüpft sind. Im Azure-Portal ist keine Aktion erforderlich.

1. Melden Sie sich beim EA-Portal unter [https://ea.azure.com](https://ea.azure.com) mit einem Enterprise-Administratorkonto an.
2. Wählen Sie im linken Bereich **Verwalten** aus.
3. Wählen Sie auf der Registerkarte **Registrierung** die Registrierung aus, die Sie verwalten möchten.
4. Klicken Sie auf die Registerkarte **Abteilung**, und klicken Sie dann auf **Administrator hinzufügen**.
5. Wählen Sie im Feld „Add Department Administrator“ (Abteilungsadministrator hinzufügen) den Authentifizierungstyp aus, und geben Sie die E-Mail-Adresse des Benutzers ein.
6. Wenn der Benutzer schreibgeschützten Zugriff auf Kosten- und Nutzungsdaten erhalten soll, wählen Sie unter **Schreibgeschützt** die Option **Ja** aus.  Klicken Sie andernfalls auf **Nein**.
7. Wählen Sie die Abteilungen aus, denen Sie Berechtigungen für die Abteilungsverwaltung gewähren möchten.
8. Klicken Sie auf **Hinzufügen**, um das Konto zu erstellen.  
    ![Eingeben der erforderlichen Informationen in das Feld „Abteilungsadministrator hinzufügen“](./media/assign-access-acm-data/add-depart-admin.png)

## <a name="assign-enrollment-account-scope-access"></a>Zuweisen des Zugriffs auf den Registrierungskontobereich

Für den Zugriff auf den Registrierungskontobereich muss im EA-Portal der Zugriff eines Kontobesitzers („AO view charges“) bestehen. Der Kontobesitzer kann Kosten- und Nutzungsdaten anzeigen, die mit den Abonnements verknüpft sind, die von einem Registrierungskonto erstellt wurden. Im Azure-Portal ist keine Aktion erforderlich.

1. Melden Sie sich beim EA-Portal unter [https://ea.azure.com](https://ea.azure.com) mit einem Enterprise-Administratorkonto an.
2. Wählen Sie im linken Bereich **Verwalten** aus.
3. Wählen Sie auf der Registerkarte **Registrierung** die Registrierung aus, die Sie verwalten möchten.
4. Klicken Sie auf die Registerkarte **Konto** und dann auf **Konto hinzufügen**.
5. Wählen Sie im Feld „Konto hinzufügen“ die **Abteilung** aus, der das Konto zugeordnet werden soll, oder lassen sie es nicht zugewiesen.
6. Wählen Sie den Authentifizierungstyp aus, und geben Sie den Kontonamen ein.
7. Geben Sie die E-Mail-Adresse des Benutzers und dann optional die Kostenstelle ein.
8. Klicken Sie auf **Hinzufügen**, um das Konto zu erstellen.  
    ![Eingeben der erforderlichen Informationen in das Feld „Konto hinzufügen“ für ein Registrierungskonto](./media/assign-access-acm-data/add-account.png)

Nachdem die obigen Schritte ausgeführt wurden, wird das Benutzerkonto zu einem Registrierungskonto im Enterprise Portal. Fortan können damit Abonnements erstellt werden. Der Benutzer kann auf Kosten- und Nutzungsdaten für von ihm erstellte Abonnements zugreifen.

## <a name="assign-management-group-scope-access"></a>Zuweisen des Zugriffs auf den Verwaltungsgruppenbereich

Für den Zugriff auf den Verwaltungsgruppenbereich ist mindestens die Berechtigung „Kostenverwaltung: Leser“ (oder „Leser“) erforderlich. Sie können die Berechtigungen für eine Verwaltungsgruppe im Azure-Portal konfigurieren. Sie benötigen mindestens die Berechtigung „Benutzerzugriffsadministrator“ (oder „Besitzer“) für die Verwaltungsgruppe, um anderen Benutzern Zugriff zu gewähren. Außerdem muss die Einstellung **AO view charges** (Gebühren anzeigen für Kontobesitzer) im EA-Portal aktiviert sein.

1. Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.
2. Wählen Sie auf der Seitenleiste **Alle Dienste** aus, suchen Sie _Verwaltungsgruppen_, und wählen Sie dann **Verwaltungsgruppen** aus.
3. Wählen Sie die Verwaltungsgruppe in der Hierarchie aus.
4. Klicken Sie neben dem Namen Ihrer Verwaltungsgruppe auf **Details**.
5. Wählen Sie im linken Bereich **Zugriffssteuerung (IAM)** aus.
6. Klicken Sie auf **Hinzufügen**.
7. Wählen Sie unter **Rolle** die Option **Cost Management-Leser** aus.
8. Wählen Sie unter **Zugriff zuweisen zu** die Option **Azure AD-Benutzer, -Gruppe oder -Anwendung** aus.
9. Um den Zugriff zuzuweisen, suchen Sie den Benutzer, und wählen Sie ihn aus.
10. Klicken Sie auf **Speichern**.  
    ![Beispielinformationen im Feld „Berechtigungen hinzufügen“ für eine Verwaltungsgruppe](./media/assign-access-acm-data/add-permissions.png)

## <a name="assign-subscription-scope-access"></a>Zuweisen des Zugriffs auf den Abonnementbereich

Der Zugriff auf ein Abonnement erfordert mindestens die Berechtigung „Kostenverwaltung: Leser“ (oder „Leser“). Sie können die Berechtigungen für ein Abonnement im Azure-Portal konfigurieren. Sie benötigen mindestens die Berechtigung „Benutzerzugriffsadministrator“ (oder „Besitzer“) für das Abonnement, um anderen Benutzern Zugriff zu gewähren. Außerdem muss die Einstellung **AO view charges** (Gebühren anzeigen für Kontobesitzer) im EA-Portal aktiviert sein.

1. Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.
2. Wählen Sie auf der Seitenleiste **Alle Dienste** aus, suchen Sie _Abonnements_, und wählen Sie dann **Abonnements** aus.
3. Wählen Sie Ihr Abonnement aus.
4. Wählen Sie im linken Bereich **Zugriffssteuerung (IAM)** aus.
5. Klicken Sie auf **Hinzufügen**.
6. Wählen Sie unter **Rolle** die Option **Cost Management-Leser** aus.
7. Wählen Sie unter **Zugriff zuweisen zu** die Option **Azure AD-Benutzer, -Gruppe oder -Anwendung** aus.
8. Um den Zugriff zuzuweisen, suchen Sie den Benutzer, und wählen Sie ihn aus.
9. Klicken Sie auf **Speichern**.

## <a name="assign-resource-group-scope-access"></a>Zuweisen des Zugriffs auf den Ressourcengruppenbereich

Für den Zugriff auf den Ressourcengruppenbereich ist mindestens die Berechtigung „Kostenverwaltung: Leser“ (oder „Leser“) erforderlich. Sie können die Berechtigungen für eine Ressourcengruppe im Azure-Portal konfigurieren. Sie benötigen mindestens die Berechtigung „Benutzerzugriffsadministrator“ (oder „Besitzer“) für die Ressourcengruppe, um anderen Benutzern Zugriff zu gewähren. Außerdem muss die Einstellung **AO view charges** (Gebühren anzeigen für Kontobesitzer) im EA-Portal aktiviert sein.

1. Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.
2. Wählen Sie auf der Seitenleiste **Alle Dienste** aus, suchen Sie _Ressourcengruppen_, und wählen Sie dann **Ressourcengruppen** aus.
3. Wählen Sie Ihre Ressourcengruppe aus.
4. Wählen Sie im linken Bereich **Zugriffssteuerung (IAM)** aus.
5. Klicken Sie auf **Hinzufügen**.
6. Wählen Sie unter **Rolle** die Option **Cost Management-Leser** aus.
7. Wählen Sie unter **Zugriff zuweisen zu** die Option **Azure AD-Benutzer, -Gruppe oder -Anwendung** aus.
8. Um den Zugriff zuzuweisen, suchen Sie den Benutzer, und wählen Sie ihn aus.
9. Klicken Sie auf **Speichern**.

## <a name="next-steps"></a>Nächste Schritte

- Falls Sie den ersten Schnellstart für Cost Management noch nicht abgeschlossen haben, lesen Sie ihn unter [Erste Schritte in der Analyse von Kosten](quick-acm-cost-analysis.md).
