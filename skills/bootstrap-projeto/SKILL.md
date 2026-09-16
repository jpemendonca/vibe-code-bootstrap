---
name: bootstrap-projeto
description: Bootstrapa um projeto novo (qualquer linguagem/stack — app, bot, API, CLI, lib) com o esquema de documentação AGENTS.md/SPECS.md/CODESTYLE.md/ROADMAP.md/README.md para desenvolvimento 100% orientado a agente ("vibe code"). Use ao começar uma PoC, MVP ou projeto novo do zero, antes de escrever qualquer código de produto.
---

# Bootstrap de projeto vibe-code

Você vai gerar a fundação documental de um projeto novo, no diretório de trabalho atual: um
pequeno conjunto de documentos que tornam o projeto executável por um agente de código do início
ao fim — inclusive por um modelo mais barato, num loop autônomo, depois que a fundação estiver
escrita.

**Isto é um método, não um template de conteúdo.** Nada aqui é específico de uma linguagem ou
stack. Adapte cada seção ao que o usuário está construindo — não copie estrutura de API para um
bot de CLI, não copie seção de UX mobile para uma lib sem interface.

## Passo 1 — Levantar contexto

Pergunte (via AskUserQuestion quando fizer sentido oferecer opções, ou direto no chat) o que
faltar do seguinte. Não assuma nada que muda a estrutura dos documentos:

1. **O que o produto faz**, em uma frase.
2. **O caminho principal.** Qual é o fluxo que justifica o produto existir, descrito como o
   usuário faria, do início ao fim. "Abrir o app, escolher um exercício, lançar as séries e
   finalizar o treino." "Colar uma URL e receber o resumo." Se o usuário descrever uma tela ou
   uma camada em vez de um percurso, insista — é a resposta que define a ordem do ROADMAP, e
   errar aqui é o erro mais caro do método.
3. **Tipo de projeto e stack.** Se o usuário não tiver decidido a stack, proponha 2-3 opções
   com trade-off objetivo — não decida sozinho uma escolha que ele vai viver por meses.
4. **De onde vêm os dados no primeiro uso.** O que precisa estar disponível pro caminho
   principal funcionar na primeira vez que o produto abre: dataset embutido, arquivo local,
   banco vazio que o usuário preenche, API externa, serviço autenticado. Se a resposta envolve
   rede ou credencial, pergunte explicitamente se o caminho principal precisa funcionar sem
   isso — é comum a resposta certa ser "sim", e isso muda a arquitetura inteira.
5. **Escopo de vida do projeto**: PoC descartável (prioriza velocidade, pode jogar fora) vs.
   projeto que pode virar produto real (prioriza rigor e fundação certa desde o início). Isso
   define o tom de todo o resto — não pergunte isso tarde.
6. **Quem executa o ROADMAP depois de pronto**: o mesmo agente/modelo que está montando a
   fundação agora, ou um modelo mais barato rodando sozinho num loop, checkpoint a checkpoint?
   Se for o segundo caso, os documentos precisam ser mais prescritivos (cada bullet do ROADMAP
   nomeia arquivo e critério de aceite, cada regra do CODESTYLE tem exemplo certo/errado) —
   porque não vai haver um humano por perto pra preencher a lacuna.
7. **Nível de rigor de testes.** Não assuma o nível mais alto por padrão — pergunte:
   - "sem teste formal / só smoke manual" — PoC descartável, prova de conceito de fim de semana;
   - "teste do caminho feliz" — cobre o fluxo principal, sem perseguir edge case;
   - "cobertura ampla com dependência real isolada" (ex.: container efêmero em vez de mock nas
     bordas de integração, nunca contra banco/serviço de dev real) — projeto que pode crescer,
     onde regressão silenciosa custa caro depois.
8. **Nível de disciplina de segredos/config.** Também não assuma o mais formal por padrão:
   - **esquema simples**: um `.env` com valores reais, sem cerimônia — ok pra protótipo local,
     uso pessoal, nada sensível de verdade;
   - **esquema formal**: toda chave de segredo é declarada em algum lugar versionado com valor
     vazio ou placeholder reconhecidamente falso; o agente nunca escreve o valor real — quem
     preenche é o usuário, fora do fluxo do agente, em `.env`/secret manager/user-secrets.
