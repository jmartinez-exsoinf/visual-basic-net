# 🚀 Tutorial: Strict, Explicit e Infer en VB.NET

Estas tres directivas controlan cómo VB.NET maneja **tipos de datos y variables** en tiempo de compilación.

Se colocan al inicio del archivo:

```vbnet
Option Strict On
Option Explicit On
Option Infer On

```

----------

# ✅ 1. `Option Strict On`

## 🔍 ¿Qué hace?

Impide conversiones implícitas peligrosas y obliga a usar tipos correctamente.

----------

## ❌ Sin `Strict On`

```vbnet
Dim numero As Integer = "123"   ' Conversión automática
Dim obj As Object = 10
Dim resultado = obj + 5         ' Late binding

```

👉 Esto puede fallar en runtime.

----------

## ✅ Con `Strict On`

```vbnet
Dim numero As Integer = CInt("123")   ' Conversión explícita obligatoria
```

```vbnet
Dim obj As Object = 10
Dim resultado = CInt(obj) + 5
```

----------

## 🔐 Qué obliga:

-   Conversiones explícitas (`CType`, `CInt`, etc.)
-   Evita `late binding`
-   Evita usar `Object` sin control

----------

## 💡 Ejemplo con clases

```vbnet
Public Class Persona
    Public Nombre As String
End Class

Dim p As Object = New Persona()

' ❌ Error con Strict On
' Console.WriteLine(p.Nombre)

' ✅ Correcto
Console.WriteLine(CType(p, Persona).Nombre)
```

----------

# ✅ 2. `Option Explicit On`

## 🔍 ¿Qué hace?

Obliga a declarar todas las variables antes de usarlas.

----------

## ❌ Sin `Explicit On`

```vbnet
x = 10   ' VB crea la variable automáticamente
```

👉 Esto puede causar errores sutiles:

```vbnet
total = 100
totla = 200  ' ❌ typo que crea otra variable
```

----------

## ✅ Con `Explicit On`

```vbnet
Dim x As Integer = 10
```

👉 Si olvidas declarar:

```
Error: 'x' no está declarado
```

----------

## 🔐 Beneficios

-   Evita errores por typos
-   Hace código más claro
-   Mejora mantenimiento

----------

# ✅ 3. `Option Infer On`

## 🔍 ¿Qué hace?

Permite que VB **infiera automáticamente el tipo** de la variable.

----------

## ✅ Ejemplo

```vbnet
Dim x = 10          ' Integer
Dim nombre = "Jose" ' String
```

VB deduce el tipo automáticamente.

----------

## ❌ Con `Infer Off`

```vbnet
Dim x = 10   ' Se convierte en Object
```

👉 Esto degrada el tipado y rendimiento.

----------

## ✅ Ejemplo avanzado (LINQ)

```vbnet
Dim numeros = {1, 2, 3, 4}

Dim pares = numeros.Where(Function(n) n Mod 2 = 0)
```

👉 Aquí `pares` obtiene su tipo automáticamente.

----------

# ⚖️ Comparativa rápida

|Opción|Propósito|Problema que evita|
|-|-|-|
|`Strict On`|Tipado fuerte|Conversiones peligrosas|
|`Explicit On`|Declaración obligatoria|Variables fantasma|
|`Infer On`|Inferencia de tipos|Código verbose innecesario|

----------

# 🧠 Ejemplo completo combinando las 3

```vbnet
Option Strict On
Option Explicit On
Option Infer On

Module Program
    Sub Main()

        Dim texto = "123"   ' Infer → String

        ' Strict obliga conversión explícita
        Dim numero As Integer = CInt(texto)

        ' Explicit obliga declarar
        Dim resultado As Integer = numero + 10

        Console.WriteLine(resultado)

    End Sub
End Module
```

----------

# ✅ Configuración recomendada (PRO)

Siempre usa:

```vbnet
Option Strict On
Option Explicit On
Option Infer On
```

👉 Es el estándar en proyectos profesionales.

----------

# 🔥 Buenas prácticas

✅ Usa `Infer` para reducir ruido  
✅ Usa `Strict` para seguridad  
✅ Usa `Explicit` para evitar errores ocultos

----------

# 💡 Consejo clave

-   `Strict On` = **seguridad**
-   `Explicit On` = **disciplina**
-   `Infer On` = **productividad**

----------

# 🚀 Conclusión

Con estas tres opciones:

✔ Tu código será más seguro  
✔ Menos errores en runtime  
✔ Mejor rendimiento  
✔ Más mantenible
