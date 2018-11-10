---
title: Bereitstellen von Avere vFXT für Azure
description: Schritte zum Bereitstellen des Avere vFXT-Clusters in Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: 47773f9375927e4d8dfbfec922e4cf11e42ade04
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50670047"
---
# <a name="deploy-the-vfxt-cluster"></a>Bereitstellen des vFXT-Clusters

Um einen vFXT-Cluster zu erstellen, ist es am einfachsten, einen Clustercontroller zu verwenden, d. h. einen virtuellen Computer, der über die erforderlichen Skripts, Vorlagen und Softwareinfrastrukturen für die Erstellung und Verwaltung des vFXT-Clusters verfügt.

Die Bereitstellung eines neuen vFXT-Clusters umfasst folgende Schritte:

1. [Erstellen des Clustercontrollers](#create-the-cluster-controller-vm).
1. Wenn Sie Azure-BLOB-Speicher verwenden, erstellen Sie [einen Speicherendpunkt](#create-a-storage-endpoint-if-using-azure-blob) in Ihrem virtuellen Netzwerk.
1. [Herstellen einer Verbindung mit dem Clustercontroller](#access-the-controller). Die restlichen Schritte werden von der Clustercontroller-VM aus durchgeführt. 
1. [Erstellen der Zugriffsrolle](#create-the-cluster-node-access-role) für die Clusterknoten. Es wird ein Prototyp zur Verfügung gestellt.
1. [Anpassen des Skripts zur Clustererstellung](#edit-the-deployment-script) für den Typ des vFXT-Clusters, den Sie erstellen möchten.
1. [Ausführen des Skripts zur Clustererstellung](#run-the-script).

Weitere Informationen zu den Schritten und der Planung der Clusterbereitstellung finden Sie unter [Planen Ihres Avere vFXT-Systems](avere-vfxt-deploy-plan.md) und [Übersicht über die Bereitstellung](avere-vfxt-deploy-overview.md). 

Nachdem Sie die Anweisungen in diesem Dokument befolgt haben, verfügen Sie über ein virtuelles Netzwerk, ein Subnetz, einen Controller und einen vFXT-Cluster, wie in der folgenden Abbildung dargestellt:

![Diagramm, das das VNet mit optionalem Blobspeicher und ein Subnetz mit drei gruppierten virtuellen Computern mit der Bezeichnung „vFXT-Knoten/vFXT-Cluster“ und einem virtuellen Computer mit der Bezeichnung „Clustercontroller“ zeigt.](media/avere-vfxt-deployment-500px.png)

Vergewissern Sie sich vor dem Start, dass diese Voraussetzungen erfüllt sind:  

1. [Neues Abonnement](avere-vfxt-prereqs.md#create-a-new-subscription)
1. [Berechtigungen des Abonnementbesitzers](avere-vfxt-prereqs.md#configure-subscription-owner-permissions)
1. [Kontingent für den vFXT-Cluster](avere-vfxt-prereqs.md#quota-for-the-vfxt-cluster)

Optional können Sie die Clusterknotenrolle [vor](avere-vfxt-pre-role.md) der Erstellung des Clustercontrollers erstellen, aber es ist einfacher, dies im Anschluss zu erledigen.

## <a name="create-the-cluster-controller-vm"></a>Erstellen der Clustercontroller-VM

Der erste Schritt ist die Erstellung des virtuellen Computers, der die vFXT-Clusterknoten erstellt und konfiguriert. 

Der Clustercontroller ist ein virtueller Linux-Computer mit vorinstallierter Avere vFXT-Clustererstellungssoftware und Skripten. Es sind keine erheblichen Verarbeitungsleistungen oder übermäßig viel Speicherplatz erforderlich, sodass Sie sich für kostengünstige Optionen entscheiden können. Dieser virtuelle Computer wird während der Lebensdauer des vFXT-Clusters gelegentlich verwendet.

Es gibt zwei Methoden, um die Clustercontroller-VM zu erstellen. Zur Vereinfachung des Prozesses wird eine [Azure Resource Manager-Vorlage](#create-controller---arm-template) [unten](#create-controller---arm-template) bereitgestellt, aber Sie können den Controller auch über das [Azure Marketplace-Image](#create-controller---azure-marketplace-image) erstellen. 

Im Rahmen der Erstellung des Controllers können Sie eine neue Ressourcengruppe erstellen.

> [!TIP]
>
> Entscheiden Sie, ob Sie eine öffentliche IP-Adresse auf dem Clustercontroller verwenden möchten. Eine öffentliche IP-Adresse ermöglicht einen einfacheren Zugriff auf den vFXT-Cluster, birgt aber ein leichtes Sicherheitsrisiko.
>
>  * Eine öffentliche IP-Adresse auf dem Clustercontroller ermöglicht es Ihnen, ihn als Jump-Host zu verwenden, um sich von außerhalb des privaten Subnetzes mit dem Avere vFXT-Cluster zu verbinden.
>  * Wenn Sie keine öffentliche IP-Adresse auf dem Controller einrichten, müssen Sie für den Zugriff auf den Cluster einen anderen Jump-Host, eine VPN-Verbindung oder ExpressRoute verwenden. Erstellen Sie z. B. den Controller in einem virtuellen Netzwerk, in dem eine VPN-Verbindung konfiguriert ist.
>  * Wenn Sie einen Controller mit einer öffentlichen IP-Adresse erstellen, sollten Sie den virtuellen Controllercomputer mit einer Netzwerksicherheitsgruppe schützen. Gestatten Sie den Zugriff nur über Port 22, um den Zugriff auf das Internet zu ermöglichen.

### <a name="create-controller---resource-manager-template"></a>Erstellen des Controllers – Resource Manager-Vorlage

Klicken Sie zum Erstellen des Clustercontrollerknotens über das Portal unten auf die Schaltfläche „In Azure bereitstellen“. Diese Bereitstellungsvorlage erstellt den virtuellen Computer, der den Avere vFXT-Cluster erstellt und verwaltet.

[![Schaltfläche zum Erstellen des Controllers](media/deploytoazure.png)](https://ms.portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FAvere%2Fmaster%2Fsrc%2Fvfxt%2Fazuredeploy.json)

Geben Sie die folgenden Informationen an.

Im Abschnitt **BASIC**:  

* **Abonnement** für den Cluster
* **Ressourcengruppe** für den Cluster 
* **Location** 

Im Abschnitt **SETTINGS**:

* Ob ein neues virtuelles Netzwerk erstellt werden soll

  * Wenn Sie ein neues VNet erstellen, wird dem Clustercontroller eine öffentliche IP-Adresse zugewiesen, damit Sie ihn erreichen können. Für dieses VNet wird auch eine Netzwerksicherheitsgruppe erstellt, die den eingehenden Datenverkehr auf den Port 22 beschränkt.
  * Wenn Sie ExpressRoute oder ein VPN verwenden möchten, um sich mit dem Clustercontroller zu verbinden, setzen Sie diesen Wert auf `false`, und geben Sie in den übrigen Feldern ein vorhandenes VNet an. Der Clustercontroller verwendet dieses VNet für die Netzwerkkommunikation. 

* Ressourcengruppe, Name und Subnetzname des virtuellen Netzwerks: Geben Sie die Namen der vorhandenen Ressourcen ein (wenn Sie ein bestehendes VNet verwenden), oder geben Sie neue Namen ein, wenn Sie ein neues VNet erstellen.
* **Controllername**: Legen Sie einen Namen für den virtuellen Controllercomputer fest.
* Benutzername des Controlleradministrators: Die Standardeinstellung ist `azureuser`.
* SSH-Schlüssel: Fügen Sie den öffentlichen Schlüssel ein, um ihn dem Administratorbenutzernamen zuzuordnen. Lesen Sie [Erstellen und Verwenden von SSH-Schlüsseln](https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys), wenn Sie Hilfe benötigen.

Unter den **NUTZUNGSBEDINGUNGEN**: 

* Lesen Sie die Nutzungsbedingungen, und aktivieren Sie das Kontrollkästchen, um sie zu akzeptieren. 

  > [!NOTE] 
  > Wenn Sie kein Abonnementbesitzer sind, lassen Sie einen Besitzer die Nutzungsbedingungen für Sie akzeptieren, indem Sie die erforderlichen Schritte unter [Akzeptieren der Softwarebedingungen im Voraus](avere-vfxt-prereqs.md#accept-software-terms-in-advance) ausführen. 

Klicken Sie auf **Kaufen**, wenn Sie fertig sind. Nach fünf oder sechs Minuten ist Ihr Controllerknoten betriebsbereit.

Sie sollten zur Ausgabeseite wechseln, um Informationen zu sammeln, die für den Cluster erforderlich sind. Weitere Informationen finden Sie unter [Erforderliche Eingaben für die Clustererstellung](#inputs-needed-for-cluster-creation).

### <a name="create-controller---azure-marketplace-image"></a>Erstellen des Controllers – Azure Marketplace-Image

Suchen Sie die Controllervorlage, indem Sie im Azure Marketplace nach dem Namen ``Avere`` suchen. Wählen Sie die Vorlage **Avere vFXT für Azure-Controller** aus. 

Wenn dies noch nicht geschehen ist, akzeptieren Sie die Nutzungsbedingungen und ermöglichen Sie den programmgesteuerten Zugriff auf das Marketplace-Image, indem Sie auf den Link „Möchten Sie die programmgesteuerte Bereitstellung konfigurieren?“ unter der Schaltfläche **Erstellen** klicken.

> [!NOTE] 
> In der ersten Woche der allgemeinen Verfügbarkeit (31. Oktober - 7. November 2018) müssen Sie die Befehlszeilenoption verwenden, um Nutzungsbedingungen für zwei Softwareimages zu akzeptieren, anstatt dieses Verfahren zu befolgen. Befolgen Sie die Anweisungen unter [Akzeptieren der Softwarebedingungen im Voraus](avere-vfxt-prereqs.md#accept-software-terms-in-advance). 

![Screenshot für den Link zum programmgesteuerten Zugriff, der sich unterhalb der Schaltfläche „Erstellen“ befindet](media/avere-vfxt-deploy-programmatically.png)

Klicken Sie auf die Schaltfläche **Aktivieren**, und speichern Sie die Einstellung.

![Screenshot für den Mausklick zur Aktivierung des programmgesteuerten Zugriffs](media/avere-vfxt-enable-program.png)

Kehren Sie zur Hauptseite der Vorlage **Avere vFXT für Azure-Controller** zurück, und klicken Sie auf **Erstellen**. 

Geben Sie im ersten Fenster diese grundlegenden Optionen ein oder bestätigen Sie sie:

* **Abonnement**
* **Ressourcengruppe**: Geben Sie einen neuen Namen ein, wenn Sie eine neue Gruppe erstellen möchten.
* **Virtueller Computername**: Der Name des Controllers.
* **Region**
* **Verfügbarkeitsoptionen**: Redundanz ist nicht erforderlich.
* **Image**: Das Avere vFXT-Controllerknotenimage.
* **Größe**: Belassen Sie die Standardeinstellung, oder wählen Sie einen anderen kostengünstigen Typ aus.
* **Administratorkonto**: Legen Sie die Vorgehensweise zur Anmeldung beim Clustercontroller fest: 
  * Wählen Sie entweder „Benutzername/Kennwort“ oder „Öffentlicher SSH-Schlüssel“ (empfohlen) aus.
  
    > [!TIP] 
    > Ein SSH-Schlüssel ist sicherer. Lesen Sie [Erstellen und Verwenden von SSH-Schlüsseln](https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys), wenn Sie Hilfe benötigen. 
  * Geben Sie den Benutzernamen an. 
  * Fügen Sie den SSH-Schlüssel ein, oder geben Sie das Kennwort ein und bestätigen Sie es.
* **Regeln für eingehende Ports**: Wenn Sie eine öffentliche IP-Adresse verwenden, öffnen Sie Port 22 (SSH).

Klicken Sie auf **Weiter**, um Datenträgeroptionen festzulegen:

* **Betriebssystemdatenträgertyp**: Der Standardwert „HDD“ ist ausreichend.
* **Nicht verwaltete Datenträger verwenden**: Nicht erforderlich.
* **Datenträger**: Nicht verwenden.

Klicken Sie auf **Weiter**, um die Netzwerkoptionen anzuzeigen:

* **Virtuelles Netzwerk**: Wählen Sie das VNet für den Controller aus, oder geben Sie einen Namen ein, um ein neues VNet zu erstellen. Wenn Sie keine öffentliche IP-Adresse auf dem Controller verwenden möchten, ziehen Sie in Betracht, diese in einem VNet zu platzieren, für das ExpressRoute oder eine andere Zugriffsmethode bereits eingerichtet ist.
* **Subnetz**: Wählen Sie ein Subnetz innerhalb des VNets aus (optional). Wenn Sie ein neues VNet erstellen, können Sie gleichzeitig ein neues Subnetz erzeugen.
* **Öffentliche IP-Adresse**: Wenn Sie eine öffentliche IP-Adresse verwenden möchten, können Sie sie hier angeben. 
* **Netzwerksicherheitsgruppe**: Übernehmen Sie die Standardeinstellung (**Standard**). 
* **Öffentliche Eingangsports**: Wenn Sie eine öffentliche IP-Adresse verwenden, verwenden Sie dieses Steuerelement, um den Zugriff über SSH-Datenverkehr zu ermöglichen. 
* **Beschleunigte Netzwerke** ist für diesen virtuellen Computer nicht verfügbar.

Klicken Sie auf **Weiter**, um Verwaltungsoptionen festzulegen:

* **Startdiagnose**: Wechseln Sie zu **Aus**.
* **Diagnose des Gastbetriebssystems**: Lassen Sie diese Option deaktiviert.
* **Diagnosespeicherkonto**: Wählen Sie optional ein Konto aus oder geben Sie ein neues Konto an, um Diagnoseinformationen zu speichern.
* **Verwaltete Dienstidentität**: Ändern Sie diese Option in **Ein**, um einen Azure AD-Dienstprinzipal für den Clustercontroller zu erstellen.
* **Automatisch herunterfahren**: Lassen Sie diese Option deaktiviert. 

An dieser Stelle können Sie auf **Überprüfen + erstellen** klicken, wenn Sie keine Instanztags verwenden möchten. Andernfalls klicken Sie zweimal auf **Weiter**, um die Seite **Gastkonfiguration** zu überspringen und zur Tagsseite zu gelangen. Wenn Sie dort fertig sind, klicken Sie auf **Überprüfen + erstellen**. 

Nachdem Ihre Auswahl überprüft wurde, klicken Sie auf die Schaltfläche **Erstellen**.  

Die Erstellung dauert fünf oder sechs Minuten.

## <a name="create-a-storage-endpoint-if-using-azure-blob"></a>Erstellen eines Speicherendpunkts (bei Verwendung von Azure-BLOB)

Wenn Sie Azure-BLOB-Speicher für Ihren Back-End-Datenspeicher verwenden, sollten Sie einen Speicherdienstendpunkt in Ihrem virtuellen Netzwerk erstellen. Dieser [Dienstendpunkt](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) sorgt dafür, dass der Azure-BLOB-Datenverkehr lokal bleibt, anstatt ihn über das Internet weiterzuleiten.

1. Klicken Sie im Portal links auf **Virtuelle Netzwerke**.
1. Wählen Sie das VNet für Ihren Controller aus. 
1. Klicken Sie links auf **Dienstendpunkte**.
1. Klicken Sie oben auf **Hinzufügen**.
1. Belassen Sie für den Dienst ``Microsoft.Storage``, und wählen Sie das Subnetz des Controllers aus.
1. Klicken Sie unten auf **Hinzufügen**.

  ![Screenshot für das Azure-Portal mit Anmerkungen für die Schritte zum Erstellen des Dienstendpunkts](media/avere-vfxt-service-endpoint.png)

## <a name="gather-needed-inputs"></a>Erfassen erforderlicher Eingaben

Für die Erstellung des Clusters sind folgende Informationen erforderlich. 

Wenn Sie den Controllerknoten mithilfe der Resource Manager-Vorlage erstellt haben, können Sie [die Informationen aus der Vorlagenausgabe](#finding-template-output) abrufen. 

Erforderlich für die Verbindung mit dem Controller: 

* Controllernutzername und SSH-Schlüssel oder Kennwort
* IP-Adresse des Controllers oder eine andere Methode zum Verbinden mit dem virtuellen Controllercomputer

Erforderlich für die Clustererstellung: 

* Ressourcengruppenname
* Azure-Standort 
* Name des virtuellen Netzwerks
* Subnetzname
* Name der Clusterknotenrolle
* Speicherkontoname beim Erstellen eines Blobcontainers

Sie können auch fehlende Informationen finden, indem Sie zur Informationsseite des virtuellen Controllercomputers navigieren. Klicken Sie z. B. auf **Alle Ressourcen**, und suchen Sie nach dem Controllernamen. Klicken Sie dann auf den Controllernamen, um die Details anzuzeigen.

### <a name="finding-template-output"></a>Suchen der Vorlagenausgabe

Gehen Sie wie folgt vor, um diese Informationen in der Resource Manager-Vorlagenausgabe zu finden:

1. Klicken Sie über das Benachrichtigungssymbol in der oberen Leiste des Azure-Portals auf **Zu Ressourcengruppe wechseln**. Dadurch wird die neue Ressourcengruppe angezeigt, die Ihren Controller und Ihr VNet enthält.

   ![Benachrichtigungsbereich des Portals mit der Meldung „Bereitstellung erfolgreich“ und den Schaltflächen „Zu Ressourcengruppe wechseln“ und „An Dashboard anheften“.](media/avere-vfxt-browse-to-rg.png)

1. Klicken Sie auf der linken Seite auf **Bereitstellungen** und dann auf **Microsoft.Template**.

   ![Portalseite „Ressourcengruppe“ mit auf der linken Seite ausgewählten Bereitstellungen und der Anzeige von „Microsoft.Template“ in einer Tabelle unter dem Bereitstellungsnamen](media/avere-vfxt-deployment-template.png)

1. Klicken Sie auf der linken Seite auf **Ausgaben**. Kopieren Sie die Werte in die einzelnen Felder. 

   ![Ausgabeseite mit SSHSTRING, RESOURCE_GROUP, LOCATION, NETWORK, SUBNET und SUBNET_ID in den Feldern rechts neben den Bezeichnungen](media/avere-vfxt-template-outputs.png)

## <a name="access-the-controller"></a>Zugreifen auf den Controller

Sie müssen eine Verbindung mit dem Clustercontroller herstellen, um die restlichen Bereitstellungsschritte durchzuführen.

1. Die Methode zum Herstellen einer Verbindung mit Ihrem Clustercontroller hängt von Ihrem Setup ab.

   * Wenn der Controller über eine öffentliche IP-Adresse verfügt, SSH an die IP-Adresse des Controllers als der von Ihnen festgelegte Administratorbenutzername (z.B. ``ssh azureuser@40.117.136.91``).
   * Wenn der Controller über keine öffentliche IP verfügt, verwenden Sie eine [ExpressRoute](https://docs.microsoft.com/azure/expressroute/) oder eine VPN-Verbindung zu Ihrem VNet.

1. Nachdem Sie sich bei Ihrem Controller angemeldet haben, authentifizieren Sie sich, indem Sie `az login` ausführen. Kopieren Sie den von der Shell bereitgestellten Authentifizierungscode, und verwenden Sie dann einen Webbrowser, um [https://microsoft.com/devicelogin](https://microsoft.com/devicelogin) zu laden und sich mit dem Microsoft-System zu authentifizieren. Kehren Sie zur Bestätigung zur Shell zurück.

   ![Befehlszeilenausgabe des Befehls „AZ login“, der den Browserlink und den Authentifizierungscode anzeigt](media/avere-vfxt-azlogin.png)

1. Fügen Sie Ihr Abonnement hinzu, indem Sie diesen Befehl mit Ihrer Abonnement-ID ausführen: ```az account set --subscription YOUR_SUBSCRIPTION_ID```

## <a name="create-the-cluster-node-access-role"></a>Erstellen der Zugriffsrolle für Clusterknoten

> [!NOTE] 
> Wenn Sie kein Abonnementbesitzer sind und die Rolle noch nicht erstellt wurde, lassen Sie einen Abonnementbesitzer diese Schritte ausführen oder verwenden Sie die Vorgehensweise unter [Erstellen der Zugriffsrolle für die Avere vFXT-Clusterlaufzeit ohne Controller](avere-vfxt-pre-role.md).

[Rollenbasierte Zugriffssteuerung](https://docs.microsoft.com/azure/role-based-access-control/) (RBAC) gibt den vFXT-Clusterknoten die Berechtigung, erforderliche Aufgaben auszuführen.  

Im Rahmen des normalen vFXT-Clusterbetriebs müssen einzelne vFXT-Knoten Aufgaben wie das Lesen von Azure-Ressourceneigenschaften, das Verwalten von Speicher und die Steuerung der Netzwerkschnittstelleneinstellungen anderer Knoten übernehmen. 

1. Öffnen Sie auf dem Controller die Datei ``/avere-cluster.json`` in einem Editor.

   ![Konsole mit einem Listenbefehl und dann „vi /avere-cluster.json“](media/avere-vfxt-open-role.png)

1. Bearbeiten Sie die Datei so, dass sie Ihre Abonnement-ID enthält, und löschen Sie die darüberliegende Zeile. Speichern Sie die Datei als ``avere-cluster.json``.

   ![Text-Editor der Konsole mit der Abonnement-ID und der zum Löschen ausgewählten Option „Diese Zeile entfernen“](media/avere-vfxt-edit-role.png)

1. Verwenden Sie diesen Befehl, um die Rolle zu erstellen:  

   ```bash
   az role definition create --role-definition /avere-cluster.json
   ```

Im nächsten Schritt übergeben Sie den Namen der Rolle an das Skript zur Clustererstellung. 

## <a name="create-nodes-and-configure-the-cluster"></a>Erstellen von Knoten und Konfigurieren des Clusters

Um den Avere vFXT-Cluster zu erstellen, bearbeiten Sie eines der auf dem Controller enthaltenen Beispielskripts und führen Sie es dort aus. Beispielskripts befinden sich im Stammverzeichnis (`/`) auf dem Clustercontroller.

* Wenn Sie einen Blobcontainer erstellen möchten, der als Back-End-Speichersystem für Avere vFXT verwendet werden soll, verwenden Sie das Skript ``create-cloudbacked-cluster``.

* Wenn Sie später Speicher hinzufügen möchten, verwenden Sie das Skript ``create-minimal-cluster``.

> [!TIP]
> Prototypskripts zum Hinzufügen von Knoten und zum Zerstören des vFXT-Clusters sind ebenfalls im Verzeichnis `/` der Clustercontroller-VM enthalten.

### <a name="edit-the-deployment-script"></a>Bearbeiten des Bereitstellungsskripts

Öffnen Sie das Beispielskript in einem Editor. Sie können das angepasste Skript unter einem anderen Namen speichern, um das Überschreiben des Originalbeispiels zu vermeiden.

Geben Sie Werte für diese Skriptvariablen ein.

* Ressourcengruppenname

  * Wenn Sie Netzwerk- oder Speicherkomponenten verwenden, die sich in verschiedenen Ressourcengruppen befinden, entfernen Sie die Kommentare der Variablen und geben Sie auch diese Namen an. 

```python
# Resource groups
# At a minimum specify the resource group.  If the network resources live in a
# different group, specify the network resource group.  Likewise for the storage
# account resource group.
RESOURCE_GROUP=
#NETWORK_RESOURCE_GROUP=
#STORAGE_RESOURCE_GROUP=
```

* Standortname
* Name des virtuellen Netzwerks
* Subnetzname
* Rollenname der Azure AD Runtime: Wenn Sie dem Beispiel in [Erstellen der Clusterknoten-Zugriffsrolle](#create-the-cluster-node-access-role) gefolgt sind, verwenden Sie ``avere-cluster``. 
* Speicherkontoname (wenn ein neuer Blobcontainer erstellt wird)
* Clustername: Sie können nicht zwei vFXT-Cluster mit demselben Namen in derselben Ressourcengruppe verwenden. 
* Administratorkennwort: Wählen Sie ein sicheres Kennwort für die Überwachung und Verwaltung des Clusters aus. Dieses Kennwort ist dem Benutzer ``admin`` zugeordnet. 
* Knoteninstanztyp: Weitere Informationen finden Sie unter [vFXT-Knotengrößen](avere-vfxt-deploy-plan.md#vfxt-node-sizes).
* Knotencachegröße: Weitere Informationen finden Sie unter [vFXT-Knotengrößen](avere-vfxt-deploy-plan.md#vfxt-node-sizes).

Speichern Sie die Datei, und beenden Sie den Vorgang.

### <a name="run-the-script"></a>Ausführen des Skripts
Führen Sie das Skript aus, indem Sie den von Ihnen erstellten Dateinamen eingeben. (Beispiel: `./create-cloudbacked-cluster-west1`)  

Erwägen Sie, diesen Befehl innerhalb eines [Terminal-Multiplexers](http://linuxcommand.org/lc3_adv_termmux.php) wie `screen` oder `tmux` auszuführen, falls Ihre Verbindung getrennt wird.  
Die Ausgabe wird ebenfalls in `~/vfxt.log` protokolliert.

Wenn das Skript abgeschlossen ist, kopieren Sie die Verwaltungs-IP-Adresse, die für die Clusterverwaltung erforderlich ist.

![Befehlszeilenausgabe des Skripts, das die Verwaltungs-IP-Adresse am Ende anzeigt](media/avere-vfxt-mgmt-ip.png)

### <a name="next-step"></a>Nächster Schritt

Nachdem der Cluster ausgeführt wird und Sie seine Verwaltungs-IP-Adresse kennen, können Sie sich mit dem [Clusterkonfigurationstool verbinden](avere-vfxt-cluster-gui.md), um den Support zu aktivieren und bei Bedarf Speicher hinzuzufügen.
