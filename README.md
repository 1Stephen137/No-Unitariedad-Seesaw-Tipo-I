# NonUnitaryLimits

Repositorio reproducible para recopilar, trazar y documentar límites y proyecciones sobre la no unitariedad leptónica, empezando por el sector μ–e.

> **Estado:** MVP μ–e en desarrollo. Incluye límites escalares curados, sensibilidades de tasa separadas y una figura dependiente de masa con bandas vectoriales digitalizadas; las proyecciones reinterpretadas se etiquetan como recasts o aproximaciones ilustrativas.

## Convención de referencia

Usamos como convención interna

\[
N=(I-\eta)U_{\rm PMNS},\qquad \eta=\eta^\dagger.
\]

En el límite de mezcla pequeña con estados neutrales pesados,

\[
\eta\simeq \tfrac12\Theta\Theta^\dagger,
\]

por lo que η debe ser semidefinida positiva en ese supuesto. Se conserva también la parametrización original de cada resultado en sus metadatos; una conversión desde la parametrización triangular α se hará solo cuando el artículo y sus aproximaciones permitan definirla sin ambigüedad.

## Tipos de contenido

- `observed`: límite o medida publicada.
- `projected`: sensibilidad futura publicada por una colaboración o un estudio.
- `recast`: reinterpretación hecha por este repositorio, claramente identificada y enlazada al método/código.

Nunca se fusionan estos tres estados en una única curva sin diferenciarlos visualmente. La no unitariedad efectiva y las búsquedas de HNL con masa explícita se mantienen como categorías de análisis distintas.

## Estructura

```text
src/nonunitarity_limits/   convenciones, validación, lectura y plotting
 data/templates/           esquemas vacíos; no son datos físicos
 tests/                    pruebas unitarias
 docs/                      convenciones, protocolo de curación y roadmap
```

## Instalación de desarrollo

```bash
python -m pip install -e '.[dev]'
pytest
```

## Añadir resultados

1. Añadir metadatos en `data/constraints.csv` (copiar el encabezado de `data/templates/constraints.csv`).
2. Para curvas, añadir los puntos en `data/curve_points.csv`; cada curva requiere un `constraint_id` existente.
3. Incluir URL/DOI/versión, observable, elemento de η, método de inferencia, CL, supuestos (un HNL/varios HNL; tipo Dirac/Majorana; orden EFT), rango de validez y si la curva es observada, proyectada o recast.
4. Validar las fuentes y la compatibilidad de los supuestos antes de graficar. No conectar puntos a través de huecos de cobertura.
5. Ejecutar las pruebas y reconstruir las figuras con el CLI.

## CLI (cuando haya datos)

```bash
nonunitarity-plot --data data --parameter eta_mue --output figures/eta_mue.pdf
```

## Principios

- datos publicados y proyecciones siempre separados;
- ninguna curva sin fuente y metadatos de interpretación;
- respetar los intervalos de masa válidos y los huecos;
- no combinar likelihoods sin correlaciones o procedimiento estadístico documentado;
- guardar junto a cada figura el CSV y la configuración usados para generarla.

Ver `docs/roadmap.md` y `docs/data_policy.md` antes de contribuir.

## Primer conjunto curado: |η_eμ|

`data/eta_mue_bounds.csv` contiene una primera extracción documentada para el sector μ–e: el ajuste global de Naredo-Tuero (95% CL) en cinco benchmarks y un recast radiativo de MEG II bajo el límite de HNL pesado. `data/rate_sensitivities.csv` conserva por separado las sensibilidades de Mu3e, COMET y Mu2e como tasas, sin traducirlas a una cota constante de η.

Para regenerar la figura categórica:

```bash
python -m pip install -e .
nonunitarity-bounds-plot --input data/eta_mue_bounds.csv --output figures/eta_mue_bounds.png
```

(También se puede ejecutar `python -m nonunitarity_limits.plot_bounds --input data/eta_mue_bounds.csv --output figures/eta_mue_bounds.png`.) Para graficar tasas cLFV sin reinterpretarlas como η, usar `nonunitarity-rates-plot --input data/rate_sensitivities.csv --output figures/mue_rates.png`. Ver `docs/eta_mue_first_dataset.md` para fuentes y supuestos. No se combinan los benchmarks de ajuste entre sí.

## Curvas μ–e dependientes de masa

`figures/eta_mue_mass_dependent.png` compara las bandas 2N-SS/orden normal de μ→3e y conversión en Au de Blennow et al. (2023), un recast masa-dependiente de MEG II, su objetivo de diseño, y proyecciones Mu3e escaladas a orden líder e identificadas como ilustrativas. El resultado de ajuste global se muestra aparte, sin combinarlo con las bandas de escaneo. No se mapea la sensibilidad de Al a η. Datos y método reproducible están en `data/curves/`, `scripts/digitize_blennow_fig1.py` y `docs/mass_dependent_mue.md`.

```bash
nonunitarity-mass-plot --input data/curves/eta_mue_cLFV_blennow2023_digitized.csv --output figures/eta_mue_mass_dependent.png
```

Para regenerar el CSV a partir del PDF vectorial de arXiv, instalar `.[digitize]` y seguir la guía de `docs/mass_dependent_mue.md`.
