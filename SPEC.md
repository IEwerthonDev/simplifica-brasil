# Simplifica Brasil — Pacote SDD (Spec Driven Development)

Versão 1 · 30/10/2026 · derivado do PRD v4 · código AGPL-3.0-or-later, conteúdo CC BY-SA 4.0

## 0. Como usar este arquivo

Este arquivo é a **fonte da verdade** para construir o Simplifica Brasil com desenvolvimento orientado a especificação (SDD). Em SDD, a ordem é sempre a mesma:

```
Constituição  →  Especificação  →  Plano técnico  →  Tarefas  →  Código
 (regras fixas)   (o quê e por quê)  (como)          (passos)     (um passo por vez)
```

Cada camada só muda se a de cima permitir. Se uma tarefa entrar em conflito com a especificação, a tarefa está errada. Se a especificação entrar em conflito com a constituição, a especificação está errada.

**Duas formas de usar:**

1. **Arquivo único.** Coloque este arquivo na raiz do repositório como `SPEC.md` e use o prompt de `prompt-sdd.md` com um agente de código (Claude Code ou similar).
2. **Com o Spec Kit** (github.com/github/spec-kit). Copie cada seção para o arquivo correspondente. Os nomes de comandos e pastas mudam entre versões do Spec Kit; confira a documentação da versão instalada.

| Seção deste arquivo | Arquivo no Spec Kit |
|:--|:--|
| 1. Constituição | `.specify/memory/constitution.md` |
| 2. Especificação | `specs/001-onda-1/spec.md` |
| 3. Plano técnico | `specs/001-onda-1/plan.md` |
| 4. Tarefas | `specs/001-onda-1/tasks.md` |

**Convenções:**

- **DEVE / NÃO DEVE** são obrigatórios. **DEVERIA** é recomendado, com exceção justificada por escrito.
- `[PRECISA ESCLARECER: ...]` marca algo que o agente **não pode inventar**. Ele para e pergunta.
- IDs são estáveis: `C-` (constituição), `HU-` (história de usuário), `RF-` (requisito funcional), `RNF-` (requisito não funcional), `T` (tarefa). Commits e PRs citam os IDs.

**Portão de entrada:** a implementação só começa depois da **Etapa 0** do PRD (teste da hipótese com protótipo em slides) e com a decisão registrada em `docs/decisoes/0001-etapa-0.md`. Se a Etapa 0 mudar o diferencial do produto, esta especificação é revisada antes de qualquer código.

---

## 1. Constituição

Regras inegociáveis. O agente DEVE recusar qualquer tarefa que viole uma delas e explicar qual.

### Privacidade e confiança

- **C-01.** O produto NÃO DEVE pedir, coletar, transmitir ou armazenar dado pessoal: sem login, sem formulário de dados, sem cookies, sem identificadores de usuário.
- **C-02.** O produto NÃO DEVE agir em nome do usuário nem integrar o login gov.br. Ele explica, prepara e encaminha.
- **C-03.** O produto NÃO DEVE usar brasão, cores (azul gov.br, verde e amarelo), nome ou identidade visual do governo. Toda página DEVE exibir: "O Simplifica Brasil não é do governo e nunca pede seus dados."
- **C-04.** Preferências de leitura (tema, tamanho da letra, velocidade da voz) PODEM ficar no `localStorage` do aparelho, envoltas em `try/catch`. Nada mais é guardado no navegador.
- **C-05.** Métricas DEVEM ser agregadas, sem cookies e sem IP. Texto de busca só é guardado quando não teve resultado, cortado em 60 caracteres e descartado se contiver padrão de CPF, telefone ou e-mail.

### Segurança

