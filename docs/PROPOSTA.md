# Projeto de Descarte Inteligente de Resíduos no Campus da UFRPE

Proposta inicial para definição do Produto Mínimo Viável (MVP).

## 1. Descrição detalhada do tema

O projeto propõe o desenvolvimento de uma solução tecnológica para incentivar, facilitar e acompanhar o descarte correto de resíduos no campus da Universidade Federal Rural de Pernambuco (UFRPE). A proposta tem como componente principal uma aplicação web acessível por navegador, apoiada por uma aplicação servidora e um sistema de registro e pontuação de descartes.

No site, o usuário poderá visualizar um mapa do campus contendo os pontos de descarte disponíveis, consultar os tipos de resíduos aceitos em cada ponto, acompanhar sua pontuação e visualizar seu histórico de descartes. O sistema terá um mecanismo de gamificação: descartes corretamente registrados gerarão pontos, permitindo estimular a participação contínua da comunidade acadêmica.

Como requisito adicional e componente de prototipação, o projeto poderá incluir uma lixeira inteligente baseada em ESP32 e conectada à rede Wi-Fi. Nesse protótipo, o usuário poderá selecionar, por meio de uma interface física simples, o tipo de resíduo que pretende descartar. O dispositivo poderá se comunicar com o servidor e gerar um código único para posterior validação no site. Essa lixeira não faz parte do núcleo obrigatório do MVP e poderá ser implementada caso o tempo e os recursos permitam.

Como evolução possível, a mesma infraestrutura poderá receber sensores para monitoramento do nível de preenchimento das lixeiras, permitindo identificar pontos que necessitam de coleta. Entretanto, essa funcionalidade não é necessária para o primeiro MVP e deve ser tratada como uma extensão futura.

## 2. Problema associado

O descarte inadequado de resíduos em ambientes universitários pode reduzir a eficiência da coleta seletiva, aumentar a contaminação dos materiais recicláveis e dificultar o trabalho de coleta e gerenciamento dos resíduos. Mesmo quando existem pontos adequados para descarte, a simples disponibilização da infraestrutura não garante que os usuários irão utilizá-la corretamente.

Nesse contexto, existe uma oportunidade de utilizar tecnologia e mecanismos de gamificação para tornar o descarte correto mais simples, rastreável e estimulante. O problema central que o projeto pretende abordar pode ser definido como:

> Como incentivar e facilitar o descarte correto de resíduos no campus, ao mesmo tempo em que se registra a participação dos usuários e se fornece informações úteis para o gerenciamento dos pontos de descarte?

Problemas específicos que a solução pretende enfrentar:

- Baixo estímulo para que o usuário realize o descarte correto.
- Dificuldade de localizar pontos de descarte adequados no campus.
- Ausência de um mecanismo simples de registro e acompanhamento da participação individual.
- Falta de integração entre o usuário e os pontos físicos de descarte.
- Possível dificuldade de acompanhamento da utilização dos pontos de descarte pela administração.

## 3. Valor esperado

O valor esperado do projeto é criar uma solução que gere benefícios simultaneamente para os usuários e para a gestão do campus, utilizando uma combinação de informação, gamificação e Internet das Coisas (IoT).

### Para os estudantes, servidores e demais usuários

- Localização rápida dos pontos de descarte por meio de um mapa.
- Orientação sobre o descarte e os tipos de resíduos aceitos.
- Sistema de pontuação que incentive comportamentos ambientalmente adequados.
- Histórico individual de descartes e pontos acumulados.
- Experiência mais interativa e participativa com a infraestrutura de coleta seletiva.

### Para a gestão do campus

- Registro digital das operações realizadas nas lixeiras inteligentes.
- Possibilidade de acompanhar a utilização dos pontos de descarte.
- Base de dados que pode futuramente apoiar decisões sobre distribuição e manutenção das lixeiras.
- Possibilidade de monitorar o nível de preenchimento das lixeiras em versões futuras.

### Para o projeto acadêmico

- Integração entre desenvolvimento mobile, backend, banco de dados, redes e IoT.
- Aplicação prática de conceitos de Engenharia de Software, como requisitos, arquitetura, testes e integração.
- Construção de um protótipo demonstrável, com interação entre hardware e software.

## 4. Produto Mínimo Viável (MVP)

