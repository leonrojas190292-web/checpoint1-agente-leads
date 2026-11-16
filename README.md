# Checkpoint 1 – Agente Base y Motor de Razonamiento

**Alumno:** Leo Rojas
**Rol del agente:** Asistente de Calificación de Leads
**Archivo:** `checkpoint1_leo_rojas.json`

## Arquitectura

```
[Chat Trigger] → [AI Agent (Tools Agent) + System Prompt] → [Code: Formatear Log] → [Gmail: Reporte de observabilidad]
                        │                  │
              [Gemini Chat Model]   [Tool: Google Sheets – Registrar Lead]
```

## Cumplimiento de la rúbrica

| Criterio | Implementación |
|---|---|
| System Prompt claro | Modular: Rol → Ámbito → Objetivo → Reglas/Restricciones → Escalamiento. Sin lenguaje inclusivo. |
| Herramienta solo cuando corresponde | Google Sheets acoplada como *Tool* (conexión `ai_tool`), con descripción semántica extensa de cuándo usarla y cuándo NO. Campos completados por la IA con `$fromAI()`. |
| Límite de iteraciones | `Max Iterations = 7` (rango 5-10). |
| Registro / observabilidad | `Return Intermediate Steps = ON` + nodo Code que arma la traza + envío por Gmail. |

## Configuración previa a la prueba

1. Credenciales: Google Gemini (PaLM) API, Google Sheets OAuth2 y Gmail OAuth2.
2. Crear una hoja de cálculo con estos encabezados en la fila 1 de la primera pestaña:
   `Fecha | Session_ID | Nombre | Empresa | Email | Cargo | Necesidad | Presupuesto | Urgencia | Puntaje_BANT | Clasificacion | Justificacion`
3. En el nodo de Sheets reemplazar el ID de la planilla y en Gmail el destinatario por los propios.

## Prompts de prueba

- **Activa la herramienta:** "Hola, soy Martín Gómez, gerente de operaciones de Logística Sur SA (mgomez@logisticasur.com). Queremos automatizar la carga de pedidos, tenemos unos USD 8.000 de presupuesto y necesitamos arrancar este mes."
- **No debe activarla:** "Hola, mi factura de octubre vino mal, ¿me ayudan?"

## Validación realizada (16/11/2026)

| Prueba | Resultado |
|---|---|
| Lead comercial completo (Logística Sur SA) | Ejecución en verde. El agente activó la Tool por decisión propia, registró la fila en Sheets (BANT 100 · CALIENTE · ESCALAR A EJECUTIVO COMERCIAL) y se envió el reporte por Gmail. |
| Consulta de facturación (no comercial) | Ejecución en verde. El agente **no** activó la Tool, aplicó la regla de escalamiento y el log informó "Herramienta activada: NO". |

Modelo: Gemini 3.5 Flash (Gemini 2.5 Flash ya no está disponible para cuentas nuevas).