- **C-06.** Todo link externo DEVE passar pelo componente `LinkOficial`, que só aceita `https:` e hosts **exatos** de `src/config/hosts-oficiais.json`. Qualquer outro host quebra o build.
- **C-07.** Uma segunda verificação no CI DEVE ler o HTML gerado em `dist/` e falhar se houver link externo fora da lista.
- **C-08.** O robô de capturas DEVE visitar só telas públicas, sem login, no máximo uma vez por semana por página, e DEVE validar cada endereço com a allowlist **e** com `isSSRFSafeURL` do pacote `ssrfcheck` antes de acessá-lo.
- **C-09.** Nenhuma captura ou alteração de link oficial é publicada sem aprovação humana. Mudanças em `hosts-oficiais.json` ou em qualquer `linkOficial` exigem duas aprovações (CODEOWNERS).
- **C-10.** CSP rígida: `default-src 'self'`, sem `'unsafe-inline'`, sem scripts de terceiros, `frame-ancestors 'none'`. Cabeçalhos: HSTS, `Referrer-Policy: no-referrer`, `X-Content-Type-Options: nosniff`, `Permissions-Policy` desligando câmera, microfone e geolocalização.
- **C-11.** Poucas dependências, versões travadas no lockfile, GitHub Actions fixadas por hash de commit.
- **C-12.** Capturas de telas oficiais NÃO DEVEM conter dado real legível. Telas após login usam dados fictícios ou borrados e passam por revisão de duas pessoas.

### Acessibilidade e linguagem

- **C-13.** Contraste de texto 7:1 (WCAG AAA) nos temas claro e escuro; demais critérios WCAG 2.2 AA. Zero violações do axe-core.
- **C-14.** Fonte base de 18 px, títulos a partir de 24 px, entrelinha 1,5. Alvos de toque com altura mínima de 56 px.
- **C-15.** NÃO DEVE haver carrossel, pop-up, tempo limite, conteúdo que some sozinho ou gesto obrigatório. Botão "Voltar" escrito, sempre no mesmo lugar.
- **C-16.** Português do Brasil em linguagem simples: frases de até 20 palavras, "você", voz ativa, termo técnico explicado na primeira vez. Botões começam com verbo.
- **C-17.** Ícone sempre acompanhado de texto. Imagens informativas têm `alt`; decorativas têm `aria-hidden`.

### Desempenho

- **C-18.** Até 150 KB transferidos por página e LCP abaixo de 2,5 s em 4G lento, medidos no CI.
- **C-19.** Site estático. JavaScript só onde for indispensável (ilhas), e a página DEVE funcionar sem JavaScript para ler o conteúdo.

### Processo e licença

- **C-20.** Código sob **AGPL-3.0-or-later**: arquivo `LICENSE` na raiz, `"license": "AGPL-3.0-or-later"` no `package.json` e `SPDX-License-Identifier: AGPL-3.0-or-later` no topo de cada arquivo de código. Conteúdo sob CC BY-SA 4.0 em `LICENSE-CONTEUDO`. Nome e logo regidos por `TRADEMARKS.md`.
- **C-21.** Toda dependência nova DEVE ter licença compatível com AGPL-3.0 (MIT, BSD, Apache-2.0, LGPL, GPL-3.0, AGPL-3.0). O CI verifica.
- **C-22.** Commits com DCO (`git commit -s`). PRs pequenos, um objetivo por PR, citando os IDs atendidos.
- **C-23.** Testes antes ou junto do código. Nenhuma tarefa está pronta sem o teste que a prova.
- **C-24.** O agente NÃO DEVE inventar endereços oficiais, telefones, níveis de conta, nomes de botões de telas oficiais ou regras de serviços públicos. Usa `[PRECISA ESCLARECER]` e pergunta.

---

## 2. Especificação (Onda 1)

Esta seção diz **o quê** e **por quê**. Ela não escolhe tecnologia.

### 2.1 Problema

Serviços públicos estão espalhados por dezenas de sites e apps, organizados por órgão e com linguagem técnica. Pessoas de 36 anos ou mais com pouca familiaridade digital travam no login, nos termos e no medo de golpe. Guias em texto já existem (por exemplo, o Me Explica); o diferencial do Simplifica é **fazer junto**: imagem da tela oficial com o lugar do toque, confirmação a cada passo e treino sem risco.

### 2.2 Personas

| ID | Persona | Resumo |
|:--|:--|:--|
| P-LUR | Dona Lurdes, 72 | Android simples, letra ampliada, WhatsApp e YouTube. Referência de projeto: se ela usa sozinha, todos usam |
| P-ANT | Seu Antônio, 58 | Recém-desempregado, celular compartilhado, pouca paciência para sites |
| P-CLA | Cláudia, 38 | Ajuda a família; descobre o site e manda o link no grupo |
| P-MAR | Marcos, 44 | Entregador, celular o dia todo, medo de golpe |
| P-SUE | Sueli, 49 | Diarista, pouco espaço no celular e dados limitados |

