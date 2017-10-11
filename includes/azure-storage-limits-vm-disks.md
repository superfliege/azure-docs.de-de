Virtueller Azure-Computer unterstützt eine Anzahl von Datenträgern anfügen. Für eine optimale Leistung sollten Sie die Anzahl der mit hoher Auslastung der virtuellen Maschine so vermeiden Sie mögliche Einschränkung angefügten Festplatten zu beschränken. Wenn alle Datenträger nicht hoch gleichzeitig genutzt werden, kann das Speicherkonto, das eine größere Anzahl Datenträger unterstützen.

* **Für Azure Managed-Datenträgern:** Limit für die Anzahl von Datenträgern verwaltet ist Land/Region und hängt auch von den Speichertyp. Standard, und auch die maximale Grenze liegt bei 10.000 pro Abonnement, pro Region und Speichertyp. Sie können beispielsweise bis zu 10.000 standard verwaltet erstellen verwalteten Datenträgern in einem Abonnement und in einer Region zu Datenträgern und auch 10.000 Premium. 

    Verwaltete Momentaufnahmen und Bilder werden Datenträger verwaltet hinsichtlich des Maximums gezählt.

* **Für standard Storage-Konten:** eine standard-Speicherkonto hat eine maximale gesamtanforderung 20.000 IOPS-Wert. Die Gesamtanzahl der IOPS aller Datenträger virtueller Computer in einer standard-Speicherkonto sollte dieses Limit nicht überschreiten.
  
    Sie können die Anzahl der stark ausgelastete Datenträger, die durch eine einzelne Standard-Speicherkonto basierend auf den Grenzwert für die tokenanforderungsrate unterstützt ungefähr berechnen. Für Beispiel für einen grundlegenden Ebene VM, die maximale Anzahl von Datenträgern mit hoher Auslastung über 66 (20.000/300 IOPS pro Datenträger) und für einen Standard-Ebene virtuellen Computer ist, ist es ca. 40 (20.000/500 IOPS pro Datenträger), wie in der folgenden Tabelle gezeigt. 
* **Für Premium Storage-Konten:** Premium-Speicherkonto hat eine maximale Gesamtdurchsatz von 50 Gbit/s. Der Gesamtdurchsatz für alle VM-Datenträger sollte dieses Limit nicht überschreiten.

