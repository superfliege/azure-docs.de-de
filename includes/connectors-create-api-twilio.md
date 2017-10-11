### <a name="prerequisites"></a>Voraussetzungen
* Ein Twilio-Konto
* Eine überprüfte Twilio-Telefonnummer, die SMS empfangen kann
* Eine überprüfte Twilio-Telefonnummer, die SMS senden können

> [!NOTE]
> Wenn Sie ein Testabonnement Twilio-Konto verwenden, können nur SMS zu senden **überprüft** Telefonnummern.  
> 
> 

Bevor Sie Ihre Twilio-Konto in einer Logik-app verwenden können, müssen Sie die Logik-app für die Verbindung mit Ihrem Twilio-Konto autorisieren. Glücklicherweise können Sie problemlos aus in der Logik-app im Azure-Portal vorgehen. 

Es folgen die Schritte zum Autorisieren der Logik-app für die Verbindung mit Ihrem Twilio-Konto:

1. Wählen Sie zum Erstellen einer Verbindungs mit Twilio, in der Logik-app-Designer **Anzeigen von Microsoft verwalteten APIs** in der Dropdownliste aus und geben Sie dann *Twilio* in das Suchfeld. Wählen Sie den Trigger oder die Aktion an, die Sie verwenden möchten, müssen:  
   ![](./media/connectors-create-api-twilio/twilio-0.png)
2. Wenn Sie alle Verbindungen mit Twilio, bevor Sie erstellt haben, werden Sie aufgefordert erhalten Ihre Twilio-Anmeldeinformationen bereitstellen. Diese Anmeldeinformationen werden verwendet, um Ihre Logik-app für die Verbindung autorisieren und einfacher Zugriff auf Ihre Twilio-Konto:  
   ![](./media/connectors-create-api-twilio/twilio-1.png)  
3. Sie müssen die **Twilio-Konto-Id** und **Twilio-Zugriffstoken** über das Dashboard in Twilio so melden Sie sich mit Ihrem Twilio-Konto jetzt, sich zu beanspruchen diese zwei Angaben:  
   ![](./media/connectors-create-api-twilio/twilio-2.png)  
4. Twilio und Logik-apps verwenden unterschiedliche Namen, um diese zwei Angaben zu identifizieren. Hier ist, wie sie zum Dialogfeld "Logik-apps" zugeordnet werden müssen:![](./media/connectors-create-api-twilio/twilio-3.png)  
5. Wählen Sie die **Verbindung erstellen** Schaltfläche:  
   ![](./media/connectors-create-api-twilio/twilio-4.png)
6. Beachten Sie, dass die Verbindung erstellt wurde, und Sie können nun mit den anderen Schritten in der Logik-app zu fortfahren:  
   ![](./media/connectors-create-api-twilio/twilio-5.png)

