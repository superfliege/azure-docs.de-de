---
title: "Beheben von Lizenzproblemen für eine Gruppe in Azure Active Directory | Microsoft-Dokumentation"
description: Vorgehensweise zum Identifizieren und Beheben von Lizenzzuweisungsproblemen bei Verwendung der gruppenbasierten Lizenzierung mit Azure Active Directory
services: active-directory
keywords: "Azure AD-Lizenzierung"
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/05/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fa16cf14def7c6b4555d6624b25e267ef01d5adb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="identify-and-resolve-license-assignment-problems-for-a-group-in-azure-active-directory"></a>Identifizieren und Beheben von Lizenzzuweisungsproblemen für eine Gruppe in Azure Active Directory

Mit der gruppenbasierten Lizenzierung in Azure Active Directory (Azure AD) wird für Benutzer das Konzept eines Fehlerzustands für die Lizenzierung eingeführt. In diesem Artikel werden die Gründe dafür beschrieben, warum Benutzer sich in diesem Zustand befinden können.

Wenn Sie Lizenzen direkt einzelnen Benutzern zuweisen, ohne die gruppenbasierte Lizenzierung zu verwenden, tritt für den Zuweisungsvorgang ggf. ein Fehler auf. Wenn Sie beispielsweise das PowerShell-Cmdlet `Set-MsolUserLicense` auf einem Benutzersystem ausführen, kann für das Cmdlet aufgrund der Geschäftslogik aus verschiedensten Gründen ein Fehler auftreten. Beispielsweise kann die Anzahl von Lizenzen nicht ausreichen, oder es kann ein Konflikt zwischen zwei Dienstplänen bestehen, die nicht gleichzeitig zugewiesen werden können. Das Problem wird Ihnen sofort gemeldet.

Bei Verwendung der gruppenbasierten Lizenzierung können die gleichen Fehler auftreten. Diese treten allerdings im Hintergrund auf, während der Azure AD-Dienst Lizenzen zuweist. Daher können Ihnen die Fehler nicht sofort gemeldet werden. Stattdessen werden sie im Benutzerobjekt aufgezeichnet und anschließend über das Verwaltungsportal gemeldet. Das ursprüngliche Ziel (die Lizenzierung Benutzers) bleibt stets erhalten und wird im Falle eines Fehlers zur künftigen Untersuchung und Behebung aufgezeichnet.

## <a name="how-to-find-license-assignment-errors"></a>Vorgehensweise: Fehlersuche bei der Lizenzzuweisung
**So suchen Sie nach Fehlern bei der Lizenzzuweisung**

   1. Um in einer bestimmten Gruppe nach Benutzern mit einem Fehlerzustand zu suchen, öffnen Sie den Bereich für die Gruppe. Falls Benutzer mit einem Fehlerzustand vorhanden sind, wird unter **Lizenzen** eine Benachrichtigung angezeigt.

   ![Fehlerbenachrichtigung in einer Gruppe](media/active-directory-licensing-group-problem-resolution-azure-portal/group-error-notification.png)

   2. Wählen Sie die Benachrichtigung aus, um eine Liste mit allen betroffenen Benutzern zu öffnen. Sie können jeden Benutzer einzeln auswählen, um weitere Details anzuzeigen.

   ![Liste der Benutzer im Fehlerzustand in einer Gruppe](media/active-directory-licensing-group-problem-resolution-azure-portal/list-of-users-with-errors.png)

   3. Um nach allen Gruppen zu suchen, die mindestens einen Fehler enthalten, klicken Sie auf dem Blatt **Azure Active Directory** erst auf **Lizenzen** und dann auf **Übersicht**. Sollte für Gruppen eine Aktion erforderlich sein, wird ein Informationsfeld angezeigt.

   ![Übersicht: Information zu Gruppen im Fehlerzustand](media/active-directory-licensing-group-problem-resolution-azure-portal/group-errors-widget.png)

   4. Klicken Sie auf das Feld, um eine Liste aller Gruppen mit Fehlern anzuzeigen. Klicken Sie bei Bedarf auf die einzelnen Gruppen, um weitere Details anzuzeigen.

   ![Übersicht: Liste der Gruppen mit Fehlern](media/active-directory-licensing-group-problem-resolution-azure-portal/list-of-groups-with-errors.png)


