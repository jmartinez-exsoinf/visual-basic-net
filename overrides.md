# 🚀 Tutorial: `Overrides` en VB.NET

## ✅ ¿Qué es `Overrides`?

`Overrides` se usa cuando una clase hija (**derivada**) **reemplaza o modifica la implementación** de un método definido en una clase base.

👉 Es un concepto fundamental de **herencia y polimorfismo**.

----------

# 🧠 Idea clave

> Una clase hija puede cambiar el comportamiento de la clase padre.

----------

# ✅ Requisitos para usar `Overrides`

Para poder usar `Overrides`, en la clase base debes usar:

```vbnet
Overridable
```

----------

# 🔧 Ejemplo básico

## Clase base

```vbnet
Public Class Animal
    Public Overridable Sub HacerSonido()
        Console.WriteLine("Sonido genérico")
    End Sub
End Class
```

----------

## Clase derivada

```vbnet
Public Class Perro
    Inherits Animal

    Public Overrides Sub HacerSonido()
        Console.WriteLine("Guau")
    End Sub
End Class
```

----------

## Uso

```vbnet
Dim a As Animal = New Perro()
a.HacerSonido()
```

👉 Resultado:

```
Guau
```

----------

# 🔥 ¿Qué está pasando aquí?

Aunque la variable es tipo `Animal`, se ejecuta el método de `Perro`.

👉 Esto se llama **polimorfismo**

----------

# ✅ Diferencia entre `Overridable` y `Overrides`

|Palabra|Dónde se usa|Qué hace|
|-|-|-|
|`Overridable`|Clase base|Permite ser reemplazado|
|`Overrides`|Clase hija|Reemplaza la implementación|

----------

# ✅ Ejemplo con funciones (Return)

```vbnet
Public Class Figura
    Public Overridable Function Area() As Double
        Return 0
    End Function
End Class
```

```vbnet
Public Class Cuadrado
    Inherits Figura

    Public Property Lado As Double

    Public Overrides Function Area() As Double
        Return Lado * Lado
    End Function
End Class
```

----------

# ✅ Llamar al método base (`MyBase`)

Puedes extender el comportamiento en vez de reemplazarlo completamente:

```vbnet
Public Class Animal
    Public Overridable Sub HacerSonido()
        Console.WriteLine("Animal hace sonido")
    End Sub
End Class
```

```vbnet
Public Class Perro
    Inherits Animal

    Public Overrides Sub HacerSonido()
        MyBase.HacerSonido()
        Console.WriteLine("Guau")
    End Sub
End Class
```

👉 Resultado:

```
Animal hace sonido
Guau
```

----------

# ✅ `MustOverride` (forzar implementación)

Si quieres obligar a que todas las clases hijas implementen el método:

```vbnet
Public MustInherit Class Animal
    Public MustOverride Sub HacerSonido()
End Class
```

```vbnet
Public Class Perro
    Inherits Animal

    Public Overrides Sub HacerSonido()
        Console.WriteLine("Guau")
    End Sub
End Class
```

👉 Aquí **no hay implementación en la base**, es obligatoria en las hijas.

----------

# ✅ `NotOverridable` (bloquear override)

Puedes evitar que una clase hija vuelva a sobrescribir un método:

```vbnet
Public Class Animal
    Public Overridable Sub HacerSonido()
        Console.WriteLine("Animal")
    End Sub
End Class
```

```vbnet
Public Class Perro
    Inherits Animal

    Public NotOverridable Overrides Sub HacerSonido()
        Console.WriteLine("Perro")
    End Sub
End Class
```

👉 Otra clase ya NO podrá hacer override.

----------

# ✅ `Overrides` en propiedades

También funciona en propiedades:

```vbnet
Public Class Persona
    Public Overridable Property Nombre As String
End Class
```

```vbnet
Public Class Empleado
    Inherits Persona

    Public Overrides Property Nombre As String
End Class
```

----------

# 🚫 Errores comunes

## ❌ 1. Usar `Overrides` sin `Overridable`

```vbnet
Public Class Base
    Public Sub Metodo()
    End Sub
End Class

Public Class Hija
    Inherits Base

    Public Overrides Sub Metodo()   ' ❌ ERROR
    End Sub
End Class
```

👉 Solución:

```vbnet
Public Overridable Sub Metodo()
```

----------

## ❌ 2. Olvidar `Overrides`

```vbnet
Public Class Perro
    Inherits Animal

    Public Sub HacerSonido()   ' ❌ No sobrescribe
    End Sub
End Class
```

👉 Esto **no reemplaza**, crea otro método.

----------

# 🔥 Diferencia con `Overloads`

|Keyword|Función|
|-|-|
|`Overrides`|Reemplaza método heredado|
|`Overloads`|Crea variantes (mismo nombre, diferentes parámetros)|

----------

## Ejemplo `Overloads`

```vbnet
Public Class Calculadora
    Public Overloads Function Sumar(a As Integer, b As Integer) As Integer
        Return a + b
    End Function

    Public Overloads Function Sumar(a As Double, b As Double) As Double
        Return a + b
    End Function
End Class
```

----------

# 🧠 Cuándo usar `Overrides`

✔ Cuando necesitas comportamiento específico por tipo  
✔ Cuando usas herencia  
✔ Cuando implementas lógica base reutilizable

----------

# 🔥 Ejemplo real tipo profesional

```vbnet
Public MustInherit Class Documento
    Public MustOverride Function Generar() As String
End Class
```

```vbnet
Public Class PDF
    Inherits Documento

    Public Overrides Function Generar() As String
        Return "Generando PDF"
    End Function
End Class
```

```vbnet
Public Class WordDoc
    Inherits Documento

    Public Overrides Function Generar() As String
        Return "Generando Word"
    End Function
End Class
```

Uso:

```vbnet
Dim docs As List(Of Documento) = New List(Of Documento) From {
    New PDF(),
    New WordDoc()
}

For Each doc In docs
    Console.WriteLine(doc.Generar())
Next
```

----------

# ✅ Conclusión

`Overrides` permite:

✅ Personalizar comportamiento heredado  
✅ Aplicar polimorfismo  
✅ Crear código flexible y reutilizable

----------

# 💡 Resumen rápido

-   `Overridable` → permite sobrescribir
-   `Overrides` → sobrescribe
-   `MustOverride` → obliga a implementar
-   `NotOverridable` → bloquea cambios
