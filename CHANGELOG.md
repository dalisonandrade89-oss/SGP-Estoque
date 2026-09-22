# Changelog

Todas as mudanças relevantes deste projeto são documentadas neste arquivo.

O formato é baseado em [Keep a Changelog](https://keepachangelog.com/pt-BR/1.0.0/),
e o versionamento segue o esquema `V AA.MM.PP` usado internamente pelo app (exibido
no rodapé da barra lateral), incrementando o último número a cada modificação.

---

## [01.01.07]

### Changed
- Cadastro padrão da equipe substituído por nomes fictícios (Ana Souza, Bruno Lima,
  Carlos Pereira, Diego Alves, Elaine Rocha), para permitir a publicação do código em
  repositório público sem expor dados reais de colaboradores.

---

## [01.01.06]

### Changed
- Sistema renomeado para **SGP Estoque** em todos os textos visíveis (título da aba do
  navegador, cabeçalho da barra lateral).

### Added
- Texto de destaque em azul ("SGP Estoque") exibido logo abaixo do indicador de status
  do arquivo conectado, na barra lateral.

---

## [01.01.05]

### Changed
- Navegação em telas estreitas (metade da tela ou menos): a barra horizontal de abas foi
  substituída por um menu dropdown, mantendo o menu lateral normal em telas cheias.

---

## [01.01.04]

### Added
- Nova aba **Atividades**, consolidando os lançamentos de Inventário Diário e Tarefas
  Extras em uma única lista, filtrável por data, colaborador e tipo.
- Exportação da aba Atividades em `.txt`, PDF e planilha `.xlsx`.

---

## [01.01.03]

### Changed
- Critério de **Qualidade** passou a considerar também o percentual de inventários
  concluídos (não apenas a nota das tarefas extras) — completar a meta diária do
  inventário conta como execução com qualidade.

### Fixed
- Sábado e domingo agora são tratados como dias neutros no cálculo de **Assiduidade**
  por padrão (não contam nem a favor nem contra), a menos que exista um lançamento de
  presença da pessoa naquele dia específico (trabalho excepcional de fim de semana).
  Antes, todo dia corrido do período contava contra a assiduidade, mesmo sem
  expectativa de trabalho no fim de semana.

---

## [01.01.02]

### Fixed
- **Bug crítico de perda de dados** no recurso "Conectar arquivo de dados": o app usava
  o seletor de arquivos de "Salvar" do navegador (`showSaveFilePicker`), que em vários
  sistemas apaga o conteúdo do arquivo assim que um arquivo existente é selecionado —
  antes mesmo de o app ler os dados. Trocado para o seletor de "Abrir"
  (`showOpenFilePicker`), que é somente leitura por natureza e nunca corre esse risco.
  Depois de abrir o arquivo com segurança, o app solicita permissão de escrita
  separadamente para manter o salvamento automático.

### Added
- Número de versão do aplicativo exibido na barra lateral, abaixo do botão
  "Apagar tudo".

---

## [01.01.01] — Base

Versão de referência a partir da qual o versionamento passou a ser acompanhado neste
changelog. Reúne as funcionalidades já existentes no projeto até este ponto:

### Added
- **Dashboard**: cálculo automático do prêmio mensal por colaborador, com cards e modo
  lista, ordenação por nome/prêmio, período de apuração configurável e exportação em
  `.txt`, PDF e planilha.
- **Equipe**: cadastro de colaboradores (nome, função, turno, valor base do prêmio),
  com edição direta na tabela e filtro por nome/função/turno.
- **Presenças**: lançamento diário de presença com status (Presente, Falta, Falta
  Justificada, Atraso, **Férias**) e nota opcional de comportamento; filtro por
  colaborador e período. Férias tratadas como neutras no cálculo de assiduidade.
- **Inventário Diário**: registro da contagem fixa de referências como força-tarefa em
  grupo (múltiplos participantes por lançamento); filtro por data, quantidade de
  referências e status.
- **Tarefas Extras**: registro de demandas pontuais com nota de qualidade; filtro por
  data, colaborador, status e qualidade.
- **Cálculo do prêmio com pesos dinâmicos**: 50% produtividade, 25% qualidade, 25%
  assiduidade e comportamento — com redistribuição automática de peso quando um
  critério não se aplica ao colaborador no período (por exemplo, ausência de tarefas
  extras), para não penalizar por falta de oportunidade.
- **Backup e sincronização**: exportar/importar `.json`, conectar um arquivo de dados
  local com salvamento automático (em navegadores compatíveis), e resetar todos os
  dados (atrás de um controle deslizante e duas confirmações).
- Renomeação do app e do arquivo de dados padrão para "Controle de Premiação" /
  "base de dados" (nomes internos anteriores ao rebranding para SGP Estoque).
