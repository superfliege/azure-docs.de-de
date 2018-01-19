---
title: Standortbedingungen beim bedingten Zugriff in Azure Active Directory | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie die auf dem Netzwerkstandort eines Benutzers beruhende Standortbedingung zum Steuern des Zugriffs auf Ihre Cloud-App verwenden können."
services: active-directory
keywords: "bedingter Zugriff auf Apps, bedingter Zugriff mit Azure AD, sicherer Zugriff auf Unternehmensressourcen, Richtlinien für bedingten Zugriff"
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/11/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 47b4d70c991bd618ea4ea6e5d2fd1dea86911798
ms.sourcegitcommit: 828cd4b47fbd7d7d620fbb93a592559256f9d234
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2018
---
# <a name="location-conditions-in-azure-active-directory-conditional-access"></a>Standortbedingungen beim bedingten Zugriff in Azure Active Directory 

Mit dem [bedingten Zugriff von Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md) können Sie den Zugriff von autorisierten Benutzern auf Ihre Cloud-Apps steuern. Mit der Standortbedingung einer Richtlinie zum bedingten Zugriff können Sie die Einstellungen der Zugriffssteuerung an die Netzwerkstandorte Ihrer Benutzer knüpfen.

In diesem Artikel erhalten Sie die erforderlichen Informationen, um die Standortbedingung zu konfigurieren. 

## <a name="locations"></a>Standorte

Azure AD ermöglicht das einmalige Anmelden bei Geräten, Apps und Diensten von beliebigen Orten im öffentlichen Internet aus. Mithilfe der Standortbedingung können Sie den Zugriff auf Ihre Cloud-Apps auf der Grundlage des Netzwerkstandorts eines Benutzers steuern. Häufige Anwendungsfälle für die Standortbedingung sind:

- Das Vorschreiben von mehrstufiger Authentifizierung für Benutzer beim Zugriff auf einen Dienst, wenn sie sich außerhalb des Unternehmensnetzwerks befinden  

- Das Blockieren des Zugriffs auf einen Dienst für Benutzer, die sich in bestimmten Ländern oder Regionen aufhalten. 

Ein Standort ist eine Bezeichnung für einen Netzwerkspeicherort, entweder in Form eines benannten Standorts oder von vertrauenswürdigen IPs aufgrund von mehrstufiger Authentifizierung.


## <a name="named-locations"></a>Benannte Orte 

Mithilfe von benannten Standorten können Sie logische Gruppierungen von IP-Adressbereichen, Ländern und Regionen erstellen. 

 Ein benannter Standort besteht aus folgenden Komponenten:

![Standorte](./media/active-directory-conditional-access-locations/42.png)

- **Name**: Der Anzeigename eines benannten Orts.

- **IP-Bereiche**: Mindestens ein Adressbereich im CIDR-Format.

