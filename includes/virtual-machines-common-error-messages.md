>[!NOTE]
> Lassen Sie auf dieser Seite für Feedback oder über Kommentare [Azure Feedback](https://feedback.azure.com/forums/216843-virtual-machines) mit #azerrormessage-Tag.

## <a name="error-response-format"></a>Fehler-Antwortformat 
Azure-VMs verwenden das folgende JSON-Format für Fehlerantwort an:

```json
{
  "status": "status code",
  "error": {
    "code":"Top level error code",
    "message":"Top level error message",
    "details":[
     {
      "code":"Inner evel error code",
      "message":"Inner level error message"
     }
    ]
   }
}
```

Eine Fehlerantwort enthält immer einen Statuscode und einen Error-Objekt. Jede Error-Objekt enthält immer einen Fehlercode und eine Nachricht. Wenn der virtuelle Computer mit einer Vorlage erstellt wird, enthält das Fehlerobjekt auch einen Abschnitt, der ein innerer Maß an Fehlercodes und die Nachricht enthält. Die meisten innere Ebene der Fehlermeldung wird in der Regel wird der Stamm-Fehler.


## <a name="common-virtual-machine-management-errors"></a>Allgemeine Fehler der virtuellen Maschine-management

In diesem Abschnitt werden die allgemeinen Fehlermeldungen, die auftreten können, wenn Sie virtuelle Computer verwalten aufgeführt:

|  Fehlercode:  |  Fehlermeldung  |  
|  :------| :-------------|  
|  AcquireDiskLeaseFailed  |  Fehler beim Abrufen Lease beim Erstellen des Datenträgers "{0}" mithilfe von BLOBs mit URI \ {1\}. BLOB wird bereits verwendet.  |  
|  AllocationFailed  |  Fehler bei Zuweisung. Reduzieren Sie die VM-Größe oder Anzahl von virtuellen Computern, später erneut versuchen oder versuchen Sie es auf eine andere Verfügbarkeitsgruppe oder einer anderen Azure-Speicherort bereitstellen.  |  
|  AllocationFailed  |  Fehler bei der VM-Zuweisung aufgrund eines internen Fehlers. Wiederholen Sie dann später, oder versuchen Sie es an einen anderen Speicherort bereitstellen.  |
|  ArtifactNotFound  |  Die VM-Erweiterung mit Verleger "{0}" und Typ "\ {1\}" konnte nicht an Ort "\ {2\}" gefunden werden.  |
|  ArtifactNotFound  |  Erweiterung mit Herausgeber '{0}', geben Sie '\ {1\}' und Handler Typversion "\ {2\}" konnte nicht im erweiterungsrepository gefunden werden.  |
|  ArtifactVersionNotFound  |  Es wurde keine Version in das Repository des Artefakt gefunden, zurück, die die angeforderte Version "{0}" erfüllt.  |
|  ArtifactVersionNotFound  |  Keine Version im Repository Artefakt, das die angeforderte Version "{0}" erfüllt für VM-Erweiterung mit dem Publisher "\ {1\}" gefunden, und geben "\ {2\}".  |
|  AttachDiskWhileBeingDetached  |  Datenträger "{0}" kann nicht für VM "\ {1\}" angefügt werden, weil der Datenträger ist derzeit getrennt wird. Warten Sie, bis der Datenträger vollständig getrennt ist, und wiederholen Sie den Vorgang.  |
|  BadRequest-Objekts  |  Ausgerichtet "Verfügbarkeitsgruppen werden nicht in dieser Region noch unterstützt.  |
|  BadRequest-Objekts  |  Hinzufügen eines virtuellen Computers mit verwalteten Datenträger, um die Verfügbarkeitsgruppe nicht verwalteten ändern oder Hinzufügen eines virtuellen Computers mit BLOB-basierte-Datenträger, um verwaltete Verfügbarkeitsgruppe wird nicht unterstützt. Erstellen Sie eine Verfügbarkeitsgruppe mit 'verwaltete'-Eigenschaft festlegen, um einen virtuellen Computer mit verwalteten Datenträger hinzufügen.  |
|  BadRequest-Objekts  |  Verwalteter Datenträger werden in dieser Region nicht unterstützt.  |
|  BadRequest-Objekts  |  Mehrere VMExtensions pro Handler für das Betriebssystem nicht unterstützt. Geben Sie "{0}". VMExtension "\ {1\,}" mit Handler "\ {2\}" wurde bereits hinzugefügt oder in der Eingabe angegebene.  |
|  BadRequest-Objekts  |  Vorgang '{0}' wird für Ressource "\ {1\,}" mit verwalteten Datenträgern nicht unterstützt.  |
|  CertificateImproperlyFormatted  |  Der geheime JSON-Darstellung von {0} abgerufen hat ein Datenfeld, die nicht auf einer ordnungsgemäß formatierten PFX-Datei ist, oder das angegebene Kennwort ist nicht decodiert werden die PFX-Datei ordnungsgemäß.  |
|  CertificateImproperlyFormatted  |  Die von {0} abgerufene Daten sind nicht in JSON deserializable.  |
|  Konflikt  |  Ändern der Größe von Datenträger ist zulässig, nur beim Erstellen eines virtuellen Computers, oder wenn die Zuordnung des virtuellen Computers aufgehoben wird.  |
|  ConflictingUserInput  |  Datenträger "{0}" kann nicht angefügt werden, wie der Datenträger bereits im Besitz VM "\ {1\}" ist.  |
|  ConflictingUserInput  |  Quell- und Zielserver-Ressourcengruppen sind identisch.  |
|  ConflictingUserInput  |  Quell- und Speicherkonten für Datenträger {0} unterscheiden.  |
|  ContainerAlreadyOnLease  |  Der Speichercontainer, halten die BLOBs mit URI ' {0} ' ist bereits eine Lease.  |
|  CrossSubscriptionMoveWithKeyVaultResources  |  Die Anforderung zum Verschieben von Ressourcen enthält KeyVault-Ressourcen, die eine oder mehrere {0} s in der Anforderung verweist. Dies wird derzeit nicht Cross-Abonnement verschieben unterstützt. Überprüfen Sie die Fehlerdetails für KeyVault-Ressourcen-Ids an.  |
|  DiagnosticsOperationInternalError  |  Interner Fehler beim Verarbeiten der Diagnose-Profil des virtuellen Computers {0}.  |
|  DiskBlobAlreadyInUseByAnotherDisk  |  BLOB {0} wird bereits von einem anderen Datenträger auf VM '\ {1\}' gehören. Sie können die Blob-Metadaten für die Datenträger-Referenzinformationen überprüfen.  |
|  DiskBlobNotFound  |  Nicht VHD-Blob mit URI {0} für den Datenträger "\ {1\}" gefunden.  |
|  DiskBlobNotFound  |  Es VHD-Blob mit dem URI ' {0} ' wurde gefunden.  |
|  DiskEncryptionKeySecretMissingTags  |  {0} geheimen verfügt nicht über die Tags \ {1\}. Aktualisieren Sie die Version des geheimen Schlüssels, fügen Sie die erforderlichen Tags hinzu, und wiederholen Sie.  |
|  DiskEncryptionKeySecretUnwrapFailed  |  Entpacken von geheimen {0}-Wert unter Verwendung der Key \ {1\}, Fehler auf.  |
|  DiskImageNotReady  |  Datenträger-Image {0} ist \ {1\} Status. Wiederholen Sie, wenn das Bild bereit ist.  |
|  DiskPreparationError  |  Fehler beim Vorbereiten der VM-Datenträgern. Finden Sie unter Datenträger Instanzansicht für Details.  |
|  DiskProcessingError  |  Datenträger-Verarbeitung wird angehalten, wie der virtuelle Computer anderen Datenträgern in fehlerhaften Datenträger verfügt.  |
|  ImageBlobNotFound  |  Nicht VHD-Blob mit URI {0} für den Datenträger "\ {1\}" gefunden.  |
|  ImageBlobNotFound  |  Es VHD-Blob mit dem URI ' {0} ' wurde gefunden.  |
|  IncorrectDiskBlobType  |  Datenträger-Blobs können nur vom Typ Seitenblob sein. BLOB {0} für den Datenträger '\ {1\}' ist vom Typ Block-Blob.  |
|  IncorrectDiskBlobType  |  Datenträger-Blobs können nur vom Typ Seitenblob sein. BLOB {0} ist vom Typ "\ {1\}".  |
|  IncorrectImageBlobType  |  Datenträger-Blobs können nur vom Typ Seitenblob sein. BLOB {0} für den Datenträger '\ {1\}' ist vom Typ Block-Blob.  |
|  IncorrectImageBlobType  |  Datenträger-Blobs können nur vom Typ Seitenblob sein. BLOB {0} ist vom Typ "\ {1\}".  |
|  InternalOperationError  |  Speicherkonto {0} konnte nicht aufgelöst werden. Stellen Sie sicher, dass sie über den Speicherressourcenanbieter am gleichen Speicherort wie die Compute-Ressource erstellt wurde.  |
|  InternalOperationError  |  Fehler bei der Zielsuche {0}-Aufgaben.  |
|  InternalOperationError  |  Überprüfen Sie das Profil des virtuellen Computers "{0}" ist ein Fehler aufgetreten.  |
|  InvalidAccountType  |  Die AccountType {0} ist ungültig.  |
|  InvalidParameter  |  Der Wert von {0}-Parameter ist ungültig.  |
|  InvalidParameter  |  Das angegebene Admin-Kennwort ist nicht zulässig.  |
|  InvalidParameter  |  "Das angegebene Kennwort muss zwischen {0}-\ {1\} Zeichen umfassen und muss mindestens erfüllen, \ {2\}, der Anforderungen an die Kennwortkomplexität von folgenden: <ol><li> Enthält ein Großbuchstabe</li><li>Enthält ein Kleinbuchstabe</li><li>Enthält eine Ziffer</li><li>Enthält ein Sonderzeichen.</li></ol>  |
|  InvalidParameter  |  Der angegebene Benutzername des Administrators ist nicht zulässig.  |
|  InvalidParameter  |  Einen vorhandenen Betriebssystemdatenträger kann nicht angefügt werden, wenn der virtuelle Computer aus einem Plattform- oder benutzerimage erstellt wird.  |
|  InvalidParameter  |  {0} Container-Name ist ungültig. Containernamen müssen zwischen 3 und 63 Zeichen lang sein und darf nur alphanumerische Kleinbuchstaben und Bindestrich enthalten. Bindestrich muss diese(r) und gefolgt von einem alphanumerischen Zeichen.  |
|  InvalidParameter  |  Container Name {0} in URL \ {1\} ist ungültig. Containernamen müssen zwischen 3 und 63 Zeichen lang sein und darf nur alphanumerische Kleinbuchstaben und Bindestrich enthalten. Bindestrich muss diese(r) und gefolgt von einem alphanumerischen Zeichen.  |
|  InvalidParameter  |  Der Blob-Name in die URL {0} enthält einen Schrägstrich. Dies wird derzeit nicht für Datenträger unterstützt.  |
|  InvalidParameter  |  Der URI {0} sieht nicht um die richtige Blob-URI sein.  |
|  InvalidParameter  |  Ein Datenträger mit dem Namen "{0}" ist bereits verwendet dieselbe LUN: \ {1\}.  |
|  InvalidParameter  |  Ein Datenträger mit dem Namen "{0}" ist bereits vorhanden ist.  |
|  InvalidParameter  |  Können nicht vom benutzerimage für einen Datenträger, die bereits in den angegebenen Bildverweis definierten Außerkraftsetzungen angeben.  |
|  InvalidParameter  |  Ein Datenträger mit dem Namen "{0}" ist bereits verwendet das gleiche VHD-URL \ {1\}.  |
|  InvalidParameter  |  Die angegebene Fehlerdomäne Domäne Anzahl {0} muss in den Bereich \ {1\}, \ {2\} liegen.  |
|  InvalidParameter  |  Die Lizenz Typ {0} ist ungültig. Gültige Lizenztypen sind: "windows_client" oder Windows_Server, Groß-/Kleinschreibung beachtet.  |
|  InvalidParameter  |  Linux-Hostname darf nicht länger als {0} Zeichen lang oder keines der folgenden Zeichen enthalten: \ {1\}.  |
|  InvalidParameter  |  Zielpfad für den öffentlichen SSH-Schlüssel ist in Linux-Agent-Bereitstellung auf ihre Standardeinstellung Wert {0} aufgrund eines bekannten Problems derzeit beschränkt.  |
|  InvalidParameter  |  Ein Datenträger am LUN {0} ' ist bereits vorhanden.  |
|  InvalidParameter  |  Abonnement {0} der Anforderung muss das Abonnement \ {1\}, enthalten in der verwalteten Datenträger-Id übereinstimmen.  |
|  InvalidParameter  |  Benutzerdefinierte Daten in OSProfile müssen Base64-Codierung und mit einer maximalen Länge von {0} Zeichen sein.  |
|  InvalidParameter  |  Blobname in die URL {0} muss mit der Erweiterung "\ {1\}" enden.  |
|  InvalidParameter  |  {0} "ist kein gültiges erfasstes VHD BLOB-Namenspräfix. Ein gültiges Präfix entspricht dem regulären Ausdruck "\ {1\}".  |
|  InvalidParameter  |  Zertifikate können nicht Ihres virtuellen Computers hinzugefügt werden, wenn der VM-Agent nicht bereitgestellt wurde.  |
|  InvalidParameter  |  Ein Datenträger am LUN {0} ' ist bereits vorhanden.  |
|  InvalidParameter  |  Kann nicht erstellt der virtuellen Computer, da die angeforderte Größe {0} nicht im Cluster verfügbar ist, in dem für die verfügbarkeitsgruppe, wird derzeit zugeordnet. Die verfügbaren Größen sind: \ {1\}. Lesen Sie weitere auf VM-Größe bei https://aka.ms/azure-resizevm.  |
|  InvalidParameter  |  {0} angeforderte VM-Größe ist in der aktuellen Region nicht verfügbar. Die Größen, die in der aktuellen Region verfügbar sind: \ {1\}. Erfahren Sie mehr zu den verfügbaren VM-Größen in jeder Region am https://aka.ms/azure-regions.  |
|  InvalidParameter  |  {0} angeforderte VM-Größe ist in der aktuellen Region nicht verfügbar. Erfahren Sie mehr zu den verfügbaren VM-Größen in jeder Region am https://aka.ms/azure-regions.  |
|  InvalidParameter  |  Windows-Admin-Benutzername darf nicht mehr als {0} Zeichen lang, enden mit einem Punkte (.) oder keines der folgenden Zeichen enthalten sein: \ {1\}.  |
|  InvalidParameter  |  Windows-Computernamen nicht mehr als {0} Zeichen lang sein, nur aus Zahlen bestehen oder folgende Zeichen nicht enthalten: \ {1\}.  |
|  MissingMoveDependentResources  |  Die Anforderung zum Verschieben von Ressourcen enthält nicht alle abhängigen Ressourcen. Überprüfen Sie die Fehlerdetails, um die fehlenden Ressourcen-Ids.  |
|  MoveResourcesHaveInvalidState  |  Verschieben von Ressourcen-Anforderung enthält die VMs, die ungültige Speicher-Konten zugeordnet sind. Prüfen Sie die Details für diese Ressourcen-Ids und Namen des Speicherkontos auf die verwiesen wird.  |
|  MoveResourcesHavePendingOperations  |  Die Anforderung zum Verschieben von Ressourcen enthält Ressourcen, die für die ein Vorgang aussteht. Prüfen Sie die Details für diese Ressourcen-Ids. Wiederholen Sie den Vorgang, nachdem die ausstehenden Vorgänge abgeschlossen.  |
|  MoveResourcesNotFound  |  Die Anforderung zum Verschieben von Ressourcen enthält Ressourcen, die nicht gefunden werden können. Prüfen Sie die Details für diese Ressourcen-Ids.  |
|  NetworkingInternalOperationError  |  Unbekanntes Netzwerk Zuordnungsfehler vor.  |
|  NetworkingInternalOperationError  |  Unbekanntes Netzwerk liegt ein Zuordnungsfehler  |
|  NetworkingInternalOperationError  |  Interner Fehler bei der Verarbeitung von Netzwerkprofil des virtuellen Computers.  |
|  nicht gefunden wurde  |  Die Verfügbarkeitsgruppe {0} kann nicht gefunden werden.  |
|  nicht gefunden wurde  |  Ursprüngliche virtuelle Maschine '{0}"in der Anforderung angegebene in Azure-Speicherort ist nicht vorhanden.  |
|  nicht gefunden wurde  |  -Mandanten Sie mit Id: {0} wurde nicht gefunden.  |
|  nicht gefunden wurde  |  Das Bild {0} kann nicht gefunden werden.  |
|  NotSupported  |  Der Lizenztyp lautet {0}, aber die Image-Blob \ {1\} ist nicht lokal.  |
|  OperationNotAllowed  |  Availability-Set {0} kann nicht gelöscht werden. Vor dem Löschen einer Verfügbarkeitssammlung stellen Sie sicher, dass sie keine virtuellen Computer enthält.  |
|  OperationNotAllowed  |  Ändern die verfügbarkeitsgruppe-SKU von "Ausgerichtet", "Klassisch" ist nicht zulässig.  |
|  OperationNotAllowed  |  Erweiterungen auf dem virtuellen Computer kann nicht geändert werden, wenn der virtuelle Computer nicht ausgeführt wird.  |
|  OperationNotAllowed  |  Der Capture-Vorgang wird nur auf einem virtuellen Computer mit Basis-BLOB-Datenträgern unterstützt. Verwenden Sie 'Image'-Ressourcen-APIs, um ein Image aus einer verwalteten virtuellen Maschine zu erstellen.  |
|  OperationNotAllowed  |  Die Ressource {0} kann vom Image \ {1\} erstellt werden, bis Image wurde erfolgreich erstellt wurde.  |
|  OperationNotAllowed  |  Updates von EncryptionSettings ist nicht zulässig, wenn VM belegt wird, versuchen Sie es nach dem virtuellen Computer freigegeben wird  |
|  OperationNotAllowed  |  Hinzufügen von einem verwalteten Datenträger zu einem virtuellen Computer mit Basis-BLOB-Datenträgern wird nicht unterstützt.  |
|  OperationNotAllowed  |  Die maximale Anzahl von Datenträgern an einen virtuellen Computer in dieser Größe angefügt werden können, ist {0}.  |
|  OperationNotAllowed  |  Hinzufügen eines Datenträgers BLOB-basierten virtuellen Computer mit verwalteten Datenträgern wird nicht unterstützt.  |
|  OperationNotAllowed  |  Vorgang "{0}" ist für Image "\ {1\}" nicht zulässig, da das Bild zum Löschen markiert ist. Sie können nur wiederholen Sie den Löschvorgang (oder warten Sie, bis eine laufende abgeschlossen).  |
|  OperationNotAllowed  |  Vorgang '{0}' ist nicht auf dem virtuellen Computer "\ {1\}" zulässig, da des virtuellen Computers generalisierte.  |
|  OperationNotAllowed  |  Vorgang '{0}' ist nicht zulässig, da Punkt Auflistung für die Wiederherstellung "\ {1\}" zum Löschen markiert ist.  |
|  OperationNotAllowed  |  Vorgang "{0}" ist auf VM-Erweiterung "\ {1\}" nicht zulässig, weil er zum Löschen gekennzeichnet ist. Sie können nur wiederholen Sie den Löschvorgang (oder warten Sie, bis eine laufende abgeschlossen).  |
|  OperationNotAllowed  |  Vorgang '{0}' ist nicht zulässig, da die virtuellen Computer "\ {1\}" mit dem Image "\ {2\}" bereitgestellt werden.  |
|  OperationNotAllowed  |  Vorgang '{0}' ist nicht zulässig, da die virtuelle Maschine ScaleSet "\ {1\}" das Bild '\ {2\}' derzeit verwendet wird.  |
|  OperationNotAllowed  |  Vorgang '{0}' ist nicht auf dem virtuellen Computer "\ {1\}" zulässig, da die VM zum Löschen markiert ist. Sie können nur wiederholen Sie den Löschvorgang (oder warten Sie, bis eine laufende abgeschlossen).  |
|  OperationNotAllowed  |  Vorgang '{0}' ist nicht auf dem virtuellen Computer "\ {1\}" zulässig, da des virtuellen Computers wird entweder freigegeben oder gekennzeichnet werden, um die Zuordnung aufgehoben werden.  |
|  OperationNotAllowed  |  Vorgang '{0}' ist nicht auf dem virtuellen Computer "\ {1\}" zulässig, da die VM ausgeführt wird. Sie wird Power off explizit für den Fall, dass Sie den virtuellen Computer aus, in das Gastbetriebssystem Herunterfahren.  |
|  OperationNotAllowed  |  Vorgang '{0}' ist nicht auf dem virtuellen Computer "\ {1\}" zulässig, da der virtuelle Computer nicht freigegeben wird.  |
|  OperationNotAllowed  |  Vorgang '{0}' ist nicht auf dem virtuellen Computer "\ {1\}" zulässig, da virtuelle Computer im Status "Fehler" Erweiterung '\ {2\}' hat.  |
|  OperationNotAllowed  |  Vorgang '{0}' ist nicht auf dem virtuellen Computer "\ {1\}" zulässig, da ein anderer Vorgang ausgeführt wird.  |
|  OperationNotAllowed  |  Der Vorgang '{0}' erfordert die virtuelle Maschine "\ {1\}" verallgemeinert werden.  |
|  OperationNotAllowed  |  Der Vorgang muss den virtuelle Computer ausgeführt werden (oder ausgeführt).  |
|  OperationNotAllowed  |  Datenträger mit einer Größe {0} GB ab, die kleiner ist als die Größe des entsprechenden Datenträgers in Abbildung {1}GB ist nicht zulässig.  |
|  OperationNotAllowed  |  VM-Skalierungsgruppe Erweiterungen des ereignishandlers '{0}' können nur zum Zeitpunkt der Erstellung von VM Scale Set hinzugefügt werden.  |
|  OperationNotAllowed  |  VM-Skalierungsgruppe Erweiterungen des ereignishandlers '{0}' können nur zum Zeitpunkt der VM-Skalierungsgruppe löschen gelöscht werden.  |
|  OperationNotAllowed  |  VM "{0}" ist bereits verwaltete Datenträger verwenden.  |
|  OperationNotAllowed  |  Virtuellen Computer '{0}' gehört zu "Klassischen" Verfügbarkeit Satz "\ {1\}". Aktualisieren Sie die verfügbarkeitsgruppe 'Aligned' SKU verwenden, und wiederholen dann die Konvertierung aus.  |
|  OperationNotAllowed  |  VM aus Image erstellt keine Basis-BLOB-Datenträger. Alle Datenträger müssen verwaltete Datenträger sein.  |
|  OperationNotAllowed  |  Erfassen Vorgang kann nicht abgeschlossen werden, da der virtuelle Computer nicht generalisierte.  |
|  OperationNotAllowed  |  Verwaltungsvorgänge auf dem virtuellen Computer "{0}" sind unzulässig, weil VM-Datenträgern auf verwalteten Datenträger konvertiert werden.  |
|  OperationNotAllowed  |  Eine laufende Operation ändert ausgeschaltetem Energiezustand des virtuellen Computers {0}, \ {1\}. Führen Sie Vorgang \ {2\} nach einiger Zeit wieder.  |
|  OperationNotAllowed  |  Kann nicht zum Hinzufügen oder Aktualisieren des virtuellen Computers. {0} angeforderte VM-Größe möglicherweise nicht in der vorhandenen Zuordnungseinheit verfügbar. Lesen Sie weitere auf VM-Größe bei https://aka.ms/azure-resizevm.  |
|  OperationNotAllowed  |  Kann nicht zum Ändern der Größe des virtuellen Computers, da die angeforderte Größe {0} nicht im Cluster verfügbar ist, in dem für die verfügbarkeitsgruppe, wird derzeit zugeordnet. Die verfügbaren Größen sind: \ {1\}. Lesen Sie weitere auf VM-Größe bei https://aka.ms/azure-resizevm.  |
|  OperationNotAllowed  |  Kann nicht auf den virtuellen Computers geändert werden, da die angeforderte Größe {0} nicht im Cluster verfügbar ist, in dem der virtuelle Computer derzeit zugewiesen ist. Um die Größe Ihres virtuellen Computers, \ {1\} Bitte deallocate (Dies ist ein Beendigungsvorgang im Azure-Portal), und wiederholen Sie den Vorgang zum Ändern der Größe. Lesen Sie weitere auf VM-Größe bei https://aka.ms/azure-resizevm.  |
|  OSProvisioningClientError  |  BS-Bereitstellung ist fehlgeschlagen für virtuellen Computer "{0}" des Gast-BS derzeit bereitgestellt wird.  |
|  OSProvisioningClientError  |  Fehler bei der BS-Bereitstellung für virtuellen Computer "{0}". Fehlerdetails: \ {1\} stellen Sie sicher, dass das Bild ordnungsgemäß vorbereitet wurde (generalisiert). <ul><li>Anweisungen für Windows: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/  </li></ul> |
|  OSProvisioningClientError  |  Fehler bei der Generierung von SSH-Host. Fehlerdetails: {0}. Zum Beheben dieses Problems überprüfen Sie, ob Linux-Agent ordnungsgemäß eingerichtet ist. <ul><li>Sehen Sie sich die Anweisungen unter: https://azure.microsoft.com/documentation/articles/virtual-machines-linux-agent-user-guide/ </li></ul> |
|  OSProvisioningClientError  |  Für den virtuellen Computer angegebene Benutzername ist ungültig für diese Linux-Distributionen. Fehlerdetails: {0}.  |
|  OSProvisioningInternalError  |  Fehler bei BS-Bereitstellung für virtuellen Computer "{0}" aufgrund eines internen Fehlers.  |
|  OSProvisioningTimedOut  |  BS-Bereitstellung für virtuellen Computer "{0}" konnte nicht in der vorgesehenen Zeit abgeschlossen. Der virtuelle Computer kann weiterhin Abschluss der Bereitstellung erfolgreich. Prüfen Sie dies später Bereitstellungsstatus.  |
|  OSProvisioningTimedOut  |  BS-Bereitstellung für virtuellen Computer "{0}" konnte nicht in der vorgesehenen Zeit abgeschlossen. Der virtuelle Computer kann weiterhin Abschluss der Bereitstellung erfolgreich. Prüfen Sie dies später Bereitstellungsstatus. Außerdem sicher, dass das Bild ordnungsgemäß vorbereitet wurde (generalisiert).   <ul><li>Anweisungen für Windows: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/ </li><li> Anweisungen für Linux: https://azure.microsoft.com/documentation/articles/virtual-machines-linux-capture-image/</li></ul>  |
|  OSProvisioningTimedOut  |  BS-Bereitstellung für virtuellen Computer "{0}" konnte nicht in der vorgesehenen Zeit abgeschlossen. Allerdings wurde die VM-Gast-Agent erkannt ausgeführt wird. Dadurch ergibt sich das Gastbetriebssystem wurde nicht ordnungsgemäß vorbereitet, die als ein VM-Image verwendet werden (mit CreateOption = FromImage). Um dieses Problem zu beheben, verwenden Sie entweder die virtuelle Festplatte mit CreateOption unverändert = anfügen oder ordnungsgemäß für die Verwendung als Image vorbereitet:   <ul><li>Anweisungen für Windows: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/ </li><li> Anweisungen für Linux: https://azure.microsoft.com/documentation/articles/virtual-machines-linux-capture-image/</li></ul>  |
|  OverConstrainedAllocationRequest  |  Die erforderliche Größe des virtuellen Computers ist zurzeit nicht verfügbar in der ausgewählten Position.  |
|  ResourceUpdateBlockedOnPlatformUpdate  |  Ressource kann nicht zu diesem Zeitpunkt aufgrund einer laufenden Plattformupdate aktualisiert werden. Versuchen Sie es später erneut.  |
|  StorageAccountLimitation  |  Storage-Konto "{0}" unterstützt keine Seiten-Blobs, die erforderlich sind, um Datenträger zu erstellen.  |
|  StorageAccountLimitation  |  Speicherkonto "{0}" hat das zugewiesene Kontingent überschritten.  |
|  StorageAccountLocationMismatch  |  Speicherkonto {0} konnte nicht aufgelöst werden. Stellen Sie sicher, dass sie über den Speicherressourcenanbieter am gleichen Speicherort wie die Compute-Ressource erstellt wurde.  |
|  StorageAccountNotFound  |  Storage-Konto {0} wurde nicht gefunden. Stellen Sie sicher, das Speicherkonto nicht gelöscht und gehört zu den gleichen Azure-Speicherort als dem virtuellen Computer.  |
|  StorageAccountNotRecognized  |  Verwenden Sie ein Speicherkonto von Speicherressourcenanbieter verwaltet werden. Verwendung von {0} wird nicht unterstützt.  |
|  StorageAccountOperationInternalError  |  Interner Fehler beim Zugriff auf das Speicherkonto {0}.  |
|  StorageAccountSubscriptionMismatch  |  {0} Storage-Konto gehört nicht zum Abonnement \ {1\}.  |
|  StorageAccountTooBusy  |  Speicherkonto "{0}" ist derzeit ausgelastet. Erwägen Sie ein anderes Konto aus.  |
|  StorageAccountTypeNotSupported  |  Datenträger {0} verwendet \ {1\}, die eine Blob-Speicherkonto ist. Wiederholen Sie den Vorgang mit dem Speicherkonto für allgemeine Zwecke.  |
|  StorageAccountTypeNotSupported  |  {0} Storage-Konto ist vom Typ \ {1\}. Startdiagnoseeinstellungen unterstützt \ {2\} Speicher Kontotypen.  |
|  SubscriptionNotAuthorizedForImage  |  Das Abonnement ist nicht autorisiert.  |
|  TargetDiskBlobAlreadyExists  |  BLOB {0} ' ist bereits vorhanden. Geben Sie einen anderen Blob-URI zum Erstellen einer neuen leeren Datenträger "\ {1\}".  |
|  TargetDiskBlobAlreadyExists  |  Erfassen Vorgang kann nicht fortgesetzt werden, da der Ziel-Image-Blob {0} bereits vorhanden ist und das Flag zum Überschreiben von VHD-Blobs ist nicht festgelegt. Löschen Sie das Blob, oder setzen Sie das Flag zum Überschreiben von VHD-Blobs und wiederholen.  |
|  TargetDiskBlobAlreadyExists  |  Erfassen Vorgang kann nicht fortgesetzt werden, da der Ziel-Image-Blob {0} eine aktive Lease verfügt.   |
|  TargetDiskBlobAlreadyExists  |  BLOB {0} ' ist bereits vorhanden. Bitte geben Sie einen anderen Blob-URI als Ziel für Datenträger "\ {1\}".  |
|  TooManyVMRedeploymentRequests  |  Für den virtuellen Computer "{0}" oder die virtuellen Computer in der gleichen Availabilityset mit diesem virtuellen Computer haben zu viele Anforderungen für die erneute Bereitstellung empfangen wurde. Versuchen Sie es später.  |
|  VHDSizeInvalid  |  Der Wert für die angegebene Datenträgergröße von {0} für den Datenträger "\ {1\,}" mit Blob \ {2\} ist ungültig. Datenträgergröße muss zwischen \ {3\} und \ {4\} liegen.  |
|  VMAgentStatusCommunicationError  |  Virtuellen Computer "{0}" wurde nicht für VM-Agent oder Erweiterungen Status gemeldet. Überprüfen Sie der virtuellen Computer hat einen ausgeführten virtuellen Computer-Agent und ausgehende Verbindungen mit Azure-Speicher herstellen.  |
|  VMArtifactRepositoryInternalError  |  Während der Kommunikation mit dem Artefakt Repository beim Abrufen der Details der VM-Element ist ein Fehler aufgetreten.  |
|  VMArtifactRepositoryInternalError  |  Interner Fehler beim Abrufen der VM-Elementdaten aus dem Repository des Artefakt.  |
|  VMExtensionHandlerNonTransientError  |  Handler "{0}" wurde für VM-Erweiterung '\ {1\}' Fehler gemeldet, mit terminal Fehler Code "\ {2\}" und Fehlermeldung: "\ {3\}"  |
|  VMExtensionManagementInternalError  |  Interner Fehler beim Verarbeiten von VM-Erweiterung "{0}".  |
|  VMExtensionManagementInternalError  |  Mehrere Fehler beim Vorbereiten der VM-Erweiterungen. Finden Sie unter VM-Erweiterung Instanzansicht für Details.  |
|  VMExtensionProvisioningError  |  VM hat einen Fehler gemeldet, bei der Verarbeitung der Erweiterung "{0}". Fehlermeldung: "\ {1\}".  |
|  VMExtensionProvisioningError  |  Mehrere VM-Erweiterungen wurden auf dem virtuellen Computer bereitgestellt werden. Finden Sie in der Instanzansicht für VM-Erweiterung für Details.  |
|  VMExtensionProvisioningTimeout  |  Bereitstellung von VM-Erweiterung "{0}" wurde überschritten. Die Installation der Erweiterung möglicherweise zu lange gedauert, oder Erweiterungsstatus konnte nicht abgerufen werden.  |
|  VMMarketplaceInvalidInput  |  Erstellen einer virtuellen Maschine aus einem nicht-Marketplace-Image muss sich nicht auf Informationen zu planen, entfernen Sie die Informationen in der Anforderung. Betriebssystemdatenträger ist {0}.  |
|  VMMarketplaceInvalidInput  |  Die Kaufinformationen stimmt nicht überein. Kann nicht aus dem Marketplace-Image bereitgestellt. Betriebssystemdatenträger ist {0}.  |
|  VMMarketplaceInvalidInput  |  Beim Erstellen einer virtuellen Maschine vom Marketplace-Image muss Informationen in der Anforderung. Betriebssystemdatenträger ist {0}.  |
|  VMNotFound  |  Der virtuelle Computer "{0}" kann nicht gefunden werden.  |
|  VMRedeploymentFailed  |  VM "{0}" für die erneute Bereitstellung ist ein interner Fehler aufgetreten. Versuchen Sie es später.  |
|  VMRedeploymentTimedOut  |  Erneute Bereitstellung des virtuellen Computers "{0}" konnte nicht in der vorgesehenen Zeit abgeschlossen. Es kann einige Zeit erfolgreich in abgeschlossen. Andernfalls können Sie die Anforderung wiederholen.  |
|  VMStartTimedOut  |  Virtuellen Computer "{0}" wurde nicht in der vorgesehenen Zeit gestartet. Der virtuelle Computer möglicherweise dennoch erfolgreich gestartet werden. Überprüfen Sie den Energiezustand später.  |


## <a name="next-steps"></a>Nächste Schritte
Wenn Sie weitere Hilfe benötigen, erhalten Sie die Azure-Experten auf [MSDN Azure und Stack Overflow-Foren](https://azure.microsoft.com/support/forums/). Alternativ können Sie einen Azure-Support Incident einreichen. Wechseln Sie zu der [Azure Support-Website](https://azure.microsoft.com/support/options/) , und wählen Sie **Get Support**.