In den folgenden Abschnitten werden die einzelnen potenziellen Probleme sowie die dazugehörigen Lösungsmöglichkeiten beschrieben.

## <a name="not-enough-licenses"></a>Nicht genügend Lizenzen

**Problem:** Es sind nicht genügend Lizenzen für eines der in der Gruppe angegebenen Produkte verfügbar. Sie müssen entweder weitere Lizenzen für das Produkt erwerben oder nicht verwendete Lizenzen anderer Benutzer oder Gruppen freigeben.

Um anzuzeigen, wie viele Lizenzen verfügbar sind, wechseln Sie zu **Azure Active Directory** > **Lizenzen** > **Alle Produkte**.

Klicken Sie auf ein Produkt, um zu ermitteln, welche Benutzer und Gruppen Lizenzen verwenden. Unter **Lizenzierte Benutzer** wird eine Liste mit allen Benutzern angezeigt, denen Lizenzen zugewiesen wurden (direkt oder über Gruppen). Unter **Lizenzierte Gruppen** sind alle Gruppen aufgeführt, denen Produkte zugewiesen sind.

**PowerShell:** PowerShell-Cmdlets melden diesen Fehler als _CountViolation_.

## <a name="conflicting-service-plans"></a>In Konflikt stehende Diensteeinstellungen

**Problem:** Eines der in der Gruppe angegebenen Produkte enthält einen Dienstplan, für den ein Konflikt mit einem anderen Dienstplan besteht, der dem Benutzer bereits über ein anderes Produkt zugewiesen ist. Einige Dienstpläne sind so konfiguriert, dass sie dem gleichen Benutzer nicht als anderer verwandter Dienstplan zugewiesen werden können.

Betrachten Sie das folgende Beispiel. Einem Benutzer ist eine Lizenz für Office 365 Enterprise *E1* direkt zugewiesen worden, wobei alle Pläne aktiviert wurden. Der Benutzer wurde einer Gruppe hinzugefügt, der das Produkt Office 365 Enterprise *E3* zugewiesen ist. Das E3-Produkt enthält Dienstpläne, die sich nicht mit den in E1 enthaltenen Plänen überschneiden dürfen. Daher tritt bei der Gruppenlizenzzuweisung der Fehler „Widersprüchliche Servicepläne“ auf. Bei diesem Beispiel stehen die folgenden Dienstpläne in Konflikt:

-   SharePoint Online (Plan 2) steht in Konflikt mit SharePoint Online (Plan 1).
-   Exchange Online (Plan 2) steht in Konflikt mit Exchange Online (Plan 1).

Zur Behebung dieses Konflikts müssen zwei der Pläne deaktiviert werden. Sie können die dem Benutzer direkt zugewiesene E1-Lizenz deaktivieren. Eine andere Möglichkeit besteht darin, die gesamte Gruppenlizenzzuweisung zu ändern und die Pläne für die E3-Lizenz zu deaktivieren. Alternativ können Sie dem Benutzer auch die E1-Lizenz entziehen, falls sie im Kontext der E3-Lizenz redundant ist.

Die Entscheidung, wie der Produktlizenzkonflikt zu lösen ist, liegt stets beim Administrator. Azure AD löst Lizenzkonflikte nicht automatisch.

**PowerShell:** PowerShell-Cmdlets melden diesen Fehler als _MutuallyExclusiveViolation_.

## <a name="other-products-depend-on-this-license"></a>Andere Produkte sind von dieser Lizenz abhängig

