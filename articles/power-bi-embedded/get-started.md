---
title: Erste Schritte mit Microsoft Power BI Embedded | Microsoft-Dokumentation
description: Integrieren von Power BI Embedded in Ihre Business Intelligence-Anwendung
services: power-bi-embedded
documentationcenter: ''
author: markingmyname
manager: kfile
editor: ''
tags: ''
ms.assetid: 4787cf44-5d1c-4bc3-b3fd-bf396e5c1176
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 01/11/2018
ms.author: maghan
ms.openlocfilehash: 7a860742192227e92a14d9bb5622dc9b61f6cb99
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34716022"
---
# <a name="get-started-with-microsoft-power-bi-embedded"></a>Erste Schritte mit Microsoft Power BI Embedded

Mit **Power BI Embedded** können unabhängige Softwarehersteller (Independent Software Vendors, ISVs) und Entwickler ihren Anwendungen auf der Grundlage eines kapazitäts- und stundenbasierten Modells im Handumdrehen beeindruckende Visualisierungen, Berichte und Dashboards hinzufügen.

![Diagramm des Einbettungsablaufs](media/get-started/introduction.png)

Power BI Embedded bietet Vorteile für ISVs, deren Entwickler sowie deren Kunden. Ein ISV kann beispielsweise mit Power BI Desktop kostenlos mit der Erstellung von Visualisierungen beginnen. Dank des geringeren Entwicklungsaufwands für visuelle Analysen können ISVs die Entwicklungszeit verkürzen und sich mit einer differenzierten Datennutzung von ihren Mitbewerbern abheben. Darüber hinaus können ISVs ggf. einen Aufpreis für den zusätzlichen Nutzen verlangen, der sich durch eingebettete Analysen ergibt.

Entwickler müssen sich nicht mit der Entwicklung von Visualisierungen und Analysen beschäftigen, sondern können sich stattdessen auf die Kernkompetenzen ihrer Anwendung konzentrieren. Entwickler können sich zeitnah den Berichts- und Dashboardanforderungen der Kunden widmen, und die Einbettung wird durch dokumentierte APIs und SDKs erleichtert. Darüber hinaus können ISVs in ihren Apps Datenuntersuchungen mit benutzerfreundlicher Navigation ermöglichen, sodass ihre Kunden unabhängig vom verwendeten Gerät schnell und sicher fundierte Entscheidungen treffen können.

## <a name="register-an-application-within-azure-active-directory"></a>Registrieren einer Anwendung in Azure Active Directory

Für die Einbettung in eine benutzerdefinierte Anwendung wird eine registrierte Anwendung in Azure Active Directory (AAD) benötigt. Von der registrierten Anwendung wird vorausgesetzt, dass es sich bei Ihrem Mandanten um einen Power BI-Mandanten handelt. Bei einem Power BI-Mandanten hat sich mindestens ein Benutzer in der Organisation für Power BI registriert. Die Power BI-Registrierung eines Benutzers ist erforderlich, damit die Power BI-APIs in der registrierten Anwendung angezeigt werden.

Weitere Informationen zum Registrieren einer Anwendung in AAD finden Sie unter [Registrieren einer Azure AD-App zum Einbetten von Power BI-Inhalten](https://powerbi.microsoft.com/documentation/powerbi-developer-register-app/).

## <a name="embed-content-in-your-application"></a>Einbetten von Inhalten in Ihre Anwendung

Nach der Anwendungsregistrierung in AAD können Sie Power BI-Inhalte in Ihre Anwendung einbetten. Verwenden Sie zum Einbetten von Inhalten die REST-API zusammen mit den JavaScript-APIs.

Wir stellen Beispiele zur Verfügung, um Ihnen den Einstieg zu erleichtern. Eine exemplarische Vorgehensweise des Beispiels finden Sie unter [Integrieren eines Dashboards, einer Kachel oder eines Berichts in die Anwendung (die App ist Besitzer der Daten)](https://powerbi.microsoft.com/documentation/powerbi-developer-embed-sample-app-owns-data/).

## <a name="get-capacity-and-move-to-production"></a>Abrufen von Kapazität und Vorbereiten der Produktion

Erstellen Sie Power BI Embedded-Kapazität in Microsoft Azure, um die Produktion Ihrer Anwendung vorzubereiten. Informationen zur Kapazitätserstellung finden Sie unter [Erstellen von Power BI Embedded-Kapazität im Azure-Portal](create-capacity.md).

> [!IMPORTANT]
> Da Einbettungstoken nur für Entwicklertests vorgesehen sind, ist die Anzahl von Einbettungstoken, die für ein Power BI-Hauptkonto generiert werden können, begrenzt. Für Einbettungen in die Produktion muss [Kapazität erworben werden](https://docs.microsoft.com/power-bi/developer/embedded-faq#technical). Wenn eine dedizierte Kapazität erworben wird, ist die Generierung von Einbettungstoken nicht beschränkt. Wechseln Sie zu [Verfügbare Features abrufen](https://msdn.microsoft.com/en-us/library/mt846473.aspx), um die aktuelle eingebettete Auslastung in Prozent zu überprüfen.

Verwalten Sie die Kapazität über das Power BI-Verwaltungsportal. Weisen Sie einen Arbeitsbereichsadministrator zu, der Sie bei Ihren App-Arbeitsbereichen unterstützt. Weitere Informationen finden Sie unter [Verwalten von Kapazitäten in Power BI Premium und Power BI Embedded](https://powerbi.microsoft.com/documentation/powerbi-admin-premium-manage/).

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Erstellen von Power BI Embedded-Kapazität finden Sie unter [Erstellen von Power BI Embedded-Kapazität im Azure-Portal](create-capacity.md).

Eine exemplarische Vorgehensweise finden Sie unter [Tutorial: Einbetten von Power BI-Berichten, -Dashboards oder -Kacheln in eine Anwendung für Ihre Kunden](https://powerbi.microsoft.com/documentation/powerbi-developer-embed-sample-app-owns-data/).

Weitere Fragen? [Power BI-Community ausprobieren](http://community.powerbi.com/)
