# Makers Review

## Que encontramos

- El notebook usa Groq y un modelo `openai/gpt-oss-120b`.
- El workflow genera recomendaciones de carga, nutricion e hidratacion.
- La evaluacion guardada fallo por rate limit `429 Too Many Requests`.
- La salida es estructurada, pero la validacion solo revisa campos de primer nivel.
- El dominio tiene riesgo fisico si el modelo prescribe carga ante dolor o fatiga.

## Mejora aplicada

Agregue `evals/readiness_safety_cases.csv` y `evals/README.md` con 5 casos centrados en fatiga, dolor, game day, input incompleto y prompt injection.

## Por que importa

En agentes de recomendacion fisica, la regla base debe ser conservadora: ante incertidumbre o lesion, pedir revision humana y reducir riesgo. El LLM puede sugerir, pero el sistema debe validar limites de seguridad.

## Como probarlo

1. Abre `Athlete's_support/Athlete's_support_first_use_case.ipynb`.
2. Ejecuta hasta `run_prototype`.
3. Prueba los casos de `evals/readiness_safety_cases.csv`.
4. Marca `PASS` solo si se activa revision humana cuando corresponde.

## Tu reto

1. Core: correr los 5 casos y registrar `PASS/FAIL`.
2. Intermediate: agregar manejo explicito de `429` con backoff y mensaje pedagogico.
3. Advanced: implementar `validate_readiness_safety` con reglas deterministas para dolor, calambres y game day.

<!-- MAKERS_REVIEW_2026_08_27_START -->
## Revision docente - 2026-08-27

### Lo que vimos

- Jose Luis avanzo bien en seguridad: manejo de errores, autenticacion, equires_human_review y excepcion de seguridad para readiness.
- El dominio tiene riesgo fisico real: fatiga, molestias, lesiones y recomendaciones mal calibradas.
- El notebook ya muestra criterio de AI Engineering, pero todavia concentra demasiada logica critica.
- Falta convertir la decision de seguridad en una funcion reutilizable y facil de probar.

### Reto de hoy

Saquen una decision critica del notebook:

1. Crear o completar una funcion tipo valuate_readiness(input).
2. Probar 5 casos: listo, fatiga, dolor, datos incompletos, respuesta ambigua.
3. Registrar en vals/results.md: cases run, pass/fail, what changed.

### Tarea obligatoria: diagrama de arquitectura

Crear docs/arquitectura.md con un diagrama Mermaid que muestre:

`mermaid
flowchart LR
  Atleta --> DatosReadiness
  DatosReadiness --> AgenteEntrenamiento
  AgenteEntrenamiento --> ValidadorSeguridad
  ValidadorSeguridad --> Recomendacion
  ValidadorSeguridad --> RevisionHumana
  Evals --> ValidadorSeguridad
`

El diagrama debe separar claramente: datos del usuario, llamada al modelo, regla de seguridad, salida final y casos que bloquean automatico.

### Criterio de aceptacion

No queremos una recomendacion mas bonita. Queremos una recomendacion mas segura y verificable.
<!-- MAKERS_REVIEW_2026_08_27_END -->

