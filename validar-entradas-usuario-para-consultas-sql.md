# ✅ Validar la entrada del usuario antes de usarla en consultas **SQL (VB.NET)**
La validación de datos es crítica para evitar vulnerabilidades como **SQL Injection** y errores en la aplicación. Nunca debes usar datos sin verificar directamente en consultas **SQL**.

🔍 ¿Por qué es importante?

Evita ataques: Un atacante puede enviar código malicioso en lugar de datos.
Previene errores: Datos inesperados pueden romper la lógica de la aplicación.
Cumple con estándares: OWASP recomienda validar y sanitizar toda entrada.


✅ Buenas prácticas para validar datos
1. Usar consultas parametrizadas
Nunca concatenes variables en la consulta:
VisualBasic' ❌ VulnerableDim sql As String = "SELECT * FROM usuarios WHERE nombre = '" & nombreUsuario & "'"Mostrar más líneas
En su lugar:
VisualBasic' ✅ SeguroUsing cmd As New NpgsqlCommand("SELECT * FROM usuarios WHERE nombre = @nombre", conn)    cmd.Parameters.AddWithValue("@nombre", nombreUsuario)End UsingMostrar más líneas

2. Validar tipo y formato
Antes de usar el dato:
VisualBasicIf String.IsNullOrWhiteSpace(nombreUsuario) Then    Throw New ArgumentException("El nombre no puede estar vacío.")End IfMostrar más líneas
Para números:
VisualBasicIf Not Integer.TryParse(idUsuario, Nothing) Then    Throw New ArgumentException("ID inválido.")End IfMostrar más líneas

3. Escapar caracteres especiales (si aplica)
Si por alguna razón no puedes usar parámetros (por ejemplo, en consultas dinámicas complejas), usa funciones de escape del proveedor (aunque lo ideal es parametrizar).

4. Centralizar la validación
Crea una función para validar entradas:
VisualBasicPrivate Function ValidarTexto(input As String) As String    If String.IsNullOrWhiteSpace(input) Then        Throw New ArgumentException("Valor inválido.")    End If    Return input.Trim()End FunctionMostrar más líneas

✅ Ejemplo completo en un Controller
VisualBasicPublic Async Function BuscarUsuario(nombreUsuario As String) As Task(Of IActionResult)    nombreUsuario = ValidarTexto(nombreUsuario)    Using conn As New NpgsqlConnection(connectionString)        Await conn.OpenAsync()        Using cmd As New NpgsqlCommand("SELECT * FROM usuarios WHERE nombre = @nombre", conn)            cmd.Parameters.AddWithValue("@nombre", nombreUsuario)            Using reader As NpgsqlDataReader = Await cmd.ExecuteReaderAsync()                ' Procesar resultados            End Using        End Using    End Using    Return Ok()End Function
