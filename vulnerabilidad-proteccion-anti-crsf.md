## 🛡️ Guía de Implementación: Protección Anti-CSRF

**Objetivo:** Corregir la vulnerabilidad "CSRF / Attack Vector" detectada por Checkmarx en controladores Web API mediante el uso de Tokens de Verificación.

### 1. Generación del Token (Capa de Vista - `.aspx`)

Para que el servidor pueda validar una petición, primero debe emitir un token único y cifrado que se entrega al cliente legítimo.

-   **Cambio:** Se agrega el helper de ASP.NET dentro del formulario principal.
    
-   **Código:**
    
    Fragmento de código
    
    ```
    <%-- Genera un input oculto con el nombre __RequestVerificationToken --%>
    <%: System.Web.Helpers.AntiForgery.GetHtml() %>
    
    ```
    

### 2. Envío del Token (Capa de Cliente - `jQuery/AJAX`)

Las cookies por sí solas son vulnerables a CSRF (el navegador las envía automáticamente). Para mitigar esto, extraemos el valor del token y lo enviamos en un **Header personalizado**. Los atacantes externos no pueden leer encabezados de otros dominios.

-   **Cambio:** Modificación de la llamada `$.ajax` para incluir el encabezado `X-XSRF-Token`.
    
-   **Código:**
    
    JavaScript
    
    ```
    var token = $('input[name="__RequestVerificationToken"]').val();
    
    $.ajax({
        url: '/api/clasificaciones/InsertInfoBD',
        type: 'POST',
        contentType: 'application/json; charset=utf-8',
        data: JSON.stringify(miObjetoData),
        headers: {
            'X-XSRF-Token': token // Nombre del encabezado que validará el servidor
        },
        success: function(res) { /* ... */ }
    });
    
    ```
    

### 3. Validación en el Servidor (Capa de Controlador - `VB.NET`)

El servidor ahora intercepta la petición y compara dos cosas: el token que viene en la **Cookie** (automático) contra el token que viene en el **Header** (manual). Si no coinciden, la petición se rechaza.

-   **Cambio:** Implementación de lógica de validación manual dentro del método del controlador.
    
-   **Código clave:**
    
    VB.Net
    
    ```
                ' --- VALIDACIÓN ANTI-CSRF OBLIGATORIA PARA CHECKMARX ---
                ' Extraer el token del Header personalizado
                Dim headers = Request.Headers
                If headers.Contains("X-XSRF-Token") Then
                    Dim formToken As String = headers.GetValues("X-XSRF-Token").FirstOrDefault()

                    ' Obtener la cookie de sesión para la comparativa
                    Dim cookie = HttpContext.Current.Request.Cookies("__RequestVerificationToken")
                    Dim cookieToken As String = If(cookie IsNot Nothing, cookie.Value, Nothing)

                    ' Validación cruzada (Si falla, lanza una excepción)
                    System.Web.Helpers.AntiForgery.Validate(cookieToken, formToken)
                Else
                    Return BadRequest("Falta Token de Seguridad")
                End If
                ' --- FIN VALIDACIÓN ---
```

----------

### ✅ Beneficios obtenidos

1.  **Cumplimiento (Compliance):** Checkmarx marcará el hallazgo como **Resuelto**, ya que ahora existe un factor de validación que el atacante no puede replicar.
    
2.  **Seguridad:** Se previene que sitios maliciosos realicen acciones (Insert/Update) en nombre de tus usuarios autenticados.
    
3.  **Compatibilidad:** Funciona correctamente en el ecosistema híbrido de ASP.NET Framework (Web Forms + Web API).
