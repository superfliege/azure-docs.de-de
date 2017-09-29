---
title: Steuerelemente beim bedingten Zugriff mit Azure Active Directory | Microsoft-Dokumentation
description: Erfahren Sie, wie Steuerelemente beim bedingten Zugriff mit Azure Active Directory funktionieren.
services: active-directory
keywords: "bedingter Zugriff auf Apps, bedingter Zugriff mit Azure AD, sicherer Zugriff auf Unternehmensressourcen, Richtlinien für bedingten Zugriff"
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/25/2017
ms.author: markvi
ms.reviewer: calebb
ms.translationtype: HT
ms.sourcegitcommit: 44e9d992de3126bf989e69e39c343de50d592792
ms.openlocfilehash: be3631db20ae744965f9f6677c536ade45e34c49
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="controls-in-azure-active-directory-conditional-access"></a>Steuerelemente beim bedingten Zugriff mit Azure Active Directory 

Mit dem [bedingten Zugriff von Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md) können Sie den Zugriff von autorisierten Benutzern auf Ihre Cloud-Apps steuern. In einer Richtlinie für den bedingten Zugriff definieren Sie die Antwort („do this“) auf eine bestimmte Bedingung („when this happens“). Im Kontext des bedingten Zugriffs gilt: 

- „**When this happens**“ wird als **Bedingungsanweisung**  bezeichnet.

- „**Then do this**“ wird als **Kontrolle** bezeichnet.

![Kontrolle](./media/active-directory-conditional-access-controls/11.png)

Die Kombination aus Bedingungsanweisung und Ihren Kontrollen ergibt eine Richtlinie für den bedingten Zugriff.

![Kontrolle](./media/active-directory-conditional-access-controls/12.png)

Jedes Steuerelement ist entweder eine Anforderung, die von der Person oder dem System erfüllt werden muss, die bzw. das sich anmeldet, oder eine Einschränkung der Aktionen, die der Benutzer nach der Anmeldung durchführen kann. 

Es gibt zwei Arten von Steuerelementen: 

- **Gewährungssteuerelemente**: Zum Steuern des Zugriffs

- **Sitzungssteuerelemente**: Zum Einschränken des Zugriffs innerhalb einer Sitzung

In diesem Thema werden die verschiedenen Steuerelemente beschrieben, die beim bedingten Zugriff mit Azure AD verfügbar sind. 

## <a name="grant-controls"></a>Gewährungssteuerelemente

Mit Gewährungssteuerelementen können Sie entweder den gesamten Zugriff sperren oder den Zugriff mit zusätzlichen Anforderungen gewähren, indem Sie die gewünschten Steuerelemente auswählen. Bei mehreren Steuerelementen können Sie folgenden Anforderungen verwenden:

- Alle ausgewählten Steuerelemente müssen erfüllt werden (*AND*) 
- Ein ausgewähltes Steuerelement muss erfüllt werden (*OR*)

![Kontrolle](./media/active-directory-conditional-access-controls/17.png)



### <a name="multi-factor-authentication"></a>Multi-Factor Authentication

Mithilfe dieses Steuerelements können Sie die mehrstufige Authentifizierung für den Zugriff auf die angegebene Cloud-App erforderlich machen. Dieses Steuerelement unterstützt die folgenden Anbieter für die mehrstufige Authentifizierung: 

- Azure Multi-Factor Authentication 

- Ein lokaler Anbieter für die mehrstufige Authentifizierung in Kombination mit Active Directory-Verbunddiensten (AD FS).
 
Die mehrstufige Authentifizierung unterstützt den Schutz Ihrer Ressourcen vor dem Zugriff durch einen nicht autorisierten Benutzer, der möglicherweise Zugriff auf die primären Anmeldeinformationen eines gültigen Benutzers erlangt hat.



### <a name="compliant-device"></a>Kompatibles Gerät

Sie können die Richtlinien für den bedingten Zugriff auf Geräteebene festlegen. Das Ziel einer gerätebasierten Richtlinie für den bedingten Zugriff ist, den Zugriff auf die konfigurierten Ressourcen nur von vertrauenswürdigen Geräten aus zuzulassen. Eine Möglichkeit, zu definieren, was ein vertrauenswürdiges Gerät ist, besteht darin, ein kompatibles Gerät vorauszusetzen. Weitere Informationen finden Sie unter [Einrichten von Richtlinien für den gerätebasierten bedingten Zugriff für Azure Active Directory](active-directory-conditional-access-policy-connected-applications.md).

### <a name="domain-joined-device"></a>In eine Domäne eingebundenes Gerät

