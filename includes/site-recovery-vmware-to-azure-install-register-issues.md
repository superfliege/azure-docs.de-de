
### <a name="installation-failures"></a>Fehler bei der Installation
| **Beispiel-Fehlermeldung** | **Empfohlene Aktion** |
|--------------------------|------------------------|
|Fehler-Fehler beim Laden von Konten. Fehler: System.IO.IOException: kann nicht zum Lesen von Daten von der übertragungsverbindung beim Installieren und registrieren die CS-Server.| Stellen Sie sicher, dass TLS 1.0 auf dem Computer aktiviert ist. |

### <a name="registration-failures"></a>Fehler bei der Registrierung
Fehler bei der Registrierung können anhand der Protokolle in gedebuggt werden die **%ProgramData%\ASRLogs** Ordner.

| **Beispiel-Fehlermeldung** | **Empfohlene Aktion** |
|--------------------------|------------------------|
|**09:20:06**: InnerException.Type: SrsRestApiClientLib.AcsException,InnerException.<br>Nachricht: ACS50008: SAML-Token ist ungültig.<br>Ablaufverfolgungs-ID: 1921ea5b-4723-4be7-8087-a75d3f9e1072<br>Korrelations-ID: 62fea7e6-2197-4be4-a2c0-71ceb7aa2d97 ><br>Zeitstempel: **2016-12-12 14:50:08Z<br>** | Stellen Sie sicher, dass die Zeit auf Ihre Systemuhr nicht mehr als 15 Minuten deaktiviert die Ortszeit ist. Führen Sie das Installationsprogramm aus, um die Registrierung abschließen erneut aus.|
|**09:35:27** : für das ausgewählte Zertifikat DRRegistrationException beim Abrufen aller Disaster Recovery-Tresor:: Exception.Type:Microsoft.DisasterRecovery.Registration.DRRegistrationException ausgelöst hat, Exception.Message: ACS50008: SAML-Token ist ungültig.<br>Ablaufverfolgungs-ID: e5ad1af1-2d39-4970-8eef-096e325c9950<br>Korrelations-ID: abe9deb8-3e64-464d-8375-36db9816427a<br>Zeitstempel: **2016-05-19 01:35:39Z**<br> | Stellen Sie sicher, dass die Zeit auf Ihre Systemuhr nicht mehr als 15 Minuten deaktiviert die Ortszeit ist. Führen Sie das Installationsprogramm aus, um die Registrierung abschließen erneut aus.|
|06:28:45: Fehler beim Erstellen des Zertifikats<br>06:28:45:Setup kann nicht fortgesetzt werden. Ein Zertifikat erforderlich, um die Authentifizierung für Site Recovery kann nicht erstellt werden. Führen Sie Setup erneut aus. | Stellen Sie sicher, dass Sie Setup als lokaler Administrator ausgeführt werden. |
