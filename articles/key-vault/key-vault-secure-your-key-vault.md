---
title: Schützen einer Azure Key Vault-Instanz | Microsoft-Dokumentation
description: Verwalten Sie Zugriffsberechtigungen für Schlüsseltresore zur Verwaltung von Azure Key Vault, Schlüsseln und Geheimnissen. Authentifizierungs- und Autorisierungsmodell für Key Vault und Schützen eines Schlüsseltresors.
services: key-vault
documentationcenter: ''
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: e5b4e083-4a39-4410-8e3a-2832ad6db405
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/09/2018
ms.author: ambapat
ms.openlocfilehash: 4a1de3c011f1f8cfa1ea9246efad4ebb7f9e3a76
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49364522"
---
# <a name="secure-your-key-vault"></a>Schützen einer Key Vault-Instanz
Der Azure Key Vault-Clouddienst schützt Verschlüsselungsschlüssel und Geheimnisse (wie Zertifikate, Verbindungszeichenfolgen und Kennwörter). Da es sich hierbei um vertrauliche und geschäftskritische Daten handelt, muss der Zugriff auf Schlüsseltresore geschützt werden, sodass nur autorisierte Anwendungen und Benutzer Zugriff erhalten. 

Dieser Artikel enthält eine Übersicht des Modells für den Zugriff auf Schlüsseltresore. Er erläutert Authentifizierung und Autorisierung und beschreibt, wie Sie den Zugriff auf Schlüsseltresore schützen.

## <a name="overview"></a>Übersicht
Der Zugriff auf eine Key Vault-Instanz wird über zwei separate Schnittstellen gesteuert: die Verwaltungsebene und die Datenebene. Auf beiden Ebenen kann ein Aufrufer (ein Benutzer oder eine Anwendung) erst nach ordnungsgemäßer Authentifizierung und Autorisierung auf den Schlüsseltresor zugreifen. Bei der Authentifizierung wird die Identität des Aufrufers ermittelt. Bei der Autorisierung wird dagegen bestimmt, welche Vorgänge der Aufrufer ausführen darf.

Für die Authentifizierung wird sowohl auf der Verwaltungs- als auch auf der Datenebene Azure Active Directory verwendet. Für die Autorisierung wird auf der Verwaltungsebene die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) verwendet, während auf der Datenebene Key Vault-Zugriffsrichtlinien zum Einsatz kommen.

Im Anschluss finden Sie eine kurze Übersicht über die behandelten Themen:

