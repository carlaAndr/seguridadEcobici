# ECOBICI y percepción de seguridad en la Ciudad de México

### Proyecto de investigación | Análisis de datos e inferencia econométrica

**Carla Andrea Sosa Peralta** · Trabajo de titulación por caso, Licenciatura en Ciencia de Datos, ITAM

**[Ver el sitio del proyecto](https://carlaandr.github.io/seguridadEcobici/)** · **[Documento (PDF)](documento/seguridad_ecobici.pdf)** · **[Replicación con Docker](https://github.com/carlaAndr/dockerSeguridadEcobici)**

Este repositorio reúne las fuentes de información, los notebooks de procesamiento, los datos derivados y el análisis econométrico desarrollados para estudiar la **relación entre la percepción de inseguridad y el uso del sistema ECOBICI en la Ciudad de México**.

La investigación integra los registros de viajes de ECOBICI, la **Encuesta Nacional de Seguridad Pública Urbana (ENSU)** y los datos de **inversión pública** del INEGI en un panel de alcaldías observado trimestre a trimestre entre 2020 y 2026.

> **En una frase:** con un panel de seis alcaldías entre 2020 y 2026, no se encontró evidencia de una asociación entre la percepción de inseguridad y los viajes de ECOBICI (β₁ = 0.0064, IC 95 % [−0.014, 0.027]; no se rechaza H₀). Esto no demuestra que la asociación sea cero: el diseño solo permite descartar asociaciones grandes.

> **Alcance:** el estudio mide una **asociación**, no un efecto causal. Los efectos fijos controlan las diferencias permanentes entre alcaldías y los choques comunes a toda la ciudad, pero no los factores que cambian a la vez por alcaldía y por trimestre.

## Contenido

1. [Pregunta de investigación](#1-pregunta-de-investigación)
2. [Resultado principal](#2-resultado-principal)
3. [Estructura del repositorio](#3-estructura-del-repositorio)
4. [Para qué sirve cada archivo](#4-para-qué-sirve-cada-archivo)
5. [Fuentes de información](#5-fuentes-de-información)
6. [Notebooks y flujo de procesamiento](#6-notebooks-y-flujo-de-procesamiento)
7. [Diccionario de artefactos](#7-diccionario-de-artefactos)
8. [Estrategia econométrica y diagrama causal](#8-estrategia-econométrica-y-diagrama-causal)
9. [Cómo ejecutar el proyecto](#9-cómo-ejecutar-el-proyecto)
10. [Limitaciones](#10-limitaciones)
11. [Fuentes y referencias institucionales](#11-fuentes-y-referencias-institucionales)
12. [Uso de inteligencia artificial y transparencia metodológica](#12-uso-de-inteligencia-artificial-y-transparencia-metodológica)

## 1. Pregunta de investigación

**¿Se asocia la percepción de inseguridad de la población con el uso de ECOBICI en las alcaldías de la Ciudad de México?**

- **Hipótesis de investigación:** una mayor percepción de inseguridad se asocia con menos viajes, porque el temor a ser víctima de un delito puede llevar a evitar la calle.
- **Prueba estadística:** H₀: β₁ = 0 contra H₁: β₁ ≠ 0, a dos colas, con un nivel de significancia de 5 %. La prueba es bilateral para no descartar de antemano una asociación positiva.

### Alcance territorial

El análisis considera las seis alcaldías con estaciones de ECOBICI:

- Álvaro Obregón
- Azcapotzalco
- Benito Juárez
- Coyoacán
- Cuauhtémoc
- Miguel Hidalgo

La ENSU y los datos de inversión pública cubren las **16 alcaldías**; el panel conserva solo estas seis.

### Periodo de referencia

El panel va de **2020-T1 a 2026-T1** y **no está balanceado**: Benito Juárez, Cuauhtémoc y Miguel Hidalgo están desde 2020, mientras que Coyoacán, Álvaro Obregón y Azcapotzalco entran en 2023, conforme se expande ECOBICI. La inversión pública cubre **2020–2025**.

## 2. Resultado principal

El modelo principal usa el **total de viajes del ejercicio 1** (sin inversión pública). Los modelos por sexo y el ejercicio 2 son extensiones. Errores estándar agrupados por alcaldía; intervalos y p-valores con t de 5 grados de libertad.

| Modelo | β₁ | EE agrupado | IC 95 % | p | Obs. |
|---|---|---|---|---|---|
| **Total, ejercicio 1** | **0.0064** | **0.0079** | **[−0.0140, 0.0267]** | **0.457** | **105** |
| Total, ejercicio 2 | 0.0081 | 0.0094 | [−0.0161, 0.0323] | 0.431 | 99 |
| Mujeres, ejercicio 1 | 0.0064 | 0.0081 | [−0.0145, 0.0273] | 0.468 | 105 |
| Mujeres, ejercicio 2 | 0.0083 | 0.0097 | [−0.0166, 0.0332] | 0.431 | 99 |
| Hombres, ejercicio 1 | 0.0063 | 0.0078 | [−0.0138, 0.0264] | 0.455 | 105 |
| Hombres, ejercicio 2 | 0.0079 | 0.0093 | [−0.0160, 0.0318] | 0.433 | 99 |
| Mujeres − hombres, ejercicio 1 | 0.0000 | 0.0004 | [−0.0009, 0.0010] | 0.907 | 105 |

- **Lectura:** no se rechaza H₀ en ningún modelo. Diez puntos más de población que se siente insegura se asocian con unos **+6.6 %** más viajes, pero el intervalo va de **−13 % a +31 %** e incluye el cero.
- **Mujeres y hombres:** no hay diferencia entre ambos.
- **Robustez:** ningún intervalo excluye el cero al quitar una alcaldía a la vez, al omitir las estaciones, al usar log(estaciones), al conservar solo las alcaldías con panel completo o al quitar el trimestre de entrada de las alcaldías nuevas. El signo cambia entre variantes, así que no se interpreta.
- **Pocos grupos:** un wild cluster bootstrap con pesos de Webb lleva a la misma conclusión (p = 0.501).

Las tablas de robustez, las figuras y la discusión completa están en el [documento](documento/seguridad_ecobici.pdf) y en el [sitio del proyecto](https://carlaandr.github.io/seguridadEcobici/).

## 3. Estructura del repositorio

```
seguridadEcobici/
├── README.md
├── index.html                  Sitio del proyecto (GitHub Pages)
├── documento/
│   └── seguridad_ecobici.pdf   Documento del caso en formato IEEE
├── images/
│   └── diagrama_causal.png     Diagrama causal del modelo
├── PaneldeDatos.ipynb          Construcción del panel
├── Modelado.ipynb              Análisis exploratorio, modelos y figuras
├── dataIngestion/
│   ├── MapeoAlcaldias.ipynb
│   ├── LecturaEcobici.ipynb
│   ├── LecturaDatosENSU.ipynb
│   ├── LecturadeDatosInversionPublica.ipynb
│   └── MapasdeSeguridad.ipynb
├── ensu/                       Tabulados originales de la ENSU (.zip)
├── finanzasPublicas/           Exportaciones originales de EFIPEM (.xls)
└── artefactos/                 Datos derivados (sección 7)
```

La versión preparada para correr con Docker vive en un repositorio aparte: **[carlaAndr/dockerSeguridadEcobici](https://github.com/carlaAndr/dockerSeguridadEcobici)** (sección 9).

## 4. Para qué sirve cada archivo

### Archivos de la raíz

| Archivo | Para qué sirve |
|---|---|
| `README.md` | Este documento: explica el proyecto, cómo está organizado y cómo replicarlo. |
| `index.html` | Sitio interactivo del proyecto, publicado con GitHub Pages. Lee `artefactos/panel_ecobici.csv` y `artefactos/modelado.csv` para dibujar el mapa, las gráficas y las tablas de resultados. |
| `PaneldeDatos.ipynb` | Une las tres fuentes procesadas en un solo panel alcaldía × trimestre × sexo y genera `artefactos/panel_ecobici.csv`. |
| `Modelado.ipynb` | Toma el panel, hace el análisis exploratorio, estima los modelos de efectos fijos, la robustez y el bootstrap, y exporta tablas, figuras y `artefactos/modelado.csv`. |
| `documento/seguridad_ecobici.pdf` | Documento final del caso, con métodos, supuestos, resultados y referencias. |
| `images/diagrama_causal.png` | Diagrama causal que se muestra en la sección 8 de este README. |

### Carpetas

| Carpeta | Para qué sirve |
|---|---|
| `dataIngestion/` | Notebooks que leen y limpian cada fuente por separado: estaciones, viajes, ENSU, inversión pública y mapas. Sus salidas van a `artefactos/`. |
| `ensu/` | Paquetes originales de tabulados básicos de la ENSU tal como los publica el INEGI. De aquí se extrae el cuadro 1.7. |
| `finanzasPublicas/` | Archivos originales de la EFIPEM del INEGI, uno por año, de donde se toma la inversión pública por alcaldía. |
| `artefactos/` | Datos derivados del procesamiento: bases limpias de cada fuente, el panel y los resultados del modelo. No sustituyen a las fuentes originales. |

## 5. Fuentes de información

| Fuente | Información utilizada | Frecuencia | Función en el proyecto |
|---|---|---|---|
| **ECOBICI** (datos abiertos) | Viajes por estación de retiro y sexo; catálogo de estaciones (feed GBFS) | Mensual, agregada a trimestre | Variable dependiente (viajes) y control de oferta (estaciones activas) |
| **ENSU**, INEGI | Cuadro 1.7: población de 18 años y más que se siente segura o insegura viviendo en su ciudad, por alcaldía | Trimestral | Variable explicativa de interés |
| **EFIPEM**, INEGI | Inversión pública ejercida por alcaldía | Anual | Control en el ejercicio 2 |

### 5.1 ECOBICI

Se usan los registros históricos de viajes publicados en el portal de datos abiertos. El procesamiento homologa los formatos que cambian entre años y agrega los viajes por **año, trimestre, estación de retiro, alcaldía y sexo**. Cada estación se asigna a una alcaldía con sus coordenadas.

### 5.2 Encuesta Nacional de Seguridad Pública Urbana (ENSU)

`LecturaDatosENSU.ipynb` extrae el **cuadro 1.7** de cada paquete de tabulados básicos. La ENSU mide la percepción de quienes **viven** en la alcaldía, no de quienes usan ECOBICI, y no publica la percepción por sexo a nivel alcaldía.

### 5.3 Finanzas públicas (EFIPEM)

`LecturadeDatosInversionPublica.ipynb` limpia y consolida los montos anuales por alcaldía. La inversión pública es una categoría agregada de gasto y **no debe interpretarse como inversión en ciclovías, ECOBICI o movilidad ciclista**.

## 6. Notebooks y flujo de procesamiento

| Orden | Notebook | Qué hace | Genera |
|---|---|---|---|
| 1 | `dataIngestion/MapeoAlcaldias.ipynb` | Descarga las estaciones del feed GBFS y las asigna a una alcaldía por sus coordenadas | `catalogo_ecobici_estaciones_alcaldia.csv` |
| 2 | `dataIngestion/LecturaEcobici.ipynb` | Lee los archivos mensuales de viajes, homologa columnas entre años y agrega por trimestre, estación, alcaldía y sexo | `ecobici_resumen_viajes_trimestre_alcaldia.csv` |
| 3 | `dataIngestion/LecturaDatosENSU.ipynb` | Extrae el cuadro 1.7 de cada levantamiento de la ENSU | `percepcion_seguridad_alcaldias_cdmx.csv` |
| 4 | `dataIngestion/LecturadeDatosInversionPublica.ipynb` | Extrae la inversión pública por alcaldía y año | `inversion_publica_alcaldia.csv` |
| 5 | `PaneldeDatos.ipynb` | Cruza las tres fuentes, normaliza los nombres de alcaldía y documenta las observaciones descartadas | `panel_ecobici.csv` |
| 6 | `Modelado.ipynb` | Análisis exploratorio, modelos de efectos fijos, robustez, bootstrap y exportación para el sitio | Tablas, figuras y `modelado.csv` |
| 7 | `dataIngestion/MapasdeSeguridad.ipynb` | Mapas de percepción de inseguridad por alcaldía | Figuras |

### Flujo de datos

```
Viajes ECOBICI ──> LecturaEcobici ─────────────────┐
Feed GBFS ───────> MapeoAlcaldias ─────────────────┤
ENSU (INEGI) ────> LecturaDatosENSU ───────────────┼──> PaneldeDatos ──> panel_ecobici.csv
EFIPEM (INEGI) ──> LecturadeDatosInversionPublica ─┘                          │
                                                                              └──> Modelado
                                                                                   ├── Análisis exploratorio
                                                                                   ├── Modelos de efectos fijos y robustez
                                                                                   ├── Tablas y figuras del documento
                                                                                   └── modelado.csv ──> index.html (sitio)
```

### Observaciones descartadas al construir el panel

- **`SIN_CATALOGO`:** viajes de estaciones que no aparecen en el catálogo actual. No se asignan a ninguna alcaldía y quedan fuera del panel.
- **2020-T2:** el INEGI no levantó la ENSU en ese trimestre por la emergencia sanitaria de COVID-19.
- **2026-T2:** un cambio en las columnas del tabulado de junio 2026 hace que ese levantamiento se pierda al construir el panel (pendiente de corregir, sección 10).
- **Panel no balanceado:** las alcaldías se incorporan a ECOBICI en fechas distintas, así que no todas tienen todos los trimestres.

## 7. Diccionario de artefactos

Los archivos de `artefactos/` son **datos derivados del procesamiento**, no sustitutos de las fuentes originales.

| Archivo | Filas | Contenido |
|---|---|---|
| `catalogo_ecobici_estaciones_alcaldia.csv` | 677 | `station_id`, `name`, `short_name`, `alcaldia`. El cruce con los viajes se hace por `short_name`. |
| `ecobici_estaciones_por_alcaldia_resumen.csv` | 6 | `alcaldia`, `num_estaciones` |
| `ecobici_resumen_viajes_trimestre_alcaldia.csv` | 46,091 | `anio`, `trimestre`, `ciclo_estacion_retiro`, `alcaldia` (o `SIN_CATALOGO`), `genero` (`F`, `M`, `O`, `?`), `n_viajes`, `estaciones_activas_alcaldia` |
| `percepcion_seguridad_alcaldias_cdmx.csv` | 400 | 16 alcaldías × 25 levantamientos. `anio`, `mes`, `trimestre`, `region`, `alcaldia`, `poblacion_18mas`, `seguro_abs`, `seguro_pct`, `inseguro_abs`, `inseguro_pct` |
| `inversion_publica_alcaldia.csv` | 96 | 16 alcaldías × 2020–2025. `año`, `alcaldia`, `inversion_publica` |
| `panel_ecobici.csv` | 345 | Panel alcaldía × trimestre × sexo (ver tabla siguiente). Lo genera `PaneldeDatos.ipynb` y lo usan `Modelado.ipynb` y el sitio. |
| `modelado.csv` | 41 | Una fila por coeficiente de cada modelo: principales, diferencia mujeres − hombres y variantes de robustez, con EE, t, p, IC, N y cambio % por 10 puntos. Lo genera `Modelado.ipynb` y lo lee el sitio. |

### `panel_ecobici.csv`

| Campo | Descripción |
|---|---|
| `anio`, `trimestre` | Identificadores temporales |
| `alcaldia_norm` | Nombre de la alcaldía normalizado (mayúsculas, sin acentos) |
| `genero` | Sexo registrado por ECOBICI (`F`, `M`, `O`, `?`) |
| `n_viajes` | Viajes de la alcaldía, trimestre y sexo |
| `estaciones_activas` | Estaciones de la alcaldía con al menos un viaje en el trimestre |
| `inseguro_pct`, `seguro_pct` | % de población de 18 años y más que se siente insegura y segura (ENSU) |
| `inversion_publica` | Inversión pública anual de la alcaldía, repetida en los cuatro trimestres del año |
| `log_viajes` | log(1 + `n_viajes`) |
| `log_inversion` | log(`inversion_publica`) |

## 8. Estrategia econométrica y diagrama causal

![Diagrama causal del análisis](images/diagrama_causal.png)

*Diagrama causal del análisis. En gris, lo que cierra el modelo (efectos fijos y controles de oferta); en línea punteada, lo que no se mide y los caminos que quedan abiertos, por los que el resultado se lee como asociación y no como efecto.*

La unidad de observación es la **alcaldía–trimestre**. `Modelado.ipynb` estima regresiones de panel con **efectos fijos de alcaldía y de trimestre**.

**Ejercicio 1 (modelo principal):**

$$
\log(1+\mathrm{Viajes}_{it}) = \beta_1\,\mathrm{Inseguridad}_{it} + \gamma\,\mathrm{Estaciones}_{it} + \alpha_i + \lambda_t + \varepsilon_{it}
$$

**Ejercicio 2:** agrega $\theta\,\log(\mathrm{Inversión}_{it})$.

| Término | Definición |
|---|---|
| $i$, $t$ | Alcaldía y trimestre |
| $\mathrm{Viajes}_{it}$ | Viajes de ECOBICI que inician en estaciones de la alcaldía (total, mujeres u hombres) |
| $\mathrm{Inseguridad}_{it}$ | % de población de 18 años y más que se siente insegura (ENSU, cuadro 1.7) |
| $\mathrm{Estaciones}_{it}$ | Estaciones con al menos un viaje en el trimestre |
| $\mathrm{Inversión}_{it}$ | Inversión pública anual de la alcaldía (EFIPEM) |
| $\alpha_i$, $\lambda_t$ | Efectos fijos de alcaldía y de trimestre |
| $\varepsilon_{it}$ | Término de error |

**Inferencia.** Los errores estándar se agrupan por alcaldía. Como solo hay seis grupos, los intervalos y los p-valores usan una t con 5 grados de libertad (G − 1) y se verifican con un wild cluster bootstrap con pesos de Webb. Los coeficientes se leen como el cambio porcentual en viajes por cada 10 puntos más de población que se siente insegura.

## 9. Cómo ejecutar el proyecto

### Opción A: con Docker (recomendada)

La replicación completa, sin Colab ni Google Drive, está en el repositorio **[carlaAndr/dockerSeguridadEcobici](https://github.com/carlaAndr/dockerSeguridadEcobici)**. Corre los notebooks en orden, deja tablas y figuras en `salidas/` y compara cada archivo regenerado contra `artefactos/`. Requisito: [Docker Desktop](https://www.docker.com/products/docker-desktop/).

```bash
git clone https://github.com/carlaAndr/dockerSeguridadEcobici.git
cd dockerSeguridadEcobici
docker compose run --rm analisis
```

El README de ese repositorio explica los dos modos (`analisis` y `completo`) y qué deja cada carpeta de `salidas/`.

### Opción B: en Google Colab

Los notebooks de este repositorio se desarrollaron en Colab:

1. Clonar o descargar este repositorio y subir las carpetas a Google Drive.
2. Ajustar las rutas al inicio de cada notebook para que apunten a esa ubicación.
3. Correr los notebooks de `dataIngestion/` en el orden de la sección 6 y verificar que se generen los archivos de `artefactos/`.
4. Correr `PaneldeDatos.ipynb` para construir `panel_ecobici.csv`.
5. Correr `Modelado.ipynb` para obtener las tablas, las figuras y `modelado.csv`.

### Ver el sitio en local

Desde la raíz del repositorio, `python -m http.server` y abrir `http://localhost:8000`. Abrir `index.html` con doble clic no funciona, porque el navegador no deja leer los CSV.

## 10. Limitaciones

- **Junio 2026 de la ENSU (pendiente).** Al corregir el cambio de columnas, el ejercicio 1 gana observaciones y las cifras pueden moverse ligeramente.
- **Escala de la percepción.** La ENSU mide a residentes de toda la alcaldía, no a quienes viajan ni el entorno de cada estación.
- **Personas y viajes.** Los datos de viajes no permiten separar cambios en el número de personas usuarias de cambios en los viajes por persona.
- **Error de medición.** Las estimaciones de la ENSU por alcaldía tienen error muestral, lo que sesga el coeficiente hacia cero.
- **Pocos grupos.** Con seis alcaldías, la inferencia con errores agrupados es aproximada aun con la corrección t(5).
- **Panel no balanceado.** La entrada de tres alcaldías en 2023 pesa en la estimación.
- **Control de estaciones.** Las estaciones activas se calculan con los mismos datos de viajes, así que son un control imperfecto.

## 11. Fuentes y referencias institucionales

- **ECOBICI.** Datos abiertos del sistema de bicicletas públicas de la Ciudad de México: <https://ecobici.cdmx.gob.mx/datos-abiertos/>
- **INEGI, ENSU.** Encuesta Nacional de Seguridad Pública Urbana, tabulados básicos.
- **INEGI, EFIPEM.** Estadística de Finanzas Públicas Estatales y Municipales.

## 12. Uso de inteligencia artificial y transparencia metodológica

Durante el desarrollo de este proyecto se usaron herramientas de inteligencia artificial generativa, en particular ChatGPT y Claude, como apoyo en:

- **Procesamiento de datos:** identificar inconsistencias, homologar estructuras de información y construir las bases de análisis.
- **Visualización:** construir y mejorar gráficas, mapas y el sitio del proyecto.
- **Código y reproducibilidad:** revisar notebooks y preparar la ejecución con Docker.
- **Documentación técnica:** redactar descripciones metodológicas, comentarios de código y la documentación del repositorio.

**Responsabilidad sobre la investigación**

La inteligencia artificial se usó como herramienta de asistencia técnica y conceptual, no como sustituto del criterio científico. La definición del problema, la selección de fuentes, el planteamiento de hipótesis, las decisiones metodológicas y la interpretación de los resultados son responsabilidad de la autora.


