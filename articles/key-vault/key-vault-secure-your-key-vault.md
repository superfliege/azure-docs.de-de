---
title: Schützen einer Azure Key Vault-Instanz – Azure Key Vault | Microsoft-Dokumentation
description: Verwalten Sie Zugriffsberechtigungen für den Azure Key Vault, Schlüssel und Geheimnisse. In diesem Artikel wird das Authentifizierungs- und Autorisierungsmodell für Key Vault und das Schützen eines Schlüsseltresors behandelt.
services: key-vault
documentationcenter: ''
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: e5b4e083-4a39-4410-8e3a-2832ad6db405
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: ambapat
ms.openlocfilehash: 8a0300eeda49d85ffc08db8f285550e217613dcf
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55821614"
---
# <a name="secure-your-key-vault"></a>Schützen einer Key Vault-Instanz

Der Azure Key Vault-Clouddienst schützt Verschlüsselungsschlüssel und Geheimnisse (wie Zertifikate, Verbindungszeichenfolgen und Kennwörter). Da es sich hierbei um vertrauliche und geschäftskritische Daten handelt, müssen Sie den Zugriff auf Schlüsseltresore schützen, sodass nur autorisierte Anwendungen und Benutzer zugelassen sind. In diesem Artikel finden Sie eine Übersicht des Modells für den Zugriff auf Schlüsseltresore. Zudem werden Authentifizierung und Autorisierung erläutert, und es wird beschrieben, wie Sie den Zugriff schützen können.

## <a name="overview"></a>Übersicht

Der Zugriff auf eine Key Vault-Instanz wird über zwei separate Schnittstellen gesteuert: die Verwaltungsebene und die Datenebene. 
Über die **Verwaltungsebene** wird der Tresor verwaltet, indem er beispielsweise erstellt, aktualisiert oder gelöscht wird. 
Über die **Datenebene** werden die Geheimnisse in einem Tresor verwaltet, indem sie beispielsweise in einem Tresor erstellt, aktualisiert, gelöscht oder gelesen werden. Auf beiden Ebenen kann ein Aufrufer (ein Benutzer oder eine Anwendung) erst nach ordnungsgemäßer Authentifizierung und Autorisierung auf den Schlüsseltresor zugreifen. Bei der Authentifizierung wird die Identität des Aufrufers ermittelt. Bei der Autorisierung wird dagegen bestimmt, welche Vorgänge der Aufrufer ausführen darf.

Für die Authentifizierung wird sowohl auf der Verwaltungs- als auch auf der Datenebene Azure Active Directory verwendet. Für die Autorisierung wird auf der Verwaltungsebene die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) verwendet, während auf der Datenebene Key Vault-Zugriffsrichtlinien zum Einsatz kommen.

Im Anschluss finden Sie eine kurze Übersicht über die behandelten Themen:

Authentifizierung über Azure Active Directory: In diesem Abschnitt erfahren Sie, wie sich ein Aufrufer bei Azure Active Directory authentifiziert, um über die Verwaltungs- oder Datenebene auf eine Schlüsseltresorinstanz zuzugreifen. 

Für die Authentifizierung verwenden beide Ebenen Azure Active Directory (Azure AD). Für die Autorisierung wird auf der Verwaltungsebene die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) verwendet, während auf der Datenebene Key Vault-Zugriffsrichtlinien zum Einsatz kommen.

## <a name="authenticate-by-using-azure-active-directory"></a>Authentifizieren mit Azure Active Directory

Wenn Sie in einem Azure-Abonnement einen Schlüsseltresor erstellen, wird dieser automatisch mit dem Azure AD-Mandanten des Abonnements verknüpft. Alle Aufrufer müssen bei diesem Mandanten registriert sein und sich authentifizieren, um auf den Schlüsseltresor zugreifen zu können. Diese Anforderung gilt sowohl auf der Verwaltungs- als auch auf der Datenebene. In beiden Fällen kann eine Anwendung auf zwei Arten auf Key Vault zugreifen:

