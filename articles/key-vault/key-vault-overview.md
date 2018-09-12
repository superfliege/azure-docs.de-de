---
title: Übersicht über Azure Key Vault | Microsoft-Dokumentation
description: Azure Key Vault ist ein Clouddienst, der als sicherer Geheimnisspeicher fungiert.
services: key-vault
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 34af20ee-3fa7-4f28-9d98-6168b1759764
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.custom: mvc
ms.date: 07/17/2018
ms.author: barclayn
ms.openlocfilehash: b509bf9575c3fdeedd1673d4ffe1395d46e3d919
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44051611"
---
# <a name="what-is-azure-key-vault"></a>Was ist der Azure-Schlüsseltresor?

Mit Azure Key Vault lassen sich folgende Probleme lösen:
- Azure Key Vault ermöglicht die sichere Speicherung und präzise Steuerung des Zugriffs auf Token, Kennwörter, Zertifikate, API-Schlüssel und andere Geheimnisse.
- Azure Key Vault kann auch als Schlüsselverwaltungslösung verwendet werden. Azure Key Vault vereinfacht das Erstellen und Verwalten der zur Verschlüsselung Ihrer Daten verwendeten Verschlüsselungsschlüssel. 
- Darüber hinaus können Sie mit dem Azure Key Vault-Dienst komfortabel öffentliche und private SSL-/TLS-Zertifikate (Secure Sockets Layer/Transport Layer Security) für die Verwendung mit Azure und Ihren internen verbundenen Ressourcen bereitstellen und verwalten. 
- Die Geheimnisse und Schlüssel können entweder per Software oder mit FIPS 140-2 Level 2 HSMs geschützt werden.

## <a name="why-use-azure-key-vault"></a>Gründe für die Verwendung von Azure Key Vault

### <a name="centralize-application-secrets"></a>Zentralisieren von Anwendungsgeheimnissen

Durch die zentralisierte Speicherung von Anwendungsgeheimnissen in Azure Key Vault können Sie deren Verteilung steuern. Mit Key Vault lässt sich das Risiko einer unbeabsichtigten Weitergabe von Geheimnissen erheblich senken. Dank Key Vault müssen Anwendungsentwickler Sicherheitsinformationen nicht mehr in der Anwendung speichern. Die Informationen müssen also nicht mehr in den Code integriert werden. Ein Beispiel: Angenommen, eine Anwendung muss eine Verbindung mit einer Datenbank herstellen. In diesem Fall können Sie die Verbindungszeichenfolge sicher in Key Vault speichern anstatt im Anwendungscode.

Über URIs können Ihre Anwendungen sicher auf die benötigten Informationen zugreifen. Die URIs ermöglichen das Abrufen spezifischer Versionen eines Geheimnisses, nachdem der Schlüssel oder das Geheimnis der Anwendung in Azure Key Vault gespeichert wurde. Dabei muss keinerlei benutzerdefinierter Code geschrieben werden, um geheime Informationen zu schützen.

### <a name="securely-store-secrets-and-keys"></a>Sicheres Speichern von Geheimnissen und Schlüsseln

Geheimnisse und Schlüssel werden mit branchenüblichen Algorithmen, Schlüssellängen und Hardwaresicherheitsmodulen (HSMs) von Azure geschützt. Die verwendeten HSMs erfüllen die Anforderungen von FIPS 140-2, Level 2 (Federal Information Processing Standards).

Ein Aufrufer (Benutzer oder Anwendung) kann erst nach ordnungsgemäßer Authentifizierung und Autorisierung auf einen Schlüsseltresor zugreifen. Bei der Authentifizierung wird die Identität des Aufrufers ermittelt. Bei der Autorisierung wird dagegen bestimmt, welche Vorgänge der Aufrufer ausführen darf.

Die Authentifizierung erfolgt über Azure Active Directory. Für die Autorisierung kann die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) oder eine Key Vault-Zugriffsrichtlinie verwendet werden. RBAC kommt bei der Verwaltung der Tresore zum Einsatz. Eine Key Vault-Zugriffsrichtlinie wird für den Zugriff auf gespeicherte Daten in einem Tresor verwendet.

