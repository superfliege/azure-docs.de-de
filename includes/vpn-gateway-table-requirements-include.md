In der folgenden Tabelle werden die Anforderungen für PolicyBased und RouteBased VPN-Gateways aufgeführt. Diese Tabelle gilt für den Ressourcen-Manager und den klassischen Bereitstellungsmodelle. Das klassische Modell PolicyBased VPN-Gateways sind identisch mit statischen Gateways und Gateways weiterleitungsbasierte entsprechen den dynamischen Gateways.

|  | **PolicyBased Basic VPN-Gateway** | **Die VPN-Gateway als routebased festgelegt Basic** | **Als routebased festgelegt Standard-VPN-Gateway** | **Als routebased festgelegt hohe Leistung VPN-Gateway** |
| --- | --- | --- | --- | --- |
| **Standort-zu-Standort-Konnektivität (S2S)** |PolicyBased VPN-Konfiguration |RouteBased VPN-Konfiguration |RouteBased VPN-Konfiguration |RouteBased VPN-Konfiguration |
| **Punkt-zu-Standort-Konnektivität (P2S**) |Nicht unterstützt |Unterstützt (kann zusammen mit S2S) |Unterstützt (kann zusammen mit S2S) |Unterstützt (kann zusammen mit S2S) |
| **Authentifizierungsmethode** |Vorinstallierter Schlüssel |Vorinstallierten Schlüssel für die S2S-Konnektivität, Zertifikate für die P2S-Konnektivität |Vorinstallierten Schlüssel für die S2S-Konnektivität, Zertifikate für die P2S-Konnektivität |Vorinstallierten Schlüssel für die S2S-Konnektivität, Zertifikate für die P2S-Konnektivität |
| **Maximale Anzahl von S2S-Verbindungen** |1 |10 |10 |30 |
| **Maximale Anzahl von P2S-Verbindungen** |Nicht unterstützt |128 |128 |128 |
| **Aktive routing-Unterstützung (BGP)** |Nicht unterstützt |Nicht unterstützt |Unterstützt |Unterstützt |

