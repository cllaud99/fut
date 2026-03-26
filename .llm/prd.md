# PRD — Fut App
**Data:** 2026-03-25

## Visão Geral

Aplicativo web responsivo para grupos de amigos acompanharem estatísticas de futebol. Inspirado no GymRats, permite criar grupos com desafios/metas, registrar partidas, contestar eventos via VAR e votar nos troféus MVP e Framboesa.

---

## Stack

| Camada | Tecnologia |
|---|---|
| Frontend | Next.js (App Router) + shadcn/ui + Tailwind |
| Backend | FastAPI + SQLAlchemy + JWT |
| Banco | PostgreSQL + Row Level Security (RLS) |
| Deploy | Vercel (frontend) + Render/Railway (backend) + Supabase/Neon (banco) |

Comunicação via REST/JSON. Sem WebSocket — atualizações via polling leve (10s) ou refresh manual.

Referência visual: https://xperiun.com/

---

## Arquitetura

```
Next.js (Vercel)
  └── REST/JSON
      └── FastAPI (Render/Railway)
              └── PostgreSQL (Supabase/Neon)
```

---

## Modelo de Dados

Todas as entidades compartilham as mesmas tabelas, isoladas por `group_id` com RLS ativo no Supabase.

- **users** — id, nome, email, senha_hash, avatar, criado_em
- **groups** — id, nome, descricao, criado_por, criado_em
- **group_members** — user_id, group_id, papel (`jogador` | `juiz`)
- **challenges** — id, group_id, tipo (`meta_vitorias` | `por_tempo`), valor (int ou date), encerrado_em
- **matches** — id, group_id, data, criado_por
- **match_events** — id, match_id, user_id, tipo (`gol` | `passe` | `resultado`), valor, criado_por
- **var_contests** — id, event_id, contestado_por, motivo, status (`aberto` | `aprovado` | `rejeitado`), resolvido_por
- **votes** — id, match_id, votante_id, votado_id, tipo (`mvp` | `framboesa`)

Estatísticas calculadas on-the-fly via queries agregadas — sem tabela de stats separada.

---

## Funcionalidades

### Autenticação
- Cadastro e login com email + senha
- JWT armazenado em cookie httpOnly
- Planejado: OAuth (Google) em versão futura

### Perfil Público
- Avatar, nome
- Estatísticas gerais: gols, passes, partidas, vitórias, derrotas, empates
- Troféus acumulados: MVP e Framboesa
- Histórico de grupos participados

### Grupos
- Criar grupo e convidar membros por link
- Definir desafio:
  - `meta_vitorias`: primeiro a atingir X vitórias vence
  - `por_tempo`: grupo encerra automaticamente na data Y
- Membros têm papel `jogador` ou `juiz`
- Placar do desafio atualizado a cada 10s

### Registro de Partida
- **Juiz** pode lançar eventos de qualquer jogador
- **Jogador** só lança seus próprios eventos
- Tipos de evento: `gol`, `passe`, `resultado` (vitória/derrota/empate)

### VAR — Contestação
- Se jogador A lança evento de jogador B, B pode clicar em "Chamar no VAR"
- Abre modal com campo de motivo
- Evento fica com status `em_contestacao` até resolução
- Juiz do grupo decide: `aprovado` (evento mantido) ou `rejeitado` (evento removido)

### Votação Pós-Partida
- Ao encerrar a partida, abre janela de votação com duração de 24h
- Cada membro vota em um MVP e um Framboesa
- Vencedor por maioria simples
- Troféus são acumulados no perfil público do jogador

---

## Telas

1. Login / Cadastro
2. Home — feed de grupos do usuário
3. Perfil público — stats + troféus + histórico
4. Grupo — placar do desafio + lista de partidas + membros
5. Partida — timeline de eventos + botão VAR + votação
6. Criar grupo / configurar desafio

---

## Plano de Implementação

### Fase 0 — Mockup Visual
- Criar mockups das telas principais (HTML/CSS estático ou Figma-like)
- Telas: Login, Home, Grupo, Partida, Perfil Público
- Referência: xperiun.com
- Aprovação antes de iniciar o desenvolvimento

### Fase 1 — Setup
- Estrutura do monorepo: `/backend` (FastAPI) e `/frontend` (Next.js)
- Configurar PostgreSQL no Supabase + RLS
- Configurar variáveis de ambiente

### Fase 2 — Backend
- Models SQLAlchemy (todas as tabelas snake_case)
- Migrations com Alembic
- Auth: cadastro, login, JWT
- CRUD de grupos e membros
- CRUD de partidas e eventos
- Endpoint VAR (criar contestação, resolver)
- Endpoint votação (abrir, votar, encerrar)
- Endpoints de estatísticas e perfil público

### Fase 3 — Frontend
- Setup Next.js + shadcn/ui + Tailwind (referência xperiun.com)
- Telas de login/cadastro
- Home — feed de grupos
- Tela de grupo — placar + partidas
- Tela de partida — eventos + VAR + votação
- Perfil público
- Responsividade mobile

### Fase 4 — Deploy
- Deploy backend no Render/Railway
- Deploy frontend na Vercel
- Conectar variáveis de ambiente de produção

---

## Critérios de Sucesso (MVP)

- Usuário consegue se cadastrar, criar um grupo e convidar amigos
- Partida pode ser registrada com gols, passes e resultado
- Mecanismo de VAR funciona: contestação criada, juiz resolve
- Votação de MVP e Framboesa funciona ao fim de uma partida
- Perfil público exibe stats corretas
- Layout responsivo funciona em desktop e mobile
