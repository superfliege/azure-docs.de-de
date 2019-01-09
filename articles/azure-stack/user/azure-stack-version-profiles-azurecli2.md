---
title: Herstellen einer Verbindung mit Azure Stack per CLI | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie die plattformübergreifende Befehlszeilenschnittstelle (Command-Line Interface, CLI) verwenden, um Ressourcen in Azure Stack zu verwalten und bereitzustellen.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2018
ms.author: sethm
ms.reviewer: sijuman
ms.openlocfilehash: dacc28c1cfe2ee896597aeaf92a22c7f6e13c306
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/21/2018
ms.locfileid: "53726611"
---
# <a name="use-api-version-profiles-with-azure-cli-in-azure-stack"></a>Verwenden von API-Versionsprofilen mit Azure CLI in Azure Stack

Mit den Schritten in diesem Artikel können Sie die Azure-Befehlszeilenschnittstelle (CLI) zum Verwalten von Azure Stack Development Kit-Ressourcen über Linux- und Mac-Clientplattformen einrichten.

## <a name="install-cli"></a>Installieren der CLI

Melden Sie sich an Ihrer Entwicklungsarbeitsstation an und installieren die CLI. Azure Stack erfordert Version 2.0 oder höher der Azure CLI. Mithilfe der im Artikel [Installieren von Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) beschriebenen Schritte können Sie diese Version installieren. Öffnen Sie ein Terminal oder ein Eingabeaufforderungsfenster, und führen Sie den folgenden Befehl aus, um zu überprüfen, ob die Installation erfolgreich war:

```azurecli
az --version
```

Daraufhin sollten die Version der Azure-CLI und die anderen abhängigen Bibliotheken angezeigt werden, die auf Ihrem Computer installiert sind.

## <a name="trust-the-azure-stack-ca-root-certificate"></a>Einstufen des Zertifizierungsstellen-Stammzertifikats für Azure Stack als vertrauenswürdig

