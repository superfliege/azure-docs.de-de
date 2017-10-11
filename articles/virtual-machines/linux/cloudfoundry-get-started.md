---
title: Erste Schritte mit Cloud-Foundry in Microsoft Azure | Microsoft Docs
description: "Betriebssysteme oder Pivot Cloud Foundry in Microsoft Azure ausführen."
services: virtual-machines-linux
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 2a15ffbf-9f86-41e4-b75b-eb44c1a2a7ab
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 01/19/2017
ms.author: seanmck
ms.openlocfilehash: 94fbde7707ea9a91076780fdefc3f5a827e0e7b2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="cloud-foundry-on-azure"></a>Cloud-Foundry in Azure

Foundry Cloud ist ein Open Source-Platform-as-a-Service (PaaS) für die Erstellung, Bereitstellung und Betrieb von 12-Faktor-Anwendungen, die in verschiedenen Sprachen und Frameworks entwickelt. Dieses Dokument beschreibt die Optionen zum Ausführen von Cloud-Foundry auf Azure und wie Ihnen den Einstieg haben.

## <a name="cloud-foundry-offerings"></a>Foundry-Cloudlösungen

Es gibt zwei Arten von Verfügbarkeit zur Ausführung auf Azure Cloud-Foundry: Open Source-Cloud Foundry (OSS CF) und Pivot Cloud Foundry (PCF). OSS-CF ist ein vollständig [Open Source-](https://github.com/cloudfoundry) Version des Cloud-Foundry verwaltet wird, von der Cloud Foundry-Foundation. Pivot Cloud Foundry ist ein Enterprise-Verteilung von Cloud-Foundry von Pivot Software Inc. Betrachten wir einige der Unterschiede zwischen den beiden angeboten.

### <a name="open-source-cloud-foundry"></a>Open-Source-Cloud Foundry

Sie können Betriebssysteme Cloud Foundry in Azure bereitstellen, indem zuerst bereitstellen BOSH Director und anschließenden Bereitstellung Cloud Foundry, mit der [Anweisungen auf GitHub](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/blob/master/docs/guidance.md). Weitere Informationen zum Verwenden von OSS-CF finden Sie unter der [Dokumentation](https://docs.cloudfoundry.org/) von der Cloud Foundry-Foundation bereitgestellt.

Microsoft bietet Best-Effort-Unterstützung für OSS-CF über den folgenden communitykanälen:

- #<a name="bosh-azure-cpi-channel-on-cloud-foundry-slackhttpsslackcloudfoundryorg"></a>Bosh-Azure-KLI Kanal [Cloud Foundry Slack](https://slack.cloudfoundry.org/)
- [CF Bosh Adressenliste](https://lists.cloudfoundry.org/pipermail/cf-bosh)
- GitHub gibt, für die [KLI](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/issues) und [Service Broker](https://github.com/Azure/meta-azure-service-broker/issues)

>[!NOTE]
> Das Maß an Unterstützung für Azure-Ressourcen, z. B. die virtuellen Computer, in denen Cloud-Foundry ausführen basiert in Ihrem Azure-Support-Vertrag. Best-Effort-Communityunterstützung gilt nur für die Cloud Foundry-spezifische Komponenten.

### <a name="pivotal-cloud-foundry"></a>Pivot Cloud Foundry

Pivot Cloud Foundry enthält die gleichen Kernplattform wie bei der OSS-Verteilung, zusammen mit einem Satz von proprietären Verwaltungstools und Enterprise unterstützt. Um PCF in Azure auszuführen, müssen Sie eine Lizenz aus Pivotal erwerben. Das Angebot PCF aus dem Azure Marketplace umfasst eine 90-Tage-Testversion-Lizenz.

Die Tools umfassen [Pivot Operations Manager](http://docs.pivotal.io/pivotalcf/customizing/), eine Webanwendung, die die Bereitstellung und Verwaltung von einer Cloud Foundry-Foundation, vereinfacht und [Pivot Apps Manager](https://docs.pivotal.io/pivotalcf/console/), eine Webanwendung für die Verwaltung von Benutzern und Anwendungen.

Zusätzlich zu den Supportkanäle für OSS-CF oben aufgelistet sind sind Sie an den Support wenden Pivotal eine Lizenz PCF berechtigt. Microsoft und Pivotal haben auch Unterstützung Workflows aktiviert, mit denen Sie weitere Unterstützung erhalten Sie beide Parteien und haben Ihre Anfrage ordnungsgemäß weitergeleitet werden, je nachdem, wo das Problem liegt.

## <a name="azure-service-broker"></a>Azure Service Broker

Cloud-Foundry vertraut zu machen, die ["zwölf-Faktor-app"](https://12factor.net/) Methodik, die eine klare Trennung von zustandslose Anwendungsprozesse und zustandsbehafteten Diensten sichern fördert. [Service Broker](https://docs.cloudfoundry.org/services/api.html) bieten eine einheitliche Methode zum Bereitstellen und unterstützende Dienste auf Anwendungen zu binden. Die [Azure Service Broker](https://github.com/Azure/meta-azure-service-broker) enthält einige der wichtigen Dienste, die über diesen Kanal, einschließlich der Azure-Speicher und Azure SQL Azure.

Bei Verwendung von Pivot Cloud Foundry ist auch der Service Broker [verfügbar als Kachel](https://docs.pivotal.io/azure-sb/installing.html) über die entscheidende Netzwerk.

## <a name="related-resources"></a>Verwandte Ressourcen

### <a name="visual-studio-team-services-plugin"></a>Visual Studio Team Services-Plug-in

Cloud-Foundry eignet sich gut für agile Softwareentwicklung, einschließlich der Verwendung von continuous Integration (CI) und der kontinuierlichen Bereitstellung (CD). Wenn Sie Visual Studio Team Services (VSTS) verwenden, um Ihre Projekte zu verwalten und möchte Einrichten einer CI/CD pipeline Cloud Foundry abzielt, können Sie mithilfe der [VSTS Cloud Foundry erstellen Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vsts.cloud-foundry-build-extension). Das Plug-in vereinfacht das Konfigurieren und Automatisieren von Bereitstellungen zum Cloud-Foundry, ob die Ausführung in Azure oder eine andere Umgebung.

## <a name="next-steps"></a>Nächste Schritte

- [Bereitstellen von Pivot Cloud Foundry aus dem Azure Marketplace](https://azure.microsoft.com/en-us/marketplace/partners/pivotal/pivotal-cloud-foundryazure-pcf/)
- [Bereitstellen einer app für die Cloud Foundry in Azure](./cloudfoundry-deploy-your-first-app.md)