### 2.3 Escopo da Onda 1

**Dentro:** dois serviços completos, com Faça Conosco e Treinar antes:

- **S-GOV:** entrar na conta gov.br e recuperar o acesso.
- **S-CNIS:** ver minhas contribuições no INSS (extrato do CNIS).

Mais as páginas "Isso é golpe?", "Esqueci minha senha", Kit de acesso, Ajustes de leitura, "Simplifica no WhatsApp" (canal de avisos, sem coleta de dados) e "Como saber que somos nós".

**Fora:** os outros cinco serviços (Onda 2), bot de WhatsApp, lembretes, serviços estaduais e municipais, busca com IA, contas de usuário, qualquer dado pessoal.

### 2.4 Histórias de usuário

Prioridade: **P1** é indispensável para lançar; **P2** entra na Onda 1 se couber; **P3** pode ficar para depois.

#### HU-01 · Achar o que preciso pela situação de vida (P1)

Como pessoa que não sabe qual órgão procurar, quero escolher um assunto ou escrever com minhas palavras, para chegar ao serviço certo em no máximo três toques.

- **Dado** que estou na tela inicial, **quando** ela carrega, **então** vejo uma saudação, um campo de busca, os cartões das situações com conteúdo pronto e os atalhos "Esqueci minha senha" e "Isso é golpe?".
- **Dado** que existe uma situação sem nenhum serviço publicado, **quando** a tela inicial é gerada, **então** o cartão dessa situação não aparece.
- **Dado** que escrevo "ver meu inss" na busca, **quando** toco em "Buscar", **então** o serviço S-CNIS aparece entre os resultados, mesmo sem internet depois da primeira visita.
- **Dado** que a busca não encontra nada, **então** vejo uma mensagem com sugestões de assunto e o atalho para a tela inicial.

#### HU-02 · Entender o serviço antes de começar (P1)

Como pessoa com medo de errar, quero saber o que é o serviço, quanto custa, quanto tempo leva e o que separar, para me preparar.

- **Dado** que abro um serviço, **então** vejo: o que é (uma frase), etiquetas de custo e de canal, o que separar antes, o nível de conta gov.br exigido, a data "Conferido em" e os botões "Fazer comigo, passo a passo", "Treinar antes, sem risco" (se existir) e "Deu errado? Veja o que fazer".
- **Dado** que o serviço tem um termo técnico (por exemplo, CNIS), **então** ele é explicado na primeira vez em que aparece.

#### HU-03 · Fazer junto, passo a passo (P1)

Como pessoa que trava nas telas oficiais, quero ver a tela real com o lugar do toque marcado e confirmar cada passo, para seguir sem me perder.

- **Dado** que estou no passo N de M, **então** vejo "Passo N de M", a barra de progresso, a captura da tela oficial com o destaque no lugar do toque, uma instrução curta que cita o nome exato do botão, a caixa "Pare e confira" quando houver e os botões "Voltar" e "Fiz isso".
- **Dado** que toco em "Fiz isso" no último passo, **então** vou para a tela de saída para o site oficial.
- **Dado** que saio do passo a passo e volto pelo mesmo link, **então** retomo no mesmo passo (estado só na URL, por exemplo `#passo-3`).
- **Dado** que a captura foi conferida há mais de 90 dias, **então** vejo o aviso: "Esta imagem pode estar diferente. Procure o botão escrito [rótulo]."
- **Dado** que uso leitor de tela, **então** o destaque é ignorado e a instrução e o `alt` descrevem a ação.

#### HU-04 · Avisar que a tela mudou (P1)

Como pessoa que percebeu que a tela oficial está diferente, quero avisar com um toque, para o guia ser corrigido.

- **Dado** que estou num passo, **quando** toco em "A tela está diferente? Avise a gente", **então** vejo uma confirmação de agradecimento, e um contador agregado por passo é incrementado, sem nenhum dado pessoal.
- **Dado** que um passo recebe 3 avisos em 7 dias, **então** a equipe é alertada. `[PRECISA ESCLARECER: canal de alerta da equipe e como o contador agregado é recebido num site estático]`

#### HU-05 · Treinar antes, sem risco (P1)

