---
title: Allgemeine Parameter und Header
description: Die Parameter und Header, die für alle Vorgänge gelten, die Sie ggf. im Zusammenhang mit Key Vault-Ressourcen ausführen
services: key-vault
documentationcenter: ''
author: lleonard-msft
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: a715d13ca9-d6e8-4e54-ac5e-0ed9400fb15b15d13ca9-d6e8-4e54-ac5e-0ed9400fb15b
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: alleonar
ms.openlocfilehash: ead1ac550c9b7c489edefd35d5672a9955e78255
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
ms.locfileid: "34011871"
---
# <a name="common-parameters-and-headers"></a>Allgemeine Parameter und Header

Die folgenden Informationen gelten für alle Vorgänge, die Sie ggf. im Zusammenhang mit Key Vault-Ressourcen ausführen:

- Ersetzen Sie `{api-version}` durch die API-Version im URI.
- Ersetzen Sie `{subscription-id}` durch Ihren Abonnementbezeichner im URI.
- Ersetzen Sie `{resource-group-name}` durch die Ressourcengruppe. Weitere Informationen finden Sie unter „Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen“.
- Ersetzen Sie `{vault-name}` durch Ihren Schlüsseltresornamen im URI.
- Legen Sie den Inhaltstyp auf „application/json“ fest.
- Legen Sie den Autorisierungsheader auf ein JSON-Webtoken fest, das Sie aus Azure Active Directory (AAD) abgerufen haben. Weitere Informationen finden Sie unter [Authentifizieren von Anforderungen des Azure-Ressourcen-Managers](authentication-requests-and-responses.md).

## <a name="common-error-response"></a>Allgemeine Fehlerantwort
Der Dienst gibt anhand von HTTP-Statuscodes an, ob der Vorgang erfolgreich oder fehlerhaft war. Außerdem enthalten Fehler eine Antwort im folgenden Format:

   {  
     "error": {  
     "code": "BadRequest",  
     "message": "Die Schlüsseltresor-SKU ist ungültig."  
     }  
   }  

|Elementname | Typ | BESCHREIBUNG |
|---|---|---|
| code | Zeichenfolge | Der Typ des aufgetretenen Fehlers.|
| Message: | Zeichenfolge | Eine Beschreibung der Ursache des Fehlers. |



## <a name="see-also"></a>Siehe auch
 [Referenz für die Azure Key Vault-REST-API](/rest/api/keyvault/)
