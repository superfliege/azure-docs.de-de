---
title: Verwenden von Azure AD Connect Health mit AD FS | Microsoft-Dokumentation
description: Dies ist die Azure AD Connect Health-Seite, auf der Sie erfahren, wie Sie Ihre lokale AD FS-Infrastruktur überwachen.
services: active-directory
documentationcenter: ''
author: zhiweiwangmsft
manager: mtillman
editor: curtand
ms.assetid: dc0e53d8-403e-462a-9543-164eaa7dd8b3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/26/2018
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7d93207e6a5f0acabcf348981e799e801c39f48b
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2018
ms.locfileid: "51278836"
---
# <a name="monitor-ad-fs-using-azure-ad-connect-health"></a>Überwachen von AD FS mithilfe von Azure AD Connect Health
Die folgende Dokumentation bezieht sich auf die Überwachung Ihrer AD FS-Infrastruktur mit Azure AD Connect Health. Informationen zum Überwachen von Azure AD Connect (Sync) mit Azure AD Connect Health finden Sie unter [Verwenden von Azure AD Connect Health für die Synchronisierung](how-to-connect-health-sync.md). Informationen zur Überwachung der Active Directory-Domänendienste mit Azure AD Connect Health finden Sie unter [Verwenden von Azure AD Connect Health mit AD DS](how-to-connect-health-adds.md).

## <a name="alerts-for-ad-fs"></a>Warnungen für AD FS
Im Abschnitt "Azure AD Connect Health-Warnungen" wird eine Liste der aktiven Warnungen angezeigt. Jede Warnung umfasst relevante Informationen, Lösungsschritte und Links zur verwandten Dokumentation.

Sie können auf eine aktive oder gelöste Warnung doppelklicken, um ein neues Blatt mit weiteren Informationen, Schritten zum Lösen der Warnung und Links zu relevanter Dokumentation zu öffnen. Sie können außerdem Verlaufsdaten zu bereits behobenen Warnungen anzeigen.

![Azure AD Connect Health-Portal](./media/how-to-connect-health-adfs/alert2.png)

## <a name="usage-analytics-for-ad-fs"></a>Nutzungsanalyse für AD FS
In der Azure AD Connect Health-Nutzungsanalyse wird der Authentifizierungsdatenverkehr für Ihre Verbundserver analysiert. Sie können auf das Feld „Nutzungsanalyse“ doppelklicken, um das Blatt „Nutzungsanalyse“ zu öffnen, auf dem mehrere Metriken und Gruppierungen angezeigt werden.

