---
title: 'Azure AD v2 – Windows-Desktop: Erste Schritte – Konfiguration | Microsoft-Dokumentation'
description: Informationen, wie Windows Desktop .NET (XAML)-Anwendungen eine API aufrufen können, die Zugriffstoken vom Azure Active Directory v2-Endpunkt anfordert.
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: dc9b5cbc4904c6b2238e56742381feb4d2811437
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56195820"
---
# <a name="add-the-applications-registration-information-to-your-app"></a>Hinzufügen der Registrierungsinformationen der Anwendung zu Ihrer App
In diesem Schritt müssen Sie die Anwendungs-ID Ihrem Projekt hinzufügen.

1.  Öffnen Sie `App.xaml.cs`, und ersetzen Sie die Zeile mit der `ClientId` durch:

```csharp
private static string ClientId = "[Enter the application Id here]";
```

### <a name="what-is-next"></a>Wie geht es weiter?

[!INCLUDE [Test and Validate](../../../../includes/active-directory-develop-guidedsetup-windesktop-test.md)]
