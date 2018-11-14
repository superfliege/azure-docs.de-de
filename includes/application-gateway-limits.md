| Ressource | Standardlimit | Hinweis |
| --- | --- | --- |
| Application Gateway |50 pro Abonnement | Maximal 1.000 |
| Front-End-IP-Konfiguration |2 |1 öffentliche und 1 private |
| Front-End-Ports |20 | |
| Back-End-Adresspools |20 | |
| Back-End-Server pro Pool |100 | |
| HTTP-Listener |20 | |
| HTTP-Lastenausgleichsregeln |200 |Anzahl der HTTP-Listener * n, Standard: n = 10 |
| Back-End-HTTP-Einstellungen |20 |1 pro Back-End-Adresspool |
| Instanzen pro Gateway |10 | Wenn Sie weitere Instanzen benötigen, erstellen Sie ein Supportticket. |
| SSL-Zertifikate |20 |1 pro HTTP-Listener |
| Authentifizierungszertifikate |5 | Maximal 10 |
| Anforderungstimeout min. |1 Sekunde | |
| Anforderungstimeout max. |24 Stunden | |
| Anzahl der Websites |20 |1 pro HTTP-Listener |
| URL-Zuordnungen pro Listener |1 | |
| Gleichzeitige WebSocket-Verbindungen |5.000| |
|Maximale URL-Länge|8.000|
| Maximale Dateiuploadgröße (Standard) |2 GB | |
| Maximale Dateiuploadgröße (WAF) |Mittlere WAF-Gateways – 100 MB<br>Große WAF-Gateways – 500 MB| |
|WAF-Textgrößenbeschränkung (ohne Dateien)|128 KB|

