Wenn der Functions-Host lokal ausgeführt wird, schreibt er Protokolle in den folgenden Pfad:

```
<DefaultTempDirectory>\LogFiles\Application\Functions
```

Unter Windows ist `<DefaultTempDirectory>` der erste gefundene Wert der Umgebungsvariablen TMP, TEMP, USERPROFILE oder das Windows-Verzeichnis.
Unter macOS oder Linux ist `<DefaultTempDirectory>` die Umgebungsvariable TMPDIR.

[!Note]
Wenn der Functions-Host gestartet wird, überschreibt er die vorhandene Dateistruktur im Verzeichnis.