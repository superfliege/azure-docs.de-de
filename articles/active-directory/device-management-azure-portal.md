---
title: "Verwalten von Geräten mit dem Azure-Portal | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie Geräte über das Azure-Portal verwalten."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: cd13ecf50cf03c362f8da2da83ab698c4e8ba0cc
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2017
---
# <a name="managing-devices-using-the-azure-portal"></a>Verwalten von Geräten mit dem Azure-Portal


Mit der Geräteverwaltung in Azure Active Directory (Azure AD) können Sie sicherstellen, dass Benutzer auf Ihre Ressourcen über Geräte zugreifen, die Ihren Standards für Sicherheit und Konformität entsprechen. 

Dieses Thema:

- Setzt voraus, dass Sie die [Einführung in die Geräteverwaltung in Azure Active Directory](device-management-introduction.md) gelesen haben.

- Enthält Informationen zum Verwalten Ihrer Geräte mithilfe des Azure-Portals.



## <a name="configure-device-settings"></a>Konfigurieren der Geräteeinstellungen

Damit Ihre Geräte im Azure-Portal verwaltet werden können, müssen diese in Azure AD [registriert oder eingebunden](device-management-introduction.md#getting-devices-under-the-control-of-azure-ad) (d.h. mit Azure AD verknüpft) werden. Als Administrator können Sie den Prozess der Registrierung und Verknüpfung von Geräten optimieren, indem Sie die Geräteeinstellungen konfigurieren. 

![Konfigurieren der Geräteeinstellungen](./media/device-management-azure-portal/22.png)

Auf der Seite mit den Geräteeinstellungen können Sie Folgendes konfigurieren:

![Verwalten eines Intune-Geräts](./media/device-management-azure-portal/21.png)


- **Benutzer dürfen Geräte in Azure AD einbinden**: Diese Einstellung ermöglicht Ihnen die Auswahl der Benutzer, die [Geräte in Azure AD einbinden](device-management-introduction.md#azure-ad-joined-devices) können. Die Standardeinstellung ist **Alle**.

- **Weitere lokale Administratoren für in Azure AD eingebundene Geräte**: Sie können die Benutzer auswählen, denen lokale Administratorrechte auf einem Gerät erteilt werden. Hier hinzugefügte Benutzer werden der Rolle *Geräteadministratoren* in Azure AD hinzugefügt. Globale Administratoren in Azure AD und Gerätebesitzer erhalten standardmäßig lokale Administratorrechte. Diese Option ist eine Premium Edition-Funktion, die über Produkte wie Azure AD Premium oder die Enterprise Mobility Suite (EMS) zur Verfügung steht. 

- **Benutzer dürfen ihre Geräte für Azure AD registrieren**: Sie müssen diese Einstellung konfigurieren, um die [Registrierung](device-management-introduction.md#azure-ad-registered-devices) von Geräten in Azure AD zuzulassen. Bei Auswahl von **Keine** dürfen Geräte nicht registriert werden, sofern sie nicht in Azure AD eingebunden oder hybrid in Azure AD eingebunden sind. Für die Registrierung bei Microsoft Intune oder der mobilen Geräteverwaltung für Office 365 ist eine Registrierung erforderlich. Wenn Sie einen dieser Dienste konfiguriert haben, wird **ALLE** ausgewählt, und die Option **KEINE** ist nicht verfügbar.

- **Multi-factor Auth zum Hinzufügen von Geräten erforderlich**: Sie können auswählen, ob Benutzer einen sekundären Authentifizierungsfaktor bereitstellen müssen, um ihr Gerät in [Azure AD](device-management-introduction.md#azure-ad-joined-devices) einzubinden. Der Standardwert lautet **Nein**. Es wird empfohlen, beim Registrieren eines Geräts die mehrstufige Authentifizierung zu verwenden. Bevor Sie die mehrstufige Authentifizierung für diesen Dienst aktivieren, müssen Sie sicherstellen, dass sie für die Benutzer konfiguriert ist, die ihre Geräte registrieren. Weitere Informationen zu verschiedenen Azure-Diensten mit mehrstufiger Authentifizierung finden Sie in den [ersten Schritten mit der mehrstufigen Azure-Authentifizierung](../multi-factor-authentication/multi-factor-authentication-get-started.md). 

- **Maximale Anzahl von Geräten pro Benutzer**: Mit dieser Einstellung können Sie die maximale Anzahl von Geräten festlegen, die ein Benutzer in Azure AD haben kann. Wenn ein Benutzer diese Anzahl erreicht, kann er keine weiteren Geräte hinzufügen, bis mindestens eines der vorhandenen Geräte entfernt wird. Das Geräteangebot wird für alle Geräte gezählt, die heute mit Azure AD verknüpft oder in Azure AD registriert sind. Der Standardwert ist **20**.

- **Benutzer können Einstellungen und App-Daten geräteübergreifend synchronisieren**: Diese Einstellung ist standardmäßig auf **KEINE** festgelegt. Wenn Sie bestimmte Benutzer oder Gruppen oder „ALLE“ auswählen, können die Einstellungen und App-Daten der Benutzer auf ihren Windows 10-Geräten synchronisiert werden. Erfahren Sie mehr über die Funktionsweise der Synchronisierung in Windows 10.
Diese Option ist eine Premium-Funktion, die über Produkte wie Azure AD Premium oder die Enterprise Mobility Suite (EMS) zur Verfügung steht.
 




## <a name="locate-devices"></a>Suchen nach Geräten

Sie haben zwei Optionen, um nach registrierten und eingebundenen Geräten zu suchen:

- **Alle Geräte** im Abschnitt **Verwalten** der Seite **Geräte**  

    ![Alle Geräte](./media/device-management-azure-portal/41.png)


- **Geräte** im Abschnitt **Verwalten** der Seite **Benutzer**
 
    ![Alle Geräte](./media/device-management-azure-portal/43.png)



Bei beiden Optionen erhalten Sie eine Ansicht, die:


- Ihnen die Suche nach Geräten mit dem Anzeigenamen als Filter ermöglicht.

- Eine detaillierte Übersicht über registrierte und verknüpfte Geräte enthält.

- Ihnen die Durchführung allgemeiner Geräteverwaltungsaufgaben ermöglicht.
   

![Alle Geräte](./media/device-management-azure-portal/51.png)


## <a name="device-management-tasks"></a>Geräteverwaltungsaufgaben

Als Administrator können Sie die registrierten oder verknüpften Geräte verwalten. Dieser Abschnitt enthält Informationen zu allgemeinen Geräteverwaltungsaufgaben.


### <a name="manage-an-intune-device"></a>Verwalten eines Intune-Geräts

Wenn Sie ein Intune-Administrator sind, können Sie Geräte verwalten, die mit der Kennzeichnung **Microsoft Intune** versehen sind. Ein Administrator kann zusätzliche Gerät anzeigen. 

![Verwalten eines Intune-Geräts](./media/device-management-azure-portal/31.png)


### <a name="enable--disable-an-azure-ad-device"></a>Aktivieren/Deaktivieren von Azure AD-Geräten

Sie haben zwei Optionen, um ein Gerät zu aktivieren bzw. zu deaktivieren:

- Aufgabenmenü („...“) auf der Seite **Alle Geräte**

    ![Verwalten eines Intune-Geräts](./media/device-management-azure-portal/71.png)

- Symbolleiste auf der Seite **Geräte**

    ![Verwalten eines Intune-Geräts](./media/device-management-azure-portal/32.png)


**Hinweise:**

- Zum Aktivieren oder Deaktivieren eines Geräts müssen Sie ein globaler Administrator in Azure AD sein. 
- Durch das Deaktivieren eines Geräts verhindern Sie, dass über das Gerät auf Ihre Azure AD-Ressourcen zugegriffen wird. 



### <a name="delete-an-azure-ad-device"></a>Löschen eines Azure AD-Geräts

Sie haben zwei Optionen, um ein Gerät zu löschen:

- Aufgabenmenü („...“) auf der Seite **Alle Geräte**

    ![Verwalten eines Intune-Geräts](./media/device-management-azure-portal/72.png)

- Symbolleiste auf der Seite **Geräte**

    ![Gerät löschen](./media/device-management-azure-portal/34.png)


**Hinweise:**

- Zum Löschen eines Geräts müssen Sie ein globaler Administrator in Azure AD sein.  

- Das Löschen eines Geräts:
 
    - Verhindert, dass über das Gerät auf Ihre Azure AD-Ressourcen zugegriffen wird 

    - Entfernt alle Details, die an das Gerät angefügt wurden, z.B. BitLocker-Schlüssel für Windows-Geräte  

    - Stellt eine Aktivität dar, die nicht rückgängig gemacht werden kann, und wird nur empfohlen, wenn das Löschen notwendig ist

Wenn ein Gerät von einer anderen Verwaltungsautorität (z.B. Microsoft Intune) verwaltet wird, sollten Sie vor dem Löschen des Geräts in Azure AD sicherstellen, dass es zurückgesetzt bzw. außer Kraft gesetzt wurde.

 


### <a name="view-or-copy-device-id"></a>Anzeigen oder Kopieren einer Geräte-ID

Sie können während der Problembehandlung eine Geräte-ID oder PowerShell verwenden, um die ID-Details eines Geräts zu überprüfen. Klicken Sie auf das Gerät, um den Kopierbefehl anzuzeigen.

![Anzeigen der Geräte-ID](./media/device-management-azure-portal/35.png)
  

### <a name="view-or-copy-bitlocker-keys"></a>Anzeigen oder Kopieren von BitLocker-Schlüsseln

Wenn Sie ein Administrator sind, können Sie die BitLocker-Schlüssel anzeigen und kopieren, um Benutzern die Wiederherstellung verschlüsselter Laufwerke zu ermöglichen. Diese Schlüssel sind nur für Windows-Geräte verfügbar, die verschlüsselt sind und deren Schlüssel in Azure AD gespeichert sind. Sie können die Schlüssel kopieren, wenn Sie auf die Details des Geräts zugreifen.
 
![BitLocker-Schlüssel anzeigen](./media/device-management-azure-portal/36.png)



## <a name="audit-logs"></a>Überwachungsprotokolle


Geräteaktivitäten sind über die Aktivitätsprotokolle verfügbar. Hierzu zählen vom Geräteregistrierungsdienst und von Benutzern ausgelöste Aktivitäten:

- Erstellen von Geräten und Hinzufügen von Besitzern/Benutzern auf dem Gerät

- Änderungen der Geräteeinstellungen

- Gerätevorgänge wie beispielsweise das Löschen oder Aktualisieren eines Geräts
 
Ihr Einstiegspunkt für die Überwachungsdaten ist die Option **Überwachungsprotokolle** im Abschnitt **Aktivität** der Seite **Geräte**.

![Überwachungsprotokolle](./media/device-management-azure-portal/61.png)


Ein Überwachungsprotokoll enthält eine Standardlistenansicht mit folgenden Informationen:

- Datum und Uhrzeit des Auftretens

- Ziele

- Initiator/Akteur einer Aktivität (Wer)

- Aktivität (Was)

![Überwachungsprotokolle](./media/device-management-azure-portal/63.png)

Sie können die Listenansicht anpassen, indem Sie in der Symbolleiste auf **Spalten** klicken.
 
![Überwachungsprotokolle](./media/device-management-azure-portal/64.png)


Sie können die Überwachungsdaten mit den folgenden Feldern filtern, um die gemeldeten Daten gemäß Ihren Bedürfnissen einzugrenzen:

- Category (Kategorie)
- Aktivitätsressourcentyp
- Aktivität
- Datumsbereich
- Ziel
- Initiiert von (Akteur)

Zusätzlich zu den Filtern können Sie nach bestimmten Einträgen suchen.

![Überwachungsprotokolle](./media/device-management-azure-portal/65.png)

## <a name="next-steps"></a>Nächste Schritte

* [Einführung in die Geräteverwaltung in Azure Active Directory](device-management-introduction.md)



