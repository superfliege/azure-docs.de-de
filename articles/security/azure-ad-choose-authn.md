---
title: Wählen der richtigen Authentifizierungsmethode für Ihre Azure AD-Hybrididentitätslösung | Microsoft-Dokumentation
description: Dieser Leitfaden richtet sich an CEOs, CIOs, CISOs, Chief Identity Architects, Enterprise Architects sowie Sicherheits- und IT-Entscheidungsträger, die für die Auswahl einer Authentifizierungsmethode für ihre Azure AD-Hybrididentitätslösung in mittleren bis großen Unternehmen verantwortlich sind.
services: active-directory
keywords: ''
author: martincoetzer
ms.author: martincoetzer
ms.date: 04/12/2018
ms.topic: article
ms.service: active-directory
ms.workload: identity
ms.openlocfilehash: e7f894733546fa8949902a82f4ae3a9c62b749c0
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2018
---
# <a name="choosing-the-right-authentication-method-for-your-azure-active-directory-hybrid-identity-solution"></a>Wählen der richtigen Authentifizierungsmethode für Ihre Azure Active Directory-Hybrididentitätslösung 

Dieser Artikel ist der erste in einer Reihe von Artikeln, die Organisationen bei der Implementierung einer kompletten Azure AD-Hybrididentitätslösung unterstützen. Als sogenanntes Hybrid Identity Digital Transformation Framework (digitales Transformationsframework für Hybrididentitäten) wurde eine komplette Azure AD-Hybrididentitätslösung entworfen. Sie umfasst die Geschäftsergebnisse und auch die Ziele, auf die sich Organisationen konzentrieren sollten, um sicherzustellen, dass sie eine stabile und sichere Hybrideidentitätslösung implementiert haben. Das erste Geschäftsergebnis des Frameworks beschreibt die Anforderungen an Organisationen, den Authentifizierungsprozess zu sichern, wenn Benutzer auf Cloud-Apps zugreifen. Das erste Unternehmensziel beim Geschäftsergebnis mit sicherer Authentifizierung ist, Benutzern die Möglichkeit zu bieten, sich mit ihren lokalen Benutzernamen und Kennwörtern bei Cloud-Apps anzumelden. Durch diesen Anmeldeprozess und die Art der Benutzerauthentifizierung stehen in der Cloud alle Türen offen.

Die Wahl der richtigen Authentifizierungsmethode ist die erste Hürde für Organisationen, die ihre Apps in die Cloud verschieben möchten. Diese Entscheidung sollte aus folgenden Gründen nicht auf die leichte Schulter genommen werden:

1. Es ist die erste Entscheidung für eine Organisation, die in die Cloud wechseln möchte. 

2. Die Authentifizierungsmethode ist ein wichtiger Bestandteil des Auftritts einer Organisation in der Cloud, denn sie steuert den Zugriff auf alle Clouddaten und -ressourcen.

3. Sie ist die Grundlage aller anderen ergänzenden Features für Sicherheit und Benutzererfahrung in Azure AD.

4. Bei der Änderung der Authentifizierungsmethode nach der Implementierung können Schwierigkeiten auftreten.

Mit der Identität als neue Steuerungsebene der IT-Sicherheit ist die Authentifizierung sozusagen der „Wächter“ einer Organisation für den Zugang zur neuen Cloudwelt. Organisationen sollten sicherstellen, dass die Identitätskontrollebene ihre Sicherheit erhöht und ihre Cloud-Apps vor Angreifern schützt.

### <a name="out-of-scope"></a>Nicht betreffende Organisationen

Organisationen, die über keine lokalen Verzeichnisse verfügen, stehen nicht im Mittelpunkt dieses Artikels. Normalerweise werden Identitäten von diesen Unternehmen nur in der Cloud erstellt, wofür keine Hybrididentitätslösung erforderlich ist. Ausschließlich in der Cloud existierende Identitäten sind nicht mit entsprechenden lokalen Identitäten verknüpft.  

## <a name="choosing-the-right-authentication-method"></a>Wählen der richtigen Authentifizierungsmethode

