| Ressource | Standardlimit | Höchstwert |
| --- | --- | --- |
| Ressourcen pro [Ressourcengruppe](../articles/azure-resource-manager/resource-group-overview.md#resource-groups) (pro Ressourcentyp "") |800 |Variiert je nach Ressourcentyp |
| Bereitstellungen pro Ressourcengruppe |800 |800 |
| Ressourcen pro Bereitstellung |800 |800 |
| Verwaltungssperren (pro eindeutigen Bereich) |20 |20 |
| Anzahl der Transponder (pro Ressource oder Ressourcengruppe) |15 |15 |
| Tag-Schlüssellänge |512 |512 |
| Die Länge der Tag-Werts |256 |256 |


#### <a name="template-limits"></a>Vorlage-Grenzwerte

| Wert | Standardlimit | Höchstwert |
| --- | --- | --- |
| Parameter |256 |256 |
| Variablen |256 |256 |
| Ressourcen (einschließlich kopieanzahl) |800 |800 |
| Ausgaben |64 |64 |
| Vorlagenausdruck |24,576 Zeichen |24,576 Zeichen |
| Ressourcen in exportierten Vorlagen |200 |200 | 
| Vorlagengröße |1 MB |1 MB |
| Parameter-Dateigröße |64 KB |64 KB |

Sie können einige Vorlage Grenzwerte überschreiten, mithilfe einer geschachtelten Vorlage. Weitere Informationen finden Sie unter [verknüpfte Vorlagen verwenden, bei der Bereitstellung von Azure-Ressourcen](../articles/azure-resource-manager/resource-group-linked-templates.md). Um die Anzahl von Parametern, Variablen oder Ausgaben zu reduzieren, können Sie mehrere Werte in ein Objekt kombinieren. Weitere Informationen finden Sie unter [Objekte als Parameter](../articles/azure-resource-manager/resource-manager-objects-as-parameters.md).