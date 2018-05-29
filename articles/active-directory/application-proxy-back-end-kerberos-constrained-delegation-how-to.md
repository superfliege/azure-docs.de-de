---
title: Problembehandlung von Konfigurationen der eingeschränkten Kerberos-Delegierung für Anwendungsproxy | Microsoft-Dokumentation
description: Problembehandlung von Konfigurationen der eingeschränkten Kerberos-Delegierung für Anwendungsproxy
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2018
ms.author: markvi
ms.reviewer: harshja
ms.openlocfilehash: 3ba089123198631c443a759ad62cb0ae5ca40ad3
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/11/2018
ms.locfileid: "34068267"
---
# <a name="troubleshoot-kerberos-constrained-delegation-configurations-for-application-proxy"></a>Problembehandlung von Konfigurationen der eingeschränkten Kerberos-Delegierung für Anwendungsproxy

Die verfügbaren Methoden zum Erreichen von einmaligem Anmelden bei veröffentlichten Anwendungen können sich zwischen den verschiedenen Anwendungen leicht voneinander unterscheiden. Eine der Optionen, die im Azure-Anwendungsproxy standardmäßig verfügbar sind, ist die eingeschränkte Kerberos-Delegierung (KCD). Sie können einen Connector konfigurieren, um die eingeschränkte Kerberos-Authentifizierung für Back-End-Anwendungen im Auftrag von Benutzern durchzuführen.

Das eigentliche Verfahren zum Aktivieren von KCD ist relativ unkompliziert und erfordert in der Regel nur allgemeine Kenntnisse zu den verschiedenen Komponenten und zum Authentifizierungsablauf, der das einmalige Anmelden nutzt. Geeignete Informationsquellen, die bei der Problembehandlung für Szenarien helfen, in denen das einmalige Anmelden mithilfe der eingeschränkten Kerberos-Delegierung nicht erwartungsgemäß funktioniert, sind möglicherweise schwer zu finden.

Daher wird in diesem Artikel versucht, einen zentralen Anlaufpunkt bereitzustellen, der dabei helfen soll, einige der häufigsten Probleme selbst zu beheben. Gleichzeitig bietet er zusätzliche Hinweise zum Analysieren komplexerer und schwierigerer Implementierungen.

Dieser Artikel geht von den folgenden Annahmen aus:

-   Die Bereitstellung des Azure-Anwendungsproxys gemäß [Dokumentation](manage-apps/application-proxy-enable.md) und der allgemeine Zugriff auf Nicht-KCD-Anwendungen funktionieren wie erwartet.

-   Die veröffentlichte Zielanwendung basiert auf IIS und der Microsoft-Implementierung von Kerberos.

