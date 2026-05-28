# 📘 Visual Basic Net

## [📘 Formatear un Integer como número](https://github.com/jmartinez-exosinf/visual-basic-net/blob/a8fb69e35f068136e34416ebe68524edb195ed11/formatear-integer.md)
En Visual Basic .NET, puedes convertir un valor Integer a una cadena con formato numérico utilizando el método .ToString() con especificadores de formato.

## [Construcción de cadenas dinámicas](https://github.com/jmartinez-exosinf/visual-basic-net/blob/7c5893f2c4cc747a151317e6d957a6f5cec47a1b/cadenas-dinamicas-strings.md)
En Visual Basic .NET, construir cadenas dinámicas (strings) es una tarea común. Hay varias formas de hacerlo, pero dos de las más recomendadas son: ```String.Format``` e interpolación de cadenas ```($"")```.

## [📅 Uso de DateTime.TryParseExact en VB.NET](datetime-tryparseexact.md)
Cuando necesitas convertir una cadena a DateTime asegurando que cumpla un formato exacto, la mejor opción es usar DateTime.TryParseExact. Esto evita errores y permite validar la entrada sin lanzar excepciones.

## [Simular una Operación Ternaria en VB.NET con `If()` y `TryParse`](simular-operacion-ternaria.md)
En VB.NET no existe directamente el operador ternario, sin embargo, puedes lograr el mismo comportamiento utilizando la función `If()` o una estructura `If...Then...Else`.

## [✅ ¿Qué es `TryCast`?](trycast.md)
`TryCast` se usa para convertir (cast) un objeto a un tipo **de forma segura**.

## [Convertir un DataTable a JSON](https://github.com/jmartinez-exosinf/visual-basic-net/blob/8faea33a24591caf0a4e8e0637fb03952280f696/datatable-a-json.md)
En **VB.NET**, puedes convertir un objeto ``DataTable`` a una cadena **JSON** utilizando la biblioteca **Json.NET (Newtonsoft.Json)**. Esto es útil para exportar datos, integrarte con APIs o guardar información estructurada.

## [Uso de Async y Await en Controllers VB.NET](async-y-await.md)
En aplicaciones ASP.NET MVC o Web API, los controladores pueden beneficiarse del uso de **métodos asíncronos** para mejorar la escalabilidad y el rendimiento, especialmente cuando se realizan operaciones I/O como consultas a base de datos o llamadas a servicios externos.

## [🚀 Strict, Explicit e Infer en VB.NET](strict-explicit-infer.md)
Estas tres directivas controlan cómo VB.NET maneja **tipos de datos y variables** en tiempo de compilación.

# Temas de Vulnerabilidad

## [✅ Validar la entrada del usuario antes de usarla en consultas **SQL (VB.NET)**](validar-entradas-usuario-para-consultas-sql.md)
La validación de datos es crítica para evitar vulnerabilidades como **SQL Injection** y errores en la aplicación. Nunca debes usar datos sin verificar directamente en consultas **SQL**.

## [🛡️ Cómo Sanitizar Nombres de Archivo](como-sanitizar-nombres-de-archivo.md)
Sanitizar nombres de archivo es esencial para evitar errores y vulnerabilidades como directory traversal o el uso de caracteres inválidos. Este tutorial muestra cómo crear una función segura en VB.NET.

## [✅ Cómo validar que una ruta esté dentro de un directorio seguro en VB.NET](como-validar-que-una-ruta-este-dentro-de-un-directorio-seguro.md)
Antes de procesar cualquier ruta proporcionada por el cliente, es fundamental validarla para garantizar que permanezca dentro del directorio seguro definido por la aplicación. Esto evita vulnerabilidades críticas como el acceso a carpetas privadas mediante técnicas de traversal.

## [✅ Pasar el objeto de conexión como parámetro puede ser vulnerable](pasar-el-objeto-de-conexion-como-parametro-puede-ser-vulnerable.md)
Si el método recibe la conexión desde fuera, el código que lo llama puede alterarla antes o después de ejecutarse, afectando seguridad y estabilidad. Aquí aprenderás **por qué ocurre y cómo evitarlo**.
