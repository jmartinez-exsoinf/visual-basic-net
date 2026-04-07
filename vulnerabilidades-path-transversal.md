# Solución 1

Evitar construir rutas de archivos dentro de los métodos, es mejor enviar la ruta completa y despues sanitizar:
```VisualBasicNet
   Public Function GetCSVArray(filePath As String) As Object
       Try
           Dim safeFilePath As String = sanitize.GetSafeFilePath(filePath)
           If Not File.Exists(safeFilePath) Then
               Return Nothing
           End If

          ....
    End Function
```

# Solución 2

En métodos privados, evitar el uso de parámetros, utilizar variables a nivel módulo o globales
