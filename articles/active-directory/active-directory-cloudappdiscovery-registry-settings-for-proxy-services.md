---
title: "Cloud App Discovery-Registrierungseinstellungen für Proxydienste | Microsoft Docs"
description: "In diesem Thema werden die Schritte beschrieben, die Sie ausführen müssen, um den erforderlichen Port auf den Computern mit dem Cloud App Discovery-Agent festzulegen."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 8d78e925-e331-40ba-904a-e4ef14260cac
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: markvi
ms.reviewer: nigu
ms.translationtype: HT
ms.sourcegitcommit: 7dceb7bb38b1dac778151e197db3b5be49dd568a
ms.openlocfilehash: b8584809b76d6be12a6f489f0bb819081d1803d2
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
# <a name="cloud-app-discovery-registry-settings-for-proxy-services"></a>Cloud App Discovery-Registrierungseinstellungen für Proxydienste
In diesem Thema wird beschrieben, wie Sie den erforderlichen Port auf den Computern mit dem Cloud App Discovery-Agent festlegen. Standardmäßig verwendet der Cloud App Discovery-Agent nur die Ports 80 oder 443. Wenn Sie planen, Cloud App Discovery in einer Umgebung mit einem Proxyserver zu installieren, der einen benutzerdefinierten Port (weder 80 noch 443) verwendet, müssen Sie die Agents zur Verwendung dieses Ports konfigurieren. Die Konfiguration basiert auf einem Registrierungsschlüssel.

> [!TIP] 
> Es wurden Verbesserungen an Cloud App Discovery in Azure Active Directory (Azure AD) vorgenommen, das jetzt ohne Agent ausgeführt wird. Diese Verbesserungen werden durch die [Integration in Microsoft Cloud App Security](https://portal.cloudappsecurity.com) verstärkt.

## <a name="modify-the-port-used-by-the-computer-running-the-cloud-app-discovery-agent"></a>Ändern des Ports, der vom Computer mit dem Cloud App Discovery-Agent verwendet wird

1. Starten Sie den Registrierungs-Editor.
  ![Run](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy01.png)
2. Wechseln Sie zu dem folgenden Registrierungsschlüssel: **HKLM_LOCAL_MACHINE\Software\Microsoft\Cloud App Discovery\Endpoint**, oder erstellen Sie ihn.
3. Erstellen Sie einen neuen Wert vom Typ **mehrteilige Zeichenfolge** namens **Ports**. 
  ![Neu](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy02.png)
4. Doppelklicken Sie auf den Wert **Ports**, um das Dialogfeld **Mehrteilige Zeichenfolge bearbeiten** zu öffnen.
5. Geben Sie bei **Wertdaten** die folgenden Werte ein, und fügen Sie alle benutzerdefinierten Ports hinzu, die von Ihrer Organisation verwendet werden: <br><br>
   **80** <br>
   **8080** <br>
   **8118** <br>
   **8888** <br>
   **81** <br>
   **12080** <br>
   **6999** <br>
   **30606** <br>
   **31595** <br>
   **4080** <br>
   **443** <br>
   **1110** <br><br>
   ![Mehrteilige Zeichenfolge bearbeiten](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy03.png)
6. Klicken Sie auf **OK**, um das Dialogfeld **Mehrteilige Zeichenfolge bearbeiten** zu schließen.

## <a name="next-steps"></a>Nächste Schritte

* [Wie ermittle ich nicht genehmigte Cloud-Apps, die in meiner Organisation verwendet werden?](active-directory-cloudappdiscovery-whatis.md) 


