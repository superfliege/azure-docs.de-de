---
title: Erneutes Generieren von HTTP-Anforderungs- und Antwortheadern mit Azure Application Gateway – Azure-Portal | Microsoft-Dokumentation
description: Informationen zum Verwenden des Azure-Portals zur Konfiguration eines Azure Application Gateway, um die HTTP-Header in den Anforderungen und Antworten erneut zu generieren, die das Gateway durchlaufen
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 04/10/2019
ms.author: absha
ms.custom: mvc
ms.openlocfilehash: 6afc07f98905469b06622e7829ec4a215b94845e
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2019
ms.locfileid: "59994604"
---
# <a name="rewrite-http-request-and-response-headers-with-azure-application-gateway---azure-portal"></a>Erneutes Generieren von HTTP-Anforderungs- und Antwortheadern mit Azure Application Gateway – Azure-Portal

In diesem Artikel erfahren Sie, wie Sie über das Azure-Portal eine [Application Gateway v2-SKU](<https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant>) konfigurieren, um die HTTP-Header in den Anforderungen und Antworten erneut zu generieren.

> [!IMPORTANT]
> Die Application Gateway-SKU mit automatischer Skalierung und Zonenredundanz ist derzeit als öffentliche Vorschau verfügbar. Diese Vorschau wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="before-you-begin"></a>Voraussetzungen

