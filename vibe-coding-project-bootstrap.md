# Prompt: bootstrap de projeto vibe-code

> Cole este arquivo inteiro como a primeira mensagem numa conversa nova com um agente de código
> (Claude Code ou similar), no diretório vazio (ou quase vazio) do projeto novo. Substitua os
> colchetes `[ ]` antes de colar, se já souber as respostas — o que deixar em branco, o agente
> pergunta antes de gerar qualquer arquivo.
>
> Isto é um método para desenvolvimento 100% orientado a agente ("vibe code"): uma pequena
> fundação documental que torna o projeto executável por um agente do início ao fim, inclusive
> por um modelo mais barato rodando sozinho num loop depois que a fundação estiver escrita. O
> método é genérico — linguagem, stack e tipo de projeto (app, bot, API, CLI, lib) não importam;
> o que importa é a estrutura dos documentos, a ordem do ROADMAP e a disciplina do loop de
> trabalho.

---

Quero que você bootstrappe este projeto seguindo o esquema abaixo. **Isto é um método, não um
template de conteúdo** — adapte cada seção ao que estou construindo, não copie estrutura de API
pra um bot de CLI nem seção de UX mobile pra uma lib sem interface.

## Contexto do projeto

- O que o produto faz: **[uma frase]**
- O caminho principal, descrito como eu faria, do início ao fim: **[ex.: "abrir o app, escolher
  um exercício, lançar as séries e finalizar o treino"]**
- Tipo de projeto e stack: **[app mobile / web app / bot / API / CLI / lib — e a linguagem/
  stack, ou "não sei, me sugira 2-3 opções com trade-off"]**
- De onde vêm os dados no primeiro uso: **[dataset embutido / arquivo local / banco vazio que eu
  preencho / API externa / serviço autenticado]** — e o caminho principal precisa funcionar sem
  rede e sem credencial? **[sim / não]**
- Escopo de vida: **[PoC descartável, prioriza velocidade / projeto que pode virar produto,
  prioriza rigor e fundação certa desde o início]**
- Quem executa o ROADMAP depois de pronto: **[eu mesmo continuando com você / um modelo mais
  barato rodando sozinho num loop, checkpoint a checkpoint]**
- Nível de rigor de testes: **[sem teste formal / teste do caminho feliz / cobertura ampla com
  dependência real isolada — container efêmero em vez de mock nas bordas de integração, nunca
  contra banco/serviço de dev real]**
- Nível de disciplina de segredos/config: **[esquema simples — .env com valor real, sem
  cerimônia / esquema formal — chave declarada com placeholder, valor real nunca escrito pelo
  agente, eu preencho manualmente fora do seu fluxo]**
- Referência visual/de produto, se houver: **[nome/link, ou "nenhuma"]**

Se eu deixei algo em branco acima, pergunte antes de gerar qualquer arquivo — não assuma. Se eu
descrevi o caminho principal como uma tela ou uma camada em vez de um percurso, insista comigo:
é a resposta que define a ordem do ROADMAP.

## O que gerar

Na raiz deste projeto, gere:

1. **`AGENTS.md`** (canônico, lido primeiro por qualquer agente) — o que é o produto; **o caminho
   principal escrito como percurso do usuário**; se houver mais de uma área no repo, tabela do
   que cada pasta é e seu estado; ordem de leitura obrigatória antes de trabalhar; **loop de
   trabalho** (pegar o primeiro bullet não marcado do ROADMAP, implementar seguindo o CODESTYLE,
   rodar o gate de qualidade, marcar o bullet, commit em Conventional Commits — bullet bloqueado
   fica não marcado com nota de bloqueio, nunca redefinido em silêncio); regras duras (ver
   abaixo); mapa do repositório; e uma linha dizendo o que o gate **não** cobre. `AGENTS.md` é
   sempre o único dono do conteúdo.

2. **`SPECS.md`** — visão de produto e escopo dentro/fora; arquitetura na forma que fizer
   sentido pro tipo de projeto (camadas de código, fluxo de mensagem/evento, hierarquia de
   telas, módulos e API pública — o que se aplicar); modelo de dados/domínio se houver; **origem
   dos dados no primeiro uso**; contratos (endpoints, tool schema de IA, comandos de CLI); a
   política de testes e de segredos que eu escolhi acima, por escrito; deploy se eu souber onde
   vai rodar. Inclua um **log de decisões**: quando uma decisão daqui mudar depois, não reescreva
   em silêncio — adicione nota datada (`> Revisão (AAAA-MM-DD): ...`).

3. **`CODESTYLE.md`** — só regras verificáveis num diff (se não dá pra checar lendo o diff, é
   intenção, não regra: não entra). Convenção de idioma; disciplina de comentário (só o porquê
   não óbvio, sem comentário do óbvio, sem código comentado, sem banner); convenções da
   linguagem escolhida; regra de camadas fiscalizada mecanicamente sempre que a
   linguagem/ferramenta permitir; estratégia de erro (validação como função pura devolvendo
   todos os erros de uma vez nas bordas de entrada, exceção reservada pra estado impossível);
   convenção de teste conforme minha escolha; regra de segredos conforme minha escolha; Git
   (Conventional Commits, uma mudança lógica por commit, nunca commitar com gate quebrado).

4. **`ROADMAP.md`** — fila ordenada de bullets em fases; pegar sempre o primeiro não marcado, em
   ordem, sem pular. A ordem das fases é a parte que mais dá errado:
   - Fase 0 é fundação (esqueleto, config de build/lint/format, gate de qualidade, estes próprios
     documentos);
   - **Fase 1 entrega o caminho principal inteiro, ponta a ponta** — feio, sem polimento, mas
     percorrível do começo ao fim;
   - depois as fases de ampliação, e por último as periféricas (preferências, tema, perfil, telas
     de apoio, créditos);
   - nenhuma fase além da 0 entrega só infraestrutura: design system, camada de domínio isolada
     ou cliente de API sozinho não são fases, são parte da fase que usa aquilo;
   - se a ordem estiver saindo por camada técnica (primeiro o visual, depois o domínio, depois os
     dados, depois as telas), pare e refaça — essa ordem produz um projeto que passa em todos os
     testes e não faz nada.

   A "definição de pronto" fica declarada no topo, separando *código escrito* (build/lint limpo,
   teste conforme minha política, formatter limpo, ROADMAP atualizado, commit convencional) de
   *caminho percorrido* (alguém abriu o produto e usou aquilo). Não precisa ser obrigatório
   percorrer — mas o bullet declara qual dos dois aconteceu. Não confunda código escrito com
   caminho validado. Se quem executa depois é um modelo mais barato, cada bullet nomeia arquivo e
   critério de aceite. Um item que depende de mim (conta paga, aparelho físico, credencial) fica
   na fila, na posição que atrapalha menos, nunca numa caixa à parte marcada como "não bloqueia
   nada".

5. **`README.md`** — documento humano: o que é, como rodar local, como testar, como fazer
   deploy se aplicável. Não duplica `AGENTS.md`/`SPECS.md`, aponta pra lá.

6. **Gate de qualidade** equivalente ao comando/hook nativo da stack escolhida, documentado no
   `AGENTS.md`.

7. **Ponteiros de agente**, desde o início e mesmo que eu use só um agente hoje: `CLAUDE.md` com
   uma única linha, `@AGENTS.md`, e nada mais. Acrescente `GEMINI.md`/`AGENT.md` se eu usar essas
   ferramentas. Nunca duplicam conteúdo — cópia repetida diverge e o agente passa a seguir a
   versão velha. Se o repo tiver mais de uma área com `AGENTS.md` próprio, cada área ganha o seu
   ponteiro.

## Regras que não mudam, qualquer que seja a stack

- **Toda mudança vira bullet no ROADMAP antes de virar código** — inclusive correção que eu pedir
  depois de testar. Escreva o bullet, depois implemente. Nada é consertado direto e em silêncio.
- Nenhuma dependência nova sem um bullet no ROADMAP que a peça.
- Warning/lint tratado como erro não se resolve com supressão — conserta o código.
- Nunca contornar o gate de qualidade.
- Bullet bloqueado não é redefinido em silêncio — fica não marcado com nota de uma linha.
- **Nada entra sem quem chame.** Função, módulo ou tela sem chamador no código de produção não é
  bullet pronto. Teste não conta como chamador: teste monta o cenário na mão e por isso passa
  mesmo quando o produto nunca executa aquele caminho.

## Erros que este método já cometeu

Casos reais, pra reconhecer o padrão antes de repetir:

- **Roadmap por camada.** Um app de treino organizou as fases em design system → domínio →
  catálogo → rotinas → treino. Oitenta bullets marcados, gate verde, 232 testes passando, e
  nenhum caminho para treinar: o botão principal era uma função vazia e a função que carregava os
  dados nunca era chamada por ninguém. Os testes passavam porque cada teste inseria os dados na
  mão antes de olhar a tela.
- **O item que provaria tudo, fora da fila.** Nesse mesmo projeto, "gerar o build e abrir no
  aparelho" ficou numa caixa separada, marcada como tarefa do autor, com a observação de que não
  bloqueava nenhuma fase. Era a única etapa que teria mostrado a tela vazia.
- **O teardown visual virando o plano.** O guia de aparência era o documento mais detalhado do
  projeto, então a fila se organizou em volta dele. Copiar a aparência ganhou prioridade sobre
  copiar o funcionamento.
- **A dependência externa decidida cedo demais.** O catálogo de dados foi parar atrás de uma API
  autenticada porque uma PoC anterior já tinha essa API. O produto passou a exigir servidor e
  token pra listar informação que cabia embutida no próprio app.

## Ao terminar

Resuma o que foi criado e liste explicitamente o que eu preciso preencher manualmente
(segredos reais, `.env`, credenciais de deploy).
