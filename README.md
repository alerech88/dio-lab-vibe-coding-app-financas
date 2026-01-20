# 💸 App Seu Bolso Amigo de Alexandre com Vibe Coding


**Prompt Final (PRD) Utilizado no Lovable, refinado com uso do Copilot:**

Você é um parceiro de desenvolvimento no estilo vibe coding. Crie um MVP de um aplicativo de organização de finanças pessoais baseado em conversa (assistente financeiro conversacional), seguindo rigorosamente as instruções abaixo. Use tom educativo, acessível e acolhedor, em português do Brasil. Estruture o projeto, as rotas, a persistência local e a interface conforme este documento.

========================
1) CONTEXTO
========================
As pessoas têm dificuldade em manter o controle financeiro porque a maioria dos aplicativos exige inserção manual constante ou interfaces complexas. Este projeto propõe um app que funcione como um assistente financeiro conversacional, permitindo ao usuário registrar e acompanhar suas finanças em linguagem natural.

========================
2) PROBLEMA
========================
- Usuários desistem por acharem os apps complicados.
- Categorização de gastos é cansativa e pouco automática.
- Falta personalização e comunicação mais humana.

Objetivo: oferecer uma experiência intuitiva, conversacional e personalizada, com insights automáticos e sem fricção.

========================
3) PÚBLICO-ALVO
========================
- Iniciantes na organização financeira.
- Pessoas que querem praticidade sem planilhas ou formulários complexos.
- Usuários que já tentaram apps/planilhas e desistiram.

========================
4) PROPOSTA DE VALOR
========================
Um assistente financeiro por conversa que simplifica o registro de gastos, classifica automaticamente, acompanha metas e entrega insights claros – tudo em linguagem natural, de forma humana e direta.

========================
5) ESCOPo DO MVP (Funcionalidades-Chave)
========================
A. Registro de gastos por chat:
   - Ex.: “Gastei 32 reais no mercado hoje”.
   - Extrair automaticamente: valor, categoria, data e descrição.
   - Exibir um “card de confirmação” antes de salvar.

B. Classificação automática:
   - Regras/NLP simples para classificar (ex.: Alimentação, Transporte, Lazer).
   - Permitir correção pelo usuário e reclassificação fácil.

C. Metas financeiras simples:
   - Ex.: “Quero economizar 200 reais este mês”.
   - Salvar meta, calcular progresso e exibir barra visual.

D. Insights/Dicas do Agente Financeiro:
   - Detectar aumento de gastos, categorias dominantes e risco de estourar meta.
   - Gerar mensagens curtas, acionáveis e educativas.

E. Relatórios personalizados:
   - Visão do mês atual com total, por categoria e comparação com mês anterior.
   - Texto em linguagem natural (ex.: “Você gastou 18% a mais em alimentação.”)
   - Gráfico simples (pizza/barras).

========================
6) REQUISITOS TÉCNICOS
========================
- Linguagem/stack: padrão do Lovable para apps web com chat + rotas de telas.
- Persistência local (ex.: IndexedDB/LocalStorage ou camada local equivalente no Lovable).
- Módulos:
  1) NLP leve (regras + regex + parsing numérico e datas em pt-BR).
  2) Motor de categorização com mapa de palavras-chave por categoria e fallback “Outros”.
  3) Módulo de metas (CRUD, cálculo de progresso por período).
  4) Módulo de relatórios (agregação mês atual e mês anterior).
  5) Módulo de insights (regras if/then sobre variações e limites).
- Internacionalização mínima: pt-BR como padrão.
- Acessibilidade: contrastes adequados, labels claros e navegação por teclado.

========================
7) MODELO DE DADOS (sugestão mínima)
========================
Tabelas/coleções:
- transacoes: { id, dataISO, valor, categoria, descricao, origem="manual|chat", criadoEm, atualizadoEm }
- metas: { id, periodo="YYYY-MM", tipo="economia|limiteCategoria", alvoValor, categoriaOpcional, criadoEm }
- configuracoes: { id="default", moeda="BRL", categoriasPadrao: [ ... ] }