* **Benutzer- und App-Zugriff:** Diese Methode wird für Anwendungen verwendet, die im Auftrag eines angemeldeten Benutzers auf Key Vault zugreifen. Beispiele hierfür wären etwa Azure PowerShell und das Azure-Portal. Es gibt zwei Möglichkeiten, Benutzern Zugriff zu gewähren:

  - Zugriff auf Key Vault über eine Anwendung.
  - Zugriff auf Key Vault nur dann gewähren, wenn eine bestimmte Anwendung verwendet wird (als Verbundidentität bezeichnet).

* **Nur App-Zugriff:** Diese Methode ist für Anwendungen vorgesehen, die als Daemondienste oder Hintergrundaufträge ausgeführt werden. Die Identität der Anwendung erhält Zugriff auf die Key Vault-Instanz.

Bei beiden Anwendungstypen authentifiziert sich die Anwendung über Azure AD mit einer der [unterstützten Authentifizierungsmethoden](../active-directory/develop/authentication-scenarios.md) und bezieht ein Token. Welche Authentifizierungsmethode verwendet wird, hängt von der Art der Anwendung ab. Anschließend sendet die Anwendung unter Verwendung des Tokens eine REST-API-Anforderung an Key Vault. Anforderungen auf Verwaltungsebene werden über einen Azure Resource Manager-Endpunkt weitergeleitet. Beim Zugriff auf die Datenebene kommunizieren die Anwendungen hingegen direkt mit einem Key Vault-Endpunkt. Weitere Informationen finden Sie in der Übersicht über den [gesamten Authentifizierungsworkflow](../active-directory/develop/v1-protocols-oauth-code.md). 

Der Ressourcenname, für den die Anwendung ein Token anfordert, hängt davon ab, auf welche Ebene die Anwendung zugreift. Beim Ressourcennamen handelt es sich je nach Azure-Umgebung entweder um einen Verwaltungsebenenendpunkt oder um einen Datenebenenendpunkt. Weitere Details finden Sie in der Tabelle, die in diesem Artikel enthalten ist.

Das Verwenden eines einzelnen Mechanismus für die Authentifizierung für beide Ebenen hat folgende Vorteile:

* Organisationen können den Zugriff auf alle Key Vault-Instanzen steuern.
* Wenn ein Benutzer eine Organisation verlässt, verliert dieser umgehend den Zugriff auf sämtliche Key Vault-Instanzen in der Organisation.
* Organisationen können die Authentifizierung über die Optionen in Azure AD anpassen und so beispielsweise die mehrstufige Authentifizierung aktivieren, um die Sicherheit zu verbessern.

## <a name="the-management-plane-and-the-data-plane"></a>Verwaltungsebene und Datenebene

Verwenden Sie die Verwaltungsebene, um den Schlüsseltresor zu verwalten. Hierzu zählen Vorgänge wie das Verwalten von Attributen und Festlegen von Richtlinien für den Zugriff auf die Datenebene. Verwenden Sie die Datenebene, um die in der Key Vault-Instanz gespeicherten Schlüssel, Geheimnisse und Zertifikate hinzuzufügen, zu löschen oder zu ändern.

Der Zugriff auf die Schnittstellen der Verwaltungs- und der Datenebene erfolgt über die in der folgenden Tabelle aufgelisteten unterschiedlichen Endpunkte. Die zweite Spalte in der Tabelle gibt Aufschluss über die DNS-Namen für diese Endpunkte in verschiedenen Azure-Umgebungen. In der dritten Spalte werden die Vorgänge beschrieben, die Sie über die jeweilige Zugriffsebene ausführen können. Jede Zugriffsebene verfügt auch über einen eigenen Zugriffssteuerungsmechanismus. Die Zugriffssteuerung auf Verwaltungsebene wird mit der rollenbasierten Zugriffssteuerung (RBAC, Role-Based Access Control) von Azure Resource Manager festgelegt. Die Zugriffssteuerung auf Datenebene wird mithilfe der Richtlinie für den Zugriff auf Key Vault festgelegt.

