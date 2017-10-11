### <a name="record-names"></a>Record-Namen

In Azure DNS werden Datensätze mit relativen Namen angegeben. Ein *vollqualifizierte* Domänennamen (FQDN) enthält den Zonennamen an, während eine *relative* Name nicht der Fall. Beispielsweise bietet der relative Namen "Www" in der Zone "contoso.com" den Datensatznamen der vollständig qualifizierte "www.contoso.com".

Ein *Apex* Datensatz ist ein DNS-Datensatz auf der Stammebene (oder *Apex*) einer DNS-Zone. Angenommen, in der DNS-Zone "contoso.com", ein Datensatz Apex verfügt auch über den voll gekennzeichneten Namen "contoso.com" (Dies wird manchmal als bezeichnet eine *naked* Domäne).  Gemäß der Konvention, des relativen namens "@" wird verwendet, um Apex Datensätze darstellen.

### <a name="record-types"></a>Datensatztypen

Jeder DNS-Eintrag hat einen Namen und einen Typ. Datensätze sind in verschiedene Typen laut den Daten organisiert, die sie enthalten. Der am häufigsten verwendete Typ ist ein "A"-Datensatz, der einen Namen in eine IPv4-Adresse zugeordnet. Eine andere gemeinsamen Typ ist ein "MX"-Datensatz, der einen Namen, e-Mail-Server zugeordnet.

Azure DNS unterstützt allgemeine DNS-Eintragstypen: A, AAAA, CNAME, MX, NS, PTR, SOA, SRV und TXT. Beachten Sie, dass [SPF-Datensätze werden mithilfe von TXT-Einträgen dargestellt](../articles/dns/dns-zones-records.md#spf-records).

### <a name="record-sets"></a>Datensatzgruppen

In einigen Fällen müssen Sie mehrere DNS-Eintrag mit einem angegebenen Namen und Typ zu erstellen. Nehmen Sie beispielsweise an, dass die Website "www.contoso.com" auf zwei verschiedenen IP-Adressen gehostet wird. Die Website erfordert zwei verschiedenen A-Datensätze, eine für jede IP-Adresse. Hier ist ein Beispiel einer Datensatzgruppe:

    www.contoso.com.        3600    IN    A    134.170.185.46
    www.contoso.com.        3600    IN    A    134.170.188.221

Azure-DNS-verwaltet alle DNS-Einträge mithilfe *aufzeichnen legt*. Eine Datensatzgruppe (auch bekannt als ein *Ressource* Satz erfassen) ist eine Sammlung von DNS-Einträge in einer Zone, die denselben Namen aufweisen und den gleichen Typ aufweisen. Die meisten Datensatzgruppen enthalten einen einzelnen Datensatz. Beispiele, wie die oben stehende, in dem eine Datensatzgruppe enthält mehr als einen Datensatz jedoch sind nicht ungewöhnlich, dass.

Beispielsweise angenommen, das Sie bereits einen A-Eintrag "Www" in der Zone "contoso.com" erstellt haben, verweist auf die IP-Adresse "134.170.185.46" (der erste Datensatz oben).  Um den zweiten Datensatz zu erstellen würde Sie die vorhandene Datensatzgruppe diesen Datensatz hinzufügen, anstatt einen zusätzlichen Datensatz Satz zu erstellen.

Die SOA und CNAME Datensatztypen sind Ausnahmen. Die DNS-Standards nicht mehrere Datensätze mit dem gleichen Namen für diese Typen ermöglichen, daher können diese Datensatzgruppen nur einen einzelnen Datensatz enthalten.