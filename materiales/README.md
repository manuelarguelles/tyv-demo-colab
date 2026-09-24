# Materiales (solo local — nunca se sube a GitHub)

Esta carpeta es el lugar para trabajar con **archivos reales** al correr los
notebooks en tu máquina (no en Colab): CVs de candidatos, rúbricas propias,
matrices de resultados, etc.

**Todo lo que pongas acá adentro está en `.gitignore`** (salvo este mismo
`README.md`) — el repositorio público **nunca** sube estos archivos. Podés
copiar y pegar con confianza.

## Estructura sugerida

```
materiales/
├── README.md          (este archivo — sí se versiona)
├── cvs/                (pegá acá los PDF/DOCX de CVs reales)
└── rubricas/           (pegá acá las rúbricas propias, en el formato del proyecto)
```

## Cómo usarlos desde un notebook

Los notebooks del repo (`00`–`04`) usan por defecto un **CV ficticio
embebido en el propio notebook**, así que corren sin depender de esta
carpeta. Si querés probarlos con un archivo real, en Google Colab subilo
directamente al entorno de ejecución (ícono de carpeta 📁 en la barra
lateral) — Colab no tiene acceso a tu disco local, así que esta carpeta
`materiales/` solo aplica si corrés los notebooks **localmente** con Jupyter
(`pip install jupyter && jupyter notebook`), apuntando la ruta del PDF a
`materiales/cvs/<archivo>.pdf` en vez de `cv_ejemplo.pdf`.

## Origen de los materiales reales del proyecto

Los CVs y rúbricas reales del laboratorio de Terry & Valdez viven fuera de
este repositorio, en el material de trabajo de la tesis (no público). Si
tenés acceso a ese material, copialo acá tal cual.
