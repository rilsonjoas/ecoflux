# Ecoflux — Fluxo de Descarte Inteligente

Solução tecnológica para incentivar, facilitar e acompanhar o **descarte correto de resíduos no campus da UFRPE**, combinando mapa de pontos de descarte, gamificação e um protótipo IoT de lixeira inteligente (ESP32).

Projeto da disciplina **Engenharia de Software (2026.2)** — tema "Tecnologia a serviço da sustentabilidade na UFRPE".

---

## Estrutura do monorepo

```
ecoflux/
├── frontend/     # Site — HTML/CSS + JavaScript + OpenStreetMap
├── backend/      # API REST — Spring Boot
├── firmware/     # Protótipo de lixeira inteligente — ESP32/Arduino
├── docs/         # Documentação do projeto
├── README.md
├── LICENSE
└── .gitignore
```

## Escopo (MVP)

O fluxo essencial: o usuário encontra um ponto de descarte no mapa, realiza o descarte na lixeira inteligente, recebe um código de validação e obtém recompensa no site.

- Mapa do campus com pontos de descarte e tipos de resíduos aceitos
- Cadastro, autenticação, pontuação e histórico de descartes
- Gamificação: descartes validados geram pontos
- *(extra)* Lixeira ESP32/Arduino: seleção de tipo de resíduo + geração de código único via Wi-Fi

Requisitos detalhados (RF01–RF11, RNF01–RNF06) e critérios de sucesso em `docs/PROPOSTA.md`. Modelo de domínio, casos de uso e arquitetura em `docs/DIAGRAMAS.md`.

## Tecnologias

| Camada | Tecnologia |
| --- | --- |
| Site | HTML/CSS + JavaScript |
| Mapa | OpenStreetMap |
| Backend/API | Spring Boot |
| Banco de dados | PostgreSQL |
| Protótipo IoT | ESP32/Arduino (Wi-Fi + HTTP/HTTPS) |

## Documentação

| Documento | Descrição |
| --- | --- |
| `docs/PROPOSTA.md` | Proposta do MVP (base original) |
| `docs/DIAGRAMAS.md` | Diagrama de classes, casos de uso e arquitetura |
| `frontend/public/Ecoflux_Documento_de_Visao_do_Projeto.pdf` | Documento de Visão atualizado |
| `frontend/public/ecoflux-mockups.html` / `-v2.html` | Mockups visuais interativos |

## Links

- Figma (wireframe): https://www.figma.com/design/2y2sJ7QM6Z0utbzRBfmmY1/projeto_Eng_sof?node-id=17-60&p=f
- Trello: https://trello.com/b/fb33NuIQ/ecoflux-fluxo-de-descarte-inteligente

## Equipe

(Equipe da disciplina — preencher)