**Problem:** Eines der in der Gruppe angegebenen Produkte enthält einen Dienstplan, der für einen anderen Dienstplan (in einem anderen Produkt) aktiviert werden muss, um zu funktionieren. Dieser Fehler tritt auf, wenn Azure AD versucht, den zugrunde liegenden Dienstplan zu entfernen. Dieser Fall kann eintreten, wenn Sie den Benutzer aus der Gruppe entfernen.

Stellen Sie zur Behebung dieses Problems sicher, dass der erforderliche Plan Benutzern weiterhin über eine andere Methode zugewiesen ist oder dass die abhängigen Dienste für diese Benutzer deaktiviert sind. Anschließend können Sie die Gruppenlizenz für diese Benutzer richtig entfernen.

**PowerShell:** PowerShell-Cmdlets melden diesen Fehler als _DependencyViolation_.

## <a name="usage-location-isnt-allowed"></a>Verwendungsstandort ist nicht zulässig

**Problem:** Einige Microsoft-Dienste sind aufgrund der vor Ort geltenden Gesetze und Bestimmungen nicht an allen Standorten verfügbar. Um eine Lizenz einem Benutzer zuweisen können, müssen Sie zunächst die Eigenschaft **Usage location** (Verwendungsstandort) für den Benutzer angeben. Den Standort können Sie im Azure-Portal unter **Benutzer** > **Profil** > **Einstellungen** angeben.

Wenn Azure AD versucht, einem Benutzer, dessen Verwendungsstandort nicht unterstützt wird, eine Gruppenlizenz zuzuweisen, tritt ein Fehler auf, der für den Benutzer aufgezeichnet wird.

Sie können dieses Problem beheben, indem Sie Benutzer aus den nicht unterstützten Standorten der lizenzierten Gruppe entfernen. Alternativ können Sie wie folgt vorgehen: Wenn die aktuellen Werte des Verwendungsstandorts nicht den tatsächlichen Benutzerstandort darstellen, können Sie sie ändern, damit die Lizenzen beim nächsten Mal richtig zugewiesen werden (wenn der neue Standort unterstützt wird).

**PowerShell:** PowerShell-Cmdlets melden diesen Fehler als _ProhibitedInUsageLocationViolation_.

> [!NOTE]
> Wenn von Azure AD Gruppenlizenzen zugewiesen werden, erben alle Benutzer ohne Verwendungsstandort den Standort des Verzeichnisses. Es wird empfohlen, dass Administratoren die richtigen Werte für den Verwendungsstandort von Benutzern festlegen, bevor sie die gruppenbasierte Lizenzierung verwenden, um die vor Ort geltenden Gesetze und Bestimmungen zu erfüllen.

## <a name="what-happens-when-theres-more-than-one-product-license-on-a-group"></a>Was geschieht, wenn mehr als eine Produktlizenz für eine Gruppe vorhanden ist?

Sie können einer Gruppe mehr als eine Produktlizenz zuweisen. Beispielsweise können Sie Office 365 Enterprise E3 und Enterprise Mobility + Security einer Gruppe zuweisen, um alle enthaltenen Dienste den Benutzern mühelos zur Verfügung zu stellen.

Azure AD versucht, den einzelnen Benutzern alle in der Gruppe angegebenen Lizenzen zuzuweisen. Falls Azure AD aufgrund von Problemen mit der Geschäftslogik eines der Produkte nicht zuweisen kann, werden die anderen Lizenzen in der Gruppe auch nicht zugewiesen. Dieser Fall kann beispielsweise eintreten, wenn nicht genügen Lizenzen zur Verfügung stehen oder wenn Konflikte mit anderen für den Benutzer aktivierten Diensten bestehen.

Sie können die Benutzer anzeigen, für die die Zuweisung nicht erfolgreich war, und ermitteln, welche Produkte von diesem Problem betroffen sind.

## <a name="how-do-you-manage-licenses-for-products-with-prerequisites"></a>Wie werden Lizenzen für Produkte verwaltet, für die bestimmte Voraussetzungen gelten?

