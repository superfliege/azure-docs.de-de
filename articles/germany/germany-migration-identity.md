---
title: Migration von Identity-Ressourcen von Azure Deutschland zu Azure weltweit
description: Dieser Artikel bietet Unterstützung bei der Migration von Identity-Ressourcen von Azure Deutschland zu Azure weltweit.
author: gitralf
services: germany
cloud: Azure Germany
ms.author: ralfwi
ms.service: germany
ms.date: 8/15/2018
ms.topic: article
ms.custom: bfmigrate
ms.openlocfilehash: 5c978cd36fe619e329df370a4b3ed818ac38353f
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/27/2018
ms.locfileid: "52335360"
---
# <a name="migration-of-identity-resources-from-azure-germany-to-global-azure"></a>Migration von Identity-Ressourcen von Azure Deutschland zu Azure weltweit

Dieser Artikel unterstützt Sie bei der Migration von Azure Identity-Ressourcen von Azure Deutschland zu Azure weltweit.

## <a name="azure-active-directory"></a>Azure Active Directory

Azure Active Directory in Azure Deutschland ist von Azure Active Directory in Azure weltweit getrennt. Zurzeit besteht keine Möglichkeit zum Verschieben von Benutzern zwischen Azure Deutschland und Azure weltweit.

Standardmandantennamen unterscheiden sich immer, da Azure das Suffix automatisch abhängig von der Umgebung anfügt. Beispielsweise lautet ein Benutzernamen für ein Mitglied des Mandanten „contoso“ in Azure weltweit `user1@contoso.microsoftazure.com`, in Azure Deutschland hingegen `user1@contoso.microsoftazure.de`.

Bei Verwendung von benutzerdefinierten Domänennamen in Azure AD (z.B. `contoso.com`) muss der Domänenname zuerst in Azure registriert werden. Benutzerdefinierte Domänennamen können **nur in einer** der Cloudumgebungen gleichzeitig definiert werden. Die Domänenüberprüfung schlägt fehl, wenn die Domäne bereits in *einem* Azure Active Directory registriert ist. Dies bedeutet, dass ein Benutzer `user1@contoso.com`, der in Azure Deutschland vorhanden ist, nicht unter dem gleichen Namen zur gleichen Zeit in Azure weltweit ebenfalls vorhanden sein kann. Die Registrierung für `contoso.com` würde bereits fehlschlagen.

Eine „sanfte“ Migration, bei der sich einige Benutzer bereits in der neuen und einige Benutzer noch in der alten Umgebung befinden, würde unterschiedliche Anmeldenamen für die verschiedenen Cloudumgebungen erfordern.

Es würde den Rahmen dieses Dokuments sprengen, jedes mögliche Migrationsszenario zu behandeln. Eine Empfehlung hängt z.B. davon ab, wie Sie die Bereitstellung von Benutzern vornehmen, welche Möglichkeiten bestehen, unterschiedliche Benutzernamen oder UserPrincipalNames zu verwenden, und von anderen Abhängigkeiten, die berücksichtigt werden müssen. Im Folgenden finden Sie jedoch einige Hinweise zur Vorgehensweise beim Inventarisieren von Benutzern und Gruppen aus Ihrer aktuellen Umgebung.

Um eine Liste aller verfügbaren Cmdlets zu erhalten, die sich auf Azure AD beziehen, verwenden Sie Folgendes:

```powershell
Get-Help Get-AzureAD*
```

### <a name="inventory-of-users"></a>Bestandsaufnahme der Benutzer

Um einen Überblick über alle Benutzer und Gruppen in Ihrem Azure Active Directory zu erhalten, können Sie den folgenden PowerShell-Befehl verwenden:

```powershell
Get-AzureADUser -All $true
```

Verwenden Sie den folgenden Filter, um nur aktivierte Konten aufzulisten:

```powershell
Get-AzureADUser -All $true | Where-Object {$_.AccountEnabled -eq $true}
```

Nehmen Sie eine vollständige Sicherung aller Attribute für den Fall vor, dass Sie etwas vergessen:

```powershell
Get-AzureADUser -All $true | Where-Object {$_.AccountEnabled -eq $true} | Format-List *
```

Wählen Sie die Attribute aus, die Sie benötigen, um die Benutzer erneut zu erstellen:

```powershell
Get-AzureADUser -All $true | Where-Object {$_.AccountEnabled -eq $true} | select UserPrincipalName,DisplayName,GivenName,Surname
```

Um die Liste nach Excel zu exportieren, verwenden das Cmdlet `Export-Csv` am Ende. Ein vollständiger Export kann wie dieses Beispiel aussehen:

```powershell
Get-AzureADUser -All $true | Where-Object {$_.AccountEnabled -eq $true} | select UserPrincipalName,DisplayName,GivenName,Surname | Export-Csv -Path c:\temp\alluserUTF8.csv -Delimiter ";" -Encoding UTF8
```

> [!NOTE]
> Kennwörter können nicht migriert werden. Sie müssen je nach Szenario neue Kennwörter zuweisen oder einen Self-Service-Mechanismus verwenden.


> [!NOTE]
> Abhängig von Ihrer Umgebung gibt es möglicherweise weitere Informationen, die sie erfassen möchten, z.B. Erweiterungen, DirectReport, LicenceDetail usw.

Formatieren Sie Ihre CSV-Datei nach Bedarf, und führen Sie die unter [Importieren von Daten aus einer CSV-Datei](/powershell/azure/active-directory/importing-data) beschriebenen Schritte aus, um die Benutzer in der neuen Umgebung erneut zu erstellen.

