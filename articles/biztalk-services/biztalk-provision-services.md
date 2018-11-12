---
title: Erstellen von Azure BizTalk Services im Azure-Portal | Microsoft Docs
description: Erfahren Sie mehr über das Bereitstellen oder Erstellen von Azure-BizTalk Services im Azure-Portal; MABS, WABS
services: biztalk-services
documentationcenter: ''
author: MandiOhlinger
manager: anneta
editor: ''
ms.assetid: 3ad18876-a649-40d6-9aa0-1509c1d62c43
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: fe56b84b881b2afce9da9a7f7dfe4637c3494aa8
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51261960"
---
# <a name="create-biztalk-services-using-the-azure-portal"></a>Konfigurieren von BizTalk Services im Azure-Portal

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

> [!TIP]
> Für die Anmeldung beim Azure-Portal benötigen Sie ein Azure-Konto und ein Azure-Abonnement. Wenn Sie noch kein Konto haben, können Sie in wenigen Minuten ein kostenloses Testkonto erstellen. Siehe [Kostenlose Azure-Testversion](https://go.microsoft.com/fwlink/p/?LinkID=239738).


## <a name="CreateService"></a>Erstellen eines BizTalk Service

> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

Abhängig vom Status des BizTalk Service können einige Vorgänge nicht abgeschlossen werden. Eine Liste dieser Vorgänge finden Sie unter [BizTalk Services: Dienststatusübersicht](biztalk-service-state-chart.md).

## <a name="post-provisioning-steps"></a>Nach der Bereitstellung auszuführende Schritte
* [Installieren des Zertifikats auf einem lokalen Computer](#InstallCert)
* [Hinzufügen eines produktionsreifen Zertifikats](#AddCert)
* [Abrufen des Access Control-Namespace](#ACS)

#### <a name="InstallCert"></a>Installieren des Zertifikats auf einem lokalen Computer

> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

#### <a name="AddCert"></a>Hinzufügen eines produktionsreifen Zertifikats

> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

#### <a name="ACS"></a>Abrufen des Access Control-Namespace

> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

Wenn Sie ein BizTalk Service-Projekt von Visual Studio aus bereitstellen, geben Sie diesen Access Control-Namespace an. Der Access Control-Namespace wird automatisch für den BizTalk Service erstellt.

Die Access Control-Werte können mit jeder Anwendung verwendet werden. Nach Erstellung von Azure BizTalk Services steuert dieser Access Control-Namespace die Authentifizierung bei der BizTalk Service-Bereitstellung. Wenn Sie das Abonnement ändern oder den Namespace verwalten möchten, wählen Sie im linken Navigationsbereich zunächst **ACTIVE DIRECTORY** und anschließend Ihren Namespace aus. In der Taskleiste sind Ihre Optionen aufgeführt.

Wenn Sie auf **Verwalten** klicken, wird das Access Control-Verwaltungsportal geöffnet. Im Access Control-Verwaltungsportal verwendet der BizTalk-Dienst **Dienstidentitäten**:  
![ACS-Dienstidentitäten im Access Control-Verwaltungsportal][ACSServiceIdentities]

Die Access Control Service-Identität besteht aus einem Satz von Anmeldeinformationen, mit denen sich Anwendungen oder Clients direkt bei Access Control authentifizieren und ein Token abrufen können.

> [!IMPORTANT]
> Der BizTalk-Dienst verwendet als Standarddienstidentität **Besitzer** und den Wert **Kennwort**. Wenn Sie anstelle des Kennwortwerts den Wert des symmetrischen Schlüssels verwenden, kann folgender Fehler auftreten:<br/><br/>*Mit den angegebenen Anmeldeinformationen konnte keine Verbindung zum Access Control-Verwaltungsdienstkonto hergestellt werden.*
> 
> 

[Verwalten Ihres ACS-Namespace](https://msdn.microsoft.com/library/azure/hh674478.aspx) enthält einige Richtlinien und Empfehlungen.

## <a name="requirements-explained"></a>Erläuterung der Anforderungen
Diese Anforderungen gelten nicht für die Free Edition.

<table border="1">
<tr bgcolor="FAF9F9">
        <td><strong>Erforderliches Element</strong></td>
        <td><strong>Grund</strong></td>
</tr>
<tr>
<td>Azure-Abonnement</td>
<td>Das Abonnement bestimmt, wer sich bei Azure anmelden kann. Der Kontoinhaber erstellt das Abonnement unter <a HREF="https://account.windowsazure.com/Subscriptions">Azure-Abonnements</a>.
<br/><br/>
Das Azure-Konto kann über mehrere Abonnements verfügen und von jeder berechtigten Person verwaltet werden. Beispielsweise kann der Azure-Kontoinhaber ein Abonnement namens <em>BizTalkServiceSubscription</em> erstellen und den BizTalk-Administratoren in Ihrem Unternehmen (z.B. ContosoBTSAdmins@live.com) Zugriff auf dieses Abonnement gewähren. In diesem Szenario melden sich die BizTalk-Administratoren bei Azure an und haben uneingeschränkte Administratorrechte für alle gehosteten Dienste des Abonnements, einschließlich Azure BizTalk Services. Die BizTalk-Administratoren sind nicht die Azure-Kontoinhaber und haben daher keinen Zugriff auf Abrechnungsinformationen.
<br/><br/>Weitere Informationen finden Sie unter 
<a HREF="https://go.microsoft.com/fwlink/p/?LinkID=267577">Verwenden der rollenbasierten Zugriffssteuerung zum Verwalten des Zugriffs auf Ihre Azure-Abonnementressourcen</a>.
</td>
</tr>
<tr>
<td>Azure SQL-Datenbank</td>
<td>Enthält die von BizTalk Service verwendeten Tabellen, Ansichten und gespeicherten Prozeduren, einschließlich Nachverfolgungsdaten.
<br/><br/>
Wenn Sie einen BizTalk Service erstellen, können Sie eine vorhandene Azure SQL Server-Instanz und eine vorhandene Azure SQL-Datenbank verwenden oder automatisch einen neuen Server bzw. eine neue Datenbank erstellen.
<br/><br/>
Die SQL-Datenbankskalierung wird automatisch konfiguriert Normalerweise ist die Standardskalierung für einen BizTalk Service ausreichend. Änderungen der Skalierung können sich auf den Preis auswirken. Siehe <a HREF="https://go.microsoft.com/fwlink/p/?LinkID=234930">Konten und Abrechnung für Azure SQL-Datenbanken</a>
.<br/><br/>
<strong>Hinweise</strong>
<br/>
<ul>
<li> Wenn Sie einen neuen Azure SQL Server und eine Datenbank erstellen, wird Azure Services automatisch aktiviert. Für den BizTalk Service muss Azure Services aktiviert sein.</li>
<li>Bei der Erstellung einer neuen Azure SQL-Datenbank auf einem vorhandenen Azure SQL Server werden die Firewallregeln auf dem Server nicht verändert. Folglich ist es möglich, dass andere Azure Services keinen Zugriff auf die Datenbanken des Servers erhalten.</li>
</ul>
</td>
</tr>
<tr>
<td>Azure Access Control-Namespace</td>
<td>Dient zur Authentifizierung bei Azure BizTalk Services. Wenn Sie ein BizTalk Service-Projekt von Visual Studio aus bereitstellen, geben Sie diesen Access Control-Namespace an. Bei der Erstellung eines BizTalk Service wird der Access Control-Namespace automatisch erstellt.</td>
</tr>

<tr>
<td>Azure-Speicherkonto</td>
<td>Bietet Zugriff auf die Tabellen, Blobs und Warteschlangen, die vom BizTalk Service für die Speicherung von Folgendem verwendet werden:

<ul>
<li>Protokolldateien zur Überwachung des BizTalk Service. </li>
<li>Beim Erstellen einer X12- oder AS2-Vereinbarung zwischen Partnern können Sie die Funktion "Überwachung" aktivieren, um Eigenschaften von Nachrichten zu speichern. Diese Daten werden im Speicherkonto gespeichert.</li>
</ul>
<br/>
Wenn Sie einen BizTalk Service erstellen, können Sie ein vorhandenes Speicherkonto verwenden oder automatisch ein neues erstellen.
<br/><br/>
Die Standardspeichereinstellungen sind für einen BizTalk Service ausreichend.
<br/><br/>
Beim Erstellen eines Speicherkontos werden automatisch ein Primärschlüssel und ein Sekundärschlüssel erstellt. Diese Schlüssel steuern den Zugriff auf das Speicherkonto. Der BizTalk-Dienst verwendet automatisch den Primärschlüssel.
<br/><br/>
Weitere Informationen finden Sie unter <a HREF="https://go.microsoft.com/fwlink/p/?LinkID=285671">Speicher</a>.
</td>
</tr>

<tr>
<td>Privates SSL-Zertifikat</td>
<td>
Bei Erstellung eines Azure BizTalk Service wird außerdem eine HTTPS-URL erstellt, die den Namen Ihres BizTalk Service enthält. Diese URL wird automatisch zur Verwendung eines selbstsignierten Zertifikats konfiguriert, das ausschließlich für die Entwicklung gilt. Für die Produktion benötigen Sie ein privates SSL-Zertifikat.
<br/><br/>
<strong>Wichtige Informationen zum SSL-Zertifikat</strong>

<ul>
<li>Das Ablaufdatum des Zertifikats muss innerhalb der nächsten 5 Jahre liegen.</li>
<li>Für alle privaten Zertifikate ist ein Kennwort erforderlich. Bringen Sie dieses Kennwort in Erfahrung, und teilen Sie es am besten Ihren Administratoren mit.</li>
<li>Selbstsignierte Zertifikate werden in Test-/Entwicklungsumgebungen verwendet. Bei Verwendung selbstsignierter Zertifikate importieren Sie das Zertifikat in Ihren persönlichen Zertifikatspeicher und den Zertifikatspeicher "Vertrauenswürdige Stammzertifizierungsstellen".</li>
</ul>
<br/>Wenn Sie die Produktionszertifikatanforderung an Ihre Zertifizierungsstelle senden, geben Sie die folgenden Zertifikateigenschaften an:
<br/>

<ul>
<li><strong>Erweiterte Schlüsselverwendung</strong>: Für Azure BizTalk Services ist mindestens die Serverauthentifizierung erforderlich.</li>
<li><strong>Gemeinsamer Name</strong>: Geben Sie den vollqualifizierten Domänennamen (FQDN) Ihrer Azure BizTalk Service-URL ein. Siehe <a HREF="#CreateService">Erstellen eines BizTalk-Diensts</a> in diesem Artikel.</li>
</ul>
<br/>
Nach der Erstellung des BizTalk Service kann ein neues oder anderes Zertifikat hinzugefügt werden.
</td>
</tr>
</table>
<!---Loc Comment: Please, check link [Create a BizTalk Service] since it is not redirecting to any location.--->



## <a name="hybrid-connections"></a>Hybridverbindungen
Wenn Sie einen Azure BizTalk Service erstellen, steht die Registerkarte **Hybridverbindungen** zur Verfügung:

![Registerkarte "Hybridverbindungen"][HybridConnectionTab]

Über Hybridverbindungen werden Azure-Websites oder Azure Mobile Services mit einer lokalen Ressource (wie SQL Server, MySQL, HTTP Web-APIs, Mobile Services und die meisten benutzerdefinierten Webdienste) verbunden, die einen statischen TCP-Port verwendet.  Hybridverbindungen unterscheiden sich vom BizTalk Adapter Service. Der BizTalk Adapter Service stellt eine Verbindung zwischen Azure BizTalk Services und einem lokalen Branchensystem her.

 Weitere Informationen, beispielsweise zum Erstellen und Verwalten von Hybridverbindungen, finden Sie unter [Hybridverbindungen](integration-hybrid-connection-overview.md) .

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie jetzt einen BizTalk Service erstellt haben, machen Sie sich noch mit den verschiedenen Registerkarten unter [BizTalk Services: Registerkarten "Dashboard", "Überwachen" und "Skalieren"](biztalk-dashboard-monitor-scale-tabs.md)vertraut. Ihr BizTalk Service steht jetzt für Ihre Anwendungen zur Verfügung. Wenn Sie mit dem Erstellen von Anwendungen beginnen möchten, wechseln Sie zu [Azure BizTalk Services](https://go.microsoft.com/fwlink/p/?LinkID=235197).

## <a name="see-also"></a>Weitere Informationen
* [BizTalk Services: Editionsübersicht](biztalk-editions-feature-chart.md)<br/>
* [BizTalk Services: Statusübersicht](biztalk-service-state-chart.md)<br/>
* [BizTalk Services: Sichern und Wiederherstellen](biztalk-backup-restore.md)<br/>
* [BizTalk Services: Drosselung](biztalk-throttling-thresholds.md)<br/>
* [BizTalk Services: Name und Schlüssel des Ausstellers](biztalk-issuer-name-issuer-key.md)<br/>
* [Wie verwende ich das Azure BizTalk Services SDK?](https://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [Hybridverbindungen](integration-hybrid-connection-overview.md)

[NewBizTalkService]: ./media/biztalk-provision-services/WABS_NewBizTalkService.png
[NEWButton]: ./media/biztalk-provision-services/WABS_New.png
[ProgressComplete]: ./media/biztalk-provision-services/WABS_ProgressComplete.png
[ACSConnectInfo]: ./media/biztalk-provision-services/WABS_ACSConnectInformation.png
[QuickGlance]: ./media/biztalk-provision-services/WABS_QuickGlance.png
[ACSServiceIdentities]: ./media/biztalk-provision-services/WABS_ACSServiceIdentities.png
[HybridConnectionTab]: ./media/biztalk-provision-services/WABS_HybridConnectionTab.png
