# CuidaMais

Prontuário eletrônico para **monitoramento domiciliar de idosos**. O cuidador registra sinais vitais e
sintomas no dia a dia; o sistema organiza o histórico, mostra tendências em gráficos, gera **alertas
clínicos** quando algum valor sai da faixa e monta um relatório consolidado para levar à consulta.

> **Status:** protótipo funcional. Projeto de extensão universitária da Universidade Católica de
> Brasília (UCB), desenvolvido em equipe de 7 estudantes entre 29/05 e 03/06/2026 e validado com
> **dados fictícios**. Fui responsável pelo desenvolvimento da aplicação (todos os commits deste
> repositório). O relatório completo do projeto está em [`docs/`](docs/).

## Funcionalidades

- **Cadastro e login** de cuidadores, com senha protegida por bcrypt e sessão em JWT num cookie HttpOnly.
- **Idosos assistidos:** cada cuidador vê e edita apenas os seus pacientes (todas as rotas filtram pelo dono).
- **Registro diário:** pressão arterial, glicemia, frequência cardíaca, oxigenação, temperatura, peso,
  dor (nível e local), fadiga e tontura.
- **Alertas clínicos** a partir do último registro (ex.: pressão sistólica ≥ 140 ou ≤ 90 mmHg).
- **Gráficos de tendência** e **histórico** do prontuário.
- **Relatório consolidado** para impressão.
- **Compartilhamento** do prontuário por link, para familiares ou profissionais de saúde.

## Tecnologias

Next.js (App Router, Route Handlers) · React · TypeScript · Prisma ORM · SQLite · Tailwind CSS ·
Recharts · bcryptjs · JWT (`jose`)

## Como rodar

Requer Node.js 20+.

```bash
git clone https://github.com/leoesquivelcouto/cuida-mais.git
cd cuida-mais
npm install
cp .env.example .env        # e troque o JWT_SECRET por um valor forte
npm run db:setup            # cria o banco SQLite local a partir do schema
npm run dev                 # http://localhost:3000
```

O banco (`*.db`) e o `.env` ficam fora do git.

## Arquitetura

```
src/
  app/
    api/auth/                  cadastro, login e logout
    api/dashboard/records/     pacientes, registros, histórico, alertas, estatísticas e relatório
    dashboard/                 páginas: idosos, registro, histórico, gráficos, alertas, relatório
    share/[id]/                visualização do prontuário por link
  lib/auth.ts                  hash de senha, emissão e validação do JWT, sessão obrigatória
  lib/prisma.ts                cliente Prisma
prisma/schema.prisma           modelos User, Patient e Record
```

## Cuidados com dados de saúde

Sinais vitais são dados sensíveis pela LGPD. No protótipo:

- o segredo do JWT vem apenas de variável de ambiente, e a aplicação não sobe sem ele;
- as senhas são guardadas com hash bcrypt;
- as rotas do painel exigem sessão e só retornam pacientes do próprio cuidador;
- o repositório não contém banco de dados nem dados reais (o histórico foi limpo antes da publicação).

## Limitações conhecidas

- **O link de compartilhamento não exige login:** quem tiver a URL vê o prontuário. Próximo passo: links
  com token aleatório, expiração e possibilidade de revogar.
- As regras de alerta usam faixas fixas; não há ajuste por paciente nem validação por profissional de saúde.
- SQLite atende ao protótipo; para uso real com vários cuidadores, migrar para PostgreSQL.
- Sem testes automatizados ainda.

## Licença

MIT
