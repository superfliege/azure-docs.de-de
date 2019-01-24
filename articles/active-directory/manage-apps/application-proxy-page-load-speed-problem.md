---
title: Zu lange Ladezeiten bei einer Anwendungsproxyanwendung | Microsoft-Dokumentation
description: Behandlung von Leistungsproblemen beim Laden von Seiten mit dem Azure AD-Anwendungsproxy
services: active-directory
documentationcenter: ''
author: barbkess
manager: daveba
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: 247b980a58b4a66a88afba91c5acb215e645eecc
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54470395"
---
# <a name="an-application-proxy-application-takes-too-long-to-load"></a>Zu lange Ladezeiten bei einer Anwendungsproxyanwendung

Dieser Artikel enthält Informationen zu möglicherweise auftretenden Leistungsproblemen beim Laden einer Azure AD-Anwendungsproxyanwendung. Zudem wird erläutert, welche Schritte Sie zur Behebung dieses Problems unternehmen können.

## <a name="overview"></a>Übersicht
Ihre Anwendungen funktionieren zwar möglicherweise, es können jedoch lange Latenzen auftreten. Eventuell können Sie die Geschwindigkeit durch Optimierung der Netzwerktopologie erhöhen. Eine Bewertung der unterschiedlichen Topologien finden Sie unter [Aspekte der Netzwerktopologie bei Verwendung des Azure Active Directory-Anwendungsproxys](application-proxy-network-topology.md).

Neben der Netzwerktopologie gibt es derzeit keine weiteren Empfehlungen zur Leistungsoptimierung. Mit zunehmender Erweiterung des Anwendungsproxydiensts kommt ein Rechenzentrum, das physisch näher gelegen ist, möglicherweise eher infrage. Denn die Nähe kann im Hinblick auf Latenzen Abhilfe leisten. Eine Liste der Azure-Rechenzentren finden Sie auf der [Latenztestseite](http://www.azurespeed.com/Azure/Latency). 

Rechenzentren mit Webanwendungsproxy-Dienst können Sie mit dem [Connectorports-Testtool](https://aadap-portcheck.connectorporttest.msappproxy.net/) suchen. 

## <a name="feedback-on-application-proxy-data-center-locations"></a>Feedback zu Anwendungsproxy-Rechenzentrumsstandorten 
Möglicherweise gibt es Azure-Rechenzentren, die noch keinen Anwendungsproxy haben, aber zu einer starken Leistungssteigerung bei der Latenz führen können. Senden Sie den Standort des Rechenzentrums an aadapfeedback@microsoft.com. Microsoft verwendet Ihr Feedback für Erweiterungspläne.

Microsoft arbeitet an zusätzlichen Funktionen zur Verbesserung der Latenzen. Sobald diese Optimierungsfunktionen verfügbar sind, wird die Dokumentation entsprechend aktualisiert.

## <a name="next-steps"></a>Nächste Schritte
[Verwenden von vorhandenen lokalen Proxyservern](application-proxy-configure-connectors-with-proxy-servers.md)
