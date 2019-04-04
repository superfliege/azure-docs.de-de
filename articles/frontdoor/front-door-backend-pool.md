---
title: 'Azure Front Door Service: Back-Ends und Back-End-Pools | Microsoft-Dokumentation'
description: In diesem Artikel werden Back-Ends und Back-End-Pools für die Front Door-Konfiguration erläutert.
services: front-door
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: 228ed5c54a382db7b47d19adacf9e5db398c53ae
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58123690"
---
# <a name="backends-and-backend-pools-in-azure-front-door-service"></a>Back-Ends und Back-End-Pools in Azure Front Door Service
In diesem Artikel werden die verschiedenen Konzepte hinsichtlich der Zuordnung Ihrer Anwendungsbereitstellung zu Front Door beschrieben. Darüber hinaus werden die verschiedenen in der Front Door-Konfiguration rund um das Anwendungs-Back-End verwendeten Begriffe erklärt.

## <a name="backend-pool"></a>Back-End-Pool
Ein Back-End-Pool in Front Door bezeichnet eine Gruppe gleichwertiger Back-Ends, die die gleiche Art von Datenverkehr für ihre Anwendung empfangen können. Mit anderen Worten: Es handelt sich um eine logische Gruppierung Ihrer Anwendungsinstanzen weltweit, die den gleichen Datenverkehr empfangen und mit dem erwarteten Verhalten reagieren können. Diese Back-Ends werden in der Regel regionsübergreifend oder innerhalb derselben Region bereitgestellt. Zudem können alle diese Back-Ends im Aktiv/Aktiv-Bereitstellungsmodus vorliegen oder auch auf eine Aktiv/Passiv-Konfiguration festgelegt sein.

Durch den Back-End-Pool wird außerdem definiert, wie die Integrität der unterschiedlichen Back-Ends über Integritätstests zu prüfen ist und wie der Lastenausgleich zwischen den Back-Ends zu erfolgen hat.

### <a name="health-probes"></a>Integritätstests
Front Door sendet regelmäßige HTTP/HTTPS-Testanforderungen an jeden der konfigurierten Back-Ends, um Entfernung und Integrität der einzelnen Back-Ends zu bestimmen und einen Lastenausgleich hinsichtlich der Endbenutzer-Anforderungen auszuführen. Mit den Integritätstesteinstellungen für einen Back-End-Pool wird festgelegt, wie der Integritätsstatus Ihrer Anwendungs-Back-Ends abgefragt wird. Die folgenden Einstellungen sind für die Konfiguration des Lastenausgleichs verfügbar:

1. **Pfad**: URL-Pfad, an den die Testanforderungen für alle Back-Ends im Back-End-Pool gesendet werden. Ist beispielsweise einer Ihrer Back-Ends `contoso-westus.azurewebsites.net`, und der Pfad ist auf `/probe/test.aspx` festgelegt, senden Front Door-Umgebungen (falls das Protokoll auf HTTP festgelegt ist) die Integritätstestanforderungen an http://contoso-westus.azurewebsites.net/probe/test.aspx. 
2. **Protokoll:** Legt fest, ob die Integritätstestanforderungen von der Front Door-Instanz an Ihre Back-Ends über das HTTP-Protokoll oder das HTTPS-Protokoll gesendet werden.
3. **Intervall (Sekunden):** In diesem Feld wird die Häufigkeit der Integritätstests Ihrer Back-Ends festgelegt, d. h. die Intervalle, in denen die einzelnen Front Door-Umgebungen einen Test senden. Hinweis: Wenn Sie an schnellerem Failover interessiert sind, legen Sie dieses Feld auf einen niedrigeren Wert fest. Je niedriger der Wert, desto größer ist jedoch das Integritätstestvolumen, das Ihre Back-Ends empfangen. Das folgende Beispiel soll eine Vorstellung davon vermitteln, welches Testvolumen Front Door auf Ihren Back-Ends generiert. Angenommen, das Intervall ist auf 30 Sekunden festgelegt, und es gibt ca. 90 Front Door-Umgebungen oder POPs weltweit. Damit empfängt jedes Back-End etwa 3 bis 5 Testanforderungen pro Sekunde.

Einzelheiten hierzu finden Sie unter [Integritätstests](front-door-health-probes.md).

