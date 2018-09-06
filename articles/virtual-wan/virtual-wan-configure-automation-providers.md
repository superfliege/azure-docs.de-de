---
title: Konfigurieren von Azure Virtual WAN-Automatisierung – für Virtual WAN-Partner | Microsoft-Dokumentation
description: Dieser Artikel unterstützt Partner für softwaredefinierte Konnektivitätslösungen bei der Einrichtung von Azure Virtual WAN-Automatisierung.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 08/23/2018
ms.author: cherylmc
Customer intent: As a Virtual WAN software-defined connectivity provider, I want to set up a provisioning environment.
ms.openlocfilehash: bac728f286c90550107b27da76a070623577ed82
ms.sourcegitcommit: ebb460ed4f1331feb56052ea84509c2d5e9bd65c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/24/2018
ms.locfileid: "42918899"
---
# <a name="configure-virtual-wan-automation---for-virtual-wan-partners-preview"></a>Konfigurieren von Virtual WAN-Automatisierung – für Virtual WAN-Partner (Vorschauversion)

In diesem Artikel erfahren Sie, wie Sie die Automatisierungsumgebung einrichten, um ein Zweigstellengerät (ein lokales VPN-Gerät oder SDWAN eines Kunden) für Azure Virtual WAN zu verbinden und zu konfigurieren. Wenn Sie Zweigstellengeräte anbieten, die für VPN-Konnektivität über IPsec/IKEv2 geeignet sind, ist dieser Artikel für Sie interessant.

Softwaredefinierte Konnektivitätslösungen verwalten ihre Zweigstellengeräte normalerweise mithilfe eines Controllers oder über ein Center für die Gerätebereitstellung. Für den Controller können Azure-APIs verwendet werden, um die Konnektivität mit Azure Virtual WAN zu automatisieren. Für diese Art von Verbindung wird ein SDWAN oder ein lokales VPN-Gerät benötigt, dem eine extern zugängliche, öffentliche IP-Adresse zugewiesen ist.

##  <a name="access"></a>Zugriffssteuerung

Kunden müssen entsprechende Zugriffssteuerungen für Virtual WAN in der Gerätebenutzeroberfläche einrichten können. Dafür wird ein Azure-Dienstprinzipal empfohlen. Der auf einem Dienstprinzipal basierende Zugriff bietet dem Gerätecontroller eine entsprechende Authentifizierung, um Zweigstelleninformationen hochzuladen. Weitere Informationen finden Sie unter [Erstellen eines Dienstprinzipals](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application).

##  <a name="site"></a>Hochladen von Zweigstelleninformationen