Categorias padrão (sugestão): Alimentação, Transporte, Moradia, Lazer, Saúde, Educação, Contas, Compras, Outros.

========================
8) NLP E REGRAS (MVP)
========================
Parsing de mensagens (exemplos):
- “Gastei 32 reais no mercado hoje” → valor=32, categoria=Alimentação, data=hoje, descricao="mercado".
- “Paguei 120 de Uber ontem” → valor=120, categoria=Transporte, data=ontem, descricao="Uber".
- “Quero economizar 200 esse mês” → criar meta: tipo=economia, alvoValor=200, periodo=mês atual.

Heurísticas:
- Valor: procurar números + “reais”, “R$” ou número isolado contextualizado por verbo gastar/pagar.
- Data: “hoje”, “ontem”, “anteontem”, “semana passada”, ou dd/mm/aaaa.
- Categoria por palavras-chave (ex.: “mercado/supermercado/comida” → Alimentação; “Uber/ônibus/táxi/combustível” → Transporte).
- Se ambíguo: perguntar confirmação (“Foi alimentação?”) com opções de botões.

========================
9) INTERAÇÕES DO CHAT
========================
- Tom: acolhedor, educativo e objetivo.
- Após interpretar uma mensagem de gasto:
  - Mostrar card: { ícone categoria, valor, data, descrição, botão [Confirmar] [Editar] }.
- Confirmado → salvar, atualizar resumo/insights.
- Mensagens úteis prontas:
  - “Como estão meus gastos este mês?”
  - “Crie uma meta de economizar 150”
  - “Mostre meus gastos por categoria”
  - “Quais dicas para eu economizar?”

========================
10) TELAS/ROTAS A IMPLEMENTAR
========================
- / (Splash/Boas-vindas): logo + botão [Começar].
- /chat (Principal): histórico, input, respostas do agente, cards de confirmação.
- /resumo: total do mês, por categoria, gráfico simples e comparação com mês anterior.
- /metas: lista de metas, criar/editar, barra de progresso.
- /insights: lista de dicas automáticas e alertas (ex.: “+18% em Alimentação”).

========================
11) CRITÉRIOS DE ACEITAÇÃO (MVP)
========================
- [Chat] Reconhece ao menos 10 variações comuns de mensagens de gasto (com e sem “R$”).
- [Confirmação] Antes de salvar, sempre exibe card com edição possível.
- [Persistência] Transações e metas persistem localmente e reaparecem após recarregar.
- [Resumo] Mostra total, por categoria e variação vs. mês anterior.
- [Metas] Cria meta por linguagem natural, calcula progresso e exibe barra.
- [Insights] Exibe ao menos 3 regras ativas (aumento de categoria, categoria dominante, aproximação de meta).
- [Acessibilidade] Labels nos inputs, foco visível, textos legíveis.

========================
12) TAREFAS DE IMPLEMENTAÇÃO (Passo a passo)
========================
1) Estruturar projeto, rotas e layout base.
2) Implementar /chat com pipeline NLP simples + card de confirmação.
3) Persistir transações e metas localmente.
4) Implementar /resumo com agregações e gráfico.
5) Implementar /metas com criação por texto e cálculo de progresso.
6) Implementar /insights com regras de dicas.
7) Refinar UX do chat (mensagens amigáveis, confirmações claras, estados de erro).
8) Testes manuais com dados simulados e exemplos de frases.

========================
13) MENSAGENS DE SISTEMA (TOM E ESTILO)
========================
- “Oi! Como posso te ajudar a organizar suas finanças hoje?”
- “Entendi: R$ {valor} em {categoria} — {data}. Confirmar?”
- “Legal! Registro salvo. Quer ver um resumo do seu mês?”
- “Percebi um aumento em {categoria}. Posso sugerir algumas ideias de economia?”
- “Sua meta de R$ {alvo} está {progresso}% completa. Ótimo trabalho!”

