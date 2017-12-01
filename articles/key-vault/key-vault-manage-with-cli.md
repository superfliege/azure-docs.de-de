---
title: Verwalten von Azure Key Vault mit der CLI | Microsoft-Dokumentation
description: "Verwenden Sie dieses Lernprogramm zum Automatisieren von häufigen Aufgaben in Schlüsseltresor mithilfe der CLI"
services: key-vault
documentationcenter: 
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 66be6e44-684a-411b-802e-884628458ae7
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2017
ms.author: barclayn
ms.openlocfilehash: 94ea95e7f40c8d47dd18422a9c0795655dae365b
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2017
---
# <a name="manage-key-vault-using-cli"></a>Verwalten von Schlüsseltresor mit CLI

Verwenden Sie dieses Tutorial für die ersten Schritte mit Azure Key Vault, um einen geschützten Container (einen Tresor) in Azure zu erstellen. Azure Key Vault wird verwendet, um kryptografische Schlüssel und Geheimnisse zu speichern und zu verwalten. Dieser Artikel führt Sie durch die Verwendung der plattformübergreifenden Azure-Befehlszeilenschnittstelle zur Erstellung eines Tresors. Sie werden dann mit dem Tresor interagieren, um verschiedene allgemeine Operationen durchzuführen. 

Azure-Tresorschlüssel ist in den meisten Regionen verfügbar. Weitere Informationen finden Sie auf der Seite [Preisübersicht für Schlüsseltresor](https://azure.microsoft.com/pricing/details/key-vault/).

**Geschätzter Zeitaufwand:** 20 Minuten

> [!NOTE]
> Dieses Tutorial enthält keine Anweisungen zum Schreiben der Azure-Anwendung, die in einem der Schritte enthalten ist. Diese Beispielanwendung wird verwendet, um zu zeigen, wie eine Anwendung autorisiert werden kann, einen Schlüssel oder ein Geheimnis zu verwenden, das im Schlüsseltresor gespeichert ist.
> Dieser Artikel konzentriert sich auf die Konfiguration von Azure Key Vault mithilfe der plattformübergreifenden Befehlszeilenschnittstelle. Azure PowerShell-Anweisungen finden Sie unter [Äquivalentes Tutorial](key-vault-get-started.md).

Eine Übersicht über den Azure-Schlüsseltresor finden Sie unter [Was ist der Azure-Schlüsseltresor?](key-vault-whatis.md)

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* Ein Abonnement für Microsoft Azure. Wenn Sie kein Abonnement haben, können Sie sich für eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial)registrieren.
* Plattformübergreifende Azure-Befehlszeilenschnittstelle Version 0.9.1 oder aktueller. Informationen zum Installieren der aktuellen Version und Verbindungsaufbau mit Ihrem Azure-Abonnement, finden Sie unter [Installieren und Konfigurieren der plattformübergreifenden Azure-Befehlszeilenschnittstelle](../cli-install-nodejs.md).
* Eine Anwendung, die zur Verwendung des Schlüssels oder Kennworts konfiguriert wird, den bzw. das Sie in diesem Lernprogramm erstellen. Eine Beispielanwendung erhalten Sie im [Microsoft Download Center](http://www.microsoft.com/download/details.aspx?id=45343). Anweisungen finden Sie in der zugehörigen Readme-Datei.

## <a name="getting-help-with-azure-cross-platform-command-line-interface"></a>Hilfestellung für die plattformübergreifende Azure-Befehlszeilenschnittstelle

In diesem Lernprogramm wird davon ausgegangen, dass Sie mit der Befehlszeilenschnittstelle (Bash, Terminal, Eingabeaufforderung) vertraut sind.

Der Parameter --help oder -h kann angegeben werden, um Hilfetexte zu bestimmten Befehlen anzuzeigen. Stattdessen kann auch das Format azure help [Befehl] [Optionen] verwendet werden, um dieselben Informationen abzurufen. Beispielsweise geben die folgenden Befehle die gleichen Informationen zurück:

    azure account set --help

    azure account set -h

    azure help account set

Wenn Sie sich nicht ganz sicher sind, welche Parameter für einen Befehl angegeben werden müssen, rufen Sie mit --help, -h oder azure help [Befehl] die entsprechende Hilfe ab.

Lesen Sie bitte auch die folgenden Tutorials, um sich mit dem Azure Resource Manager-Bereitstellungsmodell in der plattformübergreifenden Azure-Befehlszeilenschnittstelle vertraut zu machen:

* [Installieren und Konfigurieren der plattformübergreifenden Azure-Befehlszeilenschnittstelle](../cli-install-nodejs.md)
* [Verwenden der plattformübergreifenden Azure-Befehlszeilenschnittstelle mit Azure-Ressourcen-Manager](../xplat-cli-azure-resource-manager.md)

## <a name="connect-to-your-subscriptions"></a>Verbindungsherstellung mit Ihren Abonnements

Melden Sie sich mithilfe des folgenden Befehls bei Azure an:

```azurecli-interactive
azure login -u username -p password
```

oder wenn Sie sich durch interaktive Eingabe anmelden möchten.

```azurecli-interactive
azure login
```

Wenn Sie über mehrere Abonnements verfügen und zur Verwendung für den Azure-Schlüsseltresor ein spezifisches Abonnement verwenden möchten, geben Sie den folgenden Befehl ein, um die Abonnements für Ihr Konto anzuzeigen:

```azurecli-interactive
azure account list
```

Geben Sie Folgendes ein, um das zu verwendende Abonnement anzugeben:

```azurecli-interactive
azure account set <subscription name>
```

Weitere Informationen über die Konfiguration der plattformübergreifenden Azure-Befehlszeilenschnittstellen finden Sie unter [Installieren und Konfigurieren der plattformübergreifenden Azure-Befehlszeilenschnittstelle](../cli-install-nodejs.md).

## <a name="switch-to-using-azure-resource-manager"></a>Wechseln zum Azure-Ressourcen-Manager
Der Schlüsseltresor erfordert Azure-Ressourcen-Manager, geben Sie deshalb den folgenden Befehl ein, um in den Azure-Ressourcen-Manager-Modus zu wechseln:

```azurecli-interactive
azure config mode arm
```

## <a name="create-a-new-resource-group"></a>Erstellen einer neuen Ressourcengruppe
Wenn Sie den Azure-Ressourcen-Manager verwenden, werden alle zugehörigen Ressourcen in einer Ressourcengruppe erstellt. Im Rahmen dieses Lernprogramms erstellen wir eine neue Ressourcengruppe mit dem Namen "ContosoResourceGroup".

```azurecli-interactive
azure group create 'ContosoResourceGroup' 'East Asia'
```

Der erste Parameter ist der Ressourcengruppenname und der zweite Parameter der Speicherort. Verwenden Sie für den Speicherort den Befehl `azure location list` , um zu ermitteln, wie Sie einen anderen Speicherort als den in diesem Beispiel verwendeten angeben können. Wenn Sie weitere Informationen benötigen, geben Sie Folgendes ein: `azure help location`

## <a name="register-the-key-vault-resource-provider"></a>Registrieren des Schlüsseltresor-Ressourcenanbieters
Stellen Sie sicher, dass der Schlüsseltresor-Ressourcenanbieter in Ihrem Abonnement registriert ist:

```azurecli-interactive
azure provider register Microsoft.KeyVault
```

Dies muss nur einmal pro Abonnement ausgeführt werden.

## <a name="create-a-key-vault"></a>Erstellen eines Schlüsseltresors

Verwenden Sie den Befehl `azure keyvault create` , um einen Schlüsseltresor zu erstellen. Das Skript verfügt über drei erforderliche Parameter: einen Ressourcengruppennamen, einen Schlüsseltresornamen und den geografischen Standort.

Beispiel:
- Wenn Sie den folgenden Tresornamen verwenden: **ContosoKeyVault**
- Ressourcengruppename: **ContosoResourceGroup**
- Standort: **Asien-Pazifik**, dann geben Sie Folgendes ein:

```azurecli-interactive
azure keyvault create --vault-name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'
```

Die Ausgabe dieses Befehls zeigt die Eigenschaften des Schlüsseltresors, den Sie soeben erstellt haben. Die zwei wichtigsten Eigenschaften sind diese:

* **Name**: Im Beispiel ist dies "ContosoKeyVault". Sie verwenden diesen Namen für andere Schlüsseltresor-Cmdlets.
* **VaultUri**: In diesem Beispiel ist dies https://contosokeyvault.vault.azure.net. Anwendungen, die Ihren Tresor über die zugehörige REST-API nutzen, müssen diesen URI verwenden.

Ihr Azure-Konto ist jetzt autorisiert, Vorgänge in diesem Schlüsseltresor durchzuführen. Bisher sind Sie der einzige Benutzer mit dieser Berechtigung.

## <a name="add-a-key-or-secret-to-the-key-vault"></a>Hinzufügen eines Schlüssels oder geheimen Schlüssels zum Schlüsseltresor

Wenn Sie mit dem Azure-Schlüsseltresor einen softwaregeschützten Schlüssel erstellen möchten, verwenden Sie hierzu den Befehl `azure key create` und geben Sie Folgendes ein:

```azurecli-interactive
azure keyvault key create --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --destination software
````

Wenn Sie jedoch einen vorhandenen Schlüssel in einer PEM-Datei als lokale Datei in einer Datei namens softkey.pem gespeichert haben, die Sie in Azure-Schlüsseltresor hochladen möchten, geben Sie Folgendes ein, um den Schlüssel aus der PEM-Datei zu importieren, die den Schlüssel durch Software im Schlüsseltresor-Dienst schützt:

```azurecli-interactive
azure keyvault key import --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --pem-file './softkey.pem' --password 'PaSSWORD' --destination software
```

Jetzt können Sie mittels seiner URI auf den Schlüssel verweisen, den Sie erstellt oder in  Azure-Schlüsseltresor hochgeladen haben. Verwenden Sie **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey**, um immer die aktuelle Version zu erhalten, und **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87**, um diese bestimmte Version zu erhalten.

Um einen geheimen Schlüssel zum Schlüsseltresor hinzuzufügen – in diesem Fall das Kennwort "SQLPassword" mit dem Wert "Pa$$w0rd" für den Azure-Schlüsseltresor – geben Sie Folgendes ein:

```azurecli-interactive
azure keyvault secret set --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword' --value 'Pa$$w0rd'
```

Jetzt können Sie mit dem zugehörigen URI auf das Kennwort verweisen, das Sie dem Azure-Schlüsseltresor hinzugefügt haben. Verwenden Sie **https://ContosoVault.vault.azure.net/secrets/SQLPassword**, um immer die aktuelle Version zu erhalten, und **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d**, um diese bestimmte Version zu erhalten.

Zeigen wir den Schlüssel oder geheimen Schlüssel an, den Sie soeben erstellt haben:

* Geben Sie Folgendes ein, um Ihren Schlüssel anzuzeigen: `azure keyvault key list --vault-name 'ContosoKeyVault'`
* Geben Sie Folgendes ein, um Ihren geheimen Schlüssel anzuzeigen: `azure keyvault secret list --vault-name 'ContosoKeyVault'`

## <a name="register-an-application-with-azure-active-directory"></a>Registrieren einer Anwendung mit Azure Active Directory

Dieser Schritt wird üblicherweise durch einen Entwickler auf einem separaten Computer durchgeführt. Dieser Schritt ist nicht spezifisch für den Azure-Schlüsseltresor, wird der Vollständigkeit halber jedoch hier aufgeführt.

> [!IMPORTANT]
> Zum Abschließen dieses Tutorials müssen sich Ihr Konto, der Schlüsseltresor und die in diesem Schritt registrierte Anwendung im selben Azure Active Directory befinden.

Anwendungen, die einen Schlüsseltresor verwenden, müssen sich mithilfe eines Azure Active Directory-Tokens authentifizieren. Hierzu muss der Besitzer der Anwendung die Anwendung zunächst in Azure Active Directory registrieren. Zum Abschluss der Registrierung erhält der Anwendungsbesitzer die folgenden Werte:

- Eine **Anwendungs-ID** 
- Einen **Authentifizierungsschlüssel** (auch bezeichnet als gemeinsames Geheimnis) 

Die Anwendung muss beide dieser Werte in Azure Active Directory vorlegen, um ein Token zu erhalten.  Wie die Anwendung konfiguriert wird, um dies zu erreichen, richtet sich nach der Anwendung. Bei der [Key Vault-Beispielanwendung](https://www.microsoft.com/download/details.aspx?id=45343) legt der Anwendungsbesitzer diese Werte in der Datei „app.config“ fest.

Ausführliche Anweisungen zum Registrieren einer Anwendung in Azure Active Directory finden Sie im Artikel [Integrieren von Anwendungen in Azure Active Directory](../active-directory/develop/active-directory-integrating-applications.md) oder [Erstellen einer Azure Active Directory-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff mithilfe des Portals](../azure-resource-manager/resource-group-create-service-principal-portal.md). So registrieren Sie die Anwendung in Azure Active Directory

1. Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie im linken Bereich auf **App-Registrierungen**. Falls keine App-Registrierungen angezeigt werden, klicken Sie auf **Weitere Dienste**, und suchen Sie nach der Registrierung.  
    >[!NOTE]
    Sie müssen dasselbe Verzeichnis auswählen, in dem auch das Azure-Abonnement enthalten ist, mit dem Sie Ihren Schlüsseltresor erstellt haben. 
3. Klicken Sie auf **Registrierung einer neuen Anwendung**.
4. Geben Sie auf dem Blatt **Erstellen** einen Namen für die Anwendung ein, wählen Sie dann **WEBANWENDUNG UND/ODER WEB-API** (Standardeinstellung) aus, und geben Sie die **ANMELDE-URL** für die Webanwendung ein. Falls Sie zu diesem Zeitpunkt nicht über diese Informationen verfügen, können Sie eine beliebige URL angeben (z. B. http://test1.contoso.com). Hierbei spielt es keine Rolle, ob diese Websites wirklich vorhanden sind. 

   ![Registrierung einer neuen Anwendung](./media/key-vault-manage-with-cli/new-application-registration.png)

5. Klicken Sie auf die Schaltfläche **Erstellen** .
6. Nach Abschluss der App-Registrierung können Sie die Liste der registrierten Apps anzeigen. Suchen Sie nach der App, die Sie gerade registriert haben, und klicken Sie darauf.
7. Klicken Sie auf das Blatt **Registrierte App**, und kopieren Sie die **Anwendungs-ID**.
8. Klicken Sie auf **Alle Einstellungen**.
9. Klicken Sie auf dem Blatt **Einstellungen** auf **Schlüssel**.
10. Geben Sie eine Beschreibung in das Feld **Schlüsselbeschreibung** ein, wählen Sie eine Dauer aus, und klicken Sie dann auf **SPEICHERN**. Die Seite wird aktualisiert und zeigt jetzt einen Schlüsselwert. 
11. Sie verwenden die **Anwendungs-ID** und die Informationen zum **Schlüssel** im nächsten Schritt, um Berechtigungen für den Tresor festzulegen.

## <a name="authorize-the-application-to-use-the-key-or-secret"></a>Autorisieren der Anwendung zum Verwenden des Schlüssels oder geheimen Schlüssels
Verwenden Sie Folgendes, um die Anwendung zum Zugreifen auf den Schlüssel oder geheimen Schlüssel im Tresor zu autorisieren:

```azurecli-interactive
azure keyvault set-policy
```

Wenn Ihr Tresorname beispielsweise "ContosoKeyVault" lautet, die Anwendung, die Sie autorisieren möchten, über die Client-ID 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed verfügt, und Sie die Anwendung zum Entschlüsseln und Anmelden mit Schlüsseln in Ihrem Tresor autorisieren möchten, führen Sie Folgendes aus:

```azurecli-interactive
azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-keys '[\"decrypt\",\"sign\"]'
```

> [!NOTE]
> Wenn Sie an einer Windows-Eingabeaufforderung arbeiten, sollten Sie die einfachen durch doppelte Anführungszeichen ersetzen und die internen doppelten Anführungszeichen mit Escapezeichen versehen. Beispiel: "[\"decrypt\",\"sign\"]".
> 

Wenn Sie dieselbe Anwendung so autorisieren möchten, dass sie geheime Schlüssel im Tresor liest, führen Sie Folgendes aus:

```azurecli-interactive
azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-secrets '[\"get\"]'
```

## <a name="if-you-want-to-use-a-hardware-security-module-hsm"></a>Verwenden eins Hardwaresicherheitsmodul (HSM)
Zur Steigerung der Sicherheit können Sie Schlüssel in HSMs importieren oder in diesen generieren. Diese Schlüssel verbleiben immer innerhalb der HSM-Grenzen. Die HSMs sind FIPS 140-2 Ebene 2 überprüft. Wenn diese Anforderung auf Sie nicht zutrifft, überspringen Sie diesen Abschnitt, und wechseln Sie zu [Löschen des Schlüsseltresors und zugeordneter Schlüssel und geheimer Schlüssel](#delete-the-key-vault-and-associated-keys-and-secrets).

Um diese HSM-geschützten Schlüssel zu erstellen, müssen Sie über ein Tresorabonnement mit Unterstützung von HSM-geschützten Schlüsseln verfügen.

Wenn Sie den Tresor erstellen, fügen Sie den Parameter "SKU" hinzu:

```azurecli-interactive
azure azure keyvault create --vault-name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'
```

Sie können diesem Tresor softwaregeschützte Schlüssel (wie weiter oben gezeigt) und HSM-geschützte Schlüssel hinzufügen. Legen Sie den Parameter "Destination" auf "HSM" fest, um einen HSM-geschützten Schlüssel zu erstellen:

```azurecli-interactive
azure keyvault key create --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --destination 'HSM'
```

Sie können mit dem folgenden Befehl einen Schlüssel aus einer PEM-Datei auf Ihren Computer importieren. Dieser Befehl importiert den Schlüssel in HSMs im Schlüsseltresordienst:

```azurecli-interactive
azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --destination 'HSM' --password 'PaSSWORD'
```

Der nächste Befehl importiert ein BYOK-Paket (Bring Your Own Key). Auf diese Weise können Sie Ihren Schlüssel im lokalen HSM generieren und ihn in HSMs im Schlüsseltresordienst übertragen, ohne dass der Schlüssel die HSM-Grenzen verlässt:

```azurecli-interactive
azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --destination 'HSM'
```

Ausführlichere Informationen zum Generieren dieses BYOK-Pakets finden Sie unter [Verwenden von HSM-geschützten Schlüsseln mit dem Azure-Schlüsseltresor](key-vault-hsm-protected-keys.md).

## <a name="delete-the-key-vault-and-associated-keys-and-secrets"></a>Löschen vom Schlüsseltresor sowie von zugeordneten Schlüsseln und geheimen Schlüsseln
Wenn Sie den Schlüsseltresor und die darin enthaltenen Schlüssel und geheimen Schlüssel nicht länger benötigen, können Sie den Schlüsseltresor mit dem Azure-Schlüsseltresor-Löschbefehl löschen:

```azurecli-interactive
azure keyvault delete --vault-name 'ContosoKeyVault'
```

Alternativ können Sie eine gesamte Azure-Ressourcengruppe löschen, die den Schlüsseltresor und alle weiteren Ressourcen enthält, die Sie in diese Gruppe eingeschlossen haben:

```azurecli-interactive
azure group delete --name 'ContosoResourceGroup'
```


## <a name="other-azure-cross-platform-command-line-interface-commands"></a>Weitere plattformübergreifende Microsoft Azure-Befehlszeilenschnittstelle-Befehle
Die folgenden weiteren Befehle sind möglicherweise ebenfalls für das Verwalten eines Azure-Schlüsseltresors von Nutzen.

Dieser Befehl ruft eine tabellarische Anzeige aller Schlüssel und ausgewählten Eigenschaften ab:

```azurecli-interactive
azure keyvault key list --vault-name 'ContosoKeyVault'
```

Dieser Befehl zeigt eine vollständige Liste der Eigenschaften für den angegebenen Schlüssel an:

```azurecli-interactive
azure keyvault key show --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'
```

Dieser Befehl ruft eine tabellarische Anzeige der Namen aller geheimen Schlüssel und ausgewählten Eigenschaften ab:

```azurecli-interactive
azure keyvault secret list --vault-name 'ContosoKeyVault'
```

Hier ist Beispiel, wie ein bestimmter Schlüssel entfernt werden kann:

```azurecli-interactive
azure keyvault key delete --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'
```

Hier ist ein Beispiel, wie ein bestimmten geheimer Schlüssel entfernt werden kann:

```azurecli-interactive
azure keyvault secret delete --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword'
```


## <a name="next-steps"></a>Nächste Schritte
- Eine Referenz zur Programmierung finden Sie im [Entwicklerhandbuch für den Azure-Schlüsseltresor](key-vault-developers-guide.md).
- Eine Übersicht über den Azure-Schlüsseltresor finden Sie unter [Was ist der Azure-Schlüsseltresor?](key-vault-whatis.md)
- Informationen zum Arbeiten mit Azure Key Vault mithilfe von PowerShell finden Sie unter [Erste Schritte mit Azure Key Vault](key-vault-get-started.md).


