Hier wird erklärt, wie mithilfe der **Service Bus - beim Empfang einer Nachricht in einer Warteschlange** Trigger, um eine Logik-app-Workflow zu initiieren, wenn ein neues Element an eine Service Bus-Warteschlange gesendet wird.  

> [!NOTE]
> Sie werden aufgefordert, die Ihrem Service Bus-Verbindungszeichenfolge Anmeldung, wenn Sie eine Verbindung mit Service Bus nicht bereits erstellt haben.  
> 
> 

1. Geben Sie in das Suchfeld auf dem Logik-apps Designer **service Bus-**. Wählen Sie dann die **Service Bus - beim Empfang einer Nachricht in einer Warteschlange** Trigger.  
   ![Service Bus Trigger Bild 1](./media/connectors-create-api-servicebus/trigger-1.png)   
2. Die **beim Empfang einer Nachricht in einer Warteschlange** Dialogfeld wird angezeigt.  
   ![Service Bus Trigger Bild 2](./media/connectors-create-api-servicebus/trigger-2.png)   
3. Geben Sie den Namen des Service Bus-Warteschlange, die den Trigger zum überwachen möchten.   
   ![Service Bus Trigger Bild 3](./media/connectors-create-api-servicebus/trigger-3.png)   

An diesem Punkt hat Ihre Logik-app mit einem Trigger konfiguriert wurde. Wenn ein neues Element in der Warteschlange empfangen wird, die Sie ausgewählt haben, wird der Trigger eine Ausführung von den anderen Triggern und Aktionen im Workflow gestartet.    

