---
title: Anwendungsszenarien und -entwurf | Microsoft Docs
description: Übersicht über die Kategorien von Cloudanwendungen in Service Fabric. Informationen zum Anwendungsentwurf mit zustandsbehafteten und zustandslosen Diensten
services: service-fabric
documentationcenter: .net
author: athinanthny
manager: chackdan
editor: ''
ms.assetid: 3a8ca6ea-b8e9-4bc3-9e20-262437d2528e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 4/24/2019
ms.author: atsenthi
ms.openlocfilehash: f7ad295eb30d257cd195ae02d5a5b1d85de76bda
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2019
ms.locfileid: "65228504"
---
# <a name="service-fabric-application-scenarios"></a>Service Fabric-Anwendungsszenarien
Azure Service Fabric bietet eine zuverlässige und flexible Plattform, mit der Sie verschiedenste Geschäftsanwendungen und -dienste erstellen und ausführen können. Diese Anwendungen und Microservices können zustandslos oder zustandsbehaftet sein, und ihre Ressourcen werden zur Maximierung der Effizienz auf die virtuellen Maschinen verteilt. In der einzigartigen Architektur von Service Fabric können Sie Echtzeitdatenanalysen, In-Memory-Berechnungen, parallele Transaktionen und die Ereignisverarbeitung in Ihren Anwendungen ausführen. Sie können Ihre Anwendungen je nach den sich ändernden Ressourcenanforderungen problemlos zentral (bzw. horizontal oder vertikal) hoch- oder herunterskalieren.

