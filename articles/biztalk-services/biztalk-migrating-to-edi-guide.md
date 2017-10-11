---
title: "Migrieren von BizTalk Server EDI-Lösungen zu technischen Handbuch zu BizTalk Services | Microsoft Docs"
description: Migrieren von EDI zu MABS; Microsoft Azure BizTalk Services
services: biztalk-services
documentationcenter: na
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 61c179fa-3f37-495b-8016-dee7474fd3a6
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 1b70fc3d199d7f1521acb534dafec8fb3e69500e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="migrating-biztalk-server-edi-solutions-to-biztalk-services-technical-guide"></a>Migrieren von BizTalk Server EDI-Lösungen in BizTalk Services: Technisches Handbuch

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Autor: Tim Wieman und Nitin Mehrotra

Bearbeiter: Karthik Bharthy

Mit geschrieben: Microsoft Azure BizTalk Services – Februar 2014-Version.

## <a name="introduction"></a>Einführung
Electronic Data Interchange (EDI) ist eines häufigsten Mittel, durch welche Unternehmen Exchange-Daten elektronisch, auch bezeichnet als Business-to-Business- oder B2B-Transaktionen. BizTalk Server wies die EDI-Unterstützung über einem Jahrzehnt seit der ersten BizTalk Sever-Version. Mit BizTalk-Dienste weiterhin Microsoft die Unterstützung für EDI-Lösungen auf der Microsoft Azure-Plattform. B2B-Transaktionen sind größtenteils außerhalb einer Organisation, und es ist daher einfacher zu implementieren, wenn er auf einer Cloudplattform implementiert wurde. Microsoft Azure bietet diese Funktion über BizTalk Services.

Während einige Kunden BizTalk Services als "Greenfield" Plattform für neue EDI-Lösungen betrachten, verfügen zahlreiche Kunden aktuelle BizTalk Server EDI-Lösungen, die in Azure migriert werden sollen. Da die Architektur von BizTalk Services-EDI-kann basierend auf den gleichen Schlüssel Entitäten wie BizTalk Server EDI-Architektur (trading Partner, Entitäten, Verträge), BizTalk Server EDI-Artefakten in BizTalk Services migrieren.

