---
title: Verwalten von Protokollen und Verschlüsselungen in Azure API Management | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Protokolle (TLS, SSL) und Verschlüsselungen (DES) in Azure API Management verwalten.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/15/2018
ms.author: apimpm
ms.openlocfilehash: 454ba5c42694581bfa8fb1ec69ce97ac906b53d4
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47094574"
---
# <a name="manage-protocols-and-ciphers-in-azure-api-management"></a>Verwalten von Protokollen und Verschlüsselungen in Azure API Management

Azure API Management unterstützt mehrere Versionen des TLS-Protokolls für die Client- und Back-End-Seite sowie die 3DES-Verschlüsselung.

Dieser Leitfaden zeigt Ihnen, wie Sie Protokoll- und Verschlüsselungskonfigurationen für eine Azure API Management-Instanz verwalten können.

![Verwalten von Protokollen und Verschlüsselungen in API Management](./media/api-management-howto-manage-protocols-ciphers/api-management-protocols-ciphers.png)

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie den Schritten in diesem Artikel folgen können, benötigen Sie folgende Komponenten:

* Eine API Management-Instanz

## <a name="how-to-manage-tls-protocols-and-3des-cipher"></a>Verwalten von TLS-Protokollen und 3DES-Verschlüsselungen

1. Navigieren Sie im Azure-Portal zu Ihrer **API Management-Instanz**.
2. Wählen Sie im Menü die Option **SSL** aus.  
    ![Verwalten von Protokollen und Verschlüsselungen in API Management – Menü](./media/api-management-howto-manage-protocols-ciphers/api-management-menu.png)
3. Aktivieren bzw. deaktivieren Sie die gewünschten Protokolle oder Verschlüsselungen.
4. Klicken Sie auf **Speichern**. Die Änderungen werden innerhalb einer Stunde angewendet.  
    ![Verwalten von Protokollen und Verschlüsselungen in API Management – Speichern](./media/api-management-howto-manage-protocols-ciphers/api-management-protocols-ciphers-save.png)

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über [TLS (Transport Layer Security)](https://docs.microsoft.com/en-us/dotnet/framework/network-programming/tls).
* Hier finden Sie weitere [Videos](https://azure.microsoft.com/documentation/videos/index/?services=api-management) zu API Management.