Einen Entwurfsleitfaden für Anwendungen finden Sie unter [Microservices-Architektur in Azure Service Fabric](https://docs.microsoft.com/azure/architecture/reference-architectures/microservices/service-fabric) und [Best Practices für den Anwendungsentwurf mit Service Fabric](service-fabric-best-practices-applications.md).

Die Service Fabric-Plattform ist ideal für die folgenden Typen von Anwendungen geeignet:

* **Datensammlung, -verarbeitung und IoT:** Da Service Fabric durch seine zustandsbehafteten Dienste große Datenmengen mit geringer Latenz verarbeitet, eignet es sich ideal für die Datenverarbeitung auf Millionen von Geräten, auf denen die Daten für das Gerät und die Berechnung zusammengestellt werden.

    Zu den Kunden, die IoT-Dienste mit Service Fabric erstellt haben, gehören [Honeywell](https://customers.microsoft.com/story/honeywell-builds-microservices-based-thermostats-on-azure), [PCL Construction](https://customers.microsoft.com/story/pcl-construction-professional-services-azure), [Crestron](https://customers.microsoft.com/story/crestron-partner-professional-services-azure),  [BMW](https://customers.microsoft.com/story/bmw-enables-driver-mobility-via-azure-service-fabric/), [Schneider Electric](https://customers.microsoft.com/story/schneider-electric-powers-engergy-solutions-on-azure-service-fabric), und [Mesh Systems](https://customers.microsoft.com/story/mesh-systems-lights-up-the-market-with-iot-based-azure-solutions).

* **Gaming- und sitzungsbasierte interaktive Anwendungen**: Service Fabric ist ideal, wenn Ihre Anwendung eine geringe Latenz bei Lese- und Schreibvorgängen erfordert, wie beispielsweise bei Online-Spielen oder Instant Messaging. Mit Service Fabric können Sie diese interaktiven, zustandsbehafteten Anwendungen erstellen, ohne einen separaten Speicher oder Cache erstellen zu müssen. Lesen Sie [Gaminglösungen von Azure](https://azure.microsoft.com/solutions/gaming/). Dort finden Sie einen Entwurfsleitfaden zum [Verwenden von Service Fabric in Gamingdiensten](https://docs.microsoft.com/gaming/azure/reference-architectures/multiplayer-synchronous-sf).

    Zu den Kunden, die Gamingdienste mit erstellt haben, gehören [Next Games](https://customers.microsoft.com/story/next-games-media-telecommunications-azure) und [Digamore](https://customers.microsoft.com/story/digamore-entertainment-scores-with-a-new-gaming-platform-based-on-azure-service-fabric/). Zu den Kunden, die interaktive Sitzungen erstellt haben, gehören [Honeywell mit Hololens](https://customers.microsoft.com/story/honeywell-manufacturing-hololens).

* **Datenanalysen und Workflowverarbeitung**: Anwendungen, die Ereignisse oder Datenströme zuverlässig verarbeiten müssen, profitieren zudem von den optimierten Lese- und Schreibvorgängen in Service Fabric. Darüber hinaus unterstützt Service Fabric Pipelines für die Anwendungsverarbeitung, bei denen Ergebnisse zuverlässig sein und ohne Datenverlust in die nächste Verarbeitungsphase weitergeleitet werden müssen. Dazu zählen Transaktions- und Finanzsysteme, bei denen Datenkonsistenz und Berechnungsgewährleistung von größter Wichtigkeit sind.

    Zu den Kunden, die Geschäftsworkflows erstellt haben, gehören die [Zeiss Gruppe ](https://customers.microsoft.com/story/zeiss-group-focuses-on-azure-service-fabric-for-key-integration-platform) und [Digamore](https://customers.microsoft.com/story/digamore-entertainment-scores-with-a-new-gaming-platform-based-on-azure-service-fabric/).

* **Berechnung für Daten**: Mit Service Fabric können Sie zustandsbehaftete Anwendungen mit einem hohen Rechenaufwand erstellen. Service Fabric ermöglicht die Zusammenlegung von Verarbeitung (Berechnung) und Daten in Anwendungen. Wenn Ihre Anwendung auf Daten zugreifen muss, begrenzt die Netzwerklatenz, die auf die Verwendung eines externen Datencache oder einer externen Speicherebene zurückzuführen ist, in der Regel die Rechenzeit. Mit zustandsbehafteten Service Fabric-Diensten lässt sich diese Latenz beseitigen, was eine Optimierung bei Lese- und Schreibvorgängen ermöglicht. Beispiel: Angenommen, Ihre Anwendung unterbreitet Kunden nahezu in Echtzeit Vorschläge. Bei den Anforderungen muss eine Roundtrip-Zeit von unter 100 Millisekunden erreicht werden. Im Gegensatz zum standardmäßigen Implementierungsmodell, bei dem die benötigten Daten aus einem Remotespeicher abgerufen werden müssen, bieten die Latenz- und Leistungsmerkmale von Service Fabric-Diensten (bei denen die Berechnung der Vorschlagsauswahl mit den Daten und Regeln zusammengelegt wird) den Benutzern eine schnell reagierende Umgebung.

    Zu den Kunden, die Berechnungsdienste erstellt haben, gehören [Solidsoft Reply](https://customers.microsoft.com/story/solidsoft-reply-platform-powers-e-verification-of-pharmaceuticals) und [Infosupport](https://customers.microsoft.com/story/service-fabric-customer-profile-info-support-and-fudura).

* **Hoch verfügbare Dienste:** Durch das Erstellen mehrerer sekundärer Dienstreplikate bietet Service Fabric ein schnelles Failover. Wenn ein Knoten, Prozess oder individueller Dienst aufgrund eines hardwarebedingten oder anderen Fehlers ausfällt, wird eines der sekundären Replikate zu einem primären Replikat weitergeleitet – mit minimaler Dienstunterbrechung.

* **Skalierbare Dienste:** Einzelne Dienste können partitioniert werden, sodass der Zustand im Cluster horizontal hochskaliert werden kann. Darüber hinaus können einzelne Dienste im Handumdrehen erstellt und entfernt werden. Dienste können je nach Ressourcenanforderungen schnell und einfach horizontal von einigen wenigen Instanzen auf einigen wenigen Knoten auf Tausende Instanzen auf vielen Knoten hochskaliert und wieder herunterskaliert werden. Mithilfe von Service Fabric können Sie diese Dienste erstellen und ihren gesamten Lebenszyklus verwalten.

## <a name="application-design-case-studies"></a>Fallstudien zum Anwendungsentwurf
Eine Reihe von Fallstudien, die zeigen, wie Service Fabric zum Entwerfen von Anwendungen verwendet wird, sind auf [Erfahrungsberichte](https://customers.microsoft.com/search?sq=%22Azure%20Service%20Fabric%22&ff=&p=0&so=story_publish_date%20desc/) und der [Website zur Microservices-Lösungen](https://azure.microsoft.com/solutions/microservice-applications/) veröffentlicht.

## <a name="design-applications-composed-of-stateless-and-stateful-microservices"></a>Entwerfen von Anwendungen, die aus zustandslosen und zustandsbehafteten Microservices bestehen
Die Erstellung von Anwendungen mit Azure-Clouddienst-Workerrollen ist ein Beispiel für zustandslose Dienste. Im Gegensatz dazu behalten zustandsbehaftete Microservices den autorisierenden Zustand über die Anforderung und ihre Antwort hinaus bei. Durch diese Funktion lässt sich über einfache APIs, die für replikationsgestützte Transaktionsgarantien sorgen, Hochverfügbarkeit und Konsistenz erreichen. Zustandsbehaftete Service Fabric-Dienste bieten Hochverfügbarkeit für alle Arten von Anwendungen – nicht nur für Datenbanken und andere Datenspeicher. Hierbei handelt es sich um einen natürlichen Prozess: Anwendungen haben bereits den Wandel von der Verwendung rein relationaler Datenbanken für Hochverfügbarkeit zur Verwendung von NoSQL-Datenbanken vollzogen. Nun werden die Anwendung selbst hochverfügbar, und Daten werden zur Leistungsoptimierung innerhalb der Anwendung verwaltet, ohne dafür Kompromisse bei Zuverlässigkeit, Konsistenz und Verfügbarkeit einzugehen.

Bei der Erstellung von Anwendungen, die sich aus Microservices zusammensetzen, haben Sie es normalerweise mit einer Kombination aus zustandslosen Web-Apps (ASP.NET, Node.js usw.) zu tun, die zustandslose und zustandsbehaftete Geschäftsdienste der mittleren Ebene aufrufen, welche alle im gleichen Service Fabric-Cluster bereitgestellt werden und die Service Fabric-Bereitstellungsbefehle verwenden. Jeder dieser Dienste ist in puncto Skalierung, Zuverlässigkeit und Ressourcenverwendung unabhängig, was eine deutlich höhere Agilität und Flexibilität bei der Entwicklung sowie bei der Verwaltung des Lebenszyklus bedeutet.

Zustandsbehaftete Microservices vereinfachen Anwendungsentwürfe, da durch sie die Notwendigkeit zusätzlicher Warteschlangen und Caches entfällt, die traditionell erforderlich waren, um die Verfügbarkeits- und Latenzanforderungen von rein zustandslosen Anwendungen zu erfüllen. Da zustandsbehaftete Dienste normalerweise bereits hochverfügbar sind und eine niedrige Latenz bieten, fällt der Verwaltungsaufwand in Ihrer Anwendung insgesamt geringer aus. In der folgenden Abbildung werden die Unterschiede zwischen einer zustandslosen und einer zustandsbehafteten Anwendung veranschaulicht. Zustandsbehaftete Dienste verringern durch Nutzung der Programmiermodelle [Reliable Services](service-fabric-reliable-services-introduction.md) und [Reliable Actors](service-fabric-reliable-actors-introduction.md) die Komplexität der Anwendung. Gleichzeitig wird eine hohe Durchsatzrate mit geringer Latenz erzielt.

## <a name="an-application-built-using-stateless-services"></a>Eine mit zustandslosen Diensten erstellte Anwendung
![Anwendung mit zustandslosen Diensten][Image1]

## <a name="an-application-built-using-stateful-services"></a>Eine mit zustandsbehafteten Diensten erstellte Anwendung
![Anwendung mit zustandslosen Diensten][Image2]

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte

* Lesen Sie mehr über [Kundenfallstudien](https://customers.microsoft.com/search?sq=%22Azure%20Service%20Fabric%22&ff=&p=0&so=story_publish_date%20desc)
* Erfahren Sie mehr über [Muster und Szenarios](service-fabric-patterns-and-scenarios.md)

* Beginnen Sie mit der Erstellung zustandsloser und zustandsbehafteter Dienste mit den Service Fabric-Programmiermodellen [Reliable Services](service-fabric-reliable-services-quick-start.md) und [Reliable Actors](service-fabric-reliable-actors-get-started.md).
* Besuchen Sie das Azure Architecture Center. Dort finden Sie einen Leitfaden zum [Erstellen von Microservices in Azure](https://docs.microsoft.com/azure/architecture/microservices/).
* Einen Entwurfsleitfaden für Anwendungen finden Sie unter [Bewährte Methoden für Azure Service Fabric-Anwendungen und -Cluster](service-fabric-best-practices-overview.md).

* Weitere Informationen finden Sie in den folgenden Themen:
  * [Mehr über Microservices erfahren](service-fabric-overview-microservices.md)
  * [Definieren und Verwalten des Dienstzustands](service-fabric-concepts-state.md)
  * [Verfügbarkeit der Service Fabric-Dienste](service-fabric-availability-services.md)
  * [Skalieren von Service Fabric-Anwendungen](service-fabric-concepts-scalability.md)
  * [Partitionieren von Service Fabric-Diensten](service-fabric-concepts-partitioning.md)

[Image1]: media/service-fabric-application-scenarios/AppwithStatelessServices.jpg
[Image2]: media/service-fabric-application-scenarios/AppwithStatefulServices.jpg
