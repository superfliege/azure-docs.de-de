---
title: Sichern von Webdiensten mit SSL
titleSuffix: Azure Machine Learning service
description: Erfahren Sie, wie Sie einen mit dem Azure Machine Learning-Dienst bereitgestellten Webdienst schützen. Mithilfe von SSL (Secure Sockets Layer) und schlüsselbasierter Authentifizierung können Sie den Zugriff auf Webdienste einschränken und die von Clients übermittelten Daten schützen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: aashishb
author: aashishb
ms.date: 02/05/2019
ms.custom: seodec18
ms.openlocfilehash: 2f21c54100a46d2f6ba28d2063bea91b84ea06d4
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/06/2019
ms.locfileid: "55769320"
---
# <a name="use-ssl-to-secure-web-services-with-azure-machine-learning-service"></a>Verwenden von SSL zum Schützen von Webdiensten mit Azure Machine Learning Service

In diesem Artikel erfahren Sie, wie Sie einen mit dem Azure Machine Learning-Dienst bereitgestellten Webdienst schützen. Mithilfe von SSL (Secure Sockets Layer) und schlüsselbasierter Authentifizierung können Sie den Zugriff auf Webdienste einschränken und die von Clients übermittelten Daten schützen.

> [!Warning]
> Wenn Sie SSL nicht aktivieren, kann jeder Benutzer im Internet Aufrufe an den Webdienst ausführen.

SSL verschlüsselt die zwischen dem Client und dem Webdienst gesendeten Daten. SSL wird auch vom Client zum Überprüfen der Identität des Servers verwendet. Authentifizierung ist nur für Dienste aktiviert, die ein SSL-Zertifikat und einen Schlüssel angegeben haben.  Wenn Sie SSL aktivieren, ist ein Authentifizierungsschlüssel für den Zugriff auf den Webdienst erforderlich.

Ob Sie einen Webdienst mit aktiviertem SSL bereitstellen oder SSL für einen vorhandenen bereitgestellten Webdienst aktivieren, es sind die gleichen Schritte auszuführen:

1. Rufen Sie einen Domänennamen ab.

2. Rufen Sie ein SSL-Zertifikat ab.

3. Stellen Sie den Webdienst mit aktivierter SSL-Einstellung bereit, oder aktualisieren Sie ihn.

4. Aktualisieren Sie Ihren DNS, so dass er auf den Dienst verweist.

Es bestehen bei den verschiedenen [Bereitstellungszielen](how-to-deploy-and-where.md) geringfügige Unterschiede beim Schützen von Webdiensten.

## <a name="get-a-domain-name"></a>Abrufen eines Domänennamens

Wenn Sie nicht bereits über einen Domänennamen verfügen, können Sie einen solchen von einer __Domänennamen-Registrierungsstelle__ erwerben. Der Vorgang unterscheidet sich (ebenso wie die Kosten) abhängig von der Domänenregistrierungsstelle. Die Registrierungsstelle stellt Ihnen außerdem Tools zum Verwalten des Domänennamens zur Verfügung. Diese Tools werden zum Zuordnen eines vollqualifizierten Domänennamens (z.B. www.contoso.com) zur IP-Adresse verwendet, unter der Ihr Dienst gehostet ist.

## <a name="get-an-ssl-certificate"></a>Beziehen eines SSL-Zertifikats

Es gibt viele Möglichkeiten, ein SSL-Zertifikat zu erhalten. Die verbreitetste Methode ist der Erwerb eines Zertifikats von einer __Zertifizierungsstelle__ (CA). Unabhängig davon, wo Sie das Zertifikat abrufen, benötigen Sie die folgenden Dateien:

* Ein __Zertifikat__. Das Zertifikat muss die vollständige Zertifikatkette enthalten und PEM-codiert sein.
* Einen __Schlüssel__. Der Schlüssel muss PEM-codiert sein.

