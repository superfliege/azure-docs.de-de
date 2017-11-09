| Ressource | Basic | Standard | Premium |
|---|---|---|---|---|
| Speicher | 10 GiB | 100 GiB| 500 GiB |
| Lesevorgänge pro Minute<sup>1, 2</sup> | 1k | 300k | 10.000k |
| Schreibvorgänge pro Minute<sup>1, 3</sup> | 100 | 500 | 2k |
| Downloadbandbreite MBit/s<sup>1</sup> | 30 | 60 | 100 |
| Uploadbandbreite MBit/s<sup>1</sup> | 10 | 20 | 50 |
| Webhooks | 2 | 10 | 100 |
| Georeplikation | N/V | N/V | [Unterstützt *(Vorschauversion)*](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication) |

<sup>1</sup> *Lesevorgänge*, *Schreibvorgänge* und *Bandbreite* sind die geschätzten Mindestwerte. ACR soll die Leistung der Nutzung entsprechend verbessern.

<sup>2</sup> [docker pull](https://docs.docker.com/registry/spec/api/#pulling-an-image) wird auf Grundlage der Zahl an Ebenen im Image sowie des Manifestabrufs in mehrere Lesevorgänge übersetzt.

<sup>3</sup> [docker push](https://docs.docker.com/registry/spec/api/#pushing-an-image) wird auf Grundlage der Zahl an Ebenen, die per Push übertragen werden müssen, in mehrere Schreibvorgänge übersetzt. Ein `docker push` enthält *Lesevorgänge* zum Abrufen eines Manifests für ein vorhandenes Image.