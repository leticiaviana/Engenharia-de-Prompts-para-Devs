# 🤖 Engenharia de Prompts para Devs

> Caderno Temático criado no NotebookLM como parte do desafio de projeto da [DIO](https://www.dio.me/).  
> **Tema:** Como usar LLMs no dia a dia de um desenvolvedor front-end/full stack para produzir mais e com mais qualidade.  
> 📓 **[Acesse o Caderno no NotebookLM](https://notebooklm.google.com/notebook/0a10f3bd-9ba4-4ced-b279-1012731e1214)**

---

## 📌 1. Contexto e Objetivos

### Contexto

A Inteligência Artificial Generativa deixou de ser novidade e tornou-se parte do fluxo de trabalho de desenvolvedores ao redor do mundo. Ferramentas como GitHub Copilot, ChatGPT, Claude e Google Gemini já estão integradas a editores de código, pipelines de CI/CD e até fluxos de revisão de código. Saber **como formular boas perguntas (prompts)** para essas ferramentas é, hoje, uma habilidade tão importante quanto conhecer a sintaxe de uma linguagem de programação.

Este caderno temático explora, de forma prática e aplicada, a **Engenharia de Prompts voltada para desenvolvedores**, com foco em cenários reais do cotidiano de quem trabalha com Angular, React, TypeScript, Cypress e similares.

### Objetivos de Estudo

- Compreender os fundamentos da engenharia de prompts aplicada ao desenvolvimento de software.
- Aprender a usar LLMs para **gerar código**, **depurar erros**, **refatorar** funções e **criar testes automatizados**.
- Explorar técnicas de prompting (zero-shot, few-shot, chain-of-thought) em contextos de programação.
- Documentar prompts estratégicos e reutilizáveis para o dia a dia de desenvolvimento.
- Identificar limitações e boas práticas para não depender cegamente das respostas da IA.

---

## 📚 2. Curadoria de Fontes

As fontes abaixo foram selecionadas por serem abertas, confiáveis e diretamente relevantes para o tema. Todas foram inseridas no NotebookLM para análise e síntese.

| # | Fonte | Tipo | Link |
|---|-------|------|------|
| 1 | **Prompt Engineering Guide** — DAIR.AI | Documentação / Guia | [promptingguide.ai](https://www.promptingguide.ai/) |
| 2 | **OpenAI Prompt Engineering Best Practices** | Artigo Oficial | [platform.openai.com/docs/guides/prompt-engineering](https://platform.openai.com/docs/guides/prompt-engineering) |
| 3 | **A Practical Guide to LLM Prompt Engineering** — LearnPrompting | Guia Open Source | [learnprompting.org](https://learnprompting.org/docs/intro) |
| 4 | **GitHub Copilot Documentation** — Using Copilot in your IDE | Documentação Oficial | [docs.github.com/en/copilot](https://docs.github.com/en/copilot/using-github-copilot/getting-started-with-github-copilot) |
| 5 | **ChatGPT Prompt Engineering for Developers** — DeepLearning.AI (Andrew Ng) | Curso Gratuito | [learn.deeplearning.ai](https://learn.deeplearning.ai/courses/chatgpt-prompt-eng-for-developers) |

---

## 🧪 3. Engenharia de Prompts e "Cicatrizes"

Esta seção documenta os prompts testados, as variações realizadas, as respostas obtidas e as dificuldades encontradas (troubleshooting). Registrar o raciocínio por trás dos resultados é o diferencial de um profissional maduro.

---

### 🔹 Caso 1 — Geração de Código (Componente Angular)

**Objetivo:** Gerar um componente Angular com formulário reativo de login com validações.

#### Prompt v1 (zero-shot):
```
Crie um componente Angular com um formulário de login.
```

**Resultado:** A IA gerou um componente funcional, mas sem tipagem forte, sem `Validators` do Angular e com HTML não acessível. Resposta genérica e superficial.

#### Prompt v2 (few-shot + contexto):
```
Você é um desenvolvedor Angular sênior. Crie um componente standalone em Angular 17
com um formulário reativo de login contendo os campos "email" e "senha".
Use Validators.required e Validators.email para o campo de e-mail.
Exiba mensagens de erro inline abaixo de cada campo.
Use TypeScript com tipagem estrita. Não use módulos, use standalone components.
```

**Resultado:** A IA gerou um componente correto, com tipagem, `FormGroup`, `FormControl`, `Validators`, template com `@if` para erros e `standalone: true`. Muito mais aproveitável.

**🩹 Cicatriz:** A primeira versão sem contexto gerou código desatualizado (Angular 14). Ao especificar a versão e as práticas modernas (standalone), o resultado melhorou drasticamente. **Lição:** sempre informe a versão do framework e o estilo arquitetural desejado.

---

### 🔹 Caso 2 — Debug de Erro (TypeScript)

**Objetivo:** Encontrar a causa de um erro de tipo em TypeScript.

**Erro apresentado:**
```
Type 'string | undefined' is not assignable to type 'string'.
```

#### Prompt v1:
```
Corrija esse erro TypeScript: Type 'string | undefined' is not assignable to type 'string'.
```

**Resultado:** A IA sugeriu usar `as string` (type assertion), o que silencia o erro sem tratar o problema real.

#### Prompt v2 (chain-of-thought):
```
Analise o seguinte erro TypeScript e explique por que ele ocorre:
"Type 'string | undefined' is not assignable to type 'string'."

Depois, me mostre três formas diferentes de resolvê-lo corretamente,
explicando os trade-offs de cada abordagem. Não use type assertion (as string).
```

**Resultado:** A IA explicou que o erro ocorre quando `strictNullChecks` está ativo e um valor pode ser `undefined`. Apresentou três soluções: optional chaining + valor padrão, non-null assertion com justificativa de quando é seguro, e narrowing com `if`. Muito mais didático e seguro.

**🩹 Cicatriz:** Pedir à IA para "corrigir" sem contexto leva a soluções rápidas e perigosas. Pedir para "analisar e explicar" com restrições explícitas (`não use type assertion`) direciona para respostas mais robustas.

---

### 🔹 Caso 3 — Refatoração de Código (React)

**Objetivo:** Refatorar um componente React de classe para funcional com hooks.

#### Prompt v1:
```
Refatore esse componente React de classe para funcional.
[código colado]
```

**Resultado:** A IA converteu, mas usou `componentDidMount` mapeado para `useEffect` sem array de dependências correto, causando re-renders infinitos em potencial.

#### Prompt v2:
```
Refatore o componente React abaixo de classe para funcional usando hooks modernos.
Garanta que:
1. O useEffect tenha o array de dependências correto.
2. Não haja re-renders desnecessários.
3. A lógica de estado seja separada em um custom hook chamado useUserData.
4. O código final siga as boas práticas do React 18.

[código colado]
```

**Resultado:** A IA gerou o custom hook `useUserData` separado, com `useEffect` correto, sem re-renders desnecessários e seguindo o padrão de composição do React 18.

**🩹 Cicatriz:** Requisitos implícitos (como "sem re-renders") precisam ser tornados explícitos no prompt. A IA otimiza para o que foi pedido literalmente — nada mais, nada menos.

---

### 🔹 Caso 4 — Criação de Testes com Cypress

**Objetivo:** Criar testes E2E para uma tela de login com Cypress.

#### Prompt v1:
```
Crie testes Cypress para a tela de login.
```

**Resultado:** Testes muito básicos, sem Page Object Model, sem seletores robustos e sem cobertura de cenários negativos.

#### Prompt v2:
```
Crie testes E2E com Cypress para uma tela de login que possui:
- Campo de e-mail (data-cy="email-input")
- Campo de senha (data-cy="password-input")
- Botão de submit (data-cy="login-button")
- Mensagem de erro (data-cy="error-message")

Use o padrão Page Object Model. Cubra os seguintes cenários:
1. Login com credenciais válidas → redireciona para /dashboard
2. Login com e-mail inválido → exibe mensagem de erro de validação
3. Login com senha errada → exibe mensagem "Credenciais inválidas"
4. Tentativa de submit com campos vazios → exibe erros de validação em ambos os campos

Use TypeScript e as boas práticas do Cypress 12+.
```

**Resultado:** A IA gerou um Page Object `LoginPage`, os quatro cenários de teste com seletores `data-cy`, asserções corretas e tipagem TypeScript. Pronto para uso.

**🩹 Cicatriz:** Sem os seletores e os cenários explícitos, os testes gerados eram frágeis (usavam seletores CSS genéricos). Especificar os atributos `data-cy` e os fluxos de usuário resultou em testes muito mais robustos e estáveis.

---

### 🔹 Caso 5 — Documentação Automática (JSDoc)

**Objetivo:** Gerar JSDoc para uma função utilitária complexa.

#### Prompt v1:
```
Documente essa função com JSDoc.
[código colado]
```

**Resultado:** JSDoc básico apenas com `@param` e `@returns`, sem exemplos ou explicação do algoritmo.

#### Prompt v2:
```
Gere documentação JSDoc completa para a função abaixo. Inclua:
- Descrição do que a função faz e por que existe
- @param com tipos e descrições detalhadas
- @returns com tipo e descrição do valor retornado
- @throws documentando as exceções que podem ser lançadas
- @example com ao menos dois exemplos de uso (caso feliz e caso de erro)

[código colado]
```

**Resultado:** Documentação rica, com descrição narrativa do propósito da função, todos os parâmetros tipados, exemplos funcionais e documentação das exceções.

**🩹 Cicatriz:** JSDoc gerado sem template fica sempre incompleto. Criar um prompt-padrão com os campos obrigatórios e salvá-lo como snippet no VS Code é uma prática que aumenta muito a consistência da documentação do projeto.

---

## 📖 4. Miniguia de Estudo — Entrega Final

### 4.1 Resumos Estruturados

#### O que é Engenharia de Prompts?

Engenharia de Prompts é a disciplina de **projetar entradas textuais (prompts) para modelos de linguagem** de forma a obter saídas mais precisas, relevantes e úteis. Para desenvolvedores, isso significa aprender a "conversar" com a IA de forma estruturada para extrair código, explicações e análises de alta qualidade.

#### Técnicas Fundamentais

| Técnica | Descrição | Quando usar |
|---------|-----------|-------------|
| **Zero-shot** | Instrução direta sem exemplos | Tarefas simples e bem definidas |
| **Few-shot** | Fornece 2-3 exemplos antes da tarefa | Quando o padrão de saída importa |
| **Chain-of-Thought (CoT)** | Pede raciocínio passo a passo | Debugging, análise de trade-offs |
| **Role prompting** | Atribui um papel à IA ("você é um sênior dev...") | Quando o nível de expertise importa |
| **Constrained output** | Define restrições explícitas ("não use X") | Evitar respostas genéricas ou perigosas |

#### Anatomia de um Bom Prompt para Devs

```
[PAPEL]       Você é um desenvolvedor {tecnologia} sênior.
[CONTEXTO]    Estou trabalhando em um projeto {descrição breve}.
[TAREFA]      {ação clara e específica}
[RESTRIÇÕES]  Não use {X}. Use {padrão Y}. Versão: {Z}.
[FORMATO]     Responda com código + explicação em tópicos.
```

#### Boas Práticas

- **Seja específico sobre versões:** Angular 17 ≠ Angular 14. React 18 ≠ React 16.
- **Especifique o estilo arquitetural:** standalone components, hooks, Page Object Model.
- **Peça trade-offs:** "me mostre 3 formas e explique os prós e contras" produz respostas muito mais ricas.
- **Itere:** O primeiro prompt raramente é o melhor. Refine com follow-ups.
- **Valide sempre:** Nunca commite código gerado por IA sem revisão e teste.
- **Use `data-cy` e seletores semânticos** ao pedir testes — a IA vai seguir o padrão que você indicar.

---

### 4.2 Glossário

| Termo | Definição |
|-------|-----------|
| **LLM** (Large Language Model) | Modelo de linguagem de grande escala treinado em grandes volumes de texto. Ex: GPT-4, Claude, Gemini. |
| **Prompt** | A entrada de texto fornecida ao modelo para orientar sua resposta. |
| **Engenharia de Prompts** | A arte e ciência de criar prompts eficazes para obter resultados de qualidade de um LLM. |
| **Zero-shot** | Solicitação ao modelo sem fornecer exemplos prévios. |
| **Few-shot** | Solicitação ao modelo fornecendo alguns exemplos de entrada-saída antes da tarefa real. |
| **Chain-of-Thought (CoT)** | Técnica que instrui o modelo a raciocinar passo a passo antes de dar a resposta final. |
| **Hallucination** | Fenômeno em que o LLM gera informações falsas apresentadas com confiança. Exige validação humana obrigatória. |
| **Temperature** | Parâmetro que controla a criatividade das respostas do LLM. Valor 0 = determinístico; valor 1 = mais criativo. |
| **Token** | Unidade básica de texto processada por um LLM (aproximadamente 4 caracteres ou 0,75 palavras). |
| **Context window** | Quantidade máxima de tokens que o modelo pode processar em uma única interação. |
| **System prompt** | Instrução de configuração fornecida antes da conversa para definir o comportamento do modelo. |
| **RAG** (Retrieval-Augmented Generation) | Técnica que combina busca em base de conhecimento com geração de texto para respostas mais precisas. |
| **NotebookLM** | Ferramenta do Google que usa IA para analisar, resumir e interagir com documentos carregados pelo usuário. |
| **Standalone Component** | Componente Angular (v14+) que não precisa ser declarado em um NgModule. |
| **Page Object Model (POM)** | Padrão de design para testes E2E que encapsula seletores e ações de uma página em uma classe. |
| **data-cy** | Atributo HTML customizado usado pelo Cypress como seletor estável para testes, resistente a mudanças de CSS/classes. |
| **JSDoc** | Padrão de documentação em linha para JavaScript/TypeScript usando comentários com anotações `@param`, `@returns`, etc. |

---

### 4.3 Prompts Reutilizáveis

Salve estes prompts como snippets no VS Code ou no seu gerenciador de notas favorito para reutilizar no dia a dia.

---

#### 🔧 Geração de Componente Angular

```
Você é um desenvolvedor Angular sênior. Crie um componente standalone em Angular 17
para [DESCREVER FUNCIONALIDADE].
Requisitos:
- Use TypeScript com tipagem estrita
- Use Reactive Forms se houver formulário (sem FormsModule)
- Inclua validações: [LISTAR VALIDAÇÕES]
- Exiba mensagens de erro inline
- Siga as convenções do Angular Style Guide
Formato de resposta: código TypeScript + template HTML + explicação em tópicos.
```

---

#### ⚛️ Geração de Componente React

```
Você é um desenvolvedor React sênior. Crie um componente funcional em React 18
para [DESCREVER FUNCIONALIDADE].
Requisitos:
- Use TypeScript com tipagem estrita
- Use hooks modernos (useState, useEffect, useCallback conforme necessário)
- Extraia a lógica de negócio em um custom hook chamado use[Nome]
- Garanta que não haja re-renders desnecessários
- Siga as boas práticas do React 18
Formato de resposta: código do hook + código do componente + explicação.
```

---

#### 🐛 Debug e Análise de Erro

```
Analise o seguinte erro [LINGUAGEM/FRAMEWORK] e explique:
1. Por que ele ocorre
2. Em quais condições ele se manifesta
3. Três formas de resolvê-lo com os trade-offs de cada uma
4. Qual você recomenda para produção e por quê

Erro: [COLAR MENSAGEM DE ERRO]
Contexto: [COLAR TRECHO DE CÓDIGO RELEVANTE]

Restrições: Não sugira soluções que silenciem o erro sem tratá-lo (ex: type assertion, any, eslint-disable).
```

---

#### ♻️ Refatoração de Código

```
Refatore o código abaixo seguindo as boas práticas de [TECNOLOGIA versão X].
Objetivos da refatoração:
- [LISTAR OBJETIVOS: ex: extrair custom hook, eliminar duplicação, melhorar legibilidade]
Restrições:
- Mantenha o comportamento externo idêntico
- Não quebre a interface pública (props/outputs)
- [OUTRAS RESTRIÇÕES]
Formato: código refatorado + lista de mudanças realizadas e justificativas.

[COLAR CÓDIGO]
```

---

#### 🧪 Geração de Testes Cypress

```
Crie testes E2E com Cypress para [DESCREVER TELA/FUNCIONALIDADE].
Seletores disponíveis:
- [data-cy="nome-do-elemento"]: [descrição]
(liste todos os seletores relevantes)

Use o padrão Page Object Model. Cubra os cenários:
1. [CENÁRIO FELIZ]
2. [CENÁRIOS DE ERRO/VALIDAÇÃO]
3. [EDGE CASES]

Use TypeScript e as boas práticas do Cypress 12+.
```

---

#### 📝 Geração de JSDoc

```
Gere documentação JSDoc completa para a função abaixo. Inclua:
- Descrição do propósito da função (o quê e por quê)
- @param com tipos TypeScript e descrições detalhadas
- @returns com tipo e descrição
- @throws para cada exceção possível
- @example com ao menos dois exemplos (caso de sucesso e de erro/edge case)

[COLAR FUNÇÃO]
```

---

#### 📋 Revisão de Código

```
Você é um tech lead experiente revisando o código abaixo.
Avalie e forneça feedback sobre:
1. Correctness: há bugs ou comportamentos inesperados?
2. Performance: há gargalos ou operações desnecessariamente custosas?
3. Segurança: há vulnerabilidades ou dados sensíveis expostos?
4. Legibilidade: o código é fácil de entender e manter?
5. Boas práticas: segue os padrões de [TECNOLOGIA]?

Para cada problema encontrado, explique o impacto e sugira a correção.

[COLAR CÓDIGO]
```

---

## 🗒️ Notas Finais

Este caderno foi construído com o apoio do [NotebookLM](https://notebooklm.google.com/) do Google — acesse o caderno real criado para este projeto: **[Engenharia de Prompts para Devs no NotebookLM](https://notebooklm.google.com/notebook/0a10f3bd-9ba4-4ced-b279-1012731e1214)**. O NotebookLM permitiu carregar as fontes curadas e fazer perguntas diretamente sobre elas, cruzando informações e gerando resumos personalizados. A combinação de curadoria de fontes + engenharia de prompts + documentação do processo resultou em um material de estudo muito mais rico do que qualquer leitura linear das fontes originais.

> **"A IA não vai substituir o desenvolvedor que sabe usá-la bem. Mas vai substituir o desenvolvedor que ignora ela."**

---

*Projeto desenvolvido como parte do desafio de projeto da [DIO](https://www.dio.me/) — Engenharia de Prompts para Devs.*
