---
title: SSL-Terminierung mit Key Vault-Zertifikaten
description: Erfahren Sie, wie Sie Azure Application Gateway mit Key Vault für Serverzertifikate integrieren können, die an HTTPS-fähige Listener angefügt sind.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 4/19/2019
ms.author: victorh
ms.openlocfilehash: 9ab1a42578081d4e6537f221e3f3a8f4c7babde0
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2019
ms.locfileid: "60012984"
---
# <a name="ssl-termination-with-key-vault-certificates"></a>SSL-Terminierung mit Key Vault-Zertifikaten

[Azure Key Vault](../key-vault/key-vault-whatis.md) ist ein als Plattform verwalteter Geheimnisspeicher, mit dem Sie Geheimnisse, Schlüssel und SSL-Zertifikate schützen können. Application Gateway unterstützt die Integration mit Key Vault (in der öffentlichen Vorschau) für Serverzertifikate, die an HTTPS-fähige Listener angefügt sind. Diese Unterstützung ist auf die v2-SKU von Application Gateway beschränkt.

> [!IMPORTANT]
> Die Key Vault-Integration für Application Gateway befindet sich derzeit in der öffentlichen Vorschau. Diese Vorschau wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Es gibt zwei Modelle für die SSL-Terminierung mit dieser öffentlichen Vorschau:

- Sie können explizit SSL-Zertifikate angeben, die an den Listener angefügt sind. Dies ist das traditionelle Modell der Übergabe von SSL-Zertifikaten an Application Gateway für die SSL-Terminierung.
- Sie können bei der Erstellung des HTTPS-fähigen Listeners optional einen Verweis auf ein vorhandenes Key Vault-Zertifikat oder Geheimnis angeben.

Die Key Vault-Integration bietet viele Vorteile, einschließlich:

- Höhere Sicherheit, da SSL-Zertifikate nicht direkt vom Anwendungsentwicklungsteam gehandhabt werden. Durch die Integration in Key Vault kann ein getrenntes Sicherheitsteam ausgewählte Anwendungsgateways bereitstellen, ihren Lebenszyklus steuern und die entsprechenden Berechtigungen zum Zugreifen auf in Key Vault gespeicherte Zertifikate gewähren.
- Unterstützung des Imports vorhandener Zertifikate in Key Vault und der Verwendung der Key Vault-APIs zum Erstellen und Verwalten neuer Zertifikate bei den vertrauenswürdigen Key Vault-Partnern.
- Unterstützung für automatische Verlängerung von in Key Vault gespeicherten Zertifikaten.

Application Gateway unterstützt derzeit nur per Software überprüfte Zertifikate. Durch das Hardwaresicherheitsmodul (HSM) überprüfte Zertifikate werden nicht unterstützt. Sobald Application Gateway für die Verwendung von Key Vault-Zertifikaten konfiguriert wurde, rufen die Instanzen das Zertifikat von Key Vault ab und installieren sie lokal für die SSL-Terminierung. Die Instanzen rufen außerdem Key Vault regelmäßig in einem 24-Stunden-Intervall ab, um ggf. eine erneuerte Version des Zertifikats zu erhalten. Wenn ein aktualisiertes Zertifikat gefunden wird, wird das derzeit dem HTTPS-Listener zugeordnete SSL-Zertifikat automatisch rotiert.

## <a name="how-it-works"></a>So funktioniert's

Die Integration in Key Vault erfolgt über eine Konfiguration in drei Schritten:

1. **Erstellen einer benutzerseitig zugewiesenen verwalteten Identität**

   Sie müssen eine benutzerseitig zugewiesene verwaltete Identität erstellen oder eine bereits vorhandenen erneut verwenden, über die Application Gateway in Ihrem Namen Zertifikate von Key Vault abruft. Weitere Informationen finden Sie unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](../active-directory/managed-identities-azure-resources/overview.md). In diesem Schritt wird eine neue Identität im Azure AD-Mandanten erstellt, der vom Abonnement zum Erstellen der Identität als vertrauenswürdig eingestuft wird.
1. **Konfigurieren von Key Vault**

   Sie müssen dann ein Zertifikat in Key Vault importieren oder ein neues erstellen, das von Anwendungen verwendet wird, die über Application Gateway ausgeführt werden. In diesem Schritt kann auch ein Key Vault-Geheimnis verwendet werden, das als Base64-codierte PFX-Datei ohne Kennwort gespeichert ist. Die Verwendung eines Zertifikattyps wird bevorzugt, da für Zertifikattypobjekte in Key Vault Funktionen für die automatische Verlängerung zur Verfügung stehen. Nach dem Erstellen eines Zertifikats oder Geheimnisses müssen Zugriffsrichtlinien in Key Vault definiert werden, durch die der Identität *Abrufzugriff* zum Erhalten des Geheimnisses gewährt werden können.

1. **Konfigurieren von Application Gateway**

   Nachdem die beiden vorherigen Schritte abgeschlossen sind, können Sie ein Anwendungsgateway bereitstellen oder ein vorhandenes ändern, sodass die vom Benutzer zugewiesene verwaltete Identität verwendet wird. Sie können auch das SSL-Zertifikat des HTTP-Listeners so konfigurieren, dass es auf den vollständigen URI der ID des Key Vault-Zertifikats oder -Geheimnisses zeigt.

![Key Vault-Zertifikate](media/key-vault-certs/ag-kv.png)

## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren von SSL-Terminierung mit Key Vault-Zertifikaten über Azure PowerShell](configure-keyvault-ps.md)