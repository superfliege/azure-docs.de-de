## <a name="what-are-service-bus-queues"></a>Was sind Service Bus-Warteschlangen?
Service Bus-Warteschlangen unterstützen ein **brokermessaging** Kommunikationsmodell. Bei Verwendung von Warteschlangen führen Sie die Komponenten einer verteilten Anwendung nicht direkt miteinander kommunizieren; Stattdessen tauschen sie Nachrichten über eine Warteschlange fungiert als Mittler (Broker) an. Eine Nachricht Producer (Absender) eine Nachricht an die Warteschlange Dienstschicht und deren Verarbeitung fortgesetzt. Asynchron ausgeführt wird, wird ein nachrichtenconsumer (Empfänger) der Nachricht aus der Warteschlange abruft und verarbeitet es. Der Producer muss nicht in eine Antwort vom Consumer warten, um weiterhin zu verarbeiten und weitere Nachrichten senden. -Warteschlangen bieten **First In, erste Out (FIFO)** -Nachrichtenübermittlung für eine oder mehrere konkurrierende Consumer. D. h. werden von den Empfängern in der Reihenfolge, in der sie hinzugefügt wurden, an die Warteschlange und jede Nachricht wird empfangen und nur von einem nachrichtenconsumer verarbeitet, Nachrichten in der Regel empfangen und verarbeitet.

![QueueConcepts](./media/howto-service-bus-queues/sb-queues-08.png)

Service Bus-Warteschlangen sind eine allgemeine Technologie, die für eine Vielzahl von Szenarien verwendet werden kann:

* Die Kommunikation zwischen Web- und Workerrollen Rollen in einer Multi-Tier-Azure-Anwendung.
* Die Kommunikation zwischen lokalen und Azure gehosteten apps in Hybrid-Lösung.
* Kommunikation zwischen Komponenten einer verteilten Anwendung, die lokal in verschiedenen Organisationen oder Abteilungen einer Organisation ausgeführt wird.

Verwendung von Warteschlangen, können Sie Ihre Anwendungen leichter zu skalieren, und aktivieren mehr Flexibilität für Ihre Architektur.


