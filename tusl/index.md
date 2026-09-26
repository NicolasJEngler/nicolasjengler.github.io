---
layout: page
title: "Bitácora TUSL"
permalink: /tusl/
noindex: true
---

Registro de actividades y notas de la materia **Procesos Educativos y Software Libre**, de la Tecnicatura Universitaria en Software Libre (UNL).

## Enlaces de interés

- [Educ.ar](https://www.educ.ar/): portal educativo del Estado argentino, con recursos de tecnología educativa y políticas públicas.
- [Fundación Vía Libre](https://www.vialibre.org.ar/): organización de Córdoba que trabaja por el software libre, la cultura libre y los derechos digitales.
- [Do I Really Own the Digital Media I Bought? (EFF)](https://drb.eff.org/topics/wait-i-don-t-own-the-digital-downloads-i-buy): sobre medios físicos frente a licencias digitales, y por qué "comprar" ya no es poseer.

## Entradas

{%- assign entradas = site.tusl | sort: "date" | reverse -%}
{%- for entry in entradas %}
- [{{ entry.title }}]({{ entry.url | relative_url }}) ({{ entry.date | date: "%d/%m/%Y" }})
{%- endfor %}
