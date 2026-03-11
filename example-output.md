# Exemplo de Output: Plano de Desenvolvimento

Este é um exemplo de como o arquivo será gerado pela skill `plan-stories`.

Para o goal **"implementar autenticação JWT no backend"**, o arquivo gerado seria:  
**`.context/jwt-autenticacao-backend-plan.md`**

---

# Development Plan: Implementar Sistema de Autenticação JWT

**Generated**: 2026-03-11T14:30:00Z  
**Goal**: Implementar autenticação JWT no backend com login, registro e proteção de rotas

## Context Summary

Projeto Node.js/Express com TypeScript. Stack atual inclui PostgreSQL via Prisma ORM. Estrutura segue padrão MVC com controllers em `/src/controllers`, models em `/src/models`, e middlewares em `/src/middlewares`. Já existe configuração de banco de dados em `/src/config/database.ts`. Projeto usa `bcryptjs` para hash de senhas (já instalado).

## User Stories

```json
[
  {
    "id": "US-001",
    "title": "Criar modelo User e schema do banco",
    "description": "Como desenvolvedor, quero um modelo de usuário com campos necessários para autenticação para poder armazenar dados de login de forma segura.",
    "acceptanceCriteria": [
      "Schema Prisma criado com campos: id (UUID), email (unique), password_hash, name, created_at, updated_at",
      "Migration gerada e aplicada com sucesso",
      "Model TypeScript criado em /src/models/User.ts com métodos de instância",
      "Validação de email implementada (formato válido)",
      "Senha armazenada como hash (bcryptjs)"
    ],
    "priority": 1,
    "complexity": "S",
    "dependencies": [],
    "filesToTouch": [
      "prisma/schema.prisma",
      "src/models/User.ts",
      "prisma/migrations/*/migration.sql"
    ],
    "notes": "Usar UUID para ID. Seguir padrão de models existentes no projeto. Campo password_hash, nunca password em plain text."
  },
  {
    "id": "US-002",
    "title": "Criar endpoint de registro de usuários",
    "description": "Como usuário, quero me registrar com email e senha para poder acessar a aplicação.",
    "acceptanceCriteria": [
      "POST /api/auth/register recebe email, password, name",
      "Validação de entrada: email válido, password >= 8 caracteres, name não vazio",
      "Verificação de email único (retorna 409 se já existe)",
      "Senha hasheada antes de salvar no banco",
      "Retorna 201 com dados do usuário criado (sem password_hash)",
      "Retorna 400 para dados inválidos com mensagens claras"
    ],
    "priority": 2,
    "complexity": "M",
    "dependencies": ["US-001"],
    "filesToTouch": [
      "src/controllers/AuthController.ts",
      "src/routes/auth.routes.ts",
      "src/validators/auth.validator.ts"
    ],
    "notes": "Usar biblioteca `express-validator` para validação (já instalada). Seguir padrão de resposta JSON existente na aplicação."
  },
  {
    "id": "US-003",
    "title": "Configurar JWT e criar utilitários",
    "description": "Como desenvolvedor, quero utilitários para gerar e verificar tokens JWT para poder implementar autenticação stateless.",
    "acceptanceCriteria": [
      "Pacote jsonwebtoken instalado",
      "Função generateToken(userId) retorna JWT válido com expiração de 24h",
      "Função verifyToken(token) retorna payload decodificado ou null se inválido",
      "Secret key carregada de variável de ambiente JWT_SECRET",
      "Tipos TypeScript definidos para payload do JWT",
      "Testes unitários para ambas as funções"
    ],
    "priority": 3,
    "complexity": "S",
    "dependencies": [],
    "filesToTouch": [
      "src/utils/jwt.ts",
      "src/types/auth.d.ts",
      ".env.example"
    ],
    "notes": "Adicionar JWT_SECRET ao .env.example. Considerar refresh tokens em futura iteração se necessário."
  },
  {
    "id": "US-004",
    "title": "Criar endpoint de login",
    "description": "Como usuário, quero fazer login com email e senha para receber um token de acesso.",
    "acceptanceCriteria": [
      "POST /api/auth/login recebe email e password",
      "Verifica se usuário existe (404 se não encontrado)",
      "Compara senha com bcrypt.compare (401 se inválida)",
      "Gera token JWT com userId no payload",
      "Retorna 200 com token e dados básicos do usuário",
      "Retorna 401 para credenciais inválidas (mensagem genérica por segurança)"
    ],
    "priority": 4,
    "complexity": "M",
    "dependencies": ["US-001", "US-003"],
    "filesToTouch": [
      "src/controllers/AuthController.ts",
      "src/routes/auth.routes.ts"
    ],
    "notes": "Não revelar se email existe ou senha está errada (mensagem genérica: 'Credenciais inválidas'). Rate limiting será implementado em story futura."
  },
  {
    "id": "US-005",
    "title": "Criar middleware de autenticação",
    "description": "Como desenvolvedor, quero um middleware para proteger rotas privadas garantindo que apenas usuários autenticados acessem.",
    "acceptanceCriteria": [
      "Middleware authMiddleware extrai token do header Authorization (Bearer token)",
      "Verifica token JWT e decodifica userId",
      "Busca usuário no banco e anexa a req.user",
      "Retorna 401 se token ausente",
      "Retorna 401 se token inválido ou expirado",
      "Retorna 404 se usuário não encontrado (token válido mas usuário deletado)",
      "Tipagem do Request estendida para incluir user"
    ],
    "priority": 5,
    "complexity": "M",
    "dependencies": ["US-003"],
    "filesToTouch": [
      "src/middlewares/auth.ts",
      "src/types/express.d.ts"
    ],
    "notes": "Estender namespace Express para adicionar user ao Request. Middleware deve ser reutilizável em qualquer rota."
  },
  {
    "id": "US-006",
    "title": "Criar endpoint de perfil do usuário",
    "description": "Como usuário autenticado, quero ver meus dados de perfil para confirmar minhas informações.",
    "acceptanceCriteria": [
      "GET /api/users/me retorna dados do usuário logado",
      "Rota protegida pelo authMiddleware",
      "Retorna 200 com id, email, name, created_at",
      "Não retorna password_hash",
      "Retorna 401 se não autenticado"
    ],
    "priority": 6,
    "complexity": "XS",
    "dependencies": ["US-005"],
    "filesToTouch": [
      "src/controllers/UserController.ts",
      "src/routes/user.routes.ts"
    ],
    "notes": "Rota simples para validar que o middleware de autenticação está funcionando. Usar req.user populado pelo middleware."
  }
]
```

