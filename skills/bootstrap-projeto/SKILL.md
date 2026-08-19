---
name: bootstrap-projeto
description: Bootstrapa um projeto novo (qualquer linguagem/stack — app, bot, API, CLI, lib) com o esquema de documentação AGENTS.md/SPECS.md/CODESTYLE.md/ROADMAP.md/README.md validado no projeto WorkoutApp para desenvolvimento 100% orientado a agente ("vibe code"). Use ao começar uma PoC, MVP ou projeto novo do zero, antes de escrever qualquer código de produto.
---

# Bootstrap de projeto vibe-code

Você vai gerar a fundação documental de um projeto novo, no diretório de trabalho atual, seguindo
o método validado no projeto WorkoutApp (.NET + Expo): um pequeno conjunto de documentos que
tornam o projeto executável por um agente de código do início ao fim — inclusive por um modelo
mais barato, num loop autônomo, depois que a fundação estiver escrita.

**Isto é um método, não um template de conteúdo.** Nada aqui é específico de uma linguagem ou
stack. Adapte cada seção ao que o usuário está construindo — não copie estrutura de API para um
bot de CLI, não copie seção de UX mobile para uma lib sem interface.

## Passo 1 — Levantar contexto

Pergunte (via AskUserQuestion quando fizer sentido oferecer opções, ou direto no chat) o que
faltar do seguinte. Não assuma nada que muda a estrutura dos documentos:

1. **O que o produto faz**, em uma frase.
2. **Tipo de projeto e stack.** Se o usuário não tiver decidido a stack, proponha 2-3 opções
   com trade-off objetivo — não decida sozinho uma escolha que ele vai viver por meses.
3. **Escopo de vida do projeto**: PoC descartável (prioriza velocidade, pode jogar fora) vs.
   projeto que pode virar produto real (prioriza rigor e fundação certa desde o início). Isso
   define o tom de todo o resto — não pergunte isso tarde.
4. **Quem executa o ROADMAP depois de pronto**: o mesmo agente/modelo que está montando a
   fundação agora, ou um modelo mais barato rodando sozinho num loop, checkpoint a checkpoint?
   Se for o segundo caso, os documentos precisam ser mais prescritivos (cada bullet do ROADMAP
   nomeia arquivo e critério de aceite, cada regra do CODESTYLE tem exemplo certo/errado) —
   porque não vai haver um humano por perto pra preencher a lacuna.
5. **Nível de rigor de testes.** Não assuma o nível mais alto por padrão — pergunte:
   - "sem teste formal / só smoke manual" — PoC descartável, prova de conceito de fim de semana;
   - "teste do caminho feliz" — cobre o fluxo principal, sem perseguir edge case;
   - "cobertura ampla com dependência real isolada" (ex.: container efêmero em vez de mock nas
     bordas de integração, nunca contra banco/serviço de dev real) — projeto que pode crescer,
     onde regressão silenciosa custa caro depois.
6. **Nível de disciplina de segredos/config.** Também não assuma o mais formal por padrão:
   - **esquema simples**: um `.env` com valores reais, sem cerimônia — ok pra protótipo local,
     uso pessoal, nada sensível de verdade;
   - **esquema formal**: toda chave de segredo é declarada em algum lugar versionado com valor
     vazio ou placeholder reconhecidamente falso; o agente nunca escreve o valor real — quem
     preenche é o usuário, fora do fluxo do agente, em `.env`/secret manager/user-secrets.
   Independente da escolha, se existe segredo de verdade envolvido (chave de API, token), a
   regra **"o agente nunca escreve o valor real de um segredo"** vale sempre — o que muda é
   quanta estrutura isso ganha nos documentos.
7. **Referência visual/de produto**, se houver algo pra copiar o espírito (ex.: um app existente
   cujo layout/fluxo servem de inspiração) — vira um documento de teardown à parte, citado pelos
   outros, do jeito que `hevy-design-system.md` funcionou no WorkoutApp.

## Passo 2 — Gerar os documentos

Crie estes arquivos na raiz do projeto. Adapte cada seção ao que foi levantado no Passo 1 —
omita o que não se aplica em vez de deixar seção vazia ou genérica demais pra ser útil.

### `AGENTS.md` — canônico, lido primeiro

- O que é o produto, em poucas frases.
- Se houver mais de uma "área" no repo (ex.: backend + app, ou vários serviços), uma tabela
  dizendo o que cada pasta é e seu estado (ativa / congelada / não mexer).
- Ordem de leitura obrigatória antes de trabalhar (CODESTYLE.md sempre; SPECS.md e ROADMAP.md
  da área que for tocar).
- **Loop de trabalho**: pegar o primeiro bullet não marcado do ROADMAP, implementar seguindo o
  CODESTYLE, rodar o gate de qualidade da linguagem escolhida, marcar o bullet, commit em
  Conventional Commits. Se um bullet se mostrar errado ou impossível, não redefinir em silêncio:
  deixar não marcado, com uma nota curta embaixo explicando o bloqueio, e seguir pro próximo.
- **Regras duras** da seção "Regras que não mudam" abaixo, mais qualquer regra específica do
  domínio deste projeto.
- Mapa do repositório.

Se o projeto usa mais de um agente de código (Claude Code, Codex, Gemini, etc.), `AGENTS.md` é
o único dono do conteúdo — `CLAUDE.md`, `GEMINI.md` etc. só contêm `@AGENTS.md`, nunca duplicam
texto.

### `SPECS.md` — produto, arquitetura, decisões

- Visão geral do produto e o que está dentro/fora do escopo atual.
- Arquitetura, na forma que fizer sentido pro tipo de projeto: camadas de código para uma API,
  fluxo de mensagem/evento para um bot, hierarquia de telas e onde mora o estado para um app,
  módulos e API pública para uma lib.
