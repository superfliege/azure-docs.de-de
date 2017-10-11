
### <a name="cacheskuname"></a>cacheSKUName
Die Preisstufe des neuen Azure Redis-Caches.

    "cacheSKUName": {
      "type": "string",
      "allowedValues": [
        "Basic",
        "Standard"
      ],
      "defaultValue": "Basic",
      "metadata": {
        "description": "The pricing tier of the new Azure Redis Cache."
      }
    },

Die Vorlage definiert die Werte, die für diesen Parameter (Basic- oder Standard) zulässig sind, und weist den Standardwert (Basic), wenn kein Wert angegeben wird. Basic bietet einen einzigen Knoten mit verschiedenen Größen verfügbar, um bis zu 53 GB.
Standard bietet zwei Knoten primär/Replikat mit verschiedenen Größen verfügbar von 53 GB und 99,9 % SLA.

### <a name="cacheskufamily"></a>cacheSKUFamily
Der für die Sku-Familie.

    "cacheSKUFamily": {
      "type": "string",
      "allowedValues": [
        "C"
      ],
      "defaultValue": "C",
      "metadata": {
        "description": "The family for the sku."
      }
    },


### <a name="cacheskucapacity"></a>cacheSKUCapacity
Die Größe der neuen Azure-Redis-Cache-Instanz. 

    "cacheSKUCapacity": {
      "type": "int",
      "allowedValues": [
        0,
        1,
        2,
        3,
        4,
        5,
        6
      ],
      "defaultValue": 0,
      "metadata": {
        "description": "The size of the new Azure Redis Cache instance. "
      }
    }


Die Vorlage definiert die Werte, die für diesen Parameter (0, 1, 2, 3, 4, 5 oder 6) zulässig sind, und weist einen Standardwert (1), wenn kein Wert angegeben wird. Diese Zahlen entsprechen folgenden Cachegrößen: 0 = 250 MB, 1 = 1 GB, 2 = 2,5 GB, 3 = 6 GB, 4 = 13 GB, 5 = 26 GB, 6 = 53 GB