Azure Key Vault-Instanzen können durch Software oder Hardware (HSM) geschützt werden. In Szenarien mit erhöhten Sicherheitsanforderungen können Sie Schlüssel in Hardwaresicherheitsmodule (HSMs) importieren oder darin generieren. Diese Schlüssel bleiben immer innerhalb der HSM-Grenzen. Microsoft verwendet Hardwaresicherheitsmodule von Thales. Sie können Thales-Tools verwenden, um einen Schlüssel aus Ihrem HSM in Azure Key Vault zu verschieben.

Darüber hinaus ist Azure Key Vault so konzipiert, dass Ihre Daten von Microsoft weder angezeigt noch extrahiert werden können.

### <a name="monitor-access-and-use"></a>Überwachen von Zugriff und Verwendung

Nachdem Sie eine Reihe von Key Vault-Instanzen erstellt haben, können Sie überwachen, wie und wann auf Ihre Schlüssel und Geheimnisse zugegriffen wird. Hierzu können Sie die Protokollierung für Key Vault aktivieren. Azure Key Vault kann für Folgendes konfiguriert werden:

- Archivieren in einem Speicherkonto
- Streamen an einen Event Hub
- Senden der Protokolle an Log Analytics

Sie haben die Kontrolle über Ihre Protokolle: Sie können den Zugriff auf Protokolle einschränken, um sie zu schützen, und Sie können nicht mehr benötigte Protokolle löschen.

### <a name="simplified-administration-of-application-secrets"></a>Einfachere Verwaltung von Anwendungsgeheimnissen

Beim Speichern wertvoller Daten sind mehrere Punkte zu berücksichtigen: Sicherheitsinformationen müssen geschützt werden, für die Informationen muss ein Lebenszyklus konfiguriert werden, und die Informationen müssen hochverfügbar sein. Azure Key Vault vereinfacht einen Großteil dieser Punkte durch Folgendes:

- Beseitigung des Bedarfs für interne Kenntnisse von Hardware-Sicherheitsmodellen
- Kurzfristige zentrale Hochskalierung zur Abdeckung von Auslastungsspitzen Ihrer Organisation
- Replikation des Inhalts Ihrer Key Vault-Instanz innerhalb einer Region und in einer sekundären Region. Key Vault gewährleistet die Hochverfügbarkeit der Informationen, und ein Failover kann ganz ohne Eingriff des Administrators ausgelöst werden.
- Bereitstellung standardmäßiger Azure-Verwaltungsoptionen über das Portal, über die Azure-Befehlszeilenschnittstelle und über PowerShell
- Automatisierung bestimmter Aufgaben im Zusammenhang mit Zertifikaten, die Sie von öffentlichen Zertifizierungsstellen erwerben (beispielsweise Registrierung und Verlängerung)

Darüber hinaus können Azure Key Vault-Instanzen auch zur Isolierung von Anwendungsgeheimnissen verwendet werden. Anwendungen können nur auf den Tresor zugreifen, für den sie zugriffsberechtigt sind, und die ausführbaren Vorgänge können auf bestimmte Vorgänge beschränkt werden. Sie können eine Azure Key Vault-Instanz pro Anwendung erstellen und die in einer Key Vault-Instanz gespeicherten Geheimnisse auf eine bestimmte Anwendung und ein bestimmtes Entwicklerteam beschränken.

### <a name="integrate-with-other-azure-services"></a>Integrieren in andere Azure-Dienste

Als sicherer Speicher in Azure vereinfacht Key Vault beispielsweise Szenarien mit [Azure Disk Encryption](../security/azure-security-disk-encryption.md), mit der Funktion [Always Encrypted]( https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) in SQL Server und Azure SQL sowie mit [Azure-Web-Apps]( https://docs.microsoft.com/azure/app-service/web-sites-purchase-ssl-web-site). Key Vault selbst kann in Speicherkonten, Event Hubs und Protokollanalysen integriert werden.

## <a name="next-steps"></a>Nächste Schritte

- [Schnellstart: Erstellen einer Azure Key Vault-Instanz mithilfe der Befehlszeilenschnittstelle](quick-create-cli.md)
- [Tutorial: Konfigurieren einer Azure-Webanwendung zum Lesen eines Geheimnisses aus Key Vault](tutorial-web-application-keyvault.md)