Ein in eine Domäne eingebundenes Gerät zu verlangen, ist eine weitere Option, die Ihnen zum Konfigurieren von Richtlinien für den gerätebasierten bedingten Zugriff zur Verfügung steht. Diese Anforderung bezieht sich auf Windows-Desktops, Laptops und Enterprise-Tablets, die mit einem lokalen Active Directory verknüpft sind. Weitere Informationen finden Sie unter [Einrichten von Richtlinien für den gerätebasierten bedingten Zugriff für Azure Active Directory](active-directory-conditional-access-policy-connected-applications.md).





### <a name="approved-client-app"></a>Genehmigte Client-App

Da Ihre Mitarbeiter mobile Geräte sowohl für private als auch für berufliche Zwecke verwenden, sollten Sie die Möglichkeit haben, Unternehmensdaten, auf die mit den Geräten zugegriffen wird, auch dann zu schützen, wenn diese Geräte nicht von Ihnen verwaltet werden.
Sie können [Intune-Richtlinien für den App-Schutz](https://docs.microsoft.com/intune/app-protection-policy) verwenden, um die Daten Ihres Unternehmens unabhängig von der jeweiligen MDM-Lösung (Mobile Device Management, mobile Geräteverwaltung) zu schützen.


Wenn Sie genehmigte Client-Apps verwenden, können Sie die Anforderung festlegen, dass eine Client-App, die auf Ihre Cloud-Apps zugreifen möchte, die [Intune-Richtlinien für den App-Schutz](https://docs.microsoft.com/intune/app-protection-policy) unterstützen muss. Sie können zum Beispiel den Zugriff auf Exchange Online für die Outlook-App einschränken. Eine Richtlinie für den bedingten Zugriff, die genehmigte Client-Apps erfordert, wird auch als [Richtlinie für den App-basierten bedingten Zugriff](active-directory-conditional-access-mam.md) bezeichnet. Eine Liste der unterstützten genehmigten Client-Apps finden Sie unter [Genehmigte Client-App als Voraussetzung](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement).


### <a name="terms-of-use"></a>Terms of Use (Nutzungsbedingungen)

Sie können einen Benutzer in Ihrem Mandanten anfordern, den Nutzungsbedingungen zuzustimmen, bevor der Zugriff auf eine Ressource gewährt wird. Als Administrator können Sie Nutzungsbedingungen durch Hochladen eines PDF-Dokuments konfigurieren und anpassen. Fällt ein Benutzer in den Anwendungsbereich dieses Steuerelements, wird der Zugriff auf eine Anwendung nur gewährt, wenn die Nutzungsbedingungen akzeptiert wurden. 


### <a name="custom-controls"></a>Benutzerdefinierte Steuerelemente 

Sie können benutzerdefinierte Steuerelemente für den bedingten Zugriff erstellen, die Ihre Benutzer zu einem kompatiblen Dienst umleiten, um weiteren Anforderungen außerhalb von Azure Active Directory zu genügen. Dadurch können Sie bestimmte externe mehrstufige Authentifizierungs- und Überprüfungsanbieter verwenden, um Regeln für den bedingten Zugriff zu erzwingen, oder zur Erstellung Ihres eigenen benutzerdefinierten Diensts. Um die Bedingungen dieses Steuerelements zu erfüllen, wird der Browser eines Benutzers auf den externen Dienst umgeleitet, führt alle erforderlichen Authentifizierungs- oder Überprüfungsaktivitäten durch und wird dann wieder an Azure Active Directory umgeleitet. Wenn der Benutzer erfolgreich authentifiziert oder überprüft wurde, bleibt der Benutzer im Vorgangsfluss des bedingten Zugriffs. 

## <a name="custom-controls"></a>Benutzerdefinierte Steuerelemente

Benutzerdefinierte Steuerelemente für den bedingten Zugriff leiten Ihre Benutzer zu kompatiblen Diensten um, um weiteren Anforderungen außerhalb von Azure Active Directory zu genügen. Um die Bedingungen dieses Steuerelements zu erfüllen, wird der Browser eines Benutzers auf den externen Dienst umgeleitet, führt alle erforderlichen Authentifizierungs- oder Überprüfungsaktivitäten durch und wird dann wieder an Azure Active Directory umgeleitet. Azure Active Directory überprüft die Antwort, und wenn der Benutzer erfolgreich authentifiziert oder überprüft wurde, verbleibt der Benutzer im Vorgangsfluss des bedingten Zugriffs.

Diese Steuerelemente ermöglichen die Verwendung von bestimmten externen oder benutzerdefinierten Diensten als Steuerelemente für den bedingten Zugriff und erweitern in der Regel die Funktionen des bedingten Zugriffs.

Zu den Anbietern, die derzeit einen kompatiblen Dienst anbieten gehören:

- Duo Security

- RSA

- Trusona

Für weitere Informationen zu diesen Diensten wenden Sie sich direkt an die Anbieter.

### <a name="creating-custom-controls"></a>Erstellen von benutzerdefinierten Steuerelementen

Um ein benutzerdefiniertes Steuerelement zu erstellen, sollten Sie sich zuerst an den Anbieter wenden, den Sie verwenden möchten. Jeder Nicht-Microsoft-Anbieter hat seine eigenen Prozesse und Anforderungen für das Registrieren, das Abonnieren, oder um auf andere Weise Teil des Diensts zu werden, und um anzugeben, dass Sie sich in den bedingten Zugriff integrieren möchten. Zu diesem Zeitpunkt wird der Anbieter einen Block von Daten im JSON-Format für Sie bereitstellen. Diese Daten ermöglichen dem Anbieter und dem bedingten Zugriff, für Ihren Mandanten zusammenzuarbeiten. Diese Daten erstellen das neue Steuerelement und definieren, wie der bedingte Zugriff feststellen kann, ob Ihre Benutzer die Überprüfung mit dem Anbieter erfolgreich ausgeführt haben.

Kopieren Sie die JSON-Daten, und fügen Sie sie in das entsprechende Textfeld ein. Verändern Sie JSON-Daten nicht, es sei denn, Sie verstehen explizit die Änderungen, die Sie vornehmen möchten. Jede Änderung kann die Verbindung zwischen dem Anbieter und Microsoft unterbrechen und möglicherweise den Zugriff auf Ihre Konten für Sie und Ihre Benutzer sperren.

Die Option zum Erstellen eines benutzerdefinierten Steuerelements befindet sich im Abschnitt **Verwalten** der Seite **Bedingter Zugriff**.

![Kontrolle](./media/active-directory-conditional-access-controls/82.png)

Wenn Sie auf **New custom control** (Neues benutzerdefiniertes Steuerelement) klicken, wird ein Blatt geöffnet und ein Textfeld für die JSON-Daten des Steuerelements angezeigt.  


![Kontrolle](./media/active-directory-conditional-access-controls/81.png)


### <a name="deleting-custom-controls"></a>Löschen von benutzerdefinierten Steuerelementen

Um ein benutzerdefiniertes Steuerelement zu löschen, müssen Sie zunächst sicherstellen, dass es in keiner der Richtlinien für bedingten Zugriff verwendet wird. Ist der Vorgang einmal abgeschlossen:

1. Wechseln Sie zur Liste der benutzerdefinierten Steuerelemente

2. Klicken Sie auf „...“  

3. Klicken Sie auf **Löschen**.

### <a name="editing-custom-controls"></a>Bearbeiten von benutzerdefinierten Steuerelementen

Um ein benutzerdefiniertes Steuerelement zu bearbeiten, müssen Sie das aktuelle Steuerelement löschen und ein neues Steuerelement mit den aktualisierten Informationen erstellen.




## <a name="session-controls"></a>Sitzungssteuerelemente

Sitzungssteuerelemente ermöglichen das Einschränken der Benutzeroberfläche innerhalb einer Cloud-App. Die Sitzungssteuerelemente werden von Cloud-Apps erzwungen und verlassen sich auf zusätzliche Informationen über die Sitzung, die der App von Azure AD bereitgestellt werden.

![Kontrolle](./media/active-directory-conditional-access-controls/31.png)

### <a name="use-app-enforced-restrictions"></a>Verwenden von App-erzwungenen Einschränkungen

Sie können dieses Steuerelement verwenden, um von Azure AD anzufordern, die Geräteinformationen an die Cloud-App zu übergeben. Dadurch erfährt die Cloud-App, ob der Benutzer von einem konformen Gerät oder einem in eine Domäne eingebundenen Gerät stammt. Dieses Steuerelement wird derzeit nur mit SharePoint als Cloud-App unterstützt. SharePoint verwendet die Geräteinformationen dazu, Benutzern eine eingeschränkte oder vollständige Benutzeroberfläche zur Verfügung zu stellen, je nach Gerätezustand.
Weitere Informationen zum Anfordern von eingeschränktem Zugriff mit SharePoint finden Sie unter [Steuern des Zugriffs von nicht verwalteten Geräten](https://aka.ms/spolimitedaccessdocs).



## <a name="next-steps"></a>Nächste Schritte

- Wenn Sie wissen möchten, wie Sie eine Richtlinie für den bedingten Zugriff konfigurieren, helfen Ihnen die Informationen unter [Erste Schritte mit dem bedingten Zugriff in Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md) weiter.

- Wenn Sie bereit sind, Richtlinien für den bedingten Zugriff für Ihre Umgebung zu konfigurieren, lesen Sie unter [Best Practices für den bedingten Zugriff in Azure Active Directory](active-directory-conditional-access-best-practices.md) nach. 

