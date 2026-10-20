# 🎯 Guía rápida: AI Job Search — la búsqueda de empleo que corre en tu máquina (ES · PT)

[🇪🇸 Español](#-español) · [🇵🇹 Português](#-português) · [🇬🇧 English summary](#-english-summary)

Una sola página para usar **AI Job Search** (de MadsLorentzen): el framework sobre Claude Code que evalúa ofertas, adapta tu CV a cada puesto, escribe la carta de presentación y te prepara la entrevista. Todo corre en tu ordenador y tus datos quedan en archivos tuyos.

**Autor:** Guangyi Zhao · Octubre 2026

Proyecto original: https://github.com/MadsLorentzen/ai-job-search (59.637 estrellas y 18.691 forks, consultado el 20 de octubre de 2026)

> **Nota de autoría:** esta guía es una síntesis original, redactada por el autor a partir de la documentación pública del proyecto y de su propio uso. No es una traducción ni una copia de la documentación oficial, y no es un proyecto oficial de MadsLorentzen ni de Anthropic. Para el detalle completo, ve al repositorio enlazado.

---

## 🇪🇸 Español

### Qué es

Un framework de código abierto (MIT) que convierte a Claude Code en un asistente completo de búsqueda de empleo. Haces fork, rellenas tu perfil una vez, y el sistema trabaja oferta por oferta: la puntúa contra tu perfil, redacta el CV y la carta a medida con un segundo agente que hace de revisor, y te prepara la entrevista.

El núcleo —perfil, evaluación, redacción con revisor— no depende del país ni del idioma. Lo que viene «de fábrica» para Dinamarca son solo los buscadores de portales; el patrón está pensado para cambiarlos por los de tu mercado (abajo, la tabla de sustitución).

¿Funciona? El autor, geofísico de formación, lo construyó para su propia búsqueda tras perder su puesto a finales de 2025: 69 solicitudes a medida, 20 primeras entrevistas y un contrato firmado —empezó como ingeniero de IA en junio de 2026— (cifras del propio proyecto).

### El flujo, paso a paso

| Paso | Comando | Qué le das | Qué obtienes |
|---|---|---|---|
| Preparación | `/setup` | Tu CV, tu carpeta de documentos (diplomas, referencias) o una entrevista guiada | Tus archivos de perfil: experiencia, estilo de escritura, criterios de evaluación |
| 1 · Buscar | `/scrape` | Los portales configurados y tu perfil | Ofertas ordenadas por ajuste, sin duplicados; con `/rank`, todas puntuadas de golpe y una lista corta |
| 2 · Evaluar | `/apply <url>` (primera fase) | La URL de la oferta o su texto pegado | Veredicto y puntuación de ajuste, con fortalezas y huecos honestos |
| 3 · CV a medida | `/apply` (redactor + revisor) | Tu perfil y la oferta | CV en LaTeX compilado y revisado página a página hasta quedar en 2 páginas exactas; control ATS sobre la capa de texto del PDF |
| 4 · Carta | `/apply` | El CV final | Carta de presentación de 1 página exacta, orientada al futuro, no un resumen del CV |
| 5 · Entrevista | `/interview` | La oferta y lo que enviaste | Paquete por etapa: investigación de la empresa, preguntas probables cruzadas con tus ejemplos STAR y simulacro de entrevista |

Comandos que también usarás: `/outcome` (registra qué pasó con cada solicitud y archiva los materiales), `/upskill` (mapa de huecos de habilidades entre tu perfil y las ofertas, con plan de aprendizaje), `/expand` (enriquece tu perfil desde tus fuentes públicas ya enlazadas), `/add-template` (registra tus propias plantillas de CV o carta), `/gmail-sync` y `/notion-sync` (detectan señales de tus solicitudes en Gmail y publican una vista del pipeline en Notion), `/html-report` (panel HTML de tu pipeline, sin conexión), `/reset` (borrar el perfil y empezar de cero, con confirmación).

### Requisitos: todo corre en tu máquina

- **Claude Code** (CLI). No tiene nivel gratuito: necesitas una suscripción Claude Pro/Max/Team o créditos de API de Anthropic.
- **Python 3.10+** y **Bun** (para las herramientas de búsqueda en portales).
- **Una distribución LaTeX** (TeX Live, MacTeX, TinyTeX o MiKTeX): el CV compila con `lualatex` y la carta con `xelatex`.
- Opcional: `pip install pypdf` para el control ATS de `/apply`.

Tus datos personales viven en los archivos del repo, en tu disco. Nada del flujo exige subir tu CV a un servicio de terceros.

### Sustituir los portales daneses por los tuyos

El proyecto trae buscadores para Jobindex, Jobnet, Akademikernes Jobbank y Jobdanmark. El mecanismo oficial para cambiar de mercado es el comando `/add-portal`: le das la URL de tu portal, investiga su patrón de búsqueda y sus reglas de acceso, genera la herramienta con la misma estructura que las danesas y hace una búsqueda real de prueba antes de registrarla. Los portales con muro de inicio de sesión se rechazan.

| Lo que trae (Dinamarca) | Qué es | En español | Em português |
|---|---|---|---|
| Jobindex | El mayor portal comercial del país | InfoJobs (España) · Computrabajo (Latinoamérica) | InfoJobs Brasil · Catho |
| Jobnet | Portal público de empleo (gobierno) | SEPE y los servicios autonómicos de empleo | SINE / portal Emprega Brasil (governo) |
| Akademikernes Jobbank | Ofertas para perfiles académicos | Portales de universidades y colegios profesionales | Portais de universidades e conselhos profissionais |
| Jobdanmark | Portal generalista | Computrabajo | Gupy · Vagas.com |
| LinkedIn (incluido) | Listados públicos de LinkedIn | Vale tal cual: la ubicación se pasa como parámetro | Vale tal qual: a localização vai como parâmetro |
| freehire.me (incluido) | Agregador tech por API pública | Vale tal cual para ofertas tech y remotas | Vale tal qual para vagas tech e remotas |

### ⚠️ Errores típicos

- **Hacer fork público y rellenar ahí tu perfil.** Los forks de repos públicos son siempre públicos, y `/setup` escribe tus datos personales (nombre, historial laboral, expectativas salariales) en archivos versionados: cualquiera podría leerlos. Para uso propio, el proyecto recomienda un repositorio **privado** con el original como `upstream` (la receta está en su SETUP.md).
- **Inflar el CV.** El flujo verifica cada afirmación contra tu perfil. Un hueco real queda marcado como hueco; no se rellena con palabras clave que no puedas defender en una entrevista.
- **Pegar ofertas sin mirar.** Las ofertas se tratan como entrada no confiable: pueden traer texto oculto o instrucciones escondidas dirigidas al agente. El framework las filtra antes de evaluar, pero el último vistazo antes de enviar lo das tú.
- **Perfil pobre, resultado genérico.** La calidad sale del detalle: qué hiciste en cada puesto, con qué herramientas y con qué resultados medibles. Una lista de títulos produce cartas que suenan a plantilla.

---

## 🇵🇹 Português

### O que é

Um framework de código aberto (MIT) que transforma o Claude Code em um assistente completo de busca de emprego. Você faz fork, preenche seu perfil uma vez, e o sistema trabalha vaga por vaga: pontua a vaga contra o seu perfil, redige o currículo e a carta sob medida com um segundo agente como revisor, e prepara você para a entrevista.

O núcleo —perfil, avaliação, redação com revisor— não depende do país nem do idioma. O que vem «de fábrica» para a Dinamarca são apenas as buscas em portais; o padrão foi feito para trocá-las pelos do seu mercado (abaixo, a tabela de substituição).

Funciona? O autor, geofísico de formação, construiu o framework para a própria busca depois de perder o cargo no fim de 2025: 69 candidaturas sob medida, 20 primeiras entrevistas e um contrato assinado —ele começou como engenheiro de IA em junho de 2026— (números do próprio projeto).

### O fluxo, passo a passo

| Passo | Comando | O que você entrega | O que você recebe |
|---|---|---|---|
| Preparação | `/setup` | Seu currículo, sua pasta de documentos (diplomas, referências) ou uma entrevista guiada | Seus arquivos de perfil: experiência, estilo de escrita, critérios de avaliação |
| 1 · Buscar | `/scrape` | Os portais configurados e o seu perfil | Vagas ordenadas por aderência, sem duplicatas; com `/rank`, todas pontuadas de uma vez e uma lista curta |
| 2 · Avaliar | `/apply <url>` (primeira fase) | A URL da vaga ou o texto colado | Veredito e pontuação de aderência, com pontos fortes e lacunas honestos |
| 3 · Currículo sob medida | `/apply` (redator + revisor) | Seu perfil e a vaga | Currículo em LaTeX compilado e revisado página a página até ficar em exatamente 2 páginas; controle ATS sobre a camada de texto do PDF |
| 4 · Carta | `/apply` | O currículo final | Carta de apresentação de exatamente 1 página, voltada ao futuro, não um resumo do currículo |
| 5 · Entrevista | `/interview` | A vaga e o que você enviou | Pacote por etapa: pesquisa da empresa, perguntas prováveis cruzadas com seus exemplos STAR e simulação de entrevista |

Comandos que você também vai usar: `/outcome` (registra o que aconteceu com cada candidatura e arquiva os materiais), `/upskill` (mapa de lacunas de habilidades entre seu perfil e as vagas, com plano de aprendizado), `/expand` (enriquece seu perfil a partir das suas fontes públicas já ligadas), `/add-template` (registra seus próprios modelos de currículo ou carta), `/gmail-sync` e `/notion-sync` (detectam sinais das suas candidaturas no Gmail e publicam uma visão do pipeline no Notion), `/html-report` (painel HTML do seu pipeline, offline), `/reset` (apagar o perfil e começar do zero, com confirmação).

### Requisitos: tudo roda na sua máquina

- **Claude Code** (CLI). Não tem plano gratuito: é preciso uma assinatura Claude Pro/Max/Team ou créditos da API da Anthropic.
- **Python 3.10+** e **Bun** (para as ferramentas de busca em portais).
- **Uma distribuição LaTeX** (TeX Live, MacTeX, TinyTeX ou MiKTeX): o currículo compila com `lualatex` e a carta com `xelatex`.
- Opcional: `pip install pypdf` para o controle ATS do `/apply`.

Seus dados pessoais vivem nos arquivos do repo, no seu disco. Nada no fluxo exige enviar seu currículo a um serviço de terceiros.

### Trocar os portais dinamarqueses pelos seus

O projeto traz buscas para Jobindex, Jobnet, Akademikernes Jobbank e Jobdanmark. O mecanismo oficial para mudar de mercado é o comando `/add-portal`: você passa a URL do seu portal, ele investiga o padrão de busca e as regras de acesso, gera a ferramenta com a mesma estrutura das dinamarquesas e faz uma busca real de teste antes de registrar. Portais com muro de login são recusados.

| O que vem (Dinamarca) | O que é | Em português | En español |
|---|---|---|---|
| Jobindex | O maior portal comercial do país | InfoJobs Brasil · Catho | InfoJobs (España) · Computrabajo (Latinoamérica) |
| Jobnet | Portal público de emprego (governo) | SINE / portal Emprega Brasil (governo) | SEPE y los servicios autonómicos de empleo |
| Akademikernes Jobbank | Vagas para perfis acadêmicos | Portais de universidades e conselhos profissionais | Portales de universidades y colegios profesionales |
| Jobdanmark | Portal generalista | Gupy · Vagas.com | Computrabajo |
| LinkedIn (incluído) | Listagens públicas do LinkedIn | Vale tal qual: a localização vai como parâmetro | Vale tal cual: la ubicación se pasa como parámetro |
| freehire.me (incluído) | Agregador tech por API pública | Vale tal qual para vagas tech e remotas | Vale tal cual para ofertas tech y remotas |

### ⚠️ Erros típicos

- **Fazer fork público e preencher o perfil nele.** Forks de repositórios públicos são sempre públicos, e o `/setup` escreve seus dados pessoais (nome, histórico de trabalho, expectativa salarial) em arquivos versionados: qualquer um poderia lê-los. Para uso próprio, o projeto recomenda um repositório **privado** com o original como `upstream` (a receita está no SETUP.md dele).
- **Inflar o currículo.** O fluxo confere cada afirmação contra o seu perfil. Uma lacuna real fica marcada como lacuna; não se preenche com palavras-chave que você não consiga defender numa entrevista.
- **Colar vagas sem olhar.** As vagas são tratadas como entrada não confiável: podem trazer texto oculto ou instruções escondidas dirigidas ao agente. O framework filtra antes de avaliar, mas a última olhada antes de enviar é sua.
- **Perfil pobre, resultado genérico.** A qualidade sai do detalhe: o que você fez em cada cargo, com quais ferramentas e com quais resultados mensuráveis. Uma lista de cargos gera cartas com cara de modelo pronto.

---

## 🇬🇧 English summary

One-page cheat sheet (Spanish + Portuguese) for **AI Job Search** (MadsLorentzen): the Claude Code framework that runs your job search on your own machine — profile setup once (`/setup`), portal scraping (`/scrape`, `/rank`), honest per-posting fit scoring, a drafter–reviewer pipeline that drafts and compiles a tailored LaTeX CV (exactly 2 pages, with an ATS check on the PDF text layer) and a 1-page cover letter, plus interview prep (`/interview`) from your own STAR examples. The core workflow is country- and language-agnostic; only the bundled portal searches are Danish (Jobindex, Jobnet, Akademikernes Jobbank, Jobdanmark). The sheet's localization table maps them to Spanish boards (InfoJobs, Computrabajo, SEPE) and Brazilian ones (InfoJobs Brasil, Catho, Gupy, SINE), and explains the official swap mechanism, `/add-portal`, which scaffolds a search tool for your local board and test-runs it. Also covered: requirements (Claude Code subscription or API credits, Python 3.10+, Bun, LaTeX), and the typical mistakes — above all, filling in your profile inside a public fork (forks of public repos are always public, and `/setup` writes personal data into tracked files; use a private repo with upstream instead). The author's own run of the framework: 69 tailored applications, 20 first interviews, one signed contract. Original synthesis by the author from the project's public documentation; not an official document and not a translation.

**Author:** Guangyi Zhao · October 2026 · MIT License
