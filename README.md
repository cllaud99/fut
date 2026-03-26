# ½ Fut App

Sistema de acompanhamento de estatísticas de futebol para grupos de amigos.

## <¨ Mockups Visuais - Fase 0

Esta é a fase 0 do projeto, contendo os mockups visuais de todas as telas do aplicativo.

### =ñ Telas disponíveis:

- **Login/Cadastro** - Autenticação de usuários
- **Home** - Feed de grupos do usuário
- **Grupo** - Detalhes do grupo com placar, partidas e membros
- **Agenda** - Partidas futuras com sistema de confirmação de presença
- **Partida** - Timeline de eventos, sistema VAR e votação MVP/Framboesa
- **Perfil Público** - Estatísticas, troféus e histórico do jogador

### =€ Ver mockups localmente:

```bash
# Navegar até a pasta de mockups
cd .llm/mockups

# Iniciar servidor HTTP
python3 -m http.server 8000

# Acessar no navegador
# http://localhost:8000
```

### < Deploy no Vercel:

Os mockups estão hospedados no Vercel e podem ser acessados online.

## =Ë Stack Tecnológica (Planejado)

| Camada | Tecnologia |
|--------|-----------|
| Frontend | Next.js (App Router) + shadcn/ui + Tailwind |
| Backend | FastAPI + SQLAlchemy + JWT |
| Banco | PostgreSQL + Row Level Security (RLS) |
| Deploy | Vercel (frontend) + Render/Railway (backend) + Supabase/Neon (banco) |

## <¯ Funcionalidades Principais

-  Criar e gerenciar grupos de futebol
-  Registrar partidas e eventos (gols, assistências)
-  Sistema VAR para contestação de eventos
-  Votação de MVP e Framboesa pós-partida
-  Agendar partidas futuras com confirmação de presença
-  Estatísticas detalhadas por jogador
-  Troféus e conquistas
-  Desafios entre jogadores (meta de vitórias ou por tempo)

## =Ö Documentação

Veja o [PRD completo](.llm/prd.md) para mais detalhes sobre o projeto.

## <¨ Design

- **Paleta de cores**: Dark theme com verde/emerald como cor principal
- **Componentes**: Tailwind CSS + gradientes + glassmorphism
- **Responsividade**: Mobile-first design
- **Inspiração**: Apps esportivos modernos

---

**Status**: <¨ Fase 0 - Mockups Visuais  Completo

**Próximo passo**: Fase 1 - Setup do projeto (Backend + Frontend)
