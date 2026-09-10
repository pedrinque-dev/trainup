# TrainUp — Serviços para Personal Trainer

Sistema web para gestão de atendimentos de personal trainers: cadastro de alunos, montagem de treinos, registro de evolução física/desempenho, geração de relatórios e organização de agenda.

Projeto desenvolvido para a disciplina **Projeto Integrador Interdisciplinar II** — Curso Superior de Tecnologia em Análise e Desenvolvimento de Sistemas, FACULDADE DE TECNOLOGIA SENAI FÉLIX GUISARD (2º semestre/2026).

## Equipe

**Nome da equipe:** [preencher]

| Integrante |
|---|
| Ana Carolina Gonçalves |
| Bárbara Yasmin Pimenta dos Santos Tobias |
| Luana Gabrielle Ferreira Guedes Paes |
| Pedro Augusto Lombardi da Costa |
| Ruan Monteiro Brito |

**Professores orientadores:** Wesley Fioreze, Marcello Benevides

## 1. Problema

Personal trainers autônomos e pequenos estúdios administram alunos, treinos e evolução física de forma manual (planilhas, papel ou aplicativos de mensagem), o que dificulta o histórico de acompanhamento, a geração de relatórios de evolução e a organização da agenda de atendimentos, gerando retrabalho e perda de informação.

**Usuários e stakeholders:** personal trainers autônomos (usuário principal), alunos/clientes, e indiretamente estúdios/academias que empregam o profissional.

## 2. Objetivo da solução

Oferecer uma plataforma web onde o personal trainer cadastra alunos, monta treinos, registra a evolução (peso, carga, desempenho, frequência) e gera relatórios automáticos a partir de dados reais, enquanto o aluno acompanha sua evolução e agenda atendimentos.

**Fluxo central de valor (núcleo do MVP):**

```
Personal cadastra aluno → cria treino → registra evolução →
histórico é armazenado → dados são analisados → relatório é gerado →
aluno consulta seu relatório e sua evolução
```

Login e cadastro existem apenas para viabilizar esse fluxo — não são o foco da avaliação.

## 3. Escopo

**Incluído no MVP:** autenticação, cadastro de aluno, treinos, registro e consulta de evolução, indicadores calculados a partir de dados reais, relatório de evolução, cadastro de serviços, disponibilidade de agenda e agendamentos.

**Desejável:** dashboard consolidado (agregação de indicadores já existentes).

**Fora do escopo:** pagamentos, marketplace, app mobile nativo, integração com smartwatch/equipamento real, chat em tempo real, geração automática de treino por IA, sistema nutricional completo.

## 4. Arquitetura

```
React + Vite + TypeScript + Tailwind CSS  (front-end)
              │  HTTP / REST (JWT)
              ▼
FastAPI (Python) — back-end em camadas
   api (rotas) → services (regras) → repositories (dados) → models
              │
              ▼
        PostgreSQL (persistência)
```

- **Front-end:** React 19, Vite, TypeScript, Tailwind CSS, React Router, Axios, Recharts (gráficos), lucide-react (ícones).
- **Back-end:** FastAPI, SQLAlchemy (ORM), Alembic (migrations), Pydantic (validação), JWT + passlib (autenticação/hash de senha).
- **Banco de dados:** PostgreSQL.
- **Perfis de usuário:** `PERSONAL` e `ALUNO`, com controle de acesso validado sempre no back-end.

## 5. Estrutura do repositório

```
trainup/
├── backend/
│   ├── app/
│   │   ├── api/            # Rotas FastAPI (um arquivo por módulo)
│   │   ├── core/           # Config, conexão com banco, segurança/JWT
│   │   ├── models/         # Entidades SQLAlchemy (ORM)
│   │   ├── schemas/        # Schemas Pydantic (entrada/saída da API)
│   │   ├── services/       # Regras de negócio
│   │   └── repositories/   # Acesso a dados (queries)
│   ├── migrations/         # Alembic (versionamento do schema)
│   └── seeds/              # Dados fictícios de demonstração
├── frontend/
│   └── src/{components,pages,layouts,services,hooks,types}/
├── docs/
│   ├── ERS_TrainUp.pdf
│   ├── diagramas/
│   ├── prototipo/
│   └── atas/
└── README.md
```

## 6. Como executar localmente

Pré-requisitos: Python 3.11+, Node.js 18+, PostgreSQL 14+.

### Back-end

```bash
cd backend
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt
cp .env.example .env             # ajustar DATABASE_URL e SECRET_KEY
alembic upgrade head
python -m seeds.seed_data        # popula dados fictícios de demonstração
uvicorn app.main:app --reload
```

Back-end disponível em `http://localhost:8000` (documentação interativa em `http://localhost:8000/docs`).

### Front-end

```bash
cd frontend
npm install
cp .env.example .env             # ajustar VITE_API_URL
npm run dev
```

Front-end disponível em `http://localhost:5173`.

### Credenciais de demonstração

| Perfil | E-mail | Senha |
|---|---|---|
| Personal Trainer | personal@trainup.com | demo1234 |
| Aluno | ana@trainup.com | demo1234 |

## 7. Dados

Todos os dados usados em demonstração são **fictícios**, nunca dados reais de terceiros. Registros de evolução possuem o campo `origem` (`manual` ou `simulado`), nunca apresentados como coletados de equipamento real.

## 8. Documentação

- ERS completa: `docs/ERS_TrainUp.pdf`
- Diagramas (casos de uso, classes, DER, sequência): `docs/diagramas/`
- Protótipo das telas: `docs/prototipo/`
- Atas de reunião: `docs/atas/`

## 9. Licença

Projeto acadêmico — Projeto Integrador Interdisciplinar II, ADS, SENAI Félix Guisard, 2º semestre de 2026.