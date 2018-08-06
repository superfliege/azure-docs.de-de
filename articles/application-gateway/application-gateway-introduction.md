---
title: Was ist Azure Application Gateway?
description: Erfahren Sie, wie Sie Azure Application Gateway verwenden können, um den Webdatenverkehr für Ihre Anwendung zu verwalten.
services: application-gateway
author: vhorne
manager: jpconnock
ms.service: application-gateway
ms.topic: overview
ms.custom: mvc
ms.workload: infrastructure-services
ms.date: 4/9/2018
ms.author: victorh
ms.openlocfilehash: aa31bb599f665e8d3a407531d399701a5be35e41
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2018
ms.locfileid: "39364116"
---
# <a name="what-is-azure-application-gateway"></a>Was ist Azure Application Gateway?

Azure Application Gateway ist ein Lastenausgleich für Webdatenverkehr, mit dem Sie eingehenden Datenverkehr für Ihre Webanwendungen verwalten können. 

Herkömmliche Lastenausgleichsmodule sind auf der Transportschicht (OSI-Schicht 4 – TCP und UDP) aktiv und leiten Datenverkehr basierend auf der IP-Quelladresse und dem dazugehörigen Port an eine IP-Zieladresse und an den entsprechenden Port weiter. Application Gateway bietet eine höhere Präzision. So können Sie beispielsweise Datenverkehr basierend auf der eingehenden URL weiterleiten. Falls die eingehende URL also `/images` enthält, können Sie Datenverkehr an eine bestimmte Gruppe von Servern (einen so genannten Pool) weiterleiten, die für Bilder konfiguriert sind. Falls die URL `/video` enthält, wird der Datenverkehr an einen anderen Pool weitergeleitet, der für Videos optimiert ist.

![imageURLroute](./media/application-gateway-url-route-overview/figure1-720.png)

Diese Art des Routings wird als Lastenausgleich auf Anwendungsebene (OSI-Schicht 7) bezeichnet. Per Azure Application Gateway kann das URL-basierte Routing und noch mehr durchgeführt werden. Im Anschluss sind die verfügbaren Features von Azure Application Gateway aufgeführt: 

## <a name="url-based-routing"></a>URL-basiertes Routing

Mit dem Routing auf URL-Pfadbasis kann Datenverkehr basierend auf URL-Pfaden von Anforderungen an Back-End-Serverpools weitergeleitet werden. Ein mögliches Szenario ist die Weiterleitung von Anforderungen für unterschiedliche Inhaltstypen an verschiedene Pools.

Beispielsweise werden Anforderungen von `http://contoso.com/video/*` an VideoServerPool und `http://contoso.com/images/*` an ImageServerPool weitergeleitet. DefaultServerPool wird ausgewählt, wenn keines der Pfadmuster zutrifft.

## <a name="redirection"></a>Umleitung

Ein typisches Szenario vieler Webanwendungen ist die Unterstützung der automatischen Umleitung von HTTP zu HTTPS, um sicherzustellen, dass die gesamte Kommunikation zwischen einer Anwendung und ihren Benutzern über einen verschlüsselten Pfad stattfindet. 

In der Vergangenheit haben Sie unter Umständen auch Verfahren wie die Erstellung eines dedizierten Pools verwendet, der den alleinigen Zweck hatte, eingehende HTTP-Anforderungen zu HTTPS umzuleiten. Application Gateway unterstützt die Umleitung von Datenverkehr. Dies vereinfacht die Anwendungskonfiguration, optimiert die Ressourcennutzung und ermöglicht neue Umleitungsszenarien wie etwa die globale und pfadbasierte Umleitung. Die Application Gateway-Umleitung ist nicht auf HTTP zu HTTPS beschränkt. Vielmehr handelt es sich um einen generischen Umleitungsmechanismus, sodass Sie Umleitungen für jeden Port durchführen können, den Sie mithilfe von Regeln definieren. Auch die Umleitung an eine externe Website wird unterstützt.

Die Application Gateway-Umleitung bietet Folgendes:

- Globale Umleitung von einem Port zu einem anderen Port über das Anwendungsgateway. Dies ermöglicht HTTP-zu-HTTPS-Umleitungen auf einer Website.
- Pfadbasierte Umleitung. Bei dieser Art von Umleitung kann HTTP nur in einem bestimmten Websitebereich zu HTTPS umgeleitet werden – etwa in einem durch `/cart/*` gekennzeichneten Einkaufswagenbereich.
- Umleitung an eine externe Website.

## <a name="multiple-site-hosting"></a>Hosten mehrerer Websites

Das Hosten mehrerer Websites ermöglicht es Ihnen, mehr als eine Website auf derselben Application Gateway-Instanz zu konfigurieren. Mit diesem Feature können Sie eine effizientere Topologie für Ihre Bereitstellungen konfigurieren, indem Sie einem Anwendungsgateway bis zu 20 Websites hinzufügen. Jede Website kann zu einem eigenen Pool umgeleitet werden. Application Gateway kann Datenverkehr beispielsweise für `contoso.com` und `fabrikam.com` über die beiden Serverpools ContosoServerPool und FabrikamServerPool bereitstellen.

Anforderungen für `http://contoso.com` werden an „ContosoServerPool“ und Anforderungen für `http://fabrikam.com` an „FabrikamServerPool“ weitergeleitet.