Wenn Sie ein Zertifikat anfordern, müssen Sie den vollqualifizierten Domänennamen (FQDN) der Adresse angeben, die Sie für den Webdienst verwenden möchten. Beispielsweise www.contoso.com. Die in das Zertifikat gestempelte Adresse und die von den Clients verwendete Adresse werden verglichen, wenn die Identität des Webdiensts überprüft wird. Wenn die Adressen nicht übereinstimmen, erhalten die Clients einen Fehler.

> [!TIP]
> Wenn die Zertifizierungsstelle das Zertifikat und den Schlüssel nicht als PEM-codierte Dateien bereitstellen kann, können Sie ein Hilfsprogramm wie [OpenSSL](https://www.openssl.org/) zum Ändern des Formats verwenden.

> [!WARNING]
> Selbstsignierte Zertifikate sollten nur für die Entwicklung verwendet werden. Sie sollten nicht in der Produktion eingesetzt werden. Selbstsignierte Zertifikate können Probleme in Ihren Clientanwendungen verursachen. Weitere Informationen finden Sie in der Dokumentation für die in der Clientanwendung verwendeten Netzwerkbibliotheken.

## <a name="enable-ssl-and-deploy"></a>Aktivieren von SSL und Bereitstellen

Legen Sie zum Bereitstellen (oder erneuten Bereitstellen) des Diensts mit aktiviertem SSL den `ssl_enabled`-Parameter auf `True` fest, wenn zutreffend. Legen Sie den `ssl_certificate`-Parameter auf den Wert der __Zertifikat__datei und den `ssl_key` auf den Wert der __Schlüssel__datei fest.

+ **Bereitstellen auf Azure Kubernetes Service (AKS)**

  Geben Sie beim Bereitstellen des AKS-Clusters Werte für die auf SSL bezogenen Parameter ein, wie in diesem Codeausschnitt gezeigt:

    ```python
    from azureml.core.compute import AksCompute

    provisioning_config = AksCompute.provisioning_configuration(ssl_cert_pem_file="cert.pem", ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
    ```

+ **Bereitstellen auf Azure Container Instances (ACI)**

  Geben Sie beim Bereitstellen auf ACI Werte für die auf SSL bezogenen Parameter ein, wie in diesem Codeausschnitt gezeigt:

    ```python
    from azureml.core.webservice import AciWebservice

    aci_config = AciWebservice.deploy_configuration(ssl_enabled=True, ssl_cert_pem_file="cert.pem", ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
    ```

## <a name="update-your-dns"></a>Aktualisieren Ihres DNS

Als Nächstes müssen Sie Ihren DNS aktualisieren, so dass er auf den Dienst verweist.

+ **Für ACI**:

  Verwenden Sie die von Ihrer Domänennamen-Registrierungsstelle bereitgestellten Tools, um den DNS-Eintrag für Ihren Domänennamen zu aktualisieren. Der Eintrag muss auf die IP-Adresse des Diensts verweisen.

  Abhängig von der Registrierungsstelle und dem TTL-Wert (Time-to-Live), der für den Domänennamen konfiguriert ist, kann es einige Minuten bis mehrere Stunden dauern, bevor Clients den Domänennamen auflösen können.

+ **Für AKS**:

  Aktualisieren Sie den DNS auf der Registerkarte „Konfiguration“ unter „öffentliche IP-Adresse“ des AKS-Clusters, wie in der Abbildung dargestellt. Sie finden die öffentliche IP-Adresse als einen der Ressourcentypen, die unter der Ressourcengruppe erstellt wurden, die die AKS-Agent-Knoten und weitere Netzwerkressourcen enthält.

  ![Azure Machine Learning Service: Sichern von Webdiensten mit SSL](./media/how-to-secure-web-service/aks-public-ip-address.png)

+ **Für FPGA**:

Die Verwendung von SSL mit Diensten, die für FPGA bereitgestellt werden, wird derzeit nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel werden folgende Themen erläutert:
+ [Nutzen eines als Webdienst bereitgestellten Machine Learning-Modells](how-to-consume-web-service.md)
+ [Sicheres Ausführen von Experimenten und Ziehen von Rückschlüssen innerhalb eines virtuellen Azure-Netzwerks](how-to-enable-virtual-network.md)
