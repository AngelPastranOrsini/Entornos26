# Informe Técnico – Incidencia en procesamiento de formulario

##Resumen Ejecutivo

Se ha identificado una falla en el flujo de procesamiento del formulario en la aplicación BuggyWebApp. El sistema permite continuar con el procesamiento del formulario incluso cuando los datos introducidos son inválidos o están vacíos.

## Análisis del Problema

###Contexto:
El módulo de servicios procesa un formulario de contacto que contiene los campos name y email.

###Comportamiento esperado:
Si el correo electrónico no cumple con el formato válido, el proceso debería detenerse inmediatamente.

###Comportamiento observado:
El sistema ejecuta una validación del email mediante Validator.validateEmail(), pero el resultado de esta validación no controla el flujo del programa. Como consecuencia, el sistema continúa procesando el formulario aunque el email sea inválido o esté vacío.

##Diagnóstico Técnico y Reproducción

Para identificar el problema se utilizó el debugger del IDE.

Ubicación del código analizado:

src/com/example/service/ContactService.java

Pasos para reproducir el error:

Ejecutar la aplicación en modo Debug.

Colocar un breakpoint en la línea:

if (Validator.validateEmail(form.getEmail()))

Ejecutar la aplicación con un formulario vacío.

Analizar las variables durante la ejecución.

Evidencia obtenida en el debugger:

form.getName()  = ""
form.getEmail() = ""

Tras ejecutar la línea con Step Over, se observa que el programa continúa ejecutando el resto del método processForm().

##Causa Raíz (Root Cause)

La causa del problema es que la validación del email no controla el flujo de ejecución del programa. Aunque Validator.validateEmail() devuelve false, el método processForm() continúa ejecutándose.

##Propuesta de Solución

Se propone modificar la lógica de validación para detener el flujo del programa cuando el email no sea válido.

##Ejemplo de corrección:

if (!Validator.validateEmail(form.getEmail())) {
Logger.log("Error: Email inválido");
return;
}

De esta forma se evita que el sistema continúe procesando formularios con datos inválidos.