---
title: 'Listados de RSS relevantes'
date: '2026-10-26'
tags: ["tecnología 💻"]      # "linux 🐧"
              # "tecnología 💻"
              # "religión ✝️"
              # "personal"
              # "filosofía 👨‍👩‍👦"
              # "política 📜"
              # "opinión 💭"
readingTime: true
toc: false
draft: false
image: ''
---

Diariamente uso [RSS](https://rss.com/blog/how-do-rss-feeds-work/) para
mantenerme actualizado. Es una excelente herramienta para recibir noticias
directamente en una bandeja personal sin tener que navegar la web, y desde
blogs hasta sitios de noticias la han adoptado.

Existen varios listados o *feeds*, sin embargo aquí me gustaría
destacar dos en particular que me han mantenido bastante informado:

- [**Diario Oficial de la Federación:**](https://dof.gob.mx/sumario.xml) Particularmente útil porque se presentan comunicados de todo tipo de dependencias, desde Banxico al IMPI e incluso algunas sentencias de la SCJN.
- [**INEGI:**](https://inegi.org.mx/servicios/rss/) Útil para recibir información de ciertos indicadores y encuestas (INPC, PIBT, ENVIPE).
    - Desde oct. 16 parece ser que las *feeds*  principales de noticias e indicadores están rotas. Logré rescatar la de las noticias con la siguiente entrada en mis `urls`:

        ```"exec:curl -fsSL 'https://www.inegi.org.mx/rss/noticias/xmlfeeds?p=' | sed 's/ encoding=\"UTF-8\"//'" "~INEGI Banco de indicadores (nuevo)" ```

Y uno más que no está de más tener:

- [**Vatican News:**](https://www.vaticannews.va/es/evangelio-de-hoy.rss.xml) El Evangelio de hoy.

Si entiendes RSS sabes cómo suscribirte a estas *feeds*. Si no, probablemente 
vaya a hacer un video al respecto en el futuro.

