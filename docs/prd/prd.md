# Aplicação de Enquetes — Product Requirements Document (PRD)

**Versão:** 1.0
**Data:** 2026-03-22
**Autor:** Morgan (@pm)
**Status:** Aprovado

---

## Change Log

| Data | Versão | Descrição | Autor |
|------|--------|-----------|-------|
| 2026-03-22 | 1.0 | Criação inicial | Morgan (@pm) |

---

## 1. Goals & Background Context

### Goals

- Permitir que qualquer pessoa crie enquetes públicas rapidamente, sem configuração complexa
- Garantir integridade dos votos com controle de 1 voto por participante (sem cadastro obrigatório)
- Exibir resultado visual (gráfico) imediatamente após a votação
- Oferecer ao criador um dashboard para gerenciar e monitorar suas enquetes
- Suportar expiração automática de enquetes por data/hora

### Background Context

Ferramentas de enquete atuais exigem cadastro dos participantes, dificultando a adesão rápida. A proposta é uma aplicação web responsiva onde o criador (autenticado) configura a enquete — título, opções e data de expiração — e distribui um link único. Participantes acessam sem cadastro, votam uma única vez e veem o resultado em gráfico.

O diferencial está na combinação de simplicidade de acesso para o participante com controle de integridade do voto e experiência visual imediata do resultado, tudo em uma única plataforma responsiva.

---

## 2. Requirements

### Functional Requirements

- **FR1:** O criador pode se cadastrar e fazer login na aplicação
- **FR2:** O criador pode criar uma enquete com: título, mínimo 2 opções de voto, e data/hora de expiração
- **FR3:** O sistema gera um link único e compartilhável para cada enquete criada
- **FR4:** O criador possui dashboard listando todas as suas enquetes (ativas, expiradas)
- **FR5:** O participante acessa a enquete via link único sem necessidade de cadastro
- **FR6:** O sistema permite apenas 1 voto por participante por enquete (controle por fingerprint/IP)
- **FR7:** Após votar, o participante visualiza um gráfico com o resultado parcial dos votos
- **FR8:** Enquetes expiradas exibem mensagem de encerramento e resultado final (sem permitir novos votos)
- **FR9:** O criador pode visualizar os resultados da sua enquete em tempo real no dashboard

### Non-Functional Requirements

- **NFR1:** A aplicação deve ser responsiva e funcionar em desktop e mobile (web)
- **NFR2:** O tempo de carregamento do gráfico após o voto deve ser inferior a 1 segundo
- **NFR3:** O controle de 1 voto por participante deve ser eficaz em 100% dos casos durante a sessão
- **NFR4:** A aplicação deve suportar múltiplas enquetes ativas simultaneamente
- **NFR5:** A expiração da enquete deve ocorrer automaticamente na data/hora configurada

---

## 3. User Interface Design Goals

### Overall UX Vision
Interface minimalista e direta — o participante deve conseguir votar em menos de 30 segundos sem qualquer instrução. O criador deve conseguir criar e compartilhar uma enquete em menos de 2 minutos.

### Key Interaction Paradigms
- Formulário linear para criação de enquete (passo a passo)
- Votação com 1 clique (seleção + confirmação)
- Feedback imediato após voto (transição suave para o gráfico)

### Core Screens and Views
1. Landing page / Login (criador)
2. Dashboard do criador (lista de enquetes)
3. Tela de criação de enquete
4. Página pública da enquete (participante vota)
5. Página de resultado (gráfico pós-voto)
6. Página de enquete expirada

### Accessibility
WCAG AA

### Branding
Sem definição prévia — design limpo, neutro, com boa legibilidade.

### Target Platforms
Web Responsivo (desktop + mobile)

---

## 4. Technical Assumptions

### Repository Structure
Monorepo

### Service Architecture
Monolith — Next.js fullstack (App Router + API Routes). Simples, rápido de entregar, ideal para MVP.

### Testing Requirements
Unit + Integration
- Unit: lógica de negócio (controle de voto, expiração)
- Integration: fluxo completo (criar enquete → votar → ver resultado)

### Stack

| Camada | Escolha | Motivo |
|--------|---------|--------|
| Framework | Next.js 14 (App Router) | Web responsivo, SSR, API Routes nativas |
| Banco de dados | Supabase (PostgreSQL) | Auth incluído, realtime, free tier |
| Autenticação | Supabase Auth | Integrado ao banco, suporte a email/OAuth |
| Gráficos | Recharts | Leve, compatível com React, boa DX |
| Estilização | Tailwind CSS | Responsivo por padrão, rápido para prototipar |
| Deploy | Vercel | Zero-config para Next.js |
| Controle de voto | Fingerprint via IP + cookie | Sem cadastro, suficiente para MVP |

---

## 5. Epic List

| # | Título | Goal |
|---|--------|------|
| 1 | Foundation & Autenticação | Infraestrutura do projeto e login do criador |
| 2 | Gestão de Enquetes (Criador) | Criar, listar e gerenciar enquetes com dashboard |
| 3 | Votação Pública (Participante) | Votar via link e visualizar resultado em gráfico |

---

## 6. Epic Details

### Epic 1: Foundation & Autenticação

