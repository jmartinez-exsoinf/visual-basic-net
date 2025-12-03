# ✅ Pasar el objeto de conexión como parámetro puede ser vulnerable
Si el método recibe la conexión desde fuera, el código que lo llama puede alterarla antes o después de ejecutarse, afectando seguridad y estabilidad. Aquí aprenderás **por qué ocurre y cómo evitarlo**.

## ¿Qué hace el código?
```VisualBasic
Private Sub FinalizarProceso(conn As NpgsqlConnection)
    Using cmd As New NpgsqlCommand("SELECT public.spFemcoVsImportPorcentajeVentaCategorias_Finalizar()", conn)
        cmd.ExecuteNonQuery()
    End Using
End Sub
```

Este método recibe un objeto ```NpgsqlConnection``` desde fuera y lo usa para ejecutar un comando SQL.

## ¿Por qué es vulnerable?

### Exposición del objeto de conexión
Si el método recibe la conexión desde el exterior, cualquier código que llame a este método puede manipular el objeto `conn` antes o después de la ejecución.

Esto abre la puerta a:
- Cambio de estado (cerrar la conexión antes de tiempo).
- Reutilización indebida (usar la misma conexión para ejecutar comandos maliciosos).
- Inyección indirecta (si el método no controla la cadena de conexión, alguien podría pasar una conexión a otra base de datos).

### Falta de control sobre el ciclo de vida
El método no garantiza que la conexión esté en el estado correcto (abierta, segura).

Si la conexión se comparte entre hilos, puede causar errores o condiciones de carrera.

### Riesgo de escalamiento
Si el objeto de conexión se expone en capas superiores (por ejemplo, desde la API), un atacante podría interceptarlo y usarlo para ejecutar consultas arbitrarias.

## ✅ Buenas prácticas
✔ No pasar el objeto de conexión como parámetro en métodos que ejecutan lógica sensible.

✔ En su lugar:
- Crear la conexión dentro del método.
- Usar Using para garantizar cierre seguro.
- Tomar la cadena de conexión desde una fuente segura (configuración, no desde el cliente).

## ✅ Versión segura del código
```VisualBasic
Private Sub FinalizarProceso()
    Dim connectionString As String = ConfigurationManager.ConnectionStrings("MiConexion").ConnectionString
    Using conn As New NpgsqlConnection(connectionString)
        conn.Open()
        Using cmd As New NpgsqlCommand("SELECT public.spFemcoVsImportPorcentajeVentaCategorias_Finalizar()", conn)
            cmd.ExecuteNonQuery()
        End Using
    End Using
End Sub
```
✔ La conexión se crea y se destruye dentro del método.

✔ No se expone el objeto `conn` a capas externas.

✔ Se evita que el cliente manipule la conexión.

## ✅ ¿Por qué esto mejora la seguridad?
El ciclo de vida de la conexión está controlado.

No hay riesgo de que alguien pase una conexión maliciosa.

Se reduce la superficie de ataque para inyección indirecta.