Como pessoa com medo de errar no site oficial, quero praticar numa tela de mentira, para ganhar confiança.

- **Dado** que abro o treino, **então** vejo uma tela desenhada pelo Simplifica, com fundo listrado e o texto "Treino · nada aqui é de verdade".
- **Dado** que toco no lugar certo, **então** recebo "Isso! É esse botão" e posso seguir. **Dado** que toco em outro lugar, **então** recebo uma dica gentil com o nome do botão, sem punição.
- **Então** nenhuma informação sai do aparelho durante o treino.

#### HU-06 · Sair com segurança para o site oficial (P1)

Como pessoa que pode cair em site falso, quero conferir o endereço antes de digitar minha senha.

- **Dado** que toco em "Abrir o site oficial", **então** antes vejo a tela "Você vai sair do Simplifica Brasil", o endereço oficial por extenso e duas verificações: endereço igual letra por letra e chegada pelo botão, não por link de mensagem.
- **Dado** que o link oficial não está na allowlist, **então** o site não é gerado (C-06).

#### HU-07 · Resolver quando algo dá errado (P1)

- **Dado** que abro "Deu errado?", **então** vejo os erros comuns daquele serviço, cada um com a solução, a opção "A tela está diferente da imagem", o telefone oficial e a pergunta "Esta página ajudou? Sim / Não".

#### HU-08 · Recuperar o acesso ao gov.br (P1)

- **Dado** que abro "Esqueci minha senha", **então** respondo até três perguntas de sim ou não e recebo um único caminho de recuperação, com seu passo a passo. As respostas não saem do aparelho.
- `[PRECISA ESCLARECER: perguntas, ramificações e caminhos de recuperação vigentes no gov.br, conferidos na fonte oficial]`

#### HU-09 · Reconhecer golpe (P1)

- **Dado** que abro "Isso é golpe?", **então** vejo o que o governo nunca faz, o que fazer, "Mandar este aviso para a família" e "Já caí num golpe. E agora?".

#### HU-10 · Mandar para alguém (P1)

Como Cláudia, quero mandar uma página ou um passo específico para minha mãe.

- **Dado** que toco em "Mandar para alguém", **então** o compartilhamento nativo abre (Web Share API) ou, sem ele, um link `wa.me` com texto pronto.
- **Dado** que a página é aberta a partir do link, **então** ela carrega direto no conteúdo certo, inclusive no passo indicado.
- **Então** cada página tem título, descrição e imagem de prévia próprios para o WhatsApp.

#### HU-11 · Ler do meu jeito (P1)

- **Dado** que abro "Ajustes de leitura", **então** posso escolher a letra em três tamanhos, o tema claro, escuro ou igual ao celular, a velocidade da voz e "menos movimento".
- **Dado** que escolhi o tema escuro, **quando** abro outra página, **então** ela já aparece escura, sem piscar branco.
- **Dado** que não escolhi nada, **então** o tema segue o do celular.

#### HU-12 · Ouvir a página (P2)

- **Dado** que toco em "Ouvir", **então** o conteúdo principal é lido em pt-BR com a voz do próprio celular, e posso pausar. Nenhum texto é enviado a terceiros.

#### HU-13 · Seguir os avisos no WhatsApp (P2)

- **Dado** que abro "Simplifica no WhatsApp", **então** vejo o link do canal oficial, a explicação de que nunca pedimos dados nem mandamos a primeira mensagem, e a seção "Como saber que somos nós".
- `[PRECISA ESCLARECER: link do canal oficial]`

#### HU-14 · Kit de acesso e "Vou ajudar alguém" (P2)

- **Dado** que abro o Kit, **então** vejo as quatro etapas (entrar pelo banco, salvar no gerenciador do celular, testar a recuperação pelo rosto, ficha impressa) e a versão roteiro para quem ajuda um familiar. A ficha impressa nunca contém senha preenchida pelo site.

#### HU-15 · Funcionar com internet ruim (P2)

- **Dado** que já visitei o site uma vez, **quando** fico sem internet, **então** as páginas visitadas e a busca continuam funcionando.

### 2.5 Requisitos não funcionais

