---
title: Includedatei
description: Includedatei
services: virtual-machines-linux
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 04/16/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 0363b719e2587226282257e19e58e3c4e55c2219
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47454593"
---
## <a name="overview-of-ssh-and-keys"></a>Übersicht über SSH und Schlüssel

SSH ist ein Protokoll für verschlüsselte Verbindungen, das die sichere Anmeldung über ungesicherte Verbindungen ermöglicht. SSH ist das Standardverbindungsprotokoll für in Azure gehostete virtuelle Linux-Computer. SSH stellt zwar bereits eine verschlüsselte Verbindung bereit, bei Verwendung von Kennwörtern für SSH-Verbindungen ist der virtuelle Computer jedoch weiterhin anfällig für Brute-Force-Angriffe bzw. der Gefahr ausgesetzt, dass das Kennwort erraten wird. Die sicherere und bevorzugte Methode für die Verbindungsherstellung mit einem virtuellen Computer über SSH ist die Verwendung eines Paars aus einem öffentlichen und einem privaten Schlüssel, auch als *SSH-Schlüssel* bezeichnet. 

* Der *öffentliche Schlüssel* wird auf Ihrem virtuellen Linux-Computer oder in einem anderen Dienst platziert, den Sie für die Verschlüsselung mit öffentlichem Schlüssel verwenden möchten.

* Der *private Schlüssel* wird Ihrem virtuellen Linux-Computer bei der Herstellung einer SSH-Verbindung vorgelegt, um Ihre Identität zu bestätigen. Bewahren Sie diesen privaten Schlüssel sicher auf. Geben Sie ihn nicht weiter.

Abhängig von den Sicherheitsrichtlinien in Ihrer Organisation können Sie ein einzelnes Paar aus einem öffentlichen und einem privaten Schlüssel für den Zugriff auf mehrere Azure-VMs und -Dienste verwenden. Sie benötigen nicht für jeden virtuellen Computer oder Dienst, auf den Sie zugreifen möchten, ein separates Schlüsselpaar. 

Sie können Ihren öffentlichen Schlüssel an beliebige Personen weitergeben, während Ihr privater Schlüssel ausschließlich für Ihren Besitz bestimmt ist (bzw. Ihre lokale Sicherheitsinfrastruktur).