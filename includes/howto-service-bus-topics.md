## <a name="what-are-service-bus-topics-and-subscriptions"></a>Was sind Service Bus-Themen und Abonnements?
Service Bus-Themen und Abonnements Unterstützung einer *Veröffentlichen/Abonnieren* Kommunikationsmodell messaging. Wenn Themen und Abonnements verwenden möchten, führen Sie Komponenten einer verteilten Anwendung nicht direkt miteinander kommunizieren; Stattdessen tauschen sie Nachrichten über ein Thema, das als Vermittler fungiert.

![TopicConcepts](./media/howto-service-bus-topics/sb-topics-01.png)

Im Gegensatz zu Service Bus-Warteschlangen, die in denen jede Nachricht von einem einzigen Consumer verarbeitet werden bieten Themen und Abonnements über eine "eins zu viele"-Kommunikation, die mit einem Veröffentlichen/Abonnieren-Muster. Es ist möglich, mehrere Abonnements für ein Thema zu registrieren. Wenn eine Nachricht an ein Thema gesendet wird, wird es dann für alle Abonnements verfügbar versucht, Handle/unabhängig Prozess.

Ein Abonnement für ein Thema ähnelt eine virtuelle Warteschlange, die Kopien der Nachrichten empfängt, die an das Thema gesendet wurden. Optional können Sie Filterregeln für ein Thema auf der Grundlage einer pro-Abonnement registrieren, die wodurch Sie filtern oder einschränken, welche Nachrichten an ein Thema von themaabonnements empfangen werden.

Service Bus-Themen und Abonnements ermöglichen es Ihnen, zu skalieren und eine sehr große Anzahl von Nachrichten über viele Benutzer und Anwendungen zu verarbeiten.

## <a name="create-a-namespace"></a>Erstellen Sie einen namespace
Um zu Service Bus-Themen und Abonnements in Azure verwenden, erstellen Sie zuerst eine *Dienstnamespace*. Ein Namespace stellt ein bereichsdefinitionscontainer für die Adressierung von Service Bus-Ressourcen innerhalb Ihrer Anwendung bereit.

So erstellen Sie einen namespace

1. Melden Sie sich an den [Azure-Portal][Azure portal].
2. Klicken Sie im linken Navigationsbereich des Portals auf **neu**, klicken Sie dann auf **Enterprise Integration**, und klicken Sie dann auf **Service Bus**.
3. In der **Namespace erstellen** Dialogfeld, geben Sie einen Namespace ein. Das System überprüft sofort, um festzustellen, ob der Name verfügbar ist.
4. Nachdem sichergestellt wird der Name des Namespaces verfügbar ist, wählen Sie die Preisstufe (Basic, Standard oder Premium).
5. In der **Abonnement** Feld, und wählen Sie ein Azure-Abonnement, um den Namespace zu erstellen.
6. In der **Ressourcengruppe** Feld, und wählen Sie eine vorhandene Ressourcengruppe, in dem der Namespace wird Live- oder ein neues erstellen.      
7. In **Speicherort**, wählen Sie das Land bzw. die Region, die in der der Namespace gehostet werden soll.
   
    ![Namespace erstellen][create-namespace]
8. Klicken Sie auf die **erstellen** Schaltfläche. Das System jetzt Ihren Namespace erstellt und aktiviert. Möglicherweise müssen Sie einige Minuten als Vorschriften Systemressourcen für Ihr Konto zu warten.

### <a name="obtain-the-credentials"></a>Abrufen der Anmeldeinformationen
1. Klicken Sie in der Liste der Namespaces auf die neu erstellte Namespacename.
2. In der **Service Bus-Namespace** Blatt, klicken Sie auf **mit gemeinsamen Zugriffsrichtlinien**.
3. In der **mit gemeinsamen Zugriffsrichtlinien** Blatt, klicken Sie auf **RootManageSharedAccessKey**.
   
    ![Verbindungsinformationen][connection-info]
4. In der **Richtlinie: RootManageSharedAccessKey** Blatt, klicken Sie auf die Kopie Schaltfläche neben **Zeichenfolge – primäre Verbindungsschlüssel**, um die Verbindungszeichenfolge in die Zwischenablage zur späteren Verwendung zu kopieren.
   
    ![Verbindungszeichenfolgen][connection-string]

[Azure portal]: https://portal.azure.com
[create-namespace]: ./media/howto-service-bus-topics/create-namespace.png
[connection-info]: ./media/howto-service-bus-topics/connection-info.png
[connection-string]: ./media/howto-service-bus-topics/connection-string.png


