# 📚 Declarar e Inicializar Listas en VB.NET
En VB.NET, las listas se manejan con la clase genérica List(Of T), donde T es el tipo de datos que contendrá la lista (por ejemplo, String, Integer, etc.).

✅ Declarar una lista vacía
VisualBasicDim columnas As New List(Of String)()Show more lines
Esto crea una lista vacía de cadenas (String).

✅ Inicializar con valores
Puedes inicializar la lista con valores usando el inicializador de colección:
VisualBasicDim columnas As New List(Of String) From {"Nombre", "Apellido", "Edad"}Show more lines
Salida esperada:
columnas = ["Nombre", "Apellido", "Edad"]


✅ Agregar elementos después de crearla
VisualBasiccolumnas.Add("Dirección")columnas.Add("Teléfono")Show more lines

✅ Recorrer la lista
VisualBasicFor Each columna In columnas    Console.WriteLine(columna)NextShow more lines

✅ Otros ejemplos con diferentes tipos
Lista de enteros:
VisualBasicDim numeros As New List(Of Integer) From {1, 2, 3, 4, 5}Show more lines
Lista vacía y luego agregar:
VisualBasicDim frutas As New List(Of String)()frutas.Add("Manzana")frutas.Add("Pera")Show more lines

✅ Buenas prácticas

Usa List(Of T) para tipado fuerte.
Evita usar ArrayList (no es genérico).
Inicializa con From {} para mayor legibilidad.
