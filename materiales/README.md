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

Los notebooks del repo (`00`, `01`, `02`, `03`) **piden un CV real** para
correr — no traen ningún CV de ejemplo embebido. Según dónde corras:

- **En Google Colab** (lo recomendado, ver los badges del README principal):
  la celda de carga abre el selector de archivos del navegador
  (`google.colab.files.upload()`) — subís el PDF directamente desde tu
  computadora a la sesión. Colab no tiene acceso a esta carpeta local, así
  que `materiales/` no aplica ahí.
- **Corriendo localmente** (`jupyter notebook`, fuera de Colab): la misma
  celda detecta que no está en Colab y busca automáticamente un PDF en
  `materiales/cvs/`. Copiá tu CV real ahí (cualquier nombre, extensión
  `.pdf`) antes de correr esa celda.

En ambos casos el archivo **nunca se guarda en el repositorio** — en Colab
vive solo en la sesión; localmente, `materiales/` está en `.gitignore`.

## Origen de los materiales reales del proyecto

Los CVs y rúbricas reales del laboratorio de Terry & Valdez viven fuera de
este repositorio, en el material de trabajo de la tesis (no público). Si
tenés acceso a ese material, copialo acá tal cual.
