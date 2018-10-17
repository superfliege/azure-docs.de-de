---
title: 'Beispiel: Erste Schritte – Knowledge Exploration Service-API'
titlesuffix: Azure Cognitive Services
description: Verwenden Sie die KES-API (Knowledge Exploration Service) zum Erstellen einer Engine für eine interaktive Suche in akademischen Veröffentlichungen.
services: cognitive-services
author: bojunehsu
manager: cgronlun
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: sample
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 6cee339793269af0e8060cce56f94fa81db6a6c5
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "46124013"
---
# <a name="get-started-with-the-knowledge-exploration-service"></a>Erste Schritte mit dem Knowledge Exploration Service

In dieser exemplarischen Vorgehensweise verwenden Sie den Knowledge Exploration Service (KES), um die Engine für eine interaktive Suche in akademischen Veröffentlichungen zu erstellen. Sie können das Befehlszeilentool [`kes.exe`](CommandLine.md) und alle Beispieldateien aus dem [Knowledge Exploration Service SDK](https://www.microsoft.com/en-us/download/details.aspx?id=51488) installieren.

Das Beispiel der akademischen Veröffentlichungen enthält eine Stichprobe von 1.000 akademischen Dokumenten, die Forscher von Microsoft veröffentlicht haben.  Jedem Dokument sind Titel, Veröffentlichungsjahr, Autoren und Schlüsselwörter zugeordnet. Jeder Autor wird durch ID, Name und Zugehörigkeit zum Zeitpunkt der Veröffentlichung dargestellt. Jedes Schlüsselwort kann einer Gruppe von Synonymen zugeordnet werden (z.B. kann das Schlüsselwort „support vector machine“ dem Synonym „svm“ zugeordnet werden).

## <a name="define-the-schema"></a>Definieren des Schemas

Das Schema beschreibt die Attributstruktur der Objekte in der Domäne. Es gibt den Namen und den Datentyp für jedes Attribut in einem JSON-Dateiformat an. Das folgende Beispiel zeigt den Inhalt der Datei *Academic.schema*.

```json
{
  "attributes":[
    {"name":"Title", "type":"String"},
    {"name":"Year", "type":"Int32"},
    {"name":"Author", "type":"Composite"},
    {"name":"Author.Id", "type":"Int64", "operations":["equals"]},
    {"name":"Author.Name", "type":"String"},
    {"name":"Author.Affiliation", "type":"String"},
    {"name":"Keyword", "type":"String", "synonyms":"Keyword.syn"}
  ]
}
```

Hier definieren Sie *Title*, *Year* und *Keyword* jeweils als Zeichenfolge, ganze Zahl und Zeichenfolgenattribut. Da Autoren durch ID, Name und Zugehörigkeit dargestellt werden, definieren Sie *Author* als zusammengesetztes Attribut mit drei untergeordneten Attributen: *Author.Id*, *Author.Name* und *Author.Affiliation*.

Attribute unterstützen standardmäßig alle für ihren Datentyp verfügbaren Vorgänge einschließlich *equals*, *starts_with* und *is_between*. Da die Autoren-ID nur intern als Bezeichner verwendet wird, überschreiben Sie die Standardeinstellung, und geben Sie *equals* als einzigen indizierten Vorgang an.

Lassen Sie für das *Keyword*-Attribut durch Angabe der Synonymdatei *Keyword.syn* in der Attributdefinition Synonyme zu, die mit den kanonischen Schlüsselwortwerten übereinstimmen. Diese Datei enthält eine Liste der kanonischen und Synonymwertpaare:

```json
...
["support vector machine","support vector machines"]
["support vector machine","support vector networks"]
["support vector machine","support vector regression"]
["support vector machine","support vector"]
["support vector machine","svm machine learning"]
["support vector machine","svm"]
["support vector machine","svms"]
["support vector machine","vector machine"]
...
```

Weitere Informationen zur Schemadefinition finden Sie unter [Schemaformat](SchemaFormat.md).

## <a name="generate-data"></a>Generieren von Daten

Die Datendatei beschreibt die Liste der zu indizierenden Veröffentlichungen, wobei jede Zeile die Attributwerte eines Dokuments im [JSON-Format](http://json.org/) angibt.  Das folgende Beispiel ist eine einzelne, zur besseren Lesbarkeit formatierte Zeile aus der Datendatei *Academic.data*:

```
...
{
    "logprob": -16.714,
    "Title": "the world wide telescope",
    "Year": 2001,
    "Author": [
        {
            "Id": 717694024,
            "Name": "alexander s szalay",
            "Affiliation": "microsoft"
        },
        {
            "Id": 2131537204,
            "Name": "jim gray",
            "Affiliation": "microsoft"
        }
    ]
}
...
```

Geben Sie in diesem Codeausschnitt das *Title*- und *Year*-Attribut des Dokuments als JSON-Zeichenfolge bzw. -Zahl an. Mehrere Werte werden mithilfe von JSON-Arrays dargestellt. Da *Author* ein zusammengesetztes Attribut ist, wird jeder Wert mit einem JSON-Objekt dargestellt, das aus seinen untergeordneten Attributen besteht. Attribute mit fehlenden Werten, wie in diesem Fall *Keyword*, können aus der JSON-Darstellung ausgeschlossen werden.

Um die Wahrscheinlichkeit unterschiedlicher Dokumente zu differenzieren, geben Sie die relative Logarithmuswahrscheinlichkeit mithilfe des integrierten Attributs *logprob* an. Bei einer Wahrscheinlichkeit *p* zwischen 0 und 1 berechnen Sie die entsprechende Logarithmuswahrscheinlichkeit als log(*p*), wobei log() die natürliche Logarithmusfunktion ist.

Weitere Informationen finden Sie unter [Datenformat](DataFormat.md).

## <a name="build-a-compressed-binary-index"></a>Erstellen eines komprimierten binären Indexes

Sobald Sie über eine Schemadatei und Datendatei verfügen, können Sie mit [`kes.exe build_index`](CommandLine.md#build_index-command) einen komprimierten binären Index der Datenobjekte erstellen. In diesem Beispiel erstellen Sie die Indexdatei *Academic.index* aus der Eingabeschemadatei *Academic.schema* und der Datendatei *Academic.data*. Verwenden Sie den folgenden Befehl:

`kes.exe build_index Academic.schema Academic.data Academic.index`

Für die schnelle Prototyperstellung außerhalb von Azure kann [`kes.exe build_index`](CommandLine.md#build_index-command) kleine Indizes lokal aus Datendateien mit bis zu 10.000 Objekten erstellen. Für größere Datendateien können Sie entweder den Befehl von einer [Windows-VM in Azure](../../../articles/virtual-machines/windows/quick-create-portal.md) ausführen oder einen Remotebuild in Azure durchführen. Weitere Informationen finden Sie unter [Zentrales Hochskalieren zum Hosten größerer Indizes](#scaling-up).

## <a name="use-an-xml-grammar-specification"></a>Verwenden einer XML-Grammatikspezifikation

Die Grammatik gibt den Satz von Abfragen in natürlicher Sprache an, die der Dienst interpretieren kann, und wie diese Abfragen in natürlicher Sprache in Semantikabfrageausdrücke konvertiert werden. In diesem Beispiel verwenden Sie die in *academic.xml* angegebene Grammatik:

```xml
<grammar root="GetPapers">

  <!-- Import academic data schema-->
  <import schema="Academic.schema" name="academic"/>

  <!-- Define root rule-->
  <rule id="GetPapers">
    <example>papers about machine learning by michael jordan</example>

    papers
    <tag>
      yearOnce = false;
      isBeyondEndOfQuery = false;
      query = All();
    </tag>

    <item repeat="1-" repeat-logprob="-10">
      <!-- Do not complete additional attributes beyond end of query -->
      <tag>AssertEquals(isBeyondEndOfQuery, false);</tag>

      <one-of>
        <!-- about <keyword> -->
        <item logprob="-0.5">
          about <attrref uri="academic#Keyword" name="keyword"/>
          <tag>query = And(query, keyword);</tag>
        </item>

        <!-- by <authorName> [while at <authorAffiliation>] -->
        <item logprob="-1">
          by <attrref uri="academic#Author.Name" name="authorName"/>
          <tag>authorQuery = authorName;</tag>
          <item repeat="0-1" repeat-logprob="-1.5">
            while at <attrref uri="academic#Author.Affiliation" name="authorAffiliation"/>
            <tag>authorQuery = And(authorQuery, authorAffiliation);</tag>
          </item>
          <tag>
            authorQuery = Composite(authorQuery);
            query = And(query, authorQuery);
          </tag>
        </item>

        <!-- written (in|before|after) <year> -->
        <item logprob="-1.5">
          <!-- Allow this grammar path to be traversed only once -->
          <tag>
            AssertEquals(yearOnce, false);
            yearOnce = true;
          </tag>
          <ruleref uri="#GetPaperYear" name="year"/>
          <tag>query = And(query, year);</tag>
        </item>
      </one-of>

      <!-- Determine if current parse position is beyond end of query -->
      <tag>isBeyondEndOfQuery = GetVariable("IsBeyondEndOfQuery", "system");</tag>
    </item>
    <tag>out = query;</tag>
  </rule>

  <rule id="GetPaperYear">
    <tag>year = All();</tag>
    written
    <one-of>
      <item>
        in <attrref uri="academic#Year" name="year"/>
      </item>
      <item>
        before
        <one-of>
          <item>[year]</item>
          <item>
            <attrref uri="academic#Year" op="lt" name="year"/>
          </item>
        </one-of>
      </item>
      <item>
        after
        <one-of>
          <item>[year]</item>
          <item>
            <attrref uri="academic#Year" op="gt" name="year"/>
          </item>
        </one-of>
      </item>
    </one-of>
    <tag>out = year;</tag>
  </rule>
</grammar>
```

Weitere Informationen zur Syntax der Grammatikspezifikation finden Sie unter [Grammatikformat](GrammarFormat.md).

## <a name="compile-the-grammar"></a>Kompilieren der Grammatik

Sobald Sie über eine XML-Grammatikspezifikation verfügen, können Sie sie mithilfe von [`kes.exe build_grammar`](CommandLine.md#build_grammar-command) in eine binäre Grammatik kompilieren. Beachten Sie: Wenn die Grammatik ein Schema importiert, muss die Schemadatei sich im gleichen Pfad wie die Grammatik-XML-Datei befinden. In diesem Beispiel erstellen Sie die binäre Grammatikdatei *Academic.grammar* aus der XML-Grammatikeingabedatei *Academic.xml*. Verwenden Sie den folgenden Befehl:

`kes.exe build_grammar Academic.xml Academic.grammar`

## <a name="host-the-grammar-and-index-in-a-web-service"></a>Hosten von Grammatik und Index in einem Webdienst

Für die schnelle Prototyperstellung können Sie mithilfe von [`kes.exe host_service`](CommandLine.md#host_service-command) Grammatik und Index auf dem lokalen Computer in einem Webdienst hosten. Anschließend können Sie zum Überprüfen von Datenrichtigkeit und Grammatikentwurf über [Web-APIs](WebAPI.md) auf den Dienst zugreifen. In diesem Beispiel hosten Sie die Grammatikdatei *Academic.grammar* und die Indexdatei *Academic.index* auf http://localhost:8000/. Verwenden Sie den folgenden Befehl:

`kes.exe host_service Academic.grammar Academic.index --port 8000`

Hierdurch wird eine lokale Instanz des Webdiensts initiiert. Sie können den Dienst interaktiv testen, indem Sie `http::localhost:<port>` in einem Browser besuchen. Weitere Informationen finden Sie unter [Testen des Diensts](#testing-service).

Sie können verschiedene [Web-APIs](WebAPI.md) auch direkt zum Testen der Interpretation natürlicher Sprache, zur Abfragefertigstellung, strukturierten Abfrageauswertung und Histogrammberechnung aufrufen. Um den Dienst zu beenden, geben Sie „quit“ in der `kes.exe host_service`-Eingabeaufforderung ein, oder drücken Sie STRG+C. Hier einige Beispiele:

* [http://localhost:8000/interpret?query=papers by susan t dumais](http://localhost:8000/interpret?query=papers%20by%20susan%20t%20dumais)
* [http://localhost:8000/interpret?query=papers by susan t d&complete=1](http://localhost:8000/interpret?query=papers%20by%20susan%20t%20d&complete=1)
* [http://localhost:8000/evaluate?expr=Composite(Author.Name=='susan t dumais')&attributes=Title,Year,Author.Name,Author.Id&count=2](http://localhost:8000/evaluate?expr=Composite%28Author.Name==%27susan%20t%20dumais%27%29&attributes=Title,Year,Author.Name,Author.Id&count=2)
* [http://localhost:8000/calchistogram?expr=And(Composite(Author.Name=='susan t dumais'),Year>=2013)&attributes=Year,Keyword&count=4](http://localhost:8000/calchistogram?expr=And%28Composite%28Author.Name=='susan%20t%20dumais'%29,Year>=2013%29&attributes=Year,Keyword&count=4)

Außerhalb von Azure ist [`kes.exe host_service`](CommandLine.md#host_service-command) auf Indizes von bis zu 10.000 Objekten beschränkt. Andere Grenzwerte enthalten eine API-Rate von 10 Anforderungen pro Sekunde und insgesamt 1.000 Anforderungen, bevor der Prozess automatisch beendet wird. Um diese Einschränkungen zu umgehen, führen Sie den Befehl von einer [Windows-VM in Azure](../../../articles/virtual-machines/windows/quick-create-portal.md) aus, oder führen Sie mit dem [`kes.exe deploy_service`](CommandLine.md#deploy_service-command)-Befehl eine Bereitstellung für einen Azure-Clouddienst durch. Weitere Informationen finden Sie unter [Bereitstellen des Diensts](#deploying-service).

## <a name="scale-up-to-host-larger-indices"></a>Zentrales Hochskalieren zum Hosten größerer Indizes

Beim Ausführen von `kes.exe` außerhalb von Azure ist der Index auf 10.000 Objekte beschränkt. Mit Azure können Sie größere Indizes erstellen und hosten. Registrieren Sie sich für eine kostenlose [Testversion](https://azure.microsoft.com/pricing/free-trial/). Alternativ können Sie, wenn Sie Visual Studio oder MSDN abonnieren, [Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Diese bestehen in monatlichen Azure-Gutschriften.

Um `kes.exe` den Zugriff auf ein Azure-Konto zu ermöglichen, [laden Sie die Azure-Veröffentlichungseinstellungsdatei](https://portal.azure.com/#blade/Microsoft_Azure_ClassicResources/PublishingProfileBlade) aus dem Azure-Portal herunter. Wenn Sie aufgefordert werden, melden Sie sich beim gewünschten Azure-Konto an. Speichern Sie die Datei unter *AzurePublishSettings.xml* in dem Arbeitsverzeichnis, von wo aus `kes.exe` ausgeführt wird.

Es gibt zwei Möglichkeiten zum Erstellen und Hosten großer Indizes. Die erste ist, Schema- und Datendateien in einer Windows-VM in Azure vorzubereiten. Führen Sie dann [`kes.exe build_index`](#building-index) aus, um den Index lokal auf dem virtuellen Computer ohne jede Größeneinschränkung zu erstellen. Der resultierende Index kann lokal auf dem virtuellen Computer mithilfe von [`kes.exe host_service`](#hosting-service) für schnelle Prototyperstellung gehostet werden, wiederum ohne jede Einschränkung. Ausführliche Schritte finden Sie unter [Schnellstart: Erstellen eines virtuellen Windows-Computers im Azure-Portal](../../../articles/virtual-machines/windows/quick-create-portal.md).

Die zweite Methode ist das Ausführen eines Azure-Remotebuilds mit [`kes.exe build_index`](CommandLine.md#build_index-command) und dem `--remote`-Parameter. Hiermit wird eine Azure-VM-Größe angegeben. Wenn der `--remote`-Parameter angegeben wird, erstellt der Befehl eine temporäre Azure-VM dieser Größe. Anschließend erstellt er den Index auf dem virtuellen Computer, lädt den Index in den Zielblobspeicher hoch und löscht den virtuellen Computer nach Fertigstellung. Ihrem Azure-Abonnement werden die Kosten für den virtuellen Computer in Rechnung gestellt, während der Index erstellt wird.

Dank dieser Azure-Remotebuildfunktion kann [`kes.exe build_index`](CommandLine.md#build_index-command) in jeder Umgebung ausgeführt werden. Wenn Sie einen Remotebuild durchführen, können Eingabeschema und Datenargumente lokale Dateipfade oder [Azure-Blobspeicher](../../storage/blobs/storage-dotnet-how-to-use-blobs.md)-URLs sein. Das Ausgabeindexargument muss eine Blobspeicher-URL sein. Informationen zum Erstellen eines Azure-Speicherkontos finden Sie unter [Informationen zu Azure-Speicherkonten](../../storage/common/storage-create-storage-account.md). Um Dateien effizient in und aus Blobspeicher zu kopieren, verwenden Sie das Hilfsprogramm [AzCopy](../../storage/common/storage-use-azcopy.md).

In diesem Beispiel können Sie voraussetzen, dass folgender Blobspeichercontainer bereits erstellt wurde: http://&lt;*Konto*&gt;.blob.core.windows.net/&lt;*Container* &gt;/. Er enthält das Schema *Academic.schema*, die Synonymdatei *Keywords.syn*, auf die verwiesen wird, und die umfassende Datendatei *Academic.full.data*. Sie können den vollständigen Index mit folgendem Befehl remote erstellen:

`kes.exe build_index http://<account>.blob.core.windows.net/<container>/Academic.schema http://<account>.blob.core.windows.net/<container>/Academic.full.data http://<account>.blob.core.windows.net/<container>/Academic.full.index --remote <vm_size>`

Beachten Sie, dass es 5 bis 10 Minuten dauern kann, eine temporäre VM zum Erstellen des Indexes bereitzustellen. Für die schnelle Prototyperstellung können Sie:
- Mit einem kleineren Dataset lokal auf jedem Computer entwickeln.
- Manuell [eine Azure-VM erstellen](../../../articles/virtual-machines/windows/quick-create-portal.md), [eine Verbindung mit ihr](../../../articles/virtual-machines/windows/quick-create-portal.md#connect-to-virtual-machine) über Remotedesktop herstellen, das [Knowledge Exploration Service SDK](https://www.microsoft.com/en-us/download/details.aspx?id=51488) installieren und [`kes.exe`](CommandLine.md) vom virtuellen Computer ausführen.

Auslagern verlangsamt den Erstellungsprozess. Um Auslagern zu vermeiden, verwenden Sie einen virtuellen Computer, dessen Arbeitsspeicher dem Dreifachen der Größe der Eingabedatei für die Indexerstellung entspricht. Verwenden Sie einen virtuellen Computer, dessen Arbeitsspeicher um 1 GB größer ist als der Index zum Hosten. Eine Liste der verfügbaren VM-Größen finden Sie unter [Größen für virtuelle Computer](../../../articles/virtual-machines/virtual-machines-windows-sizes.md).

## <a name="deploy-the-service"></a>Bereitstellen des Diensts

Sobald Sie über eine Grammatik und einen Index verfügen, können Sie den Dienst für einen Azure-Clouddienst bereitstellen. Wie Sie einen neuen Azure-Clouddienst erstellen, erfahren Sie unter [Erstellen und Bereitstellen eines Clouddiensts](../../../articles/cloud-services/cloud-services-how-to-create-deploy-portal.md). Geben Sie zu diesem Zeitpunkt kein Bereitstellungspaket an.  

Wenn Sie den Clouddienst erstellt haben, können Sie [`kes.exe deploy_service`](CommandLine.md#deploy_service-command) zur Bereitstellung des Diensts verwenden. Ein Azure-Clouddienst verfügt über zwei Bereitstellungsslots: Produktion und Staging. Für einen Dienst, der Live-Benutzerdatenverkehr empfängt, sollten Sie zunächst im Stagingslot bereitstellen. Warten Sie, bis der Dienst startet und sich selbst initialisiert. Anschließend können Sie einige Anforderungen senden, um die Bereitstellung zu überprüfen und sicherzustellen, dass er grundlegende Tests besteht.

[Swappen](../../../articles/cloud-services/cloud-services-nodejs-stage-application.md) Sie die Inhalte von Staging- und Produktionsslot, sodass Livedatenverkehr jetzt zum neu bereitgestellten Dienst weitergeleitet wird. Sie können diesen Vorgang wiederholen, wenn Sie eine aktualisierte Version des Diensts mit neuen Daten bereitstellen. Wie bei allen anderen Azure-Clouddiensten können Sie optional das Azure-Portal verwenden, um die [automatische Skalierung](../../../articles/cloud-services/cloud-services-how-to-scale-portal.md) zu konfigurieren.

In diesem Beispiel stellen Sie den *Academic*-Index dem Stagingslot eines vorhandenen Clouddiensts mit *<vm_size >* VMs bereit. Verwenden Sie den folgenden Befehl:

`kes.exe deploy_service http://<account>.blob.core.windows.net/<container>/Academic.grammar http://<account>.blob.core.windows.net/<container>/Academic.index <serviceName> <vm_size> --slot Staging`

Eine Liste der verfügbaren VM-Größen finden Sie unter [Größen für virtuelle Computer](../../../articles/virtual-machines/virtual-machines-windows-sizes.md).

Nach der Bereitstellung des Diensts können Sie die verschiedenen [Web-APIs](WebAPI.md) zum Testen der Interpretation natürlicher Sprache, zur Abfragefertigstellung, strukturierten Abfrageauswertung und Histogrammberechnung aufrufen.  

## <a name="test-the-service"></a>Testen des Diensts

Navigieren Sie zum Debuggen eines Livediensts über einen Webbrowser zum Hostcomputer. Für einen über [host_service](#hosting-service) bereitgestellten lokalen Dienst besuchen Sie `http://localhost:<port>/`.  Für einen über [deploy_service](#deploying-service) bereitgestellten Azure-Clouddienst besuchen Sie `http://<serviceName>.cloudapp.net/`.

Diese Seite enthält sowohl einen Link zu Informationen über grundlegende Statistiken zu API-Aufrufen als auch die Grammatik und den Index, die von diesem Dienst gehostet werden. Diese Seite enthält auch eine interaktive Suchschnittstelle, die die Verwendung von Web-APIs veranschaulicht. Geben Sie Abfragen in das Suchfeld ein, um die Ergebnisse der Aufrufe von [interpret](interpretMethod.md)-, [evaluate](evaluateMethod.md)- und [calchistogram](calchistogramMethod.md)-API zu sehen. Die dieser Seite zugrunde liegende HTML-Quelle dient auch als ein Beispiel zum Integrieren von Web-APIs in eine App, um eine umfassende und interaktive Suchfunktion zu erstellen.