-   Die Server- und Anwendungshosts befinden sich in einer einzelnen Active Directory-Domäne. Ausführliche Informationen zu domänen- und gesamtstrukturübergreifenden Szenarien finden Sie im [KCD-Whitepaper](https://aka.ms/KCDPaper).

-   Die behandelte Anwendung wird in einem Azure-Mandanten mit aktivierter Vorauthentifizierung veröffentlicht. Benutzer müssen sich mithilfe der formularbasierten Authentifizierung bei Azure authentifizieren. In diesem Artikel werden keine Szenarien mit umfassender Clientauthentifizierung behandelt. Dies wird jedoch irgendwann in der Zukunft hinzugefügt.

## <a name="prerequisites"></a>Voraussetzungen

Der Azure-Anwendungsproxy kann in vielen Arten von Infrastruktur oder Umgebung bereitgestellt werden, und die Architekturen variieren zweifellos von Organisation zu Organisation. Eine der häufigsten Ursachen für KCD-bezogene Probleme sind nicht die Umgebungen selbst, sondern eher einfache Fehlkonfigurationen oder allgemeine Versehen.

Aus diesem Grund sollte vor Beginn der Problembehandlung zunächst immer überprüft werden, ob alle unter [Einmaliges Anmelden mit KCD mit dem Anwendungsproxy](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md) aufgeführten Voraussetzungen erfüllt sind.

Dies gilt Insbesondere für den Abschnitt zum Konfigurieren von KCD unter 2012R2, da dieser einen grundlegend anderen Ansatz für die KCD-Konfiguration als bei älteren Windows-Versionen verfolgt. Aber es müssen auch verschiedene andere Aspekte beachtet werden:

-   Es ist nicht ungewöhnlich, dass ein Domänenmitgliedsserver einen Dialog mit einem bestimmten Domänencontroller über einen sicheren Kanal öffnet. Wechseln Sie später zu einem beliebigen Zeitpunkt zu einem anderen Dialog, damit die Connectorhosts nicht darauf beschränkt sind, mit nur einem bestimmten Domänencontroller eines lokalen Standorts zu kommunizieren.

-   Ähnlich wie beim vorstehenden Punkt beruhen domänenübergreifende Szenarien auf Verweisen, die einen Connectorhost zu Domänencontrollern weiterleiten, die sich möglicherweise außerhalb des lokalen Netzwerkumkreises befinden. In diesem Szenario ist es ebenso wichtig sicherzustellen, dass der Datenverkehr zu denjenigen Domänencontrollern gestattet wird, die andere entsprechende Domänen repräsentieren. Andernfalls treten Delegierungsfehler auf.

-   Vermeiden Sie nach Möglichkeit das Platzieren aktiver IPS-/IDS-Geräte zwischen Connectorhosts und Domänencontrollern, da diese manchmal übermäßig stark eingreifen und mit dem RPC-Hauptdatenverkehr in Konflikt geraten.

Sie sollten die Delegierung im einfachsten Szenario testen. Je mehr Variablen Sie einführen, desto schwieriger kann sich der Vorgang gestalten. Wenn Sie Ihre Tests z. B. auf einen einzelnen Connector beschränken, kann dies wertvolle Zeit sparen. Nachdem die Probleme behoben wurden, können weitere Connectors hinzugefügt werden.

Ebenfalls tragen möglicherweise auch Umgebungsfaktoren zu einem Problem bei. Minimieren Sie während der Tests die Architektur auf das erforderliche Minimum, um diese Umgebungsfaktoren zu vermeiden. Beispielsweise sind falsch konfigurierte interne Firewall-ACLs nicht ungewöhnlich. Daher sollte sämtlicher Datenverkehr von einem Connector nach Möglichkeit direkt zu den Domänencontrollern und zur Back-End-Anwendung geleitet werden. 

Tatsächlich sollte ein Connector am besten so nah wie möglich beim Ziel platziert werden. Wenn sich während des Tests etwa eine Firewall im Übertragungsweg befindet, wird die Komplexität unnötig erhöht, und Ihre Untersuchungen nehmen möglicherweise mehr Zeit in Anspruch.

Was macht ein KCD-Problem somit aus? Es gibt verschiedene klassische Anzeichen, dass das einmalige Anmelden mit KCD fehlschlägt, und die ersten Anzeichen eines Problems äußern sich in der Regel im Browser.

   ![Fehler durch falsche KCD-Konfiguration](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic1.png)

   ![Fehler bei der Autorisierung aufgrund fehlender Berechtigungen](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic2.png)

Alle betreffen dasselbe Symptom eines Fehlers beim einmaligen Anmelden und der daraus folgenden Verweigerung des Benutzerzugriffs auf die Anwendung.

## <a name="troubleshooting"></a>Problembehandlung

Die Problembehandlung hängt vom Problem sowie den beobachteten Symptomen ab. Bevor wir fortfahren, sollten Sie sich mit den folgenden Links befassen, da diese hilfreiche Informationen enthalten, die Sie möglicherweise noch nicht kennen:

-   [Beheben von Problemen mit Anwendungsproxys und Fehlermeldungen](active-directory-application-proxy-troubleshoot.md)

-   [Kerberos-Fehler und die Symptome](active-directory-application-proxy-troubleshoot.md#kerberos-errors)

-   [Der Umgang mit SSO im lokalen Umfeld unterscheidet sich von dem mit Cloudidentitäten](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md#working-with-different-on-premises-and-cloud-identities)

Wenn Sie so weit gekommen sind, ist das Hauptproblem definitiv vorhanden. Unterteilen Sie den Ablauf für die Problembehandlung zunächst in drei separate Phasen.

**Clientvorauthentifizierung**: Der externe Benutzer authentifiziert sich über einen Browser bei Azure.

Die Möglichkeit zur Vorauthentifizierung bei Azure ist für das einmalige Anmelden mit KCD zwingend erforderlich. Sie sollten dies testen und ggf. auftretende Probleme beheben. Die Vorauthentifizierungsphase hat keinen Bezug zu KCD oder zur veröffentlichten Anwendung. Es sollte relativ einfach sein, mögliche Unstimmigkeiten zu beheben, indem Sie eine Plausibilitätsprüfung durchführen, ob das betroffene Konto in Azure vorhanden und es nicht deaktiviert/blockiert ist. Die Fehlerantwort im Browser ist in der Regel aussagekräftig genug, um die Ursache zu verstehen. Sie können auch unsere anderen Dokumentationen zur Problembehandlung zu Rate ziehen, falls Sie unsicher sind.

**Delegierungsdienst**: Der Azure-Proxy-Connector ruft im Auftrag von Benutzern ein Kerberos-Dienstticket aus einem KDC (Kerberos-Verteilungscenter) ab.

Die externe Kommunikation zwischen dem Client und dem Azure-Front-End sollte keinen anderen Bezug zur KCD haben als die Sicherstellung ihrer Funktionsweise. Dies dient dazu, dass der Azure-Proxydienst mit einer gültigen Benutzer-ID bereitgestellt werden kann, die zum Abrufen eines Kerberos-Tickets verwendet wird. Ohne diese ist KCD nicht möglich, und es würde ein Fehler auftreten.

Wie bereits erwähnt, bieten die Fehlermeldungen im Browser in der Regel einige geeignete Hinweise zu den Fehlerursachen. Stellen Sie sicher, dass Sie sich die Aktivitäts-ID und den Zeitstempel in der Antwort notieren, da diese es Ihnen ermöglichen, das Verhalten mit tatsächlichen Ereignissen im Ereignisprotokoll des Azure-Proxys zu korrelieren.

   ![Fehler durch falsche KCD-Konfiguration](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic3.png)

Die entsprechenden Einträge im Ereignisprotokoll hätten dann die Ereignis-IDs 13019 oder 12027. Die Ereignisprotokolle für den Connector befinden sich in **Anwendungs- und Dienstprotokolle** &gt; **Microsoft** &gt; **AadApplicationProxy** &gt; **Connector**&gt;**Admin**.

   ![Ereignis 13019 aus dem Ereignisprotokoll des Anwendungsproxys](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic4.png)

   ![Ereignis 12027 aus dem Ereignisprotokoll des Anwendungsproxys](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic5.png)

-   Verwenden Sie für die Adresse der Anwendung einen A-Datensatz in Ihrem internen DNS und keinen CNAME-Datensatz.

-   Vergewissern Sie sich erneut, dass dem Connectorhost die Rechte zum Delegieren an den SPN des festgelegten Zielkontos erteilt wurden und die Option **Beliebiges Authentifizierungsprotokoll verwenden** aktiviert ist. Weitere Informationen zu diesem Thema finden Sie im [Artikel zur SSO-Konfiguration](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md)

-   Stellen Sie sicher, dass nur eine einzige Instanz des SPN in AD vorhanden ist, indem Sie `setspn -x` über eine Eingabeaufforderung auf allen Domänenmitgliederhosts ausführen.

-   Überprüfen Sie, ob eine Domänenrichtlinie durchgesetzt wird, um die [maximale Größe der ausgestellten Kerberos-Token](https://blogs.technet.microsoft.com/askds/2012/09/12/maxtokensize-and-windows-8-and-windows-server-2012/) zu begrenzen, da dies den Connector daran hindert, ein Token abzurufen, wenn es als zu groß eingestuft wird.

Eine Netzwerkablaufverfolgung, die den Datenaustausch zwischen dem Connectorhost und einem Domänen-KDC erfasst, wäre dann der nächstbeste Schritt zum Abrufen weiterer allgemeiner Details zu den Problemen. Weitere Informationen finden Sie im [vertiefenden Papier zur Problembehandlung](https://aka.ms/proxytshootpaper).

Wenn die Ticketausstellung ordnungsgemäß funktioniert, sollte in den Protokollen ein Ereignis erscheinen, laut dem ein Authentifizierungsfehler aufgetreten ist, weil die Anwendung den Code 401 zurückgegeben hat. Dies deutet typischerweise darauf hin, dass die Zielanwendung Ihr Ticket ablehnt, fahren Sie deshalb mit der nächsten Phase fort:

**Zielanwendung**: Der Consumer des Kerberos-Tickets, das vom Connector bereitgestellt wurde.

In dieser Phase wird davon ausgegangen, dass der Connector bereits ein Kerberos-Dienstticket als Header in der ersten Anwendungsanforderung an das Back-End gesendet hat.

-   Überprüfen Sie mithilfe der internen URL der Anwendung, die im Portal definiert ist, dass direkt über den Browser auf dem Connectorhost auf die Anwendung zugegriffen werden kann. Dann können Sie sich erfolgreich anmelden. Details zur Vorgehensweise finden Sie auf der Connectorseite zur Problembehandlung.

-   Vergewissern Sie sich, während Sie sich weiterhin auf dem Connectorhost befinden, dass die Authentifizierung zwischen dem Browser und der Anwendung Kerberos verwendet, indem Sie eine der folgenden Aktionen ausführen:

1.  Führen Sie die Entwicklertools (**F12**) in Internet Explorer aus, oder verwenden Sie [Fiddler](https://blogs.msdn.microsoft.com/crminthefield/2012/10/10/using-fiddler-to-check-for-kerberos-auth/) auf dem Connectorhost. Wechseln Sie mithilfe der internen URL zur Anwendung, und überprüfen Sie die angebotenen WWW-Autorisierungsheader, die in der Antwort der Anwendung zurückgegeben wurden, um sicherzustellen, dass entweder eine Aushandlung oder Kerberos vorhanden ist. Ein nachfolgend in der Antwort des Browsers an die Anwendung zurückgegebenes Kerberos-Blob beginnt in der Regel mit **YII**. Dies ist daher ein geeigneter Hinweis darauf, dass Kerberos einbezogen ist. NTLM beginnt hingegen immer mit **TlRMTVNTUAAB**, das NTLMSSP lautet, wenn die Decodierung aus Base64 erfolgt ist. Wenn zu Beginn des Blobs **TlRMTVNTUAAB** angezeigt wird, bedeutet dies, dass Kerberos **nicht** verfügbar ist. Wenn es nicht angezeigt wird, ist Kerberos voraussichtlich verfügbar.
    > [!NOTE]
    > Bei der Verwendung von Fiddler würde diese Methode die vorübergehende Deaktivierung des erweiterten Schutzes für die Konfiguration der Anwendung in IIS erfordern.

     ![Browserfenster zur Netzwerküberprüfung](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic6.png)

    *Abbildung:* Da dies nicht mit TIRMTVNTUAAB beginnt, ist es ein Beispiel dafür, dass Kerberos verfügbar ist. Dies ist ein Beispiel für ein Kerberos-Blob, das nicht mit YII beginnt.

2.  Entfernen Sie NTLM vorübergehend aus der Anbieterliste der IIS-Website, und greifen Sie direkt über Internet Explorer auf dem Connectorhost auf die App zu. Ohne NTLM in der Anbieterliste sollten Sie in der Lage sein, nur mithilfe von Kerberos auf die Anwendung zuzugreifen. Wenn dies fehlschlägt, weist es darauf hin, dass ein Problem mit der Konfiguration der Anwendung vorliegt und die Kerberos-Authentifizierung nicht funktioniert.

Wenn Kerberos nicht verfügbar ist, prüfen Sie die Authentifizierungseinstellungen der Anwendung in IIS, um sicherzustellen, dass die Aushandlung ganz am Anfang und NTLM direkt darunter aufgeführt ist. (Keine Aushandlung: Kerberos oder Aushandlung: PKU2U). Setzen Sie den Vorgang nur fort, wenn Kerberos funktionsfähig ist.

   ![Windows-Authentifizierungsanbieter](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic7.png)
   
-   Wenn Kerberos und NTLM vorhanden sind, kann jetzt die Vorauthentifizierung für die Anwendung im Portal vorübergehend deaktiviert werden. Prüfen Sie, ob Sie über das Internet mithilfe der externen URL darauf zugreifen können. Sie sollten zur Authentifizierung aufgefordert werden und diese mit dem im vorherigen Schritt verwendeten Konto durchführen können. Ist dies nicht der Fall, so weist dies auf ein Problem mit der Back-End-Anwendung und nicht mit der KCD hin.

-   Jetzt aktivieren Sie die Vorauthentifizierung im Portal erneut, und authentifizieren Sie sich über Azure, indem Sie versuchen, über die externe URL eine Verbindung mit der Anwendung herzustellen. Wenn beim einmaligen Anmelden ein Fehler aufgetreten ist, sollte im Browser eine Fehlermeldung zu einem verbotenen Vorgang sowie das Ereignis 13022 im Protokoll angezeigt werden:

    *Microsoft AAD-Anwendungsproxy-Connector kann den Benutzer nicht authentifizieren, da der Back-End-Server auf Kerberos-Authentifizierungsversuche mit einem HTTP 401-Fehler reagiert.*

    ![HTTP-Fehler 401: Verboten](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic8.png)

-   Überprüfen Sie die IIS-Anwendung, um sicherzustellen, dass der konfigurierte Anwendungspool für die Verwendung desselben Kontos konfiguriert ist, für das der SPN in AD konfiguriert wurde, indem Sie in IIS navigieren, wie in der folgenden Abbildung zu sehen.

    ![Fenster zur IIS-Anwendungskonfiguration](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic9.png)

    Nachdem Sie die Identität kennen, führen Sie den folgenden Befehl über eine Eingabeaufforderung aus, um sicherzustellen, dass dieses Konto definitiv mit dem betreffenden SPN konfiguriert ist. Beispiel: `setspn –q http/spn.wacketywack.com`

    ![SetSPN-Befehlsfenster](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic10.png)

-   Überprüfen Sie, ob der für die Einstellungen der Anwendung definierte SPN im Portal derselbe SPN ist, der für das AD-Zielkonto konfiguriert ist, das vom Anwendungspool der Anwendung verwendet wird.

   ![SPN-Konfiguration im Azure-Portal](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic11.png)
   
-   Wechseln Sie zu IIS, wählen Sie die Option **Konfigurations-Editor** für die Anwendung aus, und navigieren Sie zu **system.webServer/security/authentication/windowsAuthentication**, um sicherzustellen, dass für **UseAppPoolCredentials** der Wert **True** festgelegt ist.

   ![Option für Anmeldeinformationen der Anwendungspools der IIS-Konfiguration](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic12.png)

Nach dem Ändern dieses Werts in **True** müssen alle zwischengespeicherten Kerberos-Tickets vom Back-End-Server entfernt werden. Führen Sie dazu den folgenden Befehl aus:

```powershell
Get-WmiObject Win32_LogonSession | Where-Object {$_.AuthenticationPackage -ne 'NTLM'} | ForEach-Object {klist.exe purge -li ([Convert]::ToString($_.LogonId, 16))}
``` 

Weitere Informationen finden Sie unter [Purge the Kerberos client ticket cache for all sessions](https://gallery.technet.microsoft.com/scriptcenter/Purge-the-Kerberos-client-b56987bf) (Löschen des Kerberos-Clientticketcaches für alle Sitzungen).



Obwohl es beim Optimieren der Leistung von Kerberos-Vorgängen hilfreich ist, wird das Ticket für den angeforderten Dienst mithilfe des Computerkontos entschlüsselt, wenn der Kernel-Modus aktiviert bleibt. Dies wird auch als lokales System bezeichnet. Wenn daher der Wert „true“ festgelegt ist, wird die KCD gestoppt, wenn die Anwendung auf mehreren Servern in einer Farm gehostet wird.

-   Als zusätzliche Überprüfung können Sie auch den **erweiterten** Schutz deaktivieren. Es sind Szenarien aufgetreten, in denen dies erwiesenermaßen die KCD gestoppt hat, wenn es in spezifischen Konfigurationen aktiviert wurde, in denen eine Anwendung als untergeordneter Ordner der Standardwebsite veröffentlicht wurde. Dies ist selbst nur für die anonyme Authentifizierung konfiguriert. Sämtliche Dialogfelder sind abgeblendet dargestellt und legen nahe, dass untergeordnete Objekte keine aktiven Einstellungen erben. Nach Möglichkeit sollte dies aber immer aktiviert sein. Testen Sie daher mit allen Ihnen zur Verfügung stehenden Mitteln, aber vergessen Sie nicht, dies anschließend wieder zu aktivieren.

Diese zusätzlichen Überprüfungen sollten Sie für die Verwendung Ihrer veröffentlichten Anwendung auf Kurs gebracht haben. Sie können fortfahren und weitere Connectors aktivieren, die ebenfalls als Stellvertreter konfiguriert sind, aber wenn es keine weiteren Entwicklungen gibt, würden wir empfehlen, unsere ausführlichere technische exemplarische Vorgehensweise [Vollständige Anleitung für die Problembehandlung beim Azure AD-Anwendungsproxy](https://aka.ms/proxytshootpaper) zu lesen.

Wenn Sie bei Ihrem Problem weiterhin nicht vorankommen, können Sie sich an den Support wenden, der Ihnen hier weiterhelfen kann. Erstellen Sie direkt im Portal ein Supportticket (ein Techniker wird sich dann bei Ihnen melden).

## <a name="other-scenarios"></a>Andere Szenarien

-   Der Azure-Anwendungsproxy fordert vor dem Senden der Anforderung an eine Anwendung ein Kerberos-Ticket an. Einigen Anwendungen von Drittanbietern, z. B. Tableau Server, gefällt diese Methode der Authentifizierung nicht. Stattdessen erwarten sie die Durchführung der konventionelleren Aushandlungen. Die erste Anforderung ist anonym, wodurch es der Anwendung ermöglicht wird, mit den Authentifizierungstypen zu antworten, die sie über eine 401-Meldung unterstützt.

-   Double-Hop-Authentifizierung: Häufig in Szenarien verwendet, in denen eine Anwendung abgestuft wird, mit einem Back-End und Front-End, die beide eine Authentifizierung erfordern, z. B. SQL Reporting Services.

## <a name="next-steps"></a>Nächste Schritte
[Konfigurieren der eingeschränkten Kerberos-Delegierung (KCD) in einer verwalteten Domäne](../active-directory-domain-services/active-directory-ds-enable-kcd.md)
