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
ms.openlocfilehash: c8b0529b0ae45d7bcee5574991551a424c13ba70
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34713863"
---
# <a name="azure-active-directory-device-management-faq"></a>Azure Active Directory: Häufig gestellte Fragen zur Geräteverwaltung



**F: Wie kann ich ein macOS-Gerät registrieren?**

**A:** So registrieren Sie ein macOS-Gerät:

1.  [Erstellen Sie eine Konformitätsrichtlinie](https://docs.microsoft.com/intune/compliance-policy-create-mac-os).
2.  [Definieren Sie eine Richtlinie zum bedingten Zugriff für macOS-Geräte](active-directory-conditional-access-azure-portal.md). 

**Hinweise:**

- Die in der Richtlinie zum bedingten Zugriff enthaltenen Benutzer benötigen für den Zugriff auf Ressourcen eine [unterstützte Version von Office für macOS](active-directory-conditional-access-technical-reference.md#client-apps-condition). 

- Beim ersten Zugriffsversuch werden die Benutzer aufgefordert, das Gerät über das Unternehmensportal zu registrieren.

---

**F: Ich habe das Gerät vor kurzem registriert. Warum kann ich das Gerät nicht in meinen Benutzerinformationen im Azure-Portal sehen?**

**A:** Windows 10-Geräte, die in Azure AD eingebundene Hybridgeräte sind, werden nicht unter den BENUTZER-Geräten angezeigt.
Mit PowerShell können Sie alle Geräte anzeigen. 

Nur die folgenden Geräte werden unter den BENUTZER-Geräten aufgeführt:

- Alle persönlichen Geräte, die keine in Azure AD eingebundenen Hybridgeräte sind. 
- Alle Geräte, die keine Windows 10/Windows Server 2016-Geräte sind.
- Alle Geräte ohne Windows 

---

**F: Warum kann ich im Azure-Portal nicht alle Geräte sehen, die in Azure Active Directory registriert sind?** 

**A:** Sie können diese jetzt unter „Azure AD-Verzeichnis“ -> Menü „Alle Geräte“ sehen. Sie können auch Azure PowerShell verwenden, um nach allen Geräten zu suchen. Weitere Informationen finden Sie im [Get-MsolDevice](/powershell/module/msonline/get-msoldevice?view=azureadps-1.0)-Cmdlet.

--- 

**F: Wie ermittle ich den Geräteregistrierungsstatus des Clients?**

**A:** Führen Sie für Geräte mit Windows 10, Windows Server 2016 oder höher „dsregcmd.exe/status“.

Führen Sie für frühere Betriebssystemversionen das Programm „%programFiles%\Microsoft Workplace Join\autoworkplace.exe“ aus.

---

**F: Warum ist ein Gerät, das ich im Azure-Portal oder mit Windows PowerShell gelöscht habe, immer noch als registriert aufgeführt?**

**A:** Dies ist beabsichtigt. Das Gerät hat keinen Zugriff auf Ressourcen in der Cloud. Wenn Sie das Gerät registrieren möchten, muss dies manuell auf dem Gerät erfolgen. 

So bereinigen Sie den Verknüpfungsstatus aus Windows 10 und Windows Server 2016, die in die lokale AD-Domäne eingebunden sind:

1.  Öffnen Sie die Eingabeaufforderung als Administrator.

2.  Geben Sie `dsregcmd.exe /debug /leave` ein

3.  Melden Sie sich ab und erneut an, um den geplanten Task auszulösen, der das Gerät erneut in Azure AD registriert. 

Für früherer Windows-Versionen, die in die lokale AD-Domäne eingebunden sind:

1.  Öffnen Sie die Eingabeaufforderung als Administrator.
2.  Geben Sie `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /l"`ein.
3.  Geben Sie `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /j"`ein.

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


**F: Ich sehe den Gerätedatensatz im Azure-Portal in den Informationen unter BENUTZER und sehe, dass der Status auf dem Client „registriert“ lautet. Sind diese Einstellungen für den bedingten Zugriff richtig?**

**A:** Der Verknüpfungsstatus des Geräts, der in „deviceID“ festgehalten ist, muss mit dem Status in Azure AD übereinstimmen und alle Bewertungskriterien für bedingten Zugriff erfüllen. Weitere Details finden Sie unter [Erste Schritte bei der Azure Active Directory-Geräteregistrierung](active-directory-device-registration.md).

---

**F: Warum erscheint die Meldung „Benutzername oder Kennwort ist falsch“ für ein Gerät, das ich vor kurzem in Azure AD eingebunden habe?**

**A:** Häufige Ursachen für dieses Szenario:

- Ihre Benutzeranmeldeinformationen sind nicht mehr gültig.

- Ihr Computer kann nicht mit Azure Active Directory kommunizieren. Suchen Sie nach Netzwerkkonnektivitätsproblemen.

- Die Voraussetzungen für Azure AD Join wurden nicht erfüllt. Stellen Sie sicher, dass Sie die Schritte für [Erweitern von Cloudfunktionen auf Windows 10-Geräte über Azure Active Directory Join](active-directory-azureadjoin-overview.md) befolgt haben.  

- Für Verbundanmeldungen muss der Verbundserver einen aktiven WS-Trust-Endpunkt unterstützen. 

---

**F: Warum sehe ich das Dialogfeld „Leider ist ein Fehler aufgetreten“, wenn ich versuche, meinen PC in Azure AD einzubinden?**

**A:** Dies ist ein Ergebnis der Einrichtung der Azure Active Directory-Registrierung bei Intune. Stellen Sie sicher, dass dem Benutzer, der ein Einbinden in Azure AD versucht, die richtige Intune-Lizenz zugewiesen wurde. Weitere Informationen finden Sie unter [Einrichten der Windows-Geräteverwaltung](https://docs.microsoft.com/intune/deploy-use/set-up-windows-device-management-with-microsoft-intune#azure-active-directory-enrollment).  

---

**F: Warum konnte ich meinen PC nicht einbinden, obwohl ich keine Fehlerinformationen erhalten habe?**

**A:** Wahrscheinlich ist der Benutzer mit dem integrierten Administratorkonto beim Gerät angemeldet. Erstellen Sie ein anderes lokales Konto, bevor Sie Azure Active Directory Join verwenden, um die Einrichtung abzuschließen. 

---

**F: Wo finde ich Anweisungen zum Einrichten der automatischen Geräteregistrierung?**

**A:** Detaillierte Anleitungen finden Sie unter [Konfigurieren der automatischen Registrierung von in die Domäne eingebundenen Windows-Geräten mit Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).

---

**F: Wo finde ich Informationen zur Problembehandlung bei der automatischen Geräteregistrierung?**

**A:** Informationen zur Problembehandlung finden Sie unter:

- [Beheben von Problemen bei der automatischen Registrierung von Computern, die in die Azure AD-Domäne eingebunden sind, für Windows 10 und Windows Server 2016](device-management-troubleshoot-hybrid-join-windows-current.md)

- [Beheben von Problemen bei der automatischen Registrierung von Computern, die in die Azure AD-Domäne eingebunden sind, für kompatible Windows-Clients](device-management-troubleshoot-hybrid-join-windows-legacy.md)
 
---

