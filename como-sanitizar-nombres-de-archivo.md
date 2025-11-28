# 🛡️ Cómo Sanitizar Nombres de Archivo en VB.NET
Sanitizar nombres de archivo es esencial para evitar errores y vulnerabilidades como directory traversal o el uso de caracteres inválidos. Este tutorial muestra cómo crear una función segura en VB.NET.

## ✅ Objetivo
- Eliminar caracteres peligrosos.
- Evitar rutas relativas **(../)**.
- Limitar la longitud del nombre.
- Garantizar compatibilidad con el sistema de archivos.

## 🧩 Función Mejorada
```VisualBasic
Imports System.IO
Imports System.Text.RegularExpressions

Public Function GetSafeFileName(fileName As String) As String
    ' 1. Validar que no esté vacío
    If String.IsNullOrWhiteSpace(fileName) Then
        Throw New ArgumentException("El nombre de archivo no puede estar vacío.")
    End If

    ' 2. Eliminar saltos de línea y espacios extremos
    Dim safeFileName As String = fileName.Trim().
        Replace(Environment.NewLine, "").
        Replace(vbCr, "").
        Replace(vbLf, "")

    ' 3. Eliminar rutas relativas (../ o ..\)
    safeFileName = safeFileName.Replace("../", "").Replace("..\", "")

    ' 4. Eliminar caracteres inválidos para nombres de archivo
    Dim invalidChars As String = Regex.Escape(New String(Path.GetInvalidFileNameChars()))
    safeFileName = Regex.Replace(safeFileName, "[" & invalidChars & "]", "")

    ' 5. Limitar longitud (ej. 255 caracteres)
    If safeFileName.Length > 255 Then
        safeFileName = safeFileName.Substring(0, 255)
    End If
    Return safeFileName
End Function
```

## 🔍 Explicación Paso a Paso

### Validación inicial
- Evita nombres vacíos que puedan causar errores.
- Eliminar saltos de línea
- Previene inyecciones y errores en rutas.
- Eliminar rutas relativas
- Protege contra ataques tipo directory traversal.
- Filtrar caracteres inválidos
- Usa `Path.GetInvalidFileNameChars()` para compatibilidad con el sistema.
- Controlar longitud máxima
- Evita errores en sistemas que limitan el tamaño del nombre.

## ✅ Ejemplo de uso
```VisualBasic
Dim originalName As String = "../miArchivo?.txt"
Dim safeName As String = GetSafeFileName(originalName)
Console.WriteLine(safeName) ' Salida: "miArchivo.txt"
```
