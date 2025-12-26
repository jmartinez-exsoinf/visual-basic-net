# Manejo de Excepciones (Improper Exception Handling)

Fecha: 2025-12-26
Aplicación: ICMTools (ASP.NET / VB.NET)
Autor: Martinez Lira, Jose Nestor (con apoyo de M365 Copilot)

1. Resumen Ejecutivo
Problema detectado:
Algunas funciones y controladores realizaban operaciones que pueden lanzar excepciones (E/S de archivos, parseo CSV, acceso a BD, serialización) sin manejo apropiado. Esto fue reportado por Checkmarx One como Improper Exception Handling.
Riesgos principales:

Respuestas inconsistentes (p.ej., HTTP 200 con errores).
Caídas no controladas del proceso/acción.
Falta de trazabilidad (logs insuficientes).
Potencial exposición de detalles internos si la excepción burbujea hasta el cliente.

Solución aplicada:
Se agregó manejo robusto de excepciones con Try/Catch, validación previa de entradas, liberación de recursos (Using/Finally), logging estructurado y mapeo a códigos HTTP adecuados.

2. ¿Por qué es importante agregar Try/Catch?

Resiliencia: Evita que una excepción no controlada derribe una petición o el proceso.
Observabilidad: Permite registrar el contexto de error para análisis y soporte.
Seguridad: Reduce el riesgo de filtrar información sensible en la respuesta.
UX/API Contracts: Asegura que el cliente recibe códigos HTTP y mensajes consistentes.
Cumplimiento: Cierra hallazgos de herramientas como Checkmarx One y prácticas de OWASP (robust error handling).


3. Patrón recomendado (ASP.NET MVC / VB.NET)
3.1. Estructura general

Validar antes de operar (null/empty, existencia de archivo, rangos).
Using para liberar recursos (archivos, conexiones).
Capturas específicas primero; genérica (Exception) al final.
Logging con contexto (ruta, usuario, parámetros) sin datos sensibles.
Mapear a códigos HTTP adecuados (4xx/5xx).

VisualBasicTry    ' Operación susceptible a excepción (IO/DB/Parse/Red/etc.)Catch ex As FileNotFoundException    ' 404 Not FoundCatch ex As UnauthorizedAccessException    ' 403 ForbiddenCatch ex As IOException    ' 500 Internal Server Error (IO)Catch ex As FormatException    ' 422 Unprocessable EntityCatch ex As Exception    ' 500 genéricoEnd TryShow more lines

