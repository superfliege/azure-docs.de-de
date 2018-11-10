---
title: Konfigurieren von Avere vFXT-Speicher – Azure
description: Hinzufügen eines Back-End-Speichersystems zu Avere vFXT für Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: procedural
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: aba023667094fb00647861fb2dd300eda64e4897
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50669957"
---
# <a name="configure-storage"></a>Konfigurieren des Speichers

In diesem Schritt wird das Back-End-Speichersystem für Ihren vFXT-Cluster eingerichtet.

> [!TIP]
> Wenn Sie das Prototypskript `create-cloudbacked-cluster` verwendet haben, um einen neuen Blobcontainer zusammen mit dem Avere vFXT-Cluster zu erstellen, ist dieser Container bereits für die Verwendung eingerichtet und Sie müssen keinen Speicher hinzufügen.

Befolgen Sie diese Anweisungen, wenn Sie das Prototypskript `create-minimal-cluster` für Ihren Cluster verwendet haben oder wenn Sie zusätzliche Hardware oder ein cloudbasiertes Speichersystem hinzufügen möchten.

Es gibt zwei Hauptaufgaben:

1. [Erstellen Sie eine Kernspeichereinheit](#create-a-core-filer), die Ihren vFXT-Cluster mit einem bestehenden Speichersystem oder einem Azure Storage-Konto verbindet.

1. [Erstellen Sie eine Namespaceverbindung](#create-a-junction), die den Pfad definiert, den Clients bereitstellen.

Diese Schritte werden über die Avere-Systemsteuerung durchgeführt. Informationen zur Verwendung des Clusters finden Sie unter [Zugreifen auf den vFXT-Cluster](avere-vfxt-cluster-gui.md).

## <a name="create-a-core-filer"></a>Erstellen einer Kernspeichereinheit

„Kernspeichereinheit“ ist ein vFXT-Begriff für ein Back-End-Speichersystem. Der Speicher kann eine Hardware-NAS-Appliance wie NetApp oder Isilon sein, oder ein Cloudobjektspeicher. Weitere Informationen zu Kernspeichereinheiten finden Sie [in der Anleitung zu den Avere-Clustereinstellungen](http://library.averesystems.com/ops_guide/4_7/settings_overview.html#managing-core-filers).

Wenn Sie eine Kernspeichereinheit hinzufügen möchten, wählen Sie einen der beiden Haupttypen von Kernspeichereinheiten aus:

  * [NAS-Kernspeichereinheit](#nas-core-filer): Beschreibt das Hinzufügen einer NAS-Kernspeichereinheit. 
  * [Cloudkernspeichereinheit für Azure Storage-Konto](#azure-storage-account-cloud-core-filer): Beschreibt das Hinzufügen eines Azure Storage-Kontos als Cloudkernspeichereinheit.

### <a name="nas-core-filer"></a>NAS-Kernspeichereinheit

Eine NAS-Kernspeichereinheit kann eine lokale NetApp oder Isilon sein bzw. ein NAS-Endpunkt in der Cloud.  
Das Speichersystem muss über eine zuverlässige Hochgeschwindigkeitsverbindung zum Avere vFXT-Cluster verfügen – z. B. eine 1 GBit/s ExpressRoute-Verbindung (kein VPN) – und dem Cluster Root-Zugriff auf die verwendeten NAS-Exporte erteilen.

In den folgenden Schritten wird eine NAS-Kernspeichereinheit hinzugefügt:

1. Klicken Sie oben in der Avere Systemsteuerung auf die Registerkarte **Einstellungen**.

2. Klicken Sie links auf **Kernspeichereinheit** > **Kernspeichereinheit verwalten**.

3. Klicken Sie auf **Create**.

   ![Screenshot der Seite „Neue Kernspeichereinheit hinzufügen“ mit dem Namen der Kernspeichereinheit und ihrem vollqualifizierten Domänennamen.](media/avere-vfxt-add-core-filer.png)

   * Benennen Sie Ihre Kernspeichereinheit.
   * Geben Sie einen vollqualifizierten Domänennamen (FQDN) an, falls verfügbar. Andernfalls geben Sie eine IP-Adresse oder einen Hostnamen an, die bzw. der in Ihre Kernspeichereinheit aufgelöst wird.
   * Wählen Sie Ihre Speichereinheitenklasse aus der Liste aus. Wenn Sie sich nicht sicher sind, wählen Sie **Andere** aus.
   * Klicken Sie auf **Weiter**, und wählen Sie eine Cacherrichtlinie aus. 
   * Klicken Sie auf **Speichereinheit hinzufügen**.
   * Detaillierte Informationen finden Sie unter [Hinzufügen einer neuen NAS-Kernspeichereinheit](http://library.averesystems.com/ops_guide/4_7/new_core_filer_nas.html) in der Anleitung zu den Clustereinstellungen von Avere.

Fahren Sie anschließend mit dem [Erstellen einer Verbindung](#create-a-junction) fort.  

### <a name="azure-storage-cloud-core-filer"></a>Azure Storage-Cloudkernspeichereinheit

Um den Azure-BLOB-Speicher als Back-End-Speicher Ihres vFXT-Clusters zu verwenden, benötigen Sie einen leeren Container, den Sie als Kernspeichereinheit hinzufügen können.

> [!TIP] 
> Das ``create-cloudbacked-cluster``-Beispielskript erstellt einen Speichercontainer, definiert ihn als Kernspeichereinheit und erstellt die Namespaceverbindung als Teil der vFXT-Clustererstellung. Das ``create-minimal-cluster``-Beispielskript erstellt keinen Azure-Speichercontainer. Verwenden Sie das Skript ``create-cloudbacked-cluster``, um Ihren vFXT-Cluster bereitzustellen, um zu vermeiden, dass Sie nach der Erstellung des Clusters eine Azure Storage-Kernspeichereinheit erstellen und konfigurieren müssen.

Das Hinzufügen von Blob-Speicher zu Ihrem Cluster erfordert folgende Aufgaben:

* Erstellen eines Speicherkontos (Schritt 1, unten)
* Erstellen eines leeren Blobcontainers (Schritte 2-3)
* Hinzufügen des Speicherzugriffsschlüssels als Cloudanmeldeinformationen für den vFXT-Cluster (Schritte 4-6)
* Hinzufügen des Blobcontainers als Kernspeichereinheit für den vFXT-Cluster (Schritte 7-9)
* Erstellen einer Namespaceverbindung, die Clients zum Bereitstellen der Kernspeichereinheit verwenden ([Erstellen einer Verbindung](#create-a-junction), die für Hardware- und Cloudspeicher dieselbe ist).

Führen Sie die folgenden Schritte aus, um nach der Erstellung des Clusters Blob-Speicher hinzuzufügen. 

1. Erstellen Sie mit den folgenden Einstellungen ein Speicherkonto vom Typ „Allgemein v2 (GPv2)“:

   * **Abonnement**: Wie beim vFXT-Cluster
   * **Ressourcengruppe**: Wie bei der vFXT-Clustergruppe (optional)
   * **Standort**: Wie beim vFXT-Cluster
   * **Leistung**: Standard (Storage Premium wird nicht unterstützt)
   * **Kontotyp**: Allgemein V2 (StorageV2)
   * **Replikation**: Lokal redundanter Speicher (LRS)
   * **Zugriffsebene**: „Heiß“
   * **Sichere Übertragung erforderlich**: Deaktivieren Sie diese Option (kein Standardwert)
   * **Virtuelle Netzwerke**: Nicht erforderlich

   Sie können das Azure-Portal verwenden oder unten auf die Schaltfläche „In Azure bereitstellen“ klicken.

   [![Schaltfläche zum Erstellen eines Speicherkontos](media/deploytoazure.png)](https://ms.portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FAvere%2Fmaster%2Fsrc%2Fvfxt%2Fstorageaccount%2Fazuredeploy.json)

1. Nachdem das Konto erstellt wurde, navigieren Sie zur Seite mit den Speicherkonten.

   ![Neues Speicherkonto im Azure-Portal](media/avere-vfxt-new-storage-acct.png)

1. Erstellen Sie einen Blobcontainer, indem Sie auf der Übersichtsseite auf **Blobs** und dann auf **+Container** klicken. Verwenden Sie einen beliebigen Containernamen und stellen Sie sicher, dass der Zugriff auf **Privat** eingestellt ist.

   ![Seite „Speicherblobs“ ohne vorhandene Container](media/avere-vfxt-blob-no-container.png)

1. Rufen Sie den Azure Storage-Kontoschlüssel ab, indem Sie unter **Einstellungen** auf **Zugriffsschlüssel** klicken:

   ![Grafische Benutzeroberfläche (GUI) des Azure-Portals zum Kopieren des Schlüssels](media/avere-vfxt-copy-storage-key.png) 

1. Öffnen Sie die Avere-Systemsteuerung für Ihren Cluster. Klicken Sie auf **Einstellungen**, und öffnen Sie dann **Cluster** > **Cloudanmeldeinformationen** im linken Navigationsbereich. Klicken Sie auf der Seite mit den Anmeldeinformationen für die Cloud auf **Anmeldeinformationen hinzufügen**.

   ![Klicken auf die Schaltfläche „Anmeldeinformationen hinzufügen“ auf der Konfigurationsseite für die Cloudanmeldeinformationen.](media/avere-vfxt-new-credential-button.png)

1. Geben Sie die folgenden Informationen ein, um Anmeldeinformationen für die Cloudkernspeichereinheit zu erstellen: 

   | Feld | Wert |
   | --- | --- |
   | Name der Anmeldeinformationen | Beliebiger beschreibender Name |
   | Dienstart | (ausgewählter Azure Storage-Zugriffsschlüssel) |
   | Tenant | Speicherkontoname |
   | Abonnement | Abonnement-ID |
   | Speicherzugriffsschlüssel | Azure Storage-Kontoschlüssel (im vorherigen Schritt kopiert) | 

   Klicken Sie auf **Submit**.

   ![Vervollständigtes Formular für Cloudanmeldeinformationen in der Avere-Systemsteuerung](media/avere-vfxt-new-credential-submit.png)

1. Als nächstes erstellen Sie die Kernspeichereinheit. Klicken Sie auf der linken Seite der Avere-Systemsteuerung auf **Kernspeichereinheit** >  **Kernspeichereinheiten verwalten**. 

1. Klicken Sie auf der Einstellungsseite **Kernspeichereinheiten verwalten** auf die Schaltfläche **Erstellen**.

1. Füllen Sie die Felder im Assistenten aus:

   * Wählen Sie den Speichereinheitstyp **Cloud** aus. 
   * Benennen Sie die neue Kernspeichereinheit, und klicken Sie auf **Weiter**.
   * Akzeptieren Sie die Standardcacherichtlinie, und fahren Sie mit der dritten Seite fort.
   * Wählen Sie unter **Diensttyp** die Option **Azure-Speicher** aus. 
   * Wählen Sie die zuvor erstellten Anmeldeinformationen aus.
   * Legen Sie für **Bucketinhalt** die Option **Leer** fest.
   * Ändern Sie **Zertifikatüberprüfung** in **Deaktiviert**.
   * Ändern Sie **Komprimierungsmodus** in **Kein**.  
   * Klicken Sie auf **Weiter**.
   * Geben Sie auf der vierten Seite den Namen des Containers in **Bucketname** als *Speicherkontoname*/*Containername* ein.
   * Optional können Sie **Verschlüsselungstyp** auf **Kein** festlegen.  Azure Storage ist standardmäßig verschlüsselt.
   * Klicken Sie auf **Speichereinheit hinzufügen**.

  Detaillierte Informationen finden Sie unter [Hinzufügen einer neuen Cloudkernspeichereinheit](<http://library.averesystems.com/ops_guide/4_7/new_core_filer_cloud.html>) in der Anleitung zur Clusterkonfiguration von Avere. 

Die Seite wird aktualisiert, oder Sie können die Seite aktualisieren, um Ihre neue Kernspeichereinheit anzuzeigen.

Als nächstes müssen Sie [eine Verbindung erstellen](#create-a-junction).

## <a name="create-a-junction"></a>Erstellen einer Verbindung

Eine Verbindung ist ein Pfad, den Sie für Clients erstellen. Clients binden den Pfad ein und erreichen das von Ihnen gewählte Ziel.

Sie können z. B. `/avere/files` erstellen, um es dem `/vol0/data`-Export und dem `/project/resources`-Unterverzeichnis Ihrer NetApp-Kernspeichereinheit zuzuordnen.

Weitere Informationen zu Verbindungen finden Sie im Abschnitt [Namespace](http://library.averesystems.com/ops_guide/4_7/gui_namespace.html) der Anleitung zur Avere-Clusterkonfiguration.

Führen Sie diese Schritte über die Einstellungsschnittstelle der Avere-Systemsteuerung aus:

* Klicken Sie oben links auf **VServer** > **Namespace**.
* Geben Sie einen Namespacepfad an, der mit „/“ (Schrägstrich) beginnt, z. B. ``/avere/data``.
* Wählen Sie Ihre Kernspeichereinheit aus.
* Wählen Sie den Export der Kernspeichereinheit aus.
* Klicken Sie auf **Weiter**.

  ![Screenshot der Seite „Neue Verbindung hinzufügen“ mit den ausgefüllten Feldern für Verbindung, Kernspeichereinheit und Export](media/avere-vfxt-add-junction.png)

Die Verbindung wird nach einigen Sekunden angezeigt. Erstellen Sie bei Bedarf weitere Verbindungen.

Nachdem die Verbindung erstellt wurde, können Clients [Einbinden des Avere vFXT-Clusters](avere-vfxt-mount-clients.md) verwenden, um auf das Dateisystem zuzugreifen.