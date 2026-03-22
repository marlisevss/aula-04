# Project Brief — Aplicação de Enquetes

**Versão:** 1.0
**Data:** 2026-03-22
**Autor:** Atlas (@analyst)
**Status:** Draft — pronto para PRD

---

## 1. Visão Geral

Aplicação web responsiva para criação e participação em enquetes públicas. O criador define o nome da enquete, as opções de voto e uma data de expiração. Participantes acessam via link único sem necessidade de cadastro. Após votar, visualizam um gráfico com o resultado parcial. O criador possui dashboard autenticado para gerenciar suas enquetes.

---

## 2. Problema

Criar enquetes rápidas e compartilháveis é hoje fragmentado: ferramentas existentes exigem cadastro dos participantes, não exibem resultados em tempo real ou não possuem controle de expiração. Há necessidade de uma solução simples, acessível e com UX focada no participante.

---

## 3. Público-Alvo

- **Criadores:** Qualquer pessoa que deseja coletar opinião de um grupo (equipes, comunidades, redes sociais)
- **Participantes:** Público geral — acesso via link, sem cadastro obrigatório

---

## 4. Requisitos Funcionais

### Criador (autenticado)
- [ ] Cadastro e login (autenticação)
- [ ] Dashboard com listagem de enquetes criadas
- [ ] Criar enquete: título, opções de voto (mínimo 2), data de expiração
- [ ] Compartilhar link único da enquete
- [ ] Visualizar resultados em tempo real no dashboard

### Participante (anônimo via link)
- [ ] Acessar enquete via link único sem cadastro
- [ ] Votar em uma opção (1 voto por participante)
- [ ] Após votar: ver gráfico com resultado atual dos votos
- [ ] Enquete expirada: exibir mensagem de encerramento + resultados finais

---

## 5. Requisitos Não Funcionais

- **1 voto por participante:** Controle por fingerprint de dispositivo/IP (sem cadastro)
- **Expiração:** Enquete fecha automaticamente na data/hora definida pelo criador
- **Responsivo:** Funciona em desktop e mobile (web)
- **Gráfico:** Exibição visual dos votos após votação (barras ou pizza)

---

## 6. Fora do Escopo (v1)

- App nativo iOS/Android
- Enquetes privadas com senha
- Comentários nos votos
- Integração com redes sociais
- Exportação de dados

---

## 7. Fluxo Principal

```
Criador                          Participante
  │                                  │
  ├─ Login/cadastro                  │
  ├─ Cria enquete                    │
  ├─ Obtém link único ──────────────►├─ Acessa link
  │                                  ├─ Vê opções de voto
  │                                  ├─ Vota (1x por device)
  │                                  └─ Vê gráfico de resultados
  └─ Acompanha no dashboard
```

---

## 8. Stack Sugerida (a confirmar com @architect)

| Camada | Sugestão |
|--------|----------|
| Frontend | Next.js (web responsivo) |
| Backend | Node.js / API REST ou Next.js API Routes |
| Banco de dados | PostgreSQL (Supabase) |
| Autenticação | Supabase Auth |
| Gráficos | Chart.js ou Recharts |
| Deploy | Vercel / Railway |

---

## 9. Métricas de Sucesso

- Criador consegue criar e compartilhar enquete em < 2 minutos
- Participante consegue votar em < 30 segundos
- Gráfico carrega em < 1 segundo após voto
- Controle de 1 voto por participante funciona em 100% dos casos

---

## 10. Próximos Passos

1. **@pm** — Criar PRD detalhado a partir deste brief
2. **@architect** — Validar stack e definir arquitetura
3. **@sm** — Criar primeiras stories do backlog
