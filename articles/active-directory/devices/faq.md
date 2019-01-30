---
title: 'Azure Active Directory: Häufig gestellte Fragen zur Geräteverwaltung | Microsoft-Dokumentation'
description: 'Azure Active Directory: Häufig gestellte Fragen zur Geräteverwaltung.'
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2010
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 916de2de6cdc19bfa1e3967661d40693d4be1e99
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2019
ms.locfileid: "54852387"
---
# <a name="azure-active-directory-device-management-faq"></a>Azure Active Directory: Häufig gestellte Fragen zur Geräteverwaltung

**F: Ich habe das Gerät vor Kurzem registriert. Warum kann ich das Gerät nicht in meinen Benutzerinformationen im Azure-Portal sehen? Oder warum ist der Gerätebesitzer für in Azure Active Directory (AD) eingebundene Hybridgeräte als „N/V“ markiert?**

**A:** Windows 10-Geräte, die in Azure AD eingebundene Hybridgeräte sind, werden nicht unter den **BENUTZER-Geräten** angezeigt.
Verwenden Sie die Ansicht **Alle Geräte** im Azure-Portal. Sie können auch das PowerShell-Cmdlet [Get-MsolDevice](https://docs.microsoft.com/en-us/powershell/module/msonline/get-msoldevice?view=azureadps-1.0) verwenden.

Nur die folgenden Geräte werden unter den **BENUTZER-Geräten** aufgeführt:

- Alle persönlichen Geräte, die keine in Azure AD eingebundenen Hybridgeräte sind. 
- Alle Geräte, die keine Windows 10- oder Windows Server 2016-Geräte sind.
- Alle Geräte ohne Windows. 

--- 

**F: Wie ermittle ich den Geräteregistrierungsstatus des Clients?**

**A:** Navigieren Sie im Azure-Portal zu **Alle Geräte**. Suchen Sie für das Gerät anhand der Geräte-ID. Überprüfen Sie den Wert in der Spalte „Jointyp“. Möglicherweise wurde das Gerät zurückgesetzt oder ein Reimaging durchgeführt. Daher ist es wichtig, den Geräteregistrierungsstatus auf dem Gerät zu überprüfen:

- Führen Sie für Geräte mit Windows 10, Windows Server 2016 oder höher `dsregcmd.exe /status` aus.
- Führen Sie für kompatible Betriebssystemversionen `%programFiles%\Microsoft Workplace Join\autoworkplace.exe` aus.

---

**F: Der Gerätedatensatz wird in meinen Benutzerinformationen im Azure-Portal angezeigt. Und ich sehe den Status als auf dem Gerät registriert. Sind diese Einstellungen für den bedingten Zugriff richtig?**

**A:** Der Verknüpfungsstatus des Geräts, der in **deviceID** angegeben ist, muss mit dem Status in Azure AD übereinstimmen und alle Bewertungskriterien für den bedingten Zugriff erfüllen. Weitere Informationen finden Sie unter [Vorschreiben der Verwendung verwalteter Geräte für den Zugriff auf Cloud-Apps mithilfe des bedingten Zugriffs](../conditional-access/require-managed-devices.md).

---

**F: Ich habe mein Gerät im Azure-Portal oder mithilfe von Windows PowerShell gelöscht. Laut lokalem Status auf dem Gerät ist es aber immer noch registriert.**

**A:** Dieser Vorgang ist von vornherein vorgesehen. Das Gerät hat keinen Zugriff auf Ressourcen in der Cloud. 

Wenn Sie sich erneut registrieren möchten, müssen Sie eine manuelle Aktion am Gerät durchführen. 

Um den Verknüpfungsstatus aus Windows 10 und Windows Server 2016, die in die lokale Active Directory-Domäne eingebunden sind zu löschen, führen Sie die folgenden Schritte aus:

1.  Öffnen Sie die Eingabeaufforderung als Administrator.

2.  Geben Sie `dsregcmd.exe /debug /leave` ein.

3.  Melden Sie sich ab und erneut an, um den geplanten Task auszulösen, der das Gerät erneut in Azure AD registriert. 

Gehen Sie für kompatible Windows-BS-Versionen, die in die lokale Active Directory-Domäne eingebunden sind, folgendermaßen vor:

1.  Öffnen Sie die Eingabeaufforderung als Administrator.
2.  Geben Sie `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /l"` ein.
3.  Geben Sie `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /j"` ein.

---

**F: Warum sehe ich doppelte Geräteeinträge im Azure-Portal?**

**A:**

-   Wenn unter Windows 10 und Windows Server 2016 wiederholt versucht wird, dasselbe Gerät zu entfernen und erneut hinzuzufügen, können doppelte Einträge auftreten. 

-   Jeder Windows-Benutzer, der **Geschäfts-, Schul- oder Unikonto hinzufügen** verwendet, erstellt einen neuen Gerätedatensatz mit demselben Gerätenamen.

-   Für kompatible Windows-Versionen, die über die automatische Registrierung in die lokale Active Directory-Domäne eingebunden sind, wird ein neuer Gerätedatensatz mit demselben Gerätenamen für jeden Domänenbenutzer erstellt, der sich beim Gerät anmeldet. 

-   Ein in Azure AD eingebundener Computer, der gelöscht, neu installiert und mit demselben Namen wieder eingebunden wurde, wird als anderer Datensatz mit demselben Gerätenamen angezeigt.

---

**F: Warum kann ein Benutzer weiterhin Ressourcen von einem Gerät aufrufen, das ich im Azure-Portal deaktiviert habe?**

**A:** Das Widerrufen dauert bis zu einer Stunde.

>[!NOTE] 
>Für registrierte Geräte wird empfohlen, das Gerät zu löschen, um sicherzustellen, dass Benutzer nicht auf die Ressourcen zugreifen können. Weitere Informationen finden Sie unter [Was ist die Geräteregistrierung?](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune). 

---

## <a name="azure-ad-join-faq"></a>Häufig gestellte Fragen zu Azure AD Join

**F: Wie entferne ich ein in Azure AD eingebundenes Gerät lokal auf dem Gerät?**

**A:** 
- Für in Azure AD eingebundene Hybridgeräte muss die automatische Registrierung deaktiviert sein. Der geplante Task registriert das Gerät also nicht erneut. Öffnen Sie als nächstes die Eingabeaufforderung als Administrator, und geben Sie `dsregcmd.exe /debug /leave` ein. Oder führen Sie diesen Befehl als Skript für mehrere Geräte aus, um die Einbindung für diese Geräte gleichzeitig aufzuheben.

- Stellen Sie bei ausschließlich in Azure AD eingebundenen Geräten sicher, dass Sie über ein lokales Offlineadministratorkonto verfügen. Sie können sich nicht mit Ihren Azure AD-Anmeldeinformationen anmelden. Navigieren Sie als Nächstes zu **Einstellungen** > **Konten** > **Auf Geschäfts-, Schul- oder Unikonto zugreifen**. Wählen Sie Ihr Konto aus, und klicken Sie auf **Trennen**. Befolgen Sie die Anweisungen, und geben Sie die Anmeldeinformationen für den lokalen Administrator an, wenn Sie aufgefordert werden. Starten Sie das Gerät neu, um den Vorgang zur Aufhebung einer Einbindung abzuschließen.

---

**F: Können sich meine Benutzer bei in Azure AD eingebundenen Geräten anmelden, die in Azure AD gelöscht oder deaktiviert wurden?**

**A:** Ja. Windows verfügt über eine Zwischenspeicherfunktion für Benutzernamen und Kennwörter, die es Benutzern, die sich zuvor angemeldet haben, ermöglicht, auch ohne Netzwerkverbindung schnell auf den Desktop zuzugreifen. 

Ein in Azure AD gelöschtes oder deaktiviertes Gerät wird vom Windows-Gerät nicht erkannt. Benutzer, die sich zuvor angemeldet haben, greifen also weiterhin mit dem im Cache gespeicherten Benutzernamen und Kennwort auf den Desktop zu. Wenn das Gerät gelöscht oder deaktiviert wird, können die Benutzer jedoch nicht auf durch den gerätebasierten bedingten Zugriff geschützte Ressourcen zugreifen. 

Benutzer, die sich zuvor nicht angemeldet haben, können nicht auf das Gerät zugreifen. Es wurde kein zwischengespeicherter Benutzernamen oder Kennwort für sie aktiviert. 

---

**F: Können sich deaktivierte oder gelöschte Benutzer bei in Azure AD eingebundenen Geräten anmelden?**

**A:** Ja, aber nur für einen begrenzten Zeitraum. Ein in Azure AD gelöschter oder deaktivierter Benutzer wird vom Windows-Gerät nicht sofort erkannt. Benutzer, die sich zuvor angemeldet haben, greifen also mit dem im Cache gespeicherten Benutzernamen und Kennwort auf den Desktop zu. 

In der Regel ist das Gerät in weniger als vier Stunden über den Benutzerstatus informiert. Dann blockiert Windows den Zugriff dieser Benutzer auf den Desktop. Wie der Benutzer gelöscht oder in Azure AD deaktiviert ist, werden alle seine Token widerrufen. Daher kann der Benutzer auf keine Ressourcen mehr zugreifen. 

Gelöschte oder deaktivierte Benutzer, die sich zuvor nicht angemeldet haben, können nicht auf das Gerät zugreifen. Es wurde kein zwischengespeicherter Benutzernamen oder Kennwort für sie aktiviert. 

---

**F: Meine Benutzer können über in Azure AD eingebundene Geräte keine Drucker suchen. Wie kann ich das Drucken über diese Geräte aktivieren?**

**A:** Informationen zum Bereitstellen von Druckern für in Azure AD eingebundene Geräte finden Sie unter [Bereitstellen von Windows Server Hybrid Cloud Print mit Vorauthentifizierung](https://docs.microsoft.com/windows-server/administration/hybrid-cloud-print/hybrid-cloud-print-deploy). Sie benötigen einen lokalen Windows-Server, um das Drucken in Hybrid Clouds bereitzustellen. Aktuell ist kein cloudbasierter Druckdienst verfügbar. 

---

**F: Wie kann ich eine Verbindung mit einem in Azure AD eingebundenen Remotegerät herstellen?**

**A:** Informationen finden Sie unter [Herstellen einer Verbindung mit einem in Azure AD eingebundenen Remotecomputer](https://docs.microsoft.com/windows/client-management/connect-to-remote-aadj-pc).

---

**F: Warum wird meinen Benutzern angezeigt: *Von hier aus haben Sie darauf keinen Zugriff*?**

**A:** Haben Sie bestimmte Regeln für bedingten Zugriff konfiguriert, um einen bestimmten Gerätestatus zu erfordern? Wenn das Gerät die Kriterien nicht erfüllt, wird der Benutzer blockiert und diese Meldung angezeigt. Überprüfen Sie die Regeln für bedingten Zugriff. Stellen Sie sicher, dass das Gerät die Kriterien erfüllt, um die Meldung zu vermeiden.

---

**F: Warum erhalten einige Benutzer in keine Azure Multi-Factor Authentication-Eingabeaufforderungen auf in Azure AD eingebundenen Geräten?**

**A:** Ein Benutzer kann ein Gerät mit Multi-Factor Authentication in Azure AD einbinden oder registrieren. Dann wird das Gerät zu einem vertrauenswürdigen zweiten Faktor für diesen Benutzer. Wann immer sich derselbe Benutzer bei dem Gerät anmeldet und auf eine Anwendung zugreift, betrachtet Azure AD das Gerät als einen zweiten Faktor. Es ermöglicht dem Benutzer den nahtlosen Zugriff auf Anwendungen ohne zusätzliche Multi-Factor Authentication-Eingabeaufforderungen. 

Dieses Verhalten gilt nicht für andere Benutzer, die sich bei diesem Gerät anmelden. So erhalten alle anderen Benutzer, die auf dieses Gerät zugreifen, eine Multi-Factor Authentication. Dann können sie auf Anwendungen zugreifen, für die Multi-Factor Authentication erforderlich ist.

---

**F: Warum wird die Meldung *Benutzername oder Kennwort ist falsch* für ein Gerät angezeigt, das ich vor Kurzem in Azure AD eingebunden habe?**

**A:** Häufige Ursachen für dieses Szenario sind:

- Ihre Benutzeranmeldeinformationen sind nicht mehr gültig.

- Ihr Computer kann nicht mit Azure Active Directory kommunizieren. Suchen Sie nach Netzwerkkonnektivitätsproblemen.

- Für Verbundanmeldungen muss der Verbundserver aktive und zugängliche WS-Trust-Endpunkte unterstützen. 

- Sie haben die Passthrough-Authentifizierung aktiviert. Daher muss Ihr temporäres Kennwort geändert werden, wenn Sie sich anmelden.

---

**F: Warum sehe ich das Dialogfeld *Leider ist ein Fehler aufgetreten*, wenn ich versuche, meinen PC in Azure AD einzubinden?**

**A:** Dieser Fehler tritt bei der Einrichtung der Azure Active Directory-Registrierung bei Intune auf. Stellen Sie sicher, dass dem Benutzer, der ein Einbinden in Azure AD versucht, die richtige Intune-Lizenz zugewiesen wurde. Weitere Informationen finden Sie unter [Einrichten der Registrierung für Windows-Geräte](https://docs.microsoft.com/intune/deploy-use/set-up-windows-device-management-with-microsoft-intune#azure-active-directory-enrollment).  

---

**F: Warum konnte ich meinen PC nicht in Azure AD einbinden, obwohl ich keine Fehlerinformationen erhalten habe?**

**A:** Wahrscheinlich haben Sie sich mit dem lokalen integrierten Administratorkonto beim Gerät angemeldet. Erstellen Sie ein anderes lokales Konto, bevor Sie Azure Active Directory Join verwenden, um die Einrichtung abzuschließen. 

---

## <a name="hybrid-azure-ad-join-faq"></a>Häufig gestellte Fragen zu Azure AD Hybrid Join

**F: Wo finde ich Problembehandlungsinformationen für die Diagnose von Azure AD Hybrid Join-Fehlern?**

**A:** Informationen zur Problembehandlung finden Sie in diesen Artikeln:

- [Beheben von Problemen mit Geräten unter Windows 10 und Windows Server 2016 mit Hybrideinbindung in Azure Active Directory](troubleshoot-hybrid-join-windows-current.md)

- [Beheben von Problemen mit Geräten mit Hybrideinbindung in Azure Active Directory](troubleshoot-hybrid-join-windows-legacy.md)
 
**F: Warum wird in der Liste mit den Azure AD-Geräten für mein in Azure AD eingebundenes Windows 10-Hybridgerät ein doppelter Azure AD-Registrierungseintrag angezeigt?**

**A:** Wenn Benutzer ihr Konto den Apps auf einem in die Domäne eingebundenen Gerät hinzufügen, wird ggf. eine Frage der Art **Soll das Konto Windows hinzugefügt werden?** angezeigt. Wenn Sie in der Eingabeaufforderung **Ja** eingeben, wird das Gerät in Azure AD registriert. Der Vertrauenstyp wird als in Azure AD registriert gekennzeichnet. Nachdem Sie Azure AD Hybrid Join in Ihrer Organisation aktiviert haben, wird das Gerät auch in die Azure AD-Hybridumgebung eingebunden. Dann werden zwei Gerätestatus für dasselbe Gerät angezeigt. 

Azure AD Hybrid Join hat Vorrang vor dem Azure AD-Registrierungsstatus. Ihr Gerät wird also für alle Auswertungen in Bezug auf die Authentifizierung und den bedingten Zugriff als Azure AD-Hybrideinbindung angesehen. Sie können den Azure AD-Registrierungseintrag für das Gerät daher ohne Weiteres aus dem Azure AD-Portal löschen. Erfahren Sie mehr zum [Vermeiden oder Bereinigen dieses zweifachen Status auf einem Windows 10-Computer](https://docs.microsoft.com/azure/active-directory/devices/hybrid-azuread-join-plan#review-things-you-should-know). 

---

## <a name="azure-ad-register-faq"></a>Häufig gestellte Fragen zur Azure AD-Registrierung

**F: Kann ich Android- oder iOS-BYOD-Geräte registrieren?**

**A:** Ja, aber nur mit dem Azure-Dienst zur Geräteregistrierung und wenn Sie Hybrid-Kunde sind. Es wird nicht mit dem lokalen Geräteregistrierungsdienst in Active Directory Federation Services (AD FS) unterstützt.

**F: Wie kann ich ein macOS-Gerät registrieren?**

**A:** Führen Sie die folgenden Schritte aus:

1.  [Erstellen Sie eine Konformitätsrichtlinie](https://docs.microsoft.com/intune/compliance-policy-create-mac-os).
2.  [Definieren Sie eine Richtlinie zum bedingten Zugriff für macOS-Geräte](../active-directory-conditional-access-azure-portal.md). 

**Hinweise:**

- Die in der Richtlinie zum bedingten Zugriff enthaltenen Benutzer benötigen für den Zugriff auf Ressourcen eine [unterstützte Version von Office für macOS](../conditional-access/technical-reference.md#client-apps-condition). 

- Beim ersten Zugriffsversuch werden die Benutzer aufgefordert, das Gerät über das Unternehmensportal zu registrieren.

---


**F: Welche MS-Organisation-P2P-Access-Zertifikate sind auf unseren Windows 10-Geräten vorhanden?**

**A:** Die MS-Organisation-P2P-Access-Zertifikate werden von Azure AD sowohl für in Azure AD eingebundene Geräte als auch für in Azure AD eingebundene Hybridgeräte ausgestellt. Diese Zertifikate werden verwendet, um die Vertrauensstellung zwischen Geräten desselben Mandanten für Remotedesktopszenarien zu ermöglichen. Ein Zertifikat wird für das Gerät und ein weiteres für den Benutzer ausgestellt. Das Gerätezertifikat befindet sich in `Local Computer\Personal\Certificates` und gilt für einen Tag. Dieses Zertifikat wird erneuert (durch Ausstellung eines neuen Zertifikats), wenn das Gerät noch in Azure AD aktiv ist. Das Benutzerzertifikat befindet sich in `Current User\Personal\Certificates` und dieses Zertifikat ist ebenfalls für einen Tag gültig, wird aber auf Anfrage ausgestellt, wenn ein Benutzer eine Remotedesktopsitzung mit einem anderen in Azure AD eingebundenen Gerät versucht. Es wird bei Ablauf nicht erneuert. Beide Zertifikate werden über das MS-Organisation-P2P-Access-Zertifikat in `Local Computer\AAD Token Issuer\Certificates` ausgegeben. Dieses Zertifikat wird von Azure AD bei der Registrierung des Geräts ausgegeben. 

---

**F: Warum sehe ich mehrere abgelaufene Zertifikate, die von MS-Organisation-P2P-Access auf unseren Windows 10-Geräten ausgestellt wurden? Wie kann ich diese löschen?**

**A:** Es wurde ein Problem unter Windows 10 Version 1709 und niedriger festgestellt, bei dem abgelaufene MS-Organisation-P2P-Access-Zertifikate aufgrund von kryptographischen Problemen weiterhin im Computerspeicher vorhanden waren. Ihre Benutzer könnten Probleme mit der Netzwerkkonnektivität haben, wenn Sie VPN-Clients (z.B. Cisco AnyConnect) verwenden, die die große Anzahl abgelaufener Zertifikate nicht verarbeiten können. Dieses Problem wurde in der Version Windows 10 1803 behoben, um solche abgelaufenen MS-Organisation-P2P-Access-Zertifikate automatisch zu löschen. Sie können dieses Problem beheben, indem Sie Ihre Geräte auf Windows 10 1803 aktualisieren. Wenn Sie nicht aktualisieren können, können Sie diese Zertifikate ohne negative Auswirkungen löschen.  
