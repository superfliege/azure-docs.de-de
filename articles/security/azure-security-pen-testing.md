---
title: Penetrationstests | Microsoft Docs
description: Der Artikel bietet einen Überblick über den Penetrationstestprozess und dazu, wie Sie Penetrationstests an Ihren in der Azure-Infrastruktur ausgeführten Apps durchführen können.
services: security
documentationcenter: na
author: TerryLanfear
manager: mbaldwin
editor: TomSh
ms.assetid: 695d918c-a9ac-4eba-8692-af4526734ccc
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/13/2018
ms.author: barclayn
ms.openlocfilehash: 3f7d44a2a34f6b52c2229cf6491e99c65d5bb525
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2019
ms.locfileid: "54052988"
---
# <a name="pen-testing"></a>Penetrationstests
Einer der Vorteile beim Einsatz von Azure für Testen und Bereitstellung von Anwendungen besteht darin, dass Sie schnell Umgebungen erstellen können.  Sie müssen sich keine Gedanken über Anforderung, Erwerb und Aufbau eigener lokaler Hardware machen.

Das ist hervorragend – Sie müssen jedoch noch sicherstellen, dass Sie mit angemessener Sorgfalt für Sicherheit sorgen. Sie müssen u.a. einen Penetrationstest der Anwendungen durchführen, die Sie in Azure bereitstellen.

Möglicherweise wissen Sie bereits, dass Microsoft einen [Penetrationstests der Azure-Umgebung](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)durchführt. Dies bringt Verbesserungen an Azure voran.

Wir führen keinen Penetrationstest Ihrer Anwendung durch, aber wir verstehen, dass Sie Penetrationstests Ihrer eigenen Anwendungen durchführen möchten und müssen. Das ist eine gute Sache, denn wenn Sie die Sicherheit Ihrer Anwendungen verbessern, machen Sie das gesamte Azure-Ökosystem sicherer.

Vorgehensweise

Seit dem 15. Juni 2017 benötigt Microsoft keine Vorabgenehmigung mehr, um Penetrationstests für Azure-Ressourcen durchzuführen. Kunden, die die bevorstehenden Penetrationstests gegen Microsoft Azure formell dokumentieren möchten, werden gebeten, das Formular zur [Benachrichtigung bei Azure Service-Penetrationstests](https://portal.msrc.microsoft.com/en-us/engage/pentest) auszufüllen. Dieser Prozess bezieht sich nur auf Microsoft Azure und ist nicht auf andere Microsoft Cloud-Dienste anwendbar.

>[!IMPORTANT]
>Obwohl die Benachrichtigung von Microsoft über Penetrationstestaktivitäten nicht mehr erforderlich ist, müssen Kunden weiterhin die [Microsoft Cloud Unified Penetration Testing Rules of Engagement](https://technet.microsoft.com/mt784683) (Einheitliche Einsatzregeln für Penetrationstests für die Microsoft Cloud) einhalten.

Folgende Standardtests können Sie durchführen:

* Tests an Ihren Endpunkten, um die [wichtigsten 10 OWASP-Sicherheitsrisiken (Open Web Application Security Project)](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)
* [Fuzzing](https://cloudblogs.microsoft.com/microsoftsecure/2007/09/20/fuzz-testing-at-microsoft-and-the-triage-process/) Ihrer Endpunkte
* [Portüberwachung](https://en.wikipedia.org/wiki/Port_scanner) Ihrer Endpunkte

Ein Testtyp, den Sie nicht ausführen können, ist jegliche Form eines [Denial-of-Service-Angriffs (DoS)](https://en.wikipedia.org/wiki/Denial-of-service_attack). Dies schließt einen selbst initiierten DoS-Angriff bzw. das Durchführen zugehöriger Tests ein, die einen DoS-Angriff bestimmen, demonstrieren oder simulieren könnten.

## <a name="next-steps"></a>Nächste Schritte

- Sind Sie bereit für Penetrationstests Ihrer in Microsoft Azure gehosteten Anwendungen? Wenn dies der Fall ist, navigieren Sie zu [Penetration Testing Rules of Engagement](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=2) (Einsatzregeln für Penetrationstests), und füllen Sie das Testbenachrichtigungsformular aus.