| ID | Requisito | Como medir |
|:--|:--|:--|
| RNF-01 | Acessibilidade conforme C-13 a C-17 nos dois temas | axe-core em todas as páginas; teste manual com TalkBack |
| RNF-02 | Desempenho conforme C-18 | Lighthouse CI em perfil de celular |
| RNF-03 | Funciona com a fonte do sistema ampliada a 130% ou mais, sem cortar texto nem rolar na horizontal | Teste visual com zoom de 200% |
| RNF-04 | Funciona em Android de entrada (2 a 3 GB de RAM) e no Chrome dos últimos dois anos | Teste manual em aparelho real |
| RNF-05 | Navegação completa só por teclado, com foco visível | Teste Playwright |
| RNF-06 | Segurança conforme C-06 a C-12 | CI e revisão |
| RNF-07 | Nenhuma requisição a domínio de terceiros | Teste que intercepta a rede e falha em domínio externo |

### 2.6 Modelo de conteúdo de um serviço

Campos obrigatórios salvo indicação. Os valores reais vêm das fontes oficiais, nunca do agente (C-24).

| Campo | Tipo | Observação |
|:--|:--|:--|
| `titulo` | texto | Começa com verbo quando possível |
| `situacao` | enum | `conta-govbr`, `inss`, `emprego`, `dinheiro`, `familia`, `documentos` |
| `resumo` | texto, até 200 caracteres | Uma frase |
| `custo`, `canal`, `duracaoAprox` | texto | Etiquetas da página do serviço |
| `oQueSeparar` | lista de textos | Pelo menos 1 |
| `nivelContaGovbr` | enum | `bronze`, `prata`, `ouro`, `qualquer` |
| `linkOficial` | URL | Host na allowlist |
| `telefoneOficial` | texto, opcional | |
| `passos` | lista | Ver abaixo |
| `treino` | lista, opcional | Telas de treino com área correta e dicas |
| `errosComuns` | lista de `{problema, solucao}` | De 3 a 5 |
| `revisadoEm` | data | Vence em 90 dias |

Cada passo: `texto` (até 160 caracteres, cita o rótulo do botão), `imagem`, `alt`, `alvo.rotulo`, `alvo.caixa {x, y, w, h}` em porcentagem, `pareEConfira` (opcional), `origem {app, versao?, tipo: web|android|ios}`, `conferidoEm`.

### 2.7 Casos de borda

- Imagem não carrega: a instrução em texto continua suficiente para seguir.
- JavaScript desligado: conteúdo e navegação funcionam; "Ouvir", compartilhar e ajustes podem ficar ocultos.
- `localStorage` bloqueado: o site usa os padrões, sem erro visível.
- Site oficial fora do ar: o guia não promete resolver; "Deu errado?" orienta a tentar mais tarde ou usar o telefone oficial.
- Link de passo inexistente (`#passo-9` num serviço de 5 passos): abre o passo 1.

### 2.8 Pendências que bloqueiam

- `[PRECISA ESCLARECER: lista exata de hosts oficiais da Onda 1, conferida na fonte]` (candidatos a verificar: `www.gov.br`, `sso.acesso.gov.br`, `meu.inss.gov.br`)
- `[PRECISA ESCLARECER: domínio do projeto e hospedagem (Cloudflare Pages, Netlify ou servidor com Caddy)]`
- `[PRECISA ESCLARECER: ferramenta de métricas (Umami ou GoatCounter) e onde ela roda]`
- `[PRECISA ESCLARECER: textos finais, passos, capturas e erros comuns de S-GOV e S-CNIS]`

---

## 3. Plano técnico

### 3.1 Pilha

| Camada | Escolha | Motivo |
|:--|:--|:--|
| Framework | Astro em modo estático (versão estável atual) | HTML pronto, quase nenhum JavaScript |
| Linguagem | TypeScript estrito | Erros aparecem no build |
| Conteúdo | Markdown com content collections validadas por Zod | Revisável por quem não programa |
| Estilo | CSS próprio com variáveis (tokens) | Sem framework; tema e escala de letra por variável |
| Fonte | Atkinson Hyperlegible, servida pelo próprio site | Sem fonte externa (C-10) |
| Busca | Pagefind | Offline, sem servidor |
| Offline | Service worker próprio, simples | Cache das páginas visitadas |
| Voz | Web Speech API | Voz do aparelho, sem terceiros |
| Compartilhar | Web Share API com fallback `wa.me` | Abre o WhatsApp direto |
| Testes | Vitest, Playwright, @axe-core/playwright, Lighthouse CI | Unidade, fluxo, acessibilidade, desempenho |
| Robô | Playwright + pixelmatch + pngjs + ssrfcheck; Maestro para Android depois | Detecta tela mudada |
| Segurança no CI | Gitleaks, OSV-Scanner, Semgrep, verificação de licenças | C-11, C-21 |

