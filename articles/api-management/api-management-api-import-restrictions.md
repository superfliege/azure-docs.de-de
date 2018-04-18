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
ms.openlocfilehash: ab4bc4024248675c6325159b5507add1274addc9
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2018
---
# <a name="api-import-restrictions-and-known-issues"></a>Einschränkungen und bekannte Probleme beim Import von APIs
## <a name="about-this-list"></a>Informationen zu dieser Liste
Beim Importieren einer API stoßen Sie unter Umständen auf Einschränkungen oder Probleme, die behoben werden müssen, damit der Import erfolgreich ausgeführt werden kann. Diese Probleme werden nachfolgend unter dem jeweiligen Importformat der API beschrieben.

## <a name="open-api"></a>Open API/Swagger
Wenn Ihnen beim Import von Open API-Dokumenten Fehler gemeldet werden, sollten Sie zunächst sicherstellen, dass Sie das Dokument überprüft haben. Diese Überprüfung können Sie mit dem Designer des neuen Azure-Portals (Design – Front-End – Open API Specification Editor) oder mit einem Tool eines Drittanbieters, zum Beispiel mit <a href="http://www.swagger.io">Swagger Editor</a>, vornehmen.

* Nur das JSON-Format für OpenAPI wird unterstützt.
* Schemas, auf die mit **$ref**-Eigenschaften verwiesen wird, können keine anderen **$ref**-Eigenschaften enthalten.
* Mit **$ref**-Zeigern kann nicht auf externe Dateien verwiesen werden.
* **x-ms-paths** und **x-servers** sind die einzigen unterstützten Erweiterungen.
* Benutzerdefinierte Erweiterungen werden beim Import ignoriert und nicht für den Export gespeichert oder aufbewahrt.

> [!IMPORTANT]
> Dieses [Dokument](https://blogs.msdn.microsoft.com/apimanagement/2018/03/28/important-changes-to-openapi-import-and-export/) enthält wichtige Informationen und Tipps zum OpenAPI-Import.

## <a name="wsdl"></a>WSDL
WSDL-Dateien werden zur Generierung von SOAP-Pass-Through-APIs verwendet oder dienen als Back-End einer SOAP-to-REST-API.

* **WSDL:Import**: APIs, die dieses Attribut verwenden, werden derzeit von APIM nicht unterstützt. Die Kunden sollten die importierten Elemente zu einem Dokument zusammenfassen.
* **Mehrteilige Nachrichten**: Derzeit unterstützt APIM diese Art von Nachrichten nicht.
* **WCF wsHttpBinding:** Für SOAP-Dienste, die mit Windows Communication Foundation erstellt wurden, sollte basicHttpBinding verwendet werden – wsHttpBinding wird nicht unterstützt.
* **MTOM**: Dienste, die MTOM verwenden, funktionieren <em>möglicherweise</em>. Eine offizielle Unterstützung wird derzeit nicht geboten.
* Rekursiv definierte (also beispielsweise auf ein eigenes Array verweisende) **Rekursionstypen** werden von APIM nicht unterstützt.

## <a name="wadl"></a>WADL
Derzeit sind keine Probleme beim Import im Format WADL bekannt.