9. **Referência visual/de produto**, se houver algo pra copiar o espírito (ex.: um app existente
   cujo layout/fluxo servem de inspiração) — vira um documento de teardown à parte, citado pelos
   outros. Deixe claro no próprio documento que ele descreve **aparência**, e que a ordem do
   ROADMAP obedece ao caminho principal, não ao teardown.

## Passo 2 — Gerar os documentos

Crie estes arquivos na raiz do projeto. Adapte cada seção ao que foi levantado no Passo 1 —
omita o que não se aplica em vez de deixar seção vazia ou genérica demais pra ser útil.

### `AGENTS.md` — canônico, lido primeiro

- O que é o produto, em poucas frases.
- **O caminho principal**, escrito como percurso do usuário. É a primeira coisa que um agente
  novo lê, e a régua pra decidir se um bullet importa.
- Se houver mais de uma "área" no repo (ex.: backend + app, ou vários serviços), uma tabela
  dizendo o que cada pasta é e seu estado (ativa / congelada / não mexer).
- Ordem de leitura obrigatória antes de trabalhar (CODESTYLE.md sempre; SPECS.md e ROADMAP.md
  da área que for tocar).
- **Loop de trabalho**: pegar o primeiro bullet não marcado do ROADMAP, implementar seguindo o
  CODESTYLE, rodar o gate de qualidade, marcar o bullet, commit em Conventional Commits. Se um
  bullet se mostrar errado ou impossível, não redefinir em silêncio: deixar não marcado, com uma
  nota curta embaixo explicando o bloqueio, e seguir pro próximo.
- **Regras duras** da seção "Regras que não mudam" abaixo, mais qualquer regra específica do
  domínio deste projeto.
- Mapa do repositório.

### Ponteiros de agente — `CLAUDE.md`, `GEMINI.md` e afins

`AGENTS.md` é sempre o único dono do conteúdo, mesmo que hoje só exista um agente no projeto.
Crie os ponteiros desde o começo, junto com ele. Cada ponteiro tem uma linha — `@AGENTS.md` — e
nada mais.

Nunca duplique texto entre eles. Conteúdo repetido diverge por volta da terceira sessão, e a
partir daí o agente segue a cópia velha sem avisar ninguém.

Crie no mínimo `CLAUDE.md`. Acrescente `GEMINI.md`, `AGENT.md` ou o que a ferramenta do usuário
esperar, se ele usar outras. Se o repositório tiver mais de uma área com `AGENTS.md` próprio,
cada área ganha o seu ponteiro, apontando para o `AGENTS.md` da própria pasta.

### `SPECS.md` — produto, arquitetura, decisões

- Visão geral do produto e o que está dentro/fora do escopo atual.
- Arquitetura, na forma que fizer sentido pro tipo de projeto: camadas de código para uma API,
  fluxo de mensagem/evento para um bot, hierarquia de telas e onde mora o estado para um app,
  módulos e API pública para uma lib.
- Modelo de dados / modelo de domínio, se houver.
- **Origem dos dados no primeiro uso**, conforme o Passo 1: o que vem embutido, o que vem de
  fora, e o que o caminho principal exige que esteja disponível sem rede.
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
- Convenção de teste: o que isola com dependência falsa vs. real, conforme a política do Passo 1,
  mais a nomenclatura e o corpo descritos logo abaixo.
- Segredos: os detalhes da política escolhida no Passo 1.
- Git: Conventional Commits, uma mudança lógica por commit, nunca commitar com gate quebrado.

#### Convenção de teste

Só se aplica se o Passo 1 escolheu ter testes. Escreva estas três regras no `CODESTYLE.md`, com
exemplo na linguagem do projeto — não deixe implícito.

**Nome no padrão `Metodo_Cenario_Comportamento`.** Três partes separadas por `_`: o que está
sendo exercitado, em que situação, e o que deve acontecer.

