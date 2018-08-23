---
title: 'Azure Active Directory: Häufig gestellte Fragen zur Geräteverwaltung | Microsoft-Dokumentation'
description: 'Azure Active Directory: Häufig gestellte Fragen zur Geräteverwaltung.'
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 72035c2f13f5a2a749feabbb26db5500f6c3fc0a
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/18/2018
ms.locfileid: "42143107"
---
# <a name="azure-active-directory-device-management-faq"></a>Azure Active Directory: Häufig gestellte Fragen zur Geräteverwaltung

**F: Kann ich Android- oder iOS-BYOD-Geräte registrieren?**

**A:** Ja, aber nur mit dem Azure-Dienst zur Geräteregistrierung und wenn Sie Hybrid-Kunde sind. Für den lokalen Dienst zur Geräteregistrierung in AD FS wird dies nicht unterstützt.

**F: Wie kann ich ein macOS-Gerät registrieren?**

**A:** So registrieren Sie ein macOS-Gerät:

1.  [Erstellen Sie eine Konformitätsrichtlinie](https://docs.microsoft.com/intune/compliance-policy-create-mac-os).
2.  [Definieren Sie eine Richtlinie zum bedingten Zugriff für macOS-Geräte](../active-directory-conditional-access-azure-portal.md). 

**Hinweise:**

- Die in der Richtlinie zum bedingten Zugriff enthaltenen Benutzer benötigen für den Zugriff auf Ressourcen eine [unterstützte Version von Office für macOS](../conditional-access/technical-reference.md#client-apps-condition). 

- Beim ersten Zugriffsversuch werden die Benutzer aufgefordert, das Gerät über das Unternehmensportal zu registrieren.

---

**F: Ich habe das Gerät vor kurzem registriert. Warum kann ich das Gerät nicht in meinen Benutzerinformationen im Azure-Portal sehen?**

**A:** Windows 10-Geräte, die in Azure AD eingebundene Hybridgeräte sind, werden nicht unter den BENUTZER-Geräten angezeigt.
Sie müssen die Ansicht „Alle Geräte“ im Azure-Portal verwenden. Sie können auch das PowerShell-Cmdlet [Get-MsolDevice](/powershell/module/msonline/get-msoldevice?view=azureadps-1.0) verwenden.

Nur die folgenden Geräte werden unter den BENUTZER-Geräten aufgeführt:

- Alle persönlichen Geräte, die keine in Azure AD eingebundenen Hybridgeräte sind. 
- Alle Geräte, die keine Windows 10/Windows Server 2016-Geräte sind.
- Alle Geräte ohne Windows 

--- 

**F: Wie ermittle ich den Geräteregistrierungsstatus des Clients?**

**A**: Sie können im Azure-Portal zu „Alle Geräte“ navigieren und mithilfe der Geräte-ID nach dem Gerät suchen. Überprüfen Sie den Wert in der Spalte „Jointyp“.

Wenn Sie den Registrierungsstatus eines lokalen Geräts über ein registriertes Gerät überprüfen möchten, gehen Sie wie folgt vor:

- Führen Sie für Geräte mit Windows 10, Windows Server 2016 oder höher „dsregcmd.exe /status“ aus.
- Führen Sie für frühere Betriebssystemversionen das Programm „%programFiles%\Microsoft Workplace Join\autoworkplace.exe“ aus.

---

**F: Ich habe im Azure-Portal oder mithilfe von Windows PowerShell ein Gerät gelöscht, aber der lokale Status für das Gerät besagt, dass es weiterhin registriert ist. Ist dies in Ordnung?**

**A:** Dies ist beabsichtigt. Das Gerät hat keinen Zugriff auf Ressourcen in der Cloud. 

Wenn Sie das Gerät registrieren möchten, muss dies manuell auf dem Gerät erfolgen. 

So bereinigen Sie den Verknüpfungsstatus aus Windows 10 und Windows Server 2016, die in die lokale AD-Domäne eingebunden sind:

1.  Öffnen Sie die Eingabeaufforderung als Administrator.

2.  Geben Sie `dsregcmd.exe /debug /leave` ein

3.  Melden Sie sich ab und erneut an, um den geplanten Task auszulösen, der das Gerät erneut in Azure AD registriert. 

Für früherer Windows-Versionen, die in die lokale AD-Domäne eingebunden sind:

1.  Öffnen Sie die Eingabeaufforderung als Administrator.
2.  Geben Sie `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /l"`ein.
3.  Geben Sie `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /j"`ein.

---
**F: Wie entferne ich ein in Azure AD eingebundenes Gerät lokal auf dem Gerät?**

**A:** 
- Stellen Sie bei in Hybrid-Azure AD eingebundenen Geräten sicher, dass die automatische Registrierung deaktiviert ist, damit das Gerät im Zuge des geplanten Task nicht erneut registriert wird. Öffnen Sie als Nächstes eine Eingabeaufforderung als Administrator, und geben Sie `dsregcmd.exe /debug /leave` ein. Dieser Befehl kann alternativ auf mehreren Geräten als Skript ausgeführt werden, um eine Verknüpfung als Massenoperation aufzuheben.

- Stellen Sie bei ausschließlich in Azure AD eingebundenen Geräten sicher, dass Sie über ein lokales Offlineadministratorkonto verfügen, oder erstellen Sie eines, da Sie sich nicht mit Azure AD-Benutzeranmeldeinformationen anmelden werden können. Navigieren Sie als Nächstes zu **Einstellungen** > **Konten** > **Auf Geschäfts-, Schul- oder Unikonto zugreifen**. Markieren Sie Ihr Konto, und klicken Sie auf **Trennen**. Befolgen Sie die Anweisungen, und geben Sie die Anmeldeinformationen für den lokalen Administrator an, wenn Sie aufgefordert werden. Starten Sie das Gerät neu, um den Vorgang zur Aufhebung einer Verknüpfung abzuschließen.

---

**F: Meine Benutzer können von in Azure AD eingebundenen Geräten aus keine Drucker durchsuchen. Wie kann ich das Drucken von in Azure AD eingebundenen Geräten aktivieren?**

**A:** Informationen zum Bereitstellen von Druckern für in Azure AD eingebundene Geräte finden Sie unter [Drucken in Hybrid Clouds](https://docs.microsoft.com/windows-server/administration/hybrid-cloud-print/hybrid-cloud-print-deploy). Sie benötigen einen lokalen Windows-Server, um das Drucken in Hybrid Clouds bereitzustellen. Aktuell ist kein cloudbasierter Druckdienst verfügbar. 

---

**F: Wie kann ich eine Verbindung zu einem in einen Azure AD-Remoteserver eingebundenen Gerät herstellen?**
**A:** Weitere Informationen finden Sie im Artikel https://docs.microsoft.com/windows/client-management/connect-to-remote-aadj-pc.

---

**F: Warum sehe ich doppelte Geräteeinträge im Azure-Portal?**

**A:**

-   Wenn unter Windows 10 und Windows Server 2016 wiederholt versucht wird, dasselbe Gerät zu entfernen und erneut hinzuzufügen, können doppelte Einträge auftreten. 

-   Wenn Sie „Geschäfts-, Schul- oder Unikonto hinzufügen“ verwendet haben, erstellt jeder Windows-Benutzer, der „Geschäfts-, Schul- oder Unikonto hinzufügen“ verwendet, einen neuen Gerätedatensatz mit demselben Gerätenamen.

-   Für frühere Windows-Versionen, die über die automatische Registrierung in die lokale AD-Domäne eingebunden sind, wird ein neuer Gerätedatensatz mit demselben Gerätenamen für jeden Domänenbenutzer erstellt, der sich beim Gerät anmeldet. 

-   Ein in Azure AD eingebundener Computer, der gelöscht, neu installiert und mit demselben Namen wieder eingebunden wurde, wird als anderer Datensatz mit demselben Gerätenamen angezeigt.

---

**F: Warum kann ein Benutzer weiterhin Ressourcen von einem Gerät aufrufen, das ich im Azure-Portal deaktiviert habe?**

**A:** Das Widerrufen kann bis zu einer Stunde dauern.

>[!Note] 
>Für ein registriertes Gerät wird empfohlen, das Gerät zu löschen, um sicherzustellen, dass Benutzer nicht auf die Ressourcen zugreifen können. Weitere Informationen finden Sie unter [Registrieren von Geräten für die Verwaltung in Intune](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune). 


---

**F: Warum wird meinen Benutzern angezeigt: „You can’t get there from here“ (Von hier haben Sie darauf keinen Zugriff)?**

**A:** Wenn Sie bestimmte Regeln für bedingten Zugriff konfiguriert haben, die einen spezifischen Gerätestatus erfordern, und das Gerät die Kriterien nicht erfüllt, werden Benutzer blockiert, und sie erhalten diese Meldung. Bitte werten Sie die Richtlinien zum bedingten Zugriff aus, und stellen Sie sicher, dass das Gerät die Kriterien erfüllen kann, damit diese Meldung nicht mehr angezeigt wird.

---

**F: Warum erhalten einige meiner Benutzer auf in Azure AD eingebundenen Geräten keine MFA-Aufforderungen?**

**A:** Wenn ein Benutzer ein Gerät mithilfe von mehrstufiger Authentifizierung in Azure AD einbindet oder bei Azure AD registriert, wird das Gerät selbst zum vertrauenswürdigen zweiten Faktor für diesen bestimmten Benutzer. Wann immer der gleiche Benutzer sich später beim Gerät anmeldet und auf eine Anwendung zugreift, betrachtet Azure AD das Gerät als zweiten Faktor und aktiviert den nahtlosen Zugriff des Benutzers auf seine Anwendungen ohne weitere MFA-Aufforderungen. Dieses Verhalten gilt nicht für andere Benutzer, die sich bei dem Gerät anmelden, daher wird allen anderen Benutzern, die auf das Gerät zugreifen, weiterhin eine MFA-Herausforderung angezeigt, bevor sie auf Anwendungen, die MFA erfordern, zugreifen können.

---

**F: Ich sehe den Gerätedatensatz im Azure-Portal in den Informationen unter BENUTZER und sehe, dass der Status auf dem Gerät „registriert“ lautet. Sind diese Einstellungen für den bedingten Zugriff richtig?**

**A:** Der Verknüpfungsstatus des Geräts, der in „deviceID“ festgehalten ist, muss mit dem Status in Azure AD übereinstimmen und alle Bewertungskriterien für bedingten Zugriff erfüllen. Weitere Informationen finden Sie unter [Vorschreiben der Verwendung verwalteter Geräte für den Zugriff auf Cloud-Apps mithilfe des bedingten Zugriffs](../conditional-access/require-managed-devices.md).

---

**F: Warum erscheint die Meldung „Benutzername oder Kennwort ist falsch“ für ein Gerät, das ich vor kurzem in Azure AD eingebunden habe?**

**A:** Häufige Ursachen für dieses Szenario:

- Ihre Benutzeranmeldeinformationen sind nicht mehr gültig.

- Ihr Computer kann nicht mit Azure Active Directory kommunizieren. Suchen Sie nach Netzwerkkonnektivitätsproblemen.

- Für Verbundanmeldungen muss der Verbundserver einen aktiven WS-Trust-Endpunkt unterstützen. 

- Sie haben die Passthrough-Authentifizierung aktiviert, und dem Benutzer ist ein temporäres Kennwort zugewiesen, das bei der Anmeldung geändert werden muss.

---

**F: Warum sehe ich das Dialogfeld „Leider ist ein Fehler aufgetreten“, wenn ich versuche, meinen PC in Azure AD einzubinden?**

**A:** Dies ist ein Ergebnis der Einrichtung der Azure Active Directory-Registrierung bei Intune. Stellen Sie sicher, dass dem Benutzer, der ein Einbinden in Azure AD versucht, die richtige Intune-Lizenz zugewiesen wurde. Weitere Informationen finden Sie unter [Einrichten der Windows-Geräteverwaltung](https://docs.microsoft.com/intune/deploy-use/set-up-windows-device-management-with-microsoft-intune#azure-active-directory-enrollment).  

---

**F: Warum konnte ich meinen PC nicht einbinden, obwohl ich keine Fehlerinformationen erhalten habe?**

**A**: Wahrscheinlich ist der Benutzer mit dem lokalen integrierten Administratorkonto beim Gerät angemeldet. Erstellen Sie ein anderes lokales Konto, bevor Sie Azure Active Directory Join verwenden, um die Einrichtung abzuschließen. 


---

**F: Wo finde ich Informationen zur Problembehandlung bei der automatischen Geräteregistrierung?**

**A:** Informationen zur Problembehandlung finden Sie unter:

- [Beheben von Problemen bei der automatischen Registrierung von Computern, die in die Azure AD-Domäne eingebunden sind, für Windows 10 und Windows Server 2016](troubleshoot-hybrid-join-windows-current.md)

- [Beheben von Problemen bei der automatischen Registrierung von Computern, die in die Azure AD-Domäne eingebunden sind, für kompatible Windows-Clients](troubleshoot-hybrid-join-windows-legacy.md)
 

---

