---
title: "Azure Site Recovery-Problembehandlung für Azure-zu-Azure-Replikationsprobleme und -fehler| Microsoft-Dokumentation"
description: "Problembehandlungsfehler und -probleme beim Replizieren von virtuellen Azure-Computern für die Notfallwiederherstellung"
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.date: 02/12/2018
ms.author: sujayt
ms.openlocfilehash: be43e34976682847c4756e062ec5b638ebc26063
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2018
---
# <a name="troubleshoot-azure-to-azure-vm-replication-issues"></a>Problembehandlung für Azure-zu-Azure-VM-Replikationsprobleme

In diesem Artikel werden die Probleme beschrieben, die in Azure Site Recovery beim Replizieren und Wiederherstellen von virtuellen Azure-Computern aus einer Region in einer anderen Region häufiger auftreten können, und es wird erklärt, wie die Problembehandlung durchgeführt werden kann. Weitere Informationen zu unterstützten Konfigurationen finden Sie unter [Azure Site Recovery support matrix for replicating from Azure to Azure](site-recovery-support-matrix-azure-to-azure.md) (Azure Site Recovery-Supportmatrix zum Replizieren aus Azure in Azure).

## <a name="azure-resource-quota-issues-error-code-150097"></a>Probleme mit dem Azure-Ressourcenkontingent (Fehlercode 150097)
Für Ihr Abonnement sollte es möglich sein, Azure-VMs in der Zielregion zu erstellen, die Sie als Region für die Notfallwiederherstellung nutzen möchten. Außerdem sollte für Ihr Abonnement ein ausreichendes Kontingent für die Erstellung von VMs einer bestimmten Größe vorhanden sein. Site Recovery wählt für die Ziel-VM und die Quell-VM standardmäßig die gleiche Größe. Falls keine übereinstimmende Größe verfügbar ist, wird automatisch die nächstgelegene Größe gewählt. Wenn keine übereinstimmende Größe vorhanden ist, für die die Konfiguration der Quell-VM unterstützt wird, wird diese Fehlermeldung angezeigt:

**Fehlercode** | **Mögliche Ursachen** | **Empfehlung**
--- | --- | ---
150097<br></br>**Meldung**: Die Replikation konnte für den virtuellen Computer VmName nicht aktiviert werden. | - Für Ihre Abonnement-ID ist die Erstellung von VMs am Standort der Zielregion ggf. nicht aktiviert.</br></br>- Für Ihre Abonnement-ID ist die Erstellung von bestimmten VM-Größen am Standort der Zielregion ggf. nicht aktiviert, oder es ist kein ausreichendes Kontingent vorhanden.</br></br>- Für die Abonnement-ID ist am Standort der Zielregion keine geeignete Größe der Ziel-VM vorhanden, die mit der NIC-Anzahl (2) der Quell-VM übereinstimmt.| Wenden Sie sich unter [Anforderungen zur Erhöhung von Resource Manager-Kernkontingenten](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) an den Azure-Abrechnungssupport, um die VM-Erstellung für die erforderlichen VM-Größen am Zielstandort für Ihr Abonnement aktivieren zu lassen. Wiederholen Sie den Vorgang, nachdem die Aktivierung durchgeführt wurde.

### <a name="fix-the-problem"></a>Beheben des Problems
Sie können sich unter [Anforderungen zur Erhöhung von Resource Manager-Kernkontingenten](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) an den Azure-Abrechnungssupport wenden, um Ihr Abonnement für die Erstellung von VMs der erforderlichen Größen am Zielstandort aktivieren zu lassen.

Falls für den Zielstandort eine Kapazitätsbeschränkung besteht, sollten Sie die Replikation deaktivieren und für einen anderen Standort aktivieren, an dem für Ihr Abonnement ein ausreichendes Kontingent zum Erstellen von VMs der erforderlichen Größen zur Verfügung steht.

## <a name="trusted-root-certificates-error-code-151066"></a>Vertrauenswürdige Stammzertifikate (Fehlercode: 151066)

