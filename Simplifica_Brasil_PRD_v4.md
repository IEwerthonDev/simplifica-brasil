# Simplifica Brasil — PRD do copiloto do gov

Oct 30, 2026 · @Ewerthon · versão 4 (antes chamado Mãozinha)

**O que mudou na versão 4:** licença do código definida (AGPL-3.0-or-later, depois de considerar a MIT), nova **Etapa 0** para testar a hipótese central com um protótipo em slides antes de escrever código, e uma seção sobre a relação com a HarpiaFOSS, começando por uma contribuição ao `ssrfcheck` que o próprio Simplifica vai usar.

**Versão 3:** a pesquisa sobre o Me Explica mostrou que linguagem simples, links oficiais, data de revisão e transparência editorial já existem no concorrente. Por isso, esta versão concentra o produto no que ele tem de diferente (fazer junto, com imagem e treino), reduz o lançamento para dois serviços muito bem feitos e acrescenta o que faltava para alguém começar a construir: um guia de implementação com código, um plano de testes com usuários, a operação de conteúdo, custos, riscos e um glossário.

**Versão 2:** novo nome e logo, público ampliado para 36+, modos Faça Conosco e Treinar antes, canal de avisos no WhatsApp, redes sociais, ajustes de leitura, modo escuro e análise de concorrentes.

### Como ler este documento

Este PRD (documento de requisitos de produto) é longo porque serve a pessoas diferentes. Não precisa ler tudo de uma vez.

| Se você vai… | Leia primeiro |
|:--|:--|
| Entender o produto em 10 minutos | Resumo, Público e personas, Concorrência (itens 3, 4 e 8), Escopo do MVP |
| Programar | Escopo do MVP, Relação com a HarpiaFOSS e o ssrfcheck, Guia de implementação, Segurança, Prompt SDD |
| Escrever ou revisar conteúdo | Princípios de design, Operação de conteúdo, Modo Faça Conosco |
| Desenhar telas | Princípios de design, Telas do MVP, Identidade |
| Testar com pessoas | Etapa 0, Plano de validação com usuários, Métricas de sucesso |
| Fazer a primeira contribuição | Relação com a HarpiaFOSS e o ssrfcheck |

Palavras técnicas que aparecem pelo caminho (CSP, CI, schema, allowlist e outras) estão explicadas no **Glossário**, no fim do documento.

## Resumo

O Simplifica Brasil é um guia gratuito que explica, em linguagem simples, como resolver as coisas do governo e leva a pessoa até o site oficial certo. Ele não executa nenhum serviço, não pede senha e não guarda documentos: explica, prepara e encaminha.

**Nome.** “Simplifica Brasil” diz o que o produto faz. Antes de publicar: buscar registro no INPI, conferir os domínios (`simplificabrasil.com.br` e `.org.br`) e os perfis nas redes. Há um cuidado extra: um nome com “Brasil” soa mais oficial, e o projeto proíbe parecer governo. Vale conferir também se não existe programa público com nome parecido (o governo federal já usou “Simplifique!” num programa de desburocratização; confirmar). Por isso, o aviso “não é do governo” fica colado ao logo nas primeiras telas e em toda peça de divulgação. Em espaços curtos, o nome vira “Simplifica”.

**Projeto aberto.** O código sai sob a **licença AGPL-3.0-or-later** e o conteúdo dos guias sob **Creative Commons BY-SA**, no ecossistema HarpiaFOSS.

### O problema

- **Fragmentação.** Os serviços estão espalhados em dezenas de apps e sites (Meu INSS, FGTS, CTPS Digital, Receita, SUS Digital, apps de trânsito), e a pessoa não sabe qual abrir.
- **Acesso.** Os níveis de conta (bronze, prata, ouro) confundem, a validação facial falha e a senha esquecida trava tudo.
- **Linguagem.** Palavras como “requerimento”, “exigência” e “protocolo” não dizem o que a pessoa precisa fazer.
- **Esferas.** RG, IPVA e IPTU são estaduais ou municipais, mas o cidadão procura tudo no gov.br.
- **Golpes.** Mensagens falsas de “valores a receber” e “CPF irregular” exploram exatamente essa confusão.

### A proposta

Organizar o governo pela **situação de vida** da pessoa, e não pelo órgão. Cada serviço vira uma página curta: o que é, se a pessoa tem direito, o que separar, o passo a passo com imagens, o botão para o site oficial e o que fazer se der erro.

### O que o Simplifica Brasil nunca faz

- Pedir CPF, senha, código de verificação ou foto de documento.
- Fazer login ou qualquer ação em nome do usuário.
- Imitar cores, brasão ou nome do governo.
- Mandar mensagem pedindo dados, no WhatsApp ou em qualquer canal.

## Público e personas

O público é amplo: pessoas de **36 anos ou mais** com pouca familiaridade com serviços digitais, que usam bem o WhatsApp mas travam em sites do governo. Dentro dele, o caso mais difícil continua sendo a referência de projeto: pessoas de 55 a 85 anos com celular Android de entrada. Se a Dona Lurdes conseguir usar sozinha, todo mundo consegue. A faixa de 36 a 55 anos traz uma diferença importante: muita gente ali usa o celular o dia inteiro para trabalhar (entregas, vendas, aplicativos), mas nunca precisou resolver nada no governo e tem medo de errar ou cair em golpe.

| Persona                            | Idade | Contexto                                                                        | Precisa de                                                         | Trava em                                                    |
|:-----------------------------------|:------|:--------------------------------------------------------------------------------|:-------------------------------------------------------------------|:------------------------------------------------------------|
| Dona Lurdes, aposentada            | 72    | Android simples, letra grande ligada, usa WhatsApp e YouTube                    | Extrato do INSS, cartão do SUS, recuperar a senha gov.br           | Senha esquecida, validação facial, telas com muito texto    |
| Seu Antônio, recém-desempregado    | 58    | Celular compartilhado com a esposa, pouca paciência para sites                  | Seguro-desemprego, saque do FGTS, Carteira de Trabalho Digital     | Não sabe por onde começar nem quais documentos separar      |
| Cláudia, filha que ajuda a família | 38    | Resolve tudo para a mãe e a sogra pelo próprio celular                          | Um link para mandar no grupo da família explicando o passo a passo | Explicar a mesma coisa várias vezes por áudio               |
| Marcos, entregador por aplicativo  | 44    | Usa o celular o dia todo, mas só WhatsApp e apps de trabalho; pouco tempo livre | Valores a Receber, Imposto de Renda, CPF regular                   | Medo de golpe, textos longos, não sabe qual app é o oficial |
| Sueli, diarista                    | 49    | Celular com pouco espaço e plano de dados limitado                              | CadÚnico, Bolsa Família, Tarifa Social de Energia                  | Termos como “atualização cadastral”, pouca internet         |
| Rafael, MEI                        | 31    | Vende pelo Instagram, tudo pelo celular                                         | DAS, declaração anual, nota fiscal                                 | Prazos e termos técnicos                                    |

A Cláudia é a persona de distribuição: ela descobre o Simplifica Brasil e manda o link para a família. Por isso, toda página precisa ter um botão “Mandar para alguém” e funcionar bem quando aberta direto de um link no WhatsApp.

**Pressupostos a validar:** celulares com 2 GB a 3 GB de RAM, internet instável, fonte do sistema ampliada em 130% ou mais e leitores de tela (TalkBack) em parte dos usuários.

## Princípios de design para todas as idades

Cada tela tem uma única tarefa, letra grande, botões enormes e uma imagem que confirma o que o texto diz. As regras abaixo vêm de práticas consolidadas: pesquisas do Nielsen Norman Group com usuários idosos, o GOV.UK Design System, a WCAG 2.2 e o eMAG (padrão brasileiro de acessibilidade do governo).

### Sobre as imagens

Imagens ajudam muito, desde que sigam três regras:

1.  **Ícone sempre com texto.** Um ícone sozinho é adivinhação; com o rótulo ao lado, ele acelera o reconhecimento.
2.  **Imagem que confirma, não que enfeita.** No passo a passo, a imagem mostra a tela real do app oficial com o botão certo destacado. Isso vale mais que qualquer ilustração.
3.  **Ícones simples e consistentes.** Traço grosso, formas reconhecíveis (carteira para trabalho, casa para moradia, coração para saúde) e sempre os mesmos para o mesmo assunto.

### Regras de interface

| Elemento      | Regra                                                                                                      | Por quê                                                                        |
|:--------------|:-----------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------|
| Texto         | Base de 18 px, títulos de 24 px ou mais, entrelinha 1,5                                                    | Visão reduzida e presbiopia são comuns após os 50                              |
| Contraste     | Mínimo 7:1 no texto principal (WCAG AAA)                                                                   | Tela de celular barato, uso sob sol                                            |
| Botões        | Altura mínima de 56 px, largura total, texto em verbo (“Ver o passo a passo”)                              | Dedo menos preciso, tremor leve                                                |
| Navegação     | No máximo três toques até o passo a passo; botão “Voltar” escrito, sempre no mesmo lugar                   | Menu escondido (ícone de três linhas) e gestos de deslizar não são descobertos |
| Ritmo         | Nada some sozinho: sem carrossel, sem tempo limite, sem pop-up                                             | Leitura mais lenta não pode ser punida                                         |
| Linguagem     | Frases com até 20 palavras, “você”, voz ativa, termo técnico explicado na primeira vez                     | Linguagem simples é o maior ganho de usabilidade possível                      |
| Áudio         | Botão “Ouvir esta página” em todo conteúdo, com voz do próprio celular                                     | Quem prefere áudio, como já faz no WhatsApp                                    |
| Progresso     | “Passo 2 de 5” visível em toda etapa                                                                       | Reduz a ansiedade e o abandono                                                 |
| Erros         | Cada serviço tem “Deu errado? Veja o que fazer” com os problemas mais comuns                               | O medo de errar é a maior barreira                                             |
| Leveza        | Página com menos de 150 KB, funciona com internet fraca e depois offline                                   | Plano de dados limitado e sinal ruim                                           |
| Ajustes       | Letra em três tamanhos, tema claro, escuro ou igual ao celular, menos movimento; tudo salvo só no aparelho | Cada pessoa enxerga de um jeito; o celular já pode estar no modo escuro        |
| Ação concreta | Nos passos, o botão diz “Fiz isso” em vez de “Próximo”                                                     | Confirma o que a pessoa fez e evita pular etapas                               |

### Tom de voz

Calmo, gentil e direto, como um neto paciente explicando. Nunca infantilizar: a pessoa é adulta e capaz, apenas não conhece o caminho. Exemplo: em vez de “Efetue a autenticação no portal”, escrever “Entre na sua conta gov.br”.

### Teste da avó

Toda tela nova passa por um teste com pelo menos três pessoas acima de 60 anos antes de ir ao ar. Critério de aprovação: elas chegam ao site oficial certo sem ajuda e sem pedir explicação.

## Concorrência e posicionamento

**Data da pesquisa: 30/10/2026.** Esta seção combina observação direta de páginas públicas do Me Explica, consulta a páginas oficiais e análise estratégica. Recursos não encontrados nas páginas consultadas são descritos como “não observados”, e não como prova de que o concorrente não os oferece.

A meta não é ser o maior guia do governo. É ajudar a pessoa a concluir, com calma e segurança, a tarefa que a levou até ali, mesmo quando o serviço real está em outro portal ou aplicativo.

### 1. Concorrente direto: Me Explica

O endereço encontrado para o produto é **https://meexplica.com.br/**. A busca não revelou um produto distinto em `meexpliqe.com.br`; portanto, a análise abaixo considera `meexplica.com.br` como o concorrente pretendido.

O Me Explica é um concorrente direto porque também organiza orientações sobre serviços públicos em linguagem simples e conduz o visitante a fontes oficiais. Na página inicial e nos guias consultados, foram observados:

- Busca por assunto e navegação por categorias, incluindo documentos, benefícios e cadastros.
- Temas populares como Gov.br, CPF, Bolsa Família, INSS e CadÚnico.
- Guias com instruções sequenciais, requisitos e orientações sobre o que fazer.
- Caminhos organizados por situações/perguntas para quem não sabe por onde começar.
- Links para páginas e serviços oficiais.
- Perguntas frequentes e alertas de segurança em guias consultados.
- Indicação de fontes oficiais, data de verificação/revisão e canal de feedback em páginas consultadas.
- Posicionamento como iniciativa independente, sem vínculo com o governo.
- Página de metodologia/transparência editorial. A página consultada descreve revisão humana e critérios editoriais.
- A página inicial informa uso de métricas agregadas e ferramentas de campanha mediante consentimento.

**Exemplos de conteúdo consultado:** criação de conta Gov.br, consulta de CPF, uso do Meu INSS e CadÚnico. Os guias consultados são predominantemente explicativos e textuais, com etapas, requisitos, links e perguntas frequentes. Isso não permite concluir que todo o site seja exclusivamente textual nem que não existam outros formatos ou funcionalidades.

**Conclusão competitiva:** linguagem simples, links oficiais, guias passo a passo, datas de revisão, perguntas frequentes e transparência editorial já aparecem no concorrente. O Simplifica Brasil não deve apresentar esses elementos isoladamente como diferenciais exclusivos.

### 2. Mapa de alternativas e concorrentes