| Zugriffsebene | Zugriffsendpunkte | Vorgänge | Zugriffssteuerungsmechanismus |
| --- | --- | --- | --- |
| Verwaltungsebene |**Global:**<br> management.azure.com:443<br><br> **Azure China 21Vianet:**<br> management.chinacloudapi.cn:443<br><br> **Azure US Government:**<br> management.usgovcloudapi.net:443<br><br> **Azure Deutschland:**<br>  management.microsoftazure.de:443 |Erstellen, Lesen, Aktualisieren oder Löschen von Key Vault <br> Festlegen von Zugriffsrichtlinien für Key Vault<br>Festlegen von Tags für Key Vault |Rollenbasierte Zugriffssteuerung für Azure Resource Manager |
| Datenebene |**Global:**<br> &lt;Tresorname&gt;.vault.azure.net:443<br><br> **Azure China 21Vianet:**<br> &lt;Tresorname&gt;.vault.azure.cn:443<br><br> **Azure US Government:**<br> &lt;Tresorname&gt;.vault.usgovcloudapi.net:443<br><br> **Azure Deutschland:**<br> &lt;Tresorname&gt;.vault.microsoftazure.de:443 |Für Schlüssel: Decrypt, Encrypt, UnwrapKey, WrapKey, Verify, Sign, Get, List, Update, Create, Import, Delete, Backup, Restore<br><br> Für Geheimnisse: Get, List, Set, Delete |Key Vault-Zugriffsrichtlinie |

Die Zugriffsteuerungen für die Verwaltungs- und die Datenebene sind voneinander unabhängig. Wenn Sie z.B. einer Anwendung den Zugriff auf Schlüssel in einem Schlüsseltresor gewähren möchten, müssen Sie nur Zugriff auf die Datenebene gewähren. Der Zugriff wird über Zugriffsrichtlinien für Key Vault gewährt. Im Gegensatz dazu benötigt ein Benutzer, der Key Vault-Eigenschaften und -Tags lesen muss, jedoch keine Zugriffsdaten (Schlüssel, Geheimnisse oder Zertifikate), sondern nur Zugriff auf die Steuerungsebene. Sie können den Zugriff gewähren, indem Sie dem Benutzer mit der rollenbasierten Zugriffssteuerung Lesezugriff zuweisen.

## <a name="management-plane-access-control"></a>Zugriffssteuerung auf der Verwaltungsebene

Auf der Verwaltungsebene werden Vorgänge ausgeführt, die sich auf den Schlüsseltresor selbst auswirken, wie etwa:

- Erstellen oder Löschen eines Schlüsseltresors.
- Abrufen einer Liste von Tresoren in einem Abonnement.
- Abrufen von Key Vault-Eigenschaften (z. B. SKU und Tags).
- Festlegen von Key Vault-Zugriffsrichtlinien, die den Zugriff von Benutzern und Anwendungen auf Schlüssel und Geheimnisse steuern.

Die Zugriffssteuerung auf der Verwaltungsebene basiert auf RBAC.  

### <a name="role-based-access-control-rbac"></a>Rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)