### 3.2 Estrutura de pastas

```
src/
  content/servicos/*.md
  content.config.ts
  config/hosts-oficiais.json
  components/  LinkOficial.astro  PassoComImagem.astro  TreinoTela.astro
               BotaoOuvir.astro  BotaoMandar.astro  CartaoSituacao.astro  AvisoNaoGoverno.astro
  layouts/Base.astro
  pages/  index.astro  situacao/[slug].astro  servicos/[slug]/index.astro
          servicos/[slug]/passos.astro  servicos/[slug]/treino.astro  servicos/[slug]/deu-errado.astro
          sair/[slug].astro  golpe.astro  esqueci-senha.astro  kit-de-acesso.astro
          ajustes.astro  whatsapp.astro
  styles/tokens.css
public/  capturas/  fontes/  tema.js  sw.js
robo/    pode-visitar.mjs  conferir-tela.mjs  telas.json
scripts/ checar-links.mjs  checar-licencas.mjs
tests/   unidade/  e2e/  acessibilidade/
docs/decisoes/
```

### 3.3 Contratos dos componentes principais

- **`LinkOficial`** (`href`, `slot`): valida `https:` e host exato em `hosts-oficiais.json`; lança erro no build se inválido; renderiza `<a rel="noopener noreferrer external">`. Atende C-06, HU-06.
- **`PassoComImagem`** (`imagem`, `alt`, `caixa`, `rotulo`, `conferidoEm`): imagem com SVG por cima (`viewBox="0 0 100 100"`, `preserveAspectRatio="none"`, `vector-effect="non-scaling-stroke"`, `aria-hidden`); mostra o aviso de captura vencida após 90 dias. Atende HU-03.
- **`TreinoTela`**: réplica desenhada, listrada, com uma área correta acessível por toque e teclado; feedback em região `aria-live="polite"`. Atende HU-05.
- **`tema.js`**: carregado no `<head>` sem `defer`, lê `tema` e `letra` do `localStorage` dentro de `try/catch` e aplica `data-tema` e `data-letra` no `<html>`. Atende HU-11 sem script inline (C-10).

### 3.4 Tokens de cor

| Token | Claro | Escuro |
|:--|:--|:--|
| `--fundo` | #FFFFFF | #18141C |
| `--texto` | #1F1D24 | #F4F0F7 |
| `--apoio` | #5A5562 | #CFC7D8 |
| `--primaria` / `--sobre-primaria` | #4A1D5C / #FFFFFF | #C9A3DB / #18141C |
| `--caju` | #F2A33A | #F2A33A |
| `--caixa` | #F2EFE9 | #231D29 |
| `--alerta` / `--alerta-fundo` | #8B1A1A / #FDECEC | #FFB4B4 / #2E1517 |

O tema escuro se aplica com `@media (prefers-color-scheme: dark)` em `:root:not([data-tema="claro"])` e também em `:root[data-tema="escuro"]`.

### 3.5 Pipeline de CI (cada PR)

1. `astro check` e `astro build` (tipos e schema).
2. `checar-links` (C-07) e `checar-licencas` (C-21).
3. Vitest, depois Playwright com axe nos dois temas.
4. Lighthouse CI com orçamento (C-18).
5. Gitleaks, OSV-Scanner, Semgrep.
6. Verificação de DCO e do cabeçalho SPDX em arquivos novos.

Job semanal separado: robô de capturas, que abre issue com antes, depois, diferença e caixa sugerida.

### 3.6 Estratégia de testes

