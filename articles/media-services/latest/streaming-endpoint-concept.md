---
title: Streamingendpunkte in Azure Media Services | Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, was Streamingendpunkte sind und wie sie in Azure Media Services verwendet werden.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 01/16/2019
ms.author: juliako
ms.openlocfilehash: 18c5e48b5f7dbf664b607b8b83473a914256590b
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2019
ms.locfileid: "55104554"
---
# <a name="streaming-endpoints"></a>Streamingendpunkte

In Microsoft Azure Media Services (AMS) stellt die Entität [Streamingendpunkte](https://docs.microsoft.com/rest/api/media/streamingendpoints) einen Streamingdienst dar, der Inhalte zur weiteren Verteilung direkt für eine Clientwiedergabeanwendung oder ein Content Delivery Network (CDN) bereitstellen kann. Der ausgehende Stream des **Streamingendpunkt**-Diensts kann ein Livestream oder ein Video-on-Demand-Medienobjekt in Ihrem Media Services-Konto sein. Beim Erstellen eines Media Services-Kontos wird ein **Standard**-Streamingendpunkt mit dem Zustand „Beendet“ erstellt. Der **Standard**-Streamingendpunkt kann nicht gelöscht werden. Im Konto können zusätzliche Streamingendpunkte erstellt werden. 

> [!NOTE]
> Um das Streaming von Videos zu starten, muss der **Streamingendpunkt**, von dem aus Sie das Video streamen möchten, gestartet werden. 

## <a name="naming-convention"></a>Benennungskonvention

Für den Standardendpunkt: `{AccountName}-{DatacenterAbbreviation}.streaming.media.azure.net`

Für alle zusätzlichen Endpunkte: `{EndpointName}-{AccountName}-{DatacenterAbbreviation}.streaming.media.azure.net`

## <a name="types"></a>Typen  

Es gibt zwei **Streamingendpunkt**-Typen: **Standard** und **Premium**. Der Typ wird durch die Anzahl der Skalierungseinheiten (`scaleUnits`) definiert, die Sie für den Streamingendpunkt zuordnen. 

Die Typen werden in der folgenden Tabelle beschrieben:  

|Type|Skalierungseinheiten|BESCHREIBUNG|
|--------|--------|--------|  
|**Standard-Streamingendpunkt** (empfohlen)|0|Der **Standard**-Typ ist die empfohlene Option für nahezu alle Streamingszenarien und Zielgruppengrößen. Mit dem **Standard**-Typ wird die ausgehende Bandbreite automatisch skaliert. <br/>Für Kunden mit äußerst anspruchsvollen Anforderungen umfasst Media Services **Premium**-Streamingendpunkte, mit denen die Kapazität für die größten Internetzielgruppen horizontal hochskaliert werden kann. Wenn Sie von großen Zielgruppen und einer großen Anzahl von gleichzeitigen Benutzern ausgehen, wenden Sie sich unter amsstreaming@microsoft.com an uns, um Informationen darüber zu erhalten, ob Sie zum **Premium**-Typ wechseln müssen. |
|**Premium-Streamingendpunkt**|>0|**Premium**-Streamingendpunkte eignen sich für komplexere Workloads und bieten eine dedizierte und skalierbare Bandbreitenkapazität. Zum **Premium**-Typ wechseln Sie, indem Sie `scaleUnits` anpassen. `scaleUnits` stellen eine dedizierte Ausgangskapazität bereit, die in Schritten von jeweils 200 MBit/s erworben werden kann. Bei Verwendung des **Premium**-Typs stellt jede aktivierte Einheit zusätzliche Bandbreitenkapazität für die Anwendung bereit. |

## <a name="working-with-cdn"></a>Arbeiten mit dem CDN

In den meisten Fällen sollte das CDN aktiviert sein. Wenn Sie jedoch von einer maximalen Parallelität von weniger als 500 Benutzern ausgehen, wird empfohlen, das CDN zu deaktivieren, da sich das CDN am besten mit Parallelität skalieren lässt.

> [!NOTE]
> Der Streamingendpunkt `hostname` und die Streaming-URL ändern sich nicht, und zwar unabhängig davon, ob Sie CDN aktivieren oder nicht.

### <a name="detailed-explanation-of-how-caching-works"></a>Ausführliche Erläuterung der Funktionsweise der Zwischenspeicherung

Beim Hinzufügen des CDN gilt kein spezifischer Bandbreitenwert, da die erforderliche Bandbreite für einen CDN-aktivierten Streamingendpunkt variieren kann. Vieles hängt ab von der Art des Inhalts, davon, wie beliebt dieser ist, sowie von Bitraten und den Protokollen. Im CDN wird nur der Inhalt zwischengespeichert, der angefordert wird. Das heißt, dass beliebter Inhalt direkt über das CDN bereitgestellt wird – wenn das Videofragment zwischengespeichert ist. Liveinhalte werden wahrscheinlich zwischengespeichert, da normalerweise viele Benutzer sich genau dasselbe ansehen. On-Demand-Inhalte können sich etwas schwieriger gestalten, da beliebte Inhalte und weniger beliebte Inhalte enthalten sein können. Bei Millionen von Videoobjekten, von denen keine beliebt sind (nur 1 oder 2 Benutzer pro Woche), jedoch Tausenden von Benutzern, die sich alle unterschiedlichen Videos ansehen, wird das CDN viel weniger effektiv. Mit diesen Cachefehlern erhöht sich die Last für den Streamingendpunkt.
 
Sie müssen zudem die Funktionsweise des adaptiven Streamings berücksichtigen. Jedes einzelne Videofragment wird als eigene Entität zwischengespeichert. Wenn ein Benutzer beispielsweise bei der ersten Wiedergabe eines bestimmten Videos Sequenzen überspringt und sich nur einige wenige Sekunden hier und dort ansieht, werden nur die vom Benutzer angesehenen Videofragmente im CDN zwischengespeichert. Mit dem adaptiven Streaming gibt es normalerweise 5 bis 7 unterschiedliche Videobitraten. Wenn ein Benutzer eine Bitrate ansieht und ein anderer Benutzer eine andere Bitrate, werden die Bitraten jeweils separat im CDN zwischengespeichert. Auch wenn zwei Benutzer die gleiche Bitrate ansehen, kann es sein, dass das Streaming über unterschiedliche Protokolle erfolgt. Jedes Protokoll (HLS, MPEG-DASH, Smooth Streaming) wird separat zwischengespeichert. Somit werden jede Bitrate und jedes Protokoll separat zwischengespeichert, und nur die Videofragmente, die angefordert wurden, werden zwischengespeichert.
 
## <a name="properties"></a>Eigenschaften 

Dieser Abschnitt enthält detaillierte Informationen zu einigen Streamingendpunkt-Eigenschaften. Beispiele zum Erstellen eines neuen Streamingendpunkts und Beschreibungen aller Eigenschaften finden Sie unter [Streamingendpunkte](https://docs.microsoft.com/rest/api/media/streamingendpoints/create). 

|Eigenschaft|BESCHREIBUNG|  
|--------------|----------|
|`accessControl`|Wird zum Konfigurieren der folgenden Sicherheitseinstellungen für den Streamingendpunkt verwendet: Authentifizierungsschlüssel und IP-Adressen im Signaturheader von Akamai, über die eine Verbindung mit dem Endpunkt zulässig ist.<br />Diese Eigenschaft kann festgelegt werden, wenn `cdnEnabled` auf FALSE festgelegt ist.|  
|`cdnEnabled`|Gibt an, ob die Azure CDN-Integration für den Streamingendpunkt aktiviert oder deaktiviert ist (standardmäßig deaktiviert).<br /><br /> Wenn Sie `cdnEnabled` auf TRUE festlegen, werden die folgenden Konfigurationen deaktiviert: `customHostNames` und `accessControl`.<br /><br />Nicht alle Rechenzentren unterstützen die Azure CDN-Integration. Gehen Sie wie folgt vor, um zu überprüfen, ob die Azure CDN-Integration in Ihrem Rechenzentrum zur Verfügung steht:<br /><br /> - Versuchen Sie, `cdnEnabled` auf TRUE festzulegen.<br /><br /> - Überprüfen Sie das zurückgegebene Ergebnis auf einen `HTTP Error Code 412`-Fehler (PreconditionFailed) mit der Meldung „Streaming endpoint CdnEnabled property cannot be set to true as CDN capability is not available in the current region“ (Eigenschaft CdnEnabled des Streamingendpunkts kann nicht auf TRUE festgelegt werden, da die CDN-Funktion in der aktuellen Region nicht verfügbar ist).<br /><br /> Wenn dieser Fehler angezeigt wird, wird das CDN im Rechenzentrum nicht unterstützt. Versuchen Sie es mit einem anderen Rechenzentrum.|  
|`cdnProfile`|Wenn `cdnEnabled` auf TRUE festgelegt ist, können Sie auch `cdnProfile`-Werte übergeben. `cdnProfile` ist der Name des CDN-Profils, in dem der CDN-Endpunkt erstellt wird. Sie können eine vorhandene cdnProfile-Eigenschaft angeben oder eine neue verwenden. Wenn der Wert NULL ist und `cdnEnabled` auf TRUE festgelegt ist, wird der Standardwert „AzureMediaStreamingPlatformCdnProfile“ verwendet. Wenn die angegebene `cdnProfile`-Eigenschaft bereits vorhanden ist, wird ein Endpunkt unter dieser Eigenschaft erstellt. Wenn das Profil nicht vorhanden ist, wird automatisch ein neues Profil erstellt.|
|`cdnProvider`|Wenn das CDN aktiviert ist, können Sie auch `cdnProvider`-Werte übergeben. `cdnProvider` steuert, welcher Anbieter verwendet wird. Derzeit werden drei Werte unterstützt: „StandardVerizon“, „PremiumVerizon“ und „StandardAkamai“. Wenn kein Wert angegeben wird und `cdnEnabled` auf TRUE festgelegt ist, wird „StandardVerizon“ verwendet (d.h. der Standardwert).|
|`crossSiteAccessPolicies`|Wird zum Angeben von websiteübergreifenden Zugriffsrichtlinien für verschiedene Clients verwendet. Weitere Informationen finden Sie unter [Cross-domain policy file specification](http://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html) (Dateispezifikation für domänenübergreifende Richtlinien) und [Making a Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955\(v=vs.95\).aspx) (Verfügbarmachen eines Diensts über Netzwerkgrenzen hinweg).|  
|`customHostNames`|Wird verwendet, um einen Streamingendpunkt so zu konfigurieren, dass an einen benutzerdefinierten Hostnamen gerichteter Datenverkehr akzeptiert wird. Dies ermöglicht eine einfachere Datenverkehr-Verwaltungskonfiguration über einen globalen Traffic Manager (GTM) und ebenso für mit Branding versehene Domänennamen, die als Name des Streamingendpunkts verwendet werden.<br /><br /> Der Besitz des Domänennamens muss in Azure Media Services bestätigt werden. In Azure Media Services wird der Besitz des Domänennamens durch Anfordern eines `CName`-Eintrags überprüft, der die Azure Media Services-Konto-ID als Komponente enthält, die der verwendeten Domäne hinzuzufügen ist. Beispiel: Damit „sports.contoso.com“ als benutzerdefinierter Hostname für den Streamingendpunkt verwendet wird, muss ein Eintrag für `<accountId>.contoso.com` so konfiguriert werden, dass er auf einen der Media Services-Überprüfungshostnamen verweist. Der Überprüfungshostname setzt sich aus „verifydns.\<mediaservices-dns-zone>“ zusammen. Im Folgenden sind die erwarteten DNS-Zonen aufgeführt, die im Überprüfungseintrag für verschiedene Azure-Regionen zu verwenden sind.<br /><br /> Nordamerika, Europa, Singapur, Hongkong, Japan:<br /><br /> - mediaservices.windows.net<br /><br /> - verifydns.mediaservices.windows.net<br /><br /> China:<br /><br /> - mediaservices.chinacloudapi.cn<br /><br /> - verifydns.mediaservices.chinacloudapi.cn<br /><br /> Ein `CName`-Eintrag, der eine Zuordnung von „945a4c4e-28ea-45cd-8ccb-a519f6b700ad.contoso.com“ zu „verifydns.mediaservices.windows.net“ vornimmt, beweist z.B., dass die Azure Media Services-ID 945a4c4e-28ea-45cd-8ccb-a519f6b700ad den Besitz der Domäne contoso.com hat, sodass alle Namen unter contoso.com als benutzerdefinierter Hostname für einen Streamingendpunkt in diesem Konto verwendet werden können.<br /><br /> Um den Wert der Mediendienst-ID zu suchen, wählen Sie im [Azure-Portal](https://portal.azure.com/) Ihr Mediendienstkonto aus. Die MEDIENDIENST-ID wird rechts auf der Seite DASHBOARD angezeigt.<br /><br /> **Warnung**: Wenn versucht wird, einen benutzerdefinierten Hostnamen festzulegen, ohne dass der `CName`-Eintrag ordnungsgemäß überprüft wird, treten bei der DNS-Antwort Fehler auf, und die Antwort wird dann einige Zeit zwischengespeichert. Nachdem ein entsprechender Eintrag eingerichtet wurde, kann es eine Weile dauern, bis die zwischengespeicherte Antwort erneut überprüft wird. Abhängig vom DNS-Anbieter der benutzerdefinierten Domäne kann die erneute Überprüfung des Eintrags zwischen einigen Minuten bis zu einer Stunde dauern.<br /><br /> Zusätzlich zu dem `CName`-Eintrag, der die Zuordnung von `<accountId>.<parent domain>` zu `verifydns.<mediaservices-dns-zone>` definiert, müssen Sie einen anderen `CName`-Eintrag erstellen, der den benutzerdefinierten Hostnamen (z.B. `sports.contoso.com`) dem Hostnamen des Media Services-Streamingendpunkts zuordnet (z.B. `amstest.streaming.mediaservices.windows.net`).<br /><br /> **Hinweis**: Für Streamingendpunkte, die sich im selben Rechenzentrum befinden, kann nicht der gleiche benutzerdefinierte Hostname verwendet werden.<br /> Diese Eigenschaft gilt für Standard- und Premium-Streamingendpunkte und kann festgelegt werden, wenn cdnEnabled FALSE ist.<br/><br/> AMS unterstützt derzeit SSL mit benutzerdefinierten Domänen nicht.  |  
|`maxCacheAge`|Überschreibt den standardmäßigen Max-Age-HTTP-Cache Control Header, der vom Streamingendpunkt für Medienfragmente und On-Demand-Manifeste festgelegt wird. Der Wert wird in Sekunden festgelegt.|
|`resourceState`|Für diese Eigenschaft sind folgende Werte verfügbar:<br /><br /> – Stopped: Der ursprüngliche Status eines Streamingendpunkts nach der Erstellung.<br /><br /> – Starting: Der Streamingendpunkt geht über in den Status „Running“.<br /><br /> – Running: Der Streamingendpunkt kann Inhalte an Clients streamen.<br /><br /> – Scaling: Die Skalierungseinheiten werden erhöht oder verringert.<br /><br /> – Stopping: Der Streamingendpunkt geht über in den Status „Stopped“.<br/><br/> – Deleting: Der Streamingendpunkt wird gelöscht.|
|`scaleUnits `|Skalierungseinheiten stellen eine dedizierte Ausgangskapazität bereit, die in Schritten von jeweils 200 MBit/s erworben werden kann. Wenn Sie zum **Premium**-Type wechseln möchten, passen Sie `scaleUnits` an. |

## <a name="next-steps"></a>Nächste Schritte

Mit dem Beispiel [in diesem Repository](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs) wird gezeigt, wie der Standard-Streamingendpunkt mit .NET gestartet wird.

