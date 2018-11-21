---
title: Includedatei
description: Includedatei
services: virtual-machines-linux
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 11/08/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: cff3d7bfb89d5b03f986da32edc148efcfb7e7bd
ms.sourcegitcommit: 96527c150e33a1d630836e72561a5f7d529521b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2018
ms.locfileid: "51506321"
---
## <a name="overview-of-ssh-and-keys"></a>Übersicht über SSH und Schlüssel

SSH ist ein Protokoll für verschlüsselte Verbindungen, das die sichere Anmeldung über ungesicherte Verbindungen ermöglicht. SSH ist das Standardverbindungsprotokoll für in Azure gehostete virtuelle Linux-Computer. SSH stellt zwar bereits eine verschlüsselte Verbindung bereit, bei Verwendung von Kennwörtern für SSH-Verbindungen ist der virtuelle Computer jedoch weiterhin anfällig für Brute-Force-Angriffe bzw. der Gefahr ausgesetzt, dass das Kennwort erraten wird. Die sicherere und bevorzugte Methode für die Verbindungsherstellung mit einem virtuellen Computer über SSH ist die Verwendung eines Paars aus einem öffentlichen und einem privaten Schlüssel, auch als *SSH-Schlüssel* bezeichnet. 

* Der *öffentliche Schlüssel* wird auf Ihrem virtuellen Linux-Computer oder in einem anderen Dienst platziert, den Sie für die Verschlüsselung mit öffentlichem Schlüssel verwenden möchten.

* Mit dem *privaten Schlüssel* Ihres lokalen Systems überprüft ein SSH-Client Ihre Identität, wenn Sie eine Verbindung mit Ihrem virtuellen Linux-Computer herstellen. Bewahren Sie diesen privaten Schlüssel sicher auf. Geben Sie ihn nicht weiter.

Abhängig von den Sicherheitsrichtlinien in Ihrer Organisation können Sie ein einzelnes Paar aus einem öffentlichen und einem privaten Schlüssel für den Zugriff auf mehrere Azure-VMs und -Dienste verwenden. Sie benötigen nicht für jeden virtuellen Computer oder Dienst, auf den Sie zugreifen möchten, ein separates Schlüsselpaar. 

Sie können Ihren öffentlichen Schlüssel an beliebige Personen weitergeben, während Ihr privater Schlüssel ausschließlich für Ihren Besitz bestimmt ist (bzw. Ihre lokale Sicherheitsinfrastruktur).