1. Beschaffen Sie das Zertifizierungsstellen-Stammzertifikat für Azure Stack von [Ihrem Azure Stack-Betreiber](../azure-stack-cli-admin.md#export-the-azure-stack-ca-root-certificate), und stufen Sie es als vertrauenswürdig ein. Zum Einstufen des Zertifizierungsstellen-Stammzertifikats für Azure Stack als vertrauenswürdig fügen Sie es an das vorhandene Python-Zertifikat an.

1. Suchen Sie den Speicherort des Zertifikats auf Ihrem Computer. Der Speicherort kann je nachdem, wo Sie Python installiert haben, variieren. Es müssen die Module [pip](https://pip.pypa.io) und [certifi](https://pypi.org/project/certifi/) installiert sein. Sie können den folgenden Python-Befehl über die Bash-Aufforderung verwenden:

  ```bash  
    python -c "import certifi; print(certifi.where())"
  ```

  Notieren Sie sich den Speicherort des Zertifikats. Beispiel: `~/lib/python3.5/site-packages/certifi/cacert.pem`. Der Pfad hängt davon ab, welches BS Sie verwenden und welche Python-Version installiert ist.

### <a name="set-the-path-for-a-development-machine-inside-the-cloud"></a>Pfad für einen Entwicklungscomputer innerhalb der Cloud festlegen

Wenn Sie die CLI über einen Linux-Computer ausführen, der in der Azure Stack-Umgebung erstellt wird, führen Sie den folgenden Bash-Befehl mit dem Pfad zu Ihrem Zertifikat aus.

```bash
sudo cat /var/lib/waagent/Certificates.pem >> ~/<yourpath>/cacert.pem
```

### <a name="set-the-path-for-a-development-machine-outside-the-cloud"></a>Pfad für einen Entwicklungscomputer außerhalb der Cloud festlegen

Wenn Sie die CLI von **außerhalb** der Azure Stack-Umgebung ausführen, ist folgendes zu beachten:  

1. Sie müssen eine [VPN-Verbindung zu Azure Stack](azure-stack-connect-azure-stack.md) einrichten.

1. Kopieren Sie das PEM-Zertifikat, das Sie von Ihrem Azure Stack-Betreiber erhalten haben, und notieren Sie sich den Speicherort der Datei (PATH_TO_PEM_FILE).

1. Führen Sie die folgenden Befehle aus, abhängig vom Betriebssystem Ihrer Entwicklungsarbeitsstation.

#### <a name="linux"></a>Linux

```bash
sudo cat PATH_TO_PEM_FILE >> ~/<yourpath>/cacert.pem
```

#### <a name="macos"></a>macOS

```bash
sudo cat PATH_TO_PEM_FILE >> ~/<yourpath>/cacert.pem
```

#### <a name="windows"></a>Windows

```powershell
$pemFile = "<Fully qualified path to the PEM certificate Ex: C:\Users\user1\Downloads\root.pem>"

$root = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2
$root.Import($pemFile)

Write-Host "Extracting needed information from the cert file"
$md5Hash    = (Get-FileHash -Path $pemFile -Algorithm MD5).Hash.ToLower()
$sha1Hash   = (Get-FileHash -Path $pemFile -Algorithm SHA1).Hash.ToLower()
$sha256Hash = (Get-FileHash -Path $pemFile -Algorithm SHA256).Hash.ToLower()

$issuerEntry  = [string]::Format("# Issuer: {0}", $root.Issuer)
$subjectEntry = [string]::Format("# Subject: {0}", $root.Subject)
$labelEntry   = [string]::Format("# Label: {0}", $root.Subject.Split('=')[-1])
$serialEntry  = [string]::Format("# Serial: {0}", $root.GetSerialNumberString().ToLower())
$md5Entry     = [string]::Format("# MD5 Fingerprint: {0}", $md5Hash)
$sha1Entry    = [string]::Format("# SHA1 Fingerprint: {0}", $sha1Hash)
$sha256Entry  = [string]::Format("# SHA256 Fingerprint: {0}", $sha256Hash)
$certText = (Get-Content -Path $pemFile -Raw).ToString().Replace("`r`n","`n")

$rootCertEntry = "`n" + $issuerEntry + "`n" + $subjectEntry + "`n" + $labelEntry + "`n" + `
$serialEntry + "`n" + $md5Entry + "`n" + $sha1Entry + "`n" + $sha256Entry + "`n" + $certText

Write-Host "Adding the certificate content to Python Cert store"
Add-Content "${env:ProgramFiles(x86)}\Microsoft SDKs\Azure\CLI2\Lib\site-packages\certifi\cacert.pem" $rootCertEntry

Write-Host "Python Cert store was updated for allowing the azure stack CA root certificate"
```

## <a name="get-the-virtual-machine-aliases-endpoint"></a>Abrufen des Endpunkts der VM-Aliase

Bevor Benutzer mithilfe der CLI virtuelle Computer erstellen können, müssen sie vom Azure Stack-Betreiber den Endpunkt-URI der VM-Aliase anfordern. Azure verwendet z.B. den folgenden URI: https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json. Der Cloudadministrator sollte einen entsprechenden Endpunkt für Azure Stack mit den Images einrichten, die im Azure Stack-Marketplace verfügbar sind. Wie im nächsten Abschnitt gezeigt, müssen Benutzer im nächsten Abschnitt den Endpunkt-URI an den `endpoint-vm-image-alias-doc`-Parameter des `az cloud register`-Befehls übergeben. 
   

## <a name="connect-to-azure-stack"></a>Herstellen einer Verbindung mit Azure Stack

Führen Sie die folgenden Schritte aus, um eine Verbindung mit Azure Stack herzustellen:

1. Registrieren Sie die Azure Stack-Umgebung, indem Sie den Befehl `az cloud register` ausführen.
   
   a. Registrieren Sie die Umgebung für *Cloudadministratoren* wie folgt:

      ```azurecli
      az cloud register \ 
        -n AzureStackAdmin \ 
        --endpoint-resource-manager "https://adminmanagement.local.azurestack.external" \ 
        --suffix-storage-endpoint "local.azurestack.external" \ 
        --suffix-keyvault-dns ".adminvault.local.azurestack.external" \ 
        --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases>
      ```
   b. Registrieren Sie die Umgebung für *Benutzer* wie folgt:

      ```azurecli
      az cloud register \ 
        -n AzureStackUser \ 
        --endpoint-resource-manager "https://management.local.azurestack.external" \ 
        --suffix-storage-endpoint "local.azurestack.external" \ 
        --suffix-keyvault-dns ".vault.local.azurestack.external" \ 
        --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases>
      ```
    c. Verwenden Sie zum Registrieren der *Benutzer* in einer mehrinstanzenfähigen Umgebung Folgendes:

      ```azurecli
      az cloud register \ 
        -n AzureStackUser \ 
        --endpoint-resource-manager "https://management.local.azurestack.external" \ 
        --suffix-storage-endpoint "local.azurestack.external" \ 
        --suffix-keyvault-dns ".vault.local.azurestack.external" \ 
        --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases> \
        --endpoint-active-directory-resource-id=<URI of the ActiveDirectoryServiceEndpointResourceID> \
        --profile 2018-03-01-hybrid
      ```
    d. Um den Benutzer in einer AD FS-Umgebung zu registrieren, verwenden Sie:

      ```azurecli
      az cloud register \
        -n AzureStack  \
        --endpoint-resource-manager "https://management.local.azurestack.external" \
        --suffix-storage-endpoint "local.azurestack.external" \
        --suffix-keyvault-dns ".vault.local.azurestack.external"\
        --endpoint-active-directory-resource-id "https://management.adfs.azurestack.local/<tenantID>" \
        --endpoint-active-directory-graph-resource-id "https://graph.local.azurestack.external/"\
        --endpoint-active-directory "https://adfs.local.azurestack.external/adfs/"\
        --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases> \
        --profile "2018-03-01-hybrid"
      ```
1. Legen Sie die aktive Umgebung mithilfe der folgenden Befehle fest.
   
   a. Verwenden Sie für die Umgebung für *Cloudadministratoren* Folgendes:

      ```azurecli
      az cloud set \
        -n AzureStackAdmin
      ```

   b. Verwenden Sie für die Umgebung für *Benutzer* Folgendes:

      ```azurecli
      az cloud set \
        -n AzureStackUser
      ```

1. Aktualisieren Sie Ihre Umgebungskonfiguration so, dass das spezifische API-Versionsprofil für Azure Stack verwendet wird. Führen Sie den folgenden Befehl aus, um die Konfiguration zu aktualisieren:

   ```azurecli
   az cloud update \
     --profile 2018-03-01-hybrid
   ```

    >[!NOTE]  
    >Wenn Sie eine Version von Azure Stack vor Build 1808 ausführen, müssen Sie das API-Versionsprofil **2017-03-09-profile** anstelle des API-Versionsprofils **2018-03-01-hybrid** verwenden.

1. Melden Sie sich bei der Azure Stack-Umgebung an, indem Sie den Befehl `az login` ausführen. Sie können sich entweder als Benutzer oder als [Dienstprinzipal](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects) an der Azure Stack-Umgebung anmelden. 

    * Azure AD-Umgebungen
      * Anmelden als *Benutzer*: Sie können entweder den Benutzernamen und das Kennwort direkt im Befehl `az login` eingeben oder die Authentifizierung über einen Browser ausführen. Sie müssen das letztgenannte Verfahren wählen, wenn für Ihr Konto mehrstufige Authentifizierung (Multi-Factor Authentication) aktiviert ist.

      ```azurecli
      az login \
        -u <Active directory global administrator or user account. For example: username@<aadtenant>.onmicrosoft.com> \
        --tenant <Azure Active Directory Tenant name. For example: myazurestack.onmicrosoft.com>
      ```

      > [!NOTE]
      > Wenn für Ihr Benutzerkonto die mehrstufige Authentifizierung aktiviert ist, können Sie den Befehl `az login command` verwenden, ohne den Parameter `-u` anzugeben. Durch die Ausführung des Befehls erhalten Sie eine URL und einen Code für die Authentifizierung.
   
      * Melden Sie sich als *Dienstprinzipal* an: [Erstellen Sie einen Dienstprinzipal über das Azure-Portal](azure-stack-create-service-principals.md) oder die CLI, und weisen Sie ihm eine Rolle zu, bevor Sie sich anmelden. Melden Sie sich anschließend mit dem folgenden Befehl an:

      ```azurecli  
      az login \
        --tenant <Azure Active Directory Tenant name. For example: myazurestack.onmicrosoft.com> \
        --service-principal \
        -u <Application Id of the Service Principal> \
        -p <Key generated for the Service Principal>
      ```
    * AD FS-Umgebungen

        * Melden Sie sich als Benutzer mit einem Webbrowser an:  
              ```azurecli  
              az login
              ```
        * Melden Sie sich als Benutzer mit einem Webbrowser unter Verwendung eines Gerätecodes an:  
              ```azurecli  
              az login --use-device-code
              ```
        > [!Note]  
        >Durch die Ausführung des Befehls erhalten Sie eine URL und einen Code für die Authentifizierung.

        * Melden Sie sich als Dienstprinzipal an:
        
          1. Bereiten Sie die PEM-Datei vor, die für die Anmeldung des Dienstprinzipals verwendet werden soll.

            * Exportieren Sie auf dem Clientcomputer, auf dem der Prinzipal erstellt wurde, das Dienstprinzipalzertifikat als PFX-Datei mit dem privaten Schlüssel (Sie finden diesen unter `cert:\CurrentUser\My;`, der Zertifikatname trägt den gleichen Namen wie der Prinzipal).
        
            * Konvertieren Sie die PFX-Datei in eine PEM-Datei (mit dem OpenSSL-Hilfsprogramm).

          2.  Melden Sie sich bei der Befehlszeilenschnittstelle an:
            ```azurecli  
            az login --service-principal \
              -u <Client ID from the Service Principal details> \
              -p <Certificate's fully qualified name, such as, C:\certs\spn.pem>
              --tenant <Tenant ID> \
              --debug 
            ```

## <a name="test-the-connectivity"></a>Testen der Konnektivität

Nachdem nun alles eingerichtet ist, können wir mit der CLI Ressourcen in Azure Stack erstellen. Sie können beispielsweise eine Ressourcengruppe für eine Anwendung erstellen und einen virtuellen Computer hinzufügen. Verwenden Sie den folgenden Befehl, um eine Ressourcengruppe mit dem Namen „MyResourceGroup“ zu erstellen:

```azurecli
az group create \
  -n MyResourceGroup -l local
```

Wenn die Ressourcengruppe erfolgreich erstellt wurde, werden mit dem vorherigen Befehl die folgenden Eigenschaften der neu erstellten Ressource ausgegeben:

![Ausgabe der Ressourcengruppenerstellung](media/azure-stack-connect-cli/image1.png)

## <a name="known-issues"></a>Bekannte Probleme
Beachten Sie bei der Verwendung der CLI in Azure Stack folgende bekannte Probleme:

 - Der interaktive CLI-Modus, d.h. der Befehl `az interactive`, wird in Azure Stack noch nicht unterstützt.
 - Verwenden Sie zum Abrufen der Liste der in Azure Stack verfügbaren VM-Images den Befehl `az vm image list --all` anstelle des Befehls `az vm image list`. Wenn Sie die Option `--all` angeben, stellt dies sicher, dass die Antwort nur die Images zurückgibt, die in Ihrer Azure Stack-Umgebung verfügbar sind.
 - VM-Imagealiase, die in Azure verfügbar sind, gelten möglicherweise nicht für Azure Stack. Wenn Sie VM-Images verwenden, müssen Sie den gesamten URN-Parameter (Canonical:UbuntuServer:14.04.3-LTS:1.0.0) anstelle des Imagealias verwenden. Dieser URN muss mit den Imagespezifikationen übereinstimmen, die vom Befehl `az vm images list` abgerufen wurden.

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen von Vorlagen mit der Azure CLI](azure-stack-deploy-template-command-line.md)

[Aktivieren der Azure CLI für Azure Stack-Benutzer (Betreibern)](../azure-stack-cli-admin.md)

[Verwalten von Benutzerberechtigungen](azure-stack-manage-permissions.md)