[Authentifizierung über Azure Active Directory](#authentication-using-azure-active-directory): In diesem Abschnitt erfahren Sie, wie sich ein Aufrufer mit Azure Active Directory authentifiziert, um über die Verwaltungs- oder Datenebene auf eine Key Vault-Instanz zuzugreifen. 

[Verwaltungsebene und Datenebene](#management-plane-and-data-plane): Hierbei handelt es sich um zwei Zugriffsebenen für den Zugriff auf Ihre Key Vault-Instanz. Jede Zugriffsebene unterstützt bestimmte Vorgänge. In diesem Abschnitt werden die Zugriffsendpunkte, die unterstützten Vorgänge und die Zugriffssteuerungsmethoden der jeweiligen Ebene beschrieben. 

[Zugriffssteuerung auf der Verwaltungsebene](#management-plane-access-control): Dieser Abschnitt beschäftigt sich mit der Gewährung des Zugriffs auf Verwaltungsebenenvorgänge mithilfe der rollenbasierten Zugriffssteuerung.

[Zugriffssteuerung auf der Datenebene](#data-plane-access-control): In diesem Abschnitt erfahren Sie, wie Sie mithilfe von Key Vault-Zugriffsrichtlinien den Zugriff auf die Datenebene steuern.

[Beispiel](#example): Dieses Beispiel veranschaulicht, wie Sie die Zugriffssteuerung für Ihren Schlüsseltresor einrichten, um drei Teams (Sicherheitsteam, Entwickler/Operator und Prüfer) die Ausführung bestimmter Aufgaben für die Entwicklung, Verwaltung und Überwachung einer Anwendung in Azure zu ermöglichen.

## <a name="authentication-using-azure-active-directory"></a>Authentifizierung über Azure Active Directory
Wenn Sie unter einem Azure-Abonnement einen Schlüsseltresor erstellen, wird er automatisch mit dem Azure Active Directory-Mandanten des Abonnements verknüpft. Alle Aufrufer (Benutzer und Anwendungen) müssen bei diesem Mandanten registriert sein und sich authentifizieren, um auf den Schlüsseltresor zugreifen zu können. Diese Anforderung gilt sowohl auf der Verwaltungs- als auch auf der Datenebene. In beiden Fällen kann eine Anwendung auf zwei Arten auf Key Vault zugreifen:

* **Benutzer- und App-Zugriff**: Diese Methode wird für Anwendungen verwendet, die im Auftrag eines angemeldeten Benutzers auf Key Vault zugreifen. Beispiele hierfür wären etwa Azure PowerShell und das Azure-Portal. Es gibt zwei Möglichkeiten, Benutzern Zugriff zu gewähren: 
- Benutzern derart Zugriff gewähren, dass sie von einer beliebigen Anwendung aus auf den Schlüsseltresor zugreifen können.
- Benutzern nur dann Zugriff auf den Schlüsseltresor gewähren, wenn sie eine bestimmte Anwendung verwenden (als Verbundidentität bezeichnet).

* **Nur App-Zugriff**: Diese Methode ist für Anwendungen vorgesehen, die als Daemondienste oder Hintergrundaufträge ausgeführt werden. Die Identität der Anwendung erhält Zugriff auf die Key Vault-Instanz.

Bei beiden Anwendungstypen authentifiziert sich die Anwendung über Azure Active Directory mit einer der [unterstützten Authentifizierungsmethoden](../active-directory/develop/authentication-scenarios.md) und bezieht ein Token. Welche Authentifizierungsmethode verwendet wird, hängt von der Art der Anwendung ab. Anschließend sendet die Anwendung unter Verwendung des Tokens eine REST-API-Anforderung an Key Vault. Anforderungen auf Verwaltungsebene werden über einen Azure Resource Manager-Endpunkt weitergeleitet. Beim Zugriff auf die Datenebene kommunizieren die Anwendungen hingegen direkt mit einem Key Vault-Endpunkt. Ausführlichere Informationen finden Sie in der [Gesamtdarstellung des Authentifizierungsablaufs](../active-directory/develop/v1-protocols-oauth-code.md). 

Der Ressourcenname, für den die Anwendung ein Token anfordert, hängt davon ab, ob die Anwendung auf die Verwaltungs- oder auf die Datenebene zugreift. Folglich ist der Ressourcenname je nach Azure-Umgebung entweder ein Verwaltungsebenen- oder ein Datenebenen-Endpunkt, wie in der Tabelle in einem späteren Abschnitt beschrieben.

Die Verwendung eines einzelnen Authentifizierungsmechanismus für die Verwaltungs- und die Datenebene hat einige Vorteile:

* Organisationen können den Zugriff auf alle ihre Key Vault-Instanzen steuern.
* Wenn ein Benutzer aus der Organisation ausscheidet, verliert er umgehend den Zugriff auf sämtliche Key Vault-Instanzen in der Organisation.
* Organisationen können die Authentifizierung über die Optionen in Azure Active Directory anpassen und so beispielsweise die mehrstufige Authentifizierung aktivieren, um die Sicherheit zu verbessern.

## <a name="management-plane-and-data-plane"></a>Verwaltungsebene und Datenebene
Azure Key Vault ist ein Azure-Dienst und im Rahmen des Azure Resource Manager-Bereitstellungsmodells verfügbar. Wenn Sie einen Schlüsseltresor erstellen, erhalten Sie einen virtuellen Container zum Speichern vertraulicher Objekte wie Schlüssel, Geheimnisse und Zertifikate. Bestimmte Vorgänge werden über die Verwaltungs- und Datenebene für einen Schlüsseltresor ausgeführt. Die Verwaltungsebene dient zum Verwalten des Schlüsseltresors selbst. Hierzu zählen Vorgänge wie das Verwalten von Attributen und Festlegen von Richtlinien für den Zugriff auf die Datenebene. Die Schnittstelle der Datenebene dient zum Hinzufügen, Löschen, Ändern und Verwenden der in der Key Vault-Instanz gespeicherten Schlüssel, Geheimnisse und Zertifikate.

Der Zugriff auf die Schnittstellen der Verwaltungs- und der Datenebene erfolgt über die unten aufgelisteten unterschiedlichen Endpunkte. Die zweite Spalte gibt Aufschluss über die DNS-Namen für diese Endpunkte in verschiedenen Azure-Umgebungen. In der dritten Spalte werden die Vorgänge beschrieben, die Sie über die jeweilige Zugriffsebene ausführen können. Jede Zugriffsebene verfügt auch über einen eigenen Zugriffssteuerungsmechanismus. Die Zugriffssteuerung auf Verwaltungsebene wird mit der rollenbasierten Zugriffssteuerung (RBAC, Role-Based Access Control) von Azure Resource Manager festgelegt. Die Zugriffssteuerung auf Datenebene wird mithilfe der Richtlinie für den Zugriff auf den Schlüsseltresor festgelegt.

| Zugriffsebene | Zugriffsendpunkte | Vorgänge | Zugriffssteuerungsmechanismus |
| --- | --- | --- | --- |
| Verwaltungsebene |**Global:**<br> management.azure.com:443<br><br> **Azure China 21Vianet:**<br> management.chinacloudapi.cn:443<br><br> **Azure US Government:**<br> management.usgovcloudapi.net:443<br><br> **Azure Deutschland:**<br> management.microsoftazure.de:443 |Erstellen/Lesen/Aktualisieren/Löschen von Key Vault <br> Festlegen von Zugriffsrichtlinien für Key Vault<br>Festlegen von Tags für Key Vault |Rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) im Rahmen von Azure Resource Manager |
| Datenebene |**Global:**<br> &lt;Tresorname&gt;.vault.azure.net:443<br><br> **Azure China 21Vianet:**<br> &lt;Tresorname&gt;.vault.azure.cn:443<br><br> **Azure US Government:**<br> &lt;Tresorname&gt;.vault.usgovcloudapi.net:443<br><br> **Azure Deutschland:**<br> &lt;Tresorname&gt;.vault.microsoftazure.de:443 |Für Schlüssel: decrypt, encrypt, unwrapKey, wrapKey, verify, sign, get, list, update, create, import, delete, backup, restore<br><br> Für Geheimnisse: get, list, set, delete |Key Vault-Zugriffsrichtlinie |

Die Zugriffsteuerungen für die Verwaltungs- und die Datenebene sind voneinander unabhängig. Wenn Sie z.B. einer Anwendung den Zugriff auf Schlüssel in einem Schlüsseltresor gewähren möchten, müssen Sie nur Zugriff auf die Datenebene gewähren. Zugriff wird über Zugriffsrichtlinien für den Schlüsseltresor gewährt. Im Gegensatz dazu benötigt ein Benutzer, der Tresoreigenschaften und Tags lesen muss, aber keine Zugriffsdaten (Schlüssel, Geheimnisse oder Zertifikate) nur Zugriff auf die Steuerungsebene. Durch Zuweisen des „Lesezugriffs“ für den Benutzer mithilfe von RBAC wird Zugriff gewährt.

## <a name="management-plane-access-control"></a>Zugriffssteuerung auf der Verwaltungsebene
Auf der Verwaltungsebene werden Vorgänge ausgeführt, die sich auf den Schlüsseltresor selbst auswirken, wie etwa:

- Erstellen oder Löschen eines Schlüsseltresors.
- Abrufen einer Liste von Tresoren in einem Abonnement.
- Abrufen von Schlüsseltresoreigenschaften (etwa SKU und Tags).
- Festlegen von Schlüsseltresor-Zugriffsrichtlinien, die den Zugriff von Benutzern und Anwendungen auf Schlüssel und Geheimnisse steuern.

Die Zugriffssteuerung auf der Verwaltungsebene basiert auf RBAC. Eine umfassende Liste mit den Schlüsseltresorvorgängen, die über die Verwaltungsebene ausgeführt werden können, finden Sie in der Tabelle im vorherigen Abschnitt. 

### <a name="role-based-access-control-rbac"></a>Rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)
Jedes Azure-Abonnement hat ein Azure Active Directory. Benutzern, Gruppen und Anwendungen aus diesem Verzeichnis kann Verwaltungszugriff auf Ressourcen im Azure-Abonnement gewährt werden, für die das Azure Resource Manager-Bereitstellungsmodell verwendet wird. Diese Art von Zugriffssteuerung wird als rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) bezeichnet. Zur Verwaltung dieses Zugriffs können Sie das [Azure-Portal](https://portal.azure.com/), die [Tools der Azure-Befehlszeilenschnittstelle](../cli-install-nodejs.md), [PowerShell](/powershell/azureps-cmdlets-docs) oder die [Azure Resource Manager-REST-APIs](https://msdn.microsoft.com/library/azure/dn906885.aspx) verwenden.

Sie erstellen einen Schlüsseltresor in einer Ressourcengruppe und steuern den Zugriff auf die Verwaltungsebene mit Azure Active Directory. So können Sie beispielsweise Benutzern oder einer Gruppe die Verwaltung von Schlüsseltresoren in einer Ressourcengruppe ermöglichen.

Durch Zuweisen geeigneter RBAC-Rollen können Sie Benutzern, Gruppen und Anwendungen in einem bestimmten Bereich Zugriff gewähren. Wenn Sie also etwa einem Benutzer Zugriff für die Verwaltung von Schlüsseltresoren gewähren möchten, weisen Sie ihm für einen bestimmten Bereich die vordefinierte Rolle „Key Vault-Mitwirkender“ zu. Der Bereich wäre in diesem Fall entweder ein Abonnement, eine Ressourcengruppe oder ein bestimmter Schlüsseltresor. Eine auf Abonnementebene zugewiesene Rolle gilt für alle Ressourcengruppen und Ressourcen innerhalb des Abonnements. Eine auf Ressourcengruppenebene zugewiesene Rolle gilt für alle Ressourcen in der Ressourcengruppe. Eine für eine bestimmte Ressource zugewiesene Rolle gilt für diese Ressource. Es gibt mehrere vordefinierte Rollen (siehe [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../role-based-access-control/built-in-roles.md)). Wenn eine vordefinierte Rolle nicht Ihren Anforderungen entspricht, können Sie Ihre eigene Rolle definieren.

> [!IMPORTANT]
> Hinweis: Falls ein Benutzer für eine Key Vault-Verwaltungsebene über Mitwirkungsberechtigungen (RBAC) verfügt, kann er sich durch Festlegen einer Key Vault-Zugriffsrichtlinie (steuert den Zugriff auf den Datenebene) selbst Zugriff auf die Datenebene gewähren. Es empfiehlt sich daher, sehr genau darauf zu achten, wer als Mitwirkender Zugriff auf Ihre Key Vault-Instanzen hat, um sicherzustellen, dass nur autorisierte Benutzer auf Ihre Key Vault-Instanzen, Schlüssel, Geheimnisse und Zertifikate zugreifen und diese verwalten können.
> 
> 

## <a name="data-plane-access-control"></a>Zugriffssteuerung auf der Datenebene
Vorgänge in der Schlüsseltresor-Datenebene gelten für gespeicherte Objekte wie Schlüssel, Geheimnisse und Zertifikate. Zu wichtigen Vorgängen zählen das Erstellen, Importieren, Aktualisieren, Auflisten, Sichern und Wiederherstellen von Schlüsseln. Zu kryptografischen Vorgängen zählen Signieren, Überprüfen, Verschlüsseln, Entschlüsseln, Umschließen, Entpacken, Festlegen von Tags und andere Attribute für Schlüssel. Auf ähnliche Weise zählen Abrufen, Festlegen, Auflisten und Löschen zu den Vorgängen für Geheimnisse.

Der Datenebenenzugriff wird durch Festlegen von Zugriffsrichtlinien für eine Key Vault-Instanz gewährt. Ein Benutzer, eine Gruppe oder eine Anwendung muss über Mitwirkungsberechtigungen (RBAC) für die Verwaltungsebene einer Key Vault-Instanz verfügen, um Zugriffsrichtlinien für diese Instanz festlegen zu können. Einem Benutzer, einer Gruppe oder einer Anwendung kann Zugriff zum Ausführen bestimmter Vorgänge für Schlüssel oder Geheimnisse in einer Key Vault-Instanz gewährt werden. Pro Schlüsseltresor werden bis zu 1024 Zugriffsrichtlinien unterstützt. Erstellen Sie eine Azure Active Directory-Sicherheitsgruppe, und fügen Sie ihr Benutzer hinzu, um mehreren Benutzern Datenebenenzugriff auf eine Key Vault-Instanz zu gewähren.

### <a name="key-vault-access-policies"></a>Key Vault-Zugriffsrichtlinien
Mit Key Vault-Zugriffsrichtlinien können separate Berechtigungen für Schlüssel, Geheimnisse und Zertifikate gewährt werden. Dadurch haben Sie beispielsweise die Möglichkeit, einem Benutzer nur Zugriff auf Schlüssel und keine Berechtigungen für Geheimnisse zu gewähren. Zugriffsberechtigungen für Schlüssel, Geheimnisse oder Zertifikate gelten auf Tresorebene. Die Schlüsseltresor-Zugriffsrichtlinie unterstützt keine differenzierten Berechtigungen auf Objektebene, wie z.B. für bestimmte Schlüssel/Geheimnisse/Zertifikate. Zum Festlegen von Zugriffsrichtlinien für eine Key Vault-Instanz können Sie das [Azure-Portal](https://portal.azure.com/), die [Tools der Azure-Befehlszeilenschnittstelle](../cli-install-nodejs.md), [PowerShell](/powershell/azureps-cmdlets-docs) oder die [REST-APIs für die Key Vault-Verwaltung](https://msdn.microsoft.com/library/azure/mt620024.aspx) verwenden.

> [!IMPORTANT]
> Beachten Sie, dass Key Vault-Zugriffsrichtlinien auf Tresorebene gelten. Wenn einem Benutzer also beispielsweise die Berechtigung zum Erstellen und Löschen von Schlüsseln gewährt wird, kann er diese Vorgänge für alle Schlüssel in dieser Key Vault-Instanz ausführen.

Der Datenebenenzugriff kann nicht nur mit Zugriffsrichtlinien, sondern auch mithilfe von [Dienstendpunkten virtueller Netzwerke für Azure Key Vault](key-vault-overview-vnet-service-endpoints.md) eingeschränkt werden. Konfigurieren Sie dazu als zusätzliche Schutzebene [Firewalls und Regeln für virtuelle Netzwerke](key-vault-network-security.md).

## <a name="example"></a>Beispiel
Angenommen, Sie entwickeln eine Anwendung, die ein Zertifikat für SSL, Azure Storage zum Speichern von Daten und einen RSA-Schlüssel mit 2.048 Bit für Signierungsvorgänge verwendet. Nehmen wir an, dass diese Anwendung in einer Azure-VM (oder einer VM-Skalierungsgruppe) ausgeführt wird. In diesem Fall können Sie alle Anwendungsgeheimnisse sowie das Bootstrapzertifikat, mit dem sich die Anwendung über Azure Active Directory authentifiziert, in einem Schlüsseltresor speichern.

Im Anschluss finden Sie eine Zusammenfassung aller Typen von Schlüsseln und Geheimnissen, die in einem Schlüsseltresor gespeichert werden:

* **SSL-Zertifikat**: Wird für SSL verwendet.
* **Speicherschlüssel**: Wird verwendet, um Zugriff auf das Speicherkonto zu erhalten.
* **RSA-Schlüssel (2.048 Bit)**: Wird für Signierungsvorgänge verwendet.
* **Bootstrapzertifikat** : Wird zur Authentifizierung mit Azure Active Directory verwendet. Sobald der Zugriff gewährt wird, können Sie den Speicherschlüssel abrufen und den RSA-Schlüssel zum Signieren verwenden.

Kommen wir nun zu den Benutzern, die diese Anwendung verwalten, bereitstellen und überprüfen. In diesem Beispiel werden drei Rollen verwendet:

* **Sicherheitsteam**: In der Regel IT-Mitarbeiter aus dem „Büro des CSO (Chief Security Officer)“ oder ähnlich. Dieses Team ist für die sichere Verwahrung von Geheimnissen verantwortlich. Beispiel: SSL-Zertifikate, RSA-Schlüssel für Signaturen, Verbindungszeichenfolgen und Speicherkontoschlüssel.
* **Entwickler/Operator:** Mitarbeiter, die die Anwendung entwickeln und anschließend in Azure bereitstellen. In der Regel sind sie nicht Teil des Sicherheitsteams und sollten darum keinen Zugriff auf vertrauliche Daten wie SSL-Zertifikate und RSA-Schlüssel haben. Nur die Anwendung, die sie bereitstellen, sollte auf diese Objekte zugreifen können.
* **Prüfer:** In der Regel eine weitere Personengruppe, die sich nicht mit den Entwicklern oder IT-Mitarbeitern im Allgemeinen überschneidet. Diese Gruppe prüft die Verwendung und Verwaltung von Zertifikaten, Schlüsseln sowie Geheimnissen und achtet auf die Einhaltung der Datensicherheitsstandards. 

Es gibt noch eine Rolle, die außerhalb des Bereichs dieser Anwendung liegt, aber hier erwähnt werden muss. Diese Rolle ist der Abonnementadministrator (oder Ressourcengruppenadministrator). Der Abonnementadministrator richtet die anfänglichen Zugriffsberechtigungen für das Sicherheitsteam ein. Der Abonnementadministrator gewährt mithilfe einer Ressourcengruppe, die die von dieser Anwendung benötigten Ressourcen enthält, Zugriff auf das Sicherheitsteam.

Als Nächstes sehen wir uns an, welche Aktionen die einzelnen Rollen im Kontext dieser Anwendung ausführen können.

* **Sicherheitsteam**
  * Erstellt Key Vault-Instanzen
  * Aktiviert die Key Vault-Protokollierung
  * Fügt Schlüssel/Geheimnisse hinzu
  * Erstellt Schlüsselsicherungen für die Notfallwiederherstellung
  * Legt Key Vault-Zugriffsrichtlinien fest, um Benutzern und Anwendungen Zugriff zu gewähren, damit sie bestimmte Vorgänge ausführen können
  * Ändert regelmäßig Schlüssel/Geheimnisse
* **Entwickler/Betreiber**
  * Rufen Verweise auf Bootstrap- und SSL-Zertifikate (Fingerabdrücke), Speicherschlüssel (geheimer URI) und den Signaturschlüssel (Schlüssel-URI) vom Sicherheitsteam ab
  * Entwickeln eine Anwendung, die programmgesteuert auf Schlüssel und Geheimnisse zugreift, und stellen diese Anwendung bereit
* **Prüfer**
  * Überprüfen anhand von Verwendungsprotokollen die ordnungsgemäße Verwendung von Schlüsseln/Geheimnissen sowie die Einhaltung von Datensicherheitsstandards

Als Nächstes betrachten wir, welche Zugriffsberechtigungen die einzelnen Rollen und die Anwendung für ihre jeweiligen Aufgaben benötigen. 

| Benutzerrolle | Berechtigungen auf Verwaltungsebene | Berechtigungen auf Datenebene |
| --- | --- | --- |
| Sicherheitsteam |Key Vault-Mitwirkender |Schlüssel: Sichern, Erstellen, Löschen, Abrufen, Importieren, Auflisten, Wiederherstellen <br> Geheimnisse: alle |
| Entwickler/Betreiber |Key Vault-Bereitstellungsberechtigung, damit die bereitgestellten virtuellen Computer Geheimnisse aus der Key Vault-Instanz abrufen können |Keine |
| Prüfer |Keine |Schlüssel: Auflisten<br>Geheimnisse: Auflisten |
| Anwendung |Keine |Schlüssel: Signieren<br>Geheimnisse: Abrufen |

> [!NOTE]
> Prüfer müssen über die Berechtigung zum Auflisten von Schlüsseln und Geheimnissen verfügen, damit sie Attribute für Schlüssel und Geheimnisse inspizieren können, die nicht in den Protokollen ausgegeben werden. Hierzu zählen etwa Tags sowie Aktivierungs- und Ablauftermine.
> 
> 

Neben den Schlüsseltresorberechtigungen benötigen alle drei Rollen auch Zugriff auf andere Ressourcen. Beispielsweise müssen sie zum Bereitstellen von virtuellen Computern (oder Web-Apps und Ähnlichem) berechtigt sein. Entwickler/Betreiber benötigen zudem Mitwirkungszugriff auf diese Ressourcentypen. Prüfer benötigen Lesezugriff auf das Speicherkonto mit den gespeicherten Schlüsseltresorprotokollen.

Da der Schwerpunkt dieses Artikels auf dem Schützen des Zugriffs auf Ihren Schlüsseltresor liegt, zeigen wir nur Konzepte zu diesem Thema auf. Details zu Bereitstellung von Zertifikaten, programmgesteuertem Zugriff auf Schlüssel und Geheimnisse sowie Sonstiges werden an anderer Stelle behandelt. Beispiel:

- Das Bereitstellen von Zertifikaten im Schlüsseltresor für VMs wird in einem Blogbeitrag behandelt: [Updated: Deploy Certificates to VMs from customer-managed Key Vault (Hochladen: Bereitstellen von Zertifikaten für VMs vom benutzerdefiniert verwalteten Schlüsseltresor aus)](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/).
- Der [Download von Azure Key Vault-Clientbeispielen](https://www.microsoft.com/download/details.aspx?id=45343) veranschaulicht, wie ein Bootstrapzertifikat zur Authentifizierung bei Azure AD zwecks Zugriff auf den Schlüsseltresor verwendet wird.

Die meisten Zugriffsberechtigungen können über das Azure-Portal erteilt werden. Um präzise Berechtigungen zu gewähren, müssen Sie unter Umständen Azure PowerShell oder CLI verwenden, um das gewünschte Ergebnis zu erzielen. 

Den im Anschluss bereitgestellten PowerShell-Codeausschnitten liegen folgende Annahmen zugrunde:

* Der Azure Active Directory-Administrator hat für die drei Rollen folgende Sicherheitsgruppen erstellt: „Contoso Security Team“, „Contoso App Devops“ und „Contoso App Auditors“. Der Administrator hat den Gruppen entsprechende Benutzer hinzugefügt.
* Die Ressourcen befinden sich alle in der Ressourcengruppe **ContosoAppRG**. Die Protokolle werden in **contosologstorage** gespeichert. 
* Die Key Vault-Instanz **ContosoKeyVault** und das für die Key Vault-Protokolle verwendete Speicherkonto **contosologstorage** müssen sich am gleichen Azure-Standort befinden.

Der Abonnementadministrator weist dem Sicherheitsteam zunächst die Rollen „key vault Contributor“ (Key Vault-Mitwirkender) und „User Access Administrator“ (Benutzerzugriffsadministrator) zu. Mit diesen Rollen kann das Sicherheitsteam den Zugriff auf andere Ressourcen sowie Schlüsseltresore in der Ressourcengruppe ContosoAppRG verwalten.

```
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "key vault Contributor" -ResourceGroupName ContosoAppRG
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "User Access Administrator" -ResourceGroupName ContosoAppRG
```

Das folgende Skript zeigt, wie das Sicherheitsteam einen Schlüsseltresor erstellen und Protokollierung sowie Zugriffsberechtigungen einrichten kann. Unter [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten](about-keys-secrets-and-certificates.md) finden Sie nähere Informationen zur Richtlinie für Berechtigungen zum Zugriff auf Schlüsseltresore.

```
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

Die benutzerdefinierte Rolle kann nur dem Abonnement zugewiesen werden, unter dem die Ressourcengruppe „ContosoAppRG“ erstellt wird. Falls die gleichen benutzerdefinierten Rollen für andere Projekte unter anderen Abonnements verwendet werden, kann der Bereich um weitere Abonnements erweitert werden.

Die Zuweisung der benutzerdefinierten Rolle für die Entwickler/Operator im Zusammenhang mit der Bereitstellungs-/Aktionsberechtigung ist auf die Ressourcengruppe beschränkt. So haben nur virtuelle Computer, die in der Ressourcengruppe ContosoAppRG erstellt wurden, Zugriff auf die Geheimnisse (SSL-Zertifikat und Bootstrapzertifikat). Virtuelle Computer, die in einer anderen Ressourcengruppe von einem Entwickler-/Operatorteam erstellt wurden, haben auch dann keinen Zugriff auf diese Geheimnisse, wenn sie über die geheimen URIs verfügen.

Bei diesem Beispiel handelt es sich um ein einfaches Szenario. In der Praxis können die Szenarien komplexer ausfallen und eine entsprechende Anpassung der Berechtigungen für Ihren Schlüsseltresor erfordern. In unserem Beispiel gehen wir etwa davon aus, dass das Sicherheitsteam die Verweise auf Schlüssel und Geheimnisse (URIs und Fingerabdrücke) bereitstellt, die das Entwickler-/Operatorteam in seinen Anwendungen benötigt. Entwickler/Operator benötigen keinen Zugriff auf Datenebene. In diesem Beispiel geht es in erster Linie um den Schutz Ihres Schlüsseltresors geht. Ähnliche Überlegungen sollten Sie für den Schutz Ihrer [virtuellen Computer](https://azure.microsoft.com/services/virtual-machines/security/), [Speicherkonten](../storage/common/storage-security-guide.md) und anderen Azure-Ressourcen anstellen.

> [!NOTE]
> Hinweis: Dieses Beispiel zeigt, wie der Key Vault-Zugriff in der Produktion gesperrt wird. Die Entwickler müssen über ein eigenes Abonnement oder über eine eigene Ressourcengruppe mit uneingeschränkten Berechtigungen verfügen, um ihre Tresore, ihre virtuellen Computer und ihr Speicherkonto für die Anwendungsentwicklung verwalten zu können.

Sie sollten den Zugriff auf Ihren Schlüsseltresor durch die [Konfiguration von Key Vault-Firewalls und virtuellen Netzwerken](key-vault-network-security.md) unbedingt noch weiter schützen.

## <a name="resources"></a>Ressourcen
* [Rollenbasierte Zugriffssteuerung in Azure Active Directory](../role-based-access-control/role-assignments-portal.md)
  
  Dieser Artikel beschreibt die rollenbasierte Steuerung des Zugriffs auf Azure Active Directory, und wie sie funktioniert.
* [RBAC: Integrierte Rollen](../role-based-access-control/built-in-roles.md)
  
  In diesem Artikel werden alle integrierten, in RBAC verfügbaren Rollen ausführlich beschrieben.
* [Grundlegendes zur Bereitstellung über den Ressourcen-Manager im Vergleich zur klassischen Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md)
  
  Dieser Artikel erläutert die Resource Manager-Bereitstellung sowie klassische Bereitstellungsmodelle und erläutert die Vorteile der Verwendung der Resource Manager- und Ressourcengruppen.
* [Verwalten der rollenbasierten Zugriffssteuerung mit Azure PowerShell](../role-based-access-control/role-assignments-powershell.md)
  
  In diesem Artikel erfahren Sie, wie Sie die rollenbasierte Zugriffssteuerung mit Azure PowerShell verwalten.
* [Verwalten der rollenbasierten Zugriffssteuerung mit der REST-API](../role-based-access-control/role-assignments-rest.md)
  
  Dieser Artikel beschreibt die Verwendung der REST-API zum Verwalten von RBAC.
* [Role-Based Access Control for Microsoft Azure from Ignite (Rollenbasierte Zugriffssteuerung für Microsoft Azure über Ignite)](https://channel9.msdn.com/events/Ignite/2015/BRK2707)
  
  In diesem 2015 Microsoft Ignite-Konferenzvideo werden Zugriffsverwaltung und Berichtsfunktionen in Azure erläutert. Außerdem werden dort bewährte Methoden zum Schützen des Zugriffs auf Azure-Abonnements mit Azure Active Directory untersucht.
* [Autorisieren des Zugriffs auf Webanwendungen mit OAuth 2.0 und Azure Active Directory](../active-directory/develop/v1-protocols-oauth-code.md)
  
  Dieser Artikel beschreibt den vollständige OAuth 2.0-Ablauf für die Authentifizierung über Azure Active Directory.
* [REST-APIs für die Key Vault-Verwaltung](https://msdn.microsoft.com/library/azure/mt620024.aspx)
  
  Dieses Dokument ist die Referenz für die REST-APIs zur programmgesteuerten Key Vault-Verwaltung (einschließlich Festlegung von Key Vault-Zugriffsrichtlinien).
* [Key Vault-REST-APIs](https://msdn.microsoft.com/library/azure/dn903609.aspx)
  
  Link zur Referenzdokumentation für Key Vault-REST-APIs.
* [Schlüsselzugriffssteuerung](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_KeyAccessControl)
  
  Link zur Referenzdokumentation für die Geheimniszugriffssteuerung.
* [Geheimniszugriffssteuerung](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_SecretAccessControl)
  
  Link zur Referenzdokumentation für die Geheimniszugriffssteuerung.
* [Festlegen](https://docs.microsoft.com/powershell/module/azurerm.keyvault/Set-AzureRmKeyVaultAccessPolicy) und [Entfernen](https://docs.microsoft.com/powershell/module/azurerm.keyvault/Remove-AzureRmKeyVaultAccessPolicy) von Key Vault-Zugriffsrichtlinien mithilfe von PowerShell
  
  Links zur Referenzdokumentation für PowerShell-Cmdlets zum Verwalten von Key Vault-Zugriffsrichtlinien.

## <a name="next-steps"></a>Nächste Schritte
[Konfigurieren von Key Vault-Firewalls und virtuellen Netzwerken](key-vault-network-security.md)

Ein Tutorial mit ersten Schritten für Administratoren finden Sie unter [Erste Schritte mit dem Azure-Schlüsseltresor](key-vault-get-started.md).

Weitere Informationen zur Nutzungsprotokollierung für Key Vault finden Sie unter [Azure-Schlüsseltresor-Protokollierung](key-vault-logging.md).

Weitere Informationen zur Verwendung von Schlüsseln und Geheimnissen mit Azure Key Vault finden Sie unter [About keys, secrets, and certificates](https://msdn.microsoft.com/library/azure/dn903623.aspx) (Informationen zu Schlüsseln, Geheimnissen und Zertifikaten).

Sollten Sie Fragen zu Key Vault haben, besuchen Sie die [Azure Key Vault-Foren](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).

