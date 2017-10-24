---
title: Aktivieren von Enterprise State Roaming in Azure Active Directory | Microsoft Docs
description: "Enthält häufig gestellte Fragen zu Enterprise State Roaming-Einstellungen auf Windows-Geräten. Beim Enterprise State Roaming profitieren Benutzer von einer einheitlichen Benutzeroberfläche auf allen Windows-Geräten und von einer Reduzierung der Zeit, die zum Konfigurieren eines neuen Geräts benötigt wird."
services: active-directory
keywords: Enterprise State Roaming, Windows-Cloud, Aktivieren von Enterprise State Roaming
documentationcenter: 
author: tanning
manager: femila
editor: curtand
ms.assetid: f71d66fd-7f9e-45eb-9cfe-5d989870f8a4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2017
ms.author: markvi
ms.openlocfilehash: 71212d11452d5f263b8621e1f0c13f9edd744618
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="enable-enterprise-state-roaming-in-azure-active-directory"></a>Aktivieren von Enterprise State Roaming in Azure Active Directory
Enterprise State Roaming ist für jedes Unternehmen mit einer Azure AD Premium- oder Enterprise Mobility + Security-Lizenz (EMS) verfügbar. Weitere Informationen zum Erhalt eines Azure AD-Abonnements finden Sie auf der [Azure AD-Produktseite](https://azure.microsoft.com/services/active-directory) .

Wenn Sie Enterprise State Roaming aktivieren, wird Ihrer Organisation automatisch eine kostenlose Lizenz für Azure Rights Management mit begrenzter Nutzung zugeteilt. Dieses kostenlose Abonnement ist auf das Verschlüsseln und Entschlüsseln von Enterpriseeinstellungen und durch Enterprise State Roaming synchronisierte Anwendungsdaten beschränkt. Sie benötigen [ein kostenpflichtiges Abonnement](https://azure.microsoft.com/pricing/details/active-directory/), um den vollständigen Funktionsumfang von Azure Rights Management zu verwenden.

## <a name="to-enable-enterprise-state-roaming"></a>So aktivieren Sie Enterprise State Roaming

1. Melden Sie sich beim [Azure AD Admin Center](https://aad.portal.azure.com/) an.

2. Wählen Sie **Azure Active Directory** &gt; **Geräte** &gt; **Geräteeinstellungen**.

3. Wählen Sie **Benutzer können Einstellungen und App-Daten geräteübergreifend synchronisieren**. Weitere Informationen finden Sie unter [Verwalten von Geräten im Azure-Portal – Vorschau](https://docs.microsoft.com/azure/active-directory/device-management-azure-portal).
  
  ![Bild der Geräteeinstellung „Benutzer können Einstellungen und App-Daten geräteübergreifend synchronisieren“](./media/active-directory-windows-enterprise-state-roaming-enable/device-settings.png)
  
Damit ein Windows 10-Gerät den Enterprise State Roaming-Dienst nutzen kann, muss das Gerät mit einer Azure AD-Identität authentifiziert werden. Für Geräte, die Azure AD beigetreten sind, ist die primäre Anmeldeidentität des Benutzers die Azure AD-Identität. Es ist also keine zusätzliche Konfiguration erforderlich. Für Geräte, die das lokale Active Directory verwenden, muss der IT-Administrator [in die Domäne eingebundene Geräte mit Azure AD verbinden, um Windows 10-Funktionen zu nutzen](active-directory-azureadjoin-devices-group-policy.md).

## <a name="data-storage"></a>Datenspeicher
Enterprise State Roaming-Daten werden in [Azure-Regionen](https://azure.microsoft.com/regions/) gehostet, die am besten zu dem Wert für Land/Region passen, der in der Azure Active Directory-Instanz festgelegt ist. Enterprise State Roaming-Daten werden basierend auf drei großen geografischen Regionen partitioniert: Nordamerika, EMEA und APAC. Enterprise State Roaming-Daten für Mandanten befinden sich lokal in der geografischen Region und werden nicht über Regionen hinweg repliziert.  Beispiel:
Wert für Land/Region | Daten gehostet in
---------------------|-------------------------
Ein EMEA-Land wie „Frankreich“ oder „Sambia“ | eine oder mehrere der Azure-Regionen in Europa 
Ein Land in Nordamerika wie „USA“ oder „Kanada“ | eine oder mehrere der Azure-Regionen innerhalb der USA
Ein APAC-Land wie „Australien“ oder „Neuseeland“ | eine oder mehrere der Azure-Regionen innerhalb Asiens
Regionen „Südamerika“ und „Antarktis“ | eine oder mehrere der Azure-Regionen innerhalb der USA

Der Wert für Land/Region wird im Rahmen der Azure AD-Verzeichniserstellung festgelegt und kann später nicht mehr geändert werden. Wenn Sie ausführliche Informationen zum Speicherort Ihrer Daten benötigen, können Sie beim [Azure-Support](https://azure.microsoft.com/support/options/) ein Ticket erstellen.

## <a name="view-per-user-device-sync-status"></a>Anzeigen des Gerätesynchronisierungsstatus pro Benutzer
Führen Sie diese Schritte aus, um einen Bericht zum Gerätesynchronisierungsstatus pro Benutzer anzuzeigen.

1. Melden Sie sich beim [Azure AD Admin Center](https://aad.portal.azure.com/) an.

2. Wählen Sie **Azure Active Directory**  &gt; **Benutzer und Gruppen** &gt; **Alle Benutzer** aus.

3. Wählen Sie den Benutzer und dann **Geräte** aus.

4. Wählen Sie unter **Anzeigen** die Option **Einstellungen für Gerätesynchronisierung und App-Daten**, um den Synchronisierungsstatus anzuzeigen.
  
  ![Bild der Gerätesynchronisierungsdaten-Einstellung](./media/active-directory-windows-enterprise-state-roaming-enable/sync-status.png)
  
5. Wenn für diese Benutzer Geräte synchronisiert werden, werden die Geräte hier angezeigt.
  
  ![Bild der Spaltendarstellung von Gerätesynchronisierungsdaten](./media/active-directory-windows-enterprise-state-roaming-enable/device-status-row.png)

## <a name="data-retention"></a>Beibehaltung von Daten
Daten, die mithilfe von Enterprise State Roaming mit Azure synchronisiert werden, werden beibehalten, bis sie manuell gelöscht oder als veraltet deklariert werden. 

### <a name="explicit-deletion"></a>Explizite Löschung
Bei expliziter Löschung löscht ein Azure-Administrator einen Benutzer oder ein Verzeichnis oder fordert auf andere Weise explizit die Löschung dieser Daten an.

* **Löschung eines Benutzers**: Wenn ein Benutzer in Azure AD gelöscht wird, werden die Roamingdaten des Benutzerkontos nach 90 bis 180 Tagen gelöscht. 
* **Löschung eines Verzeichnisses:**Das Löschen eines gesamten Verzeichnisses in Azure AD ist ein unmittelbarer Vorgang. Alle diesem Verzeichnis zugeordneten Einstellungsdaten werden nach 90 bis 180 Tagen gelöscht. 
* **Löschung auf Anforderung:**Wenn der Azure AD-Administrator die Einstellungen oder Daten eines bestimmten Benutzers manuell löschen möchte, kann er beim [Azure-Support](https://azure.microsoft.com/support/)ein Ticket erstellen. 

### <a name="stale-data-deletion"></a>Löschung veralteter Daten
Daten, auf die ein Jahr lang nicht zugegriffen wurde (Aufbewahrungsdauer), werden als veraltet eingestuft und können aus Azure gelöscht werden. Die Aufbewahrungsdauer kann sich ändern, wird jedoch nicht weniger als 90 Tage betragen. Bei den veralteten Daten kann es sich um eine bestimmte Gruppe von Windows-/Anwendungseinstellungen oder um alle Einstellungen eines Benutzers handeln. Beispiel:

* Wenn keine Geräte auf eine bestimmte Sammlung mit Einstellungen zugreifen (z.B. wird eine Anwendung vom Gerät entfernt, oder eine Einstellungsgruppe wie „Design“ wird für alle Geräte eines Benutzers deaktiviert), gilt diese Sammlung nach Ablauf der Aufbewahrungsdauer als veraltet und kann gelöscht werden. 
* Falls ein Benutzer die Einstellungssynchronisierung auf allen eigenen Geräten deaktiviert hat, wird nicht auf die Einstellungsdaten zugegriffen. Alle Einstellungsdaten für diesen Benutzer werden als veraltet eingestuft und können nach Ablauf der Aufbewahrungsdauer gelöscht werden. 
* Wenn der Azure AD-Verzeichnisadministrator Enterprise State Roaming für das gesamte Verzeichnis deaktiviert hat, wird die Synchronisierung der Einstellungen für alle Benutzer im jeweiligen Verzeichnis beendet. Alle Einstellungsdaten aller Benutzer gelten dann als veraltet und können nach Ablauf der Aufbewahrungsdauer gelöscht werden. 

### <a name="deleted-data-recovery"></a>Wiederherstellen von gelöschten Daten
Die Richtlinie für die Datenaufbewahrung ist nicht konfigurierbar. Sobald die Daten dauerhaft gelöscht wurden, sind sie nicht wiederherstellbar. Allerdings werden die Einstellungsdaten nur aus Azure gelöscht, nicht vom Endbenutzergerät. Falls mit einem Gerät zu einem späteren Zeitpunkt wieder eine Verbindung mit dem Enterprise State Roaming-Dienst hergestellt wird, werden die Einstellungen wieder synchronisiert und in Azure gespeichert.

## <a name="related-topics"></a>Verwandte Themen
* [Übersicht über Enterprise State Roaming](active-directory-windows-enterprise-state-roaming-overview.md)
* [Roaming von Einstellungen und Daten – Häufig gestellte Fragen](active-directory-windows-enterprise-state-roaming-faqs.md)
* [Gruppenrichtlinien- und MDM-Einstellungen für Einstellungssynchronisierung](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
* [Windows 10-Roamingeinstellungen – Referenz](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
* [Problembehandlung](active-directory-windows-enterprise-state-roaming-troubleshooting.md)
