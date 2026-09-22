# Ecoflux — Frontend (HTML/CSS + JavaScript)

Site do Ecoflux, acessível por navegador. Responsável pela interface:

- Mapa do campus com pontos de descarte (**OpenStreetMap**)
- Consulta de tipos de resíduos por ponto
- Cadastro, autenticação e acompanhamento de pontuação/histórico
- Validação do código gerado pela lixeira (RF11)

> **Decisão de stack (2026-09-21):** Flutter escanteado — o frontend é **HTML/CSS + JavaScript** puro, consumindo a API REST do backend em JSON. Os mockups interativos em `public/` servem de ponto de partida visual.

## Estrutura

```
frontend/
├── public/
│   ├── ecoflux-mockups.html       # Mockup v1 — frame mobile, paleta floresta
│   ├── ecoflux-mockups-v2.html    # Mockup v2 — navegador + quiosque, paleta costeira
│   ├── Ecoflux_Documento_de_Visao_do_Projeto.pdf
│   └── Projeto_Descarte_Inteligente_UFRPE_MVP_atualizado.pdf
├── index.html                     # App principal (a criar)
├── css/                           # Folhas de estilo (a criar)
├── js/                            # Lógica do frontend (a criar)
└── README.md
```

## Requisitos atendidos no frontend

- RF01/RF02 — Cadastro e autenticação (formulários + sessão via API)
- RF03 — Mapa de pontos de descarte (Leaflet/OpenStreetMap)
- RF04 — Informações dos pontos e tipos de resíduos
- RF07 — Consulta de pontuação acumulada
- RF08 — Histórico de descartes
- RF11 — Validação por código (inserção do código gerado pela lixeira)