Sie benötigen eine Application Gateway v2-SKU, da die Funktion zum erneuten Generieren von Headern für die v1-SKU nicht unterstützt wird. Wenn Sie nicht über die v2-SKU verfügen, erstellen Sie eine [Application Gateway v2-SKU](https://docs.microsoft.com/azure/application-gateway/tutorial-autoscale-ps), bevor Sie beginnen.

## <a name="what-is-required-to-rewrite-a-header"></a>Voraussetzungen zum erneuten Generieren eines Headers

Folgendes ist erforderlich, um das erneute Generieren eines HTTP-Headers zu konfigurieren:

1. Erstellen Sie die neuen Objekte, die benötigt werden, um die HTTP-Header neu zu generieren:

   - **Aktion für das erneute Generieren**: Dient dazu, die Felder für Anforderung und Anforderungsheader, die Sie erneut generieren möchten, und den neuen Wert anzugeben, in den die ursprünglichen Header erneut generiert werden müssen. Sie können auswählen, ob eine oder mehrere Bedingungen für das erneute Generieren mit einer Aktion für das erneute Generieren verknüpft werden sollen.

   - **Bedingung für das erneute Generieren**: Dies ist eine optionale Konfiguration. Wenn eine Bedingung für das erneute Generieren hinzugefügt wird, wertet sie den Inhalt der HTTP(S)-Anforderungen und -Antworten aus. Die Entscheidung, die mit der Bedingung für das erneute Generieren verbundene Aktion zum erneuten Generieren auszuführen, hängt davon ab, ob die HTTP(S)-Anforderung oder -Antwort mit der Bedingung für das erneute Generieren übereinstimmt. 

     Wenn einer Aktion mehr als eine Bedingung zugeordnet ist, wird die Aktion nur ausgeführt, wenn alle Bedingungen erfüllt sind, d. h. es wird eine logische UND-Operation durchgeführt.

   - **Regel zum erneuten Generieren**: Die Regel zum erneuten Generieren enthält mehrere Kombinationen aus Aktion und Bedingung zum erneuten Generieren.

   - **Regelreihenfolge**: Bestimmt die Reihenfolge, in der die verschiedenen Regeln zum erneuten Generieren ausgeführt werden. Dies ist hilfreich, wenn es mehrere Regeln zum erneuten Generieren in einem Satz zum erneuten Generieren gibt. Die Regel zum erneuten Generieren mit einem niedrigeren Wert für die Regelreihenfolge wird zuerst ausgeführt. Wenn Sie dieselbe Regelreihenfolge für zwei Regeln zum erneuten Generieren festlegen, ist die Reihenfolge der Ausführung unbestimmt.

   - **Satz zum erneuten Generieren**: Enthält mehrere Regeln zum erneuten Generieren, die einer Anforderungsroutingregel zugeordnet werden.

2. Der Satz für das erneute Generieren muss an eine Routingregel angefügt werden. Dies liegt daran, dass die erstellte Konfiguration der erneuten Generierung anhand der Routingregel an den Quelllistener angefügt wird. Bei Verwendung einer einfachen Routingregel wird die Konfiguration der erneuten Generierung eines Headers einem Quelllistener zugeordnet und fungiert als erneute Generierung eines globalen Headers. Wenn eine pfadbasierte Routingregel verwendet wird, wird die Konfiguration der erneuten Generierung eines Headers in der URL-Pfadzuordnung definiert. Daher gilt sie nur für den bestimmten Pfadbereich einer Site.

Sie können mehrere solche Sätze für das erneute Generieren eines HTTP-Headers erstellen, und jeder dieser Sätze kann auf mehrere Listener angewendet werden. Allerdings können Sie auf einen bestimmten Listener nur einen Satz für das erneute Generieren anwenden.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich mit Ihrem Azure-Konto beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="configure-header-rewrite"></a>Konfigurieren des erneuten Generierens für einen Header

In diesem Beispiel werden wir die Umleitungs-URL ändern, indem der Adressheader in der HTTP-Antwort erneut generiert wird, die von der Back-End-Anwendung gesendet wurde. 

1. Wählen Sie die Option **Alle Ressourcen** und dann Ihr Anwendungsgateway aus.

2. Wählen Sie im linken Menü die Option **Erneut generieren** aus.

3. Klicken Sie auf **+ Satz zum erneuten Generieren**. 

   ![Hinzufügen eines Satzes zum erneuten Generieren](media/rewrite-http-headers-portal/add-rewrite-set.png)

4. Geben Sie dem Satz zum erneuten Generieren einen Namen, und ordnen Sie ihm eine Routingregel zu:

   - Geben Sie den Namen für den Satz zum erneuten Generieren in das Textfeld **Name** ein.
   - Wählen Sie eine oder mehrere Regeln aus, die in der Liste der **zugeordneten Routingregeln** aufgeführt sind. Sie können nur solche Regeln auswählen, die nicht anderen Sätzen zum erneuten Generieren zugeordnet sind. Die Regeln, die bereits anderen Sätzen zum erneuten Generieren zugeordnet sind, werden abgeblendet.
   - Klicken Sie auf "Weiter".
   
     ![Hinzufügen von Name und Zuordnung](media/rewrite-http-headers-portal/name-and-association.png)

5. Erstellen einer Regel zum erneuten Generieren:

   - Klicken Sie auf **+Regel zum erneuten Generieren hinzufügen**.![Regel zum erneuten Generieren hinzufügen](media/rewrite-http-headers-portal/add-rewrite-rule.png)
   - Geben Sie der Regel für das erneute Generieren im Textfeld für den Namen der Regel für das erneute Generieren einen Namen, und stellen Sie eine Regelreihenfolge bereit.![Hinzufügen von Regelnamen](media/rewrite-http-headers-portal/rule-name.png)

6. In diesem Beispiel wird der Adressheader nur dann erneut generiert, wenn er einen Verweis auf „azurewebsites.net“ enthält. Fügen Sie dazu eine Bedingung hinzu, um zu prüfen, ob der Adressheader in der Antwort „azurewebsites.net“ enthält:

   - Klicken Sie auf **+ Bedingung hinzufügen** und dann auf den Abschnitt mit den **If**-Anweisungen, um ihn zu erweitern.![Regelname hinzufügen](media/rewrite-http-headers-portal/add-condition.png)

   - Wählen Sie **HTTP-Header** aus der Dropdownliste für den **Typ der zu überprüfenden Variablen** aus. 

   - Wählen Sie **Headertyp** als **Antwort** aus.

   - Da in diesem Beispiel der Adressheader, der ein häufiger Header ist, ausgewertet wird, aktivieren Sie das Optionsfeld **Allgemeiner Header** als **Headername**.

   - Wählen Sie **Speicherort** aus der Dropdownliste **Allgemeiner Header** aus.

   - Wählen Sie **Keine** als Einstellung für die **Groß-/Kleinschreibung** aus.

   - Wählen Sie **gleich (=)** aus der Dropdownliste **Operator** aus.

   - Geben Sie das Muster für den regulären Ausdruck ein. In diesem Beispiel verwenden wir das Muster `(https?):\/\/.*azurewebsites\.net(.*)$` .

   - Klicken Sie auf **OK**.

     ![Ändern des Adressheaders](media/rewrite-http-headers-portal/condition.png)

7. Fügen Sie eine Aktion hinzu, um den Adressheader erneut zu generieren:

   - Wählen Sie **Satz** als **Aktionstyp** aus.

   - Wählen Sie **Antwort** als **Headertyp** aus.

   - Wählen Sie **Allgemeiner Header** als **Headername** aus.

   - Wählen Sie **Speicherort** aus der Dropdownliste **Allgemeiner Header** aus.

   - Geben Sie den Headerwert ein. In diesem Beispiel verwenden wir `{http_resp_Location_1}://contoso.com{http_resp_Location_2}` als Headerwert. Dadurch wird im Adressheader *azurewebsites.net* durch *contoso.com* ersetzt.

   - Klicken Sie auf **OK**.

     ![Ändern des Adressheaders](media/rewrite-http-headers-portal/action.png)

8. Klicken Sie auf **Erstellen**, um den Satz zum erneuten Generieren zu erstellen.

   ![Ändern des Adressheaders](media/rewrite-http-headers-portal/create.png)

9. Sie werden zur Ansicht „Satz zum erneuten Generieren“ geleitet. Vergewissern Sie sich, dass der von Ihnen oben erstellte Satz zum erneuten Generieren in der Liste der Sätze zum erneuten Generieren vorhanden ist.

   ![Ändern des Adressheaders](media/rewrite-http-headers-portal/rewrite-set-list.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Konfiguration, die erforderlich ist, um einige der häufigsten Anwendungsfälle durchzuführen, finden Sie unter [Allgemeine Szenarien zum erneuten Generieren von Headern](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers).

   