- Modelo de dados / modelo de domínio, se houver.
- Contratos: endpoints REST, schema de tool call de IA, comandos de CLI e seus argumentos —
  o que for aplicável.
- Autenticação/autorização, se houver.
- Testes: a política decidida no Passo 1, por escrito (não deixe implícito).
- Segredos/config: a política decidida no Passo 1, por escrito, com a tabela de "que dado vai
  onde" se o esquema for o formal.
- Deploy, se souber onde isso vai rodar.
- **Log de decisões**: quando uma decisão registrada aqui mudar depois, não reescreva o texto
  original em silêncio — adicione uma nota datada (`> Revisão (AAAA-MM-DD): ...`) explicando o
  que mudou e por quê. Isso é o que permite um agente novo entender o histórico sem depender de
  você estar na sala.

### `CODESTYLE.md` — só regras verificáveis num diff

Regra de ouro: se não dá pra checar lendo um diff, não entra aqui — isso é intenção, não regra.

- Convenção de idioma: código/identificadores/commits em inglês (ou o que o usuário preferir),
  texto voltado ao usuário final no idioma do produto.
- Disciplina de comentário: comentário só quando o *porquê* não é óbvio; sem comentário
  explicando o óbvio, sem código comentado, sem banner de seção, sem cabeçalho de arquivo. Frase
  curta, voz ativa, sem enchimento — no estilo do inglês técnico simplificado (ASD-STE100), não
  precisa citar a norma, só seguir o espírito: uma palavra um sentido, atemporal (fala do código,
  não do diff que o gerou).
- Convenções específicas da linguagem escolhida: nomenclatura, estrutura de arquivo, o que a
  comunidade dessa linguagem já convenciona (não invente padrão novo se a linguagem já tem um
  bom).
- Regra de camadas/módulos, fiscalizada mecanicamente sempre que a linguagem/ferramenta permitir
  (teste de arquitetura, regra de lint de import boundary, etc.) — texto sozinho sem
  fiscalização automática é fácil de esquecer depois de algumas sessões de agente.
- Estratégia de erro: prefira validação como função pura que devolve todos os erros de uma vez
  nas bordas de entrada de usuário; reserve exceção para estado impossível (alarme de bug, não
  caminho de usuário) — adapte ao idioma nativo de erro da linguagem escolhida.
- Convenção de teste: nomenclatura, o que isola com dependência falsa vs. real, conforme a
  política do Passo 1.
- Segredos: a regra dura de nunca escrever valor real, mais os detalhes da política escolhida.
- Git: Conventional Commits, uma mudança lógica por commit, nunca commitar com gate quebrado.

### `ROADMAP.md` — fila de execução

- Fila ordenada de bullets, agrupada em fases. Pegue o primeiro bullet não marcado, em ordem;
  não pule à frente, não agrupe bullets sem relação.
- "Definição de pronto" declarada no topo: build/lint limpo, teste(s) conforme a política
  escolhida, formatter limpo, ROADMAP atualizado, commit convencional.
- Se o modelo que vai executar for o mais barato (Passo 1, pergunta 4), cada bullet nomeia o
  arquivo esperado e o critério de aceite — não deixe implícito.
- Bullet bloqueada fica não marcada, com nota de uma linha embaixo explicando o bloqueio, em vez
  de redefinida ou apagada.
- Fase 0 normalmente é fundação: esqueleto do projeto, config de build/lint/format, gate de
  qualidade (equivalente a um hook de pre-commit), estes próprios documentos.

### `README.md`

Documento humano: o que é, como rodar localmente, como testar, como fazer deploy se aplicável.
Não duplica o que já está em `AGENTS.md`/`SPECS.md` — aponta pra lá quando for о caso.

### Gate de qualidade

Gere o gate de qualidade equivalente ao comando/hook da linguagem escolhida (ex.: `npm run
lint && npm test`, `pytest`, `go vet && go test ./...`, `cargo fmt --check && cargo test`,
`dotnet format && dotnet build && dotnet test`), documentado no `AGENTS.md` e, se o ecossistema
tiver um mecanismo de pre-commit hook nativo ou fácil de configurar, deixe-o pronto — mas não
force um hook custom numa linguagem que já tem convenção própria pra isso (ex.: usar o hook nativo
do framework em vez de reinventar um script).

## Passo 3 — Fechar

Termine com um resumo curto do que foi criado e, explicitamente, o que o usuário precisa
preencher manualmente (segredos reais, `.env`, credenciais de deploy) — nunca preencha isso você
mesmo.

## Regras que não mudam, qualquer que seja a stack

- O agente nunca escreve o valor real de um segredo, em nenhum arquivo, em nenhuma política.
- Nenhuma dependência nova sem um bullet no ROADMAP que a peça.
- Warnings/lint tratados como erro não se resolve com supressão — conserta o código.
- Nunca contornar o gate de qualidade (`--no-verify` ou equivalente).
- Bullet bloqueado não é redefinido em silêncio.

## Camada opcional: Ponytail

O plugin [Ponytail](https://github.com/DietrichGebert/ponytail) (instalado neste ambiente) aplica
uma "escada de decisão" antes de gerar código — preferir não escrever nada, depois reusar código
existente, depois stdlib/nativo do ecossistema, só por último código custom — reduzindo bloat sem
abrir mão de guardrails de segurança. Ative com `/ponytail lite|full|ultra` quando o usuário
quiser essa camada extra de disciplina de minimalismo durante a execução do ROADMAP; `/ponytail
off` desliga. Não é obrigatório — mencione a opção ao usuário no resumo final, ele decide por
projeto, do mesmo jeito que decide o nível de rigor de teste e de segredos no Passo 1.
