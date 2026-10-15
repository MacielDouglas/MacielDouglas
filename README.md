<div align="center">

# Douglas Maciel

**Full-Stack Developer & UX Designer · Next.js (App Router, Server Actions, TypeScript, Tailwind) + LLMs**

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/douglas-maciel-4943461b0/)
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/MacielDouglas)
[![Portfolio](https://img.shields.io/badge/Portfolio-000000?style=for-the-badge&logo=vercel&logoColor=white)](https://macield.vercel.app/)

</div>

---

## Sobre

Desenvolvedor full-stack com +5 anos de experiência. Construo **produtos web reais em produção** com **Next.js 16 (App Router, Server Components, Server Actions), React 19, TypeScript estrito e Tailwind CSS v4** — integrados a **LLMs (Groq) e sistemas autônomos de regras** (rate-limit, logging, fairness algorítmico, offline-first).

Padrão que repito em todo projeto: **UI → Server Action (Zod) → Service → Prisma + PostgreSQL → LLM com output validado → log/rate-limit**. IA sempre como **co-piloto com revisão humana**, nunca veredito fechado.

```
💼 Aberto a freelas premium e times de produto
📍 Ipojuca, Pernambuco — Brasil
🌐 https://macield.vercel.app/
```

---

## Stack principal

| Camada | Escolha |
|---|---|
| Framework | Next.js 16 (App Router, RSC, Server Actions, `proxy.ts`) · React 19 |
| Linguagem | TypeScript 5 (strict) |
| UI | Tailwind CSS v4 · shadcn/ui · Radix UI · lucide-react · Framer Motion |
| Dados | Prisma 7 + PostgreSQL (Neon, `@prisma/adapter-pg`) · Drizzle (Meeting) · Zod 4 |
| Auth | Better Auth (senha + Google OAuth) · RBAC por organização |
| IA | Groq SDK (`llama-3.3-70b`, `gpt-oss-120b`) · `json_object` + validação Zod · rate-limit por user · log estruturado (Pino) |
| Storage/Infra | Cloudflare R2 (S3) + sharp · Resend · Mapbox GL · Vercel · Bun |
| Qualidade | Vitest · Playwright (POM + axe-core) · Biome/ESLint · `tsc --noEmit` · Husky |

---

## Projetos em destaque

### 🤖 1. [OrquestraAI](https://github.com/MacielDouglas/OrquestraAI) — Kanban com IA sênior no fluxo

[![Demo](https://img.shields.io/badge/Demo-Vercel-000000?style=flat-square&logo=vercel)](https://orquestra-ai.vercel.app)
[![Next.js 16](https://img.shields.io/badge/Next.js-16-black?style=flat-square&logo=next.js)](https://nextjs.org)
[![Groq](https://img.shields.io/badge/LLM-Groq-FF4D00?style=flat-square)](https://groq.com)
[![E2E 63 passing](https://img.shields.io/badge/E2E-63_passing-22c55e?style=flat-square&logo=playwright)](https://github.com/MacielDouglas/OrquestraAI/tree/main/tests/e2e)

Board Kanban onde a IA atua como membro sênior: **gera subtarefas, resume o projeto e detecta gargalos** sob demanda — com rate-limit por usuário e logging de modelo/latência/tokens.

```mermaid
flowchart TD
  Board["Board Kanban (RSC + ilhas client)"] --> A1["generateSubtasksAction"]
  Board --> A2["summarizeProjectAction"]
  Board --> A3["detectBottlenecksAction"]
  A1 & A2 & A3 --> P["groq.provider (llama-3.3-70b, json_object)"]
  P --> Z["Zod valida output"]
  Z --> DB[("Postgres + AiSuggestions/log")]
```

**Decisões de design/UX:** IA como rascunho editável; server-first com `revalidatePath`; dark mode + WCAG AA (axe-core em CI); sidebar desktop / bottom-nav mobile.
**API/Actions:** `generateSubtasksAction`, `summarizeProjectAction`, `detectBottlenecksAction` — Result pattern `{ data, error }`, Zod antes de qualquer I/O, `requireSession()` + ownership.

### 🍳 2. [Queimando Panela](https://github.com/MacielDouglas/queimando_panela) — receitas com IA co-piloto

[![Demo](https://img.shields.io/badge/Demo-Vercel-000000?style=flat-square&logo=vercel)](https://queimandopanela.vercel.app)

Comunidade de receitas brasileiras: você escreve como faz em casa, a **IA confere (utensílios, nutrição, tempo, categoria)** e você revisa e publica. Rota `/o-que-tem`: a IA sugere receitas da sua geladeira (3/dia, rate-limit no backend).

```mermaid
flowchart TD
  Form["Form RHF+Zod + dropzone"] --> UP["sharp WebP -> R2"]
  Form --> AN["POST /api/recipes/analyze (Groq + retry 3x)"]
  AN --> Draft["Rascunho editável"]
  Draft --> PUB["Publicar"]
```

**Decisões:** mel queimado ≤10% da tela, cards sem sombra, linha 65–72ch, toque 44px. **API:** `POST /api/recipes/analyze`, `POST /api/geladeira/generate`.

### 🗓️ 3. [Meeting](https://github.com/MacielDouglas/Meeting) — sistema autônomo de escala

[![Demo](https://img.shields.io/badge/Demo-Vercel-000000?style=flat-square&logo=vercel)](https://meeting-pearl.vercel.app)

Programa semanal + designações por elegibilidade + **rodízio de limpeza em 8 passadas com fairness de 90 dias** (família atômica, fallbacks com aviso). Parser real de `.jwpub`, PWA Serwist + IndexedDB offline, impressão A4, feed iCal. **480 testes Vitest.**

### 🗺️ 4. [DireccionesV3](https://github.com/MacielDouglas/direccionesV3) — evolução 2024–2026 até produção

[![Demo](https://img.shields.io/badge/Demo-Vercel-000000?style=flat-square&logo=vercel)](https://direcciones.vercel.app)

Plataforma multi-tenant de endereços: React → Next.js 16, GraphQL → Prisma. Cards com rota Mapbox, levantamento colaborativo de pins, PWA com cache agressivo para campo com rede ruim. Fluxo **UI → Server Action → Service → Prisma**, RBAC owner/admin/member validado no servidor.

---

## Arquitetura padrão (todos os repos)

```mermaid
flowchart LR
  UI["RSC + ilhas use-client<br/>Tailwind + shadcn"] --> SA["Server Actions<br/>use server + Zod"]
  SA --> SVC["Services (regras puras, testadas)"]
  SVC --> DB["Prisma + PostgreSQL"]
  SVC --> EXT["Groq / R2 / Mapbox / Resend"]
```

Regras: páginas server-first com `Promise.all`, `Suspense` + skeleton por aba, client recebe só flags (`canManage`), zero supressão de lint, testes cobrindo domínio.

---

## Documentação por repo

Cada repositório destacado contém: diagrama de arquitetura (mermaid), seção **Decisões Técnicas / Design**, tabela de **API / Server Actions** e instruções de run local (`bun install` → `.env` → `prisma migrate deploy` → `bun run dev`). Detalhes de produto em `PRODUCT.md` e de design em `DESIGN.md`.

---

## 📊 GitHub Stats

<div align="center">
  <img height="180em" src="https://github-readme-stats.vercel.app/api?username=MacielDouglas&show_icons=true&theme=dark&include_all_commits=true&count_private=true"/>
  <img height="180em" src="https://github-readme-stats.vercel.app/api/top-langs/?username=MacielDouglas&layout=compact&theme=dark&langs_count=8"/>
</div>

---

<div align="center">

**Vamos conversar?**

[![LinkedIn](https://img.shields.io/badge/Me_chame_no_LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/douglas-maciel-4943461b0/)

</div>