| Tipo | O que prova | Exemplos |
|:--|:--|:--|
| Unidade | Regras puras | Validação de host, cálculo de captura vencida, conversão de caixa para porcentagem, filtro de busca sem resultado |
| Build | Contrato do conteúdo e dos links | Serviço sem `linkOficial` quebra o build; host fora da lista quebra o build |
| E2E | Histórias de usuário | Cada critério Dado/Quando/Então das HU P1 vira ao menos um teste |
| Acessibilidade | C-13 a C-17 | axe em todas as rotas, claro e escuro; navegação por teclado |
| Rede | RNF-07 | Teste que falha se houver requisição a outro domínio |

---

## 4. Tarefas

Formato: `T### [P?] [HU/C] Descrição → arquivos · Pronto quando`. `[P]` indica que a tarefa pode rodar em paralelo com outras `[P]` da mesma fase. Cada tarefa é um PR pequeno.

### Fase 1 · Preparação

- **T001** [C-20, C-22] Criar o projeto Astro com TypeScript estrito, `LICENSE` AGPL-3.0, `LICENSE-CONTEUDO`, `TRADEMARKS.md`, `CODEOWNERS`, modelo de PR e verificação de DCO → raiz · Pronto quando `npm run build` gera um `dist/` vazio e o CI roda.
- **T002** [P] [C-20] Script que falha se um arquivo de código novo não tiver o cabeçalho SPDX → `scripts/` · Pronto quando um arquivo sem cabeçalho quebra o CI.
- **T003** [P] [C-21] `checar-licencas.mjs` com a lista de licenças compatíveis → `scripts/` · Pronto quando uma dependência de licença incompatível de teste quebra o CI.
- **T004** [P] [C-11] Workflow base de CI com Actions fixadas por hash, Gitleaks, OSV-Scanner e Semgrep → `.github/workflows/` · Pronto quando o CI passa em verde.

### Fase 2 · Fundação (bloqueia as histórias)

- **T010** [C-13, C-14] Tokens de cor, tipografia, espaçamento e alvos de toque; fonte Atkinson servida localmente → `src/styles/tokens.css`, `public/fontes/` · Pronto quando um teste de contraste calcula 7:1 ou mais para cada par texto/fundo nos dois temas.
- **T011** [HU-11, C-10] `tema.js` e atributos `data-tema`/`data-letra` → `public/tema.js`, `Base.astro` · Pronto quando um teste E2E prova que a página abre escura, sem flash, após escolher o tema escuro.
- **T012** [C-03, C-15] Layout base com cabeçalho, "Voltar" fixo, `AvisoNaoGoverno` no rodapé e link "Código-fonte" → `src/layouts/Base.astro` · Pronto quando axe não acusa violações numa página vazia nos dois temas.
- **T013** [C-10] Cabeçalhos HTTP de segurança no formato da hospedagem → `[PRECISA ESCLARECER: hospedagem]` · Pronto quando um teste confere os cabeçalhos no preview.
- **T014** [HU-02, HU-03] Schema Zod do serviço conforme 2.6 → `src/content.config.ts` · Pronto quando um teste de build com um serviço incompleto falha com mensagem clara.
- **T015** [C-06] `hosts-oficiais.json` e `LinkOficial` → `src/config/`, `src/components/` · Pronto quando testes provam: host da lista passa; `http:`, host parecido (`gov.br.exemplo.com`) e host fora da lista quebram o build.
- **T016** [P] [C-07] `checar-links.mjs` lendo `dist/` → `scripts/` · Pronto quando um link inserido à mão no Markdown quebra o CI.
- **T017** [P] [RNF-07] Teste E2E que intercepta a rede e falha em domínio externo → `tests/e2e/` · Pronto quando passa em todas as rotas.

### Fase 3 · Histórias P1

Os textos e capturas reais de S-GOV e S-CNIS vêm de quem escreve conteúdo; enquanto não chegam, use um serviço fictício `exemplo-teste` marcado como não publicável.

