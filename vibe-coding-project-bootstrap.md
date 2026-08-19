# Prompt: bootstrap de projeto vibe-code

> Cole este arquivo inteiro como a primeira mensagem numa conversa nova com um agente de código
> (Claude Code ou similar), no diretório vazio (ou quase vazio) do projeto novo. Substitua os
> colchetes `[ ]` antes de colar, se já souber as respostas — o que deixar em branco, o agente
> pergunta antes de gerar qualquer arquivo.
>
> Isto é o método validado no projeto WorkoutApp para desenvolvimento 100% orientado a agente
> ("vibe code"): uma pequena fundação documental que torna o projeto executável por um agente do
> início ao fim, inclusive por um modelo mais barato rodando sozinho num loop depois que a
> fundação estiver escrita. O método é genérico — linguagem, stack e tipo de projeto (app, bot,
> API, CLI, lib) não importam; o que importa é a estrutura dos documentos e a disciplina do loop
> de trabalho.

---

Quero que você bootstrappe este projeto seguindo o esquema abaixo. **Isto é um método, não um
template de conteúdo** — adapte cada seção ao que estou construindo, não copie estrutura de API
pra um bot de CLI nem seção de UX mobile pra uma lib sem interface.

## Contexto do projeto

- O que o produto faz: **[uma frase]**
- Tipo de projeto e stack: **[app mobile / web app / bot / API / CLI / lib — e a linguagem/
  stack, ou "não sei, me sugira 2-3 opções com trade-off"]**
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

Se eu deixei algo em branco acima, pergunte antes de gerar qualquer arquivo — não assuma.

## O que gerar

Na raiz deste projeto, gere:

1. **`AGENTS.md`** (canônico, lido primeiro por qualquer agente) — o que é o produto; se houver
   mais de uma área no repo, tabela do que cada pasta é e seu estado; ordem de leitura
   obrigatória antes de trabalhar; **loop de trabalho** (pegar o primeiro bullet não marcado do
   ROADMAP, implementar seguindo o CODESTYLE, rodar o gate de qualidade, marcar o bullet, commit
   em Conventional Commits — bullet bloqueado fica não marcado com nota de bloqueio, nunca
   redefinido em silêncio); regras duras (ver abaixo); mapa do repositório. Se eu usar mais de um
   agente de código no projeto, `CLAUDE.md`/`GEMINI.md`/etc. só importam `@AGENTS.md`, nunca
   duplicam conteúdo.

2. **`SPECS.md`** — visão de produto e escopo dentro/fora; arquitetura na forma que fizer
   sentido pro tipo de projeto (camadas de código, fluxo de mensagem/evento, hierarquia de
   telas, módulos e API pública — o que se aplicar); modelo de dados/domínio se houver;
   contratos (endpoints, tool schema de IA, comandos de CLI); a política de testes e de
   segredos que eu escolhi acima, por escrito; deploy se eu souber onde vai rodar. Inclua um
   **log de decisões**: quando uma decisão daqui mudar depois, não reescreva em silêncio —
   adicione nota datada (`> Revisão (AAAA-MM-DD): ...`).

3. **`CODESTYLE.md`** — só regras verificáveis num diff (se não dá pra checar lendo o diff, é
   intenção, não regra: não entra). Convenção de idioma; disciplina de comentário (só o porquê
   não óbvio, sem comentário do óbvio, sem código comentado, sem banner); convenções da
   linguagem escolhida; regra de camadas fiscalizada mecanicamente sempre que a
   linguagem/ferramenta permitir; estratégia de erro (validação como função pura devolvendo
   todos os erros de uma vez nas bordas de entrada, exceção reservada pra estado impossível);
   convenção de teste conforme minha escolha; regra de segredos conforme minha escolha; Git
   (Conventional Commits, uma mudança lógica por commit, nunca commitar com gate quebrado).

4. **`ROADMAP.md`** — fila ordenada de bullets em fases; pegar sempre o primeiro não marcado, em
   ordem, sem pular; "definição de pronto" declarada no topo (build/lint limpo, teste conforme
   minha política, formatter limpo, ROADMAP atualizado, commit convencional); se quem executa
   depois é um modelo mais barato, cada bullet nomeia arquivo e critério de aceite; Fase 0
   normalmente é fundação (esqueleto, config de build/lint/format, gate de qualidade, estes
   próprios documentos).

5. **`README.md`** — documento humano: o que é, como rodar local, como testar, como fazer
   deploy se aplicável. Não duplica `AGENTS.md`/`SPECS.md`, aponta pra lá.

6. **Gate de qualidade** equivalente ao comando/hook nativo da stack escolhida, documentado no
   `AGENTS.md`.

## Regras que não mudam, qualquer que seja a stack

- Você nunca escreve o valor real de um segredo, em nenhum arquivo.
- Nenhuma dependência nova sem um bullet no ROADMAP que a peça.
- Warning/lint tratado como erro não se resolve com supressão — conserta o código.
- Nunca contornar o gate de qualidade.
- Bullet bloqueado não é redefinido em silêncio — fica não marcado com nota de uma linha.

## Ao terminar

Resuma o que foi criado e liste explicitamente o que eu preciso preencher manualmente
(segredos reais, `.env`, credenciais de deploy) — não preencha isso por mim.

---

## Nota: camada opcional Ponytail

Se este ambiente tiver o plugin [Ponytail](https://github.com/DietrichGebert/ponytail)
instalado (`/plugin marketplace add DietrichGebert/ponytail` + `/plugin install
ponytail@ponytail`, se ainda não tiver), ele aplica uma "escada de decisão" antes de gerar
código — preferir não escrever nada, depois reusar código existente, depois stdlib/nativo do
ecossistema, só por último código custom. Ative com `/ponytail lite|full|ultra` quando quiser
essa disciplina extra de minimalismo durante a execução do ROADMAP; `/ponytail off` desliga.
Opcional, não faz parte do bootstrap em si.
