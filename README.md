# Vibe-Code Bootstrap

Um método genérico para bootstrappar projetos de software do zero (qualquer linguagem, qualquer
tipo: app, bot, API, CLI, lib) seguindo um esquema de desenvolvimento 100% orientado a agente
("vibe code").

## O que é

Vibe-code é um método que torna um projeto executável por um agente de código do início ao fim —
inclusive por um modelo mais barato rodando sozinho num loop depois que a fundação está pronta.
A fundação é um pequeno conjunto de documentos que especificam produto, arquitetura, estilo de
código, e uma fila ordenada de tarefas (ROADMAP), cada uma = um commit.

Este repositório contém:

- **`skills/bootstrap-projeto/SKILL.md`**: Uma skill para Claude Code (`/bootstrap-projeto`) que
  gera a fundação documental de um projeto novo. Pergunta contexto do projeto (caminho principal,
  tipo, stack, origem dos dados, nível de rigor de testes, nível de disciplina de segredos) e
  cria os arquivos adaptados.

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
1. Perguntar o contexto do projeto (produto, caminho principal, stack, origem dos dados, rigor);
2. Gerar `AGENTS.md`, `SPECS.md`, `CODESTYLE.md`, `ROADMAP.md`, `README.md` adaptados, mais o
   ponteiro `CLAUDE.md`;
3. Configurar gate de qualidade (hook/script equivalente ao comando nativo da linguagem);
4. Listar o que você precisa preencher manualmente (segredos, `.env`, etc.).

### Opção 2: Prompt autocontido (sem skill)

Copie o arquivo `vibe-coding-project-bootstrap.md`, coloque-o em algum lugar acessível, e cole
todo o conteúdo como a primeira mensagem numa conversa nova com Claude Code (ou outro agente) no
diretório do novo projeto.

## O método em 30 segundos

1. **Documentos**: `AGENTS.md` (canônico) → `SPECS.md` (produto/arquitetura/decisões) →
   `CODESTYLE.md` (regras verificáveis no diff) → `ROADMAP.md` (fila de tarefas) → `README.md`
   (humano). `CLAUDE.md` (e `GEMINI.md`, se for o caso) nascem junto, com uma única linha
   `@AGENTS.md` — ponteiro, nunca cópia.

2. **Ordem**: Fase 0 é fundação. **Fase 1 entrega o caminho principal inteiro, ponta a ponta** —
   feio, mas percorrível. Ampliação depois, periféricas (tema, preferências, telas de apoio) por
   último. Nenhuma fase além da 0 entrega só infraestrutura.

3. **Loop**: Pegar primeiro bullet não marcado do ROADMAP → implementar com teste → rodar gate
   de qualidade → marcar bullet → commit em Conventional Commits → repetir.

4. **Regras duras**:
   - Toda mudança vira bullet no ROADMAP antes de virar código, inclusive correção pedida
     depois de testar. Só escapa o que não altera comportamento (digitação, formatação, link).
   - Uma mudança lógica = um commit.
   - Bullet bloqueado fica não marcado com nota de bloqueio (nunca redefinido em silêncio).
   - Gate de qualidade nunca é contornado.
   - Nada entra sem quem chame — teste não conta como chamador.

5. **Flexibilidade**: O que é "nível de rigor de testes" e "nível de disciplina de segredos"
   são perguntas — você escolhe por projeto. PoC? Pode ser "sem teste formal" + ".env simples".
   Produto? "Teste com dependência real" + "esquema formal de segredos".

## Não confunda código escrito com caminho validado

O gate de qualidade não abre o produto. Ele não percorre o caminho principal e não prova que os
dados chegam de verdade. Gate verde é condição necessária, não prova de que existe produto.

Por isso a "definição de pronto" do ROADMAP separa duas coisas: *código escrito* (build, lint,
teste, commit) e *caminho percorrido* (alguém abriu e usou). Percorrer não precisa ser
obrigatório — mas o bullet declara qual dos dois aconteceu.

A skill traz uma seção de **erros que este método já cometeu**, com casos reais. O principal:
um app organizado por camada técnica fechou 80 bullets com gate verde e 232 testes passando, sem
nenhum caminho para o usuário fazer a única coisa que o produto prometia. Os testes passavam
porque cada teste montava os dados na mão antes de olhar a tela.

## Origem

Extraído do projeto [WorkoutApp](https://github.com/jpemendonca/WorkoutApp) (.NET + Expo) e
revisado depois de dois projetos reais: um que funcionou (app Flutter entregue em poucos dias) e
o próprio WorkoutApp, que produziu a lição do parágrafo acima. O método é agnóstico de
linguagem/stack — funciona pra Go, Python, JS, Rust, C#, Dart, o que você quiser.

## Referência de contexto

- **WorkoutApp (origem)**: https://github.com/jpemendonca/WorkoutApp

---

Comece com `/bootstrap-projeto` (se tiver a skill) ou copie `vibe-coding-project-bootstrap.md`
como prompt inicial.
