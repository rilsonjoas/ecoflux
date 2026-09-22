# Reuniões do Projeto

Registro das atas e decisões das reuniões da equipe.

## Reunião 1 — Arquitetura e Apresentação do Sistema

- **Data:** 2026-09-21
- **Natureza:** planejamento (Scrum)
- **Objetivo:** apresentar a arquitetura planejada, revisar diagramas, alinhar requisitos do MVP e distribuir as atividades de apresentação do projeto.

### 1. Objetivo da reunião

Apresentar a arquitetura planejada para o sistema, revisar os diagramas produzidos, alinhar os requisitos do produto mínimo viável e distribuir as atividades relacionadas à apresentação do projeto.

### 2. Visão geral do sistema

O projeto consiste em um sistema de descarte inteligente de resíduos. A solução permitirá que colaboradores realizem descartes em lixeiras eletrônicas, acumulem pontos e consultem seu histórico de utilização.

O sistema também contará com funcionalidades voltadas aos funcionários responsáveis pela manutenção, coleta e limpeza das lixeiras.

### 3. Perfis de usuário

#### Colaborador

Representa alunos, moradores das proximidades, trabalhadores da região e demais pessoas que utilizem os pontos de descarte.

Principais funcionalidades:

- Realizar cadastro e login;
- Consultar os pontos de descarte disponíveis;
- Consultar a pontuação acumulada;
- Consultar o histórico de descartes;
- Solicitar um código temporário de validação;
- Realizar descartes nas lixeiras eletrônicas.

#### Funcionário

Representa as pessoas responsáveis pela operação e manutenção dos equipamentos.

Principais funcionalidades:

- Consultar solicitações de limpeza;
- Assumir e realizar a limpeza de uma lixeira;
- Criar e remover pontos de descarte;
- Adicionar ou remover lixeiras;
- Consultar a localização dos pontos de descarte.

> **Nota de decisão (2026-09-21):** nos documentos do projeto, o **Funcionário consulta e executa a limpeza** (não há fluxo formal de "assumir" a solicitação no modelo atual).

#### Lixeira eletrônica

A lixeira será tratada como um agente independente integrado ao sistema.

Suas funções incluem:

- Validar o código temporário informado pelo colaborador;
- Identificar a realização de um descarte;
- Enviar os dados do descarte ao sistema;
- Detectar quando estiver cheia;
- Gerar solicitações de limpeza.

### 4. Fluxo de descarte

Passos planejados:

1. O colaborador acessa o sistema e solicita um código temporário;
2. O código é informado na lixeira eletrônica;
3. A lixeira valida o código com o back-end;
4. Após a validação, a lixeira é desbloqueada;
5. O colaborador deposita o resíduo;
6. O equipamento identifica o aumento de peso;
7. O descarte é registrado no histórico do usuário;
8. A pontuação correspondente é creditada ao colaborador.

O código deverá ser de uso único, evitando que o mesmo descarte gere pontos mais de uma vez.

> **Nota de alinhamento (2026-09-21):** o **fluxo de validação oficial do projeto é o do Documento de Visão** (código gerado pela lixeira → validado no site). O passo "solicitar código temporário" da ata será revisitado conforme a implementação; a detecção de descarte (ex.: **sensor de peso**) será definida de acordo com a **capacidade do aparelho** efetivamente implementado.

### 5. Pontos de descarte

Cada lixeira eletrônica estará vinculada a um ponto de descarte. Um mesmo ponto poderá possuir várias lixeiras.

Os pontos serão identificados por latitude e longitude e exibidos em um mapa. Foi sugerida a utilização do OpenStreetMap para implementar essa funcionalidade.

### 6. Solicitações de limpeza

Quando uma lixeira atingir determinado peso ou nível de ocupação, ela deverá gerar automaticamente uma solicitação de limpeza.

A solicitação ficará disponível para todos os funcionários cadastrados. Um funcionário poderá consultar as solicitações em aberto, selecionar uma delas e registrar a realização da limpeza.

Durante o projeto piloto, esses perfis poderão ser representados por integrantes da própria equipe.