Bei einigen Ihrer Microsoft Online-Produkte handelt es sich unter Umständen um *Add-Ons*. Bei Add-Ons muss für einen Benutzer oder eine Gruppe ein Dienstplan mit Voraussetzungen aktiviert werden, bevor eine Lizenz zugewiesen werden kann. Bei der gruppenbasierten Lizenzierung müssen der Dienstplan mit den Voraussetzungen und der Add-On-Dienstplan in der gleichen Gruppe enthalten sein. Hierdurch wird sichergestellt, dass alle Benutzer, die der Gruppe hinzugefügt werden, das voll funktionsfähige Produkt erhalten können. Betrachten Sie das folgende Beispiel:

Microsoft Workplace Analytics ist ein Add-On-Produkt. Es enthält einen einzelnen Dienstplan mit demselben Namen. Dieser Dienstplan kann einem Benutzer (oder einer Gruppe) nur zugewiesen werden, wenn auch eine der folgenden Voraussetzungen zugewiesen ist:
- Exchange Online (Plan 1) 
- Exchange Online (Plan 2)

Wenn wir versuchen, dieses Produkt separat einer Gruppe zuzuweisen, gibt das Portal einen Fehler zurück. Nach dem Klicken auf die Fehlerbenachrichtigung werden folgende Details angezeigt:

![Gruppe mit fehlender Voraussetzung](media/active-directory-licensing-group-problem-resolution-azure-portal/group-prerequisite-required.png)

Nach dem Klicken auf die Details wird die folgende Fehlermeldung angezeigt:

>Fehler bei Lizenzvorgang. Stellen Sie sicher, dass die Gruppe die notwendigen Dienste besitzt, bevor Sie einen abhängigen Dienst hinzufügen oder entfernen. **Für den Dienst „Microsoft Workplace Analytics“ muss auch „Exchange Online (Plan 2)“ aktiviert werden.**

Um diese Add-On-Lizenz einer Gruppe zuzuweisen, müssen wir sicherstellen, dass die Gruppe auch den Dienstplan mit der Voraussetzung enthält. Wir können beispielsweise eine vorhandene Gruppe aktualisieren, die bereits das vollständige Produkt „Office 365 E3“ enthält, und das Add-On-Produkt anschließend hinzufügen.

Es ist auch möglich, eine eigenständige Gruppe zu erstellen, die nur die mindestens erforderlichen Produkte für die Funktionsfähigkeit des Add-Ons enthält. Sie kann verwendet werden, um nur die ausgewählten Benutzer für das Add-On-Produkt zu lizenzieren. In diesem Beispiel haben wir die folgenden Produkte der gleichen Gruppe zugewiesen:
- Office 365 Enterprise E3, wobei nur der Dienstplan „Exchange Online (Plan 2)“ aktiviert ist
- Microsoft Workplace Analytics

![Gruppe mit enthaltener Voraussetzung](media/active-directory-licensing-group-problem-resolution-azure-portal/group-addon-with-prerequisite.png)

Ab jetzt verwenden alle Benutzer, die dieser Gruppe hinzugefügt werden, eine Lizenz des E3-Produkts und eine Lizenz des Workplace Analytics-Produkts. Diese Benutzer können gleichzeitig Mitglieder einer anderen Gruppe sein, über die sie das vollständige E3-Produkt erhalten, und sie nutzen hierbei weiterhin nur eine Lizenz für das Produkt.

> [!TIP]
> Für jeden Dienstplan mit Voraussetzungen können mehrere Gruppen erstellt werden. Wenn Sie für Ihre Benutzer sowohl „Office 365 Enterprise E1“ als auch „Office 365 Enterprise E3“ verwenden, können Sie zwei Gruppen erstellen, um Microsoft Workplace Analytics zu lizenzieren: eine mit E1 als Voraussetzung und die andere mit E3. So können Sie das Add-On auf E1- und E3-Benutzer verteilen, ohne zusätzliche Lizenzen zu verbrauchen.

