---
title: "Organisieren Ihrer Ressourcen mit Azure-Verwaltungsgruppen – Azure | Microsoft-Dokumentation"
description: Informationen zu Verwaltungsgruppen und deren Verwendung.
author: rthorn17
manager: rithorn
editor: 
ms.assetid: 482191ac-147e-4eb6-9655-c40c13846672
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/25/2017
ms.author: rithorn
ms.openlocfilehash: 18541c68b02ae1b59ae4a6a85122dff614c9978c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="organize-your-resources-with-azure-management-groups"></a>Organisieren Ihrer Ressourcen mit Azure-Verwaltungsgruppen 

Wenn Sie über mehrere Abonnements verfügen, können Sie diese in Containern organisieren, die als „Verwaltungsgruppen“ bezeichnet werden, um die abonnementübergreifende Verwaltung von Zugriff, Richtlinien, Kosten und Konformität zu vereinfachen. Beispielsweise können Sie Richtlinien auf eine Verwaltungsgruppe anwenden, die die Anzahl der Ressourcentypen begrenzt, die erstellt werden können.

> [!Note]
> Dieses Feature ist zurzeit als private Vorschau verfügbar. [Melden Sie sich hier](https://aka.ms/MGPreviewSignup) für die Vorschau an.   
 


Verwaltungsgruppen können in einer Hierarchie organisiert werden. Die gezeigte Struktur ist die Beispieldarstellung einer möglichen Verwaltungsgruppenhierarchie:


![Hierarchiestruktur](media/billing-enterprise-mgmt-groups/tree.png)



## <a name="how-management-groups-are-related-to-your-azure-enterprise-enrollment"></a>Der Zusammenhang zwischen Verwaltungsgruppen und Ihrer Azure Enterprise-Registrierung

Die Einführung von Verwaltungsgruppen ist ein Schritt im größeren Zusammenhang des Übergangs von Features des [Enterprise Portals](https://ea.azure.com) zum [Azure-Portal](https://portal.azure.com).

Die Verwaltungsgruppenstruktur wird erstellt, wie sie im Enterprise Portal definiert wurde. Die gesamte aus Registrierung, Abteilungen und Konten bestehende Hierarchie wird entsprechenden Verwaltungsgruppen zugeordnet. 

Hier sehen Sie, wie die aktuelle EA-Struktur der Verwaltungsgruppenhierarchie zugeordnet wird. 

![Hierarchiestruktur](media/billing-enterprise-mgmt-groups/tree2.png)

Die folgende Tabelle zeigt, wie die Benutzer aus dem Enterprise Portal der Verwaltungsgruppenhierarchie zugeordnet werden.

|    EA-Rolle                                       |    Rolle auf dem zugeordneten Verwaltungsgruppenknoten    |    Berechtigungen auf dem Verwaltungsgruppenknoten                                                          |
|--------------------------------------------------|--------------------------------------------------|----------------------------------------------------------------------------------------------------|
|    EA-Administrator                              |    Ressourcenrichtlinienmitwirkender                   |    Kann auf dem Registrierungsknoten und unterhalb Kosten anzeigen, Ressourcenrichtlinien verwalten und Hierarchien anzeigen    |
|    EA-Administrator im schreibgeschützten Modus            |    Abrechnungsleser                                |    Kann auf dem Registrierungsknoten und unterhalb Kosten und Hierarchien anzeigen                              |
|    Abteilungsadministrator                      |    Abrechnungsleser                                |    Kann auf dem Abteilungsknoten und unterhalb Kosten und Hierarchien anzeigen                                 |
|    Abteilungsadministrator im schreibgeschützten Modus    |    Abrechnungsleser                                |    Kann auf dem Abteilungsknoten und unterhalb Kosten und Hierarchien anzeigen                                 |
|    Kontobesitzer                                 |    Ressourcenrichtlinienmitwirkender                   |    Kann auf dem Kontoknoten und unterhalb Kosten anzeigen, Ressourcenrichtlinien verwalten und Hierarchien anzeigen       |




## <a name="view-management-groups-in-the-azure-portal"></a>Anzeigen von Verwaltungsgruppen im Azure-Portal

Um eine Registrierung, Abteilung oder ein Konto in der Vorschau anzuzeigen, melden Sie sich mit dem Link in der Begrüßungs-E-Mail beim Azure-Portal an.   

![Hierarchie](media/billing-enterprise-mgmt-groups/hierarchy.png)

### <a name="viewing-costs"></a>Anzeigen der Kosten 
Auf dem Detailbildschirm der Verwaltungsgruppen sehen Sie die aktuellen Monat-bis-Datum-Kosten. Diese Kosten basieren auf der Nutzung. Bereits gezahlte Beträge, Überschreitungen, enthaltene Mengen, Anpassungen und Steuern werden nicht berücksichtigt. Für die Verwaltungsgruppe, die Ihrer Registrierung entspricht, zeigt der Kostenabschnitt die verbleibende Verpflichtung an.  

![Registrierungsdetails](media/billing-enterprise-mgmt-groups/enrollment.png)

 „Getrennt abgerechnete Kosten“ ist die monatliche Ansammlung von separaten Änderungen wie Marketplace, Überschreitungen und sonstigen Kosten, die nicht auf die Verpflichtung Ihrer Registrierung entfallen.  Weitere Informationen zur Kostenaufschlüsselung finden Sie im [Enterprise Portal](https://ea.azure.com). 

### <a name="enabling-access-to-costs"></a>Aktivieren des Zugriffs auf Kosten
Wenn die Kosten nicht angezeigt werden, finden Sie Hilfe im Dokument [Troubleshoot enterprise cost views](https://aka.ms/enableazurecosts) (Problembehandlung bei Enterprise-Kostenansichten).  

### <a name="delays-between-the-enterprise-portal-and-azure-portal"></a>Verzögerungen zwischen Enterprise Portal und Azure-Portal 
* Während der Vorschau werden die im Azure-Portal angezeigten Beträge im Vergleich zu den Werten im Enterprise Portal möglicherweise zeitverzögert angezeigt. Dieses Problem ist temporär und wird gerade gelöst.
* Bei aktualisierten Einstellungen im Enterprise Portal tritt eine Verzögerung von ein paar Minuten auf, bis die Updates im Azure-Portal sichtbar sind. 

## <a name="management-groups-have-a-relationship-with-your-directory"></a>Verwaltungsgruppen stehen mit Ihrem Verzeichnis in Beziehung   
Wie Abonnements verfügen Verwaltungsgruppen auch über eine Vertrauensstellung mit Azure AD. Eine Verwaltungsgruppenhierarchie vertraut einem einzelnen Verzeichnis, um Benutzer zu authentifizieren. Alle Administratoren, die einer Verwaltungsgruppenhierarchie zugeordnet sind, müssen zu demselben Verzeichnis gehören. 

Da Ihre Registrierungshierarchie Verwaltungsgruppen zugeordnet ist, wird eine Vertrauensstellung mit einem einzelnen Verzeichnis eingerichtet. Wenn möglich, wird ein vorhandenes Verzeichnis ausgewählt, das den Benutzerkonten der Registrierung zugeordnet ist. In einigen Fällen wird ein neues Verzeichnis erstellt, und alle vorhandenen Registrierungsbenutzer werden in dieses Verzeichnis eingeladen. Den Abonnements der Registrierung zugeordnete Verzeichnisse sind nicht betroffen. Aus diesem Grund könnte die Hierarchie in einem Verzeichnis erstellt werden, das sich von den Abonnements unterscheidet. [Erfahren Sie mehr darüber](billing-enterprise-mgmt-grp-find.md), wie dieser Vorgang die Navigation zwischen der Hierarchie und den zugehörigen Abonnements beeinflusst.

## <a name="administering-your-management-groups"></a>Verwalten Ihrer Verwaltungsgruppen
Verwaltungsgruppen im Azure-Portal sind in der Vorschau und bei diesem ersten Release schreibgeschützt. Um Updates vorzunehmen, wechseln Sie zum Enterprise Portal. Ihre Updates werden automatisch im Azure-Portal wiedergegeben. In der Dokumentation des Enterprise Portals finden Sie Hilfe zum Erstellen von Änderungen und Ergänzungen.   

## <a name="policy-management"></a>Richtlinienverwaltung
Mit dem Ressourcen-Manager können Sie benutzerdefinierte Richtlinien zum Verwalten Ihrer Ressourcen erstellen. Mit Verwaltungsgruppen können Richtlinien auf jeder Ebene der Hierarchie zugewiesen werden und die Ressourcen diese Richtlinien erben.  [Weitere Informationen](https://go.microsoft.com/fwlink/?linkid=858942)

> [!Note]
> Die Richtlinie wird nicht Verzeichnisse übergreifend erzwungen. 