| Alternativa                                    | O que já resolve bem                                                                                                          | Limitação ou oportunidade para o Simplifica                                                                                                                     |
|:-----------------------------------------------|:------------------------------------------------------------------------------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Me Explica**                                 | Descoberta por assunto, guias em linguagem simples, orientação sequencial, fontes oficiais e conteúdo editorialmente revisado | Oportunidade de aprofundar a execução visual guiada, com confirmação de cada etapa e prática segura antes de sair para o serviço real                           |
| **Portal Gov.br e aplicativos oficiais**       | São os canais oficiais para acessar e realizar serviços; concentram muitos serviços e informações                             | A organização por órgão/serviço pode não corresponder à situação de vida da pessoa; o Simplifica pode preparar o usuário e encaminhá-lo ao destino correto      |
| **Meu INSS e assistentes oficiais**            | Oferecem consulta e execução de serviços previdenciários; há canais digitais e atendimento assistido                          | Não substituem uma explicação independente, passo a passo e orientada a quem não conhece os termos ou o fluxo                                                   |
| **Balcão GOV.BR e atendimento presencial**     | Apoio humano para acesso a serviços digitais, conta Gov.br e desenvolvimento de autonomia                                     | Depende de presença física, disponibilidade local e horário; o Simplifica pode ajudar antes, durante ou depois da visita, sem se passar por atendimento oficial |
| **Centrais telefônicas, como 135**             | Possibilitam contato humano e orientação em determinados serviços                                                             | Espera, disponibilidade e dificuldade de visualizar a tela que a pessoa está usando                                                                             |
| **Vídeos tutoriais e redes sociais**           | Demonstração visual e formato familiar para parte do público                                                                  | Vídeos podem ficar desatualizados, ser longos ou misturar instruções com publicidade; falta uma indicação consistente da versão/data da tela                    |
| **Escritórios, despachantes e intermediários** | Podem acompanhar casos específicos e oferecer ajuda individual                                                                | Podem cobrar por tarefas que são gratuitas nos canais oficiais; há risco quando solicitam senhas ou códigos                                                     |
| **Assistentes de IA generalistas**             | Respondem perguntas variadas e adaptam a explicação ao contexto                                                               | Podem errar ou inventar etapas, não garantir que a interface esteja atualizada e não oferecer uma fonte de verdade versionada para cada captura                 |
| **Família e conhecidos**                       | Ajuda próxima, contextual e de confiança                                                                                      | A pessoa que ajuda pode não estar disponível; a orientação costuma ser repetida por áudio ou mensagem e pode ficar desatualizada                                |

### 3. Posicionamento recomendado

> **O Simplifica Brasil não é só um lugar que explica qual serviço procurar. É um acompanhante visual que prepara a pessoa, mostra onde tocar e a encaminha com segurança para o canal oficial — sem fazer nada em nome dela.**

A proposta precisa ser demonstrável no produto. Não basta afirmar “mais simples” ou “mais acessível”.

### 4. Diferenciais que valem construir

1.  **Faça Conosco — orientação visual por etapa.** Cada etapa combina uma instrução curta com uma captura real da tela oficial, destacando o botão ou campo relevante. A pessoa confirma “Fiz isso” antes de avançar. Se a tela não corresponder, pode informar que está diferente.
2.  **Treinar antes — ambiente de prática.** Simulação claramente identificada como treinamento, sem login, sem dados reais e sem conexão com o serviço oficial. A pessoa aprende o fluxo antes de executá-lo de verdade.
3.  **Capturas versionadas e governança de atualização.** Cada imagem registra serviço, aplicativo/site, versão quando disponível, data de conferência e responsável pela revisão. Capturas antigas geram alerta; relatos de divergência abrem uma tarefa de revisão. A automação pode detectar mudanças, mas a publicação deve depender de validação humana.
4.  **Segurança no momento da ação.** Antes de sair do Simplifica, mostrar o domínio oficial esperado e lembrar que o Simplifica nunca pede senha, código, CPF ou foto de documento. Avisos específicos devem aparecer nos passos em que golpes são mais prováveis.
5.  **Plano B quando algo dá errado.** Para cada serviço, explicar erros comuns, o que conferir, quando tentar novamente e qual canal oficial procurar. Não prometer resolver indisponibilidades do governo.
6.  **Acessibilidade operacional.** Texto grande, áudio, alto contraste, modo escuro, navegação por teclado, compatibilidade com TalkBack e páginas leves não são apenas opções de aparência: precisam ser testados em tarefas reais.
7.  **Compartilhamento útil para quem ajuda alguém.** Link direto para o passo específico, resumo compartilhável e modo “Vou ajudar alguém”, sem coletar credenciais ou dados pessoais.
8.  **Transparência sobre limites.** Informar claramente o que é orientação do Simplifica e o que acontece no site oficial; não afirmar elegibilidade ou resultado sem base oficial atualizada.

### 5. O que não devemos alegar

- Não dizer que somos os únicos a usar linguagem simples, passo a passo, links oficiais ou revisão editorial.
- Não afirmar que o Me Explica não tem imagens, WhatsApp, simulações ou recursos específicos sem uma auditoria completa dessas funcionalidades.
- Não dizer que o Simplifica é oficial, parceiro do governo ou integrado aos sistemas públicos.
- Não prometer que o usuário conseguirá concluir todo serviço sem ajuda; alguns casos exigem análise individual, atendimento presencial ou decisão do órgão.
- Não comparar “precisão” ou “qualidade” dos concorrentes sem testar os mesmos serviços, nas mesmas condições, com critérios documentados.

### 6. Matriz de diferenciação para validar

| Critério                                        | Me Explica (observado nas páginas consultadas) | Simplifica Brasil (proposta do PRD)  | Como provar                                                   |
|:------------------------------------------------|:-----------------------------------------------|--------------------------------------|:--------------------------------------------------------------|
| Busca por assunto e temas populares             | Sim                                            | Sim                                  | Teste de localização de serviço                               |
| Guias em linguagem simples                      | Sim                                            | Sim                                  | Teste de compreensão com público-alvo                         |
| Fontes oficiais e revisão editorial             | Sim, nas páginas consultadas                   | Sim                                  | Auditoria de links e histórico de revisão                     |
| Orientação com capturas reais por etapa         | Não foi possível confirmar de forma abrangente | Sim, requisito central               | Testar cada serviço e registrar versão/data                   |
| Confirmação de conclusão de cada etapa          | Não observada nas páginas consultadas          | Sim                                  | Teste de tarefa com usuários                                  |
| Treinamento simulado antes do serviço real      | Não observado nas páginas consultadas          | Sim, para fluxos selecionados no MVP | Validar que a simulação não coleta dados nem acessa o serviço |
| Alerta de divergência entre tela real e captura | Não observado nas páginas consultadas          | Sim                                  | Medir relatos, tempo de correção e capturas vencidas          |
| Áudio, acessibilidade e modo escuro             | Não comparar sem auditoria completa            | Requisitos do produto                | Testes WCAG, TalkBack, teclado e usuários                     |
| Ajuda para compartilhar um passo específico     | Não confirmado                                 | Sim                                  | Testar abertura direta de links compartilhados                |

### 7. Prioridade estratégica

Para o MVP, priorizar o que torna a proposta verificável:

- Fazer o fluxo **Faça Conosco** funcionar muito bem em dois serviços antes de ampliar para todos.
- Implementar **Treinar antes** nos dois fluxos já definidos no PRD: entrar no Gov.br e consultar contribuições no INSS.
- Criar processo editorial simples para revisar links e capturas, com data de conferência visível.
- Incluir “A tela está diferente?” e uma rotina de triagem para os relatos.
- Testar com pessoas do público-alvo, medindo conclusão sem ajuda, erros, tempo e confiança — não apenas satisfação.
- Manter o encaminhamento para o domínio oficial explícito e nunca intermediar login.

**Métrica principal sugerida:** percentual de participantes que chegam ao canal oficial correto e concluem a tarefa de orientação sem ajuda externa, sem inserir credenciais no Simplifica e sem confundir o produto com um serviço governamental.

### 8. O que a pesquisa muda no plano

A pesquisa foi boa porque mostrou onde **não** vale competir. Ela tem quatro consequências práticas.

**1. Alguns "diferenciais" deixaram de ser diferenciais.** Linguagem simples, data de revisão, fontes oficiais e aviso de que não somos governo continuam obrigatórios, mas são o mínimo esperado, não o motivo para alguém escolher o Simplifica. A comunicação do projeto não deve se apoiar neles.

**2. O diferencial real é "fazer junto".** O que não foi observado no concorrente, e que é difícil de copiar, é o conjunto: imagem da tela oficial com o lugar do toque, confirmação a cada etapa ("Fiz isso"), treino sem risco e um processo para manter as imagens corretas. Esse conjunto exige trabalho contínuo (as imagens envelhecem), e é justamente esse trabalho que vira a vantagem.

**3. O MVP precisa encolher.** Fazer Faça Conosco com qualidade custa caro por serviço: cada passo precisa de captura, anotação, revisão e teste com pessoas. Sete serviços de uma vez significam sete serviços mal mantidos. A seção Escopo do MVP agora divide o lançamento em ondas.

**4. Colaborar pode valer mais que competir.** O Me Explica também é independente e mira o mesmo cidadão. Como o conteúdo do Simplifica sai em licença aberta (CC BY-SA), há opções que um concorrente comercial não teria: linkar os guias deles para serviços que ainda não cobrimos, oferecer o componente de Faça Conosco ou o treino para eles usarem, ou dividir o trabalho de manter capturas. Vale uma conversa depois do lançamento, quando houver algo concreto para mostrar.

### 9. Auditoria que ainda falta

A pesquisa de 30/10/2026 olhou páginas públicas pelo navegador. Antes de publicar qualquer comparação, falta uma auditoria mais completa. Ela é um bom primeiro trabalho para alguém novo no projeto, porque não exige programar.

| O que verificar | Como fazer | Onde registrar |
|:--|:--|:--|
| Se há imagens das telas oficiais e com que frequência | Abrir 10 guias e contar quantos usam captura, e se a captura tem data | Planilha com URL, data e print |
| Desempenho em celular fraco | Lighthouse no modo celular e o tamanho transferido de 5 páginas | Relatório salvo do Lighthouse |
| Acessibilidade | Extensão axe DevTools e um teste rápido com TalkBack | Lista de problemas encontrados |
| Canais além do site | Procurar WhatsApp, redes sociais e newsletter oficiais do Me Explica | Links e data |
| Modelo de sustentação | Página "sobre", anúncios, doações, parcerias | Resumo em texto |
| Mesmas 3 tarefas nos dois produtos | Ex.: "descobrir como ver as contribuições do INSS"; medir toques e tempo até o site oficial | Tabela de resultados |

Regra: anotar URL, data, celular usado e prints. Se algo não foi encontrado, escrever "não encontrado em [data]", nunca "não existe".

## Fontes da pesquisa de concorrência