Mit der Azure AD-Hybrididentitätslösung als neue Steuerungsebene bildet die Authentifizierung die Grundlage für den Cloudzugriff. Die Wahl der richtigen Authentifizierungsmethode ist daher eine wichtige erste Entscheidung bei der Einrichtung einer Azure AD-Hybrididentitätslösung. Die Implementierung der Authentifizierungsmethode erfolgt über Azure AD Connect, worüber auch Benutzer in der Cloud bereitstellt werden. 

Bei der Auswahl einer Authentifizierungsmethode müssen Sie die Zeit, die vorhandene Infrastruktur, die Komplexität und die Implementierungskosten für die gewählte Lösung berücksichtigen. Diese Faktoren sind für jede Organisation unterschiedlich und können sich im Laufe der Zeit ändern. 

Azure AD unterstützt für Hybrididentitätslösungen die folgenden Authentifizierungsmethoden:

### <a name="cloud-authentication"></a>Cloudauthentifizierung
Wenn Sie diese Authentifizierungsmethode wählen, übernimmt Azure AD die Anmeldung für Benutzer. In Verbindung mit dem nahtlosen einmaligen Anmelden (SSO) können sich Benutzer bei Cloud-Apps anmelden, ohne ihre Anmeldeinformationen erneut eingeben zu müssen. Bei der Cloudauthentifizierung können Sie zwischen zwei Optionen wählen: 

**Kennworthashsynchronisierung (Password Hash Sync, PHS)**: Der einfachste Weg, die Authentifizierung für lokale Verzeichnisobjekte in Azure AD zu ermöglichen. Die Kennworthashsynchronisierung ermöglicht es Benutzern, den gleichen Benutzernamen und das gleiche Kennwort wie lokal zu verwenden, ohne eine zusätzliche Infrastruktur bereitstellen zu müssen. Für einige Premiumfeatures von Azure AD, wie z.B. Identitätsschutz, ist eine Kennworthashsynchronisierung erforderlich, unabhängig davon, welche Authentifizierungsmethode gewählt wurde.

> [!NOTE] 
> Kennwörter werden nie im Klartext gespeichert oder mit einem umkehrbaren Algorithmus in Azure AD verschlüsselt. Weitere Informationen über den eigentlichen Prozess der Kennworthashsynchronisierung finden Sie unter [Implementieren der Kennworthashsynchronisierung mit der Azure AD Connect-Synchronisierung](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-synchronization). 

**Passthrough-Authentifizierung (PTA)**: Bietet eine einfache Kennwortvalidierung für Azure AD-Authentifizierungsdienste mithilfe eines Software-Agents, der auf mindestens einem lokalen Server ausgeführt wird. Dieser validiert die Benutzer direkt in ihrem lokalen Active Directory, sodass die Kennwortvalidierung nicht in der Cloud stattfindet. Unternehmen, die aufgrund gesetzlicher Vorgaben keine Hashkennwörter in der Cloud speichern dürfen und daher Kennworthashsynchronisierung nicht verwenden können, würden diese Authentifizierungsmethode nutzen. Weitere Informationen zum eigentlichen Passthrough-Authentifizierungsprozess finden Sie unter [Benutzeranmeldung mit der Azure Active Directory-Passthrough-Authentifizierung](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication).

### <a name="federated-authentication"></a>Verbundauthentifizierung
Wenn Sie diese Authentifizierungsmethode wählen, übergibt Azure AD den Authentifizierungsprozess zur Validierung des Benutzerkennworts an ein separates vertrauenswürdiges Authentifizierungssystem, z.B. an lokale Active Directory-Verbunddienste (AD FS). Das Authentifizierungssystem kann zusätzliche Authentifizierungsanforderungen bereitstellen, wie z.B. eine Smartcard-basierte Authentifizierung oder eine mehrstufige Authentifizierung von einem Drittanbieter. Weitere Informationen finden Sie im [Windows Server 2016 und 2012 R2 AD FS-Bereitstellungshandbuch](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/deployment/windows-server-2012-r2-ad-fs-deployment-guide).

Der folgende Abschnitt hilft Ihnen, anhand einer Entscheidungsstruktur die für Sie richtige Authentifizierungsmethode zu ermitteln. So können Sie entscheiden, ob Sie eine Cloud- oder Verbundauthentifizierung für Ihre Azure AD Hybrididentitätslösung einsetzen möchten.

