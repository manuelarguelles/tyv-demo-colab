# TYV · Sistema de filtrado curricular — demo reproducible

Material de apoyo educativo para la sustentación de una tesis de maestría:
una implementación **reproducible y 100% documentada** del sistema de
filtrado curricular de Terry & Valdez, ejecutable directamente en
**Google Colab**, sin instalar nada localmente.

El sistema recibe un currículum (PDF), lo protege, lo evalúa contra la
**rúbrica real del proyecto** (perfil AL · Asistente Legal) mediante 7
consultas **independientes** a un modelo de lenguaje, y produce una
clasificación auditable — sin decidir nunca por sí solo quién pasa a
entrevista.

**Estos notebooks trabajan con material real, no ficticio**: cada uno te
pide **subir tu propio PDF** (cualquier CV real que quieras probar) y usan
la rúbrica que efectivamente usa el sistema en producción — así el flujo
que ves es el flujo real, no una recreación con datos inventados.

> ⚠️ El PDF que subas y el nombre que ingreses quedan **solo en la memoria
> de tu sesión de Colab** — nunca se guardan en este repositorio ni se
> suben a GitHub. Si activás el modo real (ver más abajo), solo el texto
> **ya anonimizado** sale hacia la API del modelo. Ver
> [`materiales/README.md`](materiales/README.md) para trabajar con
> archivos reales guardados localmente en disco.

> ℹ️ **Nota de honestidad sobre la anonimización.** El módulo de
> `02_anonimizacion.ipynb` es código real — el mismo que existe, probado,
> en el laboratorio del proyecto — y acá corre activo sobre lo que subas.
> Pero la corrida que produjo los resultados numéricos reportados en el
> capítulo IV de la tesis (181 CVs) **no pasó por esta etapa**: por motivos
> de velocidad, esa evaluación se hizo enviando el texto extraído sin
> redactar. Este repositorio muestra el pipeline **tal como está
> diseñado para funcionar**, no reproduce esa corrida específica.

## Las 4 etapas del pipeline

| # | Etapa | Qué hace | Notebook |
|---|-------|----------|----------|
| 1 | **Extracción de texto** | PDF → texto plano, con Poppler (`pdftotext -layout`) | [`01_extraccion_texto.ipynb`](notebooks/01_extraccion_texto.ipynb) |
| 2 | **Anonimización** | Retira nombre/correo/teléfono/DNI/dirección con expresiones regulares — **determinístico, sin IA** | [`02_anonimizacion.ipynb`](notebooks/02_anonimizacion.ipynb) |
| 3 | **Siete consultas independientes** | Evalúa cada criterio de la rúbrica por separado contra el modelo (DeepSeek), valida el JSON de respuesta con Pydantic y verifica que la cita exista literalmente en el documento | [`03_siete_consultas_llm.ipynb`](notebooks/03_siete_consultas_llm.ipynb) |
| 4 | **Agregación y clasificación** | Combina las 7 notas en un subtotal sobre 60 y clasifica: no apto / reserva / apto entrevista | [`04_agregacion_clasificacion.ipynb`](notebooks/04_agregacion_clasificacion.ipynb) |

## Notebook completo (todo en uno)

[`00_pipeline_completo.ipynb`](notebooks/00_pipeline_completo.ipynb) encadena
las 4 etapas de punta a punta en un solo recorrido — ideal para una
demostración corrida o para ver el flujo completo antes de entrar al
detalle de cada notebook individual.

<a href="https://colab.research.google.com/github/manuelarguelles/tyv-demo-colab/blob/main/notebooks/00_pipeline_completo.ipynb" target="_parent"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Abrir el pipeline completo en Colab"/></a>

## Cómo elegir por dónde empezar

- **¿Querés ver el flujo entero rápido?** → `00_pipeline_completo.ipynb`.
- **¿Te interesa un paso específico en detalle** (por qué `-layout`, por qué
  el orden de los patrones de anonimización, cómo se valida el esquema del
  modelo, cómo se calcula el subtotal)? → abrí el notebook numerado de esa
  etapa. Cada uno es **independiente** — corre solo, sin depender de los
  demás.

## Modo real vs. modo simulado (etapa 3)

El notebook de las siete consultas (`03` y la Etapa 3 de `00`) funciona en
dos modos:

- **Simulado** (por defecto, sin credenciales): cada una de las 7 consultas
  responde honestamente "sin evidencia" (`null`) — sin una clave real no
  hay forma de simular una lectura genuina de tu CV, así que el notebook
  no inventa un resultado; solo te deja ver la mecánica de las 7 llamadas
  independientes.
- **Real**: si definís un secreto `DEEPSEEK_API_KEY` en Colab (ícono de
  llave 🔑 en la barra lateral izquierda) o la variable de entorno del mismo
  nombre, el notebook llama a la API real de DeepSeek
  (`deepseek-v4-flash`, compatible con el SDK de OpenAI) y el modelo lee tu
  CV real, criterio por criterio.

Ninguna clave se pide ni se lee del código — nunca la escribas directamente
en un notebook.

## Materiales reales (CVs, rúbricas propias)

La carpeta [`materiales/`](materiales/) está pensada para trabajar con
archivos reales de forma **local** — está en `.gitignore` (salvo su propio
`README.md`), así que nada de lo que pongas ahí se sube a este repositorio
público. Ver [`materiales/README.md`](materiales/README.md) para el detalle.

## Correr localmente (alternativa a Colab)

```bash
python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
# Poppler (para 01_extraccion_texto.ipynb y 00):
#   macOS:  brew install poppler
#   Ubuntu: sudo apt-get install poppler-utils
jupyter notebook notebooks/
```

## Referencia — la fórmula de agregación (perfil AL · Asistente Legal)

| Dimensión    | Criterios              | Peso |
|--------------|-------------------------|------|
| Formación    | AL_01, AL_02, AL_03     | 20 % |
| Experiencia  | AL_04, AL_05            | 25 % |
| Técnico      | AL_06, AL_07            | 15 % |

Subtotal máximo: **60 puntos**. Clasificación: **< 60 %** no apto ·
**60–80 %** reserva · **≥ 80 %** apto para entrevista. La entrevista
personal (hasta 40 puntos adicionales) queda fuera de este pipeline.

## Licencia y alcance

Este repositorio es material de apoyo educativo para una sustentación de
tesis de maestría. No es un producto, no toma decisiones de contratación
por sí solo, y no reemplaza el criterio humano de la etapa de entrevista.
