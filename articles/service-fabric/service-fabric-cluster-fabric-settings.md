---
title: Ändern von Azure Service Fabric-Clustereinstellungen | Microsoft-Dokumentation
description: Dieser Artikel beschreibt die Fabric-Einstellungen und Fabric-Upgraderichtlinien, die Sie anpassen können.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: ''
ms.assetid: 7ced36bf-bd3f-474f-a03a-6ebdbc9677e2
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 1/09/2018
ms.author: aljo
ms.openlocfilehash: d2454b09e69c26317499d17ea89b6829f52dedd2
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="customize-service-fabric-cluster-settings-and-fabric-upgrade-policy"></a>Anpassen von Service Fabric-Clustereinstellungen und der Fabric-Upgraderichtlinie
In diesem Dokument erfahren Sie, wie Sie die verschiedenen Fabric-Einstellungen und die Fabric-Upgraderichtlinie für Ihren Service Fabric-Cluster anpassen. Die Anpassungen können über das [Azure-Portal](https://portal.azure.com) oder mithilfe einer Azure Resource Manager-Vorlage vorgenommen werden.

> [!NOTE]
> Im Portal sind nicht alle Einstellungen verfügbar. Falls eine der unten aufgeführten Einstellungen nicht über das Portal verfügbar sein sollte, passen Sie dies mithilfe einer Azure Resource Manager-Vorlage an.
> 

## <a name="customize-cluster-settings-using-resource-manager-templates"></a>Anpassen von Clustereinstellungen mithilfe von Resource Manager-Vorlagen
In den folgenden Schritten wird beschrieben, wie die neue Einstellung *MaxDiskQuotaInMB* zum Abschnitt *Diagnose* hinzugefügt wird.

1. Besuchen Sie https://resources.azure.com.
2. Navigieren Sie zu Ihrem Abonnement. Erweitern Sie dazu **Abonnements** -> **\<Ihr Abonnement>** -> **Ressourcengruppen** -> **\<Ihre Ressourcengruppe>** -> **Anbieter** -> **Microsoft.ServiceFabric** -> **Cluster** -> **\<Name Ihres Clusters>**.
3. Wählen Sie in der oberen rechten Ecke **Lesen/Schreiben** aus.
4. Wählen Sie **Bearbeiten** aus, aktualisieren Sie das JSON-Element `fabricSettings`, und fügen Sie ein neues Element hinzu:

```
      {
        "name": "Diagnostics",
        "parameters": [
          {
            "name": "MaxDiskQuotaInMB",
            "value": "65536"
          }
        ]
      }
```

In der folgenden Liste sind, zusammengestellt nach Abschnitt, die Fabric-Einstellungen aufgeführt, die Sie anpassen können.

## <a name="applicationgatewayhttp"></a>ApplicationGateway/Http
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|ApplicationCertificateValidationPolicy|string, Standardwert L"None"|statischen| ApplicationCertificateValidationPolicy: None: Serverzertifikat nicht überprüfen, Anforderung ist erfolgreich. ServiceCertificateThumbprints: Die durch Kommas getrennte Liste der Fingerabdrücke der Remotezertifikate, denen der Reverseproxy vertrauen kann, finden Sie unter „config ServiceCertificateThumbprints“. ServiceCommonNameAndIssuer: Den Antragstellernamen und den Ausstellerfingerabdruck des Remotezertifikats, denen der Reverseproxy vertrauen kann, finden Sie unter „config ServiceCommonNameAndIssuer“. |
|BodyChunkSize |Uint, Standardwert 16384 |Dynamisch| Gibt die Größe des Blocks in Bytes an, der zum Lesen des Texts verwendet wird. |
|CrlCheckingFlag|uint, Standardwert 0 x 40000000 |Dynamisch| Flags für die Überprüfung der Anwendungs- bzw. Dienstzertifikatkette, z.B. Zertifikatssperrlistenüberprüfung 0x10000000 CERT_CHAIN_REVOCATION_CHECK_END_CERT 0x20000000 CERT_CHAIN_REVOCATION_CHECK_CHAIN 0x40000000 CERT_CHAIN_REVOCATION_CHECK_CHAIN_EXCLUDE_ROOT 0x80000000 CERT_CHAIN_REVOCATION_CHECK_CACHE_ONLY. Bei Festlegen des Werts auf 0 wird die Zertifikatssperrlistenüberprüfung deaktiviert. Die vollständige Liste der unterstützten Werte wird durch dwFlags von CertGetCertificateChain dokumentiert: http://msdn.microsoft.com/library/windows/desktop/aa376078(v=vs.85).aspx  |
|DefaultHttpRequestTimeout |Zeit in Sekunden. Standardwert 120 |Dynamisch|Geben Sie die Zeitspanne in Sekunden an.  Gibt das standardmäßige Anforderungstimeout für die HTTP-Anforderungen an, die im HTTP-App-Gateway verarbeitet werden. |
|ForwardClientCertificate|Boolesch, Standardwert FALSE|Dynamisch| |
|GatewayAuthCredentialType |string, Standardwert „None“ |statischen| Gibt den Typ der Sicherheitsanmeldeinformationen an, die am HTTP-App-Gatewayendpunkt verwendet werden sollen. Gültige Werte sind None/X509. |
|GatewayX509CertificateFindType |string, Standardwert „FindByThumbprint“ |Dynamisch| Gibt an, wie nach dem Zertifikat im durch GatewayX509CertificateStoreName angegebenen Speicher gesucht werden soll. Unterstützte Werte: FindByThumbprint, FindBySubjectName. |
|GatewayX509CertificateFindValue | string, Standardwert "" |Dynamisch| Suchfilterwert, der zum Suchen des HTTP-App-Gatewayzertifikats verwendet wird. Dieses Zertifikat ist für den HTTPS-Endpunkt konfiguriert und kann bei Bedarf auch von den Diensten zum Überprüfen der Identität der App verwendet werden. Nach FindValue wird zuerst gesucht. Wenn nicht vorhanden, wird nach FindValueSecondary gesucht. |
|GatewayX509CertificateFindValueSecondary | string, Standardwert "" |Dynamisch|Suchfilterwert, der zum Suchen des HTTP-App-Gatewayzertifikats verwendet wird. Dieses Zertifikat ist für den HTTPS-Endpunkt konfiguriert und kann bei Bedarf auch von den Diensten zum Überprüfen der Identität der App verwendet werden. Nach FindValue wird zuerst gesucht. Wenn nicht vorhanden, wird nach FindValueSecondary gesucht.|
|GatewayX509CertificateStoreName |string, Standardwert „My“ |Dynamisch| Name des X.509-Zertifikatspeichers, der das Zertifikat für das HTTP-App-Gateway enthält. |
|HttpRequestConnectTimeout|TimeSpan, Standardwert Common::TimeSpan::FromSeconds(5)|Dynamisch|Geben Sie die Zeitspanne in Sekunden an.  Gibt das Verbindungstimeout für die HTTP-Anforderungen an, die vom HTTP-App-Gateway gesendet werden.  |
|IgnoreCrlOfflineError|Boolesch, Standardwert TRUE|Dynamisch|Gibt an, ob ein Zertifikatsperrlisten-Offlinefehler bei der Überprüfung des Anwendungs- bzw. Dienstzertifikats ignoriert werden soll. |
|IsEnabled |Boolesch, Standardwert „false“ |statischen| Aktiviert bzw. deaktiviert HttpApplicationGateway. HttpApplicationGateway ist standardmäßig deaktiviert, und diese Konfiguration muss zur Aktivierung festgelegt werden. |
|NumberOfParallelOperations | Uint, Standardwert 5000 |statischen|Anzahl der Lesevorgänge, die an die HTTP-Serverwarteschlange gesendet werden sollen. Dadurch wird die Anzahl gleichzeitiger Anforderungen gesteuert, die von HttpGateway erfüllt werden können. |
|RemoveServiceResponseHeaders|string, Standardwert L "Date; Server"|statischen|Durch Semikolons/Kommas getrennte Liste mit Antwortheadern, die aus der Dienstantwort entfernt werden, bevor die Weiterleitung an den Client erfolgt. Wenn dieser Wert auf eine leere Zeichenfolge festgelegt ist, werden alle vom Dienst zurückgegebenen Header unverändert übergeben. z.B.: Datum und die Server nicht überschreiben |
|ResolveServiceBackoffInterval |Zeit in Sekunden, Standardwert 5 |Dynamisch|Geben Sie die Zeitspanne in Sekunden an.  Gibt das standardmäßige Backoffintervall an, nach dem ein fehlerhafter Vorgang zum Auflösen von Diensten wiederholt wird. |
|SecureOnlyMode|Boolesch, Standardwert FALSE|Dynamisch| SecureOnlyMode: true: Reverseproxy leitet nur an Dienste weiter, die sichere Endpunkte veröffentlichen. false: Reverseproxy kann Anforderungen an sichere/nicht sichere Endpunkte weiterleiten.  |
|ServiceCertificateThumbprints|string, Standardwert L""|Dynamisch| |

## <a name="applicationgatewayhttpservicecommonnameandissuer"></a>ApplicationGateway/Http/ServiceCommonNameAndIssuer
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|PropertyGroup|X509NameMap, Standardwert None|Dynamisch|  |

## <a name="clustermanager"></a>ClusterManager
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|EnableDefaultServicesUpgrade | Boolesch, Standardwert „false“ |Dynamisch|Aktivieren Sie das Aktualisieren von Standarddiensten während des Anwendungsupgrades. Standarddienstbeschreibungen würden nach dem Upgrade überschrieben werden. |
|FabricUpgradeHealthCheckInterval |Zeit in Sekunden, Standardwert 60 |Dynamisch|Die Häufigkeit der Integritätsstatusprüfungen während eines überwachten Fabric-Upgrades |
|FabricUpgradeStatusPollInterval |Zeit in Sekunden, Standardwert 60 |Dynamisch|Die Häufigkeit des Abrufs des Upgradestatus der Fabric. Dieser Wert bestimmt die Aktualisierungsrate für Aufrufe von „GetFabricUpgradeProgress“ |
|ImageBuilderTimeoutBuffer |Zeit in Sekunden, Standardwert 3 |Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Die zulässige Zeitspanne für die Rückgabe von Image Builder-Timeoutfehlern an den Client. Wenn dieser Puffer zu klein ist, tritt auf dem Client ein Timeout ein, bevor dies auf dem Server der Fall ist, sodass sich ein generischer Timeoutfehler auf dem Client ergibt. |
|InfrastructureTaskHealthCheckRetryTimeout | Zeit in Sekunden, Standardwert 60 |Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Die Zeitspanne für Wiederholungen fehlerhafter Integritätsprüfungen während der Nachbearbeitung einer Infrastrukturaufgabe. Durch die Beobachtung einer erfolgreichen Integritätsprüfung wird dieser Timer zurückgesetzt. |
|InfrastructureTaskHealthCheckStableDuration | Zeit in Sekunden, Standardwert 0|Dynamisch| Geben Sie die Zeitspanne in Sekunden an. Die Zeitspanne, die aufeinanderfolgende erfolgreiche Integritätsprüfungen beobachtet werden, bevor die Nachbearbeitung einer Infrastrukturaufgabe erfolgreich abgeschlossen ist. Durch die Beobachtung einer fehlerhaften Integritätsprüfung wird dieser Timer zurückgesetzt. |
|InfrastructureTaskHealthCheckWaitDuration |Zeit in Sekunden, Standardwert 0|Dynamisch| Geben Sie die Zeitspanne in Sekunden an. Die Zeitspanne, die vor dem Starten von Integritätsprüfungen nach der Nachbearbeitung einer Infrastrukturaufgabe gewartet wird. |
|InfrastructureTaskProcessingInterval | Zeit in Sekunden, Standardwert 10 |Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Das Verarbeitungsintervall, das von der Infrastrukturaufgabe verwendet wird, die den Zustandsautomaten verarbeitet. |
|MaxCommunicationTimeout |Zeit in Sekunden, Standardwert 600 |Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Das maximale Zeitlimit für die interne Kommunikation zwischen ClusterManager und anderen Systemdiensten (wie Naming Service, Failover-Manager usw.). Dieses Zeitlimit muss kleiner sein als der globale MaxOperationTimeout-Wert (da mehrere Kommunikationen zwischen Systemkomponenten für jeden Clientvorgang möglich sind). |
|MaxDataMigrationTimeout |Zeit in Sekunden, Standardwert 600 |Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Das maximale Zeitlimit für Wiederherstellungsvorgänge nach der Datenmigration nach einer Fabric-Aktualisierung. |
|MaxOperationRetryDelay |Zeit in Sekunden, Standardwert 5|Dynamisch| Geben Sie die Zeitspanne in Sekunden an. Die maximale Verzögerung für interne Wiederholungen, wenn Fehler gefunden werden. |
|MaxOperationTimeout |Zeit in Sekunden, Standardwert MaxValue |Dynamisch| Geben Sie die Zeitspanne in Sekunden an. Das maximale globale Zeitlimit für die interne Verarbeitung von Vorgängen für ClusterManager. |
|MaxTimeoutRetryBuffer | Zeit in Sekunden, Standardwert 600 |Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Das maximale Vorgangszeitlimit für die interne Wiederholung aufgrund von Zeitlimits ist <Original Time out> + <MaxTimeoutRetryBuffer>. Ein zusätzliches Zeitlimit wird in Schritten von MinOperationTimeout hinzugefügt. |
|MinOperationTimeout | Zeit in Sekunden, Standardwert 60 |Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Das minimale globale Zeitlimit für die interne Verarbeitung von Vorgängen für ClusterManager. |
|MinReplicaSetSize |Ganze Zahl, Standardwert 3 |Nicht zulässig|MinReplicaSetSize für ClusterManager. |
|PlacementConstraints | string, Standardwert "" |Nicht zulässig|PlacementConstraints für ClusterManager. |
|QuorumLossWaitDuration |Zeit in Sekunden, Standardwert MaxValue |Nicht zulässig| Geben Sie die Zeitspanne in Sekunden an. QuorumLossWaitDuration für ClusterManager. |
|ReplicaRestartWaitDuration |Zeit in Sekunden, Standardwert (60,0 * 30)|Nicht zulässig|Geben Sie die Zeitspanne in Sekunden an. ReplicaRestartWaitDuration für ClusterManager. |
|ReplicaSetCheckTimeoutRollbackOverride |Zeit in Sekunden, Standardwert 1200 |Dynamisch| Geben Sie die Zeitspanne in Sekunden an. Wenn ReplicaSetCheckTimeout auf den maximalen DWORD-Wert festgelegt ist, wird dies mit dem Wert dieser Konfiguration für Rollbacks überschrieben. Der Wert für Rollforwards wird nie überschrieben. |
|SkipRollbackUpdateDefaultService | Boolesch, Standardwert „false“ |Dynamisch|CM überspringt das Zurücksetzen der aktualisierten Standarddienste während des Rollbacks eines Anwendungsupgrades. |
|StandByReplicaKeepDuration | Zeit in Sekunden, Standardwert (3600,0 * 2)|Nicht zulässig|Geben Sie die Zeitspanne in Sekunden an. StandByReplicaKeepDuration für ClusterManager. |
|TargetReplicaSetSize |Ganze Zahl, Standardwert 7 |Nicht zulässig|TargetReplicaSetSize für ClusterManager. |
|UpgradeHealthCheckInterval |Zeit in Sekunden, Standardwert 60 |Dynamisch|Die Häufigkeit der Integritätsstatusprüfungen während eines überwachten Anwendungsupgrades |
|UpgradeStatusPollInterval |Zeit in Sekunden, Standardwert 60 |Dynamisch|Die Häufigkeit des Abrufs des Upgradestatus der Anwendung. Dieser Wert bestimmt die Aktualisierungsrate für Aufrufe von „GetApplicationUpgradeProgress“ |

## <a name="common"></a>Common
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|PerfMonitorInterval |Zeit in Sekunden, Standardwert 1 |Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Intervall für die Leistungsüberwachung. Mit 0 oder einem negativen Wert wird die Überwachung deaktiviert. |

## <a name="defragmentationemptynodedistributionpolicy"></a>DefragmentationEmptyNodeDistributionPolicy
| **Parameter** | **Zulässige Werte** |**Upgraderichtlinie**| **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|PropertyGroup|KeyIntegerValueMap, Standardwert None|Dynamisch|Gibt an, dass die Richtliniendefragmentierung beim Leeren von Knoten folgt. Für eine bestimmte Metrik gibt 0 an, dass SF versuchen soll, die Knoten gleichmäßig auf UDs und FDs zu defragmentieren. 1 gibt nur an, dass die Knoten defragmentiert werden müssen. |

## <a name="defragmentationmetrics"></a>DefragmentationMetrics
| **Parameter** | **Zulässige Werte** |**Upgraderichtlinie**| **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|PropertyGroup|KeyBoolValueMap, Standardwert None|Dynamisch|Bestimmt die Sammlung von Metriken, die für die Defragmentierung und nicht für den Lastenausgleich verwendet werden soll. |

## <a name="defragmentationmetricspercentornumberofemptynodestriggeringthreshold"></a>DefragmentationMetricsPercentOrNumberOfEmptyNodesTriggeringThreshold
| **Parameter** | **Zulässige Werte** |**Upgraderichtlinie**| **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|PropertyGroup|KeyDoubleValueMap, Standardwert None|Dynamisch|Bestimmt die Anzahl der freien Knoten, die erforderlich sind, um den Cluster als defragmentiert zu betrachten. Die Angabe erfolgt als Prozent im Bereich [0,0 - 1,0) oder als Anzahl der leeren Knoten als Zahl >= 1,0. |

## <a name="diagnostics"></a>Diagnose
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|AppDiagnosticStoreAccessRequiresImpersonation |Boolesch, Standardwert „true“ | Dynamisch |Gibt an, ob ein Identitätswechsel erforderlich ist, wenn für die Anwendung auf Diagnosespeicher zugegriffen wird. |
|AppEtwTraceDeletionAgeInDays |Ganze Zahl, Standardwert 3 | Dynamisch |Anzahl der Tage, nach denen wir alte ETL-Dateien löschen, die ETW-Ablaufverfolgungen von Anwendungen enthalten. |
|ApplicationLogsFormatVersion |Ganze Zahl, Standardwert 0 | Dynamisch |Version für das Format der Anwendungsprotokolle. Unterstützte Werte sind 0 und 1. Version 1 umfasst mehr Felder aus dem ETW-Ereignisdatensatz als Version 0. |
|ClusterId |Zeichenfolge | Dynamisch |Die eindeutige ID des Clusters. Diese wird generiert, wenn der Cluster erstellt wird. |
|ConsumerInstances |Zeichenfolge | Dynamisch |Die Liste der DCA-Consumerinstanzen. |
|DiskFullSafetySpaceInMB |Ganze Zahl, Standardwert 1024 | Dynamisch |Verbleibender Speicherplatz in MB, der vor der Verwendung durch DCA geschützt werden soll. |
|EnableCircularTraceSession |Boolesch, Standardwert „false“ | statischen |Das Flag gibt an, ob zirkuläre Ablaufverfolgungssitzungen verwendet werden sollen. |
|EnableTelemetry |Boolesch, Standardwert „true“ | Dynamisch |Damit werden Telemetriedaten aktiviert oder deaktiviert. |
|MaxDiskQuotaInMB |Ganze Zahl, Standardwert 65536 | Dynamisch |Datenträgerkontingent in MB für Windows Fabric-Protokolldateien. |
|ProducerInstances |Zeichenfolge | Dynamisch |Die Liste der DCA-Producerinstanzen. |

## <a name="dnsservice"></a>DnsService
| **Parameter** | **Zulässige Werte** |**Upgraderichtlinie**| **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|IsEnabled|Boolesch, Standardwert FALSE|statischen| |
|InstanceCount|Ganze Zahl, Standardwert -1|statischen|  |

## <a name="fabricclient"></a>FabricClient
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|ConnectionInitializationTimeout |Zeit in Sekunden, Standardwert 2 |Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Timeoutintervall für Verbindungen für jeden Versuch eines Clients, eine Verbindung mit dem Gateway zu öffnen.|
|HealthOperationTimeout |Zeit in Sekunden, Standardwert 120 |Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Das Timeout für eine an den Integritätsdienst gesendete Berichtsnachricht. |
|HealthReportRetrySendInterval |Zeit in Sekunden, Standardwert 30 |Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Das Intervall, in dem Berichtskomponenten kumulierte Integritätsberichte erneut an den Integritätsdienst senden. |
|HealthReportSendInterval |Zeit in Sekunden, Standardwert 30 |Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Das Intervall, in dem Berichtskomponenten kumulierte Integritätsberichte an den Integritätsdienst senden. |
|KeepAliveIntervalInSeconds |Ganze Zahl, Standardwert 20 |statischen|Das Intervall, in dem der FabricClient-Transport Keep-Alive-Nachrichten an das Gateway sendet. Bei 0 ist „keepAlive“ deaktiviert. Der Wert muss eine positive Zahl sein. |
|MaxFileSenderThreads |Uint, Standardwert 10 |statischen|Die maximale Anzahl von Dateien, die gleichzeitig übertragen werden. |
|NodeAddresses |string, Standardwert "" |statischen|Eine Sammlung von Adressen (Verbindungszeichenfolgen) auf unterschiedlichen Knoten, die für die Kommunikation mit dem Naming Service verwendet werden können. Anfangs stellt der Client eine Verbindung her, indem er eine der Adressen nach dem Zufallsprinzip auswählt. Wenn mehr als eine Verbindungszeichenfolge angegeben wird und eine Verbindung aufgrund eines Kommunikations- oder Timeoutfehlers nicht hergestellt werden kann, wechselt der Client zur nächsten Adresse in der Reihe. Details zur Semantik von Wiederholungsversuchen finden Sie im Abschnitt zu Wiederholungsversuchen für Naming Service-Adressen. |
|PartitionLocationCacheLimit |Ganze Zahl, Standardwert 100000 |statischen|Anzahl der für die Dienstlösung zwischengespeicherten Partitionen (bei 0 gilt keine Begrenzung). |
|RetryBackoffInterval |Zeit in Sekunden, Standardwert 3 |Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Das Backoffintervall vor der Wiederholung des Vorgangs. |
|ServiceChangePollInterval |Zeit in Sekunden, Standardwert 120 |Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Das Intervall zwischen aufeinanderfolgenden Abrufen von Dienständerungen vom Client an das Gateway für Benachrichtigungsrückrufe zu registrierten Dienständerungen. |

## <a name="fabrichost"></a>FabricHost
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|ActivationMaxFailureCount |Ganze Zahl, Standardwert 10 |Dynamisch|Dies ist die maximale Anzahl von Versuchen, die Aktivierung nach einem Fehler zu wiederholen, bevor aufgegeben wird. |
|ActivationMaxRetryInterval |Zeit in Sekunden, Standardwert 300 |Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Maximales Wiederholungsintervall für die Aktivierung. Bei jedem andauernden Fehler wird das Wiederholungsintervall wie folgt berechnet: Min( ActivationMaxRetryInterval; Anzahl andauernder Fehler * ActivationRetryBackoffInterval). |
|ActivationRetryBackoffInterval |Zeit in Sekunden, Standardwert 5 |Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Backoffintervall bei jedem Aktivierungsfehler. Bei jedem andauernden Aktivierungsfehler wiederholt das System die Aktivierung bis zum Wert von MaxActivationFailureCount. Das Wiederholungsintervall bei jedem Versuch ist das Produkt der Anzahl fortlaufender Aktivierungsfehler und des Backoffintervalls für die Aktivierung. |
|EnableRestartManagement |Boolesch, Standardwert „false“ |Dynamisch|Hiermit wird der Neustart des Servers aktiviert. |
|EnableServiceFabricAutomaticUpdates |Boolesch, Standardwert „false“ |Dynamisch|Hiermit werden automatische Fabricupdates über Windows Update aktiviert. |
|EnableServiceFabricBaseUpgrade |Boolesch, Standardwert „false“ |Dynamisch|Hiermit wird das Basisupdate für den Server aktiviert. |
|StartTimeout |Zeit in Sekunden, Standardwert 300 |Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Timeout für den Start von fabricactivationmanager. |
|StopTimeout |Zeit in Sekunden, Standardwert 300 |Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Das Timeout für die Aktivierung, Deaktivierung und Aktualisierung des gehosteten Diensts. |

## <a name="fabricnode"></a>FabricNode
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|ClientAuthX509FindType |string, Standardwert „FindByThumbprint“ |Dynamisch|Gibt an, wie nach dem Zertifikat im durch ClientAuthX509StoreName angegebenen Speicher gesucht werden soll. Unterstützte Werte: FindByThumbprint, FindBySubjectName. |
|ClientAuthX509FindValue |string, Standardwert "" | Dynamisch|Suchfilterwert, der zum Suchen des Zertifikats für die standardmäßige FabricClient-Administratorrolle verwendet wird. |
|ClientAuthX509FindValueSecondary |string, Standardwert "" |Dynamisch|Suchfilterwert, der zum Suchen des Zertifikats für die standardmäßige FabricClient-Administratorrolle verwendet wird. |
|ClientAuthX509StoreName |string, Standardwert „My“ |Dynamisch|Der Name des X.509-Zertifikatspeichers, der das Zertifikat für die standardmäßige FabricClient-Administratorrolle enthält. |
|ClusterX509FindType |string, Standardwert „FindByThumbprint“ |Dynamisch|Gibt an, wie nach dem Clusterzertifikat im durch ClusterX509StoreName angegebenen Speicher gesucht werden soll. Unterstützte Werte: „FindByThumbprint“, „FindBySubjectName“. Wenn es bei „FindBySubjectName“ mehrere Übereinstimmungen gibt, wird das Zertifikat mit dem längsten Ablaufdatum verwendet. |
|ClusterX509FindValue |string, Standardwert "" |Dynamisch|Suchfilterwert, der zum Suchen des Clusterzertifikats verwendet wird. |
|ClusterX509FindValueSecondary |string, Standardwert "" |Dynamisch|Suchfilterwert, der zum Suchen des Clusterzertifikats verwendet wird. |
|ClusterX509StoreName |string, Standardwert „My“ |Dynamisch|Der Name des X.509-Zertifikatspeichers, der das Clusterzertifikat zum Sichern der Kommunikation innerhalb des Clusters enthält. |
|EndApplicationPortRange |Ganze Zahl, Standardwert 0 |statischen|Ende (nicht inklusiv) der Anwendungsports, die vom Hostingsubsystem verwaltet werden. Erforderlich, wenn EndpointFilteringEnabled beim Hosting „true“ ist. |
|ServerAuthX509FindType |string, Standardwert „FindByThumbprint“ |Dynamisch|Gibt an, wie nach dem Serverzertifikat im durch ServerAuthX509StoreName angegebenen Speicher gesucht werden soll. Unterstützte Werte: FindByThumbprint, FindBySubjectName. |
|ServerAuthX509FindValue |string, Standardwert "" |Dynamisch|Suchfilterwert, der zum Suchen des Serverzertifikats verwendet wird. |
|ServerAuthX509FindValueSecondary |string, Standardwert "" |Dynamisch|Suchfilterwert, der zum Suchen des Serverzertifikats verwendet wird. |
|ServerAuthX509StoreName |string, Standardwert „My“ |Dynamisch|Name des X.509-Zertifikatspeichers, der das Serverzertifikat für den Zugangsdienst enthält. |
|StartApplicationPortRange |Ganze Zahl, Standardwert 0 |statischen|Start der Anwendungsports, die vom Hostingsubsystem verwaltet werden. Erforderlich, wenn EndpointFilteringEnabled beim Hosting „true“ ist. |
|StateTraceInterval |Zeit in Sekunden, Standardwert 300 |statischen|Geben Sie die Zeitspanne in Sekunden an. Das Intervall für die Ablaufverfolgung des Knotenstatus auf allen Knoten und aktiven Knoten in FM/FMM. |
|UserRoleClientX509FindType |string, Standardwert „FindByThumbprint“ |Dynamisch|Gibt an, wie nach dem Zertifikat im durch UserRoleClientX509StoreName angegebenen Speicher gesucht werden soll. Unterstützte Werte: FindByThumbprint, FindBySubjectName. |
|UserRoleClientX509FindValue |string, Standardwert "" |Dynamisch|Suchfilterwert, der zum Suchen des Zertifikats für die standardmäßige FabricClient-Benutzerrolle verwendet wird. |
|UserRoleClientX509FindValueSecondary |string, Standardwert "" |Dynamisch|Suchfilterwert, der zum Suchen des Zertifikats für die standardmäßige FabricClient-Benutzerrolle verwendet wird. |
|UserRoleClientX509StoreName |string, Standardwert „My“ |Dynamisch|Der Name des X.509-Zertifikatspeichers, der das Zertifikat für die standardmäßige FabricClient-Benutzerrolle enthält. |

## <a name="failovermanager"></a>FailoverManager
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|BuildReplicaTimeLimit|TimeSpan, Standardwert Common::TimeSpan::FromSeconds(3600)|Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Das Zeitlimit für das Erstellen eines zustandsbehafteten Replikats, nach dem ein Warnungsintegritätsbericht initiiert wird. |
|ClusterPauseThreshold|Ganze Zahl, Standardwert 1|Dynamisch|Wenn die Anzahl der Knoten im System diesen Wert unterschreitet, werden Platzierung, Lastenausgleich und Failover beendet. |
|CreateInstanceTimeLimit|TimeSpan, Standardwert Common::TimeSpan::FromSeconds(300)|Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Das Zeitlimit für das Erstellen einer zustandslosen Instanz, nach dem ein Warnungsintegritätsbericht initiiert wird. |
|ExpectedClusterSize|Ganze Zahl, Standardwert 1|Dynamisch|Beim ersten Start des Clusters wartet der FM darauf, dass sich diese Anzahl von Knoten meldet, bevor andere Dienste (einschließlich der Systemdienste wie Naming Service) platziert werden. Wenn Sie diesen Wert erhöhen, verlängert sich die Startzeit eines Clusters. Es wird aber verhindert, dass die frühen Knoten überlastet werden. Außerdem werden zusätzliche Verschiebungen verhindert, die erforderlich sind, wenn ein größere Anzahl von Knoten online geht. Dieser Wert sollte in der Regel auf einen kleinen Bruchteil der anfänglichen Clustergröße festgelegt werden. |
|ExpectedNodeDeactivationDuration|TimeSpan, Standardwert Common::TimeSpan::FromSeconds(60.0 * 30)|Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Dies ist die erwartete Zeitspanne, in der der Abschluss der Deaktivierung eines Knotens erwartet wird. |
|ExpectedNodeFabricUpgradeDuration|TimeSpan, Standardwert Common::TimeSpan::FromSeconds(60.0 * 30)|Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Dies ist die erwartete Dauer für die Aktualisierung eines Knotens während des Windows Fabric-Upgrades. |
|ExpectedReplicaUpgradeDuration|TimeSpan, Standardwert Common::TimeSpan::FromSeconds(60.0 * 30)|Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Dies ist die erwartete Dauer für die Aktualisierung aller Replikate auf einem Knoten während des Anwendungsupgrades. |
|IsSingletonReplicaMoveAllowedDuringUpgrade|Boolesch, Standardwert TRUE|Dynamisch|Wenn auf TRUE festgelegt, dürfen Replikate mit einer Zielgröße der Replikatgruppe von 1 während des Upgrades verschoben werden. |
|MinReplicaSetSize|Ganze Zahl, Standardwert 3|Nicht zulässig|Die Mindestgröße der Replikatgruppe für den FM. Wenn die Anzahl der aktiven FM-Replikate unter diesen Wert sinkt, weist der FM Änderungen am Cluster zurück, bis mindestens die minimale Anzahl der Replikate wiederhergestellt wurde. |
|PlacementConstraints|string, Standardwert L""|Nicht zulässig|Platzierungsbeschränkungen für die Replikate des Failover-Managers. |
|PlacementTimeLimit|TimeSpan, Standardwert Common::TimeSpan::FromSeconds(600)|Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Das Zeitlimit für das Erreichen der Zielanzahl von Replikaten, nach dem ein Warnungsintegritätsbericht initiiert wird. |
|QuorumLossWaitDuration |Zeit in Sekunden, Standardwert MaxValue |Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Dies ist die maximale Dauer, für die eine Partition in einem Zustand des Quorumverlusts sein darf. Wenn für die Partition nach Ablauf dieses Zeitraums noch ein Quorumsverlust vorliegt, wird die Partition aus dem Quorumsverlusts wiederhergestellt, indem die ausgefallenen Replikate als verloren angesehen werden. Beachten Sie, dass dies möglicherweise Datenverluste verursachen kann. |
|ReconfigurationTimeLimit|TimeSpan, Standardwert Common::TimeSpan::FromSeconds(300)|Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Das Zeitlimit für die Neukonfiguration, nach dem ein Warnungsintegritätsbericht initiiert wird. |
|ReplicaRestartWaitDuration|TimeSpan, Standardwert Common::TimeSpan::FromSeconds(60.0 * 30)|Nicht zulässig|Geben Sie die Zeitspanne in Sekunden an. Die ReplicaRestartWaitDuration für FMService. |
|StandByReplicaKeepDuration|TimeSpan, Standardwert Common::TimeSpan::FromSeconds(3600.0 * 24 * 7)|Nicht zulässig|Geben Sie die Zeitspanne in Sekunden an. Dies ist die StandByReplicaKeepDuration für den FMService. |
|TargetReplicaSetSize|Ganze Zahl, Standardwert 7|Nicht zulässig|Dies ist die Zielanzahl der FM-Replikate, die Windows Fabric verwaltet. Eine höhere Anzahl führt zu einer höheren Zuverlässigkeit der FM-Daten bei einer geringen Leistungsverschlechterung. |
|UserMaxStandByReplicaCount |Ganze Zahl, Standardwert 1 |Dynamisch|Die standardmäßige maximale Anzahl von Reservereplikaten, die das System für Benutzerdienste speichert. |
|UserReplicaRestartWaitDuration |Zeit in Sekunden, Standardwert 60,0 * 30 |Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Wenn ein dauerhaftes Replikat ausfällt, wartet Windows Fabric für diese Zeitspanne darauf, dass das Replikat wieder aktiviert wird, bevor neue Ersatzreplikate (die eine Kopie des Zustands erfordern würden) erstellt werden. |
|UserStandByReplicaKeepDuration |Zeit in Sekunden, Standardwert 3600,0 * 24 * 7 |Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Wenn ein dauerhaftes Replikat nach einem Ausfall wieder aktiv ist, wurde es möglicherweise bereits ersetzt. Dieser Timer bestimmt, wie lange FM das Reservereplikat beibehält, bevor es verworfen wird. |

## <a name="faultanalysisservice"></a>FaultAnalysisService
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|CompletedActionKeepDurationInSeconds | Ganze Zahl, Standardwert 604800 |statischen| Ungefähr für diese Dauer sollten Aktionen gespeichert werden, die in einem Endzustand sind. Dies hängt auch von StoredActionCleanupIntervalInSeconds ab, da die Bereinigung nur in diesem Intervall erfolgt. 604800 sind 7 Tage. |
|DataLossCheckPollIntervalInSeconds|Ganze Zahl, Standardwert 5|statischen|Dies ist der Zeitraum zwischen den Überprüfungen, die das System während des Wartens auf Datenverluste ausführt. Die Häufigkeit, mit der die Anzahl der Datenverluste pro interner Iteration überprüft wird, ist DataLossCheckWaitDurationInSeconds/this. |
|DataLossCheckWaitDurationInSeconds|Ganze Zahl, Standardwert 25|statischen|Die Gesamtzeit in Sekunden, die das System auf Datenverluste wartet. Dies wird intern verwendet, wenn die StartPartitionDataLossAsync()-API aufgerufen wird. |
|MinReplicaSetSize |Ganze Zahl, Standardwert 0 |statischen|MinReplicaSetSize für FaultAnalysisService. |
|PlacementConstraints | string, Standardwert ""|statischen| PlacementConstraints für FaultAnalysisService. |
|QuorumLossWaitDuration | Zeit in Sekunden, Standardwert MaxValue |statischen|Geben Sie die Zeitspanne in Sekunden an. QuorumLossWaitDuration für FaultAnalysisService. |
|ReplicaDropWaitDurationInSeconds|Ganze Zahl, Standardwert 600|statischen|Dieser Parameter wird verwendet, wenn die Datenverlust-API aufgerufen wird. Er steuert, wie lange das System wartet, bis ein Replikat gelöscht wird, nachdem intern „remove replica“ für das Replikat aufgerufen wurde. |
|ReplicaRestartWaitDuration |Zeit in Sekunden, Standardwert 60 Minuten|statischen|Geben Sie die Zeitspanne in Sekunden an. ReplicaRestartWaitDuration für FaultAnalysisService. |
|StandByReplicaKeepDuration| Zeit in Sekunden, Standardwert (60*24*7) Minuten |statischen|Geben Sie die Zeitspanne in Sekunden an. StandByReplicaKeepDuration für FaultAnalysisService. |
|StoredActionCleanupIntervalInSeconds | Ganze Zahl, Standardwert 3600 |statischen|Gibt an, wie häufig der Speicher bereinigt wird. Nur Aktionen in einem Endzustand und die mindestens vor CompletedActionKeepDurationInSeconds abgeschlossen wurden, werden entfernt. |
|StoredChaosEventCleanupIntervalInSeconds | Ganze Zahl, Standardwert 3600 |statischen|Mit dieser Häufigkeit wird der Speicher auf Bereinigungen überwacht. Wenn die Anzahl der Ereignisse größer als 30000 ist, wird die Bereinigung gestartet. |
|TargetReplicaSetSize |Ganze Zahl, Standardwert 0 |statischen|NOT_PLATFORM_UNIX_START, TargetReplicaSetSize für FaultAnalysisService. |

## <a name="federation"></a>Verbund
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|LeaseDuration |Zeit in Sekunden, Standardwert 30 |Dynamisch|Dauer einer Lease zwischen einem Knoten und seinen Nachbarn. |
|LeaseDurationAcrossFaultDomain |Zeit in Sekunden, Standardwert 30 |Dynamisch|Dauer einer fehlerdomänenübergreifenden Lease zwischen einem Knoten und seinen Nachbarn. |

## <a name="filestoreservice"></a>FileStoreService
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|AnonymousAccessEnabled | Boolesch, Standardwert „true“ |statischen|Aktivieren/deaktivieren Sie anonymen Zugriff auf die FileStoreService-Freigaben. |
|CommonName1Ntlmx509CommonName|string, Standardwert L""|statischen| Der allgemeine Name des X509-Zertifikats, der verwendet wird, um bei Verwendung der NTLM-Authentifizierung HMAC für CommonName1NtlmPasswordSecret zu generieren. |
|CommonName1Ntlmx509StoreLocation|string, Standardwert L"LocalMachine"|statischen|Der Speicherort des X509-Zertifikats, das verwendet wird, um bei Verwendung der NTLM-Authentifizierung HMAC für CommonName1NtlmPasswordSecret zu generieren. |
|CommonName1Ntlmx509StoreName|string, Standardwert L"MY"| statischen|Der Speichername des X509-Zertifikats, das verwendet wird, um bei Verwendung der NTLM-Authentifizierung HMAC für CommonName1NtlmPasswordSecret zu generieren. |
|CommonName2Ntlmx509CommonName|string, Standardwert L""|statischen|Der allgemeine Name des X509-Zertifikats, das verwendet wird, um bei Verwendung der NTLM-Authentifizierung HMAC für CommonName2NtlmPasswordSecret zu generieren. |
|CommonName2Ntlmx509StoreLocation|string, Standardwert L"LocalMachine"| statischen|Der Speicherort des X509-Zertifikats, das verwendet wird, um bei Verwendung der NTLM-Authentifizierung HMAC für CommonName2NtlmPasswordSecret zu generieren. |
|CommonName2Ntlmx509StoreName|string, Standardwert L"MY"|statischen| Der Speichername des X509-Zertifikats, das verwendet wird, um bei Verwendung der NTLM-Authentifizierung HMAC für CommonName2NtlmPasswordSecret zu generieren. |
|CommonNameNtlmPasswordSecret|SecureString, Standardwert Common::SecureString(L"")| statischen|Der geheime Schlüssel des Kennworts, der als Startwert verwendet wird, um bei Verwendung der NTLM-Authentifizierung das gleiche Kennwort zu generieren. |
|GenerateV1CommonNameAccount| Boolesch, Standardwert TRUE|statischen|Gibt an, ob ein Konto mit dem V1-Generierungsalgorithmus für Benutzernamen erstellt werden soll. Ab Service Fabric-Version 6.1 wird immer ein Konto mit v2-Generierung erstellt. Das V1-Konto ist erforderlich für Upgrades von/auf Versionen, die die V2-Generierung (vor 6.1) nicht unterstützen.|
|MaxCopyOperationThreads | Uint, Standardwert 0 |Dynamisch| Die maximale Anzahl von parallelen Dateien, die der sekundäre Replikator vom primären Server kopieren kann. 0 = Anzahl von Kernen. |
|MaxFileOperationThreads | Uint, Standardwert 100 |statischen| Die maximale Anzahl von parallelen Threads, die FileOperations (Kopieren/Verschieben) auf dem primären Replikator durchführen dürfen. 0 = Anzahl von Kernen. |
|MaxRequestProcessingThreads | Uint, Standardwert 200 |statischen|Die maximale Anzahl von parallelen Threads, die zum Verarbeiten von Anforderungen auf dem primären Replikator zulässig sind. 0 = Anzahl von Kernen. |
|MaxSecondaryFileCopyFailureThreshold | Uint, Standardwert 25|Dynamisch|Die maximale Anzahl von Wiederholungsversuchen beim Kopieren von Dateien auf den sekundären Replikator, bevor aufgegeben wird. |
|MaxStoreOperations | Uint, Standardwert 4096 |statischen|Die maximale Anzahl von parallelen Speichertransaktionen, die auf dem primären Replikator zulässig sind. 0 = Anzahl von Kernen. |
|NamingOperationTimeout |Zeit in Sekunden, Standardwert 60 |Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Das Timeout für die Durchführung des Benennungsvorgangs. |
|PrimaryAccountNTLMPasswordSecret | SecureString, Standardwert ist leer |statischen| Der geheime Schlüssel des Kennworts, der als Startwert verwendet wird, um bei Verwendung der NTLM-Authentifizierung das gleiche Kennwort zu generieren. |
|PrimaryAccountNTLMX509StoreLocation | string, Standardwert „LocalMachine“|statischen| Der Speicherort des X509-Zertifikats, das verwendet wird, um bei Verwendung der NTLM-Authentifizierung HMAC für PrimaryAccountNTLMPasswordSecret zu generieren. |
|PrimaryAccountNTLMX509StoreName | string, Standardwert „MY“|statischen| Der Speichername des X509-Zertifikats, das verwendet wird, um bei Verwendung der NTLM-Authentifizierung HMAC für PrimaryAccountNTLMPasswordSecret zu generieren. |
|PrimaryAccountNTLMX509Thumbprint | string, Standardwert ""|statischen|Der Fingerabdruck des X509-Zertifikats, das verwendet wird, um bei Verwendung der NTLM-Authentifizierung HMAC für PrimaryAccountNTLMPasswordSecret zu generieren. |
|PrimaryAccountType | string, Standardwert "" |statischen|Der primäre AccountType des Prinzipals für die ACL von FileStoreService-Freigaben. |
|PrimaryAccountUserName | string, Standardwert "" |statischen|Der primäre Kontobenutzername des Prinzipals für die ACL von FileStoreService-Freigaben. |
|PrimaryAccountUserPassword | SecureString, Standardwert ist leer |statischen|Das primäre Kontokennwort des Prinzipals für die ACL von FileStoreService-Freigaben. |
|QueryOperationTimeout | Zeit in Sekunden, Standardwert 60 |Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Das Timeout für die Durchführung des Abfragevorgangs. |
|SecondaryAccountNTLMPasswordSecret | SecureString, Standardwert ist leer |statischen| Der geheime Schlüssel des Kennworts, der als Startwert verwendet wird, um bei Verwendung der NTLM-Authentifizierung das gleiche Kennwort zu generieren. |
|SecondaryAccountNTLMX509StoreLocation | string, Standardwert „LocalMachine“ |statischen|Der Speicherort des X509-Zertifikats, das verwendet wird, um bei Verwendung der NTLM-Authentifizierung HMAC für SecondaryAccountNTLMPasswordSecret zu generieren. |
|SecondaryAccountNTLMX509StoreName | string, Standardwert „MY“ |statischen|Der Speichername des X509-Zertifikats, das verwendet wird, um bei Verwendung der NTLM-Authentifizierung HMAC für SecondaryAccountNTLMPasswordSecret zu generieren. |
|SecondaryAccountNTLMX509Thumbprint | string, Standardwert ""| statischen|Der Fingerabdruck des X509-Zertifikats, das verwendet wird, um bei Verwendung der NTLM-Authentifizierung HMAC für SecondaryAccountNTLMPasswordSecret zu generieren. |
|SecondaryAccountType | string, Standardwert ""|statischen| Der sekundäre AccountType des Prinzipals für die ACL von FileStoreService-Freigaben. |
|SecondaryAccountUserName | string, Standardwert ""| statischen|Der sekundäre Kontobenutzername des Prinzipals für die ACL von FileStoreService-Freigaben. |
|SecondaryAccountUserPassword | SecureString, Standardwert ist leer |statischen|Das sekundäre Kontokennwort des Prinzipals für die ACL von FileStoreService-Freigaben. |

## <a name="healthmanager"></a>HealthManager
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
| EnableApplicationTypeHealthEvaluation |Boolesch, Standardwert „false“ |statischen|Evaluierungsrichtlinie für die Clusterintegrität, wird für die Integritätsevaluierung pro Anwendungstyp aktiviert. |

## <a name="healthmanagerclusterhealthpolicy"></a>HealthManager/ClusterHealthPolicy
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|ConsiderWarningAsError |Boolesch, Standardwert „false“ |statischen|Evaluierungsrichtlinie für die Clusterintegrität: Warnungen werden als Fehler behandelt. |
|MaxPercentUnhealthyApplications | Ganze Zahl, Standardwert 0 |statischen|Evaluierungsrichtlinie für die Clusterintegrität: Maximaler Prozentsatz fehlerhafter Anwendungen, die für einen als fehlerfrei geltenden Cluster zulässig sind. |
|MaxPercentUnhealthyNodes | Ganze Zahl, Standardwert 0 |statischen|Evaluierungsrichtlinie für die Clusterintegrität: Maximaler Prozentsatz fehlerhafter Knoten, die für einen als fehlerfrei geltenden Cluster zulässig sind. |

## <a name="healthmanagerclusterupgradehealthpolicy"></a>HealthManager/ClusterUpgradeHealthPolicy
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|MaxPercentDeltaUnhealthyNodes|Ganze Zahl, Standardwert 10|statischen|Evaluierungsrichtlinie für die Clusterupgradeintegrität: Maximaler Prozentsatz des Deltas nicht integrer Knoten, die für einen als integer geltenden Cluster zulässig sind. |
|MaxPercentUpgradeDomainDeltaUnhealthyNodes|Ganze Zahl, Standardwert 15|statischen|Evaluierungsrichtlinie für die Clusterupgradeintegrität: Maximaler Prozentsatz des Deltas nicht integrer Knoten in einer Upgradedomäne, die für einen als integer geltenden Cluster zulässig sind. |

## <a name="hosting"></a>Hosting
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|ActivationMaxFailureCount |Ganze Zahl, Standardeinstellung 10 |Dynamisch|Die maximale Anzahl der Wiederholungsversuche nach misslungener Aktivierung, ehe die Versuche eingestellt werden |
|ActivationMaxRetryInterval |Zeit in Sekunden, Standardwert 300 |Dynamisch|Bei jedem fortlaufenden Aktivierungsfehler wiederholt das System die Aktivierung bis zum Wert von „ActivationMaxFailureCount“. „ActivationMaxRetryInterval“ gibt die Wartezeit nach jedem Aktivierungsfehler vor der Wiederholung an |
|ActivationRetryBackoffInterval |Zeit in Sekunden, Standardwert 5 |Dynamisch|Backoffintervall bei jedem Aktivierungsfehler. Bei jedem fortlaufenden Aktivierungsfehler wiederholt das System die Aktivierung bis zum Wert von „MaxActivationFailureCount“. Das Wiederholungsintervall bei jedem Versuch ist das Produkt der Anzahl fortlaufender Aktivierungsfehler und des Backoffintervalls für die Aktivierung. |
|ActivationTimeout| TimeSpan, Standardwert Common::TimeSpan::FromSeconds(180)|Dynamisch| Geben Sie die Zeitspanne in Sekunden an. Das Timeout für die Anwendungsaktivierung, die -deaktivierung und das -upgrade. |
|ApplicationHostCloseTimeout| TimeSpan, Standardwert Common::TimeSpan::FromSeconds(120)|Dynamisch| Geben Sie die Zeitspanne in Sekunden an. Wenn die Beendigung von Fabric in einem selbstaktivierten Prozess erkannt wird, schließt FabricRuntime alle Replikate im Hostprozess des Benutzers (applicationhost). Dies ist das Timeout für den Schließvorgang. |
|ApplicationUpgradeTimeout| TimeSpan, Standardwert Common::TimeSpan::FromSeconds(360)|Dynamisch| Geben Sie die Zeitspanne in Sekunden an. Das Timeout für das Anwendungsupgrade. Wenn das Timeout kleiner als „ActivationTimeout“ ist, tritt für das Bereitstellungsmodul ein Fehler auf. |
|CreateFabricRuntimeTimeout|TimeSpan, Standardwert Common::TimeSpan::FromSeconds(120)|Dynamisch| Geben Sie die Zeitspanne in Sekunden an. Der Timeoutwert für den FabricCreateRuntime-Synchronisierungsaufruf. |
|DeploymentMaxFailureCount|Ganze Zahl, Standardwert 20| Dynamisch|Die Anwendungsbereitstellung wird DeploymentMaxFailureCount Mal wiederholt, bevor ein Fehler für die Bereitstellung der betreffenden Anwendung auf dem Knoten auftritt.| 
|DeploymentMaxRetryInterval| TimeSpan, Standardwert Common::TimeSpan::FromSeconds(3600)|Dynamisch| Geben Sie die Zeitspanne in Sekunden an. Das maximale Wiederholungsintervall für die Bereitstellung. Bei jedem andauernden Fehler wird das Wiederholungsintervall wie folgt berechnet: Min( DeploymentMaxRetryInterval; Anzahl andauernder Fehler * DeploymentRetryBackoffInterval). |
|DeploymentRetryBackoffInterval| TimeSpan, Standardwert Common::TimeSpan::FromSeconds(10)|Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Backoffintervall für den Fehler bei der Bereitstellung. Bei jedem Continuous Deployment-Fehler wiederholt das System die Bereitstellung bis zu MaxDeploymentFailureCount Mal. Das Wiederholungsintervall ist das Produkt aus dem Continuous Deployment-Fehler und dem Backoffintervall der Bereitstellung. |
|EnableActivateNoWindow| Boolesch, Standardwert FALSE|Dynamisch| Der aktivierte Prozess wird im Hintergrund ohne Konsole erstellt. |
|EnableDockerHealthCheckIntegration|Boolesch, Standardwert TRUE|statischen|Ermöglicht die Integration von HEALTCHECK-Ereignissen des Docker-Tools in den Service Fabric-Systemintegritätsbericht. |
|EnableProcessDebugging|Boolesch, Standardwert FALSE|Dynamisch| Ermöglicht das Starten von Anwendungshosts unter dem Debugger. |
|EndpointProviderEnabled| Boolesch, Standardwert FALSE|statischen| Ermöglicht die Verwaltung von Endpunktressourcen durch Fabric. Erfordert die Angabe des Start- und Endportbereichs dder Anwendungen in FabricNode. |
|FabricContainerAppsEnabled| Boolesch, Standardwert FALSE|statischen| |
|FirewallPolicyEnabled|Boolesch, Standardwert FALSE|statischen| Ermöglicht das Öffnen von Firewallports für die Endpunktressourcen mit expliziten Ports, die in ServiceManifest angegeben werden. |
|GetCodePackageActivationContextTimeout|TimeSpan, Standardwert Common::TimeSpan::FromSeconds(120)|Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Der Timeoutwert für die CodePackageActivationContext-Aufrufe. Dies gilt nicht für Ad-hoc-Dienste. |
|IPProviderEnabled|Boolesch, Standardwert FALSE|statischen|Ermöglicht die Verwaltung von IP-Adressen. |
|NTLMAuthenticationEnabled|Boolesch, Standardwert FALSE|statischen| Ermöglicht die Unterstützung für die Verwendung von NTLM durch die Codepakete, die als andere Benutzer ausgeführt werden, damit die Prozesse computerübergreifend sicher kommunizieren können. |
|NTLMAuthenticationPasswordSecret|SecureString, Standardwert Common::SecureString(L"")|statischen|Eine verschlüsselte Zeichenfolge, der verwendet wird, um das Kennwort für NTLM-Benutzer zu generieren. Muss festgelegt werden, wenn NTLMAuthenticationEnabled auf TRUE festgelegt ist. Wird vom Bereitsteller überprüft. |
|NTLMSecurityUsersByX509CommonNamesRefreshInterval|TimeSpan, Standardwert Common::TimeSpan::FromMinutes(3)|Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Umgebungsspezifische Einstellungen. Das regelmäßige Intervall, in dem das Hosting auf neue Zertifikate für die FileStoreService-NTLM-Konfiguration scannt. |
|NTLMSecurityUsersByX509CommonNamesRefreshTimeout|TimeSpan, Standardwert Common::TimeSpan::FromMinutes(4)|Dynamisch| Geben Sie die Zeitspanne in Sekunden an. Das Timeout für die Konfiguration von NTLM-Benutzern mit allgemeinen Zertifikatnamen. Die NTLM-Benutzer sind für FileStoreService-Freigaben erforderlich. |
|RegisterCodePackageHostTimeout|TimeSpan, Standardwert Common::TimeSpan::FromSeconds(120)|Dynamisch| Geben Sie die Zeitspanne in Sekunden an. Der Timeoutwert für den FabricRegisterCodePackageHost-Synchronisierungsaufruf. Dies gilt für nur Multicode-Paketanwendungshosts wie FWP. |
|RequestTimeout|TimeSpan, Standardwert Common::TimeSpan::FromSeconds(30)|Dynamisch| Geben Sie die Zeitspanne in Sekunden an. Dies stellt das Timeout für die Kommunikation zwischen dem Anwendungshost des Benutzers und dem Fabric-Prozess für verschiedene hostingbezogene Vorgänge dar, z. B. für Factoryregistrierung und Runtimeregistrierung. |
|RunAsPolicyEnabled| Boolesch, Standardwert FALSE|statischen| Ermöglicht das Ausführen von Codepaketen als anderer lokaler Benutzer als der Benutzer, unter dem der Fabric-Prozess ausgeführt wird. Um diese Richtlinie zu aktivieren, muss Fabric als SYSTEM oder als Benutzer mit SeAssignPrimaryTokenPrivilege ausgeführt werden. |
|ServiceFactoryRegistrationTimeout| TimeSpan, Standardwert Common::TimeSpan::FromSeconds(120)|Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Der Timeoutwert für den Register(Stateless/Stateful)ServiceFactory-Synchronisierungsaufruf. |
|ServiceTypeDisableFailureThreshold |Ganze Zahl, Standardeinstellung 1 |Dynamisch|Dies ist der Schwellenwert für die Anzahl der Fehler. Bei Überschreitung wird FailoverManager (FM) benachrichtigt. FM deaktiviert den Diensttyp auf diesem Knoten und versucht, die Workload auf einem anderen Knoten zu platzieren. |
|ServiceTypeDisableGraceInterval|TimeSpan, Standardwert Common::TimeSpan::FromSeconds(30)|Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Das Zeitintervall, nach dem der Diensttyp deaktiviert werden kann. |
|ServiceTypeRegistrationTimeout |Zeit in Sekunden, Standardwert 300 |Dynamisch|Die maximale Zeit, die der Diensttyp bei der Fabric registriert sein darf |

## <a name="httpgateway"></a>HttpGateway
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|ActiveListeners |Uint, Standardwert 50 |statischen| Anzahl der Lesevorgänge, die an die HTTP-Serverwarteschlange gesendet werden sollen. Dadurch wird die Anzahl gleichzeitiger Anforderungen gesteuert, die von HttpGateway erfüllt werden können. |
|HttpGatewayHealthReportSendInterval |Zeit in Sekunden, Standardwert 30 |statischen|Geben Sie die Zeitspanne in Sekunden an. Das Intervall, in dem das HTTP-Gateway kumulierte Integritätsberichte an den Integritätsdienst sendet. |
|IsEnabled|Boolesch, Standardwert „false“ |statischen| Aktiviert bzw. deaktiviert „HttpGateway“. „HttpGateway“ ist standardmäßig deaktiviert. |
|MaxEntityBodySize |Uint, Standardwert 4194304 |Dynamisch|Gibt die maximale Größe des Texts an, der von einer HTTP-Anforderung erwartet werden kann. Der Standardwert ist 4 MB. Httpgateway erzeugt für eine Anforderung einen Fehler, wenn der Text größer als dieser Wert ist. Die minimale Blockgröße beim Lesen beträgt 4096 Bytes. Der Wert muss also größer oder gleich 4096. |

## <a name="imagestoreclient"></a>ImageStoreClient
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|ClientCopyTimeout | Zeit in Sekunden, Standardwert 1800 |Dynamisch| Geben Sie die Zeitspanne in Sekunden an. Der Timeoutwert für die Kopieranforderung der obersten Ebene an den Imagespeicherdienst. |
|ClientDefaultTimeout | Zeit in Sekunden, Standardwert 180 |Dynamisch| Geben Sie die Zeitspanne in Sekunden an. Der Timeoutwert für alle anderen Anforderungen außer Upload- oder Downloadanforderungen (z. B. Vorhanden, Löschen) an den Imagespeicherdienst. |
|ClientDownloadTimeout | Zeit in Sekunden, Standardwert 1800 |Dynamisch| Geben Sie die Zeitspanne in Sekunden an. Der Timeoutwert für die Downloadanforderung der obersten Ebene an den Imagespeicherdienst. |
|ClientListTimeout | Zeit in Sekunden, Standardwert 600 |Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Der Timeoutwert für die Listenanforderung der obersten Ebene an den Imagespeicherdienst. |
|ClientUploadTimeout |Zeit in Sekunden, Standardwert 1800 |Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Der Timeoutwert für die Uploadanforderung der obersten Ebene an den Imagespeicherdienst. |

## <a name="imagestoreservice"></a>ImageStoreService
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|Aktiviert |Boolesch, Standardwert „false“ |statischen|Das Flag „Enabled“ für ImageStoreService. Standardwert: FALSE |
|MinReplicaSetSize | Ganze Zahl, Standardwert 3 |statischen|MinReplicaSetSize für ImageStoreService. |
|PlacementConstraints | string, Standardwert "" |statischen| PlacementConstraints für ImageStoreService. |
|QuorumLossWaitDuration | Zeit in Sekunden, Standardwert MaxValue |statischen| Geben Sie die Zeitspanne in Sekunden an. QuorumLossWaitDuration für ImageStoreService. |
|ReplicaRestartWaitDuration | Zeit in Sekunden, Standardwert 60,0 * 30 |statischen|Geben Sie die Zeitspanne in Sekunden an. ReplicaRestartWaitDuration für ImageStoreService. |
|StandByReplicaKeepDuration | Zeit in Sekunden, Standardwert 3600,0 * 2 |statischen| Geben Sie die Zeitspanne in Sekunden an. StandByReplicaKeepDuration für ImageStoreService. |
|TargetReplicaSetSize | Ganze Zahl, Standardwert 7 |statischen|TargetReplicaSetSize für ImageStoreService. |

## <a name="ktllogger"></a>KtlLogger
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|AutomaticMemoryConfiguration |Ganze Zahl, Standardwert 1 |Dynamisch|Flag, das angibt, ob die Einstellungen für den Arbeitsspeicher automatisch und dynamisch konfiguriert werden sollen. Bei 0 werden die Konfigurationseinstellungen für den Arbeitsspeicher direkt verwendet und nicht basierend auf Systembedingungen geändert. Bei 1 werden die Einstellungen für den Arbeitsspeicher automatisch konfiguriert und können basierend auf Systembedingungen geändert werden. |
|MaximumDestagingWriteOutstandingInKB | Ganze Zahl, Standardwert 0 |Dynamisch|Die Anzahl an KB, die das freigegebene Protokoll dem dedizierten Protokoll voraus sein darf. Verwenden Sie 0, um keine Begrenzung anzugeben.
|SharedLogId |string, Standardwert "" |statischen|Eindeutige GUID für den freigegebenen Protokollcontainer. Verwenden Sie "", wenn Sie den Standardpfad im Fabricdatenstamm nutzen. |
|SharedLogPath |string, Standardwert "" |statischen|Pfad und Dateiname für den Speicherort des freigegebenen Protokollcontainers. Verwenden Sie "", um den Standardpfad im Fabricdatenstamm zu nutzen. |
|SharedLogSizeInMB |Ganze Zahl, Standardwert 8192 |statischen|Die Anzahl an MB, die für den freigegebenen Protokollcontainer reserviert wird. |
|WriteBufferMemoryPoolMaximumInKB | Ganze Zahl, Standardwert 0 |Dynamisch|Die Anzahl an KB, bis zu der der Schreibpuffer-Speicherpool anwachsen kann. Verwenden Sie 0, um keine Begrenzung anzugeben. |
|WriteBufferMemoryPoolMinimumInKB |Ganze Zahl, Standardwert 8388608 |Dynamisch|Die Anzahl an KB, die anfänglich für den Schreibpuffer-Speicherpool reserviert wird. Verwenden Sie 0, um eine Begrenzung anzugeben. Der Standardwert muss mit SharedLogSizeInMB weiter unten konsistent sein. |

## <a name="management"></a>Verwaltung
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|AzureStorageMaxConnections | Ganze Zahl, Standardwert 5000 |Dynamisch|Die maximale Anzahl gleichzeitiger Verbindungen mit Azure Storage. |
|AzureStorageMaxWorkerThreads | Ganze Zahl, Standardwert 25 |Dynamisch|Die maximale Anzahl von parallelen Workerthreads. |
|AzureStorageOperationTimeout | Zeit in Sekunden, Standardwert 6000 |Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Timeout für den Abschluss des xstore-Vorgangs. |
|DisableChecksumValidation | Boolesch, Standardwert „false“ |statischen| Diese Konfiguration ermöglicht es, die Prüfsummenüberprüfung während der Anwendungsbereitstellung zu aktivieren oder zu deaktivieren. |
|DisableServerSideCopy | Boolesch, Standardwert „false“ |statischen|Diese Konfiguration aktiviert oder deaktiviert serverseitiges Kopieren des Anwendungspakets in ImageStore während der Anwendungsbereitstellung. |
|ImageCachingEnabled | Boolesch, Standardwert „true“ |statischen|Diese Konfiguration ermöglicht es, das Zwischenspeichern zu aktivieren oder zu deaktivieren. |
|ImageStoreConnectionString |SecureString |statischen|Die Verbindungszeichenfolge für das Stammverzeichnis für ImageStore. |
|ImageStoreMinimumTransferBPS | Ganze Zahl, Standardwert 1024 |Dynamisch|Die minimale Übertragungsrate zwischen dem Cluster und ImageStore. Dieser Wert wird verwendet, um das Timeout zu bestimmen, wenn auf den externen ImageStore zugegriffen wird. Ändern Sie diesen Wert nur, wenn die Latenz zwischen dem Cluster und ImageStore hoch ist, damit der Cluster mehr Zeit für Downloads aus dem externen ImageStore erhält. |

## <a name="metricactivitythresholds"></a>MetricActivityThresholds
| **Parameter** | **Zulässige Werte** |**Upgraderichtlinie**| **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|PropertyGroup|KeyIntegerValueMap, Standardwert None|Dynamisch|Bestimmt die MetricActivityThresholds für die Metriken im Cluster. Der Lastenausgleich funktioniert, wenn maxNodeLoad größer als MetricActivityThresholds ist. Für Defragmentierungsmetriken definiert dieser Wert die Auslastung, bei oder unter der Service Fabric den Knoten als leer betrachtet. |

## <a name="metricbalancingthresholds"></a>MetricActivityThresholds
| **Parameter** | **Zulässige Werte** |**Upgraderichtlinie**| **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|PropertyGroup|KeyDoubleValueMap, Standardwert None|Dynamisch|Bestimmt die Sammlung der MetricActivityThresholds für die Metriken im Cluster. Der Lastenausgleich funktioniert, wenn maxNodeLoad/minNodeLoad größer als MetricBalancingThresholds ist. Die Defragmentierung funktioniert, wenn maxNodeLoad/minNodeLoad in mindestens einem FD oder UD kleiner als MetricBalancingThresholds ist. |

## <a name="namingservice"></a>NamingService
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|GatewayServiceDescriptionCacheLimit |Ganze Zahl, Standardwert 0 |statischen|Die maximale Anzahl von Einträgen, die im LRU-Dienstbeschreibungscache im Naming Gateway beibehalten werden (bei 0 gilt keine Begrenzung). |
|MaxClientConnections |Ganze Zahl, Standardwert 1000 |Dynamisch|Die maximal zulässige Anzahl von Clientverbindungen pro Gateway. |
|MaxFileOperationTimeout |Zeit in Sekunden, Standardwert 30 |Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Das maximale Timeout, das für den Vorgang des Dateispeicherdiensts zulässig ist. Anforderungen, die einen höheren Timeoutwert angeben, werden zurückgewiesen. |
|MaxIndexedEmptyPartitions |Ganze Zahl, Standardwert 1000 |Dynamisch|Die maximale Anzahl von leeren Partitionen, die im Benachrichtigungscache indiziert bleiben, um Clients zu synchronisieren, die erneut eine Verbindung herstellen. Leere Partitionen oberhalb dieser Anzahl werden in aufsteigender Reihenfolge der Suchversionen aus dem Index entfernt. Clients, die erneut Verbindungen herstellen, können weiterhin synchronisiert werden und versäumte Aktualisierungen leerer Partitionen empfangen. Das Synchronisierungsprotokoll wird jedoch umfangreicher. |
|MaxMessageSize |Ganze Zahl, Standardwert 4\*1024\*1024 |statischen|Die maximale Nachrichtengröße für die Clientknotenkommunikation, wenn Benennungen verwendet werden. DOS-Angriffsbekämpfung, Standardwert 4 MB. |
|MaxNamingServiceHealthReports | Ganze Zahl, Standardwert 10 |Dynamisch|Die maximale Anzahl von langsamen Vorgängen, die der Naming Service-Speicher gleichzeitig als fehlerhaft meldet. Bei 0 werden alle langsamen Vorgänge gesendet. |
|MaxOperationTimeout |Zeit in Sekunden, Standardwert 600 |Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Das maximale Timeout, das für Clientvorgänge zulässig ist. Anforderungen, die einen höheren Timeoutwert angeben, werden zurückgewiesen. |
|MaxOutstandingNotificationsPerClient |Ganze Zahl, Standardwert 1000 |Dynamisch|Die maximale Anzahl von ausstehenden Benachrichtigungen, bevor eine Clientregistrierung erzwungenermaßen vom Gateway geschlossen wird. |
|MinReplicaSetSize | Ganze Zahl, Standardwert 3 |Nicht zulässig| Die Mindestanzahl von Naming Service-Replikaten, in die geschrieben werden muss, um eine Aktualisierung abzuschließen. Wenn weniger Replikate als diese Anzahl im System aktiv sind, verweigert das System für die Zuverlässigkeit Aktualisierungen des Naming Service-Speichers, bis Replikate wiederhergestellt werden. Dieser Wert sollte nie größer als TargetReplicaSetSize sein. |
|PartitionCount |Ganze Zahl, Standardwert 3 |Nicht zulässig|Die Anzahl der zu erstellenden Partitionen des Naming Service-Speichers. Jede Partition besitzt einen einzelnen Partitionsschlüssel, der dessen Index entspricht. Daher sind Partitionsschlüssel [0; PartitionCount) vorhanden. Durch Erhöhen der Anzahl von Naming Service-Partitionen kann der Naming Service in größerem Maßstab ausgeführt werden, indem die durchschnittliche Menge von Daten verringert wird, die von sichernden Replikatgruppen gespeichert werden. Dies erhöht jedoch die Auslastung von Ressourcen (da Dienstreplikate im Umfang von PartitionCount × ReplicaSetSize beibehalten werden müssen).|
|PlacementConstraints | string, Standardwert "" |Nicht zulässig| Platzierungseinschränkung für den Naming Service. |
|QuorumLossWaitDuration | Zeit in Sekunden, Standardwert MaxValue |Nicht zulässig| Geben Sie die Zeitspanne in Sekunden an. Wenn in einem Naming Service ein Quorumverlust auftritt, wird dieser Timer gestartet. Nach dessen Ablauf betrachtet FM die ausgefallenen Replikate als verloren und versucht, das Quorum wiederherzustellen. Beachten Sie, dass dies zu Datenverlusten führen kann. |
|RepairInterval | Zeit in Sekunden, Standardwert 5 |statischen| Geben Sie die Zeitspanne in Sekunden an. Intervall, in dem die Korrektur von Benennungsinkonsistenzen zwischen dem Autoritätsbesitzer und dem Namensbesitzer gestartet wird. |
|ReplicaRestartWaitDuration | Zeit in Sekunden, Standardwert (60,0 * 30)|Nicht zulässig| Geben Sie die Zeitspanne in Sekunden an. Wenn ein Naming Service-Replikat ausfällt, wird dieser Timer gestartet. Nach Ablauf beginnt der FM damit, die ausgefallenen Replikate zu ersetzen (die Replikate werden noch nicht als verloren betrachtet). |
|ServiceDescriptionCacheLimit | Ganze Zahl, Standardwert 0 |statischen| Die maximale Anzahl von Einträgen, die im LRU-Dienstbeschreibungscache im Naming Service-Speicher beibehalten werden (bei 0 gilt keine Begrenzung). |
|ServiceNotificationTimeout |Zeit in Sekunden, Standardwert 30 |Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Das Timeout, das verwendet wird, wenn Dienstbenachrichtigungen an den Client übermittelt werden. |
|StandByReplicaKeepDuration | Zeit in Sekunden, Standardwert 3600,0 * 2 |Nicht zulässig| Geben Sie die Zeitspanne in Sekunden an. Wenn ein Naming Service-Replikat nach einem Ausfall wieder aktiv ist, wurde es möglicherweise bereits ersetzt. Dieser Timer bestimmt, wie lange FM das Reservereplikat beibehält, bevor es verworfen wird. |
|TargetReplicaSetSize |Ganze Zahl, Standardwert 7 |Nicht zulässig|Die Anzahl von Replikatgruppen für jede Partition des Naming Service-Speichers. Das Erhöhen der Anzahl von Replikatgruppen erhöht den Grad an Zuverlässigkeit der Informationen im Naming Service-Speicher. Durch eine Verringerung der Änderung gehen Informationen als Ergebnis von Knotenfehlern verloren. Es entsteht eine erhöhte Last für Windows Fabric, und die Zeitspanne zum Durchführen von Aktualisierungen an den Benennungsdaten wird verlängert.|

## <a name="nodebufferpercentage"></a>NodeBufferPercentage
| **Parameter** | **Zulässige Werte** |**Upgraderichtlinie**| **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|PropertyGroup|KeyDoubleValueMap, Standardwert None|Dynamisch|Der Knotenkapazitäts-Prozentsatz pro Metrikname. Wird als Puffer verwendet, um freien Speicherplatz auf einem Knoten für Failover bereitzustellen. |

## <a name="nodecapacities"></a>NodeCapacities
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|PropertyGroup |NodeCapacityCollectionMap |statischen|Eine Sammlung von Knotenkapazitäten für verschiedene Metriken. |

## <a name="nodedomainids"></a>NodeDomainIds
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|PropertyGroup |NodeFaultDomainIdCollection |statischen|Beschreibt die Fehlerdomäne, zu der ein Knoten gehört. Die Fehlerdomäne wird durch einen URI definiert, der die Position des Knotens im Datencenter beschreibt.  Fehlerdomänen-URIs weisen das Format fd:/fd/ gefolgt von einem URI-Pfadsegment auf.|
|UpgradeDomainId |string, Standardwert "" |statischen|Beschreibt die Upgradedomäne, zu der ein Knoten gehört. |

## <a name="nodeproperties"></a>NodeProperties
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|PropertyGroup |NodePropertyCollectionMap |statischen|Eine Sammlung der Zeichenfolgen von Schlüssel-Wert-Paaren für Knoteneigenschaften. |

## <a name="paas"></a>Paas
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|ClusterId |string, Standardwert "" |Nicht zulässig|X509-Zertifikatspeicher, der vom Fabric für den Konfigurationsschutz verwendet. |

## <a name="performancecounterlocalstore"></a>PerformanceCounterLocalStore
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|Counters |Zeichenfolge | Dynamisch |Durch Trennzeichen getrennte Liste der zu erfassenden Leistungsindikatoren. |
|IsEnabled |Boolesch, Standardwert „true“ | Dynamisch |Das Flag gibt an, ob die Erfassung von Leistungsindikatoren für den lokalen Knoten aktiviert ist. |
|MaxCounterBinaryFileSizeInMB |Ganze Zahl, Standardwert 1 | Dynamisch |Maximale Größe (in MB) für jede Leistungsindikator-Binärdatei. |
|NewCounterBinaryFileCreationIntervalInMinutes |Ganze Zahl, Standardwert 10 | Dynamisch |Maximales Intervall (in Sekunden), nach dem eine neue Leistungsindikator-Binärdatei erstellt wird. |
|SamplingIntervalInSeconds |Ganze Zahl, Standardwert 60 | Dynamisch |Samplingintervall für Leistungsindikatoren, die erfasst werden. |

## <a name="placementandloadbalancing"></a>PlacementAndLoadBalancing
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|AffinityConstraintPriority | Ganze Zahl, Standardwert 0 | Dynamisch|Bestimmt die Priorität der Affinitätseinschränkung: 0: stark; 1: schwach; negativ: Ignorieren. |
|ApplicationCapacityConstraintPriority | Ganze Zahl, Standardwert 0 | Dynamisch|Bestimmt die Priorität der Kapazitätseinschränkung: 0: stark; 1: schwach; negativ: Ignorieren. |
|AutoDetectAvailableResources|Boolesch, Standardwert TRUE|statischen|Diese Konfiguration löst eine automatische Erkennung der verfügbaren Ressourcen auf dem Knoten (CPU und Arbeitsspeicher) aus. Wenn diese Konfiguration auf TRUE festgelegt ist, lesen wir reale Kapazitäten aus und korrigieren sie, wenn der Benutzer fehlerhafte Knotenkapazitäten angegeben oder keine Kapazitäten definiert hat. Wenn diese Konfiguration auf FALSE festgelegt ist, geben wir eine Warnung aus, dass der Benutzer fehlerhafte Knotenkapazitäten angegeben hat; korrigieren diese aber nicht. Dies bedeutet, dass der Benutzer die Kapazitäten nutzen möchte, die als größer als die tatsächlichen Kapazitäten des Knotens angegeben wurden, oder wenn Kapazitäten nicht definiert sind; wird von einer unbegrenzten Kapazität ausgegangen. |
|BalancingDelayAfterNewNode | Zeit in Sekunden, Standardwert 120 |Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Starten Sie keine Aktivitäten zum Lastenausgleich innerhalb dieses Zeitraums nach dem Hinzufügen eines neuen Knotens. |
|BalancingDelayAfterNodeDown | Zeit in Sekunden, Standardwert 120 |Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Starten Sie keine Aktivitäten zum Lastenausgleich innerhalb dieses Zeitraums nach einem Knotenausfall. |
|CapacityConstraintPriority | Ganze Zahl, Standardwert 0 | Dynamisch|Bestimmt die Priorität der Kapazitätseinschränkung: 0: stark; 1: schwach; negativ: Ignorieren. |
|ConsecutiveDroppedMovementsHealthReportLimit | Ganze Zahl, Standardwert 20 | Dynamisch|Definiert die Anzahl der aufeinanderfolgenden Male, die von ResourceBalancer ausgegebene Datenverschiebungen verworfen werden, bevor eine Diagnose durchgeführt wird und Integritätswarnungen ausgegeben werden. Negativ: Unter dieser Bedingung werden keine Warnungen ausgegeben. |
|ConstraintFixPartialDelayAfterNewNode | Zeit in Sekunden, Standardwert 120 |Dynamisch| Geben Sie die Zeitspanne in Sekunden an. Beheben Sie keine FaultDomain- und UpgradeDomain-Einschränkungsverletzungen innerhalb dieses Zeitraums nach dem Hinzufügen eines neuen Knotens. |
|ConstraintFixPartialDelayAfterNodeDown | Zeit in Sekunden, Standardwert 120 |Dynamisch| Geben Sie die Zeitspanne in Sekunden an. Beheben Sie keine FaultDomain- und UpgradeDomain-Einschränkungsverletzungen innerhalb dieses Zeitraums nach einem Knotenausfall. |
|ConstraintViolationHealthReportLimit | Ganze Zahl, Standardwert 50 |Dynamisch| Definiert, wie häufig ein Einschränkungen verletzendes Replikat dauerhaft nicht korrigiert bleiben muss, bevor eine Diagnose durchgeführt wird und Integritätsberichte ausgegeben werden. |
|DetailedConstraintViolationHealthReportLimit | Ganze Zahl, Standardwert 200 |Dynamisch| Definiert, wie häufig ein Einschränkungen verletzendes Replikat dauerhaft nicht korrigiert bleiben muss, bevor eine Diagnose durchgeführt wird und detaillierte Integritätsberichte ausgegeben werden. |
|DetailedDiagnosticsInfoListLimit | Ganze Zahl, Standardwert 15 |Dynamisch| Definiert die Anzahl von Diagnoseeinträgen (mit detaillierten Informationen) pro Einschränkung, die vor der Kürzung in die Diagnose aufgenommen werden.|
|DetailedNodeListLimit | Ganze Zahl, Standardwert 15 |Dynamisch| Definiert die Anzahl von Knoten pro Einschränkung, die vor der Kürzung in Berichte zu nicht platzierten Replikaten aufgenommen werden. |
|DetailedPartitionListLimit | Ganze Zahl, Standardwert 15 |Dynamisch| Definiert die Anzahl von Partitionen pro Diagnoseeintrag für eine Einschränkung, die vor der Kürzung in die Diagnose aufgenommen werden. |
|DetailedVerboseHealthReportLimit | Ganze Zahl, Standardwert 200 | Dynamisch|Definiert, wie häufig ein nicht platziertes Replikat dauerhaft nicht platziert bleiben muss, bevor detaillierte Integritätsberichte ausgegeben werden. |
|FaultDomainConstraintPriority | Ganze Zahl, Standardwert 0 |Dynamisch| Bestimmt die Priorität der Fehlerdomäneneinschränkung: 0: stark; 1: schwach; negativ: Ignorieren. |
|GlobalMovementThrottleCountingInterval | Zeit in Sekunden, Standardwert 600 |statischen| Geben Sie die Zeitspanne in Sekunden an. Geben Sie die Länge des letzten Intervalls an, für das Datenverschiebungen pro Domänenreplikat nachverfolgt werden sollen (wird zusammen mit GlobalMovementThrottleThreshold verwendet). Kann auf 0 festgelegt werden, um die globale Drosselung vollständig zu ignorieren. |
|GlobalMovementThrottleThreshold | Uint, Standardwert 1000 |Dynamisch| Maximale Anzahl von zulässigen Datenverschiebungen in der Lastenausgleichsphase im letzten von GlobalMovementThrottleCountingInterval angegebenen Intervall. |
|GlobalMovementThrottleThresholdForBalancing | Uint, Standardwert 0 | Dynamisch|Maximale Anzahl von zulässigen Datenverschiebungen in der Lastenausgleichsphase im letzten von GlobalMovementThrottleCountingInterval angegebenen Intervall. Bei 0 gilt keine Begrenzung. |
|GlobalMovementThrottleThresholdForPlacement | Uint, Standardwert 0 |Dynamisch| Maximale Anzahl von zulässigen Datenverschiebungen in der Platzierungsphase im letzten von GlobalMovementThrottleCountingInterval angegebenen Intervall. Bei 0 gilt keine Begrenzung.|
|GlobalMovementThrottleThresholdPercentage|double, Standardwert 0|Dynamisch|Maximale Anzahl der Gesamtbewegungen, die in Phasen für Lastenausgleich und Platzierung (ausgedrückt als Prozentwert der Gesamtanzahl der Replikate im Cluster) im letzten durch GlobalMovementThrottleCountingInterval angegebenen Intervall zulässig sind. Bei 0 gilt keine Begrenzung. Wenn diese Angabe und GlobalMovementThrottleThreshold festgelegt sind, wird der konservativere Grenzwert verwendet.|
|GlobalMovementThrottleThresholdPercentageForBalancing|double, Standardwert 0|Dynamisch|Maximale Anzahl der Bewegungen, die in der Phase für Lastenausgleich (ausgedrückt als Prozentwert der Gesamtanzahl der Replikate im PLB) im letzten durch GlobalMovementThrottleCountingInterval angegebenen Intervall zulässig sind. Bei 0 gilt keine Begrenzung. Wenn sowohl diese Einstellung als auch „GlobalMovementThrottleThresholdForBalancing“ festgelegt sind, wird der konservativere Grenzwert verwendet.|
|InBuildThrottlingAssociatedMetric | string, Standardwert "" |statischen| Der zugehörige Metrikname für diese Drosselung. |
|InBuildThrottlingEnabled | Boolesch, Standardwert „false“ |Dynamisch| Bestimmen Sie, ob die integrierte Drosselung aktiviert wird. |
|InBuildThrottlingGlobalMaxValue | Ganze Zahl, Standardwert 0 |Dynamisch|Die maximale, global zulässige Anzahl von integrierten Replikaten. |
|InterruptBalancingForAllFailoverUnitUpdates | Boolesch, Standardwert „false“ | Dynamisch|Bestimmt, ob jede Art von Aktualisierung der Failovereinheiten schnelle oder langsame Lastenausgleiche unterbrechen soll. Bei „false“ werden Lastenausgleiche unterbrochen, wenn die Failovereinheit erstellt/gelöscht wird, ihr Replikate fehlen bzw. sie einen geänderten Speicherort für das primäre Replikat oder eine geänderte Anzahl von Replikaten aufweist. Lastenausgleiche werden in anderen Fällen NICHT unterbrochen: wenn die Failovereinheit über zusätzliche Replikate verfügt, ein Replikatflag geändert wurde, nur die Partitionsversion geändert wurde oder in anderen Fällen. |
|MinConstraintCheckInterval | Zeit in Sekunden, Standardwert 1 |Dynamisch| Geben Sie die Zeitspanne in Sekunden an. Definiert die minimale Zeitspanne, die vor zwei aufeinanderfolgenden Runden zur Einschränkungsüberprüfung verstreichen muss. |
|MinLoadBalancingInterval | Zeit in Sekunden, Standardwert 5 |Dynamisch| Geben Sie die Zeitspanne in Sekunden an. Definiert die minimale Zeitspanne, die vor zwei aufeinanderfolgenden Runden zum Lastenausgleich verstreichen muss. |
|MinPlacementInterval | Zeit in Sekunden, Standardwert 1 |Dynamisch| Geben Sie die Zeitspanne in Sekunden an. Definiert die minimale Zeitspanne, die vor zwei aufeinanderfolgenden Platzierungsrunden verstreichen muss. |
|MoveExistingReplicaForPlacement | Boolesch, Standardwert „true“ |Dynamisch|Einstellung, die festlegt, ob vorhandene Replikate während der Platzierung verschoben werden. |
|MovementPerPartitionThrottleCountingInterval | Zeit in Sekunden, Standardwert 600 |statischen| Geben Sie die Zeitspanne in Sekunden an. Geben Sie die Länge des letzten Intervalls an, für das Datenverschiebungen pro Replikat für jede Partition nachverfolgt werden sollen (wird zusammen mit MovementPerPartitionThrottleThreshold verwendet). |
|MovementPerPartitionThrottleThreshold | Uint, Standardwert 50 |Dynamisch| Für eine Partition werden keine Datenverschiebungen im Zusammenhang mit dem Lastenausgleich ausgeführt, wenn die Anzahl der Datenverschiebungen im Zusammenhang mit dem Lastenausgleich für Replikate dieser Partition im letzten von MovementPerPartitionThrottleCountingInterval angegebenen Intervall den Wert von MovementPerFailoverUnitThrottleThreshold erreicht oder überschritten hat. |
|MoveParentToFixAffinityViolation | Boolesch, Standardwert „false“ |Dynamisch| Einstellung, die festlegt, ob übergeordnete Replikate verschoben werden können, um Affinitätseinschränkungen zu beheben.|
|PartiallyPlaceServices | Boolesch, Standardwert „true“ |Dynamisch| Bestimmt, ob alle Dienstreplikate im Cluster „ganz oder gar nicht“ platziert werden, wenn nur beschränkt geeignete Knoten vorhanden sind.|
|PlaceChildWithoutParent | Boolesch, Standardwert „true“ | Dynamisch|Einstellung, die festlegt, ob untergeordnete Dienstreplikate platziert werden können, wenn kein übergeordnetes Replikat ausgeführt wird. |
|PlacementConstraintPriority | Ganze Zahl, Standardwert 0 | Dynamisch|Bestimmt die Priorität der Platzierungseinschränkung: 0: stark; 1: schwach; negativ: Ignorieren. |
|PlacementConstraintValidationCacheSize | Ganze Zahl, Standardwert 10000 |Dynamisch| Schränkt die Größe der Tabelle für die schnelle Überprüfung und Zwischenspeicherung von Einschränkungsausdrücken für die Platzierung ein. |
|PlacementSearchTimeout | Zeit in Sekunden, Standardwert 0,5 |Dynamisch| Geben Sie die Zeitspanne in Sekunden an. Beim Platzieren von Diensten wird für maximal diese Zeitspanne gesucht, bevor ein Ergebnis zurückgegeben wird. |
|PLBRefreshGap | Zeit in Sekunden, Standardwert 1 |Dynamisch| Geben Sie die Zeitspanne in Sekunden an. Definiert die minimale Zeitspanne, die verstreichen muss, bevor PLB den Zustand erneut aktualisiert. |
|PreferredLocationConstraintPriority | Ganze Zahl, Standardwert 2| Dynamisch|Bestimmt die Priorität der bevorzugten Speicherorteinschränkung: 0: stark; 1: schwach; 2: Optimierung; negativ: Ignorieren. |
|PreventTransientOvercommit | Boolesch, Standardwert „false“ | Dynamisch|Bestimmt, ob PLB sofort Ressourcen nutzen soll, die von den initiierten Datenverschiebungen freigegeben werden. Standardmäßig kann PLB Datenverschiebungen aus und in einen Knoten initiieren, sodass eine vorübergehende Überlastung entstehen kann. Wenn dieser Parameter auf TRUE festgelegt wird, wird diese Art von Überlastungen verhindert, und eine bedarfsgesteuerte Defragmentierung (placementWithMove) wird deaktiviert. |
|ScaleoutCountConstraintPriority | Ganze Zahl, Standardwert 0 |Dynamisch| Bestimmt die Priorität der Einschränkung für den Umfang der horizontalen Hochskalierung: 0: stark; 1: schwach; negativ: Ignorieren. |
|SwapPrimaryThrottlingAssociatedMetric | string, Standardwert ""|statischen| Der zugehörige Metrikname für diese Drosselung. |
|SwapPrimaryThrottlingEnabled | Boolesch, Standardwert „false“|Dynamisch| Bestimmen Sie, ob die Drosselung beim Austausch des primären Replikats aktiviert wird. |
|SwapPrimaryThrottlingGlobalMaxValue | Ganze Zahl, Standardwert 0 |Dynamisch| Die maximale, global zulässige Anzahl von primären Replikaten für den Austausch. |
|TraceCRMReasons |Boolesch, Standardwert „true“ |Dynamisch|Gibt an, ob Gründe für von CRM ausgegebene Datenverschiebungen zum Kanal für Betriebsereignisse verfolgt werden sollen. |
|UpgradeDomainConstraintPriority | Ganze Zahl, Standardwert 1| Dynamisch|Bestimmt die Priorität der Upgradedomäneneinschränkung: 0: stark; 1: schwach; negativ: Ignorieren. |
|UseMoveCostReports | Boolesch, Standardwert „false“ | Dynamisch|Weist LB an, das Kostenelement der Bewertungsfunktion zu ignorieren. Dies führt möglicherweise zu mehr Datenverschiebungen für eine Platzierung mit besserem Lastenausgleich. |
|UseSeparateSecondaryLoad | Boolesch, Standardwert „true“ | Dynamisch|Einstellung, die festlegt, ob eine andere sekundäre Last verwendet werden soll. |
|ValidatePlacementConstraint | Boolesch, Standardwert „true“ |Dynamisch| Gibt an, ob der PlacementConstraint-Ausdruck für einen Dienst überprüft wird, wenn ServiceDescription für einen Dienst aktualisiert wird. |
|VerboseHealthReportLimit | Ganze Zahl, Standardwert 20 | Dynamisch|Definiert, wie häufig ein Replikat nicht platziert werden muss, bevor eine Integritätswarnung dafür gemeldet wird (wenn ausführliche Integritätsberichte aktiviert sind). |

## <a name="reconfigurationagent"></a>ReconfigurationAgent
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|ApplicationUpgradeMaxReplicaCloseDuration | Zeit in Sekunden, Standardwert 900 |Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Die Dauer, die das System wartet, bevor Diensthosts beendet werden, deren Replikate während des Anwendungsupgrades nicht geöffnet werden können.|
|FabricUpgradeMaxReplicaCloseDuration | Zeit in Sekunden, Standardwert 900 |Dynamisch| Geben Sie die Zeitspanne in Sekunden an. Die Dauer, die das System wartet, bevor Diensthosts beendet werden, deren Replikate während des Fabric-Upgrades nicht geöffnet werden können. |
|GracefulReplicaShutdownMaxDuration|TimeSpan, Standardwert Common::TimeSpan::FromSeconds(120)|Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Die Dauer, die das System wartet, bevor Diensthosts beendet werden, deren Replikate nicht geöffnet werden können. Wenn der Wert auf 0 (null) festgelegt ist, werden keine Replikate zum Schließen angewiesen.|
|NodeDeactivationMaxReplicaCloseDuration | Zeit in Sekunden, Standardwert 900 |Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Die Dauer, die das System wartet, bevor Diensthosts beendet werden, deren Replikate während der Knotendeaktivierung nicht geöffnet werden können. |
|PeriodicApiSlowTraceInterval | Zeit in Sekunden, Standardwert 5 Minuten |Dynamisch| Geben Sie die Zeitspanne in Sekunden an. PeriodicApiSlowTraceInterval definiert das Intervall, in dem langsame API-Aufrufe vom API-Monitor zurückverfolgt werden. |
|ReplicaChangeRoleFailureRestartThreshold|Ganze Zahl, Standardwert 10|Dynamisch| Eine ganze Zahl. Gibt die Anzahl der API-Fehler bei der Heraufstufung des primären Replikators an, nach der eine automatische Minderungsaktion (Replikatneustart) angewendet wird. |
|ReplicaChangeRoleFailureWarningReportThreshold|Ganze Zahl, Standardwert 2147483647|Dynamisch| Eine ganze Zahl. Gibt die Anzahl der API-Fehler bei der Heraufstufung des primären Replikators an, nach der ein Warnungsintegritätsbericht ausgelöst wird.|
|ServiceApiHealthDuration | Zeit in Sekunden, Standardwert 30 Minuten |Dynamisch| Geben Sie die Zeitspanne in Sekunden an. ServiceApiHealthDuration definiert, wie lange auf die Ausführung einer Dienst-API gewartet wird, bevor sie als fehlerhaft gemeldet wird. |
|ServiceReconfigurationApiHealthDuration | Zeit in Sekunden, Standardwert 30 |Dynamisch| Geben Sie die Zeitspanne in Sekunden an. ServiceReconfigurationApiHealthDuration definiert, wie lange auf die Ausführung einer Dienst-API gewartet wird, bevor sie als fehlerhaft gemeldet wird. Dies gilt für API-Aufrufe, die sich auf die Verfügbarkeit auswirken.|

## <a name="replication"></a>Replikation
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie**| **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|BatchAcknowledgementInterval|TimeSpan, Standardwert Common::TimeSpan::FromMilliseconds(15)|statischen|Geben Sie die Zeitspanne in Sekunden an. Bestimmt die Zeitdauer, die der Replikator nach dem Empfang eines Vorgangs wartet, bevor er eine Bestätigung sendet. Für andere Vorgänge, die während dieses Zeitraums empfangen werden, werden die Bestätigungen in einer einzelnen Nachricht zurückgesendet. Dadurch wird der Netzwerkverkehr verringert, jedoch möglicherweise auch der Durchsatz des Replikators.|
|MaxCopyQueueSize|uint, Standardwert 1024|statischen|Dies ist der maximale Wert. Er definiert die Anfangsgröße für die Warteschlange, die Replikationsvorgänge verwaltet. Beachten Sie, dass der Wert eine Potenz von 2 sein muss. Wenn während der Laufzeit die Warteschlange auf diese Größe anwächst, werden Vorgänge zwischen den primären und sekundären Replikatoren gedrosselt.|
|MaxPrimaryReplicationQueueMemorySize|uint, Standardwert 0|statischen|Dies ist der maximale Wert für die primäre Replikationswarteschlange in Bytes.|
|MaxPrimaryReplicationQueueSize|uint, Standardwert 1024|statischen|Dies ist die maximale Anzahl von Vorgängen, die in der primären Replikationswarteschlange vorhanden sein können. Beachten Sie, dass der Wert eine Potenz von 2 sein muss.|
|MaxReplicationMessageSize|uint, Standardwert 52428800|statischen|Maximale Nachrichtengröße für Replikationsvorgänge. Der Standardwert ist 50 MB.|
|MaxSecondaryReplicationQueueMemorySize|uint, Standardwert 0|statischen|Dies ist der maximale Wert für die sekundäre Replikationswarteschlange in Bytes.|
|MaxSecondaryReplicationQueueSize|uint, Standardwert 2048|statischen|Dies ist die maximale Anzahl von Vorgängen, die in der sekundären Replikationswarteschlange vorhanden sein können. Beachten Sie, dass der Wert eine Potenz von 2 sein muss.|
|QueueHealthMonitoringInterval|TimeSpan, Standardwert Common::TimeSpan::FromSeconds(30)|statischen|Geben Sie die Zeitspanne in Sekunden an. Dieser Wert bestimmt den Zeitraum, der vom Replikator verwendet wird, um alle Warnungs-/Fehlerintegritätsereignisse in den Warteschlangen des Replikationsvorgangs zu überwachen. Der Wert „0“ deaktiviert die Systemüberwachung. |
|QueueHealthWarningAtUsagePercent|uint, Standardwert 80|statischen|Dieser Wert bestimmt die Verwendung der Replikationswarteschlange (in Prozent), nach der eine Warnung zu hoher Warteschlangenverwendung gemeldet wird. Dies geschieht nach einem Toleranzintervall, das durch QueueHealthMonitoringInterval angegeben wird. Wenn die Warteschlangenverwendung im Toleranzintervall unter diesen Prozentsatz fällt|
|ReplicatorAddress|string, Standardwert L"localhost:0"|statischen|Der Endpunkt in Form einer Zeichenfolge – „IP:Port“. Wird vom Windows Fabric-Replikator verwendet, um Verbindungen mit anderen Replikaten herzustellen, um Vorgänge zu senden/zu empfangen.|
|ReplicatorListenAddress|string, Standardwert L"localhost:0"|statischen|Der Endpunkt in Form einer Zeichenfolge – „IP:Port“. Wird vom Windows Fabric-Replikator verwendet, um Vorgänge von anderen Replikaten zu empfangen.|
|ReplicatorPublishAddress|string, Standardwert L"localhost:0"|statischen|Der Endpunkt in Form einer Zeichenfolge – „IP:Port“. Wird vom Windows Fabric-Replikator verwendet, um Vorgänge an andere Replikate zu senden.|
|RetryInterval|TimeSpan, Standardwert Common::TimeSpan::FromSeconds(5)|statischen|Geben Sie die Zeitspanne in Sekunden an. Wenn ein Vorgang nicht ausgeführt oder abgelehnt wurde, bestimmt dieser Zeitgeber, wie oft der Replikator das Senden des Vorgangs erneut versucht.|

## <a name="runas"></a>RunAs
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|RunAsAccountName |string, Standardwert "" |Dynamisch|Gibt den Namen des RunAs-Kontos an. Dies ist nur für die Kontotypen „DomainUser“ und „ManagedServiceAccount“ erforderlich. Gültige Werte sind „Domäne\Benutzer“ oder „user@domain“. |
|RunAsAccountType|string, Standardwert "" |Dynamisch|Gibt den Typ des RunAs-Kontos an. Dies ist für alle RunAs-Abschnitte erforderlich. Gültige Werte sind DomainUser/NetworkService/ManagedServiceAccount /LocalSystem.|
|RunAsPassword|string, Standardwert "" |Dynamisch|Gibt das Kennwort des RunAs-Kontos an. Dies ist nur für den Kontotyp „DomainUser“ erforderlich. |

## <a name="runasdca"></a>RunAs_DCA
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|RunAsAccountName |string, Standardwert "" |Dynamisch|Gibt den Namen des RunAs-Kontos an. Dies ist nur für die Kontotypen „DomainUser“ und „ManagedServiceAccount“ erforderlich. Gültige Werte sind „Domäne\Benutzer“ oder „user@domain“. |
|RunAsAccountType|string, Standardwert "" |Dynamisch|Gibt den Typ des RunAs-Kontos an. Dies ist für alle RunAs-Abschnitte erforderlich. Gültige Werte sind LocalUser/DomainUser/NetworkService/ManagedServiceAccount/LocalSystem. |
|RunAsPassword|string, Standardwert "" |Dynamisch|Gibt das Kennwort des RunAs-Kontos an. Dies ist nur für den Kontotyp „DomainUser“ erforderlich. |

## <a name="runasfabric"></a>RunAs_Fabric
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|RunAsAccountName |string, Standardwert "" |Dynamisch|Gibt den Namen des RunAs-Kontos an. Dies ist nur für die Kontotypen „DomainUser“ und „ManagedServiceAccount“ erforderlich. Gültige Werte sind „Domäne\Benutzer“ oder „user@domain“. |
|RunAsAccountType|string, Standardwert "" |Dynamisch|Gibt den Typ des RunAs-Kontos an. Dies ist für alle RunAs-Abschnitte erforderlich. Gültige Werte sind LocalUser/DomainUser/NetworkService/ManagedServiceAccount/LocalSystem. |
|RunAsPassword|string, Standardwert "" |Dynamisch|Gibt das Kennwort des RunAs-Kontos an. Dies ist nur für den Kontotyp „DomainUser“ erforderlich. |

## <a name="runashttpgateway"></a>RunAs_HttpGateway
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|RunAsAccountName |string, Standardwert "" |Dynamisch|Gibt den Namen des RunAs-Kontos an. Dies ist nur für die Kontotypen „DomainUser“ und „ManagedServiceAccount“ erforderlich. Gültige Werte sind „Domäne\Benutzer“ oder „user@domain“. |
|RunAsAccountType|string, Standardwert "" |Dynamisch|Gibt den Typ des RunAs-Kontos an. Dies ist für alle RunAs-Abschnitte erforderlich. Gültige Werte sind LocalUser/DomainUser/NetworkService/ManagedServiceAccount/LocalSystem. |
|RunAsPassword|string, Standardwert "" |Dynamisch|Gibt das Kennwort des RunAs-Kontos an. Dies ist nur für den Kontotyp „DomainUser“ erforderlich. |

## <a name="security"></a>Sicherheit
| **Parameter** | **Zulässige Werte** |**Upgraderichtlinie**| **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|AADClientApplication|string, Standardwert L""|statischen|Der Name der nativen Clientanwendung oder -ID, die Fabric-Clients darstellt. |
|AADClusterApplication|string, Standardwert L""|statischen|Web-API-Anwendungsname oder -ID, der bzw. die den Cluster darstellt. |
|AADTenantId|string, Standardwert L""|statischen|Mandanten-ID (GUID) |
|AdminClientCertThumbprints|string, Standardwert L""|Dynamisch|Fingerabdrücke von Zertifikaten, die von Clients in der Rolle „Administrator“ verwendet werden. Es handelt sich um eine durch Kommas getrennte Liste. |
|AdminClientClaims|string, Standardwert L""|Dynamisch|Alle möglichen Ansprüche, die von Administratorclients erwartet werden. Gleiches Format wie ClientClaims. Diese Liste wird intern ClientClaims hinzugefügt. Es besteht daher keine Notwendigkeit, die gleichen Einträge auch ClientClaims hinzuzufügen. |
|AdminClientIdentities|string, Standardwert L""|Dynamisch|Windows-Identitäten der Fabric-Clients in der Rolle „Administrator“. Wird verwendet, um privilegierte Fabric-Vorgänge zu autorisieren. Es handelt sich um eine durch Kommas getrennte Liste. Jeder Eintrag ist ein Domänenkonto- oder Gruppenname. Der Einfachheit halber wird dem Konto, das „fabric.exe“ ausführt, automatisch die Rolle „Administrator“ zugewiesen. Dies gilt auch für die Gruppe ServiceFabricAdministrators. |
|CertificateExpirySafetyMargin|TimeSpan, Standardwert Common::TimeSpan::FromMinutes(43200)|statischen|Geben Sie die Zeitspanne in Sekunden an. Sicherheitsspanne für den Zertifikatablauf. Der Status des Integritätsberichts des Zertifikats ändert sich aus „OK“ in „Warning“, wenn das Ablaufdatum diese Angabe überschreitet. Der Standardwert sind 30 Tage. |
|CertificateHealthReportingInterval|TimeSpan, Standardwert Common::TimeSpan::FromSeconds(3600 * 8)|statischen|Geben Sie die Zeitspanne in Sekunden an. Gibt das Intervall für Zertifikatintegritätsberichte an. Der Standardwert ist 8 Stunden. Bei Festlegung auf 0 werden Zertifikatintegritätsberichte deaktiviert. |
|ClientCertThumbprints|string, Standardwert L""|Dynamisch|Fingerabdrücke von Zertifikaten, die vom Clients für die Kommunikation mit dem Cluster verwendet werden. Cluster verwenden diese Angabe, um eingehende Verbindungen zu autorisieren. Es handelt sich um eine durch Kommas getrennte Liste. |
|ClientClaimAuthEnabled|Boolesch, Standardwert FALSE|statischen|Gibt an, ob auf Clients anspruchsbasierte Authentifizierung aktiviert ist. Durch Festlegung auf TRUE wird implizit ClientRoleEnabled festlegt. |
|ClientClaims|string, Standardwert L""|Dynamisch|Alle möglichen Ansprüche, die von Clients zum Herstellen einer Verbindung mit dem Gateway erwartet werden. Dies ist eine ODER-Liste: ClaimsEntry \|\| Claims Entry \|\| ClaimsEntry... Jeder ClaimsEntry ist eine UND-Liste: ClaimType=ClaimValue && ClaimType=ClaimValue && ClaimType=ClaimValue... |
|ClientIdentities|string, Standardwert L""|Dynamisch|Windows-Identitäten von FabricClient. Naming Gateway verwendet diese Angaben zum Autorisieren eingehender Verbindungen. Es handelt sich um eine durch Kommas getrennte Liste. Jeder Eintrag ist ein Domänenkonto- oder Gruppenname. Der Einfachheit halber ist das Konto, das „fabric.exe“ ausführt, automatisch zulässig. Dies gilt auch für die Gruppen ServiceFabricAllowedUsers und ServiceFabricAdministrators. |
|ClientRoleEnabled|Boolesch, Standardwert FALSE|statischen|Gibt an, ob die Rolle „Client“ aktiviert ist. Bei Festlegung auf TRUE werden Clients anhand ihrer Identitäten Rollen zugewiesen. Für V2 bedeutet die Aktivierung, dass Clients, die nicht in AdminClientCommonNames/AdminClientIdentities enthalten sind, nur schreibgeschützte Vorgänge ausführen können. |
|ClusterCertThumbprints|string, Standardwert L""|Dynamisch|Fingerabdrücke von Zertifikaten, die dem Cluster beitreten dürfen. Eine durch Kommas getrennte Liste. |
|ClusterCredentialType|string, Standardwert L"None"|Nicht zulässig|Gibt den Typ der Sicherheitsanmeldeinformationen an, die verwendet werden sollen, um den Cluster zu sichern. Gültige Werte sind „None/X509/Windows“ |
|ClusterIdentities|string, Standardwert L""|Dynamisch|Windows-Identitäten der Clusterknoten, für die Autorisierung der Clustermitgliedschaft verwendet. Es handelt sich um eine durch Kommas getrennte Liste. Jeder Eintrag ist ein Domänenkonto- oder Gruppenname. |
|ClusterSpn|string, Standardwert L""|Nicht zulässig|Der Dienstprinzipalname des Clusters, wenn Fabric als einzelner Domänenbenutzer (gMSA-/Domänenbenutzerkonto) ausgeführt wird. Es handelt sich um den SPN der Leaselistener und der Listener in „fabric.exe“: Verbundlistener, interne Replikationslistener, Dienstlistener zur Laufzeit und Naming Gateway-Listener. Diese Angabe sollte leer bleiben, wenn Fabric als Computerkonten ausgeführt wird. In diesem Fall wird der Computelistener-SPN aus der Listenertransportadresse verbunden. |
|CrlCheckingFlag|uint, Standardwert 0 x 40000000|Dynamisch|Standardflag für die Überprüfung der Zertifikatkette. Kann von einem komponentenspezifischen Flag überschrieben werden, z.B. Federation/X509CertChainFlags 0x10000000 CERT_CHAIN_REVOCATION_CHECK_END_CERT 0x20000000 CERT_CHAIN_REVOCATION_CHECK_CHAIN 0x40000000 CERT_CHAIN_REVOCATION_CHECK_CHAIN_EXCLUDE_ROOT 0x80000000 CERT_CHAIN_REVOCATION_CHECK_CACHE_ONLY. Bei Festlegen des Werts auf 0 wird die Zertifikatssperrlistenüberprüfung deaktiviert. Die vollständige Liste der unterstützten Werte wird durch dwFlags von CertGetCertificateChain dokumentiert: http://msdn.microsoft.com/library/windows/desktop/aa376078(v=vs.85).aspx |
|CrlDisablePeriod|TimeSpan, Standardwert Common::TimeSpan::FromMinutes(15)|Dynamisch|Geben Sie die Zeitspanne in Sekunden an. Gibt an, wie lange die Zertifikatsperrlistenüberprüfung für ein bestimmtes Zertifikat nach einem Offlinefehler deaktiviert wird, wenn der Zertifikatsperrlisten-Offlinefehler ignoriert werden kann. |
|CrlOfflineHealthReportTtl|TimeSpan, Standardwert Common::TimeSpan::FromMinutes(1440)|Dynamisch|Geben Sie die Zeitspanne in Sekunden an. |
|DisableFirewallRuleForDomainProfile| Boolesch, Standardwert TRUE |statischen| Gibt an, ob die Firewallregel das Domänenprofil nicht aktiviert werden soll. |
|DisableFirewallRuleForPrivateProfile| Boolesch, Standardwert TRUE |statischen| Gibt an, ob die Firewallregel das private Profil nicht aktiviert werden soll. | 
|DisableFirewallRuleForPublicProfile| Boolesch, Standardwert TRUE | statischen|Gibt an, ob die Firewallregel das öffentliche Profil nicht aktiviert werden soll. |
|FabricHostSpn| string, Standardwert L"" |statischen| Der Dienstprinzipalname von FabricHost, wenn Fabric als einzelner Domänenbenutzer (gMSA-/Domänenbenutzerkonto) und FabricHost unter dem Computerkonto ausgeführt wird. Es handelt sich um den SPN von IPC-Listenern für FabricHost. Standardmäßig sollte diese Angabe leer bleiben, weil FabricHost unter dem Computerkonto ausgeführt wird |
|IgnoreCrlOfflineError|Boolesch, Standardwert FALSE|Dynamisch|Gibt an, ob ein Zertifikatsperrlisten-Offlinefehler ignoriert werden soll, wenn die Serverseite eingehende Clientzertifikate überprüft. |
|IgnoreSvrCrlOfflineError|Boolesch, Standardwert TRUE|Dynamisch|Gibt an, ob ein Zertifikatsperrlisten-Offlinefehler ignoriert werden soll, wenn die Clientseite eingehende Serverzertifikate überprüft. Der Standardwert ist TRUE. Für Angriffe mit gesperrten Serverzertifikaten ist eine Gefährdung des DNS erforderlich. Dies ist schwieriger als mit gesperrten Clientzertifikaten. |
|ServerAuthCredentialType|string, Standardwert L"None"|statischen|Gibt den Typ der Sicherheitsanmeldeinformationen an, die verwendet werden sollen, um die Kommunikation zwischen FabricClient und dem Cluster zu sichern. Gültige Werte sind „None/X509/Windows“ |
|ServerCertThumbprints|string, Standardwert L""|Dynamisch|Fingerabdrücke von Serverzertifikaten, die vom Cluster für die Kommunikation mit Clients verwendet werden. Clients verwenden diese Angabe, um den Cluster zu authentifizieren. Es handelt sich um eine durch Kommas getrennte Liste. |
|SettingsX509StoreName| string, Standardwert L"MY"| Dynamisch|Der X509-Zertifikatspeicher, der von Fabric für den Konfigurationsschutz verwendet wird. |
|X509Folder|string, Standarwert /var/lib/waagent|statischen|Der Ordner, in dem X509 Zertifikate und private Schlüssel gespeichert sind. |

## <a name="securityadminclientx509names"></a>Security/AdminClientX509Names
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|PropertyGroup|X509NameMap, Standardwert None|Dynamisch| |

## <a name="securityclientaccess"></a>Security/ClientAccess
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|ActivateNode |string, Standardwert „Admin“ |Dynamisch| Sicherheitskonfiguration für die Aktivierung eines Knotens. |
|CancelTestCommand |string, Standardwert „Admin“ |Dynamisch| Bricht einen bestimmten TestCommand ab, wenn er aktiv ist. |
|CodePackageControl |string, Standardwert „Admin“ |Dynamisch| Sicherheitskonfiguration für das Neustarten von Codepaketen. |
|CreateApplication |string, Standardwert „Admin“ | Dynamisch|Sicherheitskonfiguration für die Anwendungserstellung. |
|CreateComposeDeployment|string, Standardwert L"Admin"| Dynamisch|Erstellt eine Compose-Bereitstellung, die durch Compose-Dateien beschrieben wird. |
|CreateName |string, Standardwert „Admin“ |Dynamisch|Sicherheitskonfiguration für die Erstellung des Benennungs-URI. |
|CreateService |string, Standardwert „Admin“ |Dynamisch| Sicherheitskonfiguration für die Diensterstellung. |
|CreateServiceFromTemplate |string, Standardwert „Admin“ |Dynamisch|Sicherheitskonfiguration für die Diensterstellung aus einer Vorlage. |
|DeactivateNode |string, Standardwert „Admin“ |Dynamisch| Sicherheitskonfiguration für die Deaktivierung eines Knotens. |
|DeactivateNodesBatch |string, Standardwert „Admin“ |Dynamisch| Sicherheitskonfiguration für die Deaktivierung mehrerer Knoten. |
|Löschen |string, Standardwert „Admin“ |Dynamisch| Sicherheitskonfigurationen für den Imagespeicherclient-Löschvorgang. |
|DeleteApplication |string, Standardwert „Admin“ |Dynamisch| Sicherheitskonfiguration für die Anwendungslöschung. |
|DeleteComposeDeployment|string, Standardwert L"Admin"| Dynamisch|Löscht die Compose-Bereitstellung. |
|DeleteName |string, Standardwert „Admin“ |Dynamisch|Sicherheitskonfiguration für die Löschung des Benennungs-URI. |
|DeleteService |string, Standardwert „Admin“ |Dynamisch|Sicherheitskonfiguration für die Dienstlöschung. |
|EnumerateProperties |string, Standardwert „Admin\|\|User“ | Dynamisch|Sicherheitskonfiguration für die Enumeration von Benennungseigenschaften. |
|EnumerateSubnames |string, Standardwert „Admin\|\|User“ |Dynamisch| Sicherheitskonfiguration für die Enumeration von Benennungs-URIs. |
|FileContent |string, Standardwert „Admin“ |Dynamisch| Sicherheitskonfiguration für die Imagespeicherclient-Dateiübertragung (außerhalb des Clusters). |
|FileDownload |string, Standardwert „Admin“ |Dynamisch| Sicherheitskonfiguration für die Imagespeicherclient-Dateidownloadinitiierung (außerhalb des Clusters). |
|FinishInfrastructureTask |string, Standardwert „Admin“ |Dynamisch| Sicherheitskonfiguration für das Beenden von Infrastrukturaufgaben. |
|GetChaosReport | string, Standardwert „Admin\|\|User“ |Dynamisch| Ruft den Status des Chaos innerhalb eines angegebenen Zeitbereichs ab. |
|GetClusterConfiguration | string, Standardwert „Admin\|\|User“ | Dynamisch|Löst GetClusterConfiguration auf einer Partition aus. |
|GetClusterConfigurationUpgradeStatus | string, Standardwert „Admin\|\|User“ |Dynamisch| Löst GetClusterConfigurationUpgradeStatus auf einer Partition aus. |
|GetFabricUpgradeStatus |string, Standardwert „Admin\|\|User“ |Dynamisch| Sicherheitskonfiguration für das Abrufen des Clusterupgradestatus. |
|GetNodeDeactivationStatus |string, Standardwert „Admin“ |Dynamisch| Sicherheitskonfiguration für das Überprüfen des Deaktivierungstatus. |
|GetNodeTransitionProgress | string, Standardwert „Admin\|\|User“ |Dynamisch| Sicherheitskonfiguration für das Abrufen des Status eines Knotenübergangsbefehls. |
|GetPartitionDataLossProgress | string, Standardwert „Admin\|\|User“ | Dynamisch|Ruft den Status für einen API-Aufruf zum Auslösen von Datenverlusten ab. |
|GetPartitionQuorumLossProgress | string, Standardwert „Admin\|\|User“ |Dynamisch| Ruft den Status für einen API-Aufruf zum Auslösen von Quorumverlusten ab. |
|GetPartitionRestartProgress | string, Standardwert „Admin\|\|User“ |Dynamisch| Ruft den Status für einen API-Aufruf zum Neustarten einer Partition ab. |
|GetServiceDescription |string, Standardwert „Admin\|\|User“ |Dynamisch| Sicherheitskonfiguration für Dienstbenachrichtigungen mit langen Abrufzeiten und das Lesen von Dienstbeschreibungen. |
|GetStagingLocation |string, Standardwert „Admin“ |Dynamisch| Sicherheitskonfiguration für den Abruf des Imagespeicherclient-Stagingspeicherorts. |
|GetStoreLocation |string, Standardwert „Admin“ |Dynamisch| Sicherheitskonfiguration für den Abruf des Imagespeicherclient-Speicherorts. |
|GetUpgradeOrchestrationServiceState|string, Standardwert L"Admin"| Dynamisch|Schließt GetUpgradeOrchestrationServiceState für eine Partition ein. |
|GetUpgradesPendingApproval |string, Standardwert „Admin“ |Dynamisch| Löst GetUpgradesPendingApproval auf einer Partition aus. |
|GetUpgradeStatus |string, Standardwert „Admin\|\|User“ |Dynamisch| Sicherheitskonfiguration für das Abrufen des Anwendungsupgradestatus. |
|InternalList |string, Standardwert „Admin“ | Dynamisch|Sicherheitskonfiguration für den Imagespeicherclient-Dateiauflistungsvorgang (intern). |
|InvokeInfrastructureCommand |string, Standardwert „Admin“ |Dynamisch| Sicherheitskonfiguration für Befehle zum Verwalten von Infrastrukturaufgaben. |
|InvokeInfrastructureQuery |string, Standardwert „Admin\|\|User“ | Dynamisch|Sicherheitskonfiguration für das Abfragen von Infrastrukturaufgaben. |
|Auflisten |string, Standardwert „Admin\|\|User“ | Dynamisch|Sicherheitskonfiguration für den Imagespeicherclient-Dateiauflistungsvorgang. |
|MoveNextFabricUpgradeDomain |string, Standardwert „Admin“ |Dynamisch| Sicherheitskonfiguration für das Fortsetzen von Clusterupgrades mit einer expliziten Upgradedomäne. |
|MoveNextUpgradeDomain |string, Standardwert „Admin“ |Dynamisch| Sicherheitskonfiguration für das Fortsetzen von Anwendungsupgrades mit einer expliziten Upgradedomäne. |
|MoveReplicaControl |string, Standardwert „Admin“ | Dynamisch|Verschieben von Replikaten. |
|NameExists |string, Standardwert „Admin\|\|User“ | Dynamisch|Sicherheitskonfiguration für Überprüfungen auf das Vorhandensein von Benennungs-URIs. |
|NodeControl |string, Standardwert „Admin“ |Dynamisch| Sicherheitskonfiguration für das Starten, Beenden und Neustarten von Knoten. |
|NodeStateRemoved |string, Standardwert „Admin“ |Dynamisch| Sicherheitskonfiguration für Berichte zur Entfernung des Knotenstatus. |
|Pingen |string, Standardwert „Admin\|\|User“ |Dynamisch| Sicherheitskonfiguration für Clientpingvorgänge. |
|PredeployPackageToNode |string, Standardwert „Admin“ |Dynamisch| API vor der Bereitstellung. |
|PrefixResolveService |string, Standardwert „Admin\|\|User“ |Dynamisch| Sicherheitskonfiguration für die Dienstpräfixauflösung auf Konfliktbasis. |
|PropertyReadBatch |string, Standardwert „Admin\|\|User“ |Dynamisch| Sicherheitskonfiguration für Lesevorgänge in Benennungseigenschaften. |
|PropertyWriteBatch |string, Standardwert „Admin“ |Dynamisch|Sicherheitskonfigurationen für Schreibvorgänge in Benennungseigenschaften. |
|ProvisionApplicationType |string, Standardwert „Admin“ |Dynamisch| Sicherheitskonfiguration für die Anwendungstypbereitstellung. |
|ProvisionFabric |string, Standardwert „Admin“ |Dynamisch| Sicherheitskonfiguration für die MSI- und/oder Clustermanifestbereitstellung. |
|Abfragen |string, Standardwert „Admin\|\|User“ |Dynamisch| Sicherheitskonfiguration für Abfragen. |
|RecoverPartition |string, Standardwert „Admin“ | Dynamisch|Sicherheitskonfiguration für die Wiederherstellung einer Partition. |
|RecoverPartitions |string, Standardwert „Admin“ | Dynamisch|Sicherheitskonfiguration für die Wiederherstellung von Partitionen. |
|RecoverServicePartitions |string, Standardwert „Admin“ |Dynamisch| Sicherheitskonfiguration für die Wiederherstellung von Dienstpartitionen. |
|RecoverSystemPartitions |string, Standardwert „Admin“ |Dynamisch| Sicherheitskonfiguration für die Wiederherstellung von Systemdienstpartitionen. |
|RemoveNodeDeactivations |string, Standardwert „Admin“ |Dynamisch| Sicherheitskonfiguration für das Zurücksetzen der Deaktivierung mehrerer Knoten. |
|ReportFabricUpgradeHealth |string, Standardwert „Admin“ |Dynamisch| Sicherheitskonfiguration für das Fortsetzen von Clusterupgrades mit der aktuellen Upgradedomäne. |
|ReportFault |string, Standardwert „Admin“ |Dynamisch| Sicherheitskonfiguration für Fehlerberichte. |
|ReportHealth |string, Standardwert „Admin“ |Dynamisch| Sicherheitskonfiguration für Integritätsberichte. |
|ReportUpgradeHealth |string, Standardwert „Admin“ |Dynamisch| Sicherheitskonfiguration für das Fortsetzen von Anwendungsupgrades der aktuellen Upgradedomäne. |
|ResetPartitionLoad |string, Standardwert „Admin\|\|User“ |Dynamisch| Sicherheitskonfiguration für das Zurücksetzen der failoverUnit-Auslastung. |
|ResolveNameOwner |string, Standardwert „Admin\|\|User“ | Dynamisch|Sicherheitskonfiguration für die Auflösung des Benennungs-URI-Besitzers. |
|ResolvePartition |string, Standardwert „Admin\|\|User“ | Dynamisch|Sicherheitskonfiguration für die Auflösung von Systemdiensten. |
|ResolveService |string, Standardwert „Admin\|\|User“ |Dynamisch| Sicherheitskonfiguration für die Dienstauflösung auf Konfliktbasis. |
|ResolveSystemService|string, Standardwert L"Admin\|\|User"|Dynamisch| Sicherheitskonfiguration für die Auflösung von Systemdiensten. |
|RollbackApplicationUpgrade |string, Standardwert „Admin“ |Dynamisch| Sicherheitskonfiguration für das Rollback von Anwendungsupgrades. |
|RollbackFabricUpgrade |string, Standardwert „Admin“ |Dynamisch| Sicherheitskonfiguration für das Rollback von Clusterupgrades. |
|ServiceNotifications |string, Standardwert „Admin\|\|User“ |Dynamisch| Sicherheitskonfiguration für Dienstbenachrichtigungen auf Ereignisbasis. |
|GetUpgradeOrchestrationServiceState|string, Standardwert L"Admin"| Dynamisch|Schließt SetUpgradeOrchestrationServiceState für eine Partition ein. |
|StartApprovedUpgrades |string, Standardwert „Admin“ |Dynamisch| Löst StartApprovedUpgrades auf einer Partition aus. |
|StartChaos |string, Standardwert „Admin“ |Dynamisch| Startet Chaos, wenn es noch nicht gestartet wurde. |
|StartClusterConfigurationUpgrade |string, Standardwert „Admin“ |Dynamisch| Löst StartClusterConfigurationUpgrade auf einer Partition aus. |
|StartInfrastructureTask |string, Standardwert „Admin“ | Dynamisch|Sicherheitskonfiguration für das Starten von Infrastrukturaufgaben. |
|StartNodeTransition |string, Standardwert „Admin“ |Dynamisch| Sicherheitskonfiguration für das Starten eines Knotenübergangs. |
|StartPartitionDataLoss |string, Standardwert „Admin“ |Dynamisch| Löst Datenverluste auf einer Partition aus. |
|StartPartitionQuorumLoss |string, Standardwert „Admin“ |Dynamisch| Löst Quorumverluste auf einer Partition aus. |
|StartPartitionRestart |string, Standardwert „Admin“ |Dynamisch| Startet einige oder alle Replikate einer Partition gleichzeitig neu. |
|StopChaos |string, Standardwert „Admin“ |Dynamisch| Beendet Chaos, falls es gestartet wurde. |
|ToggleVerboseServicePlacementHealthReporting | string, Standardwert „Admin\|\|User“ |Dynamisch| Sicherheitskonfiguration für Umschalten von ausführlichen Integritätsberichten zur Dienstplatzierung. |
|UnprovisionApplicationType |string, Standardwert „Admin“ |Dynamisch| Sicherheitskonfiguration für die Aufhebung der Anwendungstypbereitstellung. |
|UnprovisionFabric |string, Standardwert „Admin“ |Dynamisch| Sicherheitskonfiguration für die Aufhebung der MSI- und/oder Clustermanifestbereitstellung. |
|UnreliableTransportControl |string, Standardwert „Admin“ |Dynamisch| Unzuverlässiger Transport zum Hinzufügen und Entfernen von Verhaltensweisen. |
|UpdateService |string, Standardwert „Admin“ |Dynamisch|Sicherheitskonfiguration für Dienstupdates. |
|UpgradeApplication |string, Standardwert „Admin“ |Dynamisch| Sicherheitskonfiguration für das Starten oder Unterbrechen von Anwendungsupgrades. |
|UpgradeComposeDeployment|string, Standardwert L"Admin"| Dynamisch|Führt ein Upgrade der Compose-Bereitstellung aus. |
|UpgradeFabric |string, Standardwert „Admin“ |Dynamisch| Sicherheitskonfiguration für das Starten von Clusterupgrades. |
|Hochladen |string, Standardwert „Admin“ | Dynamisch|Sicherheitskonfiguration für den Imagespeicherclient-Uploadvorgang. |

## <a name="securityclientcertificateissuerstores"></a>Security/ClientCertificateIssuerStores
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|PropertyGroup|IssuerStoreKeyValueMap, Standardwert: None |Dynamisch|X509-Zertifikatspeicher des Zertifikatausstellers für Clientzertifikate; Name = clientIssuerCN; Wert = kommagetrennte Liste der Speicher |

## <a name="securityclientx509names"></a>Security/ClientX509Names
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|PropertyGroup|X509NameMap, Standardwert None|Dynamisch| |

## <a name="securityclustercertificateissuerstores"></a>Security/ClientCertificateIssuerStores
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|PropertyGroup|IssuerStoreKeyValueMap, Standardwert: None |Dynamisch|X509-Zertifikatspeicher des Zertifikatausstellers für Clusterzertifikate; Name = clusterIssuerCN; Wert = kommagetrennte Liste der Speicher |

## <a name="securityclusterx509names"></a>Security/ClusterX509Names
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|PropertyGroup|X509NameMap, Standardwert None|Dynamisch| |

## <a name="securityservercertificateissuerstores"></a>Security/ServerCertificateIssuerStores
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|PropertyGroup|IssuerStoreKeyValueMap, Standardwert: None |Dynamisch|X509-Zertifikatspeicher des Zertifikatausstellers für Serverzertifikate; Name = serverIssuerCN; Wert = kommagetrennte Liste der Speicher |

## <a name="securityserverx509names"></a>Security/ServerX509Names
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|PropertyGroup|X509NameMap, Standardwert None|Dynamisch| |

## <a name="setup"></a>Einrichtung
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|ContainerNetworkName|string, Standardwert L""| statischen |Der zu verwendende Netzwerkname, wenn ein Containernetzwerk eingerichtet wird.|
|ContainerNetworkSetup|Boolesch, Standardwert FALSE| statischen |Gibt an, ob ein Containernetzwerk eingerichtet werden soll.|
|FabricDataRoot |Zeichenfolge | Nicht zulässig |Das Service Fabric-Datenstammverzeichnis. Standardwert für Azure ist „d:\svcfab“. |
|FabricLogRoot |Zeichenfolge | Nicht zulässig |Das Service Fabric-Protokollstammverzeichnis. Hier werden SF-Protokolle und Ablaufverfolgungen platziert. |
|NodesToBeRemoved|string, Standardwert ""| Dynamisch |Der Knoten, die als Teil des Konfigurationsupgrades entfernt werden sollen. (Nur für eigenständige Bereitstellungen)|
|ServiceRunAsAccountName |Zeichenfolge | Nicht zulässig |Der Kontoname, unter dem der Fabric-Hostdienst ausgeführt werden soll. |
|SkipFirewallConfiguration |Boolesch, Standardwert „false“ | Nicht zulässig |Gibt an, ob Firewalleinstellungen vom System festgelegt werden müssen oder nicht. Dies gilt nur, wenn Sie die Windows-Firewall verwenden. Wenn Sie Firewalls von Drittanbietern verwenden, müssen Sie die Ports für das System und die zu verwendenden Anwendungen öffnen. |

## <a name="tokenvalidationservice"></a>TokenValidationService
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|Anbieter |string, Standardwert „DSTS“ |statischen|Durch Trennzeichen getrennte Liste von zu aktivierenden Tokenvalidierungsanbietern (gültige Anbieter sind: DSTS, AAD). Derzeit kann immer nur ein einzelnen Anbieter aktiviert werden. |

## <a name="traceetw"></a>Trace/Etw
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|Ebene |Ganze Zahl, Standardwert 4 | Dynamisch |Die Trace/Etw-Ebene kann die Werte 1, 2, 3 oder 4 annehmen. Zur Unterstützung muss die Trace-Ebene 4 beibehalten werden. |

## <a name="transactionalreplicator"></a>TransactionalReplicator
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|BatchAcknowledgementInterval | Zeit in Sekunden, Standardwert 0,015 | statischen | Geben Sie die Zeitspanne in Sekunden an. Bestimmt die Zeitdauer, die der Replikator nach dem Empfang eines Vorgangs wartet, bevor er eine Bestätigung sendet. Für andere Vorgänge, die während dieses Zeitraums empfangen werden, werden die Bestätigungen in einer einzelnen Nachricht zurückgesendet. Dadurch wird der Netzwerkverkehr verringert, jedoch möglicherweise auch der Durchsatz des Replikators. |
|CheckpointThresholdInMB |Ganze Zahl, Standardwert 50 |statischen|Ein Prüfpunkt wird initiiert, wenn die Protokollnutzung diesen Wert überschreitet. |
|InitialPrimaryReplicationQueueSize |Uint, Standardwert 64 | statischen |Dieser Wert definiert die Anfangsgröße für die Warteschlange, die Replikationsvorgänge auf dem primären Replikator verwaltet. Beachten Sie, dass der Wert eine Potenz von 2 sein muss.|
|InitialSecondaryReplicationQueueSize |Uint, Standardwert 64 | statischen |Dieser Wert definiert die Anfangsgröße für die Warteschlange, die Replikationsvorgänge auf dem sekundären Replikator verwaltet. Beachten Sie, dass der Wert eine Potenz von 2 sein muss. |
|MaxAccumulatedBackupLogSizeInMB |Ganze Zahl, Standardwert 800 |statischen|Die maximale kumulierte Größe von Sicherungsprotokollen in einer bestimmten Sicherungsprotokollkette (in MB). Die Anforderung einer inkrementellen Sicherung ist nicht erfolgreich, wenn die inkrementelle Sicherung ein Sicherungsprotokoll generiert, durch das die kumulierte Größe der Sicherungsprotokolle seit der entsprechenden vollständigen Sicherung diese Größe überschreitet. In einem solchen Fall muss der Benutzer eine vollständige Sicherung durchführen. |
|MaxCopyQueueSize |Uint, Standardwert 16384 | statischen |Dies ist der maximale Wert. Er definiert die Anfangsgröße für die Warteschlange, die Replikationsvorgänge verwaltet. Beachten Sie, dass der Wert eine Potenz von 2 sein muss. Wenn während der Laufzeit die Warteschlange auf diese Größe anwächst, werden Vorgänge zwischen den primären und sekundären Replikatoren gedrosselt. |
|MaxMetadataSizeInKB |Ganze Zahl, Standardwert 4 |Nicht zulässig|Maximale Größe der Metadaten des Protokolldatenstroms. |
|MaxPrimaryReplicationQueueMemorySize |Uint, Standardwert 0 | statischen |Dies ist der maximale Wert für die primäre Replikationswarteschlange in Bytes. |
|MaxPrimaryReplicationQueueSize |Uint, Standardwert 8192 | statischen |Dies ist die maximale Anzahl von Vorgängen, die in der primären Replikationswarteschlange vorhanden sein können. Beachten Sie, dass der Wert eine Potenz von 2 sein muss. |
|MaxRecordSizeInKB |Uint, Standardwert 1024 |Nicht zulässig| Maximale Größe des Datensatzes eines Protokolldatenstroms. |
|MaxReplicationMessageSize |Uint, Standardwert 52428800 | statischen | Maximale Nachrichtengröße für Replikationsvorgänge. Der Standardwert ist 50 MB. |
|MaxSecondaryReplicationQueueMemorySize |Uint, Standardwert 0 | statischen |Dies ist der maximale Wert für die sekundäre Replikationswarteschlange in Bytes. |
|MaxSecondaryReplicationQueueSize |Uint, Standardwert 16384 | statischen |Dies ist die maximale Anzahl von Vorgängen, die in der sekundären Replikationswarteschlange vorhanden sein können. Beachten Sie, dass der Wert eine Potenz von 2 sein muss. |
|MaxWriteQueueDepthInKB |Ganze Zahl, Standardwert 0 |Nicht zulässig| Ganze Zahl für die maximale Tiefe der Schreibwarteschlange, die für die zentrale Protokollierung verwendet werden kann, entsprechend der Angabe in Kilobytes für das Protokoll, das diesem Replikat zugeordnet ist. Dieser Wert ist die maximale Anzahl von Bytes, die während der Updates für die zentrale Protokollierung ausstehend sein können. Der Wert kann 0 sein, damit die zentrale Protokollierung einen geeigneten Wert berechnet, oder ein Vielfaches von 4. |
|MinLogSizeInMB |Ganze Zahl, Standardwert 0 |statischen|Die minimale Größe des Transaktionsprotokolls. Das Protokoll kann auf keine Größe unterhalb dieser Einstellung gekürzt werden. 0 gibt an, dass die minimale Protokollgröße durch den Replikator anhand anderer Einstellungen bestimmt wird. Bei einem höheren Wert erhöht sich die Möglichkeit zur Erstellung von Teilkopien und inkrementellen Sicherungen, da sich dadurch das Risiko verringert, dass relevante Protokolldatensätze abgeschnitten werden. |
|ReplicatorAddress |string, Standardwert „localhost:0“ | statischen | Der Endpunkt in Form einer Zeichenfolge – „IP:Port“. Wird vom Windows Fabric-Replikator verwendet, um Verbindungen mit anderen Replikaten herzustellen, um Vorgänge zu senden/zu empfangen. |
|SecondaryClearAcknowledgedOperations |Boolesch, Standardwert „false“ | statischen |Boolescher Wert, der steuert, ob die Vorgänge auf dem sekundären Replikator gelöscht werden, sobald sie auf dem primären Replikator bestätigt wurden (auf den Datenträger geleert wurden). Die Festlegung auf TRUE kann zu zusätzlichen Datenträger-Lesevorgängen auf dem neuen primären Replikator führen, wenn Replikate nach einem Failover wiederhergestellt werden. |
|SharedLogId |Zeichenfolge |Nicht zulässig|Freigegebener Protokollbezeichner. Dies ist eine GUID, und sie sollte für jedes freigegebene Protokoll eindeutig sein. |
|SharedLogPath |Zeichenfolge |Nicht zulässig|Pfad zum freigegebenen Protokoll. Wenn dieser Wert leer ist, wird das freigegebene Standardprotokoll verwendet. |
|SlowApiMonitoringDuration |Zeit in Sekunden, Standardwert 300 |statischen| Geben Sie für die API eine Dauer an, bevor eine Warnung zu einem Integritätsereignis ausgelöst wird.|

## <a name="transport"></a>Transport
| **Parameter** | **Zulässige Werte** |**Upgraderichtlinie** |**Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|ConnectionOpenTimeout|TimeSpan, Standardwert Common::TimeSpan::FromSeconds(60)|statischen|Geben Sie die Zeitspanne in Sekunden an. Timeout für das Einrichten der Verbindung auf der eingehenden und annehmenden Seite (einschließlich der Sicherheitsaushandlung im sicheren Modus). |
|ResolveOption|string, Standardwert L"unspecified"|statischen|Bestimmt, wie der FQDN aufgelöst wird.  Gültige Werte sind „unspecified/ipv4/ipv6“. |

## <a name="upgradeorchestrationservice"></a>UpgradeOrchestrationService
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|AutoupgradeEnabled | Boolesch, Standardwert „true“ |statischen| Automatische Abruf- und Aktualisierungsaktion basierend auf einer Datei für den Zielzustand. |
|MinReplicaSetSize |Ganze Zahl, Standardwert 0 |statischen |MinReplicaSetSize für UpgradeOrchestrationService.
|PlacementConstraints | string, Standardwert "" |statischen| PlacementConstraints für UpgradeOrchestrationService. |
|QuorumLossWaitDuration | Zeit in Sekunden, Standardwert MaxValue |statischen| Geben Sie die Zeitspanne in Sekunden an. QuorumLossWaitDuration für UpgradeOrchestrationService. |
|ReplicaRestartWaitDuration | Zeit in Sekunden, Standardwert 60 Minuten|statischen| Geben Sie die Zeitspanne in Sekunden an. ReplicaRestartWaitDuration für UpgradeOrchestrationService. |
|StandByReplicaKeepDuration | Zeit in Sekunden, Standardwert 60*24*7 Minuten |statischen| Geben Sie die Zeitspanne in Sekunden an. StandByReplicaKeepDuration für UpgradeOrchestrationService. |
|TargetReplicaSetSize |Ganze Zahl, Standardwert 0 |statischen |TargetReplicaSetSize für UpgradeOrchestrationService. |
|UpgradeApprovalRequired | Boolesch, Standardwert „false“ | statischen|Einstellung, damit bei der Codeaktualisierung die Genehmigung durch den Administrator erforderlich ist, bevor fortgefahren werden kann. |

## <a name="upgradeservice"></a>UpgradeService
| **Parameter** | **Zulässige Werte** | **Upgraderichtlinie** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
|BaseUrl | string, Standardwert "" |statischen|BaseUrl für UpgradeService. |
|ClusterId | string, Standardwert "" |statischen|ClusterId für UpgradeService. |
|CoordinatorType | string, Standardwert „WUTest“|Nicht zulässig|CoordinatorType für UpgradeService. |
|MinReplicaSetSize | Ganze Zahl, Standardwert 2 |Nicht zulässig| MinReplicaSetSize für UpgradeService. |
|OnlyBaseUpgrade | Boolesch, Standardwert „false“ |Dynamisch|OnlyBaseUpgrade für UpgradeService. |
|PlacementConstraints |string, Standardwert "" |Nicht zulässig|PlacementConstraints für den Aktualisierungsdienst. |
|TargetReplicaSetSize | Ganze Zahl, Standardwert 3 |Nicht zulässig| TargetReplicaSetSize für UpgradeService. |
|TestCabFolder | string, Standardwert "" |statischen| TestCabFolder für UpgradeService. |
|X509FindType | string, Standardwert ""|Dynamisch| X509FindType für UpgradeService. |
|X509FindValue | string, Standardwert "" |Dynamisch| X509FindValue für UpgradeService. |
|X509SecondaryFindValue | string, Standardwert "" |Dynamisch| X509SecondaryFindValue für UpgradeService. |
|X509StoreLocation | string, Standardwert "" |Dynamisch| X509StoreLocation für UpgradeService. |
|X509StoreName | string, Standardwert „My“|Dynamisch|X509StoreName für UpgradeService. |

## <a name="next-steps"></a>Nächste Schritte
Lesen Sie die folgenden Artikel, um weitere Informationen zur Clusterverwaltung zu enthalten:

[Hinzufügen, Rollover und Entfernen von Zertifikaten in einem Azure-Cluster ](service-fabric-cluster-security-update-certs-azure.md) 