4. Ejemplos antes / después
4.1. Conversión CSV → JSON (GetJSONFromCSV)
Antes (riesgoso):
VisualBasicUsing reader As New StreamReader(FilePath)    Using csv As New CsvReader(reader, CultureInfo.InvariantCulture)        Using JsonWriter As New JsonTextWriter(sw)            csv.Read()            csv.ReadHeader()            While csv.Read()                ' ...            End While        End Using    End UsingEnd UsingShow more lines
Después (robusto):
VisualBasicIf String.IsNullOrWhiteSpace(FilePath) Then    Throw New ArgumentException("FilePath no puede ser nulo o vacío.", NameOf(FilePath))End IfDim fullPath As String = Path.GetFullPath(FilePath)If Not File.Exists(fullPath) Then    Throw New FileNotFoundException("El archivo CSV no existe.", fullPath)End IfDim config As New CsvConfiguration(CultureInfo.InvariantCulture) With {    .IgnoreBlankLines = True,    .TrimOptions = TrimOptions.Trim,    .MissingFieldFound = Nothing,    .HeaderValidated = Nothing}Try    Using reader As New StreamReader(fullPath)        Using csv As New CsvReader(reader, config)            Using jsonWriter As New JsonTextWriter(sw)                jsonWriter.Formatting = Formatting.None                jsonWriter.WriteStartArray()                If Not csv.Read() Then                    jsonWriter.WriteEndArray()                    Return sb.ToString()                End If                csv.ReadHeader()                ' Extracción defensiva                While csv.Read()                    Dim rowValues As New List(Of String)(jsonProperties.Count)                    For i As Integer = 0 To jsonProperties.Count - 1                        Dim v As String = Nothing                        Dim ok = csv.TryGetField(Of String)(i, v)                        rowValues.Add(If(ok, v, Nothing))                    Next                    If rowValues.Any(Function(f) Not String.IsNullOrWhiteSpace(f)) Then                        jsonWriter.WriteStartObject()                        For i As Integer = 0 To jsonProperties.Count - 1                            jsonWriter.WritePropertyName(jsonProperties(i))                            jsonWriter.WriteValue(rowValues(i))                        Next                        jsonWriter.WriteEndObject()                    End If                End While                jsonWriter.WriteEndArray()            End Using        End Using    End UsingCatch ex As FileNotFoundException    _logger?.LogWarning(ex, "CSV no encontrado: {path}", fullPath)    ThrowCatch ex As UnauthorizedAccessException    _logger?.LogError(ex, "Permisos insuficientes: {path}", fullPath)    ThrowCatch ex As IOException    _logger?.LogError(ex, "Error E/S leyendo CSV: {path}", fullPath)    ThrowCatch ex As HeaderValidationException    _logger?.LogWarning(ex, "Encabezados CSV inválidos: {path}", fullPath)    Throw New FormatException("Encabezados CSV inválidos.", ex)Catch ex As ReaderException    _logger?.LogError(ex, "Contenido CSV mal formado: {path}", fullPath)    Throw New FormatException("Contenido CSV inválido.", ex)Catch ex As JsonWriterException    _logger?.LogError(ex, "Error al generar JSON: {path}", fullPath)    ThrowCatch ex As Exception    _logger?.LogError(ex, "Error inesperado procesando CSV: {path}", fullPath)    ThrowEnd Try``Show more lines
4.2. Acceso a base de datos (Npgsql) – lectura
Antes (riesgoso):
VisualBasicUsing conn As New NpgsqlConnection(NpgSQL)    Using cmd As New NpgsqlCommand(Sql, conn)        Using adapter As New NpgsqlDataAdapter(cmd)            adapter.Fill(dt)        End Using    End UsingEnd UsingShow more lines
Después (parametrizado + manejo):
VisualBasicDim baseQuery = "SELECT idventa, monto, fecha, idcliente FROM ventas WHERE monto BETWEEN @mMin AND @mMax"Dim dt As New DataTable()Try    Using conn As New NpgsqlConnection(NpgSQL)        Using cmd As New NpgsqlCommand(baseQuery, conn)            cmd.Parameters.AddWithValue("@mMin", montoMin)            cmd.Parameters.AddWithValue("@mMax", montoMax)            cmd.CommandTimeout = 30            Using adapter As New NpgsqlDataAdapter(cmd)                adapter.Fill(dt)            End Using        End Using    End Using    Return dtCatch ex As Npgsql.NpgsqlException    _logger?.LogError(ex, "Error SQL en lectura.")    ThrowCatch ex As TimeoutException    _logger?.LogError(ex, "Timeout en DB.")    ThrowCatch ex As Exception    _logger?.LogError(ex, "Error inesperado en GetDataTablePostgre.")    ThrowEnd TryShow more lines

Nota: La parametrización evita SQL Injection y el Try/Catch garantiza observabilidad y consistencia de respuestas.


5. Mapeo de excepciones → códigos HTTP (controladores)
Centralizar el manejo en controladores/filtros permite respuestas correctas:
VisualBasicTry    Dim json = GetJSONFromCSV(model.ArchivoRuta)    Return Content(json, "application/json")Catch ex As FileNotFoundException    Return New HttpStatusCodeResult(404, "Archivo no encontrado.")Catch ex As UnauthorizedAccessException    Return New HttpStatusCodeResult(403, "Permisos insuficientes.")Catch ex As FormatException    Return New HttpStatusCodeResult(422, "El archivo está mal formado.")CatchShow more lines
Opcionalmente, agrega un filtro global para excepciones no manejadas:
VisualBasicPublic Class GlobalExceptionFilter    Inherits HandleErrorAttribute    Public Overrides Sub OnException(ctx As ExceptionContext)        If ctx.ExceptionHandled Then Return        _logger?.LogError(ctx.Exception, "Excepción no manejada. Controller={0}, Action={1}, Usuario={2}",            ctx.RouteData.Values("controller"), ctx.RouteData.Values("action"), ctx.HttpContext?.User?.Identity?.Name)        ctx.HttpContext.Response.StatusCode = 500        ctx.Result = New JsonResult() With {            .Data = New With {.ok = False, .error = "Error interno del servidor."},            .JsonRequestBehavior = JsonRequestBehavior.AllowGet        }        ctx.ExceptionHandled = True    End SubEnd ClassShow more lines