> [!NOTE]
> Um die Nutzungsanalyse mit AD FS verwenden zu können, müssen Sie sicherstellen, dass die AD FS-Überwachung aktiviert ist. Weitere Informationen finden Sie unter [Aktivieren der Überwachung für AD FS](how-to-connect-health-agent-install.md#enable-auditing-for-ad-fs).
>
>

![Azure AD Connect Health-Portal](./media/how-to-connect-health-adfs/report1.png)

Um zusätzliche Metriken auszuwählen, einen Zeitraum anzugeben oder die Gruppierung zu ändern, klicken Sie mit der rechten Maustaste auf das Diagramm zur Nutzungsanalyse und wählen „Diagramm bearbeiten“. Anschließend können Sie den Zeitraum angeben, eine andere Metrik auswählen und die Gruppierung ändern. Sie können die Verteilung des Authentifizierungsdatenverkehrs basierend auf verschiedenen Metriken anzeigen und jede Metrik über die relevanten Parameter gruppieren. Diese Parameter werden im folgenden Abschnitt beschrieben:

**Metrik : Anforderungen insgesamt**: Gibt die Gesamtzahl von Anforderungen an, die von AD FS-Servern verarbeitet wurden.

|Gruppieren nach | Bedeutung und Nutzen der Gruppierung |
| --- | --- |
| Alle | Zeigt die Gesamtzahl von Anforderungen an, die von allen AD FS-Servern verarbeitet wurden.|
| Anwendung | Gruppiert die Gesamtzahl von Anforderungen basierend auf der Zielanwendung (vertrauende Seite). Diese Gruppierung ist nützlich, wenn Sie die prozentuale Verteilung des gesamten Datenverkehrs auf die verschiedenen Anwendungen anzeigen möchten. |
|  Server |Gruppiert die Gesamtzahl von Anforderungen basierend auf dem Server, der die Anforderung verarbeitet hat. Diese Gruppierung ist nützlich, um die Lastverteilung für den gesamten Datenverkehr anzuzeigen.
| In den Arbeitsplatz eingebunden |Gruppiert die Gesamtzahl von Anforderungen danach, ob sie von Geräten stammen, die mit einem Arbeitsbereich verknüpft (bekannt) sind. Diese Gruppierung ist nützlich, wenn auf Ihre Ressourcen über Geräte zugegriffen wird, die der Identitätsinfrastruktur unbekannt sind. |
|  Authentifizierungsmethode | Gruppiert die Gesamtzahl von Anforderungen basierend auf der Authentifizierungsmethode. Diese Gruppierung ist nützlich, wenn Sie wissen möchten, welche Authentifizierungsmethode in der Regel für die Authentifizierung verwendet wird. Folgende Authentifizierungsmethoden sind möglich:  <ol> <li>Integrierte Windows-Authentifizierung (Windows)</li> <li>Formularbasierte Authentifizierung (Forms)</li> <li>Einmaliges Anmelden (SSO)</li> <li>X509-Zertifikatauthentifizierung (Zertifikat)</li> <br>Wenn die Verbundserver die Anforderung mit einem SSO-Cookie empfangen, wird diese Anforderung als SSO-Anforderung eingestuft. In diesen Fällen wird, sofern es sich um ein gültiges Cookie handelt, der Benutzer nicht zur Eingabe von Anmeldeinformationen aufgefordert, sondern erhält nahtlos Zugriff auf die Anwendung. Dieses Verhalten ist üblich, wenn mehrere vertrauende Seiten von den Verbundservern geschützt werden. |
| Netzwerkadresse | Gruppiert die Gesamtzahl von Anforderungen basierend auf der Netzwerkadresse des Benutzers. Dies kann entweder ein Intranet oder Extranet sein. Diese Gruppierung ist nützlich, wenn Sie die prozentuale Verteilung des eingehenden Datenverkehrs für Intranet und Extranet anzeigen möchten. |


**Metrik: Anforderungen mit Fehlern gesamt**: Die Gesamtzahl von Anforderungen mit Fehlern, die vom Verbunddienst verarbeitet wurden. (Diese Metrik steht nur in AD FS für Windows Server 2012 R2 zur Verfügung.)

|Gruppieren nach | Bedeutung und Nutzen der Gruppierung |
| --- | --- |
| Fehlertyp | Zeigt die Anzahl von Fehlern basierend auf den vordefinierten Fehlertypen an. Diese Gruppierung ist nützlich, um die allgemeinen Arten von Fehlern zu verstehen. <ul><li>Falscher Benutzername oder falsches Kennwort: Fehler, die durch einen falschen Benutzernamen oder ein falsches Kennwort ausgelöst werden.</li> <li>„Extranetsperrung“: Fehler durch Anforderungen von einem Benutzer, der für das Extranet gesperrt wurde. </li><li> „Kennwort abgelaufen“: Fehler durch Benutzer, die sich mit einem abgelaufenen Kennwort anmelden.</li><li>„Konto deaktiviert“: Fehler durch Benutzer, die sich mit einem deaktivierten Konto anmelden.</li><li>„Authentifizierung mit Gerät“: Fehler durch Benutzer, bei denen Probleme bei der Geräteauthentifizierung auftreten.</li><li>„Authentifizierung mit Benutzerzertifikat“: Fehler durch Benutzer, bei denen die Authentifizierung aufgrund eines ungültigen Zertifikats nicht möglich ist.</li><li>„MFA“: Fehler durch Benutzer, die sich nicht per Multi-Factor Authentication authentifizieren können.</li><li>„Andere Anmeldeinformationen“: „Ausstellungsautorisierung“: Fehler aufgrund von Autorisierungsproblemen.</li><li>„Ausstellungsdelegierung“: Fehler aufgrund von Problemen bei der Ausstellungsdelegierung.</li><li>„Tokenannahme“: Fehler durch das Ablehnen von externen Identitätsanbietern durch AD FS.</li><li>„Protokoll“: Fehler aufgrund von Protokollproblemen.</li><li>„Unbekannt“: Fängt alle weiteren Fehler ab. Alle anderen Fehler, die nicht in die definierten Kategorien fallen.</li> |
| Server | Gruppiert die Fehler basierend auf dem Server. Diese Gruppierung ist nützlich, um die Fehlerverteilung auf die Server anzuzeigen. Eine ungleichmäßige Verteilung könnte darauf hinweisen, dass sich ein Server in fehlerhaftem Zustand befindet. |
| Netzwerkadresse | Gruppiert die Fehler basierend auf der Netzwerkadresse der Anforderungen (Intranet oder Extranet). Diese Gruppierung ist nützlich, um die Art der Anforderungen herauszufinden, für die Fehler auftreten. |
|  Anwendung | Gruppiert die Fehler basierend auf der Zielanwendung (vertrauende Seite). Mithilfe dieser Gruppierung können Sie anzeigen, für welche Zielanwendung die meisten Fehler auftreten. |

**Metrik: Benutzeranzahl**: Durchschnittliche Anzahl von eindeutigen Benutzern, die aktiv eine Authentifizierung mit AD FS durchführen.

|Gruppieren nach | Bedeutung und Nutzen der Gruppierung |
| --- | --- |
|Alle |Mit dieser Metrik wird die durchschnittliche Anzahl von Benutzern bereitgestellt, die den Verbunddienst im ausgewählten Zeitraum verwendet haben. Die Benutzer werden nicht gruppiert. <br>Die durchschnittliche Anzahl richtet sich nach dem ausgewählten Zeitraum. |
| Anwendung |Gruppiert die durchschnittliche Anzahl von Benutzern basierend auf der Zielanwendung (vertrauende Seite). Über diese Gruppierung können Sie anzeigen, wie viele Benutzer welche Anwendung nutzen. |

## <a name="performance-monitoring-for-ad-fs"></a>Leistungsüberwachung für AD FS
Die Azure AD Connect Health-Leistungsüberwachung liefert Überwachungsinformationen zu verschiedenen Metriken. Wenn Sie das Kontrollkästchen aktivieren, wird ein neues Blatt mit ausführlichen Informationen zu den Metriken geöffnet.

![Azure AD Connect Health-Portal](./media/how-to-connect-health-adfs/perf1.png)

Wenn Sie die Filteroption oben im Blatt auswählen, können Sie eine Filterung nach Server vornehmen, um Metriken zu einzelnen Servern anzuzeigen. Um Metriken zu ändern, klicken Sie mit der rechten Maustaste auf das Überwachungsdiagramm unterhalb des Überwachungsblatts und klicken dann auf „Diagramm bearbeiten“ (oder wählen Sie die Schaltfläche „Diagramm bearbeiten“). Auf dem dann geöffneten neuen Blatt können Sie in der Dropdownliste zusätzliche Metriken auswählen und einen Zeitbereich für die Anzeige von Leistungsdaten angeben.

## <a name="top-50-users-with-failed-usernamepassword-logins"></a>50 Benutzer mit den meisten fehlgeschlagenen Anmeldungen aufgrund eines falschen Benutzernamens/Kennworts
Einer der häufigsten Gründe für das Fehlschlagen von Authentifizierungsanforderungen bei einem AD FS-Server ist die Verwendung ungültiger Anmeldeinformationen, d. h. eines falschen Benutzernamens oder Kennworts. Dies passiert Benutzern normalerweise aufgrund von komplexen Kennwörtern, vergessenen Kennwörtern oder Schreibfehlern.

Aber es gibt noch weitere Gründe, die zu einer unerwartet hohen Anzahl von derartigen Anforderungen bei Ihren AD FS-Servern führen können, z.B. von einer Anwendung zwischengespeicherte abgelaufene Benutzeranmeldeinformationen oder ein böswilliger Benutzer, der versucht, sich mit einer Reihe von bekannten Kennwörtern bei einem Konto anzumelden. Diese beiden Beispiele sind plausible Gründe für einen Anstieg der Anforderungsanzahl.

Azure AD Connect Health für AD FS bietet einen Bericht über die 50 Benutzer, bei denen Anmeldeversuche am häufigsten aufgrund ungültiger Benutzernamen oder Kennwörter fehlschlagen. Zur Erstellung dieses Berichts werden die von allen AD FS-Servern in den Farmen generierten Überwachungsereignisse verarbeitet.

![Azure AD Connect Health-Portal](./media/how-to-connect-health-adfs/report1a.png)

In diesem Bericht können Sie ganz einfach auf die folgenden Informationen zugreifen:

* Gesamtanzahl fehlgeschlagener Anforderungen mit falschem Benutzernamen/Kennwort in den letzten 30 Tagen
* Durchschnittliche Anzahl von Benutzern pro Tag, bei denen die Anmeldung wegen eines ungültigen Benutzernamens/Kennworts fehlschlägt.

Wenn Sie auf diesen Bereich klicken, gelangen Sie zum Hauptberichtblatt, das zusätzliche Details enthält. Dieses Blatt enthält einen Graphen mit Trendinformationen zum Ermitteln einer Baseline für Anforderungen mit falschem Benutzernamen oder Kennwort. Außerdem wird die Liste mit den 50 Benutzern mit den meisten Fehlversuchen während der letzten Woche angegeben. Beachten Sie, dass die 50 Benutzer der letzten Woche zur Identifizierung von Spitzenwerten bei den fehlerhaften Kennwörtern beitragen können.  

Der Graph zeigt die folgenden Informationen:

* Gesamtanzahl fehlgeschlagener Anmeldeversuche aufgrund eines ungültigen Benutzernamens/Kennworts pro Tag
* Gesamtanzahl eindeutiger Benutzer, bei denen die Anmeldung fehlgeschlagen ist, pro Tag.
* Client-IP-Adresse der letzten Anforderung

![Azure AD Connect Health-Portal](./media/how-to-connect-health-adfs/report3a.png)

Der Bericht enthält die folgenden Informationen:

| Berichtselement | BESCHREIBUNG |
| --- | --- |
| Benutzer-ID |Die verwendete Benutzer-ID. Dies ist der Wert mit der Eingabe des Benutzers, also in einigen Fällen die Verwendung der falschen Benutzer-ID. |
| Fehlgeschlagene Anmeldeversuche |Die Gesamtanzahl fehlgeschlagener Anmeldeversuche für die jeweilige Benutzer-ID. Die Tabelle ist nach der höchsten Anzahl fehlgeschlagener Versuche in absteigender Reihenfolge sortiert. |
| Letzter Fehler |Zeigt den Zeitstempel des letzten Fehlers an. |
| Letzte Fehler-IP-Adresse |Zeigt die Client-IP-Adresse der letzten fehlerhaften Anforderung an. Sollten für diesen Wert mehrere IP-Adressen angezeigt werden, enthält er möglicherweise die Forward-Client-IP-Adresse sowie die IP-Adresse des letzten Anforderungsversuchs des Benutzers.  |

> [!NOTE]
> Der Bericht wird automatisch alle zwölf Stunden mit den neuen gesammelten Informationen aktualisiert. Anmeldeversuche innerhalb der letzten 12 Stunden sind daher unter Umständen nicht im Bericht enthalten.
>
>

## <a name="risky-ip-report-public-preview"></a>Bericht über riskante IP-Adressen (öffentliche Vorschauversion)
AD FS-Kunden können Endpunkte für die Kennwortauthentifizierung für den Zugriff über das Internet verfügbar machen, um Authentifizierungsdienste für Endbenutzer bereitzustellen, damit diese auf SaaS-Anwendungen wie Office 365 zugreifen können. In diesem Fall ist es möglich, dass ein böswilliger Benutzer versucht, sich an Ihrem AD FS-System anzumelden, um das Kennwort eines Endbenutzers zu erraten und Zugriff auf Anwendungsressourcen zu erhalten. AD FS verfügt seit der Einbindung in Windows Server 2012 R2 über die Funktion zum Sperren von Extranet-Konten, um diese Arten von Angriffen zu verhindern. Falls Sie eine frühere Version verwenden, empfehlen wir Ihnen dringend, Ihr AD FS-System auf Windows Server 2016 zu aktualisieren. <br />
Außerdem ist es möglich, dass von einer einzelnen IP-Adresse versucht wird, mehrere Anmeldungen für mehrere Benutzer durchzuführen. In diesen Fällen kann es sein, dass die Anzahl von Versuchen pro Benutzer unter dem Schwellenwert für den Schutz durch Kontosperrung in AD FS liegt. Azure AD Connect Health enthält jetzt einen „Bericht über riskante IP-Adressen“, mit dem dieser Zustand erkannt wird und Administratoren benachrichtigt werden. Hier sind die wichtigsten Vorteile dieses Berichts angegeben: 
- Erkennung von IP-Adressen, für die ein Schwellenwert für fehlgeschlagene kennwortbasierte Anmeldungen überschritten wird
- Unterstützung von fehlgeschlagenen Anmeldungen aufgrund eines fehlerhaften Kennworts oder eines Extranet-Sperrzustands
- Sofortige E-Mail-Benachrichtigung als Warnung für Administratoren nach dem Eintreten mit anpassbaren E-Mail-Einstellungen
- Anpassbare Einstellungen für den Schwellenwert, die der Sicherheitsrichtlinie einer Organisation entsprechen
- Herunterladbare Berichte für Offlineanalyse und Integration in andere Systeme per Automatisierung

> [!NOTE]
> Für die Verwendung dieses Berichts muss die AD FS-Überwachung aktiviert sein. Weitere Informationen finden Sie unter [Aktivieren der Überwachung für AD FS](how-to-connect-health-agent-install.md#enable-auditing-for-ad-fs). <br />
> Zugriff auf die Vorschauversion erhalten nur globale Administratoren und [Benutzer mit Leseberechtigung für Sicherheitsfunktionen](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#security-reader).  
> 

### <a name="what-is-in-the-report"></a>Inhalt des Berichts
Jeder Eintrag im Bericht über riskante IP-Adressen enthält aggregierte Informationen zu fehlgeschlagenen AD FS-Anmeldeaktivitäten, für die der angegebene Schwellenwert überschritten wurde. Folgende Informationen werden angegeben: ![Azure AD Connect Health-Portal](./media/how-to-connect-health-adfs/report4a.png)

| Berichtselement | BESCHREIBUNG |
| ------- | ----------- |
| Zeitstempel | Zeigt den Zeitstempel basierend auf der lokalen Zeit im Azure-Portal an, wenn das Erkennungszeitfenster beginnt.<br /> Alle täglichen Ereignisse werden um Mitternacht (UTC) generiert. <br />Für stündliche Ereignisse wird der Zeitstempel auf den Anfang der Stunde gerundet. Sie finden die Startzeit der ersten Aktivität unter „firstAuditTimestamp“ in der exportierten Datei. |
| Triggertyp | Zeigt den Typ des Erkennungszeitfensters an. Die Triggertypen der Aggregation sind „Pro Stunde“ und „Pro Tag“. Dies ist hilfreich beim Erkennen eines Brute-Force-Angriffs mit hoher Häufigkeit gegenüber einem langsamen Angriff, bei dem sich die Versuche auf den gesamten Tag verteilen. |
| IP-Adresse | Die einzelne riskante IP-Adresse, für die es entweder zu Anmeldeaktivitäten mit einem fehlerhaften Kennwort oder einer Extranetsperrung gekommen ist. Dies kann eine IPv4- oder eine IPv6-Adresse sein. |
| Fehleranzahl aufgrund eines falschen Kennworts | Die Anzahl von Fehlern der Art „Falsches Kennwort“, die für die IP-Adresse während des Erkennungszeitfensters aufgetreten sind. Fehler der Art „Falsches Kennwort“ können für bestimmte Benutzer mehrfach auftreten. Beachten Sie, dass hierin keine Fehlversuche aufgrund von abgelaufenen Kennwörtern enthalten sind. |
| Fehleranzahl aufgrund einer Extranetsperre | Die Anzahl von Fehlern der Art „Extranetsperre“, die für die IP-Adresse während des Erkennungszeitfensters aufgetreten sind. Die Fehler der Art „Extranetsperre“ können für bestimmte Benutzer mehrfach auftreten. Dies ist nur der Fall, wenn in AD FS (Versionen 2012 R2 oder höher) die Extranetsperre konfiguriert wurde. <b>Hinweis</b> Wir empfehlen Ihnen dringend, dieses Feature zu aktivieren, wenn Sie Extranetanmeldungen mit Kennwörtern zulassen. |
| Eindeutiger Benutzername bei Versuch | Die Anzahl von eindeutigen Benutzerkonten, mit denen über die IP-Adresse während des Erkennungszeitfensters Versuche unternommen wurden. Hiermit kann unterschieden werden, ob es sich um einen Angriff eines einzelnen Benutzers oder von mehreren Benutzern handelt.  |

Im Berichtseintrag unten ist beispielsweise angegeben, dass für die IP-Adresse <i>104.2XX.2XX.9</i> am 28.02.2018 in der Stunde zwischen 18 und 19 Uhr keine Fehler der Art „Falsches Kennwort“ und 284 Fehler der Art „Extranetsperre“ aufgetreten sind. Von diesen Vorgängen waren 14 eindeutige Benutzer betroffen. Für das Aktivitätsereignis wurde der angegebene Stundenschwellenwert im Bericht überschritten. 

![Azure AD Connect Health-Portal](./media/how-to-connect-health-adfs/report4b.png)

> [!NOTE]
> - In der Berichtsliste werden nur Aktivitäten angezeigt, für die der festgelegte Schwellenwert überschritten wird. 
> - Mit diesem Bericht ist die Zurückverfolgung für maximal 30 Tage möglich.
> - In diesem Warnbericht werden keine Exchange-IP-Adressen oder privaten IP-Adressen angezeigt. Sie sind weiterhin in der Exportliste enthalten. 
>

![Azure AD Connect Health-Portal](./media/how-to-connect-health-adfs/report4c.png)

### <a name="load-balancer-ip-addresses-in-the-list"></a>Load Balancer-IP-Adressen in der Liste
Bei Anmeldeaktivitäten der Load Balancer-Aggregation ist ein Fehler aufgetreten, und der Warnungsschwellenwert wurde erreicht. Falls IP-Adressen des Lastenausgleichs angezeigt werden, sendet Ihr externer Lastenausgleich höchstwahrscheinlich die Client-IP-Adresse nicht, wenn er die Anforderung an den Webanwendungsproxy-Server übergibt. Konfigurieren Sie Ihren Lastenausgleich so, dass Forward-Client-IP-Adressen ordnungsgemäß weitergegeben werden. 

### <a name="download-risky-ip-report"></a>Herunterladen des Berichts über riskante IP-Adressen 
Mit der Funktion zum **Herunterladen** kann der gesamte Bericht über riskante IP-Adressen der letzten 30 Tage aus dem Connect Health-Portal kopiert werden. Das Exportergebnis enthält alle fehlgeschlagenen AD FS-Anmeldeaktivitäten jedes Erkennungszeitfensters, damit Sie den Filtervorgang nach dem Export anpassen können. Zusätzlich zu den hervorgehobenen Aggregationen im Portal werden im Exportergebnis auch weitere Details zu fehlgeschlagenen Anmeldeaktivitäten pro IP-Adresse angezeigt:

|  Berichtselement  |  BESCHREIBUNG  | 
| ------- | ----------- | 
| firstAuditTimestamp | Zeigt den ersten Zeitstempel für den Zeitpunkt an, zu dem die fehlgeschlagenen Aktivitäten während des Erkennungszeitfensters begonnen haben.  | 
| lastAuditTimestamp | Zeigt den letzten Zeitstempel für den Zeitpunkt an, zu dem die fehlgeschlagenen Aktivitäten während des Erkennungszeitfensters geendet haben.  | 
| attemptCountThresholdIsExceeded | Dieses Flag wird angezeigt, wenn die aktuellen Aktivitäten den Schwellenwert für die Warnung überschreiten.  | 
| isWhitelistedIpAddress | Dieses Flag wird angezeigt, wenn die IP-Adresse für Warnungen und die Berichterstellung gefiltert wird. Private IP-Adressen (<i>10.x.x.x, 172.x.x.x und 192.168.x.x</i>) und Exchange-IP-Adressen werden gefiltert und als „True“ gekennzeichnet. Wenn Bereiche mit privaten IP-Adressen angezeigt werden, ist die Wahrscheinlichkeit hoch, dass Ihr externer Lastenausgleich die Client-IP-Adresse beim Übergeben der Anforderung an den Webanwendungsproxy-Server nicht sendet.  | 

### <a name="configure-notification-settings"></a>Konfigurieren von Benachrichtigungseinstellungen
Administratorkontakte des Berichts können über die **Benachrichtigungseinstellungen** aktualisiert werden. Standardmäßig ist die E-Mail-Benachrichtigung zur Warnung vor riskanten IP-Adressen nicht aktiviert. Sie können die Benachrichtigung aktivieren, indem Sie die Schaltfläche unter „E-Mail-Benachrichtigungen für den Bericht zu IP-Adressen erhalten, die den Schwellenwert für fehlerhafte Aktivitäten überschreiten“ aktivieren. Wie bei den generischen Einstellungen für Warnungsbenachrichtigungen in Connect Health auch, können Sie hier die angegebene Liste mit den Benachrichtigungsempfängern für den Bericht über riskante IP-Adressen anpassen. Sie können beim Vornehmen der Änderung auch alle globalen Administratoren benachrichtigen. 

### <a name="configure-threshold-settings"></a>Konfigurieren von Schwellenwerteinstellungen
Der Schwellenwert für Warnungen kann über die „Schwellenwerteinstellungen“ aktualisiert werden. Zu Beginn ist der Schwellenwert im System standardmäßig festgelegt. Die Schwellenwerteinstellungen für den Bericht über riskante IP-Adressen enthalten vier Kategorien:

![Azure AD Connect Health-Portal](./media/how-to-connect-health-adfs/report4d.png)

| Schwellenwertelement | BESCHREIBUNG |
| --- | --- |
| (Benutzername/Kennwort ungültig + Extranetsperre)/Tag  | Schwellenwerteinstellung zum Melden der Aktivität und Auslösen der Warnungsbenachrichtigung, wenn die Anzahl von „Falsches Kennwort“ zusammen mit der Anzahl von Extranetsperren den Schwellenwert pro **Tag** überschreitet. |
| (Benutzername/Kennwort ungültig + Extranetsperre)/Stunde | Schwellenwerteinstellung zum Melden der Aktivität und Auslösen der Warnungsbenachrichtigung, wenn die Anzahl von „Falsches Kennwort“ zusammen mit der Anzahl von Extranetsperren den Schwellenwert pro **Stunde** überschreitet. |
| Extranetsperre/Tag | Schwellenwerteinstellung zum Melden der Aktivität und Auslösen der Warnungsbenachrichtigung, wenn die Anzahl von Extranetsperren den Schwellenwert pro **Tag** überschreitet. |
| Extranetsperre/Stunde| Schwellenwerteinstellung zum Melden der Aktivität und Auslösen der Warnungsbenachrichtigung, wenn die Anzahl von Extranetsperren den Schwellenwert pro **Stunde** überschreitet. |

> [!NOTE]
> - Die Änderung des Berichtsschwellenwerts wird eine Stunde nach der Einstellungsänderung angewendet. 
> - Vorhandene gemeldete Elemente sind von der Schwellenwertänderung nicht betroffen. 
> - Wir empfehlen Ihnen, die Anzahl von Ereignissen in Ihrer Umgebung zu analysieren und den Schwellenwert entsprechend anzupassen. 
>
>

### <a name="faq"></a>Häufig gestellte Fragen
1. Warum werden im Bericht Bereiche mit privaten IP-Adressen angezeigt?  <br />
Private IP-Adressen (<i>10.x.x.x, 172.x.x.x und 192.168.x.x</i>) und Exchange-IP-Adressen werden gefiltert und in der IP-Whitelist als „True“ gekennzeichnet. Wenn Bereiche mit privaten IP-Adressen angezeigt werden, ist die Wahrscheinlichkeit hoch, dass Ihr externer Lastenausgleich die Client-IP-Adresse beim Übergeben der Anforderung an den Webanwendungsproxy-Server nicht sendet.

2. Warum enthält der Bericht IP-Adressen des Lastenausgleichs?  <br />
Falls IP-Adressen des Lastenausgleichs angezeigt werden, sendet Ihr externer Lastenausgleich höchstwahrscheinlich die Client-IP-Adresse nicht, wenn er die Anforderung an den Webanwendungsproxy-Server übergibt. Konfigurieren Sie Ihren Lastenausgleich so, dass Forward-Client-IP-Adressen ordnungsgemäß weitergegeben werden. 

3. Wie kann ich die IP-Adresse blockieren?  <br />
Sie sollten identifizierte schädliche IP-Adressen der Firewall hinzufügen oder in Exchange blockieren.   <br />

4. Warum werden in diesem Bericht keine Elemente angezeigt? <br />
   - Es liegen keine fehlgeschlagenen Anmeldeaktivitäten vor, für die die Schwellenwerteinstellungen überschritten werden. 
   - Stellen Sie sicher, dass in Ihrer AD FS-Serverliste keine Warnung vom Typ „Integritätsdienst ist nicht aktuell” aktiv ist.  Informieren Sie sich über das [Durchführen der Problembehandlung für diese Warnung](how-to-connect-health-data-freshness.md).
   - Überwachungen sind in AD FS-Farmen nicht aktiviert.
 
5. Warum habe ich keinen Zugriff auf den Bericht?  <br />
Zugriff erhalten nur globale Administratoren und [Benutzer mit Leseberechtigung für Sicherheitsfunktionen](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#security-reader). Wenden Sie sich an den globalen Administrator aus, um Zugriff zu erhalten.


## <a name="related-links"></a>Verwandte Links
* [Azure AD Connect Health](whatis-hybrid-identity-health.md)
* [Installieren des Azure AD Connect Health-Agents](how-to-connect-health-agent-install.md)
* [Azure AD Connect Health-Vorgänge](how-to-connect-health-operations.md)
* [Verwenden von Azure AD Connect Health für die Synchronisierung](how-to-connect-health-sync.md)
* [Verwenden von Azure AD Connect Health mit AD DS](how-to-connect-health-adds.md)
* [Azure AD Connect Health – FAQ](reference-connect-health-faq.md)
* [Azure AD Connect Health: Versionsverlauf](reference-connect-health-version-history.md)