Jedes Azure-Abonnement hat eine Instanz von Azure AD. Sie können Benutzern, Gruppen und Anwendungen aus diesem Verzeichnis Verwaltungszugriff auf Ressourcen im Azure-Abonnement gewähren, für die das Azure Resource Manager-Bereitstellungsmodell verwendet wird. Diese Art von Zugriffssteuerung wird als rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) bezeichnet. Zur Verwaltung dieses Zugriffs können Sie das [Azure-Portal](https://portal.azure.com/), die [Tools der Azure-Befehlszeilenschnittstelle](../cli-install-nodejs.md), [PowerShell](/powershell/azureps-cmdlets-docs) oder die [Azure Resource Manager-REST-APIs](https://msdn.microsoft.com/library/azure/dn906885.aspx) verwenden.

Mit Azure AD erstellen Sie einen Schlüsseltresor in einer Ressourcengruppe und steuern den Zugriff auf die Verwaltungsebene. So können Sie beispielsweise Benutzern oder einer Gruppe die Verwaltung von Schlüsseltresoren in einer Ressourcengruppe ermöglichen.

Durch Zuweisen geeigneter RBAC-Rollen können Sie Benutzern, Gruppen und Anwendungen in einem bestimmten Bereich Zugriff gewähren. Wenn Sie also etwa einem Benutzer Zugriff für die Verwaltung von Schlüsseltresoren gewähren möchten, weisen Sie ihm für einen bestimmten Bereich die vordefinierte Rolle „Key Vault-Mitwirkender“ zu. Der Bereich wäre in diesem Fall entweder ein Abonnement, eine Ressourcengruppe oder ein bestimmter Schlüsseltresor. Eine auf Abonnementebene zugewiesene Rolle gilt für alle Ressourcengruppen und Ressourcen innerhalb des Abonnements. Eine auf Ressourcengruppenebene zugewiesene Rolle gilt für alle Ressourcen in der Ressourcengruppe. Eine für eine bestimmte Ressource zugewiesene Rolle gilt für diese Ressource. Es gibt mehrere vordefinierte Rollen (siehe [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../role-based-access-control/built-in-roles.md)). Wenn eine vordefinierte Rolle nicht Ihren Anforderungen entspricht, können Sie Ihre eigene Rolle definieren.

> [!IMPORTANT]
> Hinweis: Falls ein Benutzer für eine Key Vault-Verwaltungsebene über Mitwirkungsberechtigungen verfügt, kann dieser sich durch das Festlegen einer Key Vault-Zugriffsrichtlinie selbst Zugriff auf die Datenebene gewähren. Deshalb sollten sie stets kontrollieren, wer als Mitwirkender auf Ihre Schlüsseltresore zugreifen kann. Stellen Sie sicher, dass nur autorisierte Personen auf Ihre Schlüsseltresore, Schlüssel, Geheimnisse und Zertifikate zugreifen und diese verwalten können.
>

## <a name="data-plane-access-control"></a>Zugriffssteuerung auf der Datenebene

Vorgänge in der Key Vault-Datenebene gelten für gespeicherte Objekte wie Schlüssel, Geheimnisse und Zertifikate. Zu wichtigen Vorgängen zählen das Erstellen, Importieren, Aktualisieren, Auflisten, Sichern und Wiederherstellen von Schlüsseln. Zu kryptografischen Vorgängen zählen das Signieren, Überprüfen, Verschlüsseln, Entschlüsseln, Umschließen, Entpacken, Festlegen von Tags und das Festlegen anderer Attribute für Schlüssel. Auf ähnliche Weise zählen Abrufen, Festlegen, Auflisten und Löschen zu den Vorgängen für Geheimnisse.

Der Datenebenenzugriff wird durch das Festlegen von Zugriffsrichtlinien für eine Key Vault-Instanz gewährt. Ein Benutzer, eine Gruppe oder eine Anwendung muss über Mitwirkungsberechtigungen für die Verwaltungsebene einer Key Vault-Instanz verfügen, um Zugriffsrichtlinien für diese Instanz festlegen zu können. Sie können einem Benutzer, einer Gruppe oder einer Anwendung Zugriff auf das Ausführen bestimmter Vorgänge für Schlüssel oder Geheimnisse in einer Key Vault-Instanz gewähren. Key Vault unterstützt bis zu 1024 Zugriffsrichtlinien. Erstellen Sie eine Azure AD-Sicherheitsgruppe, und fügen Sie ihr Benutzer hinzu, um mehreren Benutzern Datenebenenzugriff zu gewähren.

### <a name="key-vault-access-policies"></a>Key Vault-Zugriffsrichtlinien

Mit Key Vault-Zugriffsrichtlinien können separate Berechtigungen für Schlüssel, Geheimnisse und Zertifikate gewährt werden. Dadurch haben Sie beispielsweise die Möglichkeit, einem Benutzer nur Zugriff auf Schlüssel und keine Berechtigungen für Geheimnisse zu gewähren. Zugriffsberechtigungen für Schlüssel, Geheimnisse oder Zertifikate gelten auf Tresorebene. Die Key Vault-Zugriffsrichtlinie unterstützt keine differenzierten Berechtigungen auf Objektebene, wie z. B. für bestimmte Schlüssel, Geheimnisse oder Zertifikate. Zum Festlegen von Zugriffsrichtlinien für eine Key Vault-Instanz können Sie das [Azure-Portal](https://portal.azure.com/), die [Azure CLI-Tools](../cli-install-nodejs.md), [PowerShell](/powershell/azureps-cmdlets-docs) oder die [REST-APIs für die Key Vault-Verwaltung](https://msdn.microsoft.com/library/azure/mt620024.aspx) verwenden.

> [!IMPORTANT]
> Key Vault-Zugriffsrichtlinien gelten auf Tresorebene. Wenn einem Benutzer also beispielsweise die Berechtigung zum Erstellen und Löschen von Schlüsseln gewährt wird, kann er diese Vorgänge für alle Schlüssel in dieser Key Vault-Instanz ausführen.

Sie können den Datenebenenzugriff nicht nur über Zugriffsrichtlinien, sondern auch über [Dienstendpunkte virtueller Netzwerke für den Azure Key Vault](key-vault-overview-vnet-service-endpoints.md) einschränken. Konfigurieren Sie [Firewallregeln und Regeln für virtuelle Netzwerke](key-vault-network-security.md), um eine zusätzliche Sicherheitsebene zu erstellen.

## <a name="example"></a>Beispiel

Angenommen, Sie entwickeln eine Anwendung, die ein Zertifikat für SSL, Azure Storage zum Speichern von Daten und einen RSA-Schlüssel mit 2.048 Bit für Signierungsvorgänge verwendet. Nehmen Sie weiterhin an, dass diese Anwendung in einer Azure-VM (oder einer VM-Skalierungsgruppe) ausgeführt wird. In diesem Fall können Sie alle Anwendungsgeheimnisse sowie das Bootstrapzertifikat, mit dem sich die Anwendung über Azure AD authentifiziert, in einem Schlüsseltresor speichern.

Im Anschluss finden Sie eine Zusammenfassung aller Typen von Schlüsseln und Geheimnissen, die gespeichert werden:

* **SSL-Zertifikate:** Werden für SSL verwendet.
* **Speicherschlüssel:** Werden verwendet, um Zugriff auf das Speicherkonto zu erhalten.
* **RSA-Schlüssel mit 2048 Bit:** Werden für Signierungsvorgänge verwendet.
* **Bootstrapzertifikate:** Werden für die Authentifizierung über Azure AD verwendet. Sobald der Zugriff gewährt wird, können Sie den Speicherschlüssel abrufen und den RSA-Schlüssel zum Signieren verwenden.

Kommen wir nun zu den Benutzern, die diese Anwendung verwalten, bereitstellen und überprüfen. In diesem Beispiel werden drei Rollen verwendet:

* **Sicherheitsteam:** In der Regel IT-Mitarbeiter aus dem Büro des CSO (Chief Security Officer, leitender Sicherheitsbeauftragter) oder Mitarbeiter in vergleichbarer Position. Dieses Team ist für die sichere Verwahrung von Geheimnissen verantwortlich. Zu den Beispielen zählen SSL-Zertifikate, RSA-Schlüssel für Signaturen, Verbindungszeichenfolgen und Speicherkontoschlüssel.
* **Entwickler/Betreiber:** Mitarbeiter, die die Anwendung entwickeln und anschließend in Azure bereitstellen. In der Regel sind sie nicht Teil des Sicherheitsteams und sollten darum keinen Zugriff auf vertrauliche Daten wie SSL-Zertifikate und RSA-Schlüssel haben. Nur die Anwendung, die sie bereitstellen, sollte auf diese Objekte zugreifen können.
* **Prüfer:** In der Regel eine Personengruppe, die sich nicht mit den Entwicklern oder IT-Mitarbeitern überschneidet. Diese Gruppe prüft die Verwendung und Verwaltung von Zertifikaten, Schlüsseln sowie Geheimnissen und achtet auf die Einhaltung der Datensicherheitsstandards. 

Es gibt noch eine Rolle, die außerhalb des Bereichs dieser Anwendung liegt, aber hier erwähnt werden muss. Diese Rolle ist der Abonnementadministrator (oder Ressourcengruppenadministrator). Der Abonnementadministrator richtet die anfänglichen Zugriffsberechtigungen für das Sicherheitsteam ein. Der Abonnementadministrator gewährt dem Sicherheitsteam mithilfe einer Ressourcengruppe, die die von dieser Anwendung benötigten Ressourcen enthält, den erforderlichen Zugriff.

Als Nächstes sehen wir uns an, welche Aktionen die einzelnen Rollen im Kontext dieser Anwendung ausführen können.

* **Sicherheitsteam**
  * Erstellt Schlüsseltresore.
  * Aktiviert die Key Vault-Protokollierung.
  * Fügt Schlüssel und Geheimnisse hinzu.
  * Erstellt Schlüsselsicherungen für die Notfallwiederherstellung.
  * Legt Key Vault-Zugriffsrichtlinien fest, um Benutzern und Anwendungen Zugriff zu gewähren, damit sie bestimmte Vorgänge ausführen können.
  * Ändert regelmäßig Schlüssel und Geheimnisse.
* **Entwickler/Betreiber**
  * Rufen Verweise auf Bootstrap- und SSL-Zertifikate (Fingerabdrücke), Speicherschlüssel (geheimer URI) und den Signaturschlüssel (Schlüssel-URI) vom Sicherheitsteam ab.
  * Entwickeln eine Anwendung, die programmgesteuert auf Schlüssel und Geheimnisse zugreift, und stellen diese Anwendung bereit.
* **Prüfer**
  * Überprüfen anhand von Verwendungsprotokollen die ordnungsgemäße Verwendung von Schlüsseln und Geheimnissen sowie die Einhaltung von Datensicherheitsstandards.

Als Nächstes betrachten wir, welche Zugriffsberechtigungen die einzelnen Rollen und die Anwendung für ihre jeweiligen Aufgaben benötigen. 

| Benutzerrolle | Berechtigungen auf Verwaltungsebene | Berechtigungen auf Datenebene |
| --- | --- | --- |
| Sicherheitsteam |Key Vault-Mitwirkender |Schlüssel: Sichern, Erstellen, Löschen, Abrufen, Importieren, Auflisten, Wiederherstellen <br> Geheimnisse: alle |
| Entwickler/Betreiber |Key Vault-Bereitstellungsberechtigung, damit die bereitgestellten virtuellen Computer Geheimnisse aus der Key Vault-Instanz abrufen können |Keine |
| Prüfer |Keine |Schlüssel: Auflisten<br>Geheimnisse: Auflisten |
| Anwendung |Keine |Schlüssel: Signieren<br>Geheimnisse: Abrufen |

> [!NOTE]
> Prüfer müssen über die Berechtigung zum Auflisten von Schlüsseln und Geheimnissen verfügen, damit sie Attribute für Schlüssel und Geheimnisse inspizieren können, die nicht in den Protokollen ausgegeben werden. Zu diesen Attributen zählen unter anderem Tags sowie Aktivierungs- und Ablauftermine.
> 
> 

Neben den Schlüsseltresorberechtigungen benötigen alle drei Rollen auch Zugriff auf andere Ressourcen. Damit Entwickler oder Betreiber beispielsweise VMs (oder das Web-Apps-Feature von Azure App Service) bereitstellen können, müssen diese als Mitwirkende auf diese Ressourcentypen zugreifen können. Prüfer benötigen Lesezugriff auf das Speicherkonto mit den gespeicherten Key Vault-Protokollen.

Da der Schwerpunkt dieses Artikels auf dem Schützen des Zugriffs auf Ihren Schlüsseltresor liegt, werden nur Konzepte zu diesem Thema vorgestellt. Details zur Bereitstellung von Zertifikaten und dem programmgesteuerten Zugriff auf Schlüssel und Geheimnisse werden an anderer Stelle behandelt. Beispiel:

- Das Bereitstellen von Zertifikaten im Schlüsseltresor für VMs wird in folgendem Blogbeitrag behandelt: [Updated: Deploy Certificates to VMs from customer-managed Key Vault (Aktualisiert: Bereitstellen von Zertifikaten auf virtuellen Computern über eine vom Kunden verwaltete Key Vault-Instanz)](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/).
- Anhand des [Downloads von Azure Key Vault-Clientbeispielen](https://www.microsoft.com/download/details.aspx?id=45343) wird veranschaulicht, wie ein Bootstrapzertifikat zur Authentifizierung bei Azure AD für den Zugriff auf einen Schlüsseltresor verwendet wird.

Sie können die meisten Zugriffsberechtigungen über das Azure-Portal erteilen. Wenn Sie präzise Berechtigungen gewähren möchten, müssen Sie unter Umständen Azure PowerShell oder die CLI verwenden. 

Den im Anschluss bereitgestellten PowerShell-Codeausschnitten liegen folgende Annahmen zugrunde:

* Der Azure AD-Administrator hat für die drei Rollen folgende Sicherheitsgruppen erstellt: Contoso Security Team, Contoso App Devops und Contoso App Auditors. Der Administrator hat den Gruppen entsprechende Benutzer hinzugefügt.
* Die Ressourcen befinden sich alle in der Ressourcengruppe **ContosoAppRG**. Die Protokolle werden in **contosologstorage** gespeichert. 
* Der Schlüsseltresor **ContosoKeyVault** und das für die Key Vault-Protokolle verwendete Speicherkonto **contosologstorage** müssen sich am gleichen Azure-Standort befinden.

Zunächst weist der Abonnementadministrator dem Sicherheitsteam die Rollen `key vault Contributor` und `User Access Administrator` zu. Mit diesen Rollen kann das Sicherheitsteam den Zugriff auf andere Ressourcen sowie Schlüsseltresore in der Ressourcengruppe „ContosoAppRG“ verwalten.

```PowerShell
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "key vault Contributor" -ResourceGroupName ContosoAppRG
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "User Access Administrator" -ResourceGroupName ContosoAppRG
```

Das folgende Skript zeigt, wie das Sicherheitsteam einen Schlüsseltresor erstellen und Protokollierung sowie Zugriffsberechtigungen einrichten kann. Weitere Informationen zur Richtlinie für Berechtigungen zum Zugriff auf Schlüsseltresore finden Sie unter [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten](about-keys-secrets-and-certificates.md).

```PowerShell
# Create key vault and enable logging
$sa = Get-AzureRmStorageAccount -ResourceGroup ContosoAppRG -Name contosologstorage
$kv = New-AzureRmKeyVault -Name ContosoKeyVault -ResourceGroup ContosoAppRG -SKU premium -Location 'westus' -EnabledForDeployment
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

# Data plane permissions for Security team
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -PermissionsToKeys backup,create,delete,get,import,list,restore -PermissionsToSecrets get,list,set,delete,backup,restore,recover,purge

# Management plane permissions for Dev/ops
# Create a new role from an existing role
$devopsrole = Get-AzureRmRoleDefinition -Name "Virtual Machine Contributor"
$devopsrole.Id = $null
$devopsrole.Name = "Contoso App Devops"
$devopsrole.Description = "Can deploy VMs that need secrets from key vault"
$devopsrole.AssignableScopes = @("/subscriptions/<SUBSCRIPTION-GUID>")

# Add permission for dev/ops so they can deploy VMs that have secrets deployed from key vaults
$devopsrole.Actions.Add("Microsoft.KeyVault/vaults/deploy/action")
New-AzureRmRoleDefinition -Role $devopsrole

# Assign this newly defined role to Dev ops security group
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Devops')[0].Id -RoleDefinitionName "Contoso App Devops" -ResourceGroupName ContosoAppRG

# Data plane permissions for Auditors
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Auditors')[0].Id -PermissionsToKeys list -PermissionsToSecrets list
```

Die benutzerdefinierte Rolle kann nur dem Abonnement zugewiesen werden, in dem die Ressourcengruppe `ContosoAppRG` erstellt wird. Falls die gleichen benutzerdefinierten Rollen für andere Projekte in anderen Abonnements verwendet werden, kann der Bereich um weitere Abonnements erweitert werden.

Die Zuweisung der benutzerdefinierten Rolle für die Berechtigung für Bereitstellungen und Aktionen für Entwickler und Betreiber ist auf die Ressourcengruppe beschränkt. So haben nur virtuelle Computer, die in der Ressourcengruppe `ContosoAppRG` erstellt wurden, Zugriff auf die Geheimnisse (SSL-Zertifikate und Bootstrapzertifikate). Virtuelle Computer, die in einer anderen Ressourcengruppe von einem Entwickler oder Betreiber erstellt wurden, haben auch dann keinen Zugriff auf diese Geheimnisse, wenn sie über die geheimen URIs verfügen.

Bei diesem Beispiel handelt es sich um ein einfaches Szenario. In der Praxis können die Szenarios komplexer ausfallen. Sie können die Berechtigungen jedoch an Ihren Schlüsseltresor anpassen. In diesem Beispiel wird beispielsweise davon ausgegangen, dass das Sicherheitsteam die Verweise auf Schlüssel und Geheimnisse (URIs und Fingerabdrücke) bereitstellt, auf die Entwickler und Betreiber in ihren Anwendungen verweisen müssen. Entwickler und Betreiber benötigen keinen Zugriff auf die Datenebene. In diesem Beispiel geht es in erster Linie um den Schutz Ihres Schlüsseltresors geht. Ähnliche Überlegungen sollten Sie für den Schutz Ihrer [virtuellen Computer](https://azure.microsoft.com/services/virtual-machines/security/), [Speicherkonten](../storage/common/storage-security-guide.md) und anderen Azure-Ressourcen anstellen.

> [!NOTE]
> In diesem Beispiel wird veranschaulicht, wie der Key Vault-Zugriff in der Produktion gesperrt wird. Die Entwickler müssen über ein eigenes Abonnement oder über eine eigene Ressourcengruppe mit uneingeschränkten Berechtigungen verfügen, um ihre Tresore, ihre virtuellen Computer und ihr Speicherkonto für die Anwendungsentwicklung verwalten zu können.

Sie sollten den Zugriff auf Ihren Schlüsseltresor weiterhin durch die [Konfiguration von Key Vault-Firewalls und virtuellen Netzwerken](key-vault-network-security.md) schützen.

## <a name="resources"></a>Ressourcen

* [Rollenbasierte Zugriffssteuerung in Azure Active Directory](../role-based-access-control/role-assignments-portal.md)
  
* [RBAC: Integrierte Rollen](../role-based-access-control/built-in-roles.md)
  
* [Grundlegendes zur Bereitstellung über den Ressourcen-Manager im Vergleich zur klassischen Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md)
  
* [Verwalten der rollenbasierten Zugriffssteuerung mit Azure PowerShell](../role-based-access-control/role-assignments-powershell.md)
  
* [Verwalten der rollenbasierten Zugriffssteuerung mit der REST-API](../role-based-access-control/role-assignments-rest.md)
  
* [Role-Based Access Control for Microsoft Azure from Ignite (Rollenbasierte Zugriffssteuerung für Microsoft Azure (Ignite 2015))](https://channel9.msdn.com/events/Ignite/2015/BRK2707)
  
  In diesem 2015 Microsoft Ignite-Konferenzvideo werden Zugriffsverwaltung und Berichtsfunktionen in Azure erläutert. Außerdem werden dort bewährte Methoden zum Schützen des Zugriffs auf Azure-Abonnements mit Azure AD vorgestellt.
* [Autorisieren des Zugriffs auf Webanwendungen mit OAuth 2.0 und Azure AD](../active-directory/develop/v1-protocols-oauth-code.md)
  
* [REST-APIs für die Key Vault-Verwaltung](https://msdn.microsoft.com/library/azure/mt620024.aspx)
  
  Dieses Dokument stellt die Referenz für die REST-APIs zur programmgesteuerten Key Vault-Verwaltung dar (einschließlich der Festlegung von Key Vault-Zugriffsrichtlinien).
* [Key Vault-REST-APIs](https://msdn.microsoft.com/library/azure/dn903609.aspx)
  
* [Schlüsselzugriffssteuerung](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_KeyAccessControl)
  
* [Geheimniszugriffssteuerung](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_SecretAccessControl)
  
* [Festlegen](https://docs.microsoft.com/powershell/module/azurerm.keyvault/Set-AzureRmKeyVaultAccessPolicy) und [Entfernen](https://docs.microsoft.com/powershell/module/azurerm.keyvault/Remove-AzureRmKeyVaultAccessPolicy) von Key Vault-Zugriffsrichtlinien mithilfe von PowerShell
  
## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren von Key Vault-Firewalls und virtuellen Netzwerken](key-vault-network-security.md)

Ein Lernprogramm für den Einstieg für Administratoren finden Sie unter [Erste Schritte mit dem Azure-Schlüsseltresor](key-vault-get-started.md).

Weitere Informationen zur Verwendungsprotokollierung für Key Vault finden Sie unter [Azure-Schlüsseltresor-Protokollierung](key-vault-logging.md).

Weitere Informationen zur Verwendung von Schlüsseln und Geheimnissen mit dem Azure Key Vault finden Sie unter [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten](https://msdn.microsoft.com/library/azure/dn903623.aspx).

Sollten Sie Fragen zu Key Vault haben, besuchen Sie die [Foren](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).