### 7. Modelagem do sistema

O diagrama UML apresentado possui nove classes. Entre elas, destacam-se:

- **Usuário:** classe abstrata que reúne os atributos comuns dos usuários;
- **Colaborador:** especialização de usuário voltada à realização de descartes;
- **Funcionário:** especialização com permissões de manutenção e administração;
- **Lixeira eletrônica:** equipamento conectado ao sistema;
- **Ponto de descarte:** representa a localização que agrupa uma ou mais lixeiras;
- **Descarte:** registro persistente da operação realizada pelo colaborador;
- **Código de descarte:** elemento temporário utilizado na validação;
- **Tipo de resíduo:** enumeração dos resíduos aceitos — orgânico, metal, plástico, papel, vidro e **eletrônico**;
- **Solicitação de limpeza:** registro emitido quando uma lixeira precisa ser esvaziada.

A distinção entre os tipos de usuário será feita principalmente pelos métodos e permissões disponíveis para cada perfil.

> **Decisão de domínio (2026-09-21; revista no mesmo dia):** **`CONTAMINADO` fica de fora do v1 por enquanto** e volta a ser pendência de consulta. **`ELETRONICO` entra** no enum `TipoResiduo`. Ver `docs/DIAGRAMAS.md`.

### 8. Arquitetura tecnológica

A arquitetura proposta está dividida nos seguintes componentes:

#### IoT

- Microcontrolador ESP32;
- Responsável pela interação com a lixeira eletrônica;
- Comunicação com o back-end por HTTP ou HTTPS;
- Solicitação e validação de códigos junto ao back-end.

#### Front-end

- Interface utilizada pelos colaboradores e funcionários;
- Comunicação com o back-end por HTTP ou HTTPS;
- Troca de dados no formato JSON.

#### Back-end

- Desenvolvimento em Java;
- Utilização do framework Spring Boot;
- Responsável pelas regras de negócio, autenticação, pontuação, registros e integrações.

#### Banco de dados

- PostgreSQL;
- Armazenamento de usuários, pontos de descarte, lixeiras, históricos, solicitações e demais informações persistentes.

#### Segurança

Foi sugerida a utilização do **BCrypt** para armazenar as senhas de forma segura por meio de hash. (Ver RNF01.)

### 9. Requisitos e priorização

Os requisitos foram agrupados em funcionalidades maiores:

- Cadastro e autenticação;
- Mapa e informações dos pontos de descarte;
- Pontuação, histórico e registro dos descartes;
- Integração com a lixeira eletrônica.

Vários requisitos pertencem a uma mesma funcionalidade. A equipe decidiu manter a priorização para orientar o desenvolvimento do produto mínimo viável.

A integração completa com a lixeira eletrônica foi classificada com prioridade menor, podendo ser implementada depois que as funcionalidades principais estiverem funcionando corretamente.

#### Requisitos não funcionais

Foram destacados três pontos:

- **Segurança:** proteção dos dados e das credenciais dos usuários;
- **Integridade:** códigos de descarte de uso único, sem geração duplicada de pontos;
- **Disponibilidade:** manutenção do sistema em funcionamento e acessível aos usuários.

### 10. Organização da apresentação

A apresentação deverá reunir:

- Requisitos priorizados;
- Ferramentas e tecnologias;
- Diagrama UML;
- Diagrama de casos de uso;
- Fluxo de funcionamento do sistema.

Os diagramas deverão ser adaptados para um formato mais visual. A equipe deverá produzir uma **única apresentação**, mantendo o **mesmo padrão visual em todos os slides**.

### 11. Divisão das apresentações

Divisão inicial:

- **Luiz Felipe Silva (Felipe):** diagrama UML;
- **Rilson:** tecnologias e arquitetura;
- **Flávia:** fluxo e diagrama de casos de uso;
- **Marcus e Hildemário:** requisitos, divididos em duas partes.

### 12. Conclusão