```
Sacar_ComSaldoInsuficiente_LancaArgumentException
Somar_ComDoisValoresPositivos_RetornaSomaCorreta
```

O identificador segue o idioma do código do projeto (inglês, se o `CODESTYLE.md` decidiu inglês
para identificadores). O padrão é a forma das três partes, não a língua.

**Corpo marcado com `arrange`, `act`, `assert`**, em comentário, nessa ordem. É a única exceção
à disciplina de comentário deste documento: aqui o comentário não explica o óbvio, ele separa as
três fases e deixa visível quando um teste está exercitando duas coisas ao mesmo tempo.

**Descrição legível, quando o framework aceitar.** Uma frase no idioma do produto, dizendo o que
o teste garante — é o que aparece no relatório quando o teste quebra.

```csharp
[Fact(DisplayName = "Deve retornar erro ao tentar dividir por zero")]
```

O equivalente muda por ecossistema:

| Stack | Nome do caso | Descrição legível |
|---|---|---|
| xUnit / NUnit | nome do método | `[Fact(DisplayName = "...")]`, `[TestCase(TestName = "...")]` |
| Jest / Vitest | `describe` com o método | a string do `it`/`test` já é a descrição |
| pytest | nome da função `test_...` | docstring da função |
| Dart / Flutter | string do `test()` no padrão | `test()` aninhado em `group()` com a frase |
| Go | `func TestMetodo_Cenario_Comportamento` | `t.Run` com a frase |

Onde o framework só aceita uma string (Jest, Dart), ela carrega o cenário e o comportamento, e o
`describe`/`group` externo carrega o método — as três partes continuam lá, distribuídas.

### `ROADMAP.md` — fila de execução

- Fila ordenada de bullets, agrupada em fases. Pegue o primeiro bullet não marcado, em ordem;
  não pule à frente, não agrupe bullets sem relação.
- **Ordem das fases, e esta é a parte que mais dá errado:**
  - Fase 0 é fundação: esqueleto do projeto, config de build/lint/format, gate de qualidade,
    estes próprios documentos.
  - **Fase 1 entrega o caminho principal inteiro, ponta a ponta.** Feio, sem polimento, com o
    mínimo de tela que der — mas percorrível pelo usuário do começo ao fim.
  - Só depois vêm as fases de ampliação, e por último as periféricas: preferências, tema,
    perfil, telas de apoio, créditos.
  - Nenhuma fase além da 0 pode entregar só infraestrutura. Design system, camada de domínio
    isolada, cliente de API sozinho: nada disso é uma fase — é parte da fase que usa aquilo.
  - Se você está montando a ordem por camada técnica (primeiro o visual, depois o domínio,
    depois os dados, depois as telas), pare e refaça. Essa ordem produz um projeto que passa em
    todos os testes e não faz nada.
- **Definição de pronto**, declarada no topo, separando duas coisas que é fácil confundir:
  - *código escrito*: build/lint limpo, teste(s) conforme a política escolhida, formatter limpo,
    ROADMAP atualizado, commit convencional;
  - *caminho percorrido*: alguém abriu o produto e usou aquilo. Não precisa ser obrigatório —
    é decisão do usuário se isso trava ou não um bullet. Mas o bullet declara qual dos dois
    aconteceu. Um bullet que só tem código escrito diz isso com todas as letras.
  - A frase que resume: **não confunda código escrito com caminho validado.**
- Formato de cada bullet, quando o projeto for maior que uma PoC de fim de semana: origem (de
  onde veio o requisito), escopo (o que entra e o que fica fora), aceite (quando ganha `[x]`).
  Ao concluir, uma nota datada embaixo — `> Validação (AAAA-MM-DD): ...` — dizendo o que foi
  feito e o que foi observado funcionando. Ao parar no meio, `> Em andamento (AAAA-MM-DD): ...`
  com o que existe, o que falta e o próximo passo.