## Implementation Notes

- **Bibliotecas necessárias**: jsonwebtoken (não instalada ainda), bcryptjs (já instalada)
- **Segurança**: Senhas sempre hasheadas, tokens com expiração, mensagens de erro genéricas em autenticação
- **Testes**: Criar testes unitários para JWT utils e testes de integração para endpoints
- **Documentação**: Atualizar README.md com instruções de autenticação
- **Variáveis de ambiente**: Adicionar JWT_SECRET ao .env
- **Próximos passos futuros** (não incluídos neste plano): Refresh tokens, logout (blacklist), rate limiting, 2FA

## Definition of Done

- [ ] Todas as stories implementadas e testadas
- [ ] Testes unitários para funções de JWT
- [ ] Testes de integração para endpoints de auth
- [ ] Documentação da API atualizada (endpoints, headers, respostas)
- [ ] Variáveis de ambiente documentadas
- [ ] Code review realizado
- [ ] Nenhum secret hardcoded no código

---

## Como Usar Este Plano

1. **Localize** o arquivo gerado em `.context/[nome-da-tarefa]-plan.md`
2. **Implemente** seguindo a ordem das priorities (US-001 → US-006)
3. **Verifique** os acceptance criteria antes de considerar cada story pronta
4. **Marque** as checkboxes na Definition of Done conforme avança
5. **Atualize** este arquivo se encontrar novas dependências ou requisitos durante a implementação

### Comandos Úteis

```bash
# Listar todos os planos
ls -la .context/*-plan.md

# Ver o último plano criado
ls -lt .context/*-plan.md | head -1
```