6. Buenas prácticas complementarias

Validación temprana: antes de abrir archivos o ejecutar consultas (existencia, permisos, formatos).
No “tragar” excepciones: evitar Catch ex As Exception vacío; siempre log y propagar/mapear.
No filtrar detalles técnicos al cliente: mensajes genéricos; detalles solo en logs internos.
Liberación de recursos: Using para streams, conexiones, writers.
Tiempos de espera: configurar CommandTimeout en DB y timeouts razonables en I/O/red.
Estandarizar logging: usar un logger (NLog/log4net/Microsoft.Extensions.Logging) con placeholders:
VisualBasic_logger?.LogError(ex, "Error leyendo archivo {ruta}, Usuario={usuario}", fullPath, User?.Identity?.Name)Show more lines

Pruebas: unitarias e integraciones para rutas de error (archivo inexistente, CSV corrupto, permisos insuficientes).


7. Pruebas recomendadas

Archivo inexistente → esperar FileNotFoundException → 404.
Permisos insuficientes → UnauthorizedAccessException → 403/500.
CSV con menos columnas → sin excepción; filas vacías se filtran.
Encabezados inválidos → FormatException → 422.
DB caída / timeout → NpgsqlException / TimeoutException → 500.
Verificar logs: entradas con contexto y sin secretos.


8. Checklist para Pull Requests (cerrar hallazgos similares)

 Validación previa de inputs (null/empty, rangos, existencia).
 Try/Catch alrededor de operaciones riesgosas.
 Capturas específicas antes de la genérica.
 Logging estructurado con contexto, sin datos sensibles.
 Using/Finally para recursos.
 Mapeo a HTTP status coherentes.
 Documentar cambios en CHANGELOG/README.
 Pruebas para rutas de error.


9. Ejemplo de Diff estilo PR
Diff--- a/ICMTools/Controllers/ImportVentasController.vb+++ b/ICMTools/Controllers/ImportVentasController.vb@@ -450,12 +450,44 @@-    Using stream = System.IO.File.OpenRead(model.ArchivoRuta)-        Dim result = _servicioImportacion.Procesar(stream, model.Opiones)-        Return Json(New With {.ok = True, .importId = result.Id})-    End Using+    If model Is Nothing OrElse String.IsNullOrWhiteSpace(model.ArchivoRuta) Then+        Return New HttpStatusCodeResult(400, "Parámetros inválidos.")+    End If+    Try+        If Not System.IO.File.Exists(model.ArchivoRuta) Then+            Return New HttpStatusCodeResult(404, "Archivo no encontrado.")+        End If+        Using stream = System.IO.File.Open(model.ArchivoRuta, IO.FileMode.Open, IO.FileAccess.Read, IO.FileShare.Read)+            Dim result = _servicioImportacion.Procesar(stream, model.Opiones)+            Return Json(New With {.ok = True, .importId = result.Id})+        End Using+    Catch ex As FormatException+        _logger?.LogWarning(ex, "Formato inválido en {ruta}", model.ArchivoRuta)+        Response.StatusCode = 422+        Return Json(New With {.ok = False, .error = "Formato inválido."})+    Catch ex As IO.FileNotFoundException+        _logger?.LogWarning(ex, "Archivo no encontrado {ruta}", model.ArchivoRuta)+        Response.StatusCode = 404+        Return Json(New With {.ok = False, .error = "Archivo no encontrado."})+    Catch ex As Exception+        _logger?.LogError(ex, "Error inesperado importando {ruta}", model.ArchivoRuta)+        Response.StatusCode = 500+        Return Json(New With {.ok = False, .error = "Error interno del servidor."})+    End TryShow more lines

10. Conclusión
El manejo correcto de excepciones con Try/Catch no solo cierra el hallazgo de Checkmarx One, sino que mejora la seguridad, resiliencia y observabilidad de la aplicación. La clave es combinar Try/Catch con validaciones, liberación de recursos, logging y respuestas HTTP apropiadas.
