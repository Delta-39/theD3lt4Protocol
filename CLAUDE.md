# Plan de implementación — The D3LT4 Protocol reestructura

## Contexto

Repo: `github.com/Delta-39/theD3lt4Protocol`
Stack: MkDocs + Material theme, hosteado en GitHub Pages
Branch: `main`
Estructura actual: `docs/` (contenido markdown), `site/` (build), `mkdocs.yml` (config)

El sitio es un blog técnico de ciberseguridad en español creado por Martin Terribile (DevOps, apasionado por security). Tiene contenido sólido de write-ups (DockerLabs, CyberDefenders), tutoriales (AD, pfSense, Docker, malware) y recursos. Está desactualizado (~2024) y necesita expandirse a cloud/devops y AI security sin perder la esencia técnica.

## Qué hay que hacer

### 1. Reestructurar `mkdocs.yml`

Reemplazar la sección `nav:` con esta nueva estructura. Security va primero (es lo que más le gusta), cloud & devops después (es su laburo), AI & security cruza ambos:

```yaml
nav:
  - Inicio: index.md

  - Offensive Security:
    - maquinas-vulnerables/index.md
    - DockerLabs:
      - Muy Fácil:
        - maquinas-vulnerables/dockerlabs/muyFacil/injection.md
        - maquinas-vulnerables/dockerlabs/muyFacil/firsthacking.md
      - Fácil:
        - maquinas-vulnerables/dockerlabs/facil/amor.md
        - maquinas-vulnerables/dockerlabs/facil/aguademayo.md
        - maquinas-vulnerables/dockerlabs/facil/anonymousPingu.md
        - maquinas-vulnerables/dockerlabs/facil/pequeñas_mentirosas.md
        - maquinas-vulnerables/dockerlabs/facil/psycho.md
    - Herramientas:
      - tutoriales/herramientas-hacking/nmap.md
      - tutoriales/herramientas-hacking/wireshark.md

  - Blue Team & DFIR:
    - cyberdefenders/index.md
    - Endpoint Forensics:
      - cyberdefenders/endpoint-forensics/insider.md
      - cyberdefenders/endpoint-forensics/redline-stealer-lab.md
      - cyberdefenders/endpoint-forensics/ramnit-lab.md
      - cyberdefenders/endpoint-forensics/reveal-lab.md
    - Network Forensics:
      - cyberdefenders/network-forensics/webstrike-lab.md
      - cyberdefenders/network-forensics/poisoned-credentials-lab.md
      - cyberdefenders/network-forensics/tomcat-lab.md
      - cyberdefenders/network-forensics/packet-maze.md
    - Threat Intel:
      - cyberdefenders/threat-intel/iceId-lab.md
    - Análisis de Malware:
      - tutoriales/analisis-malware/laboratorio-malware.md

  - Cloud & DevOps:
    - cloud-devops/index.md
    - Contenedores:
      - tutoriales/Docker/primeros-pasos-docker.md
      - tutoriales/Docker/instalacion-portainer.md
    - Redes & Firewalls:
      - tutoriales/pfSense/primeros-pasos-pfsense.md
      - tutoriales/pfSense/configurar-openVpn-segura.md
    - Identity:
      - tutoriales/active-directory/creacion-dominio.md
      - tutoriales/active-directory/creacion-gpo.md

  - AI & Security:
    - ai-security/index.md
    - ai-security/ai-seguridad-infra-2026.md

  - Blog:
    - blog/index.md

  - Recursos: recursos/enlaces-utiles.md
  - Acerca de: about.md
```

Otros cambios al `mkdocs.yml`:
- `site_description`: cambiar a `"Ciberseguridad, Cloud e Inteligencia Artificial — Guías técnicas, labs y artículos"`
- `copyright`: cambiar año a 2026
- Agregar `extra.social` con GitHub y LinkedIn:
  ```yaml
  extra:
    social:
      - icon: fontawesome/brands/github
        link: https://github.com/Delta-39
      - icon: fontawesome/brands/linkedin
        link: https://www.linkedin.com/in/juan-martin-terribile-giles-90a4b1229/
  ```
- Agregar plugin `tags` si no está
- Agregar extensión `pymdownx.tabbed` con `alternate_style: true`
- Agregar extensión `pymdownx.tasklist` con `custom_checkbox: true`
- Agregar `toc: permalink: true`
- Cambiar ícono de logo a `material/shield-lock` (más representativo)
- Traducir los toggles de tema a español

### 2. Crear carpetas nuevas dentro de `docs/`

```
docs/cloud-devops/
docs/ai-security/
docs/blog/
```

### 3. Crear archivos nuevos

#### `docs/index.md` — Homepage renovada
Reescribir el inicio. Debe tener:
- Título "The D3LT4 Protocol"
- Descripción breve: blog técnico sobre ciberseguridad, cloud e IA
- Sección con las 5 áreas (Offensive, Blue Team, Cloud & DevOps, AI & Security, Blog) — cada una con ícono Material, descripción de 1 línea y link
- Tabla de "Últimos contenidos" con el primer artículo
- Tip box con quién es Martin y link a About
- Tono: directo, técnico, sin florituras. Argentino pero profesional.

#### `docs/cloud-devops/index.md` — Índice de Cloud & DevOps
- Título con ícono
- Descripción: tu lado DevOps — containers, IaC, CI/CD, cloud providers, redes
- Listar el contenido existente que se mudó acá (Docker, pfSense, AD) con links
- Roadmap con checkboxes de lo que viene: Kubernetes, Terraform, Ansible, AWS/Azure fundamentals, CI/CD security, GitHub Actions