- **T020** [HU-01] Tela inicial com cartões só de situações com conteúdo → `pages/index.astro`, `CartaoSituacao.astro` · Pronto quando os critérios de HU-01 (exceto busca) passam.
- **T021** [HU-01] Tela de situação → `pages/situacao/[slug].astro`.
- **T022** [HU-02] Página do serviço → `pages/servicos/[slug]/index.astro`.
- **T023** [HU-03] `PassoComImagem` e a rota de passos com progresso, `#passo-N`, "Fiz isso" e aviso de captura vencida → `components/`, `pages/servicos/[slug]/passos.astro`.
- **T024** [HU-06] Tela de saída para o site oficial → `pages/sair/[slug].astro`.
- **T025** [HU-07] Página "Deu errado?" com "Esta página ajudou?" → `pages/servicos/[slug]/deu-errado.astro`.
- **T026** [HU-05] `TreinoTela` e a rota de treino → `components/`, `pages/servicos/[slug]/treino.astro`.
- **T027** [P] [HU-09] Página "Isso é golpe?" → `pages/golpe.astro`.
- **T028** [HU-08] Página "Esqueci minha senha" → bloqueada por `[PRECISA ESCLARECER]` em HU-08.
- **T029** [P] [HU-10] `BotaoMandar` e metadados de prévia por página → `components/`, `Base.astro`.
- **T030** [P] [HU-11] Página de ajustes de leitura → `pages/ajustes.astro`.
- **T031** [HU-01] Busca com Pagefind e registro de busca sem resultado conforme C-05 → `pages/`, `components/`.
- **T032** [HU-04] "A tela está diferente?" → bloqueada por `[PRECISA ESCLARECER]` em HU-04.

Cada tarefa da Fase 3 está pronta quando todos os critérios Dado/Quando/Então da sua história passam em testes E2E e o axe não acusa violações nos dois temas.

### Fase 4 · Histórias P2

- **T040** [P] [HU-12] `BotaoOuvir` com Web Speech API, pausa e velocidade dos ajustes.
- **T041** [P] [HU-13] Página "Simplifica no WhatsApp" e "Como saber que somos nós".
- **T042** [P] [HU-14] Kit de acesso e roteiro "Vou ajudar alguém".
- **T043** [HU-15] Service worker com cache das páginas visitadas e da busca.

### Fase 5 · Robô de capturas e operação

- **T050** [C-08] `pode-visitar.mjs` com allowlist e `isSSRFSafeURL` · Pronto quando testes provam que `http://169.254.169.254/`, `http://localhost`, um IP decimal e um host fora da lista são recusados.
- **T051** [C-08, C-09] `conferir-tela.mjs` com Playwright, pixelmatch e caixa sugerida em porcentagem.
- **T052** [C-09] Workflow semanal que abre issue com antes, depois, diferença e caixa sugerida; nunca publica sozinho.
- **T053** [P] Verificação no CI de `revisadoEm` vencido (90 dias) → falha com a lista de serviços.

### Fase 6 · Qualidade e lançamento

- **T060** [RNF-02] Lighthouse CI com orçamento por rota.
- **T061** [RNF-03, RNF-05] Testes de zoom a 200% e navegação só por teclado.
- **T062** Observabilidade agregada → bloqueada por `[PRECISA ESCLARECER]` em 2.8.
- **T063** Checklist de lançamento: `SECURITY.md`, `/.well-known/security.txt`, política de privacidade curta, rodada de testes com usuários aprovada.

---

## 5. Rastreabilidade

| Requisito | Tarefas | Prova |
|:--|:--|:--|
| C-06, C-07 | T015, T016 | Testes de build e do script de links |
| C-08 | T050, T051 | Testes de `pode-visitar` |
| C-10 | T011, T013 | E2E de tema sem flash; teste de cabeçalhos |
| C-13 a C-17 | T010, T012, todas da Fase 3 | axe nos dois temas, teste de contraste |
| C-20 a C-22 | T001 a T003 | CI de SPDX, licenças e DCO |
| HU-01 | T020, T021, T031 | E2E |
| HU-03 | T023 | E2E |
| HU-05 | T026 | E2E |
| HU-06 | T015, T024 | Build e E2E |
| HU-11 | T011, T030 | E2E |
| RNF-07 | T017 | E2E de rede |

## 6. Definição de pronto (vale para toda tarefa)

- Testes que provam a tarefa existem e passam; nenhum teste anterior quebrou.
- axe sem violações nos temas claro e escuro nas rotas afetadas.
- Nenhum link fora da allowlist; nenhum segredo detectado; dependências com licença compatível.
- Orçamento de 150 KB respeitado nas rotas afetadas.
- Arquivos novos com cabeçalho SPDX; commits com `Signed-off-by`.
- PR cita os IDs atendidos e foi aprovado por uma pessoa (duas se mexer em link oficial).
