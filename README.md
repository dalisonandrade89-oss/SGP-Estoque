# SGP Estoque

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](LICENSE)

**Sistema de Gestão de Presenças, Inventário, Tarefas e Prêmio — para equipes de estoque.**

Um aplicativo web de arquivo único (single-file), que roda inteiramente no navegador, sem
back-end, sem instalação e sem necessidade de servidor. Controla o dia a dia de uma equipe
de estoque — presenças, inventário diário, tarefas extras — e calcula automaticamente o
prêmio mensal de cada colaborador com base em critérios ponderados e dinâmicos.

> 📄 Todo o sistema está em **um único arquivo `.html`**. Baixe, dê duplo clique, e está
> rodando — não precisa de Node, Python, banco de dados ou qualquer instalação.

---

## Índice

- [Funcionalidades](#funcionalidades)
- [Como usar](#como-usar)
- [Como funciona o cálculo do prêmio](#como-funciona-o-cálculo-do-prêmio)
- [Onde os dados ficam salvos](#onde-os-dados-ficam-salvos)
- [Compatibilidade de navegador](#compatibilidade-de-navegador)
- [Estrutura do projeto](#estrutura-do-projeto)
- [Rodando localmente / contribuindo](#rodando-localmente--contribuindo)
- [Roadmap / limitações conhecidas](#roadmap--limitações-conhecidas)
- [Licença](#licença)

---

## Funcionalidades

### 📊 Dashboard
- Cálculo automático da nota final do prêmio de cada colaborador, com **pesos dinâmicos**
  (veja a seção de cálculo abaixo).
- Duas visualizações: **cards** (um cartão por colaborador) e **lista** (compacta,
  ocupando toda a largura da tela).
- Ordenação por nome ou por nota do prêmio, crescente/decrescente.
- Período de apuração configurável, com contagem automática de dias.
- Exportação em **.txt**, **PDF** (via impressão do navegador) e **planilha .xlsx**.

### ✓ Atividades
- Visão consolidada de tudo que a equipe executou no período — inventário diário e
  tarefas extras juntos, numa lista só.
- Filtros por data, colaborador e tipo de atividade.
- Mesmas opções de exportação do Dashboard.

### 👥 Equipe
- Cadastro de colaboradores: nome, função, turno e valor base do prêmio.
- Edição direta na tabela (sem botão salvar — salva ao sair do campo).
- Busca por nome e filtro por função/turno (as opções são geradas a partir do que já
  está cadastrado).

### 🕐 Presenças
- Lançamento diário de presença por colaborador, com status (Presente, Falta, Falta
  Justificada, Atraso, **Férias**) e nota opcional de comportamento (1–5).
- **Férias são neutras**: não contam como falta nem como presença — o dia sai do
  denominador do cálculo de assiduidade.
- **Fins de semana são neutros por padrão**: sábado e domingo não entram na conta de
  assiduidade, a menos que exista um lançamento de presença naquele dia específico
  (ou seja, a pessoa foi chamada a trabalhar excepcionalmente).
- Filtro por colaborador e intervalo de datas.

### 📦 Inventário Diário
- Registro da contagem fixa de referências do dia, tratada como **força-tarefa em
  grupo**: vários colaboradores podem ser marcados como participantes do mesmo
  lançamento, e a conclusão vale igualmente para todos, independente de quantas
  referências cada um contou individualmente.
- Filtro por data, faixa de quantidade de referências contadas e status.

### 📝 Tarefas Extras
- Registro de demandas pontuais do dia a dia (fora do inventário fixo), com nota de
  qualidade (1–5) quando concluídas.
- Filtro por data, colaborador, status e qualidade.

### 💾 Backup e sincronização
- **Exportar / Importar `.json`** — backup manual completo de todos os dados.
- **Conectar arquivo de dados** — em navegadores compatíveis (Chrome, Edge, Opera),
  conecta a um arquivo `.json` local e salva automaticamente nele a cada alteração,
  sem precisar clicar em nada.
- **Resetar todos os dados** — atrás de um controle deslizante ("arraste para
  revelar") e duas confirmações, para evitar apagar dados por engano.

---

## Como usar

1. Baixe o arquivo `SGP-Estoque.html` deste repositório.
2. Dê duplo clique nele — abre direto no seu navegador padrão.
3. Cadastre sua equipe na aba **Equipe**.
4. Lance presenças, inventário e tarefas extras nas abas correspondentes, no dia a dia.
5. Vá até o **Dashboard**, configure o período de apuração e veja o prêmio calculado.
6. Exporte o relatório (.txt, PDF ou planilha) para enviar ao RH ou arquivar.

Não é necessário nenhum passo de instalação, build ou servidor.

---

## Como funciona o cálculo do prêmio

A nota final é uma média ponderada de três critérios:

```
Nota Final = 50% Produtividade + 25% Qualidade + 25% Assiduidade e Comportamento
```

| Critério | Peso padrão | Como é medido |
|---|---|---|
| **Produtividade** | 50% | (inventários concluídos + tarefas concluídas) ÷ (inventários atribuídos + tarefas atribuídas) |
| **Qualidade** | 25% | Combina o % de inventários concluídos com a nota média das tarefas extras (1–5 estrelas) — usa as duas fontes quando existirem, ou só a que existir |
| **Assiduidade e Comportamento** | 25% | Metade vem de (dias presentes ÷ dias úteis do período) e metade da nota média de comportamento lançada nas presenças |

### Pesos dinâmicos

Tarefas extras são ocasionais — nem todo mundo recebe uma no período. Por isso, se um
critério **não se aplica** a um colaborador (por exemplo, ninguém atribuiu tarefa extra
nem inventário a ele no período), o peso desse critério é **redistribuído
proporcionalmente** entre os critérios que se aplicam, em vez de simplesmente zerar a
nota. Ninguém é penalizado por não ter recebido uma oportunidade.

### Dias úteis: fins de semana e férias são neutros

O denominador de assiduidade não conta todos os dias corridos do período. A regra é:

- **Segunda a sexta**: sempre contam.
- **Sábado e domingo**: só contam se existir um lançamento de presença da pessoa
  naquele dia específico (ou seja, ela foi chamada a trabalhar excepcionalmente).
- **Dias marcados como "Férias"**: são descontados do total — não contam contra a
  assiduidade.

---

## Onde os dados ficam salvos

Por padrão, os dados ficam salvos no **`localStorage` do navegador**, no computador
onde o arquivo foi aberto. Isso significa:

- Os dados **não** são enviados para nenhum servidor.
- Se você abrir o mesmo arquivo em outro computador (ou em modo anônimo), ele começa
  vazio.
- Para levar os dados entre computadores, use o backup manual (**Exportar/Importar
  .json**) ou conecte um arquivo de dados compartilhado (**Conectar arquivo de
  dados**, disponível em navegadores baseados em Chromium).

---

## Compatibilidade de navegador

| Recurso | Navegadores suportados |
|---|---|
| Uso geral do sistema | Qualquer navegador moderno (Chrome, Edge, Firefox, Safari) |
| Salvamento automático em arquivo (Conectar arquivo de dados) | Chrome, Edge, Opera (requer [File System Access API](https://developer.mozilla.org/en-US/docs/Web/API/File_System_Access_API)) |
| Exportar Planilha (.xlsx) | Qualquer navegador, mas **requer conexão com a internet** no momento do clique (carrega a biblioteca [SheetJS](https://sheetjs.com/) via CDN) |
| Exportar PDF / .txt | Qualquer navegador, funciona offline |

---

## Estrutura do projeto

```
.
├── SGP-Estoque.html   # aplicativo completo — HTML, CSS e JS em um único arquivo
└── README.md
```

Não há dependências de build, `package.json` ou processo de compilação. O arquivo usa:
- Vanilla JavaScript (sem frameworks).
- [SheetJS](https://sheetjs.com/) via CDN, carregado sob demanda, apenas para a
  exportação de planilha.

---

## Rodando localmente / contribuindo

Como é um arquivo único, "rodar localmente" é simplesmente abrir o `.html` no
navegador. Para desenvolver:

1. Clone o repositório.
2. Edite `SGP-Estoque.html` no seu editor de preferência.
3. Abra o arquivo no navegador para testar — recarregue a página a cada alteração.

Sugestões de melhoria, correções de bugs e pull requests são bem-vindos. Ao propor
mudanças no cálculo do prêmio, descreva o cenário (dados de entrada) e o resultado
esperado, para facilitar a revisão.

---

## Roadmap / limitações conhecidas

- Os dados não sincronizam automaticamente entre computadores diferentes — depende de
  backup manual ou de um arquivo de dados compartilhado numa pasta em comum.
- Colaboradores removidos da equipe saem do cálculo do prêmio para **todos** os
  períodos (inclusive os passados) — o histórico de lançamentos permanece salvo, mas
  não é recalculado automaticamente. Uma versão futura pode adicionar datas de
  admissão/desligamento para tratar isso de forma automática por período.
- Sem suporte a múltiplos usuários/permissões — é uma ferramenta de uso individual ou
  de pequena equipe compartilhando um único arquivo de dados.

---

## Licença

Este projeto está licenciado sob a **[GNU General Public License v3.0](LICENSE)**.

Em resumo, a GPL-3.0 permite que qualquer pessoa use, estude, modifique e distribua
este software livremente, desde que:

- Trabalhos derivados também sejam distribuídos sob a **GPL-3.0** (é uma licença
  *copyleft* — o código permanece livre em qualquer redistribuição).
- O aviso de copyright e a licença original sejam mantidos.
- Alterações feitas no código sejam identificadas.
- O código-fonte esteja disponível para quem receber o software.

Veja o arquivo [`LICENSE`](LICENSE) na raiz do repositório para o texto completo.
