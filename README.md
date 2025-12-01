# 📘 Visual Basic Net

## [📘 Formatear un Integer como número](https://github.com/jmartinez-exosinf/visual-basic-net/blob/a8fb69e35f068136e34416ebe68524edb195ed11/formatear-integer.md)
En Visual Basic .NET, puedes convertir un valor Integer a una cadena con formato numérico utilizando el método .ToString() con especificadores de formato.

## [Construcción de cadenas dinámicas](https://github.com/jmartinez-exosinf/visual-basic-net/blob/7c5893f2c4cc747a151317e6d957a6f5cec47a1b/cadenas-dinamicas-strings.md)
En Visual Basic .NET, construir cadenas dinámicas (strings) es una tarea común. Hay varias formas de hacerlo, pero dos de las más recomendadas son: ```String.Format``` e interpolación de cadenas ```($"")```.

## [📅 Uso de DateTime.TryParseExact en VB.NET](datetime-tryparseexact.md)
Cuando necesitas convertir una cadena a DateTime asegurando que cumpla un formato exacto, la mejor opción es usar DateTime.TryParseExact. Esto evita errores y permite validar la entrada sin lanzar excepciones.

## [Simular una Operación Ternaria en VB.NET con `If()` y `TryParse`](simular-operacion-ternaria.md)
En VB.NET no existe directamente el operador ternario, sin embargo, puedes lograr el mismo comportamiento utilizando la función `If()` o una estructura `If...Then...Else`.

## [Convertir un DataTable a JSON](https://github.com/jmartinez-exosinf/visual-basic-net/blob/8faea33a24591caf0a4e8e0637fb03952280f696/datatable-a-json.md)
En **VB.NET**, puedes convertir un objeto ``DataTable`` a una cadena **JSON** utilizando la biblioteca **Json.NET (Newtonsoft.Json)**. Esto es útil para exportar datos, integrarte con APIs o guardar información estructurada.

## [Uso de Async y Await en Controllers VB.NET](https://github.com/jmartinez-exosinf/visual-basic-net/blob/f33f74d26e730515773d79593384a665900eb41b/async-y-await.md)
En aplicaciones ASP.NET MVC o Web API, los controladores pueden beneficiarse del uso de **métodos asíncronos** para mejorar la escalabilidad y el rendimiento, especialmente cuando se realizan operaciones I/O como consultas a base de datos o llamadas a servicios externos.

# Temas de Vulnerabilidad

## [✅ Validar la entrada del usuario antes de usarla en consultas **SQL (VB.NET)**](validar-entradas-usuario-para-consultas-sql.md)
La validación de datos es crítica para evitar vulnerabilidades como **SQL Injection** y errores en la aplicación. Nunca debes usar datos sin verificar directamente en consultas **SQL**.

## [🛡️ Cómo Sanitizar Nombres de Archivo](como-sanitizar-nombres-de-archivo.md)
Sanitizar nombres de archivo es esencial para evitar errores y vulnerabilidades como directory traversal o el uso de caracteres inválidos. Este tutorial muestra cómo crear una función segura en VB.NET.