O MVP deverá priorizar o fluxo essencial de valor: o usuário encontra um ponto de descarte, realiza um descarte em uma lixeira inteligente, identifica o tipo de resíduo, recebe um código de validação e obtém uma recompensa no site.

Fluxo principal esperado:

1. Usuário realiza cadastro e autenticação no site.
2. Usuário consulta o mapa e localiza um ponto de descarte.
3. Usuário utiliza a lixeira inteligente e seleciona o tipo de resíduo.
4. ESP32 comunica a operação ao servidor por Wi-Fi.
5. Servidor registra o descarte e gera um código único.
6. Código é apresentado ao usuário pela lixeira.
7. O usuário consulta seu histórico e sua pontuação atualizada.
8. Servidor valida o código e atribui a pontuação.
9. Site atualiza a pontuação e o histórico do usuário.

## 5. Requisitos-chave para o MVP

### 5.1 Requisitos funcionais

| ID | Requisito |
| --- | --- |
| **RF01** | Cadastro de usuário: o sistema deve permitir que um usuário crie uma conta utilizando, no mínimo, nome, identificador/e-mail e senha. |
| **RF02** | Autenticação: o sistema deve permitir que o usuário faça login e acesse sua conta de forma autenticada. |
| **RF03** | Mapa de pontos de descarte: o site deve apresentar um mapa do campus com a localização dos pontos de descarte cadastrados. |
| **RF04** | Informações dos pontos: o site deve permitir consultar informações básicas de cada ponto, incluindo localização e tipos de resíduos aceitos. |
| **RF05** | Registro de descarte: o sistema deve registrar cada descarte validado, associando-o ao usuário, ponto de descarte, tipo de resíduo e data/hora. |
| **RF06** | Pontuação: o sistema deve atribuir pontos ao usuário após a validação de um descarte de acordo com as regras definidas pela equipe. |
| **RF07** | Consulta de pontuação: o site deve exibir a pontuação acumulada pelo usuário. |
| **RF08** | Histórico: o site deve permitir consultar os descartes e recompensas já registrados pelo usuário. |
| **RF09** | Gestão de pontos de descarte: o sistema deve permitir cadastrar e manter os pontos de descarte e seus respectivos tipos de resíduos aceitos. |
| **RF10** | *(adicional)* Protótipo de lixeira eletrônica: uma lixeira baseada em ESP32 capaz de selecionar o tipo de resíduo, comunicar-se com o servidor e gerar um código para validação no site. |
| **RF11** | *(adicional)* Validação por código: o site poderá permitir a inserção do código gerado pelo protótipo e o servidor deverá validá-lo antes de atribuir a recompensa. |

### 5.2 Requisitos não funcionais essenciais

| ID | Requisito |
| --- | --- |
| **RNF01** | **Segurança:** dados de autenticação não devem ser armazenados em texto puro (hash com **BCrypt**) e operações de descarte devem ser protegidas contra reutilização simples de códigos. |
| **RNF02** | **Disponibilidade:** o sistema deve lidar de forma controlada com falhas temporárias de conexão entre ESP32 e servidor, informando o usuário quando uma operação não puder ser concluída. |
| **RNF03** | **Usabilidade:** o fluxo de descarte e validação deve ser simples o suficiente para ser realizado rapidamente por um usuário no campus. |
| **RNF04** | **Manutenibilidade:** o backend, site e firmware devem possuir separação clara de responsabilidades para facilitar testes e evolução. |
| **RNF05** | **Integridade:** um código de descarte deve ser de uso único e não pode gerar pontos múltiplas vezes. |
| **RNF06** | **Escalabilidade básica:** a arquitetura deve permitir cadastrar mais de uma lixeira e mais de um ponto de descarte sem alteração estrutural significativa. |

> **Pendência de consulta (2026-09-21):** **responsividade** (comportamento correto em mobile e desktop, mobile-first) é necessária e ainda **não foi validada com a professora** — deve ser formalizada como requisito somente após essa consulta. Coerência visual segue a identidade visual (§9).

## 6. Escopo tecnológico sugerido

Uma arquitetura inicial compatível com o projeto é:

- Site (frontend): HTML/CSS + JavaScript — decisão de 2026-09-21 (o Flutter foi escanteado; ver também `docs/DIAGRAMAS.md` e o Documento de Visão).
- Backend/API REST: Spring Boot.
- Banco de dados: PostgreSQL.
- Protótipo IoT opcional: ESP32.
- Comunicação do ESP32 (caso o protótipo seja implementado): Wi-Fi + HTTP/HTTPS.
- Mapa: OpenStreetMap ou outro provedor de mapas adequado ao projeto.
- Segurança de senhas: BCrypt (hash no cadastro/login).

> Essas tecnologias são sugestões e podem ser substituídas conforme o conhecimento da equipe e as exigências da disciplina.

## 7. Funcionalidades fora do MVP

Para evitar que o projeto se torne excessivamente grande, as funcionalidades abaixo podem ser tratadas como extensões futuras:

- Sensor ultrassônico ou outro sensor para medir o nível de preenchimento da lixeira.
- Dashboard administrativo para acompanhamento dos pontos.
- QR Code em substituição ou complemento ao código digitado.
- Ranking entre usuários ou turmas.
- Sistema de recompensas reais.
- Notificações sobre campanhas e pontos de descarte.
- Reconhecimento automático do tipo de material por sensores ou visão computacional.
- Geolocalização do usuário e validação da proximidade do ponto de descarte.

## 8. Critério de sucesso do MVP

O MVP será considerado funcional quando a equipe conseguir demonstrar o fluxo essencial do site:

1. Usuário cadastrado acessa o site.
2. Usuário encontra a localização da lixeira/ponto de descarte no mapa.
3. Usuário realiza e registra um descarte em um ponto disponível.
4. Backend recebe e registra a operação de descarte.
5. Backend valida a operação e aplica as regras de pontuação.
6. O site apresenta a confirmação do descarte e a pontuação obtida.
7. O usuário consulta seu histórico e sua pontuação atualizada.
8. A integração com a lixeira eletrônica, incluindo códigos de validação, pode ser demonstrada como funcionalidade adicional.

**Resumo:** o MVP deve provar que site, backend e banco de dados conseguem trabalhar juntos para transformar um descarte em uma operação digital registrada e recompensada. O protótipo de lixeira eletrônica poderá ampliar a demonstração por meio de IoT, mas não será requisito essencial da primeira entrega.

## 9. Identidade visual (decisões de 2026-09-21)

O padrão visual do projeto segue o **mockup v1** (`frontend/public/ecoflux-mockups.html`), paleta *Floresta* e tipografia associada.

### Paleta Floresta

| Token | Cor | Hex |
| --- | --- | --- |
| `--page-bg` | Fundo claro | `#F7F5EF` |
| `--page-ink` | Texto | `#16231A` |
| `--page-muted` | Texto secundário | `#5B6B5C` |
| `--paper` | Cartões | `#F3EEE1` |
| `--paper-2` | Cartões secundários | `#E9E1CC` |
| `--forest` | Verde profundo (brand) | `#1C2B1E` |
| `--forest-2` | Verde escuro | `#24371F` |
| `--marigold` | Destaque/CTA | `#E7B23C` |
| `--brick` | Alerta/erro | `#B34B28` |
| `--sage` | Suporte/positivo | `#557A5F` |
| `--sage-light` | Suporte claro | `#86A98C` |

### Tipografia

- Títulos: **Fraunces** (serif/display).
- Texto: **Space Grotesk** (sans).

### Logo

- **Oficial:** `frontend/public/Logo Ecoflux — floresta.png` (recolor para a paleta Floresta).
- **Variações mantidas:** `Logo Ecoflux.png` (teal original) e `Variações do Logo Ecoflux.png` (monocromáticas) — para impressos em escala de cinza e uso histórico.

### Decisão de domínio (resolvida)

O enum `TipoResiduo` é `ORGANICO, METAL, PAPEL, PLASTICO, VIDRO, CONTAMINADO, ELETRONICO` — **tanto `CONTAMINADO` quanto `ELETRONICO` entram**, decisão da Reunião 1 (2026-09-21, ver `docs/REUNIOES.md`).

## 10. Links de referência

- Wireframing (Figma): https://www.figma.com/design/2y2sJ7QM6Z0utbzRBfmmY1/projeto_Eng_sof?node-id=17-60&p=f
- Trello (g-bot Ecoflux — fluxo de descarte inteligente): https://trello.com/b/fb33NuIQ/ecoflux-fluxo-de-descarte-inteligente