Analog dazu können zwei Unterdomänen der gleichen übergeordneten Domäne in der gleichen Application Gateway-Bereitstellung gehostet werden. Beispiele für die Verwendung von untergeordneten Domänen sind u.a. `http://blog.contoso.com` und `http://app.contoso.com`, die in einer einzelnen Application Gateway-Bereitstellung gehostet werden.

## <a name="session-affinity"></a>Sitzungsaffinität

Das Feature „Cookiebasierte Sitzungsaffinität“ ist hilfreich, wenn eine Benutzersitzung auf dem gleichen Server bleiben soll. Mithilfe von durch das Gateway verwalteten Cookies kann das Anwendungsgateway weiteren Datenverkehr einer Benutzersitzung zur Verarbeitung an den gleichen Server weiterleiten. Dies ist hilfreich, wenn der Sitzungsstatus für eine Benutzersitzung lokal auf dem Server gespeichert wird.

## <a name="secure-sockets-layer-ssl-termination"></a>SSL-Beendigung (Secure Sockets Layer)

Application Gateway unterstützt die SSL-Beendigung am Gateway, wonach der Datenverkehr in der Regel unverschlüsselt zu den Back-End-Servern gelangt. Mit diesem Feature können Webserver vom kostspieligen Verschlüsselungs- und Entschlüsselungsaufwand befreit werden. Manchmal ist die unverschlüsselte Kommunikation mit den Servern allerdings keine akzeptable Option. Dies kann beispielsweise der Fall sein, wenn Sicherheits- oder Complianceanforderungen erfüllt werden müssen oder die Anwendung nur eine sichere Verbindung akzeptiert. Für Anwendungen dieser Art unterstützt Application Gateway die End-to-End-SSL-Verschlüsselung.

## <a name="web-application-firewall"></a>Web Application Firewall

Web Application Firewall (WAF) ist ein Feature von Application Gateway, das zentralisierten Schutz Ihrer Webanwendungen vor allgemeinen Exploits und Sicherheitsrisiken bietet. Die WAF basiert auf den Regeln der [OWASP-Kernregelsätze (Open Web Application Security Project)](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) der Version 3.0 oder 2.2.9. 

Webanwendungen sind zunehmend Ziele böswilliger Angriffe, die bekannte Sicherheitslücken ausnutzen. Zu diesen Exploits gehören üblicherweise Angriffe durch Einschleusung von SQL-Befehlen oder Angriffe durch websiteübergreifende Skripts (XSS). Die Verhinderung solcher Angriffe im Anwendungscode ist oft schwierig und erfordert strenge Wartung, Patching und Überwachung auf vielen Ebenen der Anwendungstopologie. Eine zentrale Web Application Firewall vereinfacht die Sicherheitsverwaltung erheblich und bietet Anwendungsadministratoren einen besseren Schutz vor Bedrohungen und Angriffen. Mit einer WAF-Lösung können Sie ebenfalls schneller auf ein Sicherheitsrisiko reagieren, da eine bekannte Schwachstelle an einem zentralen Ort gepatcht wird, statt jede Bereitstellung einzelner Webanwendungen separat zu sichern. Vorhandene Anwendungsgateways lassen sich problemlos in ein Anwendungsgateway konvertieren, für das WAF aktiviert ist.

## <a name="websocket-and-http2-traffic"></a>WebSocket- und HTTP/2-Datenverkehr

Application Gateway verfügt über native Unterstützung für das WebSocket- und das HTTP/2-Protokoll. Die WebSocket-Unterstützung kann von Benutzern nicht selektiv aktiviert oder deaktiviert werden. Die HTTP/2-Unterstützung kann mithilfe von Azure PowerShell aktiviert werden.
 
Das WebSocket- und das HTTP/2-Protokoll ermöglichen die Vollduplexkommunikation zwischen einem Server und einem Client über eine TCP-Verbindung mit langer Laufzeit. Dies ermöglicht wiederum mehr Interaktivität bei der Kommunikation zwischen dem Webserver und dem Client, da die Kommunikation auch ohne die bei HTTP-basierten Implementierungen erforderlichen Abfragen bidirektional sein kann. Diese Protokolle zeichnen sich im Vergleich zu HTTP durch einen geringen Mehraufwand aus. Außerdem können sie die gleiche TCP-Verbindung für mehrere Anforderungen/Antworten verwenden, was eine effizientere Ressourcennutzung zur Folge hat. Diese Protokolle sind für die Nutzung der üblichen HTTP-Ports 80 und 443 konzipiert.



## <a name="next-steps"></a>Nächste Schritte

Je nach Ihren Anforderungen und der vorhandenen Umgebung können Sie eine Application Gateway-Testinstanz über das Azure-Portal, mithilfe von Azure PowerShell oder unter Verwendung der Azure CLI erstellen:

- [Quickstart: Direct web traffic with Azure Application Gateway – Azure portal](quick-create-portal.md) (Schnellstart: Weiterleiten von Webdatenverkehr per Azure Application Gateway – Azure-Portal)
- [Quickstart: Direct web traffic with Azure Application Gateway – Azure PowerShell](quick-create-powershell.md) (Schnellstart: Weiterleiten von Webdatenverkehr per Azure Application Gateway – Azure PowerShell)
- [Schnellstart: Weiterleiten von Webdatenverkehr per Azure Application Gateway – Azure CLI](quick-create-cli.md)