### <a name="load-balancing-settings"></a>Lastenausgleichseinstellungen
Die Lastenausgleichseinstellungen für den Back-End-Pool legen fest, wie die Integritätstests ausgewertet werden und ein Back-End als fehlerfrei oder fehlerhaft bewertet wird. Zudem wird damit bestimmt, wie der Lastenausgleich zwischen den verschiedenen Back-Ends im Back-End-Pool auszuführen ist. Die folgenden Einstellungen sind für die Konfiguration des Lastenausgleichs verfügbar:

1. **Stichprobengröße:** Durch diese Eigenschaft wird angegeben, wie viele Stichproben von Integritätstests bei der Auswertung der Integrität der Back-Ends zu berücksichtigen sind.
2. **Erfolgreiche Stichprobengröße:** Diese Eigenschaft legt fest, wie viele Stichproben bei dem oben erläuterten Stichprobenumfang benötigt werden, um auf Integrität zu prüfen und das Back-End als fehlerfrei zu bewerten. 
</br>Angenommen, Sie haben für Ihre Front Door das *Intervall* des Integritätstests auf 30 Sekunden, die *Stichprobengröße* auf '5' und *Successful sample size* (Erfolgreiche Stichprobengröße) auf '3' festgelegt. Diese Konfiguration besagt, dass wir bei jeder Auswertung der Integritätstests für Ihr Back-End die letzten fünf Stichproben untersuchen, die sich über die letzten 150 Sekunden (=5*30 s) erstrecken. Sind nicht mindestens drei dieser Stichproben erfolgreich, wird das Back-End für fehlerhaft erklärt. Angenommen, es gab nur zwei erfolgreiche Tests, daher wird das Back-End als fehlerhaft markiert. Wenn bei der nächsten Auswertung in den letzten fünf Tests drei erfolgreiche Tests festgestellt werden, markieren wird das Back-End wieder als fehlerfrei.
3. **Wartezeitenaktivität (zusätzliche Wartezeit):** Das Feld für die Wartezeitenaktivität legt fest, ob die Front Door-Instanz die Anforderung an Back-Ends senden soll, die sich hinsichtlich der Wartezeit im Wartezeitbereich befinden, oder ob die Anforderung an das nächstgelegene Back-End weitergeleitet werden soll. Weitere Informationen finden Sie unter [Routingmethode mit geringsten Wartezeiten](front-door-routing-methods.md#latency) für Front Door.

## <a name="backend"></a>Back-End
Ein Back-End entspricht der Bereitstellungsinstanz einer Anwendung in einer Region. Front Door unterstützt sowohl Azure-Back-Ends als auch Nicht-Azure-Back-Ends. Daher ist die Region nicht nur auf Azure-Regionen beschränkt, sondern es kann sich auch um Ihr lokales Rechenzentrum oder eine Anwendungsinstanz in einer anderen Cloud handeln.

Back-End bezeichnet im Zusammenhang mit Front Doors den Hostnamen oder die öffentliche IP Ihrer Anwendung, die Clientanforderungen verarbeiten kann. Daher dürfen Back-Ends nicht mit Ihrer Datenbankebene oder Speicherebene verwechselt werden; betrachten Sie sie stattdessen als öffentlichen Endpunkt Ihres Anwendungs-Back-Ends.

Wenn Sie ein Back-End in einem Back-End-Pool Ihrer Front Door hinzufügen, müssen Sie folgende Details angeben:

1. **Back-End-Hosttyp:** Der Typ der Ressource, die hinzugefügt werden soll. Front Door unterstützt die automatische Erkennung Ihrer Anwendungs-Back-Ends, wenn diese aus dem App Service, Cloud-Dienst oder Speicher stammen. Wenn Sie eine andere Ressource in Azure oder sogar ein Nicht-Azure-Back-End hinzufügen möchten, wählen Sie „Benutzerdefinierter Host“ aus. Hinweis: Während der Konfiguration wird von den APIs nicht überprüft, ob aus Front Door-Umgebungen auf das Back-End zugegriffen werden kann. Stattdessen müssen Sie sicherstellen, dass Ihr Back-End für Front Door zugänglich ist. 
2. **Abonnement und Back-End-Hostname:** Wenn Sie als Back-End-Hosttyp nicht „Benutzerdefinierter Host“ ausgewählt haben, müssen Sie eine Eingrenzung vornehmen und das Back-End auswählen. Wählen Sie dazu das entsprechende Abonnement und den entsprechenden Back-End-Hostnamen in der Benutzeroberfläche aus.
3. **Back-End-Hostheader:** Der Hostheaderwert, der für jede Anforderung an das Back-End gesendet wird. Weitere Einzelheiten hierzu finden Sie unter [Back-Endhost-Hostheader](#hostheader).
4. **Priorität:** Sie können Ihren verschiedenen Back-Ends Prioritäten zuweisen, wenn Sie ein primäres Dienst-Back-End für den gesamten Datenverkehr verwenden. Außerdem können Sie zusätzliche Back-Ends bereitstellen, falls das primäre Back-End oder die Ersatz-Back-Ends nicht verfügbar sind. Weitere Informationen hierzu finden Sie unter [Priorität](front-door-routing-methods.md#priority).
5. **Gewichtung:** Sie können Ihren verschiedenen Back-Ends Gewichtungen zuweisen, wenn Sie Datenverkehr entweder gleichmäßig oder gemäß Gewichtungskoeffizienten auf eine Gruppe von Back-Ends verteilen möchten. Weitere Informationen finden Sie unter [Gewichtungen](front-door-routing-methods.md#weighted).


### <a name = "hostheader"></a>Back-End-Hostheader

Von Front Door an ein Back-End weitergeleitete Anforderungen weisen ein Hostheaderfeld auf, anhand dessen das Back-End die Zielressource abruft. Der Wert für dieses Feld stammt in der Regel aus dem Back-End-URI und gibt Host und Port an. Eine für `www.contoso.com` gesendete Anforderung weist beispielsweise den Hostheader `www.contoso.com` auf. Wenn Sie Ihr Back-End mit Azure-Portal konfigurieren, wird in diesem Feld standardmäßig der Hostname des Back-Ends eingetragen. Wenn Ihr Back-End beispielsweise `contoso-westus.azurewebsites.net` ist, wird im Azure-Portal automatisch der Wert `contoso-westus.azurewebsites.net` für den Back-End-Hostheader übernommen. 
</br>Verwenden Sie hingegen Resource Manager-Vorlagen oder andere Mechanismen und legen Sie dieses Feld nicht explizit fest, sendet Front Door den eingehenden Hostnamen als Wert für den Hostheader. Wurde die Anforderung beispielsweise für `www.contoso.com` gesendet und Ihr Back-End ist `contoso-westus.azurewebsites.net`, wobei das Feld für den Back-End-Hostheader leer ist, legt Front Door den Hostheader auf `www.contoso.com` fest.

Bei den meisten Anwendungs-Back-Ends (wie Web-Apps, Blob Storage und Cloud Services) muss der Hostheader der Domäne des Back-Ends entsprechen. Der Front-End-Host, der an Ihr Back-End weiterleitet, hat jedoch einen anderen Hostnamen, z. B. „www\.contoso.azurefd.net“. Wenn das eingerichtete Back-End fordert, dass der Hostheader dem Hostnamen des Back-Ends entspricht, müssen Sie sicherstellen, dass das Feld „Back-End-Hostheader“ ebenfalls den Hostnamen des Back-Ends enthält.

#### <a name="configuring-the-backend-host-header-for-the-backend"></a>Konfigurieren des Back-End-Hostheaders für das Back-End
Das Feld „Back-End-Hostheader“ kann für ein Back-End im Abschnitt für Back-End-Pools konfiguriert werden.

1. Öffnen Sie die Front Door-Ressource, und klicken Sie auf den Back-End-Pool, der das zu konfigurierende Back-End enthält.

2. Fügen Sie ein Back-End hinzu, falls Sie noch keines hinzugefügt haben, oder bearbeiten Sie ein vorhandenes Back-End. Das Feld „Back-End-Hostheader“ kann auf einen benutzerdefinierten Wert festgelegt oder leer gelassen werden, was bedeutet, dass der Hostname für die eingehende Anforderung als Wert für den Hostheader verwendet wird.



## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über das [Erstellen einer Azure Front Door Service-Konfiguration](quickstart-create-front-door.md).
- Informieren Sie sich über die [Funktionsweise von Azure Front Door Service](front-door-routing-architecture.md).