---
title: 'Problembehandlung für Azure Application Gateway mit App Service: Umleitung zur App Service-URL'
description: Dieser Artikel enthält Informationen dazu, wie sich das Umleitungsproblem beheben lässt, wenn Azure Application Gateway mit Azure App Service verwendet wird.
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 02/22/2019
ms.author: absha
ms.openlocfilehash: 359d75f10f95b0e41ccd9a869d49247355f0d5d0
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58123180"
---
# <a name="troubleshoot-application-gateway-with-app-service--redirection-to-app-services-url"></a>Problembehandlung für Application Gateway mit App Service: Umleitung zur App Service-URL

 Erfahren Sie, wie Sie Umleitungsprobleme mit Application Gateway in Szenarios diagnostizieren und beheben, in denen die App Service-URL verfügbar gemacht wird.

## <a name="overview"></a>Übersicht

Wenn Sie eine öffentliche App Service-Instanz im Back-End-Pool von Application Gateway konfigurieren und im Anwendungscode eine Umleitung eingerichtet ist, werden Sie beim Zugriff auf Application Gateway möglicherweise vom Browser direkt zur App Service-URL umgeleitet.

Dieses Problem kann folgende Hauptursachen haben:

- Sie haben für App Service eine Umleitung konfiguriert. Diese ist im einfachsten Fall auf einen nachgestellten Schrägstrich zurückzuführen, der der Anforderung hinzugefügt wurde.
- Die Umleitung wird durch die Nutzung der Azure AD-Authentifizierung ausgelöst.
- Sie haben in den HTTP-Einstellungen von Application Gateway die Option „Hostnamen aus Back-End-Adresse auswählen“ aktiviert.
- Sie haben Ihre benutzerdefinierte Domäne nicht bei App Service registriert.

## <a name="sample-configuration"></a>Beispielkonfiguration

- HTTP-Listener: Basislistener oder Listener für mehrere Standorte
- Back-End-Adresspool: App Service
- HTTP-Einstellungen: „Hostnamen aus Back-End-Adresse auswählen“ aktiviert
- Test: „Pick Hostname from Backend HTTP Settings“ (Hostnamen aus Back-End-HTTP-Einstellungen auswählen)

## <a name="cause"></a>Ursache

Auf eine App Service-Instanz kann nur über die konfigurierten Hostnamen zugegriffen werden, die sich in den Einstellungen der benutzerdefinierten Domäne befinden. Standardmäßig lautet der Name „example.azurewebsites.net“. Wenn Sie mithilfe von Application Gateway und einem Hostnamen, der nicht in App Service registriert ist, oder mit dem FQDN von Application Gateway auf Ihre App Service-Instanz zugreifen möchten, müssen Sie den Hostnamen in der ursprünglichen Anforderung an den Hostnamen von App Service überschreiben.

Hierzu verwenden Sie für Application Gateway die Option „Hostnamen aus Back-End-Adresse auswählen“ in den HTTP-Einstellungen. Für die Testkonfiguration nutzen Sie „Pick Hostname from Backend HTTP Settings“ (Hostnamen aus Back-End-HTTP-Einstellungen auswählen).

![App Service (1)](./media/troubleshoot-app-service-redirection-app-service-url/appservice-1.png)

Wenn die App Service-Instanz nun eine Umleitung durchführt, wird anstelle des ursprünglichen Hostnamens „example.azurewebsites.net“ im Adressheader verwendet, falls keine andere Konfiguration vorgenommen wurde. Im Folgenden sehen Sie ein Beispiel für Anforderungs- und Antwortheader.
```
## Request headers to Application Gateway:

Request URL: http://www.contoso.com/path

Request Method: GET

Host: www.contoso.com

## Response headers:

Status Code: 301 Moved Permanently

Location: http://example.azurewebsites.net/path/

Server: Microsoft-IIS/10.0

Set-Cookie: ARRAffinity=b5b1b14066f35b3e4533a1974cacfbbd969bf1960b6518aa2c2e2619700e4010;Path=/;HttpOnly;Domain=example.azurewebsites.net

X-Powered-By: ASP.NET
```
Im obigen Beispiel verfügt der Antwortheader über den Statuscode 301 für die Umleitung. Außerdem entspricht der Adressheader nicht dem ursprünglichen Hostnamen „www.contoso.com“, sondern dem von App Service.

## <a name="solution"></a>Lösung