#### `docs/ai-security/index.md` — Índice de AI & Security
- Título con ícono
- Descripción: donde AI cruza con security e infra — deploy seguro de modelos, ataques a LLMs, AI para defensa, seguridad de agentes
- Link al primer artículo
- Roadmap: prompt injection 101, self-hosting LLMs, AI en el SOC, MLOps security, RAG seguro

#### `docs/cyberdefenders/index.md` — Índice de Blue Team & DFIR
- Título con ícono
- Tablas con los labs existentes organizados por categoría (Endpoint Forensics, Network Forensics, Threat Intel)
- Link al lab de malware
- Mantener el estilo que ya tienen los otros index

#### `docs/blog/index.md` — Índice del Blog
- Título con ícono
- Descripción: artículos técnicos, análisis de incidentes, opinión con sustancia
- Nota de "próximamente" con ideas del backlog: brechas cloud 2026, review de herramientas open-source, Zero Trust en LATAM, por qué tu CI/CD es un vector de ataque

#### `docs/ai-security/ai-seguridad-infra-2026.md` — PRIMER ARTÍCULO (para publicar mañana)

**Título:** "AI y Seguridad en 2026: el panorama para alguien que hace DevOps y Security"

**Tags:** ai, seguridad, infraestructura, cloud, devops, 2026

**Enfoque:** No es un artículo genérico de tendencias. Es la perspectiva de alguien que labura en DevOps y le apasiona la seguridad, mirando cómo AI cambia ambos mundos. Técnico, con código y configs reales.

**Estructura del artículo:**

1. **Intro** — En 2026 AI se metió en la infra y en el stack de seguridad. GenAI presente en el 77% de security stacks (dato de CSA 2026). Solo el 14% deja que AI tome acciones sola. Qué significa eso en la práctica.

2. **AI en el stack de seguridad** — Qué se usa hoy de verdad:
   - Análisis de logs con LLMs (ejemplo práctico: `journalctl | ollama run`)
   - IaC review automatizado (Terraform + LLM como primer filtro antes de tfsec)
   - Generación de playbooks de Ansible para hardening
   - Qué NO funciona todavía: SOC autónomo, detección de zero-days con AI

3. **AI como superficie de ataque** — Los riesgos concretos:
   - Prompt injection (explicar con ejemplo técnico)
   - Model supply chain: modelos con código malicioso en pickle files, usar safetensors
   - Agentes AI con acceso a herramientas: el caso OpenClaw (135K stars en GitHub, vulnerabilidades críticas, shadow AI en Slack/Workspace)
   - Social engineering potenciado por AI

4. **El lado DevOps: AI como workload** — Deployar modelos es un problema de infra:
   - Compute: GPUs cloud vs on-prem, costos ($32/hr una p4d.24xlarge)
   - Networking: rate limiting, balanceo para APIs de inferencia
   - Storage: modelos de 40GB+, datasets de TB
   - Orquestación: K8s con GPU scheduling
   - Ejemplo práctico: self-hosting Ollama, qué pasa por detrás, cómo hardenearlo (bind localhost, reverse proxy nginx con auth, Docker con resource limits y read-only)

5. **Lo que viene** — Zero Trust + AI, seguridad de pipelines CI/CD con checks de AI, cloud-native security, preemptive cybersecurity (Gartner 2026). Qué skills priorizar.

6. **Cierre** — Links a próximos posts del blog.

**Estilo:** Español argentino natural pero profesional. Incluir bloques de código (bash, nginx, docker, yaml). Usar admonitions de MkDocs (tip, warning, info). Tablas donde aporten. Sin emojis excesivos. Técnico y directo — como si lo explicaras a un compañero de laburo.

### 4. Actualizar `docs/about.md`

- Actualizar bio: "Juan Martin Terribile — DevOps con foco en cloud e infra, apasionado por la ciberseguridad"
- Agregar links a GitHub y LinkedIn visibles
- Mencionar el blog como espacio para documentar lo que aprende en la intersección de infra, cloud y security
- Actualizar año a 2026
- Mantener la foto si la tiene

### 5. Build y verificar

```bash
mkdocs build
mkdocs serve  # verificar que todo renderice bien
```

Verificar que:
- Todos los links internos funcionen (especialmente los archivos que se movieron de sección en la nav pero siguen en la misma ruta física)
- Las nuevas secciones aparezcan en la barra de navegación
- El primer artículo renderice correctamente con code blocks y admonitions
- El dark mode siga funcionando
- La búsqueda indexe el contenido nuevo

## Notas importantes

- **NO mover archivos físicamente.** Los .md quedan donde están (tutoriales/Docker/, tutoriales/pfSense/, etc.). Solo cambia la organización en `nav:` del mkdocs.yml. MkDocs permite referenciar archivos desde cualquier path.
- **NO borrar contenido existente.** Todo se mantiene, solo se reorganiza.
- **El tono es técnico y directo.** No es un blog corporativo. Es un espacio personal de un DevOps que le gusta security. Español argentino, vos en vez de tú, pero sin slang excesivo.
- **El artículo tiene que tener código real.** Configs de nginx, comandos docker, ejemplos con ollama, yaml de kubernetes. Sin esto no es "The D3LT4 Protocol".