## <a name="license-assignment-fails-silently-for-a-user-due-to-duplicate-proxy-addresses-in-exchange-online"></a>Die Lizenzzuweisung zu einem Benutzer schlägt ohne Fehlermeldung fehl wegen doppelter Proxyadressen in Exchange Online

Bei Verwendung von Exchange Online sind unter Umständen einige Benutzer in Ihrem Mandanten fälschlicherweise mit dem gleichen Proxyadresswert konfiguriert. Wenn die gruppenbasierte Lizenzierung versucht, einem solchen Benutzer eine Lizenz zuweisen, tritt ein Fehler auf. Dieser Fehler wird nicht erfasst. Die Nichterfassung des Fehlers ist eine Einschränkung in der Vorschauversion dieses Features, die vor der *allgemeinen Verfügbarkeit* noch behoben wird.

> [!TIP]
> Wenn Sie feststellen, dass einige Benutzer keine Lizenz erhalten haben und für diese Benutzer kein Fehler erfasst wurde, überprüfen Sie zunächst, ob bei diesen Benutzern doppelte Proxyadressen vorhanden sind.
> Um zu ermitteln, ob eine doppelte Proxyadresse vorhanden ist, führen Sie das folgende PowerShell-Cmdlet für Exchange Online aus:
```
Run Get-Recipient | where {$_.EmailAddresses -match "user@contoso.onmicrosoft.com"} | fL Name, RecipientType,emailaddresses
```
> Weitere Informationen zu diesem Problem finden Sie unter ["Proxy address is already being used" error message in Exchange Online](https://support.microsoft.com/help/3042584/-proxy-address-address-is-already-being-used-error-message-in-exchange-online) (Fehlermeldung „Proxyadresse wird bereits verwendet“ in Exchange Online). Der Artikel enthält auch Informationen zum [Herstellen einer Verbindung mit Exchange Online mithilfe einer PowerShell-Remotesitzung](https://technet.microsoft.com/library/jj984289.aspx).

Nachdem Sie die Probleme mit den Proxyadressen für die betroffenen Benutzer behoben haben, erzwingen Sie die Lizenzverarbeitung für diese Gruppe, um sicherzustellen, dass die Lizenzen nun zugewiesen werden können.

## <a name="how-do-you-force-license-processing-in-a-group-to-resolve-errors"></a>Wie können Sie die Lizenzverarbeitung für eine Gruppe durchsetzen, um Fehler zu beheben?

Je nachdem, welche Maßnahmen Sie ergriffen haben, um die Fehler zu beheben, müssen Sie die Verarbeitung einer Gruppe ggf. manuell auslösen, um den Benutzerzustand zu aktualisieren.

Wenn Sie beispielsweise einige Lizenzen freigegeben haben, indem Sie direkte Zuweisungen von Lizenzen für Benutzer entfernt haben, müssen Sie die Verarbeitung von Gruppen auslösen, für die zuvor ein Fehler aufgetreten ist. Nur so erreichen Sie die vollständige Versorgung von Benutzern mit Lizenzen. Navigieren Sie zum erneuten Verarbeiten einer Gruppe zum Namen der Gruppe, öffnen Sie **Lizenzen**, und klicken Sie auf der Symbolleiste auf **Erneut verarbeiten**.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu anderen Szenarien für die Lizenzverwaltung über Gruppen finden Sie unter:

* [Zuweisen von Lizenzen zu einer Gruppe in Azure Active Directory](active-directory-licensing-group-assignment-azure-portal.md)
* [Was ist die gruppenbasierte Lizenzierung in Azure Active Directory?](active-directory-licensing-whatis-azure-portal.md)
* [Migrieren einzelner lizenzierter Benutzer zur gruppenbasierten Lizenzierung in Azure Active Directory](active-directory-licensing-group-migration-azure-portal.md)
* [Gruppenbasierte Azure Active Directory-Lizenzierung – zusätzliche Szenarien](active-directory-licensing-group-advanced.md)
