Ein virtueller Azure-Computer unterstützt das Anfügen mehrerer Datenträger. In diesem Artikel werden die Skalierbarkeits- und Leistungsziele für die Datenträger eines virtuellen Computers beschrieben. Wählen Sie anhand dieser Ziele die Anzahl und die Art der Datenträger, die Sie benötigen, um Leistungs- und Kapazitätsanforderungen zu erfüllen. 

> [!IMPORTANT]
> Begrenzen Sie zur Optimierung der Leistung die Anzahl stark ausgelasteter Datenträger, die an den virtuellen Computer angefügt sind, um eine mögliche Drosselung zu vermeiden. Wenn nicht alle angefügten Datenträger zum gleichen Zeitpunkt stark ausgelastet sind, kann der virtuelle Computer eine größere Anzahl von Datenträgern unterstützen.

* **Für Azure Managed Disks:** Der Datenträgergrenzwert für verwaltete Datenträger gilt pro Region und Datenträgertyp. Der Grenzwert (gleichzeitig auch der Standardgrenzwert) liegt bei 10.000 verwalteten Datenträgern pro Region und Datenträgertyp für ein Abonnement. Sie können beispielsweise bis zu 10.000 verwaltete Standarddatenträger und auch 10.000 verwaltete Premium-Datenträger in einer Region pro Abonnement erstellen.

    Verwaltete Momentaufnahmen und Images werden beim Grenzwert für verwaltete Datenträger berücksichtigt.

* **Für Standardspeicherkonten:** Ein Standardspeicherkonto hat eine maximale Gesamtanforderungsrate von 20.000 IOPS. Die gesamten IOPS auf allen Datenträgern eines virtuellen Computers in einem Standardspeicherkonto dürfen dieses Limit nicht überschreiten.
  
    Basierend auf dem Limit für Anforderungsraten können Sie die Anzahl der Datenträger mit hoher Auslastung ungefähr berechnen, die von einem Speicherkonto unterstützt werden. Im Basic-Tarif liegt die maximal zulässige Anzahl von Datenträgern mit hoher Auslastung für einen virtuellen Computer beispielsweise bei 66 (20.000 : 300 IOPS pro Datenträger) und im Standard-Tarif bei ungefähr 40 (20.000 : 500 IOPS pro Datenträger). 

* **Für Storage Premium-Konten:** Ein Storage Premium-Konto hat eine maximale Gesamtdurchsatzrate von 50 Gbit/s. Der Gesamtdurchsatz aller Ihrer VM-Datenträger darf dieses Limit nicht überschreiten.

