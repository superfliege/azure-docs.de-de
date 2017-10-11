---
title: Zugriff auf Key Vault hinter einer Firewall | Microsoft Docs
description: Erfahren Sie, wie eine Anwendung hinter einer Firewall Azure Key Vault zugreifen
services: key-vault
documentationcenter: 
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 50d21774-2ee1-4212-8995-570c9de603c5
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/07/2017
ms.author: ambapat
ms.openlocfilehash: d00c6e0acf437d2bfc3c27e948f4646a6685b08f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="access-azure-key-vault-behind-a-firewall"></a>Zugriff Azure Key Vault hinter einer firewall
### <a name="q-my-key-vault-client-application-needs-to-be-behind-a-firewall-what-ports-hosts-or-ip-addresses-should-i-open-to-enable-access-to-a-key-vault"></a>F: Meine schlüsseltresor-Clientanwendung muss hinter einer Firewall befinden. Welche Ports, die Hosts oder die IP-Adressen muss ich öffnen Sie zum Aktivieren des Zugriffs auf einen schlüsseltresor?
Verfügt über einen schlüsseltresor für den Zugriff auf die schlüsseltresor-Clientanwendung auf mehrere Endpunkte für verschiedene Funktionen:

* Authentifizierung über Azure Active Directory (Azure AD).
* Verwaltung von Azure Key Vault. Dies schließt erstellen, lesen, aktualisieren, löschen und Festlegen von Richtlinien für den Zugriff über Azure-Ressourcen-Manager.
* Zugreifen auf und Verwalten von Objekten (Schlüsseln und geheimen Schlüsseln), die im Schlüssel gespeichert Tresor Sie selbst, durchlaufen die Key Vault-spezifische-Endpunkt (z. B. [https://yourvaultname.vault.azure.net](https://yourvaultname.vault.azure.net)).  

Abhängig von Ihrer Konfiguration und die Umgebung gibt es einige Varianten.   

## <a name="ports"></a>Ports
Gesamte Datenverkehr an einen schlüsseltresor für alle drei Funktionen (Authentifizierung, Verwaltungs- und die Ebene des Zugriffs) wird über HTTPS: port 443. Allerdings werden gibt es gelegentlich HTTP (Port 80)-Datenverkehr für Zertifikatsperrlisten. Clients, die Unterstützung von OCSP darf keine CRL erreichen, aber möglicherweise gelegentlich erreichen [http://cdp1.public-trust.com/CRL/Omniroot2025.crl](http://cdp1.public-trust.com/CRL/Omniroot2025.crl).  

## <a name="authentication"></a>Authentifizierung
Schlüsseltresor-Clientanwendungen müssen auf Azure Active Directory-Endpunkte für die Authentifizierung. Der Endpunkt verwendet, hängt von der Azure AD-Mandanten-Konfiguration, der Typ des Prinzipals (Benutzerprinzipalnamen oder einen Dienstprinzipal), und der Typ des Konto an – beispielsweise eine Microsoft-Konto oder ein Geschäfts- oder schulkonto an.  

| Prinzipaltyp | Endpunktport: |
| --- | --- |
| Benutzer, die mit Microsoft-Konto<br> (z. B. user@hotmail.com) |**Global:**<br> Login.microsoftonline.com:443<br><br> **Azure China:**<br> Login.chinacloudapi.CN:443<br><br>**Azure US-Regierung:**<br> Login-us.microsoftonline.com:443<br><br>**Azure Deutschland:**<br> Login.microsoftonline.de:443<br><br> - und <br>Login.Live.com:443 |
| Benutzer oder Dienstprinzipal mithilfe der Geschäfts-oder schulkonto bei Azure AD (z. B. user@contoso.com) |**Global:**<br> Login.microsoftonline.com:443<br><br> **Azure China:**<br> Login.chinacloudapi.CN:443<br><br>**Azure US-Regierung:**<br> Login-us.microsoftonline.com:443<br><br>**Azure Deutschland:**<br> Login.microsoftonline.de:443 |
| Benutzer oder Dienstprinzipal über ein Geschäfts- oder schulkonto sowie Active Directory-Verbunddienste (AD FS) oder andere verbundendpunkt (z. B. user@contoso.com) |Alle Endpunkte für ein Geschäfts- oder schulkonto, plus AD FS oder andere federated Endpunkte |

Es gibt andere komplexe Szenarien. Verweisen auf [Azure Active Directory-Authentifizierung Flow](/documentation/articles/active-directory-authentication-scenarios/), [Integrieren von Anwendungen in Azure Active Directory](/documentation/articles/active-directory-integrating-applications/), und [Active Directory-Authentifizierungsprotokolle](https://msdn.microsoft.com/library/azure/dn151124.aspx) zusätzliche Informationen.  

## <a name="key-vault-management"></a>Key Vault-management
Für die Key Vault-Verwaltung (CRUD und Festlegen der Zugriffsrichtlinie) muss die schlüsseltresor-Clientanwendung einen Azure Resource Manager-Endpunkt zuzugreifen.  

| Typ der Operation | Endpunktport: |
| --- | --- |
| Key Vault Steuerungsvorgänge-Ebene<br> über Azure Ressourcen-Manager |**Global:**<br> Management.Azure.com:443<br><br> **Azure China:**<br> Management.chinacloudapi.CN:443<br><br> **Azure US-Regierung:**<br> Management.usgovcloudapi.NET:443<br><br> **Azure Deutschland:**<br> Management.microsoftazure.de:443 |
| Azure Active Directory Graph-API |**Global:**<br> Graph.Windows.NET:443<br><br> **Azure China:**<br> Graph.chinacloudapi.CN:443<br><br> **Azure US-Regierung:**<br> Graph.Windows.NET:443<br><br> **Azure Deutschland:**<br> Graph.cloudapi.de:443 |

## <a name="key-vault-operations"></a>Key Vault-Vorgänge
Der schlüsseltresor-Client muss für alle schlüsseltresorverwaltung-Objekt (Schlüsseln und geheimen Schlüsseln) und kryptografische Vorgänge auf den schlüsseltresor-Endpunkt zugreifen. Das DNS-endpunktsuffix variiert je nach Speicherort Ihres schlüsseltresors. Der schlüsseltresor-Endpunkt weist das Format *tresorname*. *Region-spezifische-Dns-Suffix*, wie in der folgenden Tabelle beschrieben.  

| Typ der Operation | Endpunktport: |
| --- | --- |
| Vorgänge, einschließlich Kryptografievorgänge für Schlüssel; Erstellen, lesen, aktualisieren und Löschen von Schlüsseln und geheimen Schlüsseln; Festlegen oder Abrufen von Tags und andere Attribute für Key Vault-Objekte (Schlüssel oder Kennwörter) |**Global:**<br> &lt;tresorname&gt;. vault.azure.net:443<br><br> **Azure China:**<br> &lt;tresorname&gt;. vault.azure.cn:443<br><br> **Azure US-Regierung:**<br> &lt;tresorname&gt;. vault.usgovcloudapi.net:443<br><br> **Azure Deutschland:**<br> &lt;tresorname&gt;. vault.microsoftazure.de:443 |

## <a name="ip-address-ranges"></a>IP-Adressbereiche
Die Key Vault-Diensts verwendet andere Azure-Ressourcen wie PaaS-Infrastruktur. Daher ist es nicht möglich, einen bestimmten Bereich von IP-Adressen angeben, für die Key Vault-Dienstendpunkte zu einem bestimmten Zeitpunkt. Wenn Ihre Firewall nur IP-Adressbereiche unterstützt, finden Sie in der [Microsoft Azure Datacenter IP-Adressbereiche](https://www.microsoft.com/download/details.aspx?id=41653) Dokument. Für die Authentifizierung und Identität (Azure Active Directory), die Anwendung muss möglicherweise auf die Endpunkte, die in beschriebenen herstellen [Authentifizierung und Identität Adressen](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie Fragen zum Schlüsseltresor haben, besuchen Sie die [in Azure Key Vault-Foren](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).