A reunião consolidou a arquitetura inicial do sistema, o fluxo de descarte, os perfis de usuário, os requisitos prioritários e as tecnologias previstas. Também foram definidos os responsáveis pelas partes da apresentação e os próximos passos para organização dos materiais do projeto.

---

### Decisões registradas nesta reunião

| Decisão | Status |
| --- | --- |
| Enum `TipoResiduo` inclui **CONTAMINADO e ELETRONICO** | **Revista (2026-09-21):** `CONTAMINADO` sai do v1 (pendência); `ELETRONICO` fica |
| **BCrypt** para hash de senhas (cadastro/login) | Adotado (ver RNF01) |
| **Luiz Felipe Silva** eleito **Scrum Master** | Eleito (2026-09-21) |
| Fluxo de validação oficial = Documento (código da lixeira → site) | Confirmado (2026-09-21) |
| Sensor de descarte (peso/nível) conforme capacidade do aparelho | Em definição |
| Papéis/responsabilidades da equipe no projeto | A definir |
---

## Reunião 2 — Hospedagem, Enum e Papéis da Equipe

- **Data:** a definir (próxima)
- **Natureza:** planejamento (Scrum)
- **Objetivo:** decidir onde o banco e a aplicação ficam hospedados, revisar a pendência do `CONTAMINADO` no enum, validar a responsividade com a professora e fechar os papéis da equipe.

### 1. Contexto: o que já está fechado (não reabrir)

- Enum `TipoResiduo` v1 = 6 valores: `ORGANICO, METAL, PAPEL, PLASTICO, VIDRO, ELETRONICO`.
- `CONTAMINADO` está **fora** do v1 por enquanto (voltou a ser pendência — ver item 2).
- Senhas: **BCrypt** (RNF01).
- **Scrum Master:** Luiz Felipe Silva.
- Fluxo de descarte = o do Documento (código da lixeira → site).
- Reunião 1 (2026-09-21) definiu arquitetura, diagramas e divisão de apresentação.

### 2. Pauta — decisões em aberto

#### 2.1 Onde hospedar banco e aplicação
Recomendação (VPS própria, custo zero, demo acessível de qualquer lugar):

| Opção | Custo | Prós | Contras |
| --- | --- | --- | --- |
| Localhost | R$ 0 | Demo garantida na própria máquina | Só acessível na máquina do autor; professora não testa |
| Postgres gerenciado (ex.: Neon/Supabase) | ~US$ 15–25/mês | Set-up rápido, sem operação | Custo mensal; free tiers limitados/imprevisíveis |
| **VPS própria** (decisão recomendada) | R$ 0 | Custo zero; backup/restore já testados; demo acessível em qualquer lugar | Requer manutenção básica |

**Proposta:** VPS própria + Postgres já com backup/restore testados.

#### 2.2 `CONTAMINADO` volta ao enum?
- Vai impactar: comportamento de descarte, pontuação (0 pontos? rejeita? avisa?).
- Voto da equipe necessário. Se não entrar no v1, registrar justificativa para a professora.

#### 2.3 Responsividade × professora
- Cell-first (`720px` do mockup) é necessário, mas **não validado** se vira requisito (RF/RNF).
- **Ação:** levar à professora para confirmar se entra como requisito formal.

#### 2.4 Papéis e responsabilidades da equipe
- Só Scrum Master eleito (Luiz Felipe). Definir: Product Owner, desenvolvedores (front/back/IoT), testes, apresentação.

### 3. Status das decisões

| Decisão | Status |
| --- | --- |
| Hospedagem (local × gerenciado × VPS própria) | Em aberto — proposta: VPS própria |
| `CONTAMINADO` volta ao enum? | Em aberto |
| Responsividade vira requisito? | Em aberto — consultar professora |
| Papéis/responsabilidades da equipe | Em aberto |

### 4. Ações desta reunião

| Ação | Responsável | Prazo |
| --- | --- | --- |
| Validar responsividade com a professora | Equipe/Scrum Master | Próxima reunião |
| Decidir hospedagem (voto) | Equipe | Próxima reunião |
| Definir papéis | Equipe | Próxima reunião |