## <a name="azure-ad-authentication-decision-tree"></a>Entscheidungsstruktur zur Azure AD-Authentifizierung

![Bild1](media/azure-ad/azure-ad-authn-image1.png)

## <a name="detailed-considerations-on-authentication-methods"></a>Ausführliche Überlegungen zu Authentifizierungsmethoden

### <a name="cloud-authentication-password-hash-sync"></a>Cloudauthentifizierung: Kennworthashsynchronisierung 

* **Aufwand**: Die Kennworthashsynchronisierung erfordert den geringsten Aufwand in Bezug auf Bereitstellung, Wartung und Infrastruktur für Organisationen, die ihren Benutzern lediglich die Anmeldung bei Office 365, SaaS-Apps und anderen Azure AD-basierten Ressourcen ermöglichen müssen. Wenn aktiviert, ist die Kennworthashsynchronisierung Teil des Azure AD Connect-Synchronisierungsprozesses, der alle zwei Minuten ausgeführt wird. 

* **Benutzererfahrung**: Es wird empfohlen, dass Organisationen nahtloses einmaliges Anmelden (SSO) mit Kennworthashsynchronisierung verwenden, um das Benutzererlebnis bei der Anmeldung zu verbessern. So werden unnötige Aufforderungen nach deren Anmeldung vermieden.

