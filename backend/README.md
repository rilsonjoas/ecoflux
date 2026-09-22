# Ecoflux — Backend/API (Spring Boot)

API REST do Ecoflux. Responsável pela lógica de negócio e persistência:

- Cadastro e autenticação de usuários
- CRUD de pontos de descarte e tipos de resíduos
- Registro e validação de descartes (código de uso único)
- Regras de pontuação e histórico

> Stack sugerida: **Spring Boot** + **PostgreSQL**. Inicializar com Spring Initializr (https://start.spring.io) com dependências: Web, Data JPA, PostgreSQL, Security, Validation.