**Goal:** Estabelecer a infraestrutura do projeto (Next.js + Supabase + Vercel) e implementar autenticação do criador, garantindo uma base sólida e deployável antes de qualquer funcionalidade de negócio.

---

#### Story 1.1 — Setup do Projeto

> Como desenvolvedor, quero o projeto Next.js configurado com Tailwind, Supabase e deploy na Vercel, para que a equipe tenha uma base funcional desde o início.

**Acceptance Criteria:**
1. Projeto Next.js 14 (App Router) inicializado com Tailwind CSS
2. Supabase conectado (variáveis de ambiente configuradas)
3. Deploy automático na Vercel funcionando via push na main
4. Página inicial (canary page) acessível em produção
5. CI com lint e typecheck passando no GitHub Actions

---

#### Story 1.2 — Autenticação do Criador

> Como criador, quero me cadastrar e fazer login com email/senha, para que eu possa acessar minha área exclusiva.

**Acceptance Criteria:**
1. Tela de cadastro com email e senha (validação básica)
2. Tela de login com email e senha
3. Supabase Auth configurado para autenticação por email
4. Redirecionamento para dashboard após login bem-sucedido
5. Logout funcional com redirecionamento para login
6. Rotas protegidas: dashboard inacessível sem autenticação

---

### Epic 2: Gestão de Enquetes (Criador)

**Goal:** Permitir que o criador autenticado crie enquetes completas (título, opções, expiração), visualize-as no dashboard e obtenha o link de compartilhamento.

---

#### Story 2.1 — Criar Enquete

> Como criador, quero criar uma enquete com título, opções e data de expiração, para que eu possa coletar votos do meu público.

**Acceptance Criteria:**
1. Formulário com campo de título (obrigatório)
2. Mínimo 2 opções de voto, máximo 10 (adição/remoção dinâmica)
3. Campo de data e hora de expiração (obrigatório, deve ser futura)
4. Enquete salva no banco de dados vinculada ao criador
5. Link único gerado automaticamente após criação
6. Redirecionamento para dashboard com confirmação de criação

---

#### Story 2.2 — Dashboard do Criador

> Como criador, quero ver todas as minhas enquetes listadas, para que eu possa gerenciá-las e acompanhar seu status.

**Acceptance Criteria:**
1. Lista todas as enquetes do criador autenticado
2. Exibe status de cada enquete: Ativa / Expirada
3. Exibe data de expiração e total de votos de cada enquete
4. Botão para copiar o link único de cada enquete
5. Link para visualizar resultados de cada enquete
6. Estado vazio tratado (mensagem quando não há enquetes)

---

#### Story 2.3 — Resultados da Enquete (Criador)

> Como criador, quero visualizar os resultados da minha enquete em tempo real, para que eu possa acompanhar a votação.

**Acceptance Criteria:**
1. Página de resultado acessível pelo dashboard
2. Exibe título, opções e contagem de votos por opção
3. Gráfico de barras ou pizza com distribuição dos votos (Recharts)
4. Total de votos exibido
5. Status da enquete exibido (Ativa / Expirada)
6. Atualização dos dados ao recarregar a página

---

### Epic 3: Votação Pública (Participante)

**Goal:** Permitir que qualquer pessoa acesse a enquete via link, vote uma única vez e visualize o resultado em gráfico — sem necessidade de cadastro.

---

#### Story 3.1 — Página Pública da Enquete

> Como participante, quero acessar a enquete pelo link e ver as opções disponíveis, para que eu possa votar.

**Acceptance Criteria:**
1. Página acessível via link único sem autenticação
2. Exibe título da enquete e todas as opções de voto
3. Exibe data de expiração da enquete
4. Enquete expirada exibe mensagem de encerramento (sem opções de voto)
5. Enquete inexistente exibe página de erro amigável

---

#### Story 3.2 — Votação com Controle de Duplicidade

> Como participante, quero votar em uma opção, para que minha preferência seja registrada uma única vez.

**Acceptance Criteria:**
1. Participante seleciona uma opção e confirma o voto
2. Voto registrado no banco de dados vinculado à enquete
3. Controle de duplicidade: cookie + IP impedem segundo voto
4. Tentativa de segundo voto exibe mensagem informativa
5. Não é possível votar em enquete expirada (validação no backend)

---

#### Story 3.3 — Gráfico de Resultado Pós-Voto

> Como participante, quero ver o gráfico de votos após votar, para que eu saiba como está a distribuição das respostas.

**Acceptance Criteria:**
1. Após votar, participante é redirecionado automaticamente para tela de resultado
2. Gráfico (barras ou pizza) exibe distribuição de votos por opção
3. Total de votos e percentual por opção exibidos
4. Opção escolhida pelo participante destacada no gráfico
5. Resultado final exibido também quando a enquete está expirada

---

## 7. Next Steps

### UX Expert Prompt
> `@ux-design-expert` — revisar o PRD em `docs/prd/prd.md` e criar o design system e wireframes para as 6 telas principais da aplicação de enquetes.

### Architect Prompt
> `@architect` — revisar o PRD em `docs/prd/prd.md` e criar a arquitetura técnica para a aplicação: Next.js 14 + Supabase + Vercel, incluindo schema do banco de dados, estrutura de rotas e estratégia de controle de voto único por participante.