========================
14) DADOS INICIAIS (MOCK) – OPCIONAL
========================
Se necessário, pré-carregar alguns exemplos:
- transacoes: [ {dataISO: hoje-3, valor: 45.90, categoria:"Alimentação", descricao:"supermercado"},
                {dataISO: hoje-2, valor: 18.00, categoria:"Transporte", descricao:"Uber"},
                {dataISO: hoje-1, valor: 120.00, categoria:"Contas", descricao:"luz"} ]
- metas: [ {periodo: mês atual, tipo:"economia", alvoValor: 200} ]

========================
15) ENTREGÁVEL
========================
- App funcional com as rotas acima, layout simples e responsivo.
- Código organizado, componentes reutilizáveis, funções bem nomeadas.
- Sem dependências externas que exijam backend; tudo local no MVP.
- Textos em pt-BR e mensagens educativas.

========================
16) WORKFLOW DE TELAS (Wireframe Textual)
========================
A) Splash (/):
-----------------------------------------
| [LOGO]                                 |
| Olá! Vamos organizar suas finanças?    |
| [Começar] → navega para /chat          |
-----------------------------------------

B) Chat (/chat):
-------------------------------------------------
| Agente Financeiro (mensagens)                 |
| Histórico de conversas                        |
| Card de confirmação (quando houver)           |
| > Campo de texto [Enviar]                     |
-------------------------------------------------
Ações: registrar gasto, criar meta, pedir resumo e dicas.

C) Resumo (/resumo):
-------------------------------------------------
| Resumo do Mês                                 |
| Total gasto: R$ XXX                           |
| Top categorias e gráfico simples              |
| Variação vs mês anterior                      |
| [Ver detalhes por categoria]                  |
-------------------------------------------------

D) Metas (/metas):
-------------------------------------------------
| Minhas metas                                  |
| Meta: Economizar R$ 200 (exemplo)             |
| Progresso: ███████░░░░ 60%                    |
| [Criar nova meta]                             |
-------------------------------------------------

E) Insights (/insights):
-------------------------------------------------
| Insights do Agente Financeiro                 |
| - Você gastou 18% a mais em Alimentação.     |
| - Você atingiu 60% da meta mensal.           |
-------------------------------------------------

Fluxo geral:
Splash → Chat → (Resumo / Metas / Insights) → retorna ao Chat como hub

========================
17) INSTRUÇÕES FINAIS
========================
- Priorize simplicidade, clareza e feedbacks curtos no chat.
- Sempre que houver ambiguidade, pergunte em 1 mensagem de confirmação.
- Otimize para o fluxo conversacional: registrar → confirmar → refletir no resumo/insights.
- Entregue o projeto pronto para navegação entre as rotas e com persistência local.

**Interações com o Lovable**

Além do PRD enviado, foi feita uma nova interação abaixo:
"Adicione a opção de também acrescentar receitas, por exemplo salário e outras entradas, e não somente despesas, e acrescente uma tela de extrato com receitas e despesas e saldo do mês."

**Resultado Final no Lovable: https://conversa-financeira-legal.lovable.app**

<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/72b0c379-3ad4-4485-ade0-e5d0f34e5c91" />

**Resumo das funcionalidades do aplicativo:**

O aplicativo é um assistente financeiro conversacional que permite registrar despesas e receitas em linguagem natural, acompanhar metas de economia e visualizar um extrato mensal com saldo. Ele analisa automaticamente os gastos, classifica as transações, oferece insights personalizados e apresenta relatórios simples para ajudar o usuário a entender e organizar melhor suas finanças.


Uma breve **reflexão sobre o processo**:
  - O que funcionou bem?
    Todo o fluxo de refinamento com o Copilot e também a criação e melhoria da aplicação com o Lovable funcionaram muito bem.
    
  - O que não funcionou como o esperado?
    Algumas features do app não saíram exatamente como esperado, alguns tipos de despesas lançadas não estavam classificando corretamente, porém já não tinha mais créditos no Lovable.
    
  - O que aprendeu sobre conversar com IAs?
    Aprendi que o segredo de tudo está nos prompts, a IA funciona muito bem quando bem direcionada e guiada através de prompts corretos, contextualizados, com entregáveis claros.


