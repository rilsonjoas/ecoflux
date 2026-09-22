# Contrato de API — Ecoflux

Contrato REST de referência da aplicação (backend **Spring Boot** + **PostgreSQL**), derivado do modelo UML (`docs/DIAGRAMAS.md`), dos casos de uso e dos requisitos (`docs/PROPOSTA.md`).

- **Base URL:** `/api/v1`
- **Formato:** JSON (`application/json`)
- **Autenticação:** JWT (Bearer) — ver seção [Autenticação](#autenticação)
- **Idioma:** toda a documentação, **nomes de campos**, mensagens de erro e textos de negócio em **português (PT-BR)**, com notação camelCase para os campos.
- **Timestamps:** ISO-8601 (ex.: `2026-09-22T15:04:05Z`)
- **Enum `TipoResiduo` (v1):** `ORGANICO`, `METAL`, `PAPEL`, `PLASTICO`, `VIDRO`, `ELETRONICO` — (removido `CONTAMINADO` por ora; pendência em [Pendências](#10-pendências-para-a-próxima-reunião))

---

## 1. Autenticação

- **JWT** via header `Authorization: Bearer <token>`.
- Senhas armazenadas com hash **BCrypt** (RNF01).
- Papéis: `COLABORADOR` (colaboradores) e `FUNCIONARIO` (funcionários/manutenção).
- Validade do token: **24h** (configurável). Sem *refresh token* no MVP.
- Toda rota exceto `cadastro` e `login` exige token. Descartado em 401.

| Método | Rota | Perfil | Descrição |
| --- | --- | --- | --- |
| POST | `/auth/cadastro` | público | Cadastro de colaborador |
| POST | `/auth/login` | público | Autenticação |
| GET | `/auth/eu` | autenticado | Perfil do usuário logado |

### POST `/auth/cadastro`

**Requisição**

```json
{
  "nome": "Maria Silva",
  "email": "maria.silva@ufrpe.br",
  "senha": "segredo123",
  "dataNascimento": "2001-03-15"
}
```

**Resposta `201 Created`**

```json
{
  "id": "3f9c1e2a-...",
  "nome": "Maria Silva",
  "email": "maria.silva@ufrpe.br",
  "perfil": "COLABORADOR",
  "token": "eyJhbGciOiJIUzI1NiJ9..."
}
```

> Cadastro retorna o token (login automático). Para autenticidade total, a equipe pode trocar para exigir login separado.

Obs.: `dataNascimento` é opcional. Perfil criado como `COLABORADOR`; funcionários são cadastrados pela gestão (pendência — ver [Pendências](#10-pendências-para-a-próxima-reunião)).

### POST `/auth/login`

**Requisição**

```json
{ "email": "maria.silva@ufrpe.br", "senha": "segredo123" }
```

**Resposta `200 OK`**

```json
{
  "token": "eyJhbGciOiJIUzI1NiJ9...",
  "tipo": "Bearer",
  "usuario": {
    "id": "3f9c1e2a-...",
    "nome": "Maria Silva",
    "email": "maria.silva@ufrpe.br",
    "perfil": "COLABORADOR"
  }
}
```

### GET `/auth/eu`

**Resposta `200 OK`**

```json
{
  "id": "3f9c1e2a-...",
  "nome": "Maria Silva",
  "email": "maria.silva@ufrpe.br",
  "perfil": "COLABORADOR"
}
```

---

## 2. Pontos de descarte

| Método | Rota | Perfil | Descrição |
| --- | --- | --- | --- |
| GET | `/pontos-descarte` | colaborador, funcionário | Lista os pontos (mapa) |
| GET | `/pontos-descarte/{id}` | colaborador, funcionário | Detalhe de um ponto |
| POST | `/pontos-descarte` | funcionário | Cadastrar ponto (RF09) |
| PUT | `/pontos-descarte/{id}` | funcionário | Atualizar ponto (RF09) |
| DELETE | `/pontos-descarte/{id}` | funcionário | Remover ponto (RF09) |

### GET `/pontos-descarte`

Parâmetros opcionais de consulta: `ativo=true` (padrão `true`), `pagina` (padrão `0`), `tamanho` (padrão `20`).

**Resposta `200 OK`**

```json
{
  "conteudo": [
    {
      "id": "b8e2a4d1-...",
      "nome": "Em frente ao Departamento de Computação",
      "descricao": "Ponto de descarte seletivo em frente ao DC",
      "latitude": -8.0176,
      "longitude": -34.9495,
      "ativo": true,
      "tiposAceitos": ["PAPEL", "PLASTICO", "VIDRO", "METAL"],
      "lixeiras": [
        { "id": "c1d2e3f4-...", "identificacao": "DC-1", "ativa": true }
      ]
    }
  ],
  "pagina": 0,
  "tamanho": 20,
  "totalElementos": 4
}
```

### POST `/pontos-descarte` (funcionário)

**Requisição**

```json
{
  "nome": "Em frente ao CEGOE",
  "descricao": "Ponto de descarte seletivo em frente ao CEGOE",
  "latitude": -8.0183,
  "longitude": -34.9507,
  "tiposAceitos": ["PAPEL", "PLASTICO", "VIDRO", "METAL", "ORGANICO"]
}
```

**Resposta `201 Created`** — corpo igual ao item de lista (ponto criado). `400` quando latitude/longitude/tipos inválidos.

### PUT `/pontos-descarte/{id}` e DELETE `/pontos-descarte/{id}` (funcionário)

- `PUT` → `200 OK` (corpo igual ao POST). `404` quando o ponto não existe.
- `DELETE` → `204 No Content`. `404` quando não existe. Remoção lógica (marcar `ativo=false`) recomendada se houver descartes associados.

---

## 3. Lixeiras eletrônicas

| Método | Rota | Perfil | Descrição |
| --- | --- | --- | --- |
| GET | `/lixeiras?pontoId=` | funcionário | Lista lixeiras (filtro opcional por ponto) |
| POST | `/lixeiras` | funcionário | Adicionar lixeira a um ponto |
| PUT | `/lixeiras/{id}` | funcionário | Atualizar lixeira |
| DELETE | `/lixeiras/{id}` | funcionário | Remover lixeira |

**Exemplo do corpo** (POST/PUT):

```json
{
  "identificacao": "DC-2",
  "pontoId": "b8e2a4d1-...",
  "ativa": true
}
```

Resposta `201` (POST) / `200` (PUT) com a lixeira criada/atualizada; `404` quando o ponto não existe.

---

## 4. Descartes e pontuação

| Método | Rota | Perfil | Descrição |
| --- | --- | --- | --- |
| POST | `/descartes` | colaborador | Registrar um descarte (RF05/RF06) |
| GET | `/eu/pontuacao` | colaborador | Pontuação acumulada (RF07) |
| GET | `/eu/historico` | colaborador | Histórico de descartes (RF08) |
| GET | `/regras-pontuacao` | autenticado | Regras de pontuação vigentes |
| PUT | `/regras-pontuacao` | funcionário | Ajustar regras de pontuação |

### POST `/descartes`

Registro de um ato de descarte. Sem hardware, o colaborador insere `origem: "SIMULACAO"`. O **código de uso único** (RNF05) é exigido quando `origem: "LIXEIRA"` (integração IoT posterior).

**Requisição**

```json
{
  "pontoDescarteId": "b8e2a4d1-...",
  "lixeiraId": "c1d2e3f4-...",
  "tipoResiduo": "PAPEL",
  "origem": "SIMULACAO",
  "codigo": null
}
```

**Resposta `201 Created`**

```json
{
  "id": "9a8b7c6d-...",
  "dataHora": "2026-09-22T15:04:05Z",
  "tipoResiduo": "PAPEL",
  "pontoDescarte": "Em frente ao Departamento de Computação",
  "pontosObtidos": 15,
  "mensagem": "Descarte registrado com sucesso! Você ganhou 15 pontos.",
  "sequenciaAtual": null,
  "posicaoNoRanking": null
}
```

**Regras aplicadas pelo backend:**
- Pontos por **ato de descarte**, conforme a tabela de regras no banco (§7).
- **Limite diário de 5 descartes** por colaborador — além disso, `422` com código `LIMITE_DIARIO_ATINGIDO` (valor padrão; pendência de consenso na equipe — ver [Pendências](#10-pendências-para-a-próxima-reunião)).
- **Código de uso único** quando `origem=LIXEIRA`: código já utilizado → `422` código `CODIGO_JA_UTILIZADO`.
- `tipoResiduo` não aceito no ponto → `422` código `TIPO_NAO_ACEITO`.

**Erros de negócio (exemplos):**

```json
{
  "codigo": "LIMITE_DIARIO_ATINGIDO",
  "mensagem": "Você atingiu o limite diário de 5 descartes. Volte amanhã!",
  "detalhes": []
}
```

### GET `/eu/pontuacao`

**Resposta `200 OK`**

```json
{
  "totalPontos": 120,
  "descartesRegistrados": 8,
  "descartesHoje": 4,
  "limiteDiario": 5,
  "sequenciaAtual": null,
  "posicaoNoRanking": null
}
```

> `sequenciaAtual` e `posicaoNoRanking` ficam **reservados no contrato** para a v1.1 (streaks e ranking — ver [Planejado (v1.1)](#6-planejado-v11)).

### GET `/eu/historico`

Resposta paginada (`pagina`,`tamanho`), itens em ordem cronológica decrescente:

```json
{
  "conteudo": [
    {
      "id": "9a8b7c6d-...",
      "dataHora": "2026-09-22T15:04:05Z",
      "tipoResiduo": "PAPEL",
      "pontosObtidos": 15,
      "pontoDescarte": "Em frente ao Departamento de Computação"
    }
  ],
  "pagina": 0,
  "tamanho": 20,
  "totalElementos": 8
}
```

### Regras de pontuação (leitura e ajuste)

`GET /regras-pontuacao` → `200 OK`

```json
{
  "regras": [
    { "tipoResiduo": "ORGANICO", "pontos": 10 },
    { "tipoResiduo": "METAL", "pontos": 20 },
    { "tipoResiduo": "PAPEL", "pontos": 15 },
    { "tipoResiduo": "PLASTICO", "pontos": 15 },
    { "tipoResiduo": "VIDRO", "pontos": 20 },
    { "tipoResiduo": "ELETRONICO", "pontos": 25 }
  ],
  "limiteDiario": 5
}
```

`PUT /regras-pontuacao` (funcionário) — corpo no mesmo formato; atualiza os valores da tabela de regras **sem recompilar** (regras no banco).

---

## 5. Solicitações de limpeza (funcionário)

| Método | Rota | Perfil | Descrição |
| --- | --- | --- | --- |
| GET | `/solicitacoes-limpeza?abertas=true` | funcionário | Lista solicitações (filtro `abertas`) |
| POST | `/solicitacoes-limpeza/{id}/concluir` | funcionário | Registrar a realização da limpeza |

**Exemplo de item (GET):**

```json
{
  "id": "d4e5f6a7-...",
  "lixeira": { "id": "c1d2e3f4-...", "identificacao": "DC-1" },
  "pontoDescarte": "Em frente ao Departamento de Computação",
  "data": "2026-09-22T18:00:00Z",
  "aberta": true
}
```

`POST /solicitacoes-limpeza/{id}/concluir` — requisição `{"observacoes": "Lixeira esvaziada e limpa."}` → `200 OK`. `404` quando não existe; `422` quando a solicitação já foi concluída.

> Sem IoT, as solicitações são criadas **em simulação** (seed/dev). O gatilho automático por nível/peso da lixeira entra com o hardware (sensores) — pendência (ver [Pendências](#10-pendências-para-a-próxima-reunião)).

---

## 6. Planejado (v1.1)

Recursos **já previstos no contrato**, com campos reservados nas respostas atuais (`sequenciaAtual`, `posicaoNoRanking`), porém **sem implementação obrigatória agora**:

| Recurso | Rota futura | Nota |
| --- | --- | --- |
| Ranking | `GET /ranking` | Lista de colaboradores por total de pontos |
| Sequência (streak) | (interno à pontuação) | Dias seguidos com descarte |
| Recompensas | (em definição) | Resgates futuros; hoje só o acumulado de pontos |

**Decisão (2026-09-21):** entram no contrato como planejado/v1.1, mantendo o foco do MVP nas funcionalidades principais.

---

## 7. Regras de pontuação (valores padrão no banco)

Tabela `regra_pontuacao` (base fixa, **por ato** de descarte):

| `tipo_residuo` | `pontos` |
| --- | --- |
| ORGANICO | 10 |
| METAL | 20 |
| PAPEL | 15 |
| PLASTICO | 15 |
| VIDRO | 20 |
| ELETRONICO | 25 |

Parâmetro `limite_diario = 5` (padrão; pendência de consenso).

---

## 8. Códigos de erro (envelope padrão)

Toda resposta de erro usa o envelope:

```json
{
  "codigo": "CAMPO_OBRIGATORIO",
  "mensagem": "O campo 'senha' é obrigatório.",
  "detalhes": []
}
```

| HTTP | Códigos mais comuns |
| --- | --- |
| 400 | `JSON_INVALIDO`, `CAMPOS_INVALIDOS` (validação de formato) |
| 401 | `NAO_AUTENTICADO`, `TOKEN_EXPIRADO`, `SENHA_INCORRETA` |
| 403 | `PERMISSAO_NEGADA` |
| 404 | `RECURSO_NAO_ENCONTRADO` |
| 409 | `EMAIL_JA_CADASTRADO` |
| 422 | `LIMITE_DIARIO_ATINGIDO`, `CODIGO_JA_UTILIZADO`, `TIPO_NAO_ACEITO`, `SOLICITACAO_JA_CONCLUIDA` |

---

## 9. Carga inicial de exemplo (seed)

Pontos **plausíveis no campus da UFRPE (Recife)**, com coordenadas de exemplo — ainda aguardando **georreferenciamento real** (pendência):

| Nome do ponto | latitude | longitude | (exemplo) |
| --- | --- | --- | --- |
| Em frente ao Departamento de Computação | -8.0176 | -34.9495 | perto do DC |
| Em frente ao CEGOE | -8.0183 | -34.9507 | — |
| Biblioteca Setorial (DED) | -8.0168 | -34.9512 | — |
| Restaurante Universitário (RU) | -8.0170 | -34.9480 | — |

---

## 10. Pendências para a próxima reunião

Consolidado de tudo que deve ser discutido com a equipe/professora (a trazer na próxima semana):

1. **Limite diário de 5 descartes** — valor padrão aplicado, aguardando consenso da equipe.
2. **`CONTAMINADO` no enum `TipoResiduo`** — **removido do v1**; decidir se volta (e comportamento: aceita com 0 pontos, rejeita ou avisa).
3. **Regra de pontuação por tipo** — validar os valores padrão da tabela (§7) com a equipe.
4. **Responsividade como requisito** — ainda não conversado com a professora (vira RNF?).
5. **Sensor de descarte (peso/nível) da lixeira** — definição final conforme a **capacidade do aparelho** que será adquirido/implementado.
6. **Coordenadas reais dos pontos de descarte** — substituir os valores plausíveis do seed por georreferenciamento do campus.
7. **Fluxo de cadastro de funcionários** — quem e como cria contas de funcionário (fora do cadastro público).
8. **Sequência (streak) e ranking na prática** — confirmar entrada na v1.1 (contrato já reserva os campos).
9. **Recompensas** — sem recompensas reais no momento; manter apenas acumulado de pontos.
10. **Papéis e responsabilidades da equipe** — ainda "a definir" no README.

---

*Este documento é um contrato de referência para desenvolvimento. Altera-se junto com o código (springdoc gerará a versão viva da OpenAPI a partir da implementação). Em caso de divergência entre este arquivo e a implementação, a implementação + testes são a fonte da verdade e este arquivo deve ser atualizado.*