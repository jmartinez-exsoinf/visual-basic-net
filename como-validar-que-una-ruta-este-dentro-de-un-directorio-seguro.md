# ✅ Cómo validar que una ruta esté dentro de un directorio seguro en VB.NET

## ¿Por qué es importante?
Cuando recibes una ruta desde el cliente (por ejemplo, para subir o eliminar un archivo), un atacante podría enviar algo como:
```dir
..\..\Windows\System32\config\SAM
```

Esto intenta salir del directorio seguro y acceder a archivos del sistema.
Solución: Normalizar la ruta y verificar que esté dentro de tu carpeta segura (ej. `~/UploadedFiles`).

## ⚠️ Recomendación clave
Nunca envíes la ruta completa como parámetro desde el cliente. En su lugar, envía solo el nombre de la carpeta o archivo que deseas trabajar. Esto minimiza el riesgo y facilita la validación en el servidor.

Ejemplo:
```dir
MALO: "C:\inetpub\wwwroot\UploadedFiles\Reportes\archivo.txt"
BUENO: "Reportes\archivo.txt"
```

## Paso 1: Define tu directorio base seguro
Este será el único lugar donde se permitirán operaciones:
```VisualBasic
Dim basePath As String = HttpContext.Current.Server.MapPath("~/UploadedFiles/")
```

## Paso 2: Normaliza la ruta proporcionada
Convierte la ruta relativa en absoluta y limpia separadores:
```VisualBasic
Dim relativePath As String = request.FilePath.Replace("/", "\").Trim()
```

## Paso 3: Combina y obtiene la ruta completa
Usa `Path.Combine` para unir la ruta base con la relativa:
```VisualBasic
Dim fullPath As String = Path.Combine(basePath, relativePath)
```

## Paso 4: Normaliza la ruta final
Esto elimina cualquier `..\` y convierte la ruta en absoluta:
```VisualBasic
Dim normalizedPath As String = Path.GetFullPath(fullPath)
```

## Paso 5: Verifica que esté dentro del directorio seguro
Compara que la ruta normalizada comience con la ruta base:
```VisualBasic
If Not normalizedPath.StartsWith(basePath, StringComparison.OrdinalIgnoreCase) Then
    Throw New UnauthorizedAccessException("Ruta fuera del directorio permitido.")
End If
```

## ✅ Código completo
```VisualBasic
Imports System.IO

' Directorio base seguro
Dim basePath As String = HttpContext.Current.Server.MapPath("~/UploadedFiles/")

' ⚠️ Se recomienda que `request.FilePath` contenga solo la carpeta o subcarpeta, NO la ruta completa
Dim relativePath As String = request.FilePath.Replace("/", "\").Trim()

' Combinar y normalizar
Dim fullPath As String = Path.Combine(basePath, relativePath)
Dim normalizedPath As String = Path.GetFullPath(fullPath)

' Validar que esté dentro del directorio seguro
If Not normalizedPath.StartsWith(basePath, StringComparison.OrdinalIgnoreCase) Then
    Throw New UnauthorizedAccessException("Ruta fuera del directorio permitido.")
End If

```

## ✅ Buenas prácticas adicionales
✔ Valida caracteres inválidos con `Path.GetInvalidFileNameChars()`.
✔ Usa lista blanca para nombres de carpetas y archivos.
✔ Nunca confíes en rutas enviadas por el cliente sin sanitización.
✔ Siempre envía solo nombres relativos (carpeta/archivo), no rutas completas.