Dieses Dokument erläutert einige der Unterschiede bei der Migration von BizTalk Server EDI-Artefakte in BizTalk Services. Dieses Dokument geht ausreichende Kenntnisse der BizTalk Server EDI-Verarbeitung und-handelspartnerverträge. Weitere Informationen zu BizTalk Server EDI finden Sie unter [Handelspartnerverwaltung mithilfe von BizTalk Server](https://msdn.microsoft.com/library/bb259970.aspx).

## <a name="which-version-of-biztalk-server-edi-artifacts-can-be-migrated-to-biztalk-services"></a>Welche Version von BizTalk Server EDI-Artefakten in BizTalk Services migriert werden kann?
Das BizTalk Server EDI-Modul wurde für BizTalk Server 2010 erheblich verbessert, Partnern, Profilen und Vereinbarungen eingeschlossen wurde überarbeitet. BizTalk Services verwendet das gleiche Modell, um die Handelspartner und die Geschäftsbereiche innerhalb dieser Handelspartner zu organisieren. Daher ist die Migration von EDI-Artefakten aus BizTalk Server 2010 und höheren Versionen zu BizTalk-Dienste, ein sehr geradliniger Vorgang. Um mit Versionen vor BizTalk Server 2010 EDI-Artefakte migrieren möchten, müssen Sie zunächst ein upgrade auf BizTalk Server 2010 und dann Ihre EDI-Artefakte in BizTalk Services migrieren.

## <a name="scenariosmessage-flow"></a>Szenarien/Meldungsfluss
Als wird mit BizTalk Server EDI-Verarbeitung in BizTalk Services in einer Lösung (Trading Partner Management, Handelspartnerverwaltung) erstellt. Die TPM-Lösung weist die folgenden Hauptkomponenten:

* Handelspartner, die Organisation in einer B2B-Transaktion darstellen.
* Profile, die Geschäftsbereiche innerhalb eines Handelspartners darstellen.
* Trading Partner (oder Verträge), die den Geschäftsvertrag zwischen zwei Partnern/Profilen darstellen.

Die folgende Abbildung zeigt die ähnlichkeiten sowie die Unterschiede zwischen einer BizTalk Server EDI-Lösung und BizTalk Services EDI-Lösung:

![][EDImessageflow]

Der wesentliche Unterschiede und ähnlichkeiten, zwischen einem EDI-lösungsfluss in BizTalk Server und BizTalk-Dienste sind:

* Wie alle BizTalk Server eine EDIReceive-Pipeline zum Empfangen einer EDI-Nachricht und eine EDISend-Pipeline zum Senden einer EDI-Nachricht verwendet wird, verwendet BizTalk Services an eine EDI-empfangsbrücke zum Empfangen und die EDI-sendebrücke zum Senden von EDI-Nachrichten. In BizTalk Server die Pipelines mit einer Vereinbarung verknüpft sind, mithilfe von Sende- oder Empfangsports. In BizTalk-Dienste der Vertrag selbst kennzeichnet das Senden oder empfangsbrücke.
* Nachdem die EDIReceive-Pipeline die EDI-Nachricht verarbeitet, wird die Nachricht in BizTalk Server in einer SQL Server-Datenbank ausgegeben. Die EdiSend-Pipeline nimmt dann die Nachricht aus der SQL Server-Datenbank, verarbeitet sie und sendet sie dann an den Handelspartner.
  
    In BizTalk-Dienste nachdem die EDI-Brücke verarbeitet die EDI-Nachricht, es leitet die Nachricht an einen externen Prozess. Der externe Prozess kann auf Microsoft Azure oder lokal ausgeführt werden. Der externe Prozess sollte die Nachricht an die EDI-sendebrücke weiterleiten; die sendebridge ziehen die Nachricht nicht grundsätzlich. Nach der Verarbeitung der Nachricht, leitet die EDI-sendebrücke die Nachricht an den Handelspartner weiter.

BizTalk-Dienste bietet eine einfach zu verwendende Konfiguration-Erfahrung, um schnell erstellen und Bereitstellen einer B2B-Vertrag zwischen Handelspartnern ohne konfigurieren die Microsoft Azure Compute-Instanzen (Web- oder Workerrollen Rollen), Microsoft Azure SQL-Datenbanken oder alle Microsoft Azure-Speicherkonten. Komplexere Szenarien erfordern die Bindung in Workflows oder anderen verarbeitenden Dienst "an den Rändern" einen Handelspartnervertrag, d. h. vor oder nach der Verarbeitung von Trading Partner Vereinbarung EDI-Bridge. Im Detail treten die folgenden Ereignisfolgen während der EDI-Nachrichtenverarbeitung in BizTalk Services auf.

1. Eine EDI-Nachricht wird vom Handelspartner Fabrikam empfangen.  Für den Empfang von EDI-Nachrichten von Handelspartnern, unterstützt BizTalk Services Transportprotokolle wie FTP, SFTP, AS2 und HTTP/S.
2. Der Handelspartner Partner empfangsseitige vereinbarungsverarbeitung disassembliert die EDI-Nachricht in XML-Format.  Sie können die disassemblierte EDI-Nachricht (im XML-Format) mit Service Bus-Endpunkten wie z. B. ein Service Bus-Relay-Endpunkt, Service Bus-Thema, Service Bus-Warteschlange oder einem BizTalk Services-Brücke weiterleiten.
3. Die zurückverwandelten XML-Meldungen konnte vom Endpunkt für weitere benutzerdefinierte Verarbeitung dann empfangen werden.  Diese Endpunkte können z. B. nach einer lokalen Komponente oder zum weiteren Verarbeiten der Nachricht in einem Windows Workflows (WF) oder Windows Communication Foundation (WCF)-Dienst, eine Microsoft Azure Compute-Instanz verarbeitet werden.
4. "Absenderseitige Verarbeitung" des Handelspartners Vereinbarung dann assembliert die XML-Nachricht in EDI-Format und sendet sie an den Handelspartner Contoso.  Zum Senden von EDI-Nachrichten an Handelspartner, unterstützt der BizTalk-Dienste die gleichen Protokolle, die zum Empfangen von EDI-Nachrichten verwendet.

Weitere dieses Dokument enthält grundlegende Hinweise auf Migration einiger der verschiedenen BizTalk Server EDI-Artefakte in BizTalk Services.

## <a name="sendreceive-ports-to-trading-partners"></a>Sende-/Empfangsports an Handelspartner
In BizTalk Server richten Sie Empfangsspeicherorte und Empfangsports, um EDI-/XML-Nachrichten von Handelspartnern empfangen und Sendeports zum Senden von EDI-/XML-Nachrichten an Handelspartner einrichten. Anschließend binden Sie diese Ports an einen Handelspartnervertrag mithilfe der BizTalk Server-Verwaltungskonsole. In BizTalk-Dienste, die Speicherorte, in denen Sie Nachrichten von Handelspartnern empfangen, und, in dem Sie senden Nachrichten an Handelspartner als Teil des handelspartnervertrags selbst (als Teil der Transporteinstellungen) im BizTalk Services-Portal konfiguriert werden.  Daher müssen Sie nicht wirklich das Konzept "Sendeports" und "Empfangsspeicherorte" pro se in BizTalk Services. Weitere Informationen finden Sie unter [Erstellen von Verträgen](https://msdn.microsoft.com/library/windowsazure/hh689908.aspx).

## <a name="pipelines-bridges"></a>Pipelines (Brücken)
In BizTalk Server EDI sind Pipelines Entitäten, die auch benutzerdefinierten Logik für bestimmte Verarbeitungsfunktionen nach Bedarf von der Anwendung einschließen können. Für BizTalk-Dienste würde das Äquivalent eine EDI-Bridge. Jedoch werden in BizTalk-Dienste jetzt die EDI-Brücken "geschlossen".  Sie können keiner EDI-Bridge, also Ihre eigenen benutzerdefinierten Aktivitäten hinzufügen. Jede benutzerdefinierte Verarbeitung muss außerhalb der EDI-Bridge in Ihrer Anwendung erfolgen, bevor oder nachdem die Nachricht die Brücke, die als Teil des handelspartnervertrags konfiguriert eingegeben hat. EAI-Brücken besteht die Möglichkeit, benutzerdefinierte Verarbeitung auszuführen. Wenn Sie benutzerdefinierte Verarbeitung wünschen, können Sie EAI-Bridges verwenden, bevor oder nachdem die Nachricht von der EDI-Bridge verarbeitet wird. Weitere Informationen finden Sie unter [zum Einschließen von benutzerdefiniertem Code in Bridges](https://msdn.microsoft.com/library/azure/dn232389.aspx).

Sie können einen Veröffentlichen/Abonnieren-Fluss mit benutzerdefiniertem Code und/oder mithilfe von Service Bus-Warteschlangen und-Themen messaging, bevor der Handelspartnervertrag die Nachricht empfängt oder nachdem der Vertrag die Nachricht verarbeitet und leitet sie an einen Service Bus-Endpunkt einfügen.

Finden Sie unter **Szenarien/Meldungsfluss** in diesem Thema nachrichtenflussmuster.

## <a name="agreements"></a>Verträge
Wenn Sie mit der BizTalk Server 2010-Handelspartnerverträgen für EDI-Verarbeitung verwendet vertraut sind, suchen BizTalk Services handelspartnerverträge sehr vertraut vorkommen. Die meisten vertragseinstellungen sind gleich und verwenden dieselbe Terminologie. In einigen Fällen sind die vereinbarungseinstellungen wesentlich einfacher im Vergleich zu den gleichen Einstellungen in BizTalk Server. Microsoft Azure BizTalk Services unterstützt X12, EDIFACT und AS2-transport.

Microsoft Azure BizTalk Services bietet eine **TPM-Datenmigration** Tool zum Migrieren von Handelspartner und Vereinbarungen aus BizTalk Server handelspartnermodul in BizTalk Services-Portal. Die TPM-datenmigrationstool ist verfügbar als Teil des Pakets Tools, heruntergeladen werden kann von der [MABS SDK](http://go.microsoft.com/fwlink/p/?LinkId=235057). Das Paket enthält auch eine Infodatei, die Anweisungen zum Verwenden des Tools sowie grundlegende Informationen zur Problembehandlung für das Tool enthält.

## <a name="schemas"></a>Schemas
BizTalk-Dienste bietet EDI-Schemas, das in BizTalk Services-Lösungen verwendet werden kann.  Darüber hinaus können BizTalk Server EDI-Schemas auch mit BizTalk-Dienste verwendet werden, da der Stammknoten des EDI-Schemas in BizTalk Server als auch BizTalk Services identisch ist. Daher werden Sie direkt nehmen Ihre BizTalk Server EDI-Schemas und in den EDI-Lösungen, die Sie entwickeln, mit der BizTalk-Dienste verwenden können. Sie können auch die Schemas aus der [MABS SDK](http://go.microsoft.com/fwlink/p/?LinkId=235057).

## <a name="maps-transforms"></a>Zuordnungen (Transformationen)
Zuordnungen in BizTalk Server werden in der BizTalk-Dienste als Transformationen bezeichnet. Migrieren von Zuordnungen aus BizTalk Server in BizTalk Services konnte eine komplexere Aufgaben (je nach zuordnungskomplexität) erreicht werden. Das Zuordnungstool, das für die BizTalk-Dienste verwendet, unterscheidet sich von der BizTalk-Mapper. Obwohl der Mapper zumeist genauso aussieht, unterscheidet sich das zugrunde liegende Zuordnungsformat. Die Funktoide (aufgerufen **Zuordnungsvorgänge** in BizTalk Services) zur Verfügung, um die Benutzer sind ebenfalls unterschiedlich.  Sie können nicht aktiviert ist, direkt eine BizTalk-Zuordnung in BizTalk Services verwenden. Darüber hinaus sind nicht alle Funktoide in BizTalk Server als Zuordnungsvorgänge in BizTalk Services verfügbar.

### <a name="new-transform-operations"></a>Neue Transformationsvorgänge
Während die Liste der verfügbaren transformationszuordnungsvorgänge scheint recht stark vom BizTalk Server-Mapper ist, haben die BizTalk Services transformiert neue Methoden für die gleichen Aufgaben ausführen. Beispielsweise BizTalk Services transformiert haben **Auflistungsvorgänge** verfügbar. Dies war nicht in der BizTalk-Mapper verfügbar.  Die **Auflistungsvorgänge** ermöglichen es Ihnen, erstellen und Ausführen auf eine "Liste", in denen eine Liste ist eine Menge von Elementen (auch bekannt als "Zeilen") und jedes Element mehrere Mitglieder (auch bekannt als "Spalten") aufweisen kann.  Sie können sortieren Sie die Liste, wählen Sie Elemente auf Grundlage einer Bedingung usw..

Ein weiteres Beispiel für neue Funktionen in BizTalk Services transformiert werden die **Schleifenzuordnungsvorgänge**.  Es ist schwierig, geschachtelte Schleifen in BizTalk Server-Mapper zu erstellen.  Schleifenzuordnungsvorgänge werden daher für die BizTalk Services-Transformationen hinzugefügt.

Ein weiteres Beispiel ist die **If-Then-Else** ausdruckszuordnungsvorgang.  Auf diese Weise einer If-Then-else-Operation in der BizTalk-Mapper möglich war, aber es erforderlich, dass mehrere Funktoide, um eine scheinbar einfache Aufgabe auszuführen.

### <a name="migrating-biztalk-server-maps"></a>Migrieren von BizTalk Server zugeordnet ist.
Microsoft Azure BizTalk Services bietet ein Tool zum Migrieren von BizTalk Server in BizTalk Services-Transformationen zugeordnet. Die **BTMMigrationTool** steht als Teil der **Tools** Paket bereitgestellt, mit der [BizTalk Services SDK-Download](http://go.microsoft.com/fwlink/p/?LinkId=235057). Weitere Informationen zu diesem Tool finden Sie unter [Konvertieren einer BizTalk-Zuordnung in eine BizTalk Services-Transformation](https://msdn.microsoft.com/library/windowsazure/hh949812.aspx).

Sie können auch Beispiel von Sandro Pereira, BizTalk-MVP zum Suchen [Migrieren von BizTalk Server-Zuordnungen in BizTalk Services-Transformationen](http://social.technet.microsoft.com/wiki/contents/articles/23220.migrating-biztalk-server-maps-to-windows-azure-biztalk-services-wabs-maps.aspx).

## <a name="orchestrations"></a>Orchestrierungen
Wenn Sie BizTalk Server-Orchestrierungen in Microsoft Azure migrieren müssen, müssen die Orchestrierungen neu geschrieben werden, da es sich bei Microsoft Azure die Ausführung von BizTalk Server-Orchestrierungen nicht unterstützt.  Sie können der Orchestrierungsfunktionen in einem Windows Workflow Foundation 4.0 (WF4)-Dienst schreiben.  Es ist derzeit keine Migration von BizTalk Server-Orchestrierungen zu WF4, wäre dies eine vollständig neue Version. Hier sind einige Ressourcen für Windows Workflow:

* [*Vorgehensweise beim Integrieren von einem WCF-Workflowdienst in Service Bus-Warteschlangen und Themen* ](https://msdn.microsoft.com/library/azure/hh709041.aspx) von Paolo Salvatori. 
* [*Erstellen von apps mit Windows Workflow Foundation und Azure* Sitzung](http://go.microsoft.com/fwlink/p/?LinkId=237314) aus der Konferenz Build 2011.
* [*Windows Workflow Foundation Developer Center* ](http://go.microsoft.com/fwlink/p/?LinkId=237315) auf MSDN.
* [*Dokumentation zu Windows Workflow Foundation 4 (WF4)* ](https://msdn.microsoft.com/library/dd489441.aspx) auf MSDN.

## <a name="other-considerations"></a>Weitere Überlegungen
Es folgen einige Überlegungen, die Sie vornehmen müssen, bei der Verwendung von BizTalk-Dienste.

### <a name="fallback-agreements"></a>Ausweichvereinbarungen
BizTalk Server EDI-Verarbeitung wurde das Konzept der "Ausweichvereinbarungen".  BizTalk Services unterstützt **nicht** bisher verfügen über ein Konzept für die Ausweichvereinbarung.  Finden Sie in BizTalk-Dokumentationsthemen [die Rolle der Verträge bei EDI-Verarbeitungen](http://go.microsoft.com/fwlink/p/?LinkId=237317) und [globale konfigurieren oder Fallback-Vereinbarungseigenschaften](https://msdn.microsoft.com/library/bb245981.aspx) Informationen darüber, wie Ausweichvereinbarungen in BizTalk Server verwendet werden.

### <a name="routing-to-multiple-destinations"></a>Routing an mehrere Ziele
BizTalk Services-Bridges in seinem aktuellen Status nicht unterstützt Weiterleiten von Nachrichten an mehrere Ziele mithilfe einer Veröffentlichen-Abonnieren-Modell. Stattdessen konnte Sie Nachrichten aus einem BizTalk Services-Bridge an einen Service Bus-Topic weiterleiten, die dann mehrere Abonnements zum Empfangen der Nachricht an mehreren Endpunkten verfügen können.

## <a name="conclusion"></a>Zusammenfassung
Microsoft Azure BizTalk Services wird an regelmäßigen Meilensteinen hinzuzufügende Weitere Features und Funktionen aktualisiert. Mit jedem Update freuen wir umfangreichere Funktionen, um zu ermöglichen, erstellen die End-to-End-Lösungen, die mit der BizTalk-Dienste und andere Azure-Technologien unterstützt.

## <a name="see-also"></a>Siehe auch
[Entwickeln von Enterprise-Anwendungen mit Azure](https://msdn.microsoft.com/library/azure/hh674490.aspx)

[EDImessageflow]: ./media/biztalk-migrating-to-edi-guide/IC719455.png