- **Als vertrauenswürdigen Standort markieren**: Ein Flag, das Sie für einen benannten Standort festlegen können, um ihn als vertrauenswürdigen Standort auszuweisen. Normalerweise handelt es sich bei vertrauenswürdigen Standorten um Netzwerkbereiche, die der Kontrolle Ihrer IT-Abteilung unterliegen. Über den bedingten Zugriff hinaus werden vertrauenswürdige benannte Standorte auch in Sicherheitsberichten von Azure Identity Protection und Azure AD verwendet, um [falsch positive](active-directory-reporting-risk-events.md#impossible-travel-to-atypical-locations-1) Meldungen zu reduzieren.

- **Land/Region**: Mithilfe dieser Option können Sie Länder oder Regionen auswählen, um einen benannten Ort zu definieren. 

- **Unbekannte Bereiche einschließen**: Einige IP-Adressen sind keinem bestimmten Land zugeordnet. Mithilfe dieser Option können Sie auswählen, ob der benannte Standort diese IP-Adressen umfassen soll. Sie kann aktiviert werden, wenn die Richtlinie, die sich auf den benannten Standort bezieht, für unbekannte Standorte gelten soll.

Die Anzahl von benannten Orten, die Sie konfigurieren können, wird durch die Größe des zugehörigen Objekts in Azure AD eingeschränkt. Sie können Folgendes konfigurieren:

- Einen benannten Standort mit bis zu 1200 IP-Bereichen.

- Maximal 90 benannte Standorte, denen jeweils ein IP-Bereich zugewiesen ist.




## <a name="trusted-ips"></a>Vertrauenswürdige IP-Adressen

Ferner können Sie in den [Einstellungen für den mehrstufigen Authentifizierungsdienst](https://account.activedirectory.windowsazure.com/usermanagement/mfasettings.aspx) IP-Adressbereiche konfigurieren, die das lokale Intranet Ihrer Organisation darstellen. Mithilfe dieser Funktion können Sie bis zu 50 IP-Adressbereiche konfigurieren. Die IP-Adressbereiche müssen im CIDR-Format angegeben werden. Weitere Informationen finden Sie unter [vertrauenswürdige IPs](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).  

Wenn bei Ihnen vertrauenswürdige IPs konfiguriert sind, sind sie in der Liste der Standorte für die Standortbedingung als **Für MFA vertrauenswürdige IPs** aufgeführt.   

### <a name="skipping-multi-factor-authentication"></a>Überspringen der mehrstufigen Authentifizierung

Auf der Einstellungsseite für den mehrstufigen Authentifizierungsdienst können Sie Benutzer aus dem Unternehmensintranet identifizieren, indem Sie **Für Anforderungen von Partnerbenutzern in meinem Intranet die mehrstufige Authentifizierung überspringen** aktivieren. Diese Einstellung gibt an, dass der Anspruch innerhalb des Unternehmensnetzwerks, der von AD FS ausgestellt wird, als vertrauenswürdig angesehen und dazu verwendet wird, den Benutzer als innerhalb des Unternehmensnetzwerks ansässig zu erkennen. Weitere Informationen finden Sie unter [Aktivieren des Features vertrauenswürdige IPs beim bedingten Zugriff](../multi-factor-authentication/multi-factor-authentication-whats-next.md#enable-the-trusted-ips-feature-by-using-conditional-access).

Nach dem Aktivieren wird diese Option, einschließlich des benannten Standorts **Für MFA vertrauenswürdige IPs**, auf alle Richtlinien angewendet, für die dies ausgewählt ist.

Für mobile und Desktopanwendungen mit langer Sitzungslebensdauer wird der bedingte Zugriff in regelmäßigen Abständen neu ausgewertet. Die Standardeinstellung ist einmal pro Stunde. Wenn der Anspruch innerhalb des Unternehmensnetzwerks nur zum Zeitpunkt der erstmaligen Authentifizierung ausgegeben wird, verfügt Azure AD möglicherweise nicht über eine Liste der vertrauenswürdigen IP-Bereiche. In diesem Fall ist die Bestimmung, ob sich der Benutzer noch im Unternehmensnetzwerk befindet, schwieriger:

1. Überprüfen Sie, ob die IP-Adresse des Benutzers in einem der vertrauenswürdigen IP-Bereiche liegt.

2. Überprüfen Sie, ob die ersten drei Oktette der IP-Adresse des Benutzers mit den ersten drei Oktetten der IP-Adresse der erstmaligen Authentifizierung übereinstimmen. Die IP-Adresse wird mit der erstmaligen Authentifizierung verglichen, da diese den Zeitpunkt darstellt, zu dem der Anspruch innerhalb des internen Netzwerks ursprünglich ausgestellt und der Benutzerstandort überprüft wurde.

Wenn bei beiden Schritten ein Fehler auftritt, wird ein Benutzer nicht mehr als vertrauenswürdige IP angesehen.



## <a name="location-condition-configuration"></a>Konfiguration der Standortbedingung

Beim Konfigurieren der Standortbedingung können Sie zwischen diesen Optionen wählen:

- Jeden beliebigen Speicherort 
- Alle vertrauenswürdigen Speicherorte
- Ausgewählte Speicherorte

![Standorte](./media/active-directory-conditional-access-locations/01.png)

### <a name="any-location"></a>Jeden beliebigen Speicherort

Standardmäßig bewirkt das Aktivieren von **Alle Standorte**, dass eine Richtlinie auf alle IP-Adressen angewendet wird, was jede beliebige Adresse im Internet bedeutet. Diese Einstellung ist nicht auf IP-Adressen beschränkt, die von Ihnen als benannter Standort konfiguriert wurden. Wenn Sie **Alle Standorte** aktivieren, können Sie bestimmte Standorte trotzdem noch von einer Richtlinie ausschließen. Beispielsweise können Sie eine Richtlinie auf alle Standorte mit Ausnahme vertrauenswürdiger Standorte anwenden, um den Geltungsbereich auf alle Standorte mit Ausnahme des Unternehmensnetzwerks festzulegen.

### <a name="all-trusted-locations"></a>Alle vertrauenswürdigen Speicherorte

Diese Option gilt für:

- Alle Standorte, die als vertrauenswürdiger Standort gekennzeichnet wurden
- **Für MFA vertrauenswürdige IPs** (sofern konfiguriert)


### <a name="selected-locations"></a>Ausgewählte Speicherorte

Mit dieser Option können Sie einen oder mehrere benannte Speicherorte auswählen. Damit eine Richtlinie mit dieser Einstellung gilt, muss ein Benutzer von einem der ausgewählten Standorte aus eine Verbindung herstellen. Wenn Sie auf **Auswählen** klicken, wird das Steuerelement für die Auswahl von benannten Netzwerken geöffnet und zeigt die Liste der benannten Netzwerke an. In dieser Liste ist außerdem zu sehen, ob der Netzwerkstandort als vertrauenswürdig gekennzeichnet wurde. Der benannte Standort, der als **Für MFA vertrauenswürdige IPs** bezeichnet ist, wird zum Einschließen der IP-Einstellungen verwendet, die auf der Einstellungsseite des mehrstufigen Authentifizierungsdiensts konfiguriert werden können.

## <a name="what-you-should-know"></a>Wichtige Informationen

### <a name="when-is-a-location-evaluated"></a>Wann wird ein Standort ausgewertet?

Richtlinien für bedingten Zugriff werden in diesen Situationen ausgewertet: 

- Ein Benutzer meldet sich erstmalig an 

- Azure AD stellt ein Token für die Cloud-App aus, für die die Richtlinie zum bedingten Zugriff eingerichtet wurde. 

Standardmäßig stellt Azure AD stündlich ein Token aus. Nach dem Verlassen des Unternehmensnetzwerks wird die Richtlinie für Anwendungen mit moderner Authentifizierung innerhalb einer Stunde durchgesetzt.


### <a name="user-ip-address"></a>Benutzer-IP-Adresse

Die IP-Adresse, die in der Auswertung der Richtlinie verwendet wird, ist die öffentliche IP-Adresse des Benutzers. Bei Geräten in einem privaten Netzwerk ist dies nicht die Client-IP des Geräts des Benutzers im Intranet, sondern die vom Netzwerk für das Herstellen der Verbindung mit dem öffentlichen Internet verwendete Adresse. 

### <a name="bulk-uploading-and-downloading-of-named-locations"></a>Massenhoch- und -herunterladen von benannten Standorten

Beim Erstellen oder Aktualisieren benannter Standorte können Sie für eine Massenaktualisierung eine CSV-Datei mit den IP-Adressbereichen hoch- oder herunterladen. Bei einem Upload werden die IP-Bereiche in der Liste durch die aus der Datei ersetzt. Jede Zeile der Datei enthält einen IP-Adressbereich im CIDR-Format. 


### <a name="cloud-proxies-and-vpns"></a>Cloud-Proxys und VPNs 

Wenn Sie einen in der Cloud gehosteten Proxy oder eine VPN-Lösung verwenden, ist die von Azure AD bei der Auswertung einer Richtlinie verwendete IP-Adresse die IP-Adresse des Proxys. Der XFF-Header (X-Forwarded-For), der die öffentliche IP-Adresse des Benutzers enthält, wird nicht verwendet, da es keine Überprüfung gibt, ob er aus einer vertrauenswürdigen Quelle stammt, sodass er ein Verfahren zum Fälschen einer IP-Adresse darstellen kann. 

Wenn ein Cloud-Proxy zum Einsatz kommt, kann eine Richtlinie verwendet werden, mit der die Verwendung von der Domäne beigetretenen Geräten oder des Anspruchs aus dem internen Unternehmensnetzwerk von AD FS vorgeschrieben wird.



### <a name="api-support-and-powershell"></a>API-Unterstützung und PowerShell 

API und PowerShell werden für benannte Standorte oder Richtlinien für bedingten Zugriff noch nicht unterstützt.





## <a name="next-steps"></a>Nächste Schritte

- Wenn Sie wissen möchten, wie Sie eine Richtlinie für den bedingten Zugriff konfigurieren, helfen Ihnen die Informationen unter [Erste Schritte mit dem bedingten Zugriff in Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md) weiter.

- Wenn Sie bereit sind, Richtlinien für den bedingten Zugriff für Ihre Umgebung zu konfigurieren, lesen Sie unter [Best Practices für den bedingten Zugriff in Azure Active Directory](active-directory-conditional-access-best-practices.md) nach. 