- Bullet bloqueado fica não marcado, com nota de uma linha embaixo explicando o bloqueio, em vez
  de redefinido ou apagado. Se existe um item que depende do usuário (conta paga, aparelho
  físico, credencial), ele fica **na fila, na posição que atrapalha menos**, e nunca com uma
  observação dizendo que não bloqueia nada — porque normalmente ele é justamente o item que
  provaria que o resto funciona.
- **O ROADMAP é a única fila de trabalho.** Quando o usuário testar e pedir uma correção ou uma
  mudança, ela entra como bullet novo, na posição que fizer sentido, antes de ser implementada.
  Vale para bug encontrado no uso, ajuste de layout, mudança de escopo e ideia que surgiu no meio
  da conversa. Nada é consertado direto e em silêncio: o roadmap é o que permite outro agente,
  em outra máquina, saber o que aconteceu sem ler o histórico do chat.
- **Exceção, uma só: mudança que não altera comportamento.** Erro de digitação em comentário ou
  documento, formatação, link quebrado, renomear variável local. Essas você conserta direto, no
  commit do bullet em que estiver.

  A exceção acaba onde começa o comportamento. Ajuste de texto que o usuário lê na tela é
  comportamento. Correção de lógica é comportamento, mesmo que caiba numa linha. E **se o usuário
  reportou, é bullet** — o tamanho do conserto não decide, quem decide é o fato de aquilo ter
  aparecido no uso. O propósito do bullet ali não é organizar o trabalho, é registrar que o
  problema existiu.

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

Diga no `AGENTS.md`, em uma linha, o que o gate **não** cobre: normalmente ele não abre o
produto, não percorre o caminho principal e não prova que os dados chegam de verdade. Gate verde
é condição necessária, não prova de que existe produto.

## Passo 3 — Fechar

Termine com um resumo curto do que foi criado e, explicitamente, o que o usuário precisa
preencher manualmente (segredos reais, `.env`, credenciais de deploy).

## Regras que não mudam, qualquer que seja a stack

- **Toda mudança vira bullet no ROADMAP antes de virar código** — inclusive correção que o
  usuário pediu depois de testar. Escreva o bullet, depois implemente. A única exceção é mudança
  que não altera comportamento (digitação, formatação, link, nome de variável local); se o
  usuário reportou, é bullet, por menor que seja o conserto.
- Nenhuma dependência nova sem um bullet no ROADMAP que a peça.
- Warnings/lint tratados como erro não se resolve com supressão — conserta o código.
- Nunca contornar o gate de qualidade (`--no-verify` ou equivalente).
- Bullet bloqueado não é redefinido em silêncio.
- **Nada entra sem quem chame.** Função, módulo ou tela sem chamador no código de produção não
  é bullet pronto. Teste não conta como chamador: teste monta o cenário na mão e por isso passa
  mesmo quando o produto nunca executa aquele caminho.

## Erros que este método já cometeu

Casos reais, para reconhecer o padrão antes de repeti-lo:

- **Roadmap por camada.** Um app de treino organizou as fases em design system → domínio →
  catálogo → rotinas → treino. Oitenta bullets marcados, gate verde, 232 testes passando, e
  nenhum caminho para treinar: o botão principal era uma função vazia e a função que carregava
  os dados nunca era chamada por ninguém. Os testes passavam porque cada teste inseria os dados
  na mão antes de olhar a tela.
- **O item que provaria tudo, fora da fila.** Nesse mesmo projeto, "gerar o build e abrir no
  aparelho" ficou numa caixa separada, marcada como tarefa do autor, com a observação de que não
  bloqueava nenhuma fase. Era a única etapa que teria mostrado a tela vazia.
- **O teardown visual virando o plano.** O guia de aparência era o documento mais detalhado do
  projeto, então a fila se organizou em volta dele. Copiar a aparência ganhou prioridade sobre
  copiar o funcionamento.
- **A dependência externa decidida cedo demais.** O catálogo de dados foi parar atrás de uma API
  autenticada porque uma PoC anterior já tinha essa API. O produto passou a exigir servidor e
  token pra listar informação que cabia embutida no próprio app.