Sie können dieses Problem beheben, indem Sie auf eine Umleitung auf Anwendungsseite verzichten. Wenn dies nicht möglich ist, müssen Sie App Service denselben Hostheader übergeben, der von Application Gateway empfangen wird, anstatt den Hostnamen zu überschreiben.

Dadurch führt App Service Umleitungen nicht für den eigenen Header, sondern für denselben ursprünglichen Hostheader durch, der auf Application Gateway verweist.

Hierzu müssen Sie über eine benutzerdefinierte Domäne verfügen und die im Folgenden genannten Schritte ausführen:

- Tragen Sie die Domäne in die Liste der benutzerdefinierten Domänen von App Service ein. Dazu benötigen Sie in Ihrer benutzerdefinierten Domäne einen CNAME-Eintrag, der auf den FQDN von App Service verweist. Weitere Informationen finden Sie unter [Zuordnen eines vorhandenen benutzerdefinierten DNS-Namens zu Azure App Service](https://docs.microsoft.com//azure/app-service/app-service-web-tutorial-custom-domain).

![App Service (2)](./media/troubleshoot-app-service-redirection-app-service-url/appservice-2.png)

- Anschließend kann Ihrer App Service-Instanz der Hostname „www.contoso.com“ übergeben werden. Ändern Sie nun den CNAME-Eintrag im DNS so, dass er wieder auf den FQDN von Application Gateway verweist. Beispiel: „appgw.eastus.cloudapp.azure.com“.

- Stellen Sie sicher, dass die Domäne „www.contoso.com“ in den FQDN von Application Gateway aufgelöst wird, wenn Sie eine DNS-Abfrage ausführen.

- Deaktivieren Sie für Ihren benutzerdefinierten Test „Pick Hostname from Backend HTTP Settings“ (Hostnamen aus Back-End-HTTP-Einstellungen auswählen). Dazu können Sie im Azure-Portal in den Testeinstellungen das entsprechende Kontrollkästchen deaktivieren oder in PowerShell bei Nutzung des Befehls „Set-AzApplicationGatewayProbeConfig“ auf die Option „-PickHostNameFromBackendHttpSettings“ verzichten. Geben Sie im Feld für den Hostnamen des Tests den FQDN „example.azurewebsites.net“ von App Service ein, da die von Application Gateway gesendeten Testanforderungen diesen Namen im Hostheader enthalten.

  > [!NOTE]
  > Stellen Sie vor dem nächsten Schritt sicher, dass der benutzerdefinierte Test nicht Ihren Back-End-HTTP-Einstellungen zugeordnet ist. Für Ihre HTTP-Einstellungen ist zu diesem Zeitpunkt nach wie vor die Option „Hostnamen aus Back-End-Adresse auswählen“ aktiviert.

- Passen Sie die HTTP-Einstellungen von Application Gateway so an, dass „Hostnamen aus Back-End-Adresse auswählen“ deaktiviert wird. Dazu können Sie im Azure-Portal das entsprechende Kontrollkästchen deaktivieren oder in PowerShell bei Nutzung des Befehls „Set-AzApplicationGatewayBackendHttpSettings“ auf die Option „-PickHostNameFromBackendAddress“ verzichten.

- Ordnen Sie den benutzerdefinierten Test wieder den Back-End-HTTP-Einstellungen zu, und überprüfen Sie die Back-End-Integrität.

- Der Hostname „www.contoso.com“ sollte nun von Application Gateway an die App Service-Instanz weitergeleitet und außerdem für die Umleitung verwendet werden. Im Folgenden sehen Sie ein Beispiel für Anforderungs- und Antwortheader.
  ```
  ## Request headers to Application Gateway:

  Request URL: http://www.contoso.com/path

  Request Method: GET

  Host: www.contoso.com

  ## Response headers:

  Status Code: 301 Moved Permanently

  Location: http://www.contoso.com/path/

  Server: Microsoft-IIS/10.0

  Set-Cookie: ARRAffinity=b5b1b14066f35b3e4533a1974cacfbbd969bf1960b6518aa2c2e2619700e4010;Path=/;HttpOnly;Domain=www.contoso.com

  X-Powered-By: ASP.NET
  ```
  ## <a name="next-steps"></a>Nächste Schritte

Sollte sich das Problem mit den oben genannten Schritten nicht beheben lassen, erstellen Sie ein [Supportticket](https://azure.microsoft.com/support/options/).
