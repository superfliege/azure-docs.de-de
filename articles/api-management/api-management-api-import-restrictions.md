---
title: Einschränkungen und bekannte Probleme beim Import von APIs in Azure API Management | Microsoft-Dokumentation
description: Hier erhalten Sie Informationen zu bekannten Problemen und Einschränkungen beim Import in Azure API Management bei Verwendung der Formate Open API, WSDL oder WADL.
services: api-management
documentationcenter: ''
author: vladvino
manager: vlvinogr
editor: ''
ms.assetid: 7a5a63f0-3e72-49d3-a28c-1bb23ab495e2
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2017
ms.author: apipm
ms.openlocfilehash: bad87931feb11012f23f0ef19bd853b38566c07c
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2019
ms.locfileid: "54106823"
---
# <a name="api-import-restrictions-and-known-issues"></a>Einschränkungen und bekannte Probleme beim Import von APIs
## <a name="about-this-list"></a>Informationen zu dieser Liste
Beim Importieren einer API stoßen Sie unter Umständen auf Einschränkungen oder Probleme, die behoben werden müssen, damit der Import erfolgreich ausgeführt werden kann. Diese Probleme werden nachfolgend unter dem jeweiligen Importformat der API beschrieben.

## <a name="open-api"> </a>OpenAPI/Swagger
Wenn Ihnen beim Importieren Ihres OpenAPI-Dokuments Fehler gemeldet werden, vergewissern Sie sich, dass Sie das Dokument überprüft haben. Diese Überprüfung können Sie mit dem Designer des neuen Azure-Portals (Design – Front-End – OpenAPI Specification Editor) oder mit einem Tool eines Drittanbieters, zum Beispiel mit <a href="https://editor.swagger.io">Swagger Editor</a>, vornehmen.

* Nur das JSON-Format für OpenAPI wird unterstützt.
* Erforderliche Parameter für Pfad und Abfrage müssen eindeutige Namen besitzen. (In OpenAPI muss ein Parametername nur an einem Ort (beispielsweise im Pfad, in der Abfrage oder im Header) eindeutig sein.  In API Management können Vorgänge jedoch sowohl nach Pfad- als auch nach Abfrageparametern unterschieden werden. (Dies wird von OpenAPI nicht unterstützt.) Daher müssen Parameternamen innerhalb der gesamten URL-Vorlage eindeutig sein.)
* Schemas, auf die mit **$ref**-Eigenschaften verwiesen wird, können keine anderen **$ref**-Eigenschaften enthalten.
* Mit **$ref**-Zeigern kann nicht auf externe Dateien verwiesen werden.
* **x-ms-paths** und **x-servers** sind die einzigen unterstützten Erweiterungen.
* Benutzerdefinierte Erweiterungen werden beim Import ignoriert und nicht für den Export gespeichert oder aufbewahrt.
* **Rekursion**: Definitionen, die rekursiv definiert sind (beispielsweise auf sich selbst verweisen), werden von APIM nicht unterstützt.

> [!IMPORTANT]
> Dieses [Dokument](https://blogs.msdn.microsoft.com/apimanagement/2018/04/11/important-changes-to-openapi-import-and-export/) enthält wichtige Informationen und Tipps zum OpenAPI-Import.

## <a name="wsdl"></a>WSDL
WSDL-Dateien werden zur Generierung von SOAP-Pass-Through-APIs verwendet oder dienen als Back-End einer SOAP-to-REST-API.
* **SOAP-Bindungen**: Es werden nur SOAP-Bindungen unterstützt, die im Format von „document“- oder „literal“-Codierung vorliegen. Das „rpc“-Format und SOAP-Codierung werden nicht unterstützt.
* **WSDL:Import**: Dieses Attribut wird nicht unterstützt. Kunden sollten die zu importierenden Elemente in einem Dokument zusammenführen.
* **Mehrteilige Nachrichten**: Diese Art von Nachrichten wird nicht ünterstützt.
* **WCF wsHttpBinding**: Für SOAP-Dienste, die mit Windows Communication Foundation erstellt wurden, sollte basicHttpBinding verwendet werden – wsHttpBinding wird nicht unterstützt.
* **MTOM**: Dienste, die MTOM verwenden, funktionieren <em>möglicherweise</em>. Eine offizielle Unterstützung wird derzeit nicht geboten.
* **Rekursion**: Typen, die rekursiv definiert sind (beispielsweise auf ein eigenes Array verweisen), werden von APIM nicht unterstützt.
* **Mehrere Namespaces**: In einem Schema können zwar mehrere Namespaces verwendet werden, aber nur der Zielnamespace kann zum Definieren von Nachrichtenteilen verwendet werden. Namespaces, die zum Definieren anderer Ein- oder Ausgabeelememente verwendet werden und bei denen es sich nicht um den Zielnamespace handelt, werden nicht beibehalten. Ein solches WSDL-Dokument kann zwar importiert werden, beim Exportieren haben jedoch alle Nachrichtenteile den WSDL-Zielnamespace.

## <a name="wadl"></a>WADL
Derzeit sind keine Probleme beim Import im Format WADL bekannt.