* **Erweiterte Szenarien**: Wenn Organisationen sich hierfür entscheiden, lassen sich Erkenntnisse aus Identitäten mit Azure AD Identity Protection-Berichten zu nutzen, wie z.B. den Bericht über kompromittierte Anmeldeinformationen. Windows Hello for Business ist eine weitere Option mit [spezifischen Anforderungen, wenn Sie Kennworthashsynchronisierung verwenden](https://docs.microsoft.com/en-us/windows/access-protection/hello-for-business/hello-identity-verification). Organisationen, die eine mehrstufige Authentifizierung mit Kennworthashsynchronisierung erfordern, müssen die mehrstufige Azure AD-Authentifizierung verwenden und können keine externen oder lokalen mehrstufigen Authentifizierungsmethoden verwenden.

* **Geschäftskontinuität**: Die Kennworthashsynchronisierung ist von sich aus hochverfügbar als Clouddienst, der auf alle Microsoft-Rechenzentren skaliert wird. Es wird empfohlen, einen zweiten Azure AD Connect-Server im Stagingmodus in einer Standbykonfiguration für die Notfallwiederherstellung einzusetzen.

* **Überlegungen**: Die Kennworthashsynchronisierung erzwingt derzeit keine umgehenden Änderungen an den Status lokaler Konten. In dieser Situation hat ein Benutzer Zugriff auf Cloud-Apps, bis der Status des Benutzerkontos mit Azure AD synchronisiert ist. Wenn Organisationen diese Einschränkung umgehen möchten, wird empfohlen, einen neuen Synchronisierungszyklus zu aktivieren, nachdem Administratoren Massenaktualisierungen von lokalen Benutzerkonten durchgeführt haben, wie z.B. das Deaktivieren von Konten. Die Kontosperre ist ein weiterer Status des Benutzerkontos, der im nächsten Zyklus synchronisiert wird. 

> [!NOTE] 
> Der Status eines abgelaufenen Kennworts wird derzeit nicht mit Azure AD Connect in Azure AD synchronisiert. 

Informationen zu den Bereitstellungsschritten finden Sie unter [Implementieren der Kennworthashsynchronisierung mit der Azure AD Connect-Synchronisierung](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-synchronization).

### <a name="cloud-authentication-pass-through-authentication"></a>Cloudauthentifizierung: Passthrough-Authentifizierung  

* **Aufwand**: Für die Passthrough-Authentifizierung benötigen Sie einen oder mehrere einfache Agents (empfohlen sind drei), die auf vorhandenen Servern installiert sind und Zugriff auf Ihre lokalen Active Directory Domain Services sowie auf Ihre lokalen AD-Domänencontroller haben. Diese Agents benötigen Berechtigungen für den Zugriff auf das Internet und haben Zugriff auf Ihre Domänencontroller. Aus diesem Grund wird die Bereitstellung der Agents in einem Umkreisnetzwerk nicht unterstützt, da dies uneingeschränkten Netzwerkzugriff auf Domänencontroller erfordert. Der gesamte Netzwerkverkehr ist verschlüsselt und auf Authentifizierungsanfragen beschränkt. Weitere Informationen zu diesem Prozess finden Sie unter [Azure Active Directory-Passthrough-Authentifizierung – ausführliche Informationen zur Sicherheit](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-security-deep-dive).

* **Benutzererfahrung**: Es wird empfohlen, dass Organisationen nahtloses einmaliges Anmelden mit Passthrough-Authentifizierung verwenden, um das Benutzererlebnis bei der Anmeldung zu verbessern. So werden unnötige Aufforderungen nach deren Anmeldung vermieden.

* **Erweiterte Szenarien**: Durch die Passthrough-Authentifizierung wird sichergestellt, dass Authentifizierungsanforderungen sofort abgelehnt werden, wenn ein lokales Benutzerkonto einen deaktivierten oder gesperrten Status aufweist oder dessen Kennwort abgelaufen ist. Organisationen, die eine mehrstufige Authentifizierung mit Passthrough-Authentifizierung erfordern, müssen die mehrstufige Azure AD-Authentifizierung verwenden und können keine externe oder lokale mehrstufige Authentifizierungsmethode nutzen. Für erweiterte Features, wie z.B. den Identity Protection-Bericht über kompromittierte Anmeldeinformationen, muss die Kennworthashsynchronisierung unabhängig davon eingesetzt werden, ob Sie die Passthrough-Authentifizierung wählen.

* **Geschäftskontinuität**: Es wird empfohlen, neben dem ersten Agent auf dem Azure AD Connect-Server zwei weitere Passthrough-Agents bereitzustellen, um eine Hochverfügbarkeit der Authentifizierungsanforderungen zu gewährleisten. Wenn Sie drei Agents bereitgestellt haben, kann ein Agent immer noch ausfallen, wenn ein anderer Agent wegen Wartungsarbeiten ausfällt. Ein weiterer Vorteil des Einsatzes der Kennworthashsynchronisierung zusätzlich zur Passthrough-Authentifizierung ist, dass sie als Ersatzauthentifizierungsmethode fungieren kann, wenn die primäre Authentifizierungsmethode, z.B. bei einem lokalen Serverausfall, nicht mehr verfügbar ist.

* **Überlegungen**: Wenn Sie die Kennworthashsynchronisierung als Ersatzauthentifizierungsmethode für die Passthrough-Authentifizierung verwenden und die Agents die Anmeldeinformationen des Benutzers nicht validieren können, erfolgt kein automatisches Failover zur Kennworthashsynchronisierung. Sie müssten die Anmeldemethode manuell mit Azure AD Connect wechseln. Die Passthrough-Authentifizierung unterstützt nur Cloud-Apps, die moderne Authentifizierungsmethoden und spezifische Exchange Online-Protokolle wie ActiveSync, POP3 und IMAP4 verwenden. Lesen Sie weitere Informationen darüber, dass z.B. [Microsoft Office 2013 und höher moderne Authentifizierungsmethoden unterstützt, Vorgängerversionen jedoch nicht](https://blogs.office.com/en-us/2015/11/19/updated-office-365-modern-authentication-public-preview/). Mehr erfahren Sie auch in den [häufig gestellten Fragen](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-faq) und anderen Überlegungen zur Passthrough-Authentifizierung, wie der Unterstützung für alternative IDs.

Weitere Informationen zu den Bereitstellungsschritten finden Sie unter [Benutzeranmeldung mit der Azure Active Directory-Passthrough-Authentifizierung](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication).

### <a name="federated-authentication"></a>Verbundauthentifizierung

* **Aufwand**: Bei Verwendung eines Verbundauthentifizierungssystems greift dieses auf ein externes System zurück, um Benutzer zu authentifizieren. Einige Unternehmen möchten ihre bestehenden Investitionen in Verbundsysteme in Verbindung mit ihrer Azure AD-Hybrididentitätslösung wiederverwenden. Die Wartung und Verwaltung des Verbundsystems wird nicht über Azure AD gesteuert. Die Organisation, die das Verbundsystem verwendet, muss selbst sicherstellen, dass es die erforderliche Sicherheit bietet und die Authentifizierungslast bewältigen kann. 

* **Benutzererfahrung**: Die Benutzererfahrung der Verbundauthentifizierung hängt von der Implementierung der Funktionen, der Topologie und der Konfiguration der Verbundfarm ab. Einige Organisationen benötigen diese Flexibilität, um den Zugriff auf die Verbundfarm an ihre Sicherheitsanforderungen anzupassen und zu konfigurieren. Beispielsweise ist es möglich, intern verbundene Benutzer und Geräte so zu konfigurieren, dass sie automatisch angemeldet werden. So werden sie nicht nach Anmeldeinformationen gefragt, da sie sich bereits bei ihren Geräten angemeldet haben. Andererseits können ggf. einige erweiterte Sicherheitsfunktionen den Anmeldeprozess des Benutzers erschweren.

* **Erweiterte Szenarien**: Eine Verbundauthentifizierungslösung ist in der Regel erforderlich, wenn die Authentifizierungsanforderung eines Kunden nicht nativ von Azure AD unterstützt wird. Detaillierte Informationen sind [hier aufgeführt](https://blogs.msdn.microsoft.com/samueld/2017/06/13/choosing-the-right-sign-in-option-to-connect-to-azure-ad-office-365/), zu den allgemeinen Anforderungen jedoch gehören:

    * Authentifizierung mit Smartcards oder Zertifikaten
    * Verwendung eines lokalen MFA-Servers oder eines MFA-Drittanbieters
    * Authentifizierung mit einer Drittanbieter-Authentifizierungslösung. Siehe die [Azure AD-Verbund – Kompatibilitätsliste](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-federation-compatibility).
    * Benutzer müssen sich mit ihrem sAMAccountName anmelden, z.B. „DOMÄNE\Benutzername“, statt mit einem Benutzerprinzipalnamen (UPN), z.B. user@domain.com.
    * Eine Windows Hello for Business-Implementierung, die von einer internen Enterprise Public Key-Infrastruktur (PKI) für vertrauenswürdige Zertifikate abhängig ist.

* **Geschäftskontinuität**: Verbundsysteme erfordern in der Regel ein Serverarray mit Lastenausgleich (auch bekannt als Farm), das in einer internen Netzwerk- und Umkreisnetzwerktopologie konfiguriert ist, um Hochverfügbarkeit für Authentifizierungsanforderungen zu gewährleisten. Die Kennworthashsynchronisierung kann zusammen mit der Verbundauthentifizierung als Ersatzauthentifizierungsmethode verwendet werden. Diese wird dann genutzt, wenn die primäre Authentifizierungsmethode z.B. bei einem lokalen Serverausfall nicht mehr verfügbar ist. Einige Großunternehmen benötigen eine Verbundlösung, um mehrere mit Geo-DNS konfigurierte Internetzugangspunkte für Authentifizierungsanfragen mit geringer Latenz zu unterstützen.

* **Überlegungen**: Für Verbundsysteme müssen in der Regel größere Investitionen in die lokale Infrastruktur getätigt werden. Die meisten Organisationen entscheiden sich für diese Option, wenn sie bereits in ein lokales Verbundsystem investiert haben und es eine hohe Geschäftsanforderung ist, einen einzigen Identitätsanbieter zu verwenden. Die Bereitstellung und Fehlerbehebung bei einer Verbundlösung ist im Vergleich zu Cloudauthentifizierungslösungen komplexer. Für die Anmeldung von Benutzer-IDs mit einer nicht routingfähigen Domäne, die nicht in Azure AD verifiziert werden kann, ist eine zusätzliche Konfiguration erforderlich. Diese Anforderung wird als Unterstützung für alternative Anmelde-IDs bezeichnet. Weitere Informationen zu Einschränkungen und Anforderungen finden Sie unter [Konfigurieren von alternativen Anmelde-ID](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/operations/configuring-alternate-login-id).

Informationen zu Bereitstellungsschritten finden Sie unter [Bereitstellen von Verbundservern](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/deployment/deploying-federation-servers).

> [!NOTE] 
> Bei der Bereitstellung Ihrer Azure AD-Hybrididentitätslösung müssen Sie sicherstellen, dass Sie eine der von Azure AD Connect unterstützten Topologien implementieren. Erfahren Sie mehr über unterstützte und nicht unterstützte Konfigurationen unter [Topologien für Azure AD Connect](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-topologies).

## <a name="architecture-diagrams"></a>Architekturdiagramme

Das folgende Diagramm zeigt die höchste Ebene der Architekturkomponenten, die für jede Authentifizierungsmethode erforderlich sind, die Sie mit Ihrer Azure AD-Hybrididentitätslösung verwenden können. Es gibt Ihnen einen Überblick über die Unterschiede zwischen den Lösungen.

Das folgende Diagramm zeigt die Einfachheit einer Lösung mit Kennworthashsynchronisierung:

![Kennworthashsynchronisierung](media/azure-ad/azure-ad-authn-image2.png)

Das folgende Diagramm verdeutlicht die Agent-Anforderungen für die Passthrough-Authentifizierung:

![PTA](media/azure-ad/azure-ad-authn-image3.png)

Das folgende Diagramm zeigt die Komponenten, die für den Verbund im Umkreisnetzwerk und internen Netzwerk Ihrer Organisation erforderlich sind:

![ADFS](media/azure-ad/azure-ad-authn-image4.png)

## <a name="recommendations-and-considerations-from-azure-ad"></a>Empfehlungen und Überlegungen von Azure AD

Ihr Identitätssystem stellt sicher, dass Ihre Benutzer Zugriff auf Cloud-Apps und branchenspezifische Apps haben, die Sie migrieren und in der Cloud zur Verfügung stellen. Die Authentifizierung steuert den Zugriff auf Apps, um autorisierte Benutzer produktiv und unautorisierte Akteure von den vertraulichen Daten Ihrer Organisation fern zu halten. Beachten Sie daher die folgenden Empfehlungen bei der Wahl der richtigen Authentifizierungsmethode für Ihre Organisation. 

Verwenden oder aktivieren Sie die Kennworthashsynchronisierung aus folgenden Gründen, unabhängig von der gewählten Authentifizierungsmethode:

1. **Hochverfügbarkeit und Notfallwiederherstellung**: Die Passthrough- und die Verbundauthentifizierungsmethode basieren auf der lokalen Infrastruktur. Bei der Passthrough-Authentifizierung zählen dazu auch die Serverhardware und das Netzwerk. Die Methode ist zudem darauf angewiesen, dass Domänencontroller auf Authentifizierungsanforderungen der Passthrough-Authentifizierungs-Agents reagieren. Für die Verbundauthentifizierung ist sogar noch mehr lokale Hardware erforderlich, denn es werden sowohl Server in Ihrem Umkreisnetzwerk für Proxyauthentifizierungsanforderungen als auch die internen Verbundserver benötigt. Um Single Point of Failures zu vermeiden, sollte Ihre Organisation mithilfe von redundanten Servern sicherstellen, dass Authentifizierungsanforderungen auch dann bedient werden, wenn eine Komponente ausfällt. Viele Komponenten müssen gewartet werden, um deren Integrität zu gewährleisten. Daher sind Ausfälle wahrscheinlicher, wenn die Wartung nicht ordnungsgemäß geplant und ausgeführt wird. Ausfälle können mithilfe der Kennworthashsynchronisierung vermieden werden, da der Microsoft-Dienst für die Azure AD-Cloudauthentifizierung weltweit skaliert und immer verfügbar ist.

2. **Sicherung bei lokalem Ausfall**: Die Folgen eines lokalen Ausfalls aufgrund eines Cyberangriffs oder einer Katastrophe können beträchtlich sein. Sie reichen von einem Markenschaden bis hin zu einer handlungsunfähigen Organisation, die den Angriff nicht bewältigen kann. Im letzten Jahr wurden viele Organisationen Opfer von Malware-Angriffen, wie gezielter Ransomware, die zu einem Ausfall ihrer lokalen Server führten. Bei der Unterstützung von Kunden, diese Art von Angriffen zu bewältigen, hat Microsoft zwei Kategorien von Organisationen erkannt:

   a. Organisationen, die zuvor die Kennworthashsynchronisierung aktiviert hatten, waren innerhalb weniger Stunden wieder online, indem sie ihre Authentifizierungsmethode auf die Kennworthashsynchronisierung umgestellt hatten. Durch den Zugriff auf E-Mails über Office 365 konnten sie weiterarbeiten, um Probleme zu lösen und Zugang zu anderen cloudbasierten Workloads zu erhalten.

   b. Organisationen, die die Kennworthashsynchronisierung nicht zuvor aktiviert hatten, mussten für die Kommunikation und Problembehebung auf nicht vertrauenswürdige externe E-Mail-Systeme zurückgreifen. In diesen Fällen dauerte es Wochen oder länger, bis sie wieder voll einsatzbereit waren.

3. **Identity Protection**: Eine der besten Möglichkeiten, Benutzer in der Cloud zu schützen, ist Azure AD Identity Protection. Microsoft überprüft das Internet ständig nach Benutzer- und Kennwortlisten, die unautorisierte Akteure verkaufen möchten und im Darknet anbieten. Azure AD kann diese Informationen verwenden, um zu überprüfen, ob einer der Benutzernamen mit Kennwort in Ihrer Organisation kompromittiert ist. Daher ist es wichtig, die Kennworthashsynchronisierung zu aktivieren, unabhängig davon, welche Authentifizierungsmethode Sie verwenden (Verbund- oder Passthrough-Authentifizierung). Kompromittierte Anmeldeinformationen werden als Bericht angezeigt und können verwendet werden, um einen Benutzer zu blockieren oder zu einer Änderung seines Kennworts zu zwingen, wenn er versucht, sich mit einem kompromittierten Kennwort anzumelden.

Schließlich verfügt Microsoft laut [Gartner](https://info.microsoft.com/landingIAMGartnerreportregistration.html) über die umfangreichsten Funktionen für das Identitäts- und Zugriffsmanagement. Microsoft wehrt sieben Billionen Cyberangriffe pro Tag ab und bietet autorisierten Benutzern die Möglichkeit, sich von praktisch jedem Gerät aus für Tausende von SaaS-Anwendungen wie Office 365 anzumelden. 

## <a name="conclusion"></a>Zusammenfassung

Dieser Artikel beschreibt verschiedene Authentifizierungsoptionen, die Organisationen konfigurieren und bereitstellen können, um den Zugriff auf Cloud-Apps zu unterstützen. Um verschiedene geschäftliche, sicherheitstechnische und technische Anforderungen zu erfüllen, können Organisationen zwischen Kennworthashsynchronisierung, Passthrough-Authentifizierung und der Verbundmethode wählen. Bei jeder Authentifizierungsmethode kann Ihre Organisation entscheiden, ob ihre Geschäftsanforderungen durch den beim Bereitstellen der Lösung entstehenden Aufwand und die Benutzerfreundlichkeit des Anmeldeprozesses erfüllt werden. Außerdem müssen Sie prüfen, ob Ihre Organisation die Funktionen für erweiterte Szenarien und Geschäftskontinuität der einzelnen Authentifizierungsmethoden benötigt. Schließlich müssen Sie die Überlegungen zu den einzelnen Authentifizierungsmethoden auswerten, um zu sehen, ob Sie Ihre gewünschte Lösung tatsächlich implementieren können.

## <a name="next-steps"></a>Nächste Schritte

In der heutigen Welt sind Bedrohungen 24 Stunden am Tag präsent und kommen von überall her. Die Implementierung der richtigen Authentifizierungsmethode trägt dazu bei, Ihre Sicherheitsrisiken zu minimieren und Ihre Identitäten zu schützen. 

[Beginnen Sie](https://docs.microsoft.com/en-us/azure/active-directory/get-started-azure-ad) mit Azure AD, und setzen Sie die richtige Authentifizierungslösung für Ihre Organisation ein.

Wenn Sie eine Migration von der Verbund- zur Cloudauthentifizierung erwägen, lesen Sie [weitere Informationen zur Änderung der Anmeldemethode](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-user-signin#changing-the-user-sign-in-method). Als Unterstützung beim Planen und Durchführen der Migration können Sie [diese hilfreichen Projektpläne verwenden](https://github.com/Identity-Deployment-Guides/Identity-Deployment-Guides/tree/master/Authentication).
