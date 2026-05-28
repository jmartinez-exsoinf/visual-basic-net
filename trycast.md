# 🚀 Tutorial: `TryCast` en VB.NET

## ✅ ¿Qué es `TryCast`?

`TryCast` se usa para convertir (cast) un objeto a un tipo **de forma segura**.

👉 Si la conversión falla:

-   ❌ **NO lanza excepción**
-   ✅ **Devuelve `Nothing`**

----------

# 🔍 Sintaxis

```vbnet
TryCast(expresion, Tipo)
```

----------

# ✅ Ejemplo básico

```vbnet
Dim obj As Object = "Hola mundo"
Dim texto As String = TryCast(obj, String)
If texto IsNot Nothing Then
    Console.WriteLine(texto)
Else
    Console.WriteLine("No se pudo convertir")
End If
```

----------

# ⚠️ ¿Qué pasa si falla?

```vbnet
Dim obj As Object = 123
Dim texto As String = TryCast(obj, String)
If texto Is Nothing Then
    Console.WriteLine("Conversión fallida")
End If
```

👉 Resultado:  
`texto = Nothing` (no hay error)

----------

## ✅ Ejemplo comparativo

```vbnet
Dim obj As Object = 123

' ❌ CType - lanza excepción
Dim a As String = CType(obj, String)

' ❌ DirectCast - lanza excepción
Dim b As String = DirectCast(obj, String)

' ✅ TryCast - seguro
Dim c As String = TryCast(obj, String)  ' Nothing

```

----------

# ✅ ¿Cuándo usar `TryCast`?

✔ Cuando no estás seguro del tipo  
✔ Cuando quieres evitar excepciones  
✔ Cuando trabajas con `Object`  
✔ Cuando recorres colecciones heterogéneas

----------

# 💡 Ejemplo práctico con clases

```vbnet
Public Class Animal
End Class

Public Class Perro
    Inherits Animal
    Public Sub Ladrar()
        Console.WriteLine("Guau")
    End Sub
End Class
```

Uso:

```vbnet
Dim a As Animal = New Perro()

Dim p As Perro = TryCast(a, Perro)

If p IsNot Nothing Then
    p.Ladrar()
End If

```

----------

# ❗ Importante

`TryCast` **solo funciona con tipos de referencia (clases)**

❌ No funciona con tipos valor:

```vbnet
Dim x As Object = 10
Dim n As Integer = TryCast(x, Integer)  ' ❌ ERROR
```

----------

## ✅ Solución para tipos valor

```vbnet
Dim n As Integer = CType(x, Integer)
```

----------

# ✅ Patrón común (muy usado)

```vbnet
Dim control As Object = GetControl()

Dim btn As Button = TryCast(control, Button)

If btn IsNot Nothing Then
    btn.Text = "Click"
End If
```

👉 Muy común en:

-   WinForms
-   WPF
-   ASP.NET

----------

# ✅ Forma corta (inline)

```vbnet
If TryCast(obj, String) IsNot Nothing Then
    Console.WriteLine("Es String")
End If
```

----------

# ✅ Buenas prácticas

✔ Siempre valida `Nothing`  
✔ Usa `TryCast` en lugar de `CType` cuando no estás seguro  
✔ Combínalo con `Option Strict On`  
✔ Evita excepciones innecesarias

----------

# 🚫 Errores comunes

## ❌ 1. No validar `Nothing`

```vbnet
Dim s As String = TryCast(obj, String)
Console.WriteLine(s.Length)  ' 💥 NullReferenceException
```

✅ Correcto:

```vbnet
If s IsNot Nothing Then
    Console.WriteLine(s.Length)
End If
```

----------

## ❌ 2. Usarlo con tipos valor

```vbnet
Dim n As Integer = TryCast(obj, Integer)  ' ❌
```

----------

# 🧠 Regla mental

👉 Usa `TryCast` cuando piensas:

> “Tal vez este objeto es de este tipo… pero no estoy seguro”

----------

# 🔥 Ejemplo real tipo profesional

```vbnet
Option Strict On

Dim lista As New List(Of Object) From {
    "Hola",
    123,
    New Perro()
}

For Each item In lista

    Dim texto = TryCast(item, String)
    If texto IsNot Nothing Then
        Console.WriteLine("Texto: " & texto)
        Continue For
    End If

    Dim perro = TryCast(item, Perro)
    If perro IsNot Nothing Then
        perro.Ladrar()
        Continue For
    End If

Next
```

----------

# ✅ Conclusión

`TryCast` es:

✅ Seguro (no rompe la app)  
✅ Ideal con `Option Strict On`  
✅ Perfecto para código robusto
