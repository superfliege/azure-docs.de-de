



Abhängig von Ihrer Umgebung und Ihrer Auswahl kann das Skript alle die Clusterinfrastruktur, einschließlich virtuellen Azure-Netzwerk, Speicherkonten, Cloud-Dienste, Domänencontroller, Remote- oder lokalen SQL-Datenbanken, Hauptknoten und zusätzliche Clusterknoten erstellt. Alternativ können Sie das Skript mit bereits vorhandenen Azure-Infrastruktur und erstellen die HPC-Clusterknoten.

Hintergrundinformationen zum Planen eines HPC Pack-Clusters finden Sie unter der [Produktbewertung und Planung](https://technet.microsoft.com/library/jj899596.aspx) und [Einstieg](https://technet.microsoft.com/library/jj899590.aspx) Inhalte in der TechNet-Bibliothek für HPC Pack 2012 R2.

## <a name="prerequisites"></a>Voraussetzungen
* **Azure-Abonnement**: Sie können ein Abonnement im Azure global- oder Azure China Service verwenden. Ihren abonnementgrenzen Auswirkungen auf die Anzahl und Art der Clusterknoten aus, die Sie bereitstellen können. Informationen finden Sie unter [Azure-Abonnement und dienstbeschränkungen, Kontingente und Einschränkungen](../articles/azure-subscription-service-limits.md).
* **Windows-Clientcomputer mithilfe von Azure PowerShell 0.8.10 oder höher installiert und konfiguriert**: finden Sie unter [erste Schritte mit Azure PowerShell](/powershell/azureps-cmdlets-docs) installationsanweisungen und Schritte zur Verbindung mit Ihrem Azure-Abonnement.
* **HPC Pack IaaS-Bereitstellungsskript**: herunterladen und Entpacken Sie die neueste Version des Skripts aus dem [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). Überprüfen Sie die Version des Skripts durch Ausführen von `New-HPCIaaSCluster.ps1 –Version`. In diesem Artikel basiert auf Version 4.5.2 des Skripts.
* **Skriptkonfigurationsdatei**: erstellen eine XML-Datei, die das Skript verwendet, um den HPC-Cluster zu konfigurieren. Informationen und Beispiele finden Sie unter Abschnitten weiter unten in diesem Artikel und die Datei Manual.rtf, die das Bereitstellungsskript gehören.

## <a name="syntax"></a>Syntax
```PowerShell
New-HPCIaaSCluster.ps1 [-ConfigFile] <String> [-AdminUserName]<String> [[-AdminPassword] <String>] [[-HPCImageName] <String>] [[-LogFile] <String>] [-Force] [-NoCleanOnFailure] [-PSSessionSkipCACheck] [<CommonParameters>]
```
> [!NOTE]
> Führen Sie das Skript als Administrator an.
> 
> 

### <a name="parameters"></a>Parameter
* **ConfigFile**: Gibt den Dateipfad der Konfigurationsdatei, um den HPC-Cluster zu beschreiben. Finden Sie mehr über die Konfigurationsdatei in diesem Thema oder in der Datei Manual.rtf im Ordner "", die das Skript enthält.
* **AdminUserName**: Gibt den Benutzernamen an. Wenn die Domänengesamtstruktur durch das Skript erstellt wird, wird diese der lokalen Administratorbenutzername für alle virtuellen Computer und der Name des Domänenadministrators an. Wenn die Domänengesamtstruktur bereits vorhanden ist, gibt dies den Domänenbenutzer als Benutzername des lokalen Administrators HPC Pack installieren.
* **AdminPassword**: Gibt das Kennwort des Administrators an. Wenn in der Befehlszeile nicht angegeben wird, fordert das Skript Sie zur Eingabe des Kennworts.
* **HPCImageName** (optional): Gibt an, der Name der HPC Pack VM verwendet, um den HPC-Cluster bereitstellen. Es muss eine bereitgestellte Microsoft HPC Pack-Images aus dem Azure Marketplace. Wenn nicht angegeben (in der Regel empfohlen), wählt das Skript das zuletzt veröffentlichte [HPC Pack 2012 R2-Abbild](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/). Das aktuelle Image basiert auf Windows Server 2012 R2 Datacenter mit HPC Pack 2012 R2 Update 3 installiert.
  
  > [!NOTE]
  > Bereitstellung schlägt fehl, wenn Sie ein gültiges HPC Pack-Bild angeben.
  > 
  > 
* **LogFile** (optional): Gibt an, der Bereitstellung Protokolldateipfad. Wenn nicht angegeben ist, erstellt das Skript eine Protokolldatei im temporären Verzeichnis des Computers, das Skript ausgeführt.
* **Force** (optional): Unterdrückt alle bestätigungsaufforderungen.
* **NoCleanOnFailure** (optional): Gibt an, dass die Azure-VMs, die nicht erfolgreich bereitgestellt wurden nicht entfernt werden. Entfernen diese virtuellen Computer vor dem erneuten Ausführen des Skripts zum Fortsetzen des Bereitstellungsvorgangs manuell, oder der Bereitstellung möglicherweise ein Fehler auf.
* **PSSessionSkipCACheck** (optional): für jeden Clouddienst mit virtuellen Computern, die von diesem Skript bereitgestellt werden soll, wird ein selbstsigniertes Zertifikat automatisch von Azure generiert, und alle virtuellen Computer im Cloud-Dienst verwenden dieses Zertifikat als Standardzertifikat für Windows-Remoteverwaltung (WinRM). Zum Bereitstellen von HPC-Funktionen in diesen Azure-virtuellen Computern installiert das Skript wird standardmäßig diese Zertifikate vorübergehend auf dem lokalen Computer\\vertrauenswürdige Stammzertifizierungsstellen des Clientcomputers, um den Sicherheitsfehler "nicht vertrauenswürdige Zertifizierungsstelle" während der skriptausführung zu unterdrücken. Die Zertifikate werden entfernt, wenn das Skript abgeschlossen wurde. Wenn dieser Parameter angegeben wird, die Zertifikate nicht auf dem Clientcomputer installiert sind, und die sicherheitswarnung wird unterdrückt.
  
  > [!IMPORTANT]
  > Dieser Parameter wird nicht für produktionsbereitstellungen empfohlen.
  > 
  > 

### <a name="example"></a>Beispiel
Das folgende Beispiel erstellt einen HPC Pack-Cluster mithilfe der Konfigurationsdatei *MyConfigFile.xml*, und gibt Administratoranmeldeinformationen für den Cluster installieren.

```PowerShell
.\New-HPCIaaSCluster.ps1 –ConfigFile MyConfigFile.xml -AdminUserName <username> –AdminPassword <password>
```

### <a name="additional-considerations"></a>Weitere Überlegungen
* Das Skript kann auch das Übermitteln von Aufträgen über das HPC Pack-Webportal oder die HPC Pack-REST-API aktivieren.
* Das Skript kann optional benutzerdefinierte Skripts zur vor und nach der Konfiguration auf dem Hauptknoten ausführen, wenn Sie zusätzliche Software installieren oder andere Einstellungen konfigurieren möchten.

## <a name="configuration-file"></a>Konfigurationsdatei
Die Konfigurationsdatei für das Bereitstellungsskript wird eine XML-Datei. Die Schemadatei ist hpciaasclusterconfig.xsd befindet, in das Skript Bereitstellungsordner für HPC Pack IaaS. **IaaSClusterConfig** ist das Stammelement der Konfigurationsdatei, enthält die untergeordneten Elemente in der Datei Manual.rtf im Bereitstellungsverzeichnis Skript ausführlich beschrieben.

