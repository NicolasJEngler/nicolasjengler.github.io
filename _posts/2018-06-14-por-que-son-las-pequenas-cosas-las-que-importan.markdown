---
layout: post
title:  "[Spanish] ¿Por qué son las pequeñas cosas las que importan?"
date:   2018-06-14 10:00:00 -0300
categories: 'design'
---

### Originally on [Tienda Nube's Medium Publication](https://medium.com/tienda-nube/por-que-son-las-pequenas-cosas-las-que-importan-28486ea8edc0)

Es muy común escuchar que la magia está en las pequeñas cosas, y si bien esto suele aplicarse a cuestiones más motivacionales, la realidad es que aplica también para diferentes aspectos del desarrollo y diseño de productos digitales.

Suele ser fácil discernir cuándo una experiencia es óptima y cuándo no, pero no es tan fácil encontrar a simple vista las razones que causan esta diferencia. Aquí es cuando entra en juego el análisis de las diferentes aristas que separan un gran producto de uno no tan bueno. Como diseñadores UI/UX, desarrolladores front-end, productores digitales, o cualquiera sea nuestro título, estamos todo el día pendientes de que los usuarios se sientan a gusto usando nuestra app/website/producto, que su experiencia les sea placentera y sencilla, y esto se logra a través de una serie de conocimientos y aplicaciones muy amplios que no podríamos cubrir únicamente en un artículo, por lo cual hoy nos vamos a enfocar en 2 cosas específicas: las micro-animaciones y las micro-interacciones, trabajando en conjunto.

[Example of an animated popover versus a non-animated popover](https://codepen.io/nicolasjengler/pen/QrBKJV)

En el ejemplo anterior podemos decir que el popover animado es sin duda el que ofrece la mejor experiencia de usuario, pero ¿podemos discernir el por qué a primera vista? ¿o acaso estamos únicamente decantando por la opción más atractiva visualmente? Bueno, la realidad es que si bien el popover de la izquierda es mucho más sexy e interesante que el de la derecha, también hay un par de grandes razones que prueban que ofrece una mejor experiencia.

#### Carga cognitiva

La opción animada permite al usuario entender qué está pasando, el tiempo de transición permite aminorar la carga cognitiva dejándole al usuario algo de tiempo para entender que la interacción está disparando un cambio que se dió al momento de un evento en particular (hover, tap, focus, comando de voz, etcétera).

#### Origen

El animar el popover desde su centro inferior (o como lo veríamos en coordenadas de CSS: `50% 100%`) permite al usuario captar de dónde está saliendo este nuevo elemento que aparece en pantalla, y relacionarlo con su elemento de origen.

Toda animación a una interacción debería estar apuntada a darle la mayor cantidad de información posible, sin ser intrusiva, al usuario para que este pueda comprender la razón de este cambio, qué acción (o inacción) necesita tomar, y cuál puede ser el resultado.

#### Otros ejemplos

Este tipo de mejoras en la experiencia de usuario también se puede ver en grandes sistemas de diseño, y el caso más directo que podemos encontrar es el de Material Design. Material Design utiliza, principalmente, el concepto de profundidad para que el usuario pueda entender con qué objetos está interactuando en un website o una app, cuál tiene mayor jerarquía, cuál necesita atención con mayor urgencia, etcétera. El sistema de diseño de Google le da feedback al usuario, en mayor parte, cambiando la elevación y el orden de los objetos, y un cambio de elevación en estos a su vez produce, entre otras cosas, un cambio en su sombra, por lo que todas estas micro-transformaciones terminan creando un sistema cohesivo y comunicativo que no solo luce visualmente atractivo sino que también mejora su usabilidad y accesibilidad, lo guía a lo largo de la app haciendo que su comunicación con el usuario sea clara y amena.

[Elevation in Material Design](https://material.io/design/environment/elevation.html)

Por otro lado, entrando en un caso más aislado que forma parte de un grupo más complejo, iOS utiliza la elasticidad para demostrarle al usuario cómo se da la interacción con los diferentes elementos de la interfaz (en el ejemplo que se ve abajo se trata de Peek and Pop particularmente), lo que, de nuevo, permite comprender de dónde viene el objeto (su origen se da desde el elemento en el que estamos haciendo 3D Touch), cuál es su estado inicial (únicamente vemos un preview de la imagen), y qué acción requiere o no por parte del usuario (inicialmente aparece con un fade in el chevron superior, indicando que se puede hacer swipe hacia arriba, y luego hacen una entrada los botones accionables de la UI, por lo cual la interacción llegó al limite y la imagen vuelve a su lugar con una especie de efecto elástico).

[User interaction in the iOS HIG](https://developer.apple.com/ios/human-interface-guidelines/user-interaction/)

#### Resumiendo

Si estamos trabajando en un proyecto que se basa en HTML/CSS, tener un punto de partida es tan sencillo como definir un set de easings para las animaciones (si es que no queremos usar las keywords predefinidas de CSS: ease, ease-in, ease-out, ease-in-out), definir las interacciones que se verán a lo largo del site junto a sus cambios visuales en el CSS, y finalmente colocar los elementos en el HTML del sitio. Como dijo Val Head en [un capítulo del podcast de Intercom](https://blog.intercom.com/val-head-designing-interface-animation/):

> People should definitely be thinking about creating their own motion guidelines. That doesn’t have to be a giant, huge, public beautiful thing like material design.

Es necesario entender que no se necesita crear un gran sistema de diseño o interacciones si estamos trabajando en un proyecto chico o mediano, con un crear una pequeña checklist que nos ayude a mantener consistentemente las mismas animaciones para cada interacción, es suficiente. Se puede definir, por ejemplo, que todos los botones de un website se eleven `1px` y que la opacidad de su sombra pase de `.75` a `.5` en un estado de hover/focus; acciones tan pequeñas y sencillas como estas nos pueden ayudar a elevar el look and feel de nuestro proyecto. La magia realmente está en las pequeñas cosas.