### <a name="inventory-of-groups"></a>Bestandsaufnahme der Gruppen

Verwenden Sie die folgenden PowerShell-Cmdlets, um die Gruppenmitgliedschaft zu dokumentieren:

```powershell
Get-AzureADGroup
```

Durchlaufen Sie die Liste der Gruppen, um die Liste der Mitglieder für jede Gruppe zu erhalten:

```powershell
Get-AzureADGroup | ForEach-Object {$_.DisplayName; Get-AzureADGroupMember -ObjectId $_.ObjectId}
```

### <a name="inventory-of-service-principals-and-applications"></a>Bestandsaufnahme der Dienstprinzipale und Anwendungen

Auch wenn alle Dienstprinzipale und Anwendungen neu erstellt werden müssen, ist es empfehlenswert, den Status zu dokumentieren. Sie können die folgenden Cmdlets verwenden, um eine umfassende Liste aller Dienstprinzipale abzurufen.

```powershell
Get-AzureADServicePrincipal |Format-List *
```

```powershell
Get-AzureADApplication |Format-List *
```

Sie können weitere Informationen mit anderen Cmdlets abrufen, die mit `Get-AzureADServicePrincipal*` oder `Get-AzureADApplication*` beginnen. 

### <a name="directory-roles"></a>Verzeichnisrollen

Um die aktuellen Rollenzuweisungen zu dokumentieren, verwenden Sie ein ähnliches Verfahren wie es oben für die Gruppen beschrieben wurde:

```powershell
Get-AzureADDirectoryRole
```

Durchlaufen Sie jede Rolle, um Benutzer oder Anwendungen zu ermitteln, die der jeweiligen Rolle zugeordnet sind:

```powershell
Get-AzureADDirectoryRole | ForEach-Object {$_.DisplayName; Get-AzureADDirectoryRoleMember -ObjectId
$_.ObjectId | Format-Table}
```



## <a name="next-steps"></a>Nächste Schritte

- Informationen zu [Hybrididentitätslösungen](../active-directory/choose-hybrid-identity-solution.md)
- Lesen Sie [diesen Blog](https://blogs.technet.microsoft.com/ralfwi/2017/01/24/using-adconnect-with-multiple-clouds/) zu Methoden für die Synchronisierung in verschiedenen Cloudumgebungen.

## <a name="references"></a>Referenzen

- [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/)
- [Benutzerdefinierte Domänennamen](../active-directory/fundamentals/add-custom-domain.md)
- [Importieren von Daten aus einer CSV-Datei in Azure AD](/powershell/azure/active-directory/importing-data)

## <a name="adconnect"></a>ADConnect

ADConnect ist ein Tool, das Ihre Identitätsdaten zwischen dem lokalen Active Directory und Azure Active Directory synchronisiert. Die aktuelle Version von ADConnect funktioniert für beide Cloudumgebungen: für Azure Deutschland und Azure weltweit. ADConnect kann die Synchronisierung nur mit jeweils einem Azure AD gleichzeitig ausführen. Wenn die Synchronisierung mit Azure Deutschland und Azure weltweit gleichzeitig erfolgen soll, finden Sie weitere Informationen in diesen Themen:

- Verwenden Sie einen zusätzlichen Server für eine zweite Instanz von ADConnect. Mehrere Instanzen von ADConnect auf dem gleichen Server werden nicht unterstützt.
- Definieren Sie einen neuen Anmeldenamen für Ihre Benutzer. Der Domänenteil (nach dem @-Zeichen) des Anmeldenamens muss in beiden Umgebungen unterschiedlich sein.
- Definieren Sie eine klare „Quelle der Wahrheit“, wenn Sie auch zurück synchronisieren (aus Azure AD in das lokale AD).

Weitere Informationen zur Synchronisierung in verschiedenen Cloudumgebungen mit ADConnect finden Sie in [diesem Blog](https://blogs.technet.microsoft.com/ralfwi/2017/01/24/using-adconnect-with-multiple-clouds/).

Wenn Sie ADConnect bereits für die Synchronisierung in und aus Azure Deutschland verwenden, stellen Sie sicher, dass Sie nicht vergessen, manuell erstellte Benutzer zu migrieren. Das folgende PowerShell-Cmdlet listet alle Benutzer auf, die nicht durch ADConnect synchronisiert werden:

```powershell
Get-AzureADUser -All $true |Where-Object {$_.DirSyncEnabled -ne "True"}
```

### <a name="next-steps"></a>Nächste Schritte

- Informationen zu [ADConnect](../active-directory/hybrid/reference-connect-dirsync-deprecated.md)







## <a name="multi-factor-authentication"></a>Multi-Factor Authentication

Da Benutzer in der neuen Umgebung erneut erstellt werden müssen, muss auch die mehrstufige Authentifizierung erneut definiert werden. Zum Abrufen einer Liste von Benutzerkonten, für die mehrstufige Authentifizierung aktiviert ist oder erzwungen wird, gehen Sie folgendermaßen vor:

- Melden Sie sich am Azure-Portal an.
- Wählen Sie `Users` > `All Users` > `Multi-Factor Authentication` aus.
- Legen Sie nach der Weiterleitung an die Seite für den mehrstufigen Authentifizierungsdienst die gewünschten Filter fest, um eine Liste der Benutzer zu erhalten.

### <a name="next-steps"></a>Nächste Schritte

- Informationen zu [Azure Multi-Factor Authentication](../active-directory/authentication/howto-mfa-getstarted.md)