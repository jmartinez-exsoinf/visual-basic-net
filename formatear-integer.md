# 📘 Formatear un Integer como número en VB.NET
En Visual Basic .NET, puedes convertir un valor Integer a una cadena con formato numérico utilizando el método .ToString() con especificadores de formato.
🎯 Objetivo
Convertir un valor Integer en una cadena con formato numérico legible, incluyendo separadores de miles y decimales si es necesario.
🧪 Ejemplos prácticos
✅ Formato con separador de miles (sin decimales)
VisualBasicDim numero As Integer = 123456Dim resultado As String = numero.ToString("N0")Console.WriteLine(resultado) ' Salida: "123,456"Mostrar más líneas
✅ Formato con 2 decimales
VisualBasicDim numero As Integer = 123456Dim resultado As String = numero.ToString("N2")Console.WriteLine(resultado) ' Salida: "123,456.00"Mostrar más líneas
✅ Formato personalizado
VisualBasicDim numero As Integer = 123456Dim resultado As String = numero.ToString("#,##0")Console.WriteLine(resultado) ' Salida: "123,456"Mostrar más líneas

🌍 Consideraciones regionales
El formato puede variar según la configuración regional del sistema (por ejemplo, coma o punto como separador decimal).
✅ Usar formato invariable (por ejemplo, siempre con punto decimal)
VisualBasicImports System.GlobalizationDim numero As Integer = 123456Dim resultado As String = numero.ToString("N2", CultureInfo.InvariantCulture)Console.WriteLine(resultado) ' Salida: "123,456.00"Mostrar más líneas

🧩 Aplicaciones comunes

Mostrar valores en interfaces gráficas (TextBox, Label, etc.)
Exportar datos a archivos (Excel, PDF, CSV)
Generar reportes legibles para usuarios