**Me Explica** - [Página inicial do Me Explica](https://meexplica.com.br/) - [Metodologia e transparência editorial](https://meexplica.com.br/transparencia/) - Guias consultados no site: criação de conta Gov.br; consulta de CPF; Meu INSS; CadÚnico. As páginas foram usadas para observar a estrutura editorial, exemplos de etapas, perguntas frequentes, fontes e datas de revisão.

**Fontes oficiais e alternativas** - [Portal Gov.br](https://www.gov.br/) - [Meu INSS](https://meu.inss.gov.br/) - [Assistente virtual Helô — Meu INSS](https://helo.dataprev.gov.br/) - [Balcão GOV.BR — informações oficiais](https://www.gov.br/governodigital/) - [Central 135 — INSS](https://www.gov.br/inss/pt-br/canais_atendimento/central-135) - [Orientações de segurança do Gov.br](https://www.gov.br/governodigital/pt-br/identidade/conta-gov-br) - [Fala.BR](https://falabr.cgu.gov.br/)

**Nota metodológica:** páginas e recursos públicos podem mudar. Antes de publicar comparações externas ou afirmar que um recurso não existe em determinado concorrente, repetir a verificação e registrar URL, data, dispositivo e evidência. A análise acima é um retrato da pesquisa feita em 30/10/2026, não uma certificação independente dos serviços.

## Escopo do MVP

O MVP sai em duas ondas. A ideia é provar o diferencial (fazer junto) em poucos serviços, medir com pessoas reais e só então ampliar. Tudo é conteúdo estático revisado à mão; não há login nem banco de dados de usuários.

**Onda 0, antes de qualquer código.** Testar a hipótese central com um protótipo em slides (ver a seção Etapa 0). Se o teste não mostrar diferença a favor do "fazer junto", o escopo das ondas seguintes muda antes de alguém gastar semanas programando.

**Onda 1, o lançamento.** Dois serviços completos, com Faça Conosco e Treinar antes:

1. **Entrar na conta gov.br e recuperar o acesso.** É a porta de quase tudo; se a pessoa não passa daqui, nenhum outro guia ajuda.
2. **Ver minhas contribuições no INSS (extrato do CNIS).** Muito procurado pelo público mais velho e com fluxo curto.

Junto deles vão as páginas que não dependem de captura: "Isso é golpe?", "Esqueci minha senha", o Kit de acesso, os ajustes de leitura e a página do canal de avisos.

**Onda 2.** Os outros cinco serviços da tabela abaixo, cada um só depois de passar no teste com usuários. Até lá, a tela inicial mostra apenas as situações que já têm conteúdo pronto. Cartões com "em breve" frustram quem tem pouca familiaridade com a internet, então eles não aparecem para o público.

**Critério para passar da Onda 1 para a 2:** pelo menos 4 de 5 participantes da rodada de testes chegam ao site oficial certo sem ajuda nos dois serviços, e o processo de atualização de capturas já resolveu pelo menos um aviso real de "tela diferente".

### Situações de vida e serviços

| Situação na tela inicial | Serviços no MVP                                                         | Site ou app oficial de destino  |
|:-------------------------|:------------------------------------------------------------------------|:--------------------------------|
| Minha conta gov.br       | Entrar, recuperar o acesso, subir para prata ou ouro                    | gov.br e app gov.br             |
| Aposentadoria e INSS     | Extrato de contribuições (CNIS), pedir aposentadoria, acompanhar pedido | Meu INSS                        |
| Perdi o emprego          | Saldo e saque do FGTS                                                   | App FGTS (Caixa)                |
| Dinheiro que posso ter   | Valores a Receber, restituição do Imposto de Renda                      | Banco Central e Receita Federal |
| Benefícios da família    | Consultar Bolsa Família, atualizar o CadÚnico                           | App Bolsa Família e CadÚnico    |
| Meus documentos          | Consultar e regularizar o CPF                                           | Receita Federal                 |

A tela inicial também mostra dois atalhos fixos: **“Esqueci minha senha”** e **“Isso é golpe?”**.

### Modelo de página de serviço

Todo serviço segue a mesma estrutura, para a pessoa aprender o caminho uma vez:

1.  **O que é**, em uma frase.
2.  **Quem tem direito**, com perguntas de sim ou não quando couber.
3.  **O que separar antes**: documentos e nível de conta gov.br exigido.
4.  **Faça Conosco**, uma etapa por tela, cada uma com imagem da tela oficial, o lugar do toque marcado e a data da conferência. Quando existir, o botão **Treinar antes** aparece junto.
5.  **Botão para o site oficial**, com o endereço escrito por extenso para a pessoa conferir.
6.  **Deu errado?** Os três a cinco erros mais comuns e o que fazer.
7.  **Ainda com dúvida?** Telefone oficial do órgão (135 do INSS, por exemplo) e quando ir presencialmente.
8.  **Revisado em**: data da última conferência do conteúdo.

### Fora do MVP

- Serviços estaduais e municipais (RG, IPVA, IPTU).
- Número de WhatsApp que conversa (bot), lembretes e “Faça Conosco pelo WhatsApp”. O canal de avisos, que só transmite, entra no MVP.
- Contas de usuário, favoritos sincronizados ou qualquer dado pessoal.
- Busca com inteligência artificial (fica para depois, com cuidado; ver Roadmap).

## Modo Faça Conosco

O Faça Conosco guia a pessoa pela tela oficial, uma etapa por vez: mostra a captura real, marca onde tocar, diz o que conferir antes de seguir e espera ela confirmar com “Fiz isso”. O desafio é manter essas imagens corretas, porque o governo muda as telas sem aviso. A solução é em camadas, das mais baratas às mais caras.

### Como cada passo é guardado

A imagem e o destaque ficam separados. Cada passo é um registro com a captura crua e uma anotação:

``` yaml
- texto: "Toque no botão Entrar com gov.br."
  imagem: capturas/meu-inss/v5.2/entrar.webp
  alvo:
    rotulo: "Entrar com gov.br"     # texto do botão na tela oficial
    caixa: { x: 12%, y: 41%, w: 76%, h: 7% }
  pare_e_confira: "O endereço no alto da tela deve ser meu.inss.gov.br"
  origem: { app: "Meu INSS", versao: "5.2", tipo: "web" }
  conferido_em: 2026-10-30
```

O destaque em caju, o número e a etiqueta “Toque aqui” são desenhados por cima, pelo próprio site. Quando a tela oficial muda, troca-se só a imagem e, se preciso, a caixa. Guardar o rótulo do botão permite achar a caixa de novo automaticamente.

### As cinco camadas de atualização

| Camada                                    | Como funciona                                                                                                                                                                                                                                                                                                                                     | Custo                                            |
|:------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------|
| 1\. Instrução pelo texto                  | O passo sempre diz o nome do botão (“toque no botão que diz Entrar com gov.br”). Se a imagem estiver velha, o texto ainda guia                                                                                                                                                                                                                    | Quase zero                                       |
| 2\. Sinais de mudança                     | Monitorar a versão dos apps oficiais nas lojas, um resumo (hash) do HTML das páginas públicas e as notícias do gov.br; o botão “A tela está diferente?” conta avisos (3 em 7 dias abre alerta)                                                                                                                                                    | Baixo                                            |
| 3\. Captura automática das telas públicas | Um robô semanal (Playwright para sites, Maestro ou Appium num emulador Android para apps) abre as telas antes do login, acha o botão pelo rótulo, tira a foto e compara com a atual (pixelmatch). Se mudou, abre uma issue com antes, depois e a nova caixa já calculada, para uma pessoa aprovar                                                 | Médio: tempo de CI e uma máquina para o emulador |
| 4\. Guardiões de serviço                  | Telas depois do login não podem ser automatizadas (exigiriam contas reais). Cada serviço tem um guardião, voluntário da comunidade ou pessoa paga, que confere com a própria conta. Uma ferramenta de captura borra automaticamente CPF, nomes e números (reconhecimento de texto e padrões) antes de qualquer envio, e uma segunda pessoa revisa | O mais caro: horas humanas                       |
| 5\. Aviso honesto                         | Captura com mais de 90 dias ou com alerta aberto ganha o aviso “Esta imagem pode estar diferente. Procure o botão escrito Entrar com gov.br”                                                                                                                                                                                                      | Zero                                             |

Ordem de implantação: camadas 1, 2 e 5 no MVP; camada 4 com dois guardiões no lançamento; camada 3 logo depois, começando pelas telas de login do gov.br, que aparecem em quase todos os serviços.

**Regras para o robô:** só telas públicas, no máximo uma visita por semana a cada página, nenhum login, respeito aos termos de uso dos sites. Conferir com orientação jurídica antes de ligar.

### Alternativa considerada e descartada

Um app Android que desenhasse o destaque por cima do app oficial, em tempo real, seria o guia perfeito. Mas ele precisaria do serviço de acessibilidade do Android, que lê e controla a tela inteira: é exatamente o mecanismo usado por malwares bancários. Ensinar o público a conceder essa permissão iria contra tudo o que o projeto ensina sobre segurança.

### Modo Treinar antes

Uma réplica simplificada da tela, desenhada pelo Simplifica Brasil, com fundo listrado e o aviso “nada aqui é de verdade”, para não ser confundida com a tela oficial nem imitar a identidade do governo. A pessoa toca onde acha certo e recebe uma resposta gentil. Errar não tem consequência. O treino usa as mesmas anotações do Faça Conosco, então é mantido junto.

### Como a pessoa alterna entre o guia e o app oficial

No celular, não dá para ver os dois ao mesmo tempo. O MVP aceita isso e facilita: cada passo é curto o bastante para ser lembrado, o botão “Ouvir” lê a instrução antes de a pessoa trocar de app, e o progresso fica salvo na página para ela voltar no mesmo passo. Na Fase 3, o Faça Conosco pelo WhatsApp resolve de outro jeito (ver abaixo).

## Operação de conteúdo

O código é a parte fácil de manter. O conteúdo é o que envelhece: o governo muda telas, regras e endereços sem avisar. Esta seção define quem cuida de quê e como uma mudança chega ao ar.

### Papéis

| Papel | O que faz | Quem pode ser |
|:--|:--|:--|
| Redator | Escreve o guia em linguagem simples, seguindo o modelo de página de serviço | Voluntário, inclusive quem não programa |
| Guardião do serviço | Confere o fluxo real com a própria conta, faz as capturas, responde aos avisos de "tela diferente" daquele serviço | Voluntário fixo por serviço ou pessoa paga |
| Revisor | Confere o texto contra a fonte oficial e testa o passo a passo; aprova o pull request | Outra pessoa, nunca quem escreveu |
| Mantenedor | Aprova mudanças em links oficiais (segunda aprovação), cuida do CI e da hospedagem | Pessoas de confiança do núcleo do projeto |

### Como uma mudança chega ao ar

1. Um aviso chega: o robô abriu uma issue, alguém tocou em "A tela está diferente?" ou um guardião percebeu a mudança.
2. O guardião confirma no site ou app oficial e faz a nova captura com dados fictícios ou borrados.
3. Abre um pull request usando o modelo abaixo.
4. O CI valida o schema, os links e a acessibilidade.
5. Um revisor testa o passo no próprio celular e aprova. Se o PR mexe em link oficial, um mantenedor dá a segunda aprovação.
6. O merge publica o site e atualiza o "Conferido em".

### Modelo de pull request de conteúdo

```markdown
## O que mudou
- [ ] Texto   - [ ] Captura   - [ ] Link oficial   - [ ] Erros comuns

## Por quê
(issue relacionada, print ou link da fonte oficial)

## Conferência
- [ ] Testei o fluxo no site/app oficial em: (data, celular, versão do app)
- [ ] As capturas não mostram nenhum dado real legível
- [ ] Frases com até 20 palavras, "você", voz ativa
- [ ] O texto cita o nome exato dos botões da tela oficial
- [ ] Atualizei `conferidoEm` e `revisadoEm`
```

### Ritmo de revisão

- **Toda semana:** triagem das issues do robô e dos avisos de "tela diferente". Meta de resolução em até 7 dias.
- **Todo mês:** cada guardião refaz o fluxo do seu serviço do começo ao fim, mesmo sem aviso.
- **A cada 90 dias:** revisão completa do texto contra a fonte oficial. O CI marca o serviço como vencido se passar disso.
- **Picos conhecidos:** antes do prazo do Imposto de Renda, de recadastramentos e em ano de eleição, revisar os serviços afetados com antecedência.

## WhatsApp e redes sociais

O WhatsApp é onde o público já está, e também onde os golpes chegam. A regra de ouro vale em qualquer canal: **o Simplifica Brasil nunca pede dados e nunca manda a primeira mensagem pedindo algo.**

### Fase 1, no MVP: WhatsApp sem coletar nenhum dado

- **Botão “Mandar para alguém”** em toda página, com mensagem pronta e prévia do link caprichada: cada serviço tem uma imagem de prévia própria que já resume o assunto.
- **Cartões para encaminhar.** Uma imagem por serviço e por golpe (“O governo nunca pede senha por mensagem”), feita para circular nos grupos de família.
- **Canal de avisos no WhatsApp.** Transmissão de mão única com golpes novos e prazos (IR, saques, recadastramentos), uma ou duas vezes por semana. Em canais, quem segue não expõe o número para o administrador; conferir a política atual antes de lançar.
- **Página “Como saber que somos nós”**, com o único link oficial do canal.

### Fase 3: um número que conversa

- **Número verificado** pela plataforma oficial do WhatsApp Business, com menu de botões e respostas tiradas só do conteúdo revisado. Nada de IA livre: além do risco de erro, a Meta tem regras específicas sobre chatbots de IA de uso geral no WhatsApp Business (conferir a política vigente).
- **Faça Conosco pelo WhatsApp.** O número manda um passo por mensagem, com a imagem marcada; a pessoa responde “pronto” e recebe o próximo. Ela alterna entre o WhatsApp e o app oficial, algo que já sabe fazer.
- **Lembretes pedidos pela pessoa** (prazo do IR, data de recadastramento), com consentimento explícito e botão de sair.
- **Custos:** a plataforma cobra por conversa iniciada pela empresa, e o número exige verificação da empresa. Prever orçamento e um responsável legal.

### Fase 4, com muito cuidado: “Isso é golpe?” por encaminhamento

A pessoa encaminha a mensagem suspeita e recebe uma análise baseada em regras (link fora da lista oficial, pedido de código, ameaça de bloqueio). É muito útil, mas recebe dados de terceiros, então só entra com descarte imediato do conteúdo, nenhuma gravação e parecer jurídico.

### X (Twitter) e outras redes

O público central usa pouco o X. Lá estão as Cláudias, a imprensa e os desenvolvedores que podem contribuir. O X serve para:

- **Picos de assunto:** prazo do IR, onda de golpe, mudança no gov.br. Um fio curto com o passo a passo em imagens e o link.
- **Responder quem reclama.** Monitorar buscas como “senha gov.br” e “Meu INSS não funciona” e responder com o guia certo, uma vez, sem insistir.
- **Recrutar guardiões e contribuidores** para o projeto aberto na HarpiaFOSS.

Para alcançar o público de 36+ diretamente, os canais mais promissores são YouTube, Facebook (grupos de aposentados e de bairro), Instagram, TikTok e Kwai. Cada guia pode virar um vídeo vertical de até 60 segundos mostrando as mesmas capturas anotadas, sempre terminando com “o link está na descrição; confira o endereço”.

### Fora da internet

Cartazes com QR code e fichas impressas em CRAS, unidades de saúde, igrejas, sindicatos, associações de bairro, bibliotecas e rádios comunitárias. Muita gente do público só confia no que alguém próximo indicou.

## Kit de acesso

O Kit de acesso resolve a senha esquecida sem guardar senha nenhuma: em cerca de 10 minutos, feito uma vez com um familiar, ele deixa configurados caminhos de entrada que não dependem de memória. O Simplifica Brasil só ensina; quem guarda e valida é o celular da pessoa e o próprio gov.br.

### Por que não existe cofre de senhas no Simplifica Brasil

A conta gov.br dá acesso a FGTS, INSS, assinatura digital e dados de saúde. Um cofre central com essas senhas seria o alvo mais valioso possível para atacantes, e ensinar que “a senha fica no WhatsApp” prepara a pessoa para cair em golpe. O lugar certo para a senha é o gerenciador do próprio celular, que é criptografado e protegido pela digital ou pelo rosto.

### As quatro etapas do Kit

1.  **Entrar pelo banco.** Verificar se o banco da pessoa está entre os credenciados no gov.br e ensinar a entrar por ele. Quem lembra a senha do banco não precisa lembrar outra.
2.  **Salvar no celular.** Ativar o Gerenciador de Senhas do Google (Android) ou as Senhas do iPhone e salvar a senha gov.br nele. Na próxima vez, o celular preenche sozinho com a digital.
3.  **Testar a recuperação pelo rosto.** Instalar o app gov.br e fazer um teste de recuperação com reconhecimento facial enquanto o familiar está junto, com boa luz e sem óculos escuros.
4.  **Ficha de acesso impressa.** Gerar uma ficha para guardar em casa, junto dos documentos, que diz **como** a pessoa entra (“Eu entro pelo Banco X”) e **como** recupera. A senha em si só é anotada à mão, se a família decidir, e a ficha nunca vai na carteira nem é fotografada.

### Botão “Esqueci minha senha”

Uma página com três perguntas de sim ou não leva ao caminho de recuperação mais fácil para aquela pessoa: pelo banco, pelo rosto no app gov.br ou presencialmente. Cada caminho tem o seu passo a passo com imagens.

### Modo “Vou ajudar alguém”

Uma versão do Kit pensada para a Cláudia: um roteiro para fazer junto com a mãe, presencialmente ou por chamada de vídeo, com o que falar em cada etapa. Nenhuma informação passa pelo Simplifica Brasil.

**A conferir antes do lançamento:** a lista atual de bancos credenciados no login gov.br e os métodos de recuperação disponíveis no app, que mudam com frequência.

## Telas do MVP

São 11 telas, cada uma em modo claro e escuro (na Onda 1, as telas de serviço mostram só os dois serviços do lançamento), e qualquer serviço fica a no máximo três toques da tela inicial. O desenho visual está no PDF **Simplifica Brasil · Telas do MVP v2**, junto com a prancha da marca.

| Tela                          | O que mostra                                                                                                                         | Ação principal                         |
|:------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------|
| 1\. Início                    | Saudação, busca, 6 cartões de situação, atalhos “Esqueci minha senha” e “Isso é golpe?”, cartão do canal de avisos e botão “Letra”   | Tocar numa situação                    |
| 2\. Situação                  | Título da situação e a lista dos serviços dela, cada um com uma frase                                                                | Tocar num serviço                      |
| 3\. Serviço                   | O que é, quanto custa, quanto tempo leva, o que separar, data da conferência e versão do app                                         | “Fazer comigo, passo a passo”          |
| 4\. Faça Conosco              | “Passo 2 de 5”, captura da tela oficial com o toque marcado, instrução curta, caixa “Pare e confira” e link “A tela está diferente?” | “Fiz isso” e “Voltar”                  |
| 5\. Treinar antes             | Tela de treino listrada, pergunta “Onde você tocaria?” e resposta gentil                                                             | “Próximo treino” ou “Fazer de verdade” |
| 6\. Saída para o site oficial | Aviso de saída, endereço por extenso e o lembrete de conferir letra por letra                                                        | “Abrir o site oficial”                 |
| 7\. Deu errado?               | Os erros mais comuns, agora com “A tela está diferente da imagem”                                                                    | Tocar no erro parecido                 |
| 8\. Esqueci minha senha       | Três perguntas de sim ou não                                                                                                         | Responder                              |
| 9\. Isso é golpe?             | O que o governo nunca faz, o que fazer e o que fazer se já caiu                                                                      | “Mandar este aviso para a família”     |
| 10\. Simplifica no WhatsApp   | Canal de avisos, mandar página, “Fazer comigo pelo WhatsApp (em breve)” e “Como saber que somos nós”                                 | “Seguir o canal de avisos”             |
| 11\. Ajustes de leitura       | Tamanho da letra, tema claro, escuro ou igual ao celular, velocidade da voz e menos movimento                                        | “Pronto”                               |

A tela 6 é proposital: ela ensina, toda vez, a conferir o endereço antes de digitar qualquer senha. Assim o próprio uso do Simplifica Brasil treina a pessoa contra páginas falsas.

## Identidade

A marca precisa parecer acolhedora e confiável sem jamais lembrar o governo. Por isso, nada de azul gov.br, verde e amarelo ou brasão.

### Nome e assinatura

- **Nome:** Simplifica Brasil; “Simplifica” em espaços curtos. Escrito em minúsculas no logo.
- **Assinatura:** “O governo explicado sem complicação.”
- **Aviso fixo no rodapé:** “O Simplifica Brasil não é do governo e nunca pede seus dados.”

### Conceito do logo

O balão de conversa arredondado continua, porque ele carrega a ideia da explicação paciente. A mão deu lugar a um caminho em forma de S: ele sai do alto do balão, faz as curvas e termina num ponto caju, que é a chegada. A leitura é a do produto: um caminho complicado que vira um trajeto só, até o lugar certo. O desenho usa traço grosso e poucas formas, e tem uma versão simplificada para 16 px. No modo escuro, o balão fica lilás e o caminho fica da cor do fundo.

### Paleta

| Papel          | Cor                                   | Uso                                        | Contraste                      |
|:---------------|:--------------------------------------|:-------------------------------------------|:-------------------------------|
| Fundo          | Branco \#FFFFFF                       | Fundo das telas                            | Base                           |
| Texto          | Tinta \#1F1D24                        | Todo o texto                               | Acima de 16:1                  |
| Texto de apoio | Cinza \#5A5562                        | Explicações curtas                         | Cerca de 7:1 no branco         |
| Primária       | Jabuticaba \#4A1D5C                   | Botões principais, logo, ícones            | Cerca de 13:1 com texto branco |
| Destaque       | Caju \#F2A33A                         | Dicas, destaque na imagem, detalhe do logo | Cerca de 8:1 com texto Tinta   |
| Caixas         | Areia \#F2EFE9 e lilás claro \#F3ECF6 | Fundo de caixas e de ícones                | Usadas só como fundo           |
| Alerta         | Vinho \#8B1A1A sobre rosa \#FDECEC    | “Isso é golpe?” e avisos                   | Acima de 7:1                   |

### Modo escuro

O tema segue o do celular por padrão, e a pessoa pode fixar claro ou escuro nos ajustes. As cores foram escolhidas para manter o mesmo jeito da marca e o contraste AAA.

| Papel          | Cor                                                                           | Contraste medido             |
|:---------------|:------------------------------------------------------------------------------|:-----------------------------|
| Fundo          | Noite \#18141C                                                                | Base                         |
| Texto          | Névoa \#F4F0F7                                                                | 16,1:1 no fundo              |
| Texto de apoio | \#CFC7D8                                                                      | 11,1:1 no fundo              |
| Caixas         | Ameixa \#231D29; fundo de ícone \#2D2236                                      | Usadas só como fundo         |
| Primária       | Lilás \#C9A3DB, com texto Noite nos botões                                    | 8,4:1                        |
| Destaque       | Caju \#F2A33A (o mesmo do modo claro); dica com texto \#F4F0F7 sobre \#2B2116 | 8,7:1 no fundo; 14:1 na dica |
| Alerta         | Rosa \#FFB4B4 sobre \#2E1517                                                  | 10,1:1                       |

Regras: nada de preto puro nem branco puro (cansa a vista), as capturas de tela oficiais aparecem como são (sem inverter cores) dentro de uma moldura clara, e o logo troca de versão junto com o tema.

O roxo-jabuticaba foi escolhido por ser caloroso, brasileiro e distante do azul do gov.br e do verde e amarelo.

### Tipografia

Atkinson Hyperlegible, fonte gratuita criada pelo Braille Institute para pessoas com baixa visão. Ela diferencia letras que costumam se confundir, como I, l e 1. Fallback: system-ui.

## Arquitetura técnica

O Simplifica Brasil é um site estático: o conteúdo vive em arquivos Markdown, passa por verificações automáticas e vira páginas leves servidas por CDN. Sem servidor de aplicação e sem banco de dados, sobra pouca superfície de ataque.

\[embedded content: arquitetura · do repositório ao celular\]

O portão central é o CI: nenhuma página com link fora da lista de domínios oficiais chega ao ar. As linhas tracejadas mostram o que alimenta a observabilidade.

| Camada           | Escolha                                                                                                                          | Motivo                                                                 |
|:-----------------|:---------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------|
| Framework        | Astro em modo estático                                                                                                           | HTML pronto, quase nenhum JavaScript                                   |
| Linguagem        | TypeScript estrito                                                                                                               | Erros aparecem no build, não no celular                                |
| Conteúdo         | Markdown com schema Zod                                                                                                          | Revisável por quem não programa e validado automaticamente             |
| Busca            | Pagefind                                                                                                                         | Roda no navegador, offline, sem servidor                               |
| Offline          | Service worker                                                                                                                   | Funciona com sinal ruim depois da primeira visita                      |
| Voz              | Web Speech API                                                                                                                   | Usa a voz do próprio celular, sem custo e sem enviar texto a terceiros |
| Compartilhar     | Web Share API, com link wa.me de reserva                                                                                         | Abre o WhatsApp direto                                                 |
| Tema e leitura   | `prefers-color-scheme` mais escolha salva em `localStorage`; tamanho da letra por variável CSS                                   | Funciona sem servidor e sem dado pessoal                               |
| Capturas         | Imagens WebP versionadas no repositório, anotações em YAML validadas por schema, destaque desenhado em SVG                       | Trocar a imagem não exige redesenhar o destaque                        |
| Robô de capturas | Playwright (sites) e Maestro ou Appium em emulador Android (apps), pixelmatch para comparar, issue automática com antes e depois | Detecta tela mudada sem esperar reclamação                             |
| Hospedagem       | Estática com cabeçalhos próprios (Cloudflare Pages, Netlify ou servidor com Caddy)                                               | Precisa permitir CSP e HSTS                                            |
| Código           | Repositório no ecossistema HarpiaFOSS, com espelho opcional no GitHub                                                            | Projeto aberto e auditável                                             |

**A conferir:** se o Codeberg Pages permite cabeçalhos HTTP próprios. Se não permitir, o site sai por outra hospedagem e o código continua no Codeberg.

## Etapa 0: testar a hipótese antes de programar

Todo o produto está apoiado numa aposta: **guiar a pessoa com a imagem da tela oficial, o lugar do toque marcado e uma confirmação a cada passo ajuda mais do que um bom guia em texto.** O Me Explica e outros já fazem guias em texto. Se a aposta estiver errada, o Simplifica vira só mais um guia, e isso precisa ser descoberto antes de construir o site, não depois.

A boa notícia é que essa aposta pode ser testada sem nenhuma linha de código, em cerca de duas semanas e quase sem custo.

### A hipótese, escrita de forma testável

> Pessoas de 36 anos ou mais, com pouca familiaridade com serviços digitais, que usam o modo Faça Conosco (imagem da tela oficial com o toque marcado e "Fiz isso" a cada passo) **acertam mais** onde tocar, **conferem mais** o endereço oficial e **se sentem mais seguras** do que quando usam um guia só com texto.

Escrever assim ajuda porque define, antes do teste, o que vai ser medido: acerto, conferência do endereço e segurança. Sem isso, qualquer resultado parece confirmar a ideia.

### O que montar: dois protótipos para cada serviço

Para cada um dos dois serviços da Onda 1 (entrar no gov.br e ver as contribuições do INSS), monte duas versões:

| Versão | Como é | Para que serve |
|:--|:--|:--|
| **A: só texto** | Os mesmos passos, escritos em linguagem simples, sem imagens. É o formato de um bom guia comum | É a comparação justa. Se o texto já resolver, o Faça Conosco não se paga |
| **B: Faça Conosco** | Um slide por passo: captura da tela, destaque caju no botão, instrução curta, caixa "Pare e confira" e botão "Fiz isso" | É a aposta |

Junto da versão B, monte também dois ou três slides do **Treinar antes**: a tela listrada de treino, com uma área clicável no botão certo que leva a um slide "Isso! É esse botão", e o resto da tela levando a "Quase! Procure o botão escrito Entrar com gov.br".

**Ferramentas, da mais simples para a mais completa:**

- **Google Slides ou Apresentações do celular.** Cada passo é um slide; formas transparentes por cima do botão "Fiz isso" viram links para o próximo slide. Abre no celular em modo apresentação.
- **Figma (plano gratuito).** Permite protótipo com áreas clicáveis ("hotspots") e roda no app do Figma no celular, com cara de app de verdade.
- **O PDF das telas do MVP** já tem o visual pronto; dá para recortar as telas e usar como base.

Use o tamanho de um celular real (por exemplo, 390 × 844) e teste no celular, não no computador. Letra e botões precisam seguir as regras de interface do PRD, senão o teste mede um protótipo ruim, e não a ideia.

### Como fazer as capturas para o protótipo

- **Telas públicas** (página inicial do Meu INSS, tela de entrada do gov.br): capture direto pelo celular.
- **Telas depois do login**: capture com a conta de alguém do time e **borre todos os dados** (nome, CPF, datas, valores, vínculos) antes de colocar no slide. Melhor ainda: redesenhe a tela por cima com dados claramente fictícios ("Maria Exemplo").
- **Nunca** use a conta ou os dados de quem vai participar do teste.
- Anote a data e o celular de cada captura. Se o teste der certo, essas capturas viram as primeiras do repositório.

### Como conduzir o teste

**Quem:** de 5 a 6 pessoas do público, recrutadas como descrito no Plano de validação com usuários. Seis é melhor que cinco porque divide igualmente as ordens abaixo.

**Cada pessoa vê as duas versões, uma em cada serviço,** e a ordem se alterna. Isso evita que a segunda tarefa pareça mais fácil só porque a pessoa já aprendeu com a primeira.

| Participante | Primeira tarefa | Segunda tarefa |
|:--|:--|:--|
| 1, 3 e 5 | gov.br com a **versão A** | INSS com a **versão B** |
| 2, 4 e 6 | gov.br com a **versão B** | INSS com a **versão A** |

**A tarefa termina sem login.** Depois de passar pelo protótipo, a pessoa abre no próprio celular a página pública oficial (por exemplo, a tela inicial do Meu INSS) e responde, apontando com o dedo, sem digitar nada:

1. "Onde você tocaria agora?"
2. "Como você sabe que este é o site certo?" (a resposta esperada é conferir o endereço)
3. "O que vem depois?"

**Perguntas finais:** "De 1 a 5, quanto você se sentiu segura para fazer isso sozinha, em cada versão?", "Qual das duas você mandaria para alguém da família? Por quê?", "Quem você acha que fez isso?" e, para quem viu o treino, "O treino ajudou ou atrapalhou?".

### O que anotar

| Medida | Versão A | Versão B |
|:--|:--|:--|
| Apontou o botão certo na tela oficial | sim/não | sim/não |
| Conferiu o endereço sem ser lembrada | sim/não | sim/não |
| Explicou o próximo passo corretamente | sim/não | sim/não |
| Segurança (1 a 5) | nota | nota |
| Pontos em que travou | tela e frase dita | tela e frase dita |

Mais a preferência da pessoa e o motivo, com as palavras dela.

### Decidir o critério antes de testar

Escreva no relatório, **antes** do teste, o que conta como resultado. Decidir depois de ver os dados é a forma mais comum de enganar a si mesmo.

- **A aposta se confirma** se a versão B tiver mais acertos que a A em pelo menos duas das três primeiras medidas, a segurança média for maior e a maioria preferir B. Segue para a Onda 1 como está.
- **Resultado misto** (por exemplo, B ajuda a achar o botão mas a pessoa se perde com muitos slides): ajustar o formato (menos passos, imagem maior, áudio) e repetir com outras pessoas antes de programar.
- **A aposta não se confirma** se A e B empatarem ou A for melhor. Nesse caso, o diferencial precisa mudar. Candidatos: o modo Treinar antes sozinho, o "Vou ajudar alguém" para a família, ou foco em segurança contra golpe. O relatório deve dizer qual caminho seguir.

Com cinco ou seis pessoas, isso não é estatística: é um sinal forte para decidir. Por isso as frases ditas e os pontos em que as pessoas travaram valem tanto quanto as contagens.

### Cronograma e entregáveis

| Quando | O quê |
|:--|:--|
| Dias 1 a 5 | Capturas, textos das duas versões, protótipos no celular, roteiro e termo de consentimento; um teste piloto com alguém conhecido para achar falhas no roteiro |
| Dias 6 a 9 | Recrutamento e agendamento |
| Um dia (uma tarde ou duas) | Os testes, cerca de 40 minutos por pessoa |
| Dias seguintes | Relatório de uma página e a decisão registrada como ADR em `docs/decisoes/` |

Custo: o agradecimento aos participantes e o tempo do time. O relatório e os protótipos entram no repositório e na proposta para a HarpiaFOSS.

## Relação com a HarpiaFOSS e o ssrfcheck

O Simplifica Brasil quer nascer dentro da comunidade HarpiaFOSS. Antes de pedir que a comunidade revise e apoie um projeto novo, faz sentido contribuir com algo que ela já mantém. O candidato natural é o [ssrfcheck](https://github.com/harpiafoss/ssrfcheck): uma biblioteca JavaScript, sem dependências, que diz se uma URL é segura para o servidor acessar ou se aponta para algo interno (localhost, IPs privados, endereços de metadados de nuvem, IPs disfarçados em decimal ou octal).

### Ordem das coisas

1. **Contribuir com o ssrfcheck** (abaixo). É pequeno e cria relação com quem mantém a comunidade.
2. **Fazer a Etapa 0** em paralelo, porque ela não depende de código.
3. **Abrir a proposta do Simplifica** como issue no repositório `discuss` da HarpiaFOSS, em inglês, já com o PR do ssrfcheck e o resultado da Etapa 0 como prova de trabalho.
4. **Começar o código**, seguindo o Guia de implementação.

### A contribuição: deixar a CLI do ssrfcheck pronta para scripts e agentes

Numa leitura do código em 30/10/2026, a CLI (`src/cli.js`) tinha quatro limitações. Confira o repositório de novo antes de começar, porque ele pode ter mudado.

| Como estava | Por que é um problema | Proposta |
|:--|:--|:--|
| Sempre termina com código de saída 0, mesmo para URL perigosa | Scripts e CI usam o código de saída para saber se algo passou. `ssrfcheck "$URL" && curl "$URL"` acessaria a URL perigosa | Sair com 1 para URL perigosa e 2 para erro de uso |
| Imprime só `Safe` ou `Danger!` | Programas e agentes de IA precisam de saída estruturada e do motivo | Opção `--json`, por exemplo `{"url":"...","safe":false}`; o motivo pode vir depois, se o mantenedor topar mexer na biblioteca |
| Não tem `--help` | Ninguém descobre as opções sem ler o código | `--help` com descrição e exemplos |
| Sem argumentos, não imprime nada | Parece que funcionou | Mensagem de uso e saída com código 2 |

**Como fazer bem feito:**

- **Abra uma issue antes do PR**, em inglês, explicando o problema do código de saída com um exemplo de uma linha e perguntando se o mantenedor aceita a mudança.
- **Avise que é uma mudança que pode quebrar** quem dependia do código 0. É improvável, mas a decisão de lançar como versão nova "maior" (major) é do mantenedor.
- **Siga o estilo do projeto:** zero dependências (nada de commander ou yargs; leia `process.argv` à mão, como o arquivo já faz), testes no mesmo formato da pasta `tests` e tudo em inglês.
- **PR pequeno.** Código de saída, `--help` e mensagem sem argumentos num PR; `--json` em outro, se o mantenedor preferir.

### Onde o Simplifica usa o ssrfcheck

A contribuição não é só boa vizinhança: o Simplifica tem três pontos onde o ssrfcheck é útil.

**1. Proteger o robô de capturas.** O robô visita URLs que estão no repositório. Se um pull request malicioso colocasse ali um endereço interno (como `http://169.254.169.254/`, que em vários provedores de nuvem devolve credenciais da máquina), o próprio CI poderia ser usado para vazar segredos. Isso é um ataque de SSRF contra a infraestrutura do projeto. A allowlist já bloqueia, e o ssrfcheck entra como segunda barreira (defesa em profundidade): se algum dia alguém afrouxar a allowlist, o robô ainda recusa endereços internos.

```js
// robo/pode-visitar.mjs
import { isSSRFSafeURL } from 'ssrfcheck';
import { HOSTS_OFICIAIS } from './hosts-oficiais.mjs';

export function podeVisitar(endereco) {
  const url = new URL(endereco);
  return (
    url.protocol === 'https:' &&            // só HTTPS
    HOSTS_OFICIAIS.includes(url.hostname) && // primeira barreira: lista fechada
    isSSRFSafeURL(endereco)                  // segunda barreira: nada interno ou disfarçado
  );
}
```

**2. No CI, pela linha de comando.** Com a contribuição aceita, um passo do CI pode validar todas as URLs do conteúdo de uma vez e falhar pelo código de saída, sem script extra. É exatamente o uso que motivou o PR.

**3. Na Fase 4, no "Isso é golpe?" por encaminhamento.** Links que apontam para IP em vez de nome, ou para IPs disfarçados, são um sinal forte de mensagem suspeita. O ssrfcheck analisa a URL **sem acessá-la**, o que é essencial: o Simplifica nunca deve abrir um link de golpe.

**O que o ssrfcheck não faz:** ele não detecta phishing comum (um site falso num domínio normal, como `inss-beneficio.com`, passa como "seguro" para ele, porque não é um endereço interno). Também não acompanha redirecionamentos nem resolve DNS. No Simplifica, ele é uma camada a mais, nunca a única.

### Esboço da proposta no `discuss`

A issue deve ser curta e mostrar trabalho feito:

```markdown
# RFC: Simplifica Brasil — a plain-language guide to Brazilian government services

## Problem
Government services are split across dozens of sites and apps. People 36+ with
low digital familiarity get stuck on login, jargon and scams.

## Proposal
A static, open-source guide (AGPL-3.0-or-later code, CC BY-SA content) that walks people
through official screens step by step and sends them to the official site.
It never asks for personal data and never logs in on anyone's behalf.

## What is already done
- PRD (link)
- Hypothesis test with N participants (link to one-page report)
- ssrfcheck CLI contribution (link to PR), which the project will use

## MVP scope
Two services (gov.br login and INSS contributions), static site, no backend.

## What we are asking for
Reviewers for security and accessibility, and volunteers to keep screenshots up to date.
```

## Guia de implementação para quem está começando

Esta seção explica, em ordem, como transformar o PRD em código. Ela assume que você já sabe JavaScript e o básico de HTML e CSS, mas nunca usou Astro. Os exemplos são pontos de partida: confira a documentação oficial de cada ferramenta, porque versões mudam.

### A ideia geral em uma frase

O site é **estático**: na hora do build, o Astro lê os arquivos Markdown de cada serviço, valida se estão completos, monta páginas HTML prontas e joga tudo numa pasta `dist/`. Essa pasta é publicada numa CDN. Não existe servidor rodando código quando o usuário acessa, então não existe banco para vazar nem servidor para invadir.

```
Markdown dos serviços ──► Astro (build) ──► HTML pronto em dist/ ──► CDN ──► celular
        ▲                     │
   quem escreve           validação (Zod),
   conteúdo               checagem de links,
                          testes no CI
```

### Estrutura de pastas sugerida

```
simplifica-brasil/
├── src/
│   ├── content/
│   │   └── servicos/
│   │       ├── govbr-entrar.md          # um arquivo por serviço
│   │       └── inss-contribuicoes.md
│   ├── content.config.ts                # o "contrato" de cada serviço (schema)
│   ├── config/
│   │   └── dominios-oficiais.ts         # a allowlist de links
│   ├── components/
│   │   ├── LinkOficial.astro            # único jeito de linkar para fora
│   │   ├── PassoComImagem.astro         # captura + destaque desenhado por cima
│   │   └── BotaoOuvir.astro
│   ├── layouts/Base.astro               # cabeçalho, rodapé, aviso "não é do governo"
│   ├── pages/
│   │   ├── index.astro                  # tela inicial
│   │   └── servicos/[slug].astro        # página de cada serviço
│   └── styles/tokens.css                # cores, fontes, espaçamentos
├── public/
│   ├── capturas/                        # imagens das telas oficiais
│   └── tema.js                          # aplica tema e letra antes de pintar a tela
├── robo/                                # robô de capturas (roda no CI)
├── tests/                               # Playwright + axe
└── .github/workflows/                   # CI
```

### Passo 1: o contrato de cada serviço (schema com Zod)

Um **schema** descreve o formato que um dado precisa ter. Se alguém esquecer o link oficial ou escrever uma data errada no Markdown, o build quebra com uma mensagem clara, antes de o erro chegar ao celular de alguém. É isso que permite que pessoas que não programam escrevam conteúdo com segurança.

```ts
// src/content.config.ts (Astro 5; confira a sintaxe na versão que você usar)
import { defineCollection, z } from 'astro:content';
import { glob } from 'astro/loaders';

const caixa = z.object({
  x: z.number().min(0).max(100), // posições em porcentagem da imagem
  y: z.number().min(0).max(100),
  w: z.number().min(1).max(100),
  h: z.number().min(1).max(100),
});

const passo = z.object({
  texto: z.string().max(160),                  // instrução curta
  imagem: z.string(),                          // caminho em public/capturas
  alt: z.string(),                             // descrição para leitor de tela
  alvo: z.object({ rotulo: z.string(), caixa }),
  pareEConfira: z.string().optional(),
  origem: z.object({
    app: z.string(),
    versao: z.string().optional(),
    tipo: z.enum(['web', 'android', 'ios']),
  }),
  conferidoEm: z.coerce.date(),
});

const servicos = defineCollection({
  loader: glob({ pattern: '**/*.md', base: './src/content/servicos' }),
  schema: z.object({
    titulo: z.string(),
    situacao: z.enum(['conta-govbr', 'inss', 'emprego', 'dinheiro', 'familia', 'documentos']),
    resumo: z.string().max(200),
    oQueSeparar: z.array(z.string()).min(1),
    nivelContaGovbr: z.enum(['bronze', 'prata', 'ouro', 'qualquer']),
    passos: z.array(passo).min(1),
    linkOficial: z.string().url(),
    errosComuns: z.array(z.object({ problema: z.string(), solucao: z.string() })),
    telefoneOficial: z.string().optional(),
    revisadoEm: z.coerce.date(),
  }),
});

export const collections = { servicos };
```

Um arquivo de serviço fica assim (o que vem depois do segundo `---` é texto livre em Markdown):

```markdown
---
titulo: Ver minhas contribuições
situacao: inss
resumo: É a lista de todos os trabalhos e pagamentos que o INSS tem no seu nome.
oQueSeparar: ["Celular com internet", "CPF e senha da conta gov.br"]
nivelContaGovbr: prata
linkOficial: https://meu.inss.gov.br/
revisadoEm: 2026-10-30
passos:
  - texto: Toque no botão Entrar com gov.br.
    imagem: /capturas/inss/entrar.webp
    alt: Tela inicial do Meu INSS com o botão Entrar com gov.br no meio.
    alvo: { rotulo: "Entrar com gov.br", caixa: { x: 12, y: 41, w: 76, h: 7 } }
    pareEConfira: O endereço no alto da tela deve ser meu.inss.gov.br
    origem: { app: Meu INSS, tipo: web }
    conferidoEm: 2026-10-30
errosComuns:
  - problema: O site diz que meu nível de conta é baixo
    solucao: Veja como subir para prata pelo app do seu banco.
---
O nome oficial deste documento é extrato do CNIS.
```

> O nível de conta exigido e o endereço acima são exemplos. Confirme no site oficial antes de publicar.

### Passo 2: o componente que protege os links

Este é o componente mais importante do projeto. O maior risco do Simplifica é alguém trocar um link oficial por um site de golpe (por uma contribuição maliciosa ou uma conta invadida). Por isso, **nenhuma página escreve `<a href>` para fora diretamente**: tudo passa por aqui, e um link fora da lista quebra o build.

```ts
// src/config/dominios-oficiais.ts
// Lista de endereços EXATOS. Preferimos a lista exata a aceitar "qualquer coisa .gov.br",
// porque assim um subdomínio esquecido ou invadido não entra sem revisão.
export const HOSTS_OFICIAIS = [
  'www.gov.br',
  'sso.acesso.gov.br',
  'meu.inss.gov.br',
] as const;
```

```astro
---
// src/components/LinkOficial.astro
import { HOSTS_OFICIAIS } from '../config/dominios-oficiais';

interface Props { href: string }
const url = new URL(Astro.props.href);

const permitido =
  url.protocol === 'https:' &&
  (HOSTS_OFICIAIS as readonly string[]).includes(url.hostname);

if (!permitido) {
  // Em site estático, isso roda no build: o deploy simplesmente não acontece.
  throw new Error(`Link fora da lista oficial: ${url.href}`);
}
---
<a href={url.href} rel="noopener noreferrer external" class="botao botao-primario">
  <slot />
</a>
```

Por que comparar o `hostname` inteiro e não usar `href.includes('gov.br')`? Porque `https://gov.br.golpe.com` e `https://golpe.com/?gov.br` também "contêm gov.br". Comparar o nome exato do host com uma lista fechada não tem essa brecha.

Como segunda barreira, um script no CI lê todo o HTML gerado em `dist/` e falha se encontrar qualquer link externo fora da lista, inclusive um que alguém tenha escrito à mão no Markdown:

```js
// scripts/checar-links.mjs
import { readFileSync } from 'node:fs';
import { globSync } from 'glob';
import { HOSTS_OFICIAIS } from '../src/config/dominios-oficiais.ts';

const PROPRIO = 'simplificabrasil.com.br';
let erros = 0;
for (const arquivo of globSync('dist/**/*.html')) {
  const html = readFileSync(arquivo, 'utf8');
  for (const [, href] of html.matchAll(/href="(https?:\/\/[^"]+)"/g)) {
    const host = new URL(href).hostname;
    if (host !== PROPRIO && !HOSTS_OFICIAIS.includes(host)) {
      console.error(`${arquivo}: link não permitido -> ${href}`);
      erros++;
    }
  }
}
process.exit(erros ? 1 : 0);
```

(Importar um arquivo `.ts` direto no Node exige um carregador como `tsx`; rode com `npx tsx scripts/checar-links.mjs`, ou mantenha a lista num `.json` que os dois lados leem.)

### Passo 3: o passo com imagem e destaque

A captura é uma imagem comum. O destaque caju é um SVG desenhado **por cima**, usando as porcentagens da anotação. Como as medidas são relativas, o destaque acompanha a imagem em qualquer tamanho de tela, e trocar a captura não exige redesenhar nada.

```astro
---
// src/components/PassoComImagem.astro
const { imagem, alt, caixa } = Astro.props;
---
<figure class="passo-figura">
  <img src={imagem} alt={alt} loading="lazy" decoding="async" />
  <svg viewBox="0 0 100 100" preserveAspectRatio="none" aria-hidden="true">
    <rect x={caixa.x} y={caixa.y} width={caixa.w} height={caixa.h} rx="1.5"
          fill="none" stroke="#F2A33A" stroke-width="4"
          vector-effect="non-scaling-stroke" />
  </svg>
</figure>

<style>
  .passo-figura { position: relative; max-width: 320px; margin: 0 auto; }
  .passo-figura img { display: block; width: 100%; height: auto; }
  .passo-figura svg { position: absolute; inset: 0; width: 100%; height: 100%; }
</style>
```

Dois detalhes: `vector-effect="non-scaling-stroke"` mantém a borda com a mesma espessura mesmo com o SVG esticado, e `aria-hidden` esconde o desenho do leitor de tela, porque quem não enxerga recebe a informação pelo `alt` e pelo texto do passo.

### Passo 4: tema escuro e tamanho da letra sem quebrar a CSP

As cores vivem em variáveis CSS. O tema segue o celular por padrão e pode ser fixado pela pessoa.

```css
/* src/styles/tokens.css */
:root {
  --fundo: #FFFFFF; --texto: #1F1D24; --apoio: #5A5562;
  --primaria: #4A1D5C; --sobre-primaria: #FFFFFF; --caju: #F2A33A;
  --escala-letra: 1;
  font-size: calc(18px * var(--escala-letra));
}
@media (prefers-color-scheme: dark) {
  :root:not([data-tema="claro"]) {
    --fundo: #18141C; --texto: #F4F0F7; --apoio: #CFC7D8;
    --primaria: #C9A3DB; --sobre-primaria: #18141C;
  }
}
:root[data-tema="escuro"] {
  --fundo: #18141C; --texto: #F4F0F7; --apoio: #CFC7D8;
  --primaria: #C9A3DB; --sobre-primaria: #18141C;
}
:root[data-letra="grande"] { --escala-letra: 1.2; }
:root[data-letra="maior"]  { --escala-letra: 1.4; }
```

A escolha da pessoa precisa ser aplicada **antes** de a página aparecer, senão a tela pisca branca e depois fica escura. O jeito comum é um `<script>` dentro do HTML, mas a CSP do projeto proíbe scripts inline (é uma das proteções contra injeção). A solução é um arquivo pequeno, carregado no `<head>` sem `defer`:

```js
// public/tema.js  →  <script src="/tema.js"></script> no <head>
try {
  const tema = localStorage.getItem('tema');   // 'claro' | 'escuro' | null (igual ao celular)
  const letra = localStorage.getItem('letra'); // 'normal' | 'grande' | 'maior'
  if (tema) document.documentElement.dataset.tema = tema;
  if (letra) document.documentElement.dataset.letra = letra;
} catch (e) {
  // Navegação privada ou armazenamento bloqueado: segue com o padrão.
}
```

Guardar preferência visual no aparelho não é dado pessoal e não sai do celular, então não conflita com a regra de não coletar dados.

### Passo 5: o robô que percebe quando a tela oficial mudou

Uma vez por semana, o CI roda um script que abre as telas **públicas** (antes do login), encontra o botão pelo texto, tira uma foto e compara com a captura publicada.

```js
// robo/conferir-tela.mjs
import { chromium } from 'playwright';
import { PNG } from 'pngjs';
import pixelmatch from 'pixelmatch';
import { readFileSync, writeFileSync } from 'node:fs';

const LARGURA = 390, ALTURA = 844; // tamanho de um celular comum

export async function conferir({ url, rotulo, capturaAtual }) {
  const browser = await chromium.launch();
  const page = await browser.newPage({ viewport: { width: LARGURA, height: ALTURA } });
  await page.goto(url, { waitUntil: 'networkidle' });

  // Acha o botão pelo texto visível, do jeito que uma pessoa acharia.
  const alvo = page.getByRole('button', { name: rotulo })
    .or(page.getByRole('link', { name: rotulo })).first();
  const box = await alvo.boundingBox(); // null se o botão sumiu ou mudou de nome

  const nova = PNG.sync.read(await page.screenshot());
  const antiga = PNG.sync.read(readFileSync(capturaAtual));
  await browser.close();

  if (!box) return { mudou: true, motivo: `Botão "${rotulo}" não encontrado` };
  if (nova.width !== antiga.width || nova.height !== antiga.height) {
    return { mudou: true, motivo: 'A captura antiga foi feita com outro tamanho de tela' };
  }

  const diff = new PNG({ width: nova.width, height: nova.height });
  const pixelsDiferentes = pixelmatch(antiga.data, nova.data, diff.data,
    nova.width, nova.height, { threshold: 0.1 });
  const proporcao = pixelsDiferentes / (nova.width * nova.height);
  writeFileSync('robo/saida/diferenca.png', PNG.sync.write(diff));

  return {
    mudou: proporcao > 0.02, // mais de 2% da tela diferente
    proporcao,
    caixaSugerida: {          // já em porcentagem, pronta para a anotação
      x: +(box.x / LARGURA * 100).toFixed(1),
      y: +(box.y / ALTURA * 100).toFixed(1),
      w: +(box.width / LARGURA * 100).toFixed(1),
      h: +(box.height / ALTURA * 100).toFixed(1),
    },
  };
}
```

Quando `mudou` é verdadeiro, o workflow abre uma issue com a captura antiga, a nova, a imagem de diferença e a caixa sugerida. **Uma pessoa sempre aprova antes de publicar**: o robô detecta, não decide. Os 2% são um chute inicial; ajuste depois de algumas semanas vendo falsos alarmes (um banner de aviso no site oficial, por exemplo, já muda a imagem). Para apps Android, a mesma ideia funciona com o Maestro rodando num emulador.

Regras do robô: só telas públicas, uma visita por semana a cada página, nenhum login, e respeito aos termos de uso dos sites. Antes de visitar qualquer endereço, o robô passa pela função `podeVisitar`, que combina a allowlist com o `ssrfcheck` (ver a seção Relação com a HarpiaFOSS e o ssrfcheck).

### Passo 6: testes que provam acessibilidade

O PRD exige zero violações de acessibilidade nos dois temas. Isso vira teste automatizado com Playwright e axe:

```ts
// tests/acessibilidade.spec.ts
import { test, expect } from '@playwright/test';
import AxeBuilder from '@axe-core/playwright';

const paginas = ['/', '/servicos/inss-contribuicoes/', '/golpe/'];

for (const tema of ['light', 'dark'] as const) {
  for (const caminho of paginas) {
    test(`${caminho} sem violações no tema ${tema}`, async ({ page }) => {
      await page.emulateMedia({ colorScheme: tema });
      await page.goto(caminho);
      const resultado = await new AxeBuilder({ page })
        .withTags(['wcag2a', 'wcag2aa', 'wcag22aa'])
        .analyze();
      expect(resultado.violations).toEqual([]);
    });
  }
}
```

O axe pega problemas automáticos (contraste, botão sem nome, imagem sem `alt`), mas não diz se o texto é fácil de entender ou se a ordem de leitura faz sentido. Por isso o teste com pessoas e com TalkBack continua obrigatório.

### Passo 7: o CI, de forma resumida

Todo pull request roda, em ordem:

1. `astro check` e `astro build`: tipos e schema. Conteúdo incompleto para aqui.
2. `checar-links`: nenhum link fora da allowlist.
3. Testes de unidade (Vitest) e de interface com acessibilidade (Playwright + axe).
4. Lighthouse CI: acessibilidade 100 e orçamento de 150 KB por página.
5. Varreduras de segurança: Gitleaks (senhas esquecidas no código), OSV-Scanner (dependências com falhas conhecidas) e Semgrep (padrões de código perigosos).

Mudanças em `dominios-oficiais.ts` ou em qualquer `linkOficial` exigem duas aprovações (configurado com um arquivo `CODEOWNERS` e proteção de branch).

### Ordem de trabalho sugerida

Estimativa para uma pessoa trabalhando cerca de 10 horas por semana. É uma referência para planejar, não um prazo.

| Etapa | O que fica pronto | Como saber que acabou |
|:--|:--|:--|
| 0a. Etapa 0 (cerca de 2 semanas, sem código) | Protótipos A e B dos dois serviços, testes com 5 ou 6 pessoas, relatório e decisão | Decisão registrada em `docs/decisoes/` |
| 0b. Contribuição ao ssrfcheck (em paralelo) | Issue aberta e PR da CLI | PR revisado pelo mantenedor |
| 1. Base (2 semanas) | Projeto Astro, tokens de cor e fonte, layout com cabeçalho e rodapé, temas claro e escuro | Página em branco com o layout passa no axe nos dois temas |
| 2. Conteúdo (1 semana) | Schema Zod e os dois arquivos de serviço da Onda 1 | Build quebra se um campo obrigatório faltar |
| 3. Componentes (2 semanas) | LinkOficial, PassoComImagem, BotaoOuvir, cartões | Teste que prova que um link fora da lista quebra o build |
| 4. Telas (2 semanas) | Início, Situação, Serviço, Faça Conosco, Saída, Deu errado, Golpe, Senha, Ajustes | Fluxo completo navegável no celular |
| 5. Treino (1 semana) | Treinar antes nos dois serviços | Pessoa consegue errar e acertar sem nenhum dado sair do aparelho |
| 6. Segurança e CI (1 semana) | Workflow completo, cabeçalhos HTTP, CODEOWNERS | PR de teste com link falso é bloqueado |
| 7. Teste com pessoas (1 a 2 semanas) | Rodada de 5 participantes | Relatório com resultados e lista de correções |
| 8. Ajustes e lançamento | Correções da rodada, robô de capturas, observabilidade | Critérios da Onda 1 atingidos |

Dica para quem está começando: abra uma issue por linha da tabela, faça um pull request pequeno por vez e peça revisão cedo. Um PR de 200 linhas é revisado no mesmo dia; um de 3 000 fica parado.

## Segurança

A decisão de segurança mais importante já está no produto: o Simplifica Brasil não tem login, banco de dados nem dado pessoal, então não há o que vazar. O risco real passa a ser alguém adulterar o site para mandar pessoas a páginas falsas, e todo o resto desta seção protege contra isso.

### Modelo de ameaças

| Ameaça                                                                             | Impacto                                                                         | Defesa principal                                                                                                |
|:-----------------------------------------------------------------------------------|:--------------------------------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------|
| Link oficial trocado por um site de golpe (contribuição maliciosa, conta invadida) | Crítico: o usuário confia no Simplifica Brasil e digita a senha no lugar errado | Lista de domínios oficiais permitidos verificada no CI, revisão obrigatória por duas pessoas, commits assinados |
| Ataque à cadeia de suprimentos (pacote npm ou Action comprometida)                 | Crítico: código malicioso no site                                               | Poucas dependências, versões travadas, Actions fixadas por hash, varredura de vulnerabilidades                  |
| Invasão da conta de hospedagem, domínio ou repositório                             | Crítico: site trocado inteiro                                                   | Chave física ou app autenticador em todas as contas, bloqueio de transferência do domínio, DNSSEC               |
| Sites falsos imitando o Simplifica Brasil                                          | Alto: golpistas usam a marca                                                    | Registrar variações do domínio, aviso fixo “nunca pedimos dados”, canal de denúncia                             |
| Script injetado (XSS)                                                              | Alto                                                                            | Site estático, conteúdo em Markdown sem HTML livre, CSP rígida                                                  |
| Derrubada do site (DDoS)                                                           | Médio: o conteúdo some temporariamente                                          | CDN na frente, site estático em cache, funciona offline depois da primeira visita                               |

### Proteções no site

- **CSP rígida:** `default-src 'self'`, sem scripts inline, sem scripts de terceiros, `frame-ancestors 'none'`, com relatório de violações ligado à observabilidade.
- **Cabeçalhos:** HSTS com preload, `Referrer-Policy: no-referrer`, `X-Content-Type-Options: nosniff`, `Permissions-Policy` desligando câmera, microfone e geolocalização.
- **Links de saída:** todo link externo passa por um componente único que só aceita domínios da lista permitida (`gov.br`, `caixa.gov.br`, `bcb.gov.br` e poucos outros) e quebra o build se encontrar outro.
- **Sem cookies e sem armazenamento de dados pessoais** no navegador; o progresso no passo a passo fica só na própria página.

### Proteções no processo

- Branch principal protegida: nada entra sem revisão e testes verdes; mudanças em links oficiais exigem duas aprovações.
- Commits assinados e histórico linear.
- Varreduras automáticas a cada mudança: segredos (Gitleaks), dependências (OSV-Scanner), análise estática (Semgrep) e checagem de links.
- Lista de componentes do software (SBOM) gerada em cada versão.
- Atualizações de dependência automáticas (Renovate), aplicadas só depois de passar pelos testes.
- Arquivo `SECURITY.md` e `/.well-known/security.txt` explicando como relatar uma falha de forma responsável.

### Referências de verificação

O projeto usa o OWASP ASVS nível 2 como checklist, o OWASP Top 10 como guia de riscos de aplicações web e o modelo SLSA para a integridade do processo de build. Antes do lançamento, rodar uma revisão externa com a comunidade HarpiaFOSS.

## Observabilidade

A observabilidade responde a três perguntas: as pessoas estão conseguindo chegar ao site oficial certo, o conteúdo ainda está correto e alguém está atacando o site. Tudo é medido de forma agregada, sem cookies, sem IP guardado e sem identificar ninguém.

### O que medir

| Sinal                                 | Como medir                                                               | Alerta quando                                                                 |
|:--------------------------------------|:-------------------------------------------------------------------------|:------------------------------------------------------------------------------|
| Links oficiais quebrados ou alterados | Verificação diária de todos os links externos (lychee)                   | Qualquer link oficial com erro ou redirecionamento para domínio fora da lista |
| Conteúdo desatualizado                | Data “Revisado em” de cada serviço, conferida no CI                      | Serviço sem revisão há mais de 90 dias                                        |
| Tentativas de injeção de script       | Relatórios de violação da CSP                                            | Pico acima do normal ou violação vinda de origem nova                         |
| Disponibilidade                       | Monitor externo a cada minuto (Uptime Kuma ou Gatus)                     | Duas falhas seguidas                                                          |
| Erros de JavaScript                   | GlitchTip, alternativa aberta ao Sentry, auto-hospedada                  | Erro novo ou mais de 1% das visitas com erro                                  |
| Velocidade real                       | Core Web Vitals coletados no navegador (biblioteca web-vitals)           | LCP acima de 2,5 s no percentil 75 em celulares                               |
| Conclusão do passo a passo            | Visitas que chegam ao último passo e tocam em “Abrir o site oficial”     | Queda de mais de 30% em um serviço de uma semana para outra                   |
| Onde as pessoas travam                | Aberturas de “Deu errado?” e respostas “Isso ajudou? Não”                | Serviço com mais de 25% de “Não”                                              |
| O que falta                           | Buscas sem resultado                                                     | Revisão semanal, vira pauta de novos conteúdos                                |
| Capturas desatualizadas               | Robô semanal, versão dos apps nas lojas e botão “A tela está diferente?” | Diferença visual acima do limite ou 3 avisos em 7 dias                        |
| Uso do treino                         | Treinos iniciados e concluídos, e se a pessoa seguiu para o Faça Conosco | Taxa de conclusão abaixo de 50%                                               |

### Ferramentas

Umami ou GoatCounter para métricas de uso, ambos abertos, sem cookies e hospedados pelo próprio projeto. Os alertas vão para um canal da equipe (Matrix, Telegram ou e-mail). Um painel único reúne disponibilidade, erros, links e conclusão por serviço.

### Regra de ouro

Nenhum dado de uso pode identificar uma pessoa. O texto digitado na busca é guardado apenas quando não teve resultado, cortado em 60 caracteres e descartado se contiver números com formato de CPF, telefone ou e-mail.

## Privacidade e cuidados legais

Como o Simplifica Brasil não coleta dado pessoal, a LGPD pesa pouco no MVP, mas o projeto deve deixar isso escrito e verificável. Esta seção não substitui a orientação de um advogado, que vale consultar antes do lançamento.

- **Política de privacidade curta e honesta**, em linguagem simples: o que é medido (contagens agregadas), o que nunca é coletado e onde ficam os servidores.
- **Sem identidade visual do governo.** Usar brasão, cores ou nome que confundam pode configurar falsa identidade e viola as regras de uso da marca gov.br.
- **Capturas de tela dos apps oficiais** só com dados fictícios ou borrados por ferramenta e revisados por duas pessoas, nunca com dados reais legíveis, e com a indicação de qual app e versão aparecem. Confirmar com advogado o uso de capturas de telas oficiais para fins educativos.
- **Telas de treino** desenhadas pelo projeto, listradas e com o aviso “nada aqui é de verdade”, para não imitar a identidade do governo.
- **Robô de capturas** só em telas públicas, sem login e com poucas visitas, respeitando os termos de uso.
- **Isenção clara:** o Simplifica Brasil é um guia; a palavra final é sempre do órgão oficial, e cada página traz a data da última revisão.
- **Licenças (decidido):** código sob **AGPL-3.0-or-later** (GNU Affero General Public License, versão 3 ou qualquer versão posterior) e conteúdo sob **Creative Commons BY-SA**. São duas licenças porque código e texto têm papéis diferentes:
  - **AGPL-3.0 no código** é uma licença *copyleft*: qualquer pessoa pode usar, copiar e modificar o código, mas quem distribuir uma versão modificada precisa publicá-la sob a mesma AGPL, com o código-fonte aberto. A diferença para a GPL comum é a cláusula de rede: se alguém rodar uma versão modificada como serviço na internet, também precisa oferecer o código-fonte a quem usa. Na prática, isso garante que nenhuma empresa pegue o Simplifica, faça melhorias e feche o resultado, por exemplo para vender a prefeituras um "guia do cidadão" proprietário. Toda melhoria feita sobre o projeto continua aberta para todo mundo.
  - **O custo da escolha:** organizações que não querem abrir o próprio código vão evitar reaproveitar os componentes (o Faça Conosco, o LinkOficial, o robô de capturas). Isso inclui, possivelmente, o Me Explica, se o código dele não for compatível com a AGPL. Projetos abertos sob GPL ou AGPL podem reaproveitar sem problema. Se surgir uma parceria que dependa disso, os detentores dos direitos podem oferecer uma licença adicional caso a caso, desde que todos os autores relevantes concordem (ver "contribuições" abaixo).
  - **Dependências:** bibliotecas com licenças permissivas (MIT, BSD, Apache-2.0) podem ser usadas dentro de um projeto AGPL. O contrário é que não vale: código AGPL não pode entrar num projeto fechado. Antes de adicionar uma dependência, conferir se a licença dela é compatível; uma ferramenta como `license-checker` no CI ajuda.
  - **Site estático e a cláusula de rede:** como o Simplifica não roda código no servidor, a cláusula de rede pesa menos do que num sistema web comum. Mesmo assim, o rodapé do site terá um link "Código-fonte" para o repositório. É a forma simples e clara de cumprir a licença e reforça a transparência do projeto.
  - **CC BY-SA no conteúdo** obriga quem reaproveitar os guias a dar crédito e publicar as versões modificadas sob a mesma licença. Ela segue a mesma lógica da AGPL, aplicada ao texto.
  - **Nome e logo ficam fora das duas licenças.** A AGPL não dá direito de usar a marca. Um arquivo `TRADEMARKS.md` explica que cópias do projeto precisam de outro nome e outro logo. Isso importa para a segurança: um site falso usando o nome "Simplifica Brasil" poderia enganar pessoas.
  - **Contribuições:** cada contribuição continua pertencendo a quem a escreveu, licenciada sob AGPL. Para deixar isso registrado sem burocracia, o projeto adota o DCO (Developer Certificate of Origin): cada commit leva a linha `Signed-off-by`, gerada com `git commit -s`, em que a pessoa declara ter o direito de enviar aquele código.
  - **Como aplicar:** arquivo `LICENSE` com o texto completo da AGPL-3.0 na raiz do repositório, campo `"license": "AGPL-3.0-or-later"` no `package.json`, um cabeçalho curto com o identificador SPDX (`// SPDX-License-Identifier: AGPL-3.0-or-later`) nos arquivos de código, arquivo `LICENSE-CONTEUDO` com a CC BY-SA 4.0 na pasta de conteúdo e o link "Código-fonte" no rodapé. Confirmar com a HarpiaFOSS se há preferência ou modelo de cabeçalho para os projetos da comunidade.
- **WhatsApp na Fase 1:** o canal de avisos só transmite e não coleta número. **Na Fase 3**, com o número que conversa, a LGPD passa a valer de verdade: telefone é dado pessoal e exige consentimento explícito, finalidade definida, botão de sair e política de retenção curta.

## Plano de validação com usuários

O "teste da avó" descrito nos princípios de design é a forma mais barata de saber se o produto funciona. Esta seção detalha como fazer.

### Quantas pessoas e quem

Cinco pessoas por rodada costumam revelar a maior parte dos problemas graves de usabilidade; é mais útil fazer várias rodadas pequenas do que uma grande. Para a Onda 1, misture: três pessoas acima de 60 anos e duas entre 36 e 55 com pouca familiaridade com serviços do governo. Recrute fora do círculo do time (associações de bairro, igrejas, grupos de aposentados), porque parentes tendem a ser gentis demais.

### Antes do teste

- **Consentimento por escrito**, em linguagem simples: o que vai acontecer, que a pessoa pode parar quando quiser e que o teste é do site, não dela.
- **Nunca usar a conta real da pessoa.** As tarefas terminam na porta do site oficial (a tela de saída) ou dentro do modo Treinar antes. Ninguém digita CPF ou senha durante o teste.
- Use o celular da própria pessoa, quando ela permitir, com as configurações dela (letra grande, por exemplo). Leve um Android de entrada como reserva.
- Ofereça um agradecimento pelo tempo (lanche, transporte ou valor simbólico) e combine antes.

### Roteiro (cerca de 30 minutos)

1. **Conversa inicial (5 min):** como a pessoa resolve coisas do governo hoje e quem a ajuda.
2. **Tarefa 1 (10 min):** "Você quer ver quanto tempo já contribuiu para o INSS. Use este site para descobrir como fazer." Termina quando a pessoa toca em "Abrir o site oficial".
3. **Tarefa 2 (5 min):** "Imagine que você esqueceu a senha do gov.br. O que você faria?"
4. **Tarefa 3 (5 min):** mostrar um print de mensagem de golpe e perguntar se é verdadeira, deixando a pessoa usar o site.
5. **Perguntas finais (5 min):** "Quem você acha que fez este site?", "Você digitaria sua senha aqui?" e "Mandaria este link para alguém?".

Durante as tarefas, quem conduz **não ajuda**. Se a pessoa travar por mais de dois minutos, anote o ponto exato e só então ofereça uma dica.

### O que anotar

| Medida | Como registrar |
|:--|:--|
| Concluiu sem ajuda? | Sim, com dica ou não |
| Tempo até o site oficial | Cronômetro |
| Onde travou | Tela e elemento exato |
| Frases ditas em voz alta | Citação literal (as mais úteis do relatório) |
| Achou que era do governo? | Resposta à pergunta final |
| Confiança | "De 1 a 5, quanto você se sentiu segura para fazer isso sozinha?" |

Depois de cada rodada, um relatório de uma página com os três problemas mais graves e o que vai mudar. Corrige, testa de novo com outras cinco pessoas.

## Métricas de sucesso

**Métrica principal:** a porcentagem de participantes dos testes que chegam ao site oficial certo e concluem a orientação sem ajuda, sem digitar nenhuma credencial no Simplifica e sem achar que o Simplifica é do governo. As três condições contam juntas: chegar ao lugar certo achando que o site é oficial também é uma falha, porque ensina a confiar em qualquer página com cara de governo.

As metas abaixo valem para os três primeiros meses e devem ser recalibradas com os primeiros dados reais.

| Métrica                                                        | Meta                                               | Fonte                             |
|:---------------------------------------------------------------|:---------------------------------------------------|:----------------------------------|
| Testados acima de 60 anos que chegam ao site oficial sem ajuda | 8 em cada 10                                       | Testes presenciais de usabilidade |
| Testados que acham que o Simplifica é do governo | Nenhum; se acontecer, revisar marca e textos | Pergunta no fim do teste |
| Passo a passo concluído (chega ao botão do site oficial)       | 50% de quem abre um serviço                        | Umami ou GoatCounter              |
| Respostas “Isso ajudou? Sim”                                   | 75% ou mais                                        | Botão no fim de cada serviço      |
| Páginas compartilhadas pelo botão “Mandar para alguém”         | 10% das visitas                                    | Contagem de toques no botão       |
| Pontuação de acessibilidade no Lighthouse                      | 100 em todas as páginas, nos dois temas            | CI                                |
| Avisos de “tela diferente” resolvidos                          | Em até 7 dias                                      | Issues do robô e do botão         |
| Quem treinou e depois concluiu o Faça Conosco                  | 60% ou mais                                        | Umami ou GoatCounter              |
| Seguidores do canal de avisos                                  | Acompanhar a tendência; meta após 3 meses de dados | Painel do canal                   |
| Serviços revisados nos últimos 90 dias                         | 100%                                               | CI                                |
| Incidentes de segurança com impacto em usuário                 | Zero                                               | Observabilidade e relatos         |

## Sustentabilidade e custos

Um guia que fica desatualizado é pior do que nenhum guia, porque ensina errado com aparência de certo. Por isso, antes de crescer, o projeto precisa saber quanto custa se manter vivo. Os valores dependem de escolhas ainda abertas; a tabela mostra a ordem de grandeza.

| Item | Custo | Observação |
|:--|:--|:--|
| Domínio e variações (contra imitadores) | Baixo, anual | Registrar `.com.br`, `.org.br` e grafias parecidas |
| Hospedagem estática e CDN | Zero a baixo | Planos gratuitos costumam bastar para site estático |
| CI | Zero a baixo | Muitos serviços dão minutos gratuitos para projetos abertos |
| Máquina para o emulador Android do robô | Médio, mensal | Precisa de virtualização; pode começar só com o robô web |
| Guardiões de serviço | O maior custo | Horas humanas todo mês; voluntariado funciona no começo, mas some com o tempo |
| Testes com usuários | Baixo por rodada | Agradecimento, transporte, celular de reserva |
| Revisão jurídica | Pontual | Uso de capturas, marca, termos e LGPD antes do lançamento |
| WhatsApp que conversa (Fase 3) | Variável, por conversa | Conferir a tabela de preços vigente da plataforma |

### De onde pode vir o dinheiro

- **Doações recorrentes** de pessoas, sem nenhum benefício em troca.
- **Editais e fundações** de tecnologia cívica, inclusão digital e direitos do consumidor.
- **Apoio institucional** de universidades ou organizações que queiram usar os guias (a licença aberta permite).
- **Voluntariado organizado** pela HarpiaFOSS, com papéis claros e reconhecimento público.

### O que o projeto nunca aceita

Anúncios, venda ou compartilhamento de dados, links de afiliado e qualquer parceria com despachantes ou serviços pagos que façam pelo cidadão o que é gratuito no canal oficial. Qualquer um desses mina a confiança, que é o único ativo do projeto.

## Riscos e decisões em aberto

### Principais riscos de produto

| Risco | Chance | Impacto | Como reduzir |
|:--|:--|:--|:--|
| Capturas envelhecem mais rápido do que o time consegue atualizar | Alta | Alto | Onda 1 com só dois serviços, robô semanal, aviso honesto quando a imagem está velha |
| Voluntários se cansam e serviços ficam sem guardião | Alta | Alto | Um guardião reserva por serviço; tirar do ar o Faça Conosco de um serviço sem guardião, mantendo só o texto |
| Pessoas acham que o Simplifica é do governo | Média | Alto | Aviso junto ao logo, pergunta fixa nos testes, nenhum elemento visual do governo |
| Golpistas criam sites imitando o Simplifica | Média | Alto | Domínios registrados, página "Como saber que somos nós", canal de denúncia |
| Questionamento jurídico sobre uso de capturas oficiais | Baixa a média | Médio | Parecer antes do lançamento, dados fictícios, indicação de fonte e versão |
| Pouca gente encontra o site | Alta | Médio | Distribuição pela persona Cláudia, cartões para WhatsApp, parcerias locais |

### Decisões já tomadas

| Decisão | Escolha | Motivo |
|:--|:--|:--|
| Licença do código | AGPL-3.0 | Garante que melhorias feitas sobre o projeto continuem abertas, inclusive quando alguém o oferece como serviço; a MIT foi considerada e descartada porque permitiria versões fechadas. O conteúdo fica sob CC BY-SA e a marca, no `TRADEMARKS.md` |
| Variante da AGPL | `AGPL-3.0-or-later` | Aceita versões futuras da licença publicadas pela Free Software Foundation, evitando ter de pedir autorização a todos os contribuidores para atualizar |
| Validar antes de programar | Etapa 0 com protótipo em slides | Descobrir em dias, e quase sem custo, se o "fazer junto" faz diferença |
| Primeira contribuição à HarpiaFOSS | PR na CLI do `ssrfcheck` | Pequena, útil para o próprio Simplifica e cria relação com a comunidade antes da proposta |

### Decisões em aberto

Cada decisão importante vira um registro curto (ADR: contexto, opções, decisão, consequências) numa pasta `docs/decisoes/` do repositório, para quem chegar depois entender o porquê.

| Decisão | Opções | Quando decidir |
|:--|:--|:--|
| Hospedagem | Cloudflare Pages, Netlify ou servidor próprio com Caddy | Etapa 6 do guia de implementação |
| Nome definitivo | Simplifica Brasil ou alternativa, após busca no INPI | Antes de registrar domínios |
| Guardiões | Só voluntários ou núcleo pago com voluntários de apoio | Antes da Onda 2 |
| Conversa com o Me Explica | Depois do lançamento ou nunca | Após a primeira rodada de testes |

## Roadmap e próximos serviços

Antes da Fase 1 vem a Etapa 0: o teste da hipótese com protótipo em slides e a contribuição ao ssrfcheck. O projeto cresce em quatro fases, e cada uma só começa quando a anterior passa no seu portão de qualidade. As datas ficam em aberto até o tamanho da equipe estar definido.

\[embedded content: roadmap · 4 fases, 3 portões\]

No WhatsApp, o canal de avisos entra já no MVP. O número que conversa, os lembretes e o Faça Conosco pelo WhatsApp entram na Fase 3, nunca para pedir dados. Busca com inteligência artificial fica fora até haver uma forma de garantir que ela só responda com o conteúdo revisado.

### Serviços da Fase 2

| Serviço                            | Situação de vida        | Por que entra                                                                                    |
|:-----------------------------------|:------------------------|:-------------------------------------------------------------------------------------------------|
| BPC/LOAS                           | Aposentadoria e INSS    | Benefício para idosos e pessoas com deficiência de baixa renda; muita gente com direito não pede |
| Farmácia Popular                   | Saúde                   | Remédios gratuitos ou baratos para pressão e diabetes, muito usados por idosos                   |
| Tarifa Social de Energia           | Benefícios da família   | Desconto na conta de luz para famílias do CadÚnico                                               |
| Meu SUS Digital                    | Saúde                   | Cartão do SUS, vacinas e exames                                                                  |
| Seguro-desemprego                  | Perdi o emprego         | Completa a jornada de quem foi demitido                                                          |
| Carteira de Trabalho Digital       | Perdi o emprego         | Conferência de vínculos e salários                                                               |
| CNH digital, multas e renovação    | Meus documentos         | Muito buscado, com app confuso                                                                   |
| Título de eleitor (e-Título)       | Meus documentos         | Pico de busca em ano de eleição                                                                  |
| Assinatura digital gov.br          | Meus documentos         | Gratuita, pouca gente conhece e evita ida ao cartório                                            |
| Registrato (Banco Central)         | Isso é golpe?           | Mostra empréstimos e contas abertos no nome da pessoa                                            |
| Antecedentes criminais             | Meus documentos         | Pedido comum em contratação                                                                      |
| MEI: abrir, DAS e declaração anual | Tenho um negócio (nova) | Atende o Rafael e milhões de microempreendedores                                                 |
| Declarar o Imposto de Renda        | Dinheiro que posso ter  | Hoje o MVP cobre só a restituição                                                                |
| Pé-de-Meia                         | Benefícios da família   | Incentivo para estudantes do ensino médio                                                        |
| Pensão por morte                   | Aposentadoria e INSS    | Momento difícil em que a família precisa de um caminho claro                                     |
| Passaporte                         | Meus documentos         | Menos urgente para o público central, mas muito procurado                                        |
| “É você ou é o site?”              | Página fixa             | Mostra se os sites oficiais estão fora do ar, reaproveitando o monitor de disponibilidade        |
| Ficha do serviço para imprimir     | Todas                   | Resumo de uma folha para levar ao posto ou guardar em casa                                       |
| Glossário tocável                  | Todas                   | Tocar em “exigência” ou “protocolo” mostra o significado sem sair da página                      |

### Serviços da Fase 4

Carteira de Identidade Nacional (RG), IPVA e licenciamento, IPTU, agendamento em centrais de atendimento estaduais e matrícula escolar. Cada estado tem regras próprias, por isso a fase começa por três estados e depende de revisores voluntários locais.

## Prompt SDD para desenvolvimento

O prompt abaixo segue o desenvolvimento orientado a especificação (SDD): primeiro a constituição e a especificação, depois o plano, as tarefas e só então o código. Cole-o no início da sessão com um agente de código (Claude Code ou similar) e peça uma fase por vez, revisando cada saída antes de seguir.

``` text
Você vai construir o Simplifica Brasil, um site estático que explica serviços do governo brasileiro em linguagem simples e encaminha para o site oficial. Trabalhe em SDD: gere e me mostre cada artefato abaixo, um por vez, e espere minha aprovação antes do próximo.

# 1. CONSTITUIÇÃO (regras inegociáveis; recuse qualquer tarefa que as viole)
- Nunca coletar, pedir ou armazenar dado pessoal: sem login, sem formulário de dados, sem cookies.
- Nunca agir em nome do usuário nem integrar login gov.br.
- Todo link externo passa pelo componente <LinkOficial>, que só aceita domínios da allowlist em src/config/dominios-oficiais.ts; qualquer outro domínio quebra o build.
- Acessibilidade WCAG 2.2 AAA no texto (contraste 7:1), AA no restante; alvos de toque de 56 px; fonte base de 18 px; nada de carrossel, pop-up, tempo limite ou gesto obrigatório.
- Português do Brasil em linguagem simples: frases de até 20 palavras, voz ativa, "você".
- Sem scripts de terceiros; CSP rígida sem 'unsafe-inline'; JavaScript só onde for indispensável (ilhas).
- Orçamento por página: até 150 KB transferidos e LCP abaixo de 2,5 s em 4G lento.
- Nenhuma identidade visual do governo.

# 2. ESPECIFICAÇÃO
Telas: Início, Situação, Serviço, Faça Conosco, Treinar antes, Saída para o site oficial, Deu errado?, Esqueci minha senha, Isso é golpe?, Simplifica no WhatsApp, Ajustes de leitura. Todas em tema claro e escuro.
Conteúdo da Onda 1: dois serviços completos com Faça Conosco e Treinar antes (entrar e recuperar a conta gov.br; extrato de contribuições do INSS), mais Isso é golpe?, Esqueci minha senha, Kit de acesso, Ajustes de leitura e a página do canal de avisos. A tela inicial mostra só as situações com conteúdo pronto. A Onda 2 acrescenta FGTS, Valores a Receber, restituição do IR, Bolsa Família e CadÚnico, e CPF, um por vez, cada um após teste com usuários.
Cada serviço é um arquivo Markdown com frontmatter validado por schema: titulo, situacao, resumo, quemTemDireito, oQueSeparar[], nivelContaGovbr, passos[{texto, imagem, alt, alvo{rotulo, caixa{x,y,w,h}}, pareEConfira, origem{app, versao, tipo}, conferidoEm}], treino (opcional), linkOficial, errosComuns[{problema, solucao}], telefoneOficial, revisadoEm.
Recursos: Faça Conosco com destaque desenhado em SVG sobre a captura, aviso automático quando a captura tiver mais de 90 dias, botão "A tela está diferente?", ajustes de leitura (letra, tema claro/escuro/sistema, menos movimento) salvos só no aparelho, busca local offline, botão "Ouvir esta página" (Web Speech API, pt-BR), botão "Mandar para alguém" (Web Share API com fallback de link wa.me), "Isso ajudou?" sim/não, funcionamento offline após a primeira visita.
Critérios de aceite: escreva-os em Given/When/Then para cada tela, incluindo navegação só por teclado e leitura com TalkBack.

# 3. PLANO TÉCNICO
- Astro em modo estático, TypeScript estrito, content collections com Zod.
- CSS próprio com design tokens (cores, tipografia Atkinson Hyperlegible, espaçamentos) em variáveis.
- Pagefind para a busca; service worker para o modo offline.
- Robô de capturas: Playwright e Maestro em job semanal, comparação com pixelmatch, issue automática com antes e depois.
- Testes: Vitest (unidade), Playwright + axe-core (acessibilidade e fluxos), Lighthouse CI (desempenho e acessibilidade 100), lychee (links).
- Segurança no CI: Gitleaks, OSV-Scanner, Semgrep, validação da allowlist, SBOM CycloneDX; Actions fixadas por hash.
- Observabilidade: Umami sem cookies, GlitchTip, coleta de Web Vitals e endpoint de relatórios CSP.
- Hospedagem estática com cabeçalhos HTTP próprios (HSTS, CSP, Referrer-Policy, Permissions-Policy).

# 4. TAREFAS
Quebre o plano em tarefas pequenas, cada uma com: objetivo, arquivos tocados, testes que provam que está pronta. Ordem sugerida: tokens e layout base -> schema de conteúdo -> componentes (Cartão de situação, LinkOficial, Passo, BotãoOuvir) -> telas -> busca e offline -> CI de segurança e acessibilidade -> observabilidade.

# 5. DEFINIÇÃO DE PRONTO
Testes verdes, Lighthouse 100 em acessibilidade nos dois temas, zero violações do axe, nenhum domínio fora da allowlist, nenhum segredo detectado, página dentro do orçamento e revisão humana aprovada.
```

## Referências

Este documento foi escrito sem acesso a busca na web; as referências abaixo vêm de conhecimento até meados de 2026 e não foram abertas agora. Confira cada uma antes de citar.

**Design e acessibilidade**

- [Nielsen Norman Group](https://www.nngroup.com/): pesquisas de usabilidade com usuários acima de 65 anos.
- [GOV.UK Design System](https://design-system.service.gov.uk/): referência mundial em serviço público simples.
- [WCAG 2.2](https://www.w3.org/TR/WCAG22/): diretrizes de acessibilidade do W3C.
- [eMAG](https://emag.governoeletronico.gov.br/): modelo brasileiro de acessibilidade em governo eletrônico.
- [Atkinson Hyperlegible](https://www.brailleinstitute.org/freefont/): fonte do Braille Institute.

**Segurança**

- [OWASP ASVS](https://owasp.org/www-project-application-security-verification-standard/): checklist de verificação de segurança.
- [OWASP Top 10](https://owasp.org/www-project-top-ten/): principais riscos de aplicações web.
- [SLSA](https://slsa.dev/): integridade da cadeia de build.

**Ferramentas citadas nesta versão**

- [Playwright](https://playwright.dev/): automação de navegador para as capturas.
- [Maestro](https://maestro.mobile.dev/): automação de apps Android.
- [pixelmatch](https://github.com/mapbox/pixelmatch): comparação de imagens.
- [WhatsApp Business Platform](https://business.whatsapp.com/): conferir preços e políticas atuais.

**Desenvolvimento**

- [Spec Kit](https://github.com/github/spec-kit): ferramenta aberta de desenvolvimento orientado a especificação.

## Glossário

| Termo | O que significa aqui |
|:--|:--|
| Copyleft | Tipo de licença que exige que versões modificadas continuem livres, sob a mesma licença. AGPL e CC BY-SA são copyleft; MIT não é. |
| Código de saída | Número que um programa devolve ao terminar: 0 significa sucesso; qualquer outro valor, falha. Scripts e CI decidem o que fazer com base nele. |
| AGPL-3.0 | Licença de software livre copyleft: quem distribuir ou oferecer pela rede uma versão modificada precisa abrir o código sob a mesma licença. |
| ADR | Registro de decisão de arquitetura: um texto curto explicando uma escolha técnica e o porquê. |
| Allowlist | Lista fechada do que é permitido. Aqui, os endereços oficiais para onde o site pode mandar a pessoa. |
| Astro | Framework que gera sites a partir de componentes e Markdown, enviando pouquíssimo JavaScript ao navegador. |
| Build | O processo que transforma o código-fonte nas páginas finais em `dist/`. |
| CDN | Rede de servidores espalhados que entrega arquivos prontos perto de quem acessa. Rápida e barata para site estático. |
| CI | Integração contínua: testes e verificações que rodam sozinhos a cada pull request. |
| CNIS | Cadastro Nacional de Informações Sociais, a base do INSS com os vínculos e contribuições de cada pessoa. |
| DCO | Developer Certificate of Origin: declaração, feita com `git commit -s`, de que quem contribui tem direito de enviar aquele código. |
| CSP | Content Security Policy, um cabeçalho HTTP que diz ao navegador de onde a página pode carregar scripts e outros recursos. Bloqueia código injetado. |
| HSTS | Cabeçalho que obriga o navegador a usar sempre HTTPS no site. |
| Hipótese | Uma aposta escrita de forma que um teste possa confirmar ou derrubar. |
| LGPD | Lei Geral de Proteção de Dados, que regula o uso de dados pessoais no Brasil. |
| MVP | Produto mínimo viável: a menor versão que já entrega valor e permite aprender com usuários reais. |
| pixelmatch | Biblioteca que compara duas imagens e conta quantos pixels mudaram. |
| Playwright | Ferramenta que controla um navegador por código, usada nos testes e no robô de capturas. |
| PRD | Documento de requisitos de produto, como este. |
| SBOM | Lista de todos os componentes e dependências de uma versão do software, para auditoria. |
| SPDX | Padrão de identificadores curtos de licença, como `AGPL-3.0-or-later`, usados em cabeçalhos de arquivo e no `package.json`. |
| Schema | Descrição do formato que um dado deve ter. Aqui, feito com a biblioteca Zod. |
| SDD | Desenvolvimento orientado a especificação: escrever regras e especificação antes do código. |
| Service worker | Script do navegador que guarda páginas para funcionarem offline. |
| SSRF | Server-Side Request Forgery: quando um atacante faz um servidor (ou o CI) acessar um endereço interno que ele não alcançaria de fora. |
| Site estático | Site cujas páginas já estão prontas antes do acesso; não há servidor gerando nada na hora. |
| TalkBack | Leitor de tela do Android, que lê em voz alta o que aparece na tela. |
| WCAG / AAA | Diretrizes internacionais de acessibilidade; AAA é o nível mais exigente, usado aqui para contraste de texto. |
