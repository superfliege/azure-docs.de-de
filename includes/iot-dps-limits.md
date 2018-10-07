Die folgende Tabelle enthält die Grenzwerte, die für Ressourcen des IoT Hub Device Provisioning-Diensts gelten:

| Ressource | Begrenzung |
| --- | --- |
| Maximale Device Provisioning-Dienste pro Azure-Abonnement | 10 |
| Maximale Anzahl von Registrierungen (Enrollments) | 500.000 |
| Maximale Anzahl von Registrierungen (Registrations) | 500.000 |
| Maximale Anzahl von Registrierungsgruppen | 100 |
| Maximale Anzahl von Zertifizierungsstellen | 25 |

> [!NOTE]
> Sie können sich an den [Microsoft-Support](https://azure.microsoft.com/support/options/) wenden, um die Anzahl von Instanzen in Ihrem Abonnement erhöhen zu lassen.

> [!NOTE]
> Sie können sich an den [Microsoft-Support](https://azure.microsoft.com/support/options/) wenden, um die Anzahl der Registrierungen für Ihren Bereitstellungsdienst zu erhöhen.

Der Device Provisioning-Dienst drosselt Anforderungen, wenn die folgenden Kontingente überschritten werden:

| Drosselung | Wert pro Einheit |
| --- | --- |
| Vorgänge | 200/Minute/Dienst |
| Geräteregistrierungen | 200/Minute/Dienst |
| Abrufvorgang für Geräte | 5/10 Sekunden/Gerät |
