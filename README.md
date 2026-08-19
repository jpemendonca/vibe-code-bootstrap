# Vibe-Code Bootstrap

Um método genérico para bootstrappar projetos de software do zero (qualquer linguagem, qualquer
tipo: app, bot, API, CLI, lib) seguindo o esquema de desenvolvimento 100% orientado a agente
("vibe code") validado no projeto WorkoutApp.

## O que é

Vibe-code é um método que torna um projeto executável por um agente de código do início ao fim —
inclusive por um modelo mais barato rodando sozinho num loop depois que a fundação está pronta.
A fundação é um pequeno conjunto de documentos que especificam produto, arquitetura, estilo de
código, e um fila ordenada de tarefas (ROADMAP), cada uma = um commit.

Este repositório contém:

- **`skills/bootstrap-projeto/SKILL.md`**: Uma skill para Claude Code (`/bootstrap-projeto`) que
  gera a fundação documental de um projeto novo. Pergunta contexto do projeto (tipo, stack,
  nível de rigor de testes, nível de disciplina de segredos) e cria os arquivos adaptados.

- **`vibe-coding-project-bootstrap.md`**: O mesmo método em formato de prompt autocontido —
  cole em qualquer conversa nova com um agente de código, no diretório vazio do projeto.

## Como usar

### Opção 1: Skill do Claude Code (recomendado se você tiver a skill instalada)

Coloque a skill em seu ambiente:

```bash
mkdir -p ~/.claude/skills/bootstrap-projeto
cp skills/bootstrap-projeto/SKILL.md ~/.claude/skills/bootstrap-projeto/
```

Depois, em qualquer novo projeto (dir vazio ou quase vazio), abra Claude Code e rode:

```
/bootstrap-projeto
```

A skill vai:
1. Perguntar o contexto do projeto (produto, stack, tipo, nível de rigor);
2. Gerar `AGENTS.md`, `SPECS.md`, `CODESTYLE.md`, `ROADMAP.md`, `README.md` adaptados;
3. Configurar gate de qualidade (hook/script equivalente ao comando nativo da linguagem);
4. Listar o que você precisa preencher manualmente (segredos, `.env`, etc.).

### Opção 2: Prompt autocontido (sem skill)

Copie o arquivo `vibe-coding-project-bootstrap.md`, coloque-o em algum lugar acessível, e cole
todo o conteúdo como a primeira mensagem numa conversa nova com Claude Code (ou outro agente) no
diretório do novo projeto.

## O método em 30 segundos

1. **Documentos**: `AGENTS.md` (canônico) → `SPECS.md` (produto/arquitetura/decisões) →
   `CODESTYLE.md` (regras verificáveis no diff) → `ROADMAP.md` (fila de tarefas) → `README.md`
   (humano).

2. **Loop**: Pegar primeiro bullet não marcado do ROADMAP → implementar com teste → rodar gate
   de qualidade → marcar bullet → commit em Conventional Commits → repetir.

3. **Regras duras**:
   - Agente nunca escreve valor real de segredo.
   - Uma mudança lógica = um commit.
   - Bullet bloqueado fica não marcado com nota de bloqueio (nunca redefinido em silêncio).
   - Gate de qualidade nunca é contornado.

4. **Flexibilidade**: O que é "nível de rigor de testes" e "nível de disciplina de segredos"
   são perguntas — você escolhe por projeto. PoC? Pode ser "sem teste formal" + ".env simples".
   Produto? "Teste com dependência real" + "esquema formal de segredos".

## Origem

Extraído e generalizado do projeto [WorkoutApp](https://github.com/jpemendonca/WorkoutApp), que
validou esse método de ponta a ponta em um projeto .NET + Expo. O método é agnóstico de
linguagem/stack — funciona pra Go, Python, JS, Rust, C#, o que você quiser.

## Camada opcional: Ponytail

O repositório sugere (opcional) o plugin
[Ponytail](https://github.com/DietrichGebert/ponytail), que aplica uma "escada de decisão"
antes de gerar código: prefira não escrever nada → reusar código → stdlib/nativo →
código custom. Reduz bloat sem sacrificar segurança.

Instale com:

```bash
claude plugin marketplace add DietrichGebert/ponytail
claude plugin install ponytail@ponytail
```

Use com `/ponytail lite|full|ultra` durante a execução do ROADMAP.

## Referência de contexto

- **WorkoutApp (origem)**: https://github.com/jpemendonca/WorkoutApp
- **Ponytail (plugin complementar)**: https://github.com/DietrichGebert/ponytail

---

Comece com `/bootstrap-projeto` (se tiver a skill) ou copie `vibe-coding-project-bootstrap.md`
como prompt inicial.