Wenn auf der VM nicht alle aktuellen vertrauenswürdigen Stammzertifikate vorhanden sind, ist der Auftrag „Replikation aktivieren“ ggf. nicht erfolgreich. Ohne die Zertifikate tritt für die Authentifizierung und Autorisierung von Site Recovery-Dienstaufrufen über die VM ein Fehler auf. Die Fehlermeldung für den fehlgeschlagenen Site Recovery-Auftrag „Replikation aktivieren“ wird angezeigt:

**Fehlercode** | **Mögliche Ursache** | **Empfehlungen**
--- | --- | ---
151066<br></br>**Meldung**: Fehler bei der Site Recovery-Konfiguration. | Die erforderlichen vertrauenswürdigen Stammzertifikate, die für die Autorisierung und Authentifizierung verwendet werden, sind auf dem Computer nicht vorhanden. | - Für eine VM, auf der das Windows-Betriebssystem ausgeführt wird, sollten Sie sicherstellen, dass die vertrauenswürdigen Stammzertifikate auf dem Computer vorhanden sind. Informationen finden Sie unter [Konfigurieren vertrauenswürdiger Stämme und unzulässiger Zertifikate](https://technet.microsoft.com/library/dn265983.aspx).<br></br>- Befolgen Sie für eine VM, auf der das Linux-Betriebssystem ausgeführt wird, die Anleitung für vertrauenswürdige Stammzertifikate. Diese Anleitung wird vom Distributor der Linux-Betriebssystemversion veröffentlicht.

### <a name="fix-the-problem"></a>Beheben des Problems
**Windows**

Installieren Sie alle aktuellen Windows-Updates auf der VM, damit alle vertrauenswürdigen Stammzertifikate auf dem Computer vorhanden sind. Wenn Sie sich in einer nicht verbundenen Umgebung befinden, können Sie den Standardprozess für Windows-Updates in Ihrer Organisation durchführen, um die Zertifikate abzurufen. Falls die erforderlichen Zertifikate auf der VM nicht vorhanden sind, schlagen die Aufrufe des Site Recovery-Diensts aus Sicherheitsgründen fehl.

Führen Sie den üblichen Prozess Ihrer Organisation für die Verwaltung von Windows-Updates oder Zertifikaten durch, um alle aktuellen Stammzertifikate und die aktualisierte Zertifikatssperrliste auf den VMs abzurufen.

Navigieren Sie mit einem Browser auf Ihrer VM zu „login.microsoftonline.com“, um zu überprüfen, ob das Problem behoben wurde.

**Linux**

Befolgen Sie die Anleitung Ihres Linux-Distributors, um die aktuellen vertrauenswürdigen Stammzertifikate und die aktuelle Zertifikatssperrliste auf der VM abzurufen.

Führen Sie diese Schritte aus, da SuSE Linux zum Verwalten einer Zertifikatliste symlinks verwendet:

1.  Melden Sie sich als Stammbenutzer an.

2.  Führen Sie den folgenden Befehl aus, um das Verzeichnis zu ändern:

      ``# cd /etc/ssl/certs``

3. Überprüfen Sie, ob das Symantec-Zertifikat der Stammzertifizierungsstelle vorhanden ist:

      ``# ls VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

4. Wurde das Symantec-Zertifikat der Stammzertifizierungsstelle nicht gefunden, führen Sie den folgenden Befehl aus, um die Datei herunterzuladen. Überprüfen Sie, ob Fehler aufgetreten sind, und führen Sie die empfohlenen Maßnahmen für Netzwerkfehler aus.

      ``# wget https://www.symantec.com/content/dam/symantec/docs/other-resources/verisign-class-3-public-primary-certification-authority-g5-en.pem -O VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

5. Überprüfen Sie, ob das Baltimore-Zertifikat der Stammzertifizierungsstelle vorhanden ist:

      ``# ls Baltimore_CyberTrust_Root.pem``

6. Wurde das Baltimore-Zertifikat der Stammzertifizierungsstelle nicht gefunden, laden Sie das Zertifikat herunter.  

    ``# wget http://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem -O Baltimore_CyberTrust_Root.pem``

7. Überprüfen Sie, ob das Zertifikat „DigiCert_Global_Root_CA“ vorhanden ist:

    ``# ls DigiCert_Global_Root_CA.pem``

8. Wurde „DigiCert_Global_Root_CA“ nicht gefunden, führen Sie die folgenden Befehle aus, um das Zertifikat herunterzuladen:

    ``# wget http://www.digicert.com/CACerts/DigiCertGlobalRootCA.crt``

    ``# openssl x509 -in DigiCertGlobalRootCA.crt -inform der -outform pem -out DigiCert_Global_Root_CA.pem``

9. Führen Sie das Skript für das erneute Ausführen von Hashvorgängen aus, um die Zertifikatantragstellerhashes für die neu heruntergeladenen Zertifikate zu aktualisieren.

    ``# c_rehash``

10. Überprüfen Sie, ob die Antragstellerhashes als Symlinks für die Zertifikate erstellt werden.

    - Get-Help

      ``# ls -l | grep Baltimore``

    - Output

      ``lrwxrwxrwx 1 root root   29 Jan  8 09:48 3ad48a91.0 -> Baltimore_CyberTrust_Root.pem
      -rw-r--r-- 1 root root 1303 Jun  5  2014 Baltimore_CyberTrust_Root.pem``

    - Get-Help

      ``# ls -l | grep VeriSign_Class_3_Public_Primary_Certification_Authority_G5``

    - Output

      ``-rw-r--r-- 1 root root 1774 Jun  5  2014 VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem
      lrwxrwxrwx 1 root root   62 Jan  8 09:48 facacbc6.0 -> VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

    - Get-Help

      ``# ls -l | grep DigiCert_Global_Root``

    - Output

      ``lrwxrwxrwx 1 root root   27 Jan  8 09:48 399e7759.0 -> DigiCert_Global_Root_CA.pem
      -rw-r--r-- 1 root root 1380 Jun  5  2014 DigiCert_Global_Root_CA.pem``

11. Erstellen Sie eine Kopie der Datei „VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem“ mit dem Dateinamen „b204d74a.0“.

    ``# cp VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem b204d74a.0``

12. Erstellen Sie eine Kopie der Datei „Baltimore_CyberTrust_Root.pem“ mit dem Dateinamen „653b494a.0“.

    ``# cp Baltimore_CyberTrust_Root.pem 653b494a.0``

13. Erstellen Sie eine Kopie der Datei „DigiCert_Global_Root_CA.pem“ mit dem Dateinamen „3513523f.0“.

    ``# cp DigiCert_Global_Root_CA.pem 3513523f.0``  


14. Überprüfen Sie, ob die Dateien vorhanden sind.  

    - Get-Help

      ``# ls -l 653b494a.0 b204d74a.0 3513523f.0``

    - Output

      ``-rw-r--r-- 1 root root 1774 Jan  8 09:52 3513523f.0
      -rw-r--r-- 1 root root 1303 Jan  8 09:52 653b494a.0
      -rw-r--r-- 1 root root 1774 Jan  8 09:52 b204d74a.0``


## <a name="outbound-connectivity-for-site-recovery-urls-or-ip-ranges-error-code-151037-or-151072"></a>Ausgehende Konnektivität für Site Recovery-URLs oder IP-Bereiche (Fehlercode 151037 oder 151072)

Damit die Site Recovery-Replikation funktioniert, ist für die VM die ausgehende Konnektivität zu bestimmten URLs oder IP-Bereichen erforderlich. Wenn sich Ihre VM hinter eine Firewall befindet oder Netzwerksicherheitsgruppen-Regeln (NSG-Regeln) zum Steuern der ausgehenden Konnektivität verwendet werden, wird ggf. eine dieser Fehlermeldungen angezeigt:

**Fehlercodes** | **Mögliche Ursachen** | **Empfehlungen**
--- | --- | ---
151037<br></br>**Meldung**: Fehler beim Registrieren des virtuellen Azure-Computers bei Site Recovery. | - Sie verwenden die NSG zum Steuern des Zugriffs in ausgehender Richtung auf der VM, und die erforderlichen IP-Bereiche sind für den Zugriff in ausgehender Richtung nicht auf einer Positivliste aufgeführt.</br></br>- Sie verwenden Firewalltools von Drittanbietern, und die erforderlichen IP-Bereiche/URLs sind nicht auf einer Positivliste aufgeführt.</br>| - Wenn Sie einen Firewallproxy zum Steuern der ausgehenden Netzwerkkonnektivität auf der VM verwenden, sollten Sie sicherstellen, dass die erforderlichen URLs oder Datencenter-IP-Bereiche auf der Positivliste enthalten sind. Siehe hierzu die [Firewallproxy-Anleitung](https://aka.ms/a2a-firewall-proxy-guidance).</br></br>- Wenn Sie NSG-Regeln zum Steuern der ausgehenden Netzwerkkonnektivität auf der VM verwenden, sollten Sie sicherstellen, dass die erforderlichen Datencenter-IP-Bereiche auf der Positivliste enthalten sind. Informationen hierzu unter [Networking guidance for replicating Azure virtual machines](https://aka.ms/a2a-nsg-guidance) (Netzwerkanleitung für die Replikation von virtuellen Azure-Computern).
151072<br></br>**Meldung**: Fehler bei der Site Recovery-Konfiguration. | Die Verbindung mit Site Recovery-Dienstendpunkten kann nicht hergestellt werden. | - Wenn Sie einen Firewallproxy zum Steuern der ausgehenden Netzwerkkonnektivität auf der VM verwenden, sollten Sie sicherstellen, dass die erforderlichen URLs oder Datencenter-IP-Bereiche auf der Positivliste enthalten sind. Siehe hierzu die [Firewallproxy-Anleitung](https://aka.ms/a2a-firewall-proxy-guidance).</br></br>- Wenn Sie NSG-Regeln zum Steuern der ausgehenden Netzwerkkonnektivität auf der VM verwenden, sollten Sie sicherstellen, dass die erforderlichen Datencenter-IP-Bereiche auf der Positivliste enthalten sind. Informationen hierzu unter [Networking guidance for replicating Azure virtual machines](https://aka.ms/a2a-nsg-guidance) (Netzwerkanleitung für die Replikation von virtuellen Azure-Computern).

### <a name="fix-the-problem"></a>Beheben des Problems
Führen Sie die Schritte im [Dokument mit der Netzwerkanleitung](site-recovery-azure-to-azure-networking-guidance.md) aus, um die [erforderlichen URLs](azure-to-azure-about-networking.md#outbound-connectivity-for-urls) bzw. die [erforderlichen IP-Bereiche](azure-to-azure-about-networking.md#outbound-connectivity-for-ip-address-ranges) auf eine Positivliste zu setzen.

## <a name="disk-not-found-in-the-machine-error-code-150039"></a>Datenträger wurde auf dem Computer nicht gefunden (Fehlercode 150039)

Es muss ein neuer Datenträger initialisiert werden, der an die VM angefügt ist.

**Fehlercode** | **Mögliche Ursachen** | **Empfehlungen**
--- | --- | ---
150039<br></br>**Meldung**: Der Azure-Datenträger (DiskName) (DiskURI) mit der logischen Gerätenummer (LUN) (LUNValue) wurde keinem entsprechenden Datenträger zugeordnet, der vom virtuellen Computer gemeldet wird und denselben LUN-Wert aufweist. | - Es wurde ein neuer Datenträger an den virtuellen Computer angefügt, aber der Datenträger wurde nicht initialisiert.</br></br>- Der Datenträger innerhalb des virtuellen Computers meldet nicht den richtigen LUN-Wert, mit dem der Datenträger an den virtuellen Computer angefügt wurde.| Stellen Sie sicher, dass die Datenträger initialisiert wurden, und wiederholen Sie den Vorgang.</br></br>Für Windows: [Anfügen und Initialisieren eines neuen Datenträgers](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal)</br></br>Für Linux: [Initialisieren eines neuen Datenträgers unter Linux](https://docs.microsoft.com/azure/virtual-machines/linux/add-disk)

### <a name="fix-the-problem"></a>Beheben des Problems
Stellen Sie sicher, dass die Datenträger initialisiert wurden, und wiederholen Sie den Vorgang:

- Für Windows: [Anfügen und Initialisieren eines neuen Datenträgers](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal)
- Für Linux: [Hinzufügen eines neuen Datenträgers in Linux](https://docs.microsoft.com/azure/virtual-machines/linux/add-disk)

Wenden Sie sich an den Support, wenn das Problem weiterhin besteht.


## <a name="unable-to-see-the-azure-vm-for-selection-in-enable-replication"></a>Azure-VM wird für „Replikation aktivieren“ nicht zur Auswahl angezeigt

Wird der virtuelle Computer, für den Sie die Replikation aktivieren möchten, nicht angezeigt, ist die Ursache unter Umständen eine veraltete Site Recovery-Konfiguration auf dem virtuellen Azure-Computer. Die veraltete Konfiguration kann in den folgenden Fällen auf einer Azure-VM verbleiben:

- Sie haben die Replikation für die Azure-VM mithilfe von Site Recovery aktiviert und anschließend den Site Recovery-Tresor gelöscht, ohne die Replikation auf der VM explizit zu deaktivieren.
- Sie haben die Replikation für die Azure-VM mithilfe von Site Recovery aktiviert und anschließend die Ressourcengruppe mit dem Site Recovery-Tresor gelöscht, ohne die Replikation auf der VM explizit zu deaktivieren.

### <a name="fix-the-problem"></a>Beheben des Problems

Sie können das [Skript zum Entfernen veralteter ASR-Konfigurationen](https://gallery.technet.microsoft.com/Azure-Recovery-ASR-script-3a93f412) verwenden und die veraltete Site Recovery-Konfiguration auf der Azure-VM entfernen. Nach dem Entfernen der veralteten Konfiguration sollte der virtuelle Computer angezeigt werden.

## <a name="vms-provisioning-state-is-not-valid-error-code-150019"></a>Der Bereitstellungsstatus des virtuellen Computers ist ungültig (Fehlercode 150019)

Um die Replikation auf dem virtuellen Computer zu aktivieren, muss der Bereitstellungsstatus **Erfolgreich** lauten. Sie können den Status des virtuellen Computers überprüfen, indem Sie die nachstehenden Schritte ausführen.

1.  Wählen Sie unter **Alle Dienste** im Azure-Portal **Ressourcen-Explorer** aus.
2.  Erweitern Sie die Liste **Abonnements**, und wählen Sie Ihr Abonnement aus.
3.  Erweitern Sie die Liste **Ressourcengruppen**, und wählen Sie die Ressourcengruppe des virtuellen Computers aus.
4.  Erweitern Sie die Liste **Ressourcen**, und wählen Sie den virtuellen Computer aus.
5.  Überprüfen Sie das Feld **provisioningState** in der Instanzansicht rechts.

### <a name="fix-the-problem"></a>Beheben des Problems

- Wenn **provisioningState** den Status **Fehler** aufweist, wenden Sie sich mit den Informationen zur Problembehandlung an den Support.
- Wenn **provisioningState** den Status **Aktualisieren** aufweist, wird möglicherweise gerade eine andere Erweiterung bereitgestellt. Überprüfen Sie, ob gerade Vorgänge mit dem virtuellen Computer ausgeführt werden, warten Sie, bis diese abgeschlossen sind, und wiederholen Sie dann den Auftrag **Replikation aktivieren** für die Sitewiederherstellung.

## <a name="next-steps"></a>Nächste Schritte
[Replizieren virtueller Azure-Computer](site-recovery-replicate-azure-to-azure.md)