Gestalten Sie die Benutzererfahrung, um Zweigstelleninformationen (lokal) in Azure hochzuladen. Zur Erstellung der Siteinformationen in Virtual WAN können [REST-APIs](https://docs.microsoft.com/rest/api/virtualwan/vpnsites) für **VPNSite** verwendet werden. Sie können alle SDWAN/VPN-Zweigstellengeräte bereitstellen oder entsprechende Geräteanpassungen auswählen.

##  <a name="hub"></a>Hub und Dienste

Sobald das Zweigstellengerät zu Azure hochgeladen wurde, wählt der Kunde in der Regel die Hubregion und/oder die Dienste im Azure-Portal aus, wodurch eine Reihe von Vorgängen zur Erstellung des virtuellen Hubnetzwerks und des VPN-Endpunkts innerhalb des Hubs aufgerufen wird. Das VPN Gateway ist ein skalierbares Gateway, das je nach Bandbreiten- und Verbindungsbedarf entsprechend dimensioniert ist.

## <a name="device"></a>Gerätekonfiguration

In diesem Schritt würde ein Kunde, der keinen Anbieter verwendet, die Azure-Konfiguration manuell herunterladen und auf sein lokales SDWAN/VPN-Gerät anwenden. Als Anbieter sollten Sie diesen Schritt automatisieren. Der Controller kann die **GetVpnConfiguration**-REST-API zum Herunterladen der Azure-Konfiguration aufrufen, die typischerweise ähnlich der folgenden Datei aussieht.

**Konfigurationshinweise**

  * Wenn Azure-VNets an den virtuellen Hub angefügt sind, werden sie als ConnectedSubnets angezeigt.
  * VPN-Konnektivität verwendet eine routenbasierte Konfiguration und IKEv2.

### <a name="understanding-the-device-configuration-file"></a>Grundlegendes zur Gerätekonfigurationsdatei

Die Gerätekonfigurationsdatei enthält die Einstellungen, die beim Konfigurieren Ihrer lokalen VPN-Geräte verwendet werden. Beachten Sie beim Anzeigen dieser Datei die folgenden Informationen:

* **vpnSiteConfiguration**: In diesem Abschnitt sind die Gerätedetails für die Einrichtung einer Site angegeben, für die eine Verbindung mit dem virtuellen WAN hergestellt wird. Sie enthält den Namen und die öffentliche IP-Adresse des Zweigstellengeräts.
* **vpnSiteConnections**: Dieser Abschnitt enthält die folgenden Informationen:

    * **Adressraum** des virtuellen Hub-VNET.<br>Beispiel:
 
        ```
        "AddressSpace":"10.1.0.0/24"
        ```
    * **Adressraum** der VNETs, die mit dem Hub verbunden sind.<br>Beispiel:

         ```
        "ConnectedSubnets":["10.2.0.0/16","10.30.0.0/16"]
         ```
    * **IP-Adressen** des vpngateway für den virtuellen Hub. Da für das vpngateway jede Verbindung aus zwei Tunneln mit Aktiv-Aktiv-Konfiguration besteht, sind in dieser Datei beide IP-Adressen angegeben. In diesem Beispiel werden für jede Site „Instance0“ und „Instance1“ angezeigt.<br>Beispiel:

        ``` 
        "Instance0":"104.45.18.186"
        "Instance1":"104.45.13.195"
        ```
    * **Details zur Konfiguration der vpngateway-Verbindung**, z.B. BGP, vorinstallierter Schlüssel usw. Der vorinstallierte Schlüssel (Pre-Shared Key, PSK) wird automatisch für Sie erstellt. Sie können die Verbindung für einen benutzerdefinierten PSK auf der Seite „Übersicht“ jederzeit bearbeiten.
  
### <a name="example-device-configuration-file"></a>Beispiel für Gerätekonfigurationsdatei

  ```
  { 
      "configurationVersion":{ 
         "LastUpdatedTime":"2018-07-03T18:29:49.8405161Z",
         "Version":"r403583d-9c82-4cb8-8570-1cbbcd9983b5"
      },
      "vpnSiteConfiguration":{ 
         "Name":"testsite1",
         "IPAddress":"73.239.3.208"
      },
      "vpnSiteConnections":[ 
         { 
            "hubConfiguration":{ 
               "AddressSpace":"10.1.0.0/24",
               "Region":"West Europe",
               "ConnectedSubnets":[ 
                  "10.2.0.0/16",
                  "10.30.0.0/16"
               ]
            },
            "gatewayConfiguration":{ 
               "IpAddresses":{ 
                  "Instance0":"104.45.18.186",
                  "Instance1":"104.45.13.195"
               }
            },
            "connectionConfiguration":{ 
               "IsBgpEnabled":false,
               "PSK":"bkOWe5dPPqkx0DfFE3tyuP7y3oYqAEbI",
               "IPsecParameters":{ 
                  "SADataSizeInKilobytes":102400000,
                  "SALifeTimeInSeconds":3600
               }
            }
         }
      ]
   },
   { 
      "configurationVersion":{ 
         "LastUpdatedTime":"2018-07-03T18:29:49.8405161Z",
         "Version":"1f33f891-e1ab-42b8-8d8c-c024d337bcac"
      },
      "vpnSiteConfiguration":{ 
         "Name":" testsite2",
         "IPAddress":"66.193.205.122"
      },
      "vpnSiteConnections":[ 
         { 
            "hubConfiguration":{ 
               "AddressSpace":"10.1.0.0/24",
               "Region":"West Europe"
            },
            "gatewayConfiguration":{ 
               "IpAddresses":{ 
                  "Instance0":"104.45.18.187",
                  "Instance1":"104.45.13.195"
               }
            },
            "connectionConfiguration":{ 
               "IsBgpEnabled":false,
               "PSK":"XzODPyAYQqFs4ai9WzrJour0qLzeg7Qg",
               "IPsecParameters":{ 
                  "SADataSizeInKilobytes":102400000,
                  "SALifeTimeInSeconds":3600
               }
            }
         }
      ]
   },
   { 
      "configurationVersion":{ 
         "LastUpdatedTime":"2018-07-03T18:29:49.8405161Z",
         "Version":"cd1e4a23-96bd-43a9-93b5-b51c2a945c7"
      },
      "vpnSiteConfiguration":{ 
         "Name":" testsite3",
         "IPAddress":"182.71.123.228"
      },
      "vpnSiteConnections":[ 
         { 
            "hubConfiguration":{ 
               "AddressSpace":"10.1.0.0/24",
               "Region":"West Europe"
            },
            "gatewayConfiguration":{ 
               "IpAddresses":{ 
                  "Instance0":"104.45.18.187",
                  "Instance1":"104.45.13.195"
               }
            },
            "connectionConfiguration":{ 
               "IsBgpEnabled":false,
               "PSK":"YLkSdSYd4wjjEThR3aIxaXaqNdxUwSo9",
               "IPsecParameters":{ 
                  "SADataSizeInKilobytes":102400000,
                  "SALifeTimeInSeconds":3600
               }
            }
         }
      ]
   }
  ```

## <a name="default"></a>Standardrichtlinien

### <a name="initiator"></a>Initiator

Die folgenden Abschnitte listen die unterstützten Richtlinienkombinationen auf, wenn Azure der Initiator für den Tunnel ist.

**Phase-1**

* AES_256, SHA1, DH_GROUP_2
* AES_256, SHA_256, DH_GROUP_2
* AES_128, SHA1, DH_GROUP_2
* AES_128, SHA_256, DH_GROUP_2
* 3DES, SHA1, DH_GROUP_2
* 3DES, SHA_256, DH_GROUP_2

**Phase-2**

* GCM_AES_256, GCM_AES_256, PFS_NONE
* AES_256, SHA_1, PFS_NONE
* CBC_3DES, SHA_1, PFS_NONE
* AES_256, SHA_256, PFS_NONE
* AES_128, SHA_1, PFS_NONE
* CBC_3DES, SHA_256, PFS_NONE


### <a name="responder"></a>Antwortdienst

Die folgenden Abschnitte listen die unterstützten Richtlinienkombinationen auf, wenn Azure der Antwortdienst für den Tunnel ist.

**Phase-1**

* AES_256, SHA1, DH_GROUP_2
* AES_256, SHA_256, DH_GROUP_2
* AES_128, SHA1, DH_GROUP_2
* AES_128, SHA_256, DH_GROUP_2
* 3DES, SHA1, DH_GROUP_2
* 3DES, SHA_256, DH_GROUP_2

**Phase-2**

* GCM_AES_256, GCM_AES_256, PFS_NONE
* AES_256, SHA_1, PFS_NONE
* CBC_3DES, SHA_1, PFS_NONE
* AES_256, SHA_256, PFS_NONE
* AES_128, SHA_1, PFS_NONE
* CBC_3DES, SHA_256, PFS_NONE
* CBC_DES, SHA_1, PFS_NONE 
* AES_256, SHA_1, PFS_1
* AES_256, SHA_1, PFS_2
* AES_256, SHA_1, PFS_14
* AES_128, SHA_1, PFS_1
* AES_128, SHA_1, PFS_2
* AES_128, SHA_1, PFS_14
* CBC_3DES, SHA_1, PFS_1
* CBC_3DES, SHA_1, PFS_2
* CBC_3DES, SHA_256, PFS_2
* AES_256, SHA_256, PFS_1
* AES_256, SHA_256, PFS_2
* AES_256, SHA_256, PFS_14
* AES_256, SHA_1, PFS_24
* AES_256, SHA_256, PFS_24
* AES_128, SHA_256, PFS_NONE
* AES_128, SHA_256, PFS_1
* AES_128, SHA_256, PFS_2
* AES_128, SHA_256, PFS_14
* CBC_3DES, SHA_1, PFS_14

### <a name="does-everything-need-to-match-between-the-virtual-hub-vpngateway-policy-and-my-on-premises-sdwanvpn-device-or-sd-wan-configuration"></a>Müssen alle Angaben zwischen der Richtlinie des vpngateway für den virtuellen Hub und meiner lokalen SDWAN/VPN- oder SD-WAN-Gerätekonfiguration übereinstimmen?

Ihre lokale SDWAN/VPN- oder SD-WAN-Gerätekonfiguration muss folgenden Algorithmus- und Parameterangaben für die Azure-IPsec-/IKE-Richtlinie entsprechen oder selbige enthalten. Bei der SA-Gültigkeitsdauer handelt es sich lediglich um eine lokale Angabe. Diese muss nicht übereinstimmen.

* IKE-Verschlüsselungsalgorithmus
* IKE-Integritätsalgorithmus
* DH-Gruppe
* IPsec-Verschlüsselungsalgorithmus
* IPsec-Integritätsalgorithmus
* PFS-Gruppe

## <a name="feedback"></a>Feedback zur Vorschauversion

Wir schätzen Ihr Feedback. Senden Sie eine E-Mail an <azurevirtualwan@microsoft.com>, um Probleme zu melden oder Feedback zu Virtual WAN zu geben (positiv oder negativ). Fügen Sie den Namen Ihres Unternehmens in eckigen Klammern in die Betreffzeile ein. Wenn Sie ein Problem melden, geben Sie auch Ihre Abonnement-ID an.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Virtual WAN, finden Sie unter [Über Virtual WAN](virtual-wan-about.md) und [FAQS zu Azure Virtual WAN](virtual-wan-faq.md).
