# 🏃 Calculadora VDOT — Jack Daniels Running Formula

> Calculadora VDOT completa em Português Europeu. Descobre o teu score VDOT, previsões de tempo para distâncias standard e ritmos personalizados para as 5 zonas de treino do método Jack Daniels.

![Versão](https://img.shields.io/badge/versão-1.0-FFB92E?style=flat-square&labelColor=332501)
![Idioma](https://img.shields.io/badge/idioma-Português%20Europeu-FFB446?style=flat-square&labelColor=332501)
![Stack](https://img.shields.io/badge/stack-HTML%20·%20CSS%20·%20JavaScript-E79B10?style=flat-square&labelColor=332501)
![Licença](https://img.shields.io/badge/licença-MIT-A16700?style=flat-square&labelColor=332501)

---

## Índice

- [Visão Geral](#visão-geral)
- [Funcionalidades](#funcionalidades)
- [Demo](#demo)
- [Como Usar](#como-usar)
- [Motor de Cálculo](#motor-de-cálculo)
- [Zonas de Treino](#zonas-de-treino)
- [Ajustes Avançados](#ajustes-avançados)
- [Exportação PDF](#exportação-pdf)
- [Estrutura do Projeto](#estrutura-do-projeto)
- [Roadmap](#roadmap)
- [Aviso Legal](#aviso-legal)

---

## Visão Geral

A **Calculadora VDOT** é uma aplicação web estática de página única (SPA) que implementa fielmente o Sistema VDOT desenvolvido pelo Dr. Jack Daniels e pelo matemático Jimmy Gilbert.

Toda a lógica de cálculo é executada **100% no browser do utilizador** — não existe servidor de aplicação, base de dados, cookies de rastreamento ou envio de dados para servidores externos.

### Princípios de Design

- **Zero dependências de servidor** — lógica totalmente client-side
- **Privacidade por defeito** — nenhum dado é transmitido ou armazenado
- **Métricas SI** — quilómetros, metros, graus Celsius
- **Idioma fixo** — Português Europeu (pt-PT)
- **Ficheiro único** — toda a aplicação num único `.html`, sem build step, sem dependências npm

---

## Funcionalidades

### Cálculo Principal
- Introdução de resultado de prova (distância + tempo) com validação em tempo real
- Cálculo do **score VDOT** via fórmula original de Daniels-Gilbert
- Classificação automática do nível do corredor (Iniciante → Elite Nacional)

### Aba 1 — Tempos Equivalentes
- Previsão de tempos para **10 km, 15 km, Meia-Maratona e Maratona**
- Pace médio por distância em min:ss/km
- Destaque visual da distância de referência ("Âncora")

### Aba 2 — Ritmos de Treino
- **5 zonas de treino** (E, M, T, I, R) com paces em min:ss/km
- Intervalos de pace (mínimo/máximo) para zonas E, M e T
- Passagens de pista (200m, 400m, 800m, 1000m, 1200m) para zona I
- Passagens de pista (200m, 400m) para zona R
- Tooltip ⓘ com descrição fisiológica de cada zona

### Aba 3 — Ajustes Avançados
- **Temperatura e humidade** — ajuste de pace para condições quentes/húmidas
- **Altitude** — correção para treino em altitude elevada (zonas E, M, T, I)
- **Inclinação do terreno** — Grade Adjusted Pace (GAP) para trail e estrada ondulada
- Tabela comparativa com pace base, pace ajustado e delta em seg/km

### Exportação PDF
- Geração de PDF 100% client-side via jsPDF
- Nome do ficheiro automático: `vdot-[score]-[distância]-[data].pdf`
- Inclui aviso legal e indicação de que os valores são estimativos

---

## Demo

Abre o ficheiro `vdot-calculator.html` diretamente no browser — não é necessário servidor.

```bash
# Clonar o repositório
git clone https://github.com/[teu-utilizador]/vdot-calculator.git

# Abrir no browser
open vdot-calculator.html       # macOS
start vdot-calculator.html      # Windows
xdg-open vdot-calculator.html  # Linux
```

Ou serve localmente com qualquer servidor estático:

```bash
# Python
python -m http.server 8080

# Node.js (npx)
npx serve .
```

---

## Como Usar

1. **Seleciona a distância** da prova de referência no dropdown (10 km, 15 km, Meia-Maratona ou Maratona)
2. **Introduz o tempo** realizado no formato `HH:MM:SS` — a validação ocorre em tempo real
3. Clica em **Calcular VDOT** — o dashboard aparece imediatamente
4. Navega pelas três abas para consultar tempos equivalentes, ritmos de treino e ajustes avançados
5. Na Aba 3, ativa os modificadores desejados e clica em **Recalcular com Ajustes**
6. Na Aba 2, clica em **Exportar PDF** para guardar os ritmos de treino

### Limites de Tempo Válidos

| Distância | Tempo Mínimo | Tempo Máximo | VDOT |
|-----------|-------------|-------------|------|
| 10 km | 0:26:00 | 1:30:00 | 30–85 |
| 15 km | 0:41:00 | 2:15:00 | 30–85 |
| Meia-Maratona | 1:03:00 | 3:30:00 | 30–85 |
| Maratona | 2:10:00 | 7:00:00 | 30–85 |

---

## Motor de Cálculo

### Fórmula de Daniels-Gilbert

O VDOT é calculado através da fórmula original publicada por Daniels & Gilbert (1979):

```
VO2 = (-4,60 + 0,182258 × S + 0,000104 × S²)
      ÷ (0,8 + 0,1894393 × e^(-0,012778 × T) + 0,2989558 × e^(-0,1932605 × T))
```

Onde `S` = velocidade em m/s e `T` = tempo em segundos.

A inversão da fórmula (VDOT → tempo previsto por distância) é realizada por **busca binária** com tolerância de ±0,1 segundos.

### Valores de Referência (Golden Tests)

| Input | Tempo | VDOT Esperado | Tolerância |
|-------|-------|--------------|------------|
| 10 km | 40:00 | 52,4 | ±0,2 |
| 10 km | 35:00 | 59,6 | ±0,2 |
| Meia-Maratona | 1:30:00 | 52,1 | ±0,2 |
| Maratona | 3:30:00 | 46,9 | ±0,2 |
| Maratona | 4:00:00 | 41,4 | ±0,2 |
| 15 km | 1:00:00 | 52,6 | ±0,2 |

---

## Zonas de Treino

| Zona | Nome | Intensidade VDOT | FC Máx. | Objetivo |
|------|------|-----------------|---------|----------|
| E | Easy / Fácil | 59%–74% | 65%–79% | Base aeróbica, recuperação |
| M | Marathon Pace | 75%–84% | 80%–90% | Adaptação ao ritmo de maratona |
| T | Threshold / Limiar | 83%–88% | 88%–92% | Clearance de lactato |
| I | Intervalos | 95%–100% | 98%–100% | Maximização do VO2Max |
| R | Repetições | >100% | N/A | Velocidade pura, economia de corrida |

---

## Ajustes Avançados

### Temperatura e Humidade

| Condição | Fórmula | Exemplo |
|----------|---------|---------|
| Temperatura > 15,5°C | +0,4% de pace por °C acima de 15,5°C | 25°C → +3,8% |
| Humidade > 60% | +0,2% de pace por cada 1% acima de 60% | 80% HR → +4,0% |
| Combinação | Soma aditiva (cap: +20%) | 25°C + 80% HR → +7,8% |

### Altitude

| Altitude | Ajuste (seg/km) |
|----------|----------------|
| ≤ 1.219 m | Sem ajuste |
| 1.219–1.524 m | +2,5 a +3,1 seg/km |
| 1.524–1.829 m | +5,0 a +6,2 seg/km |
| 1.829–2.134 m | +7,5 a +9,3 seg/km |
| 2.134–2.438 m | +9,9 a +12,4 seg/km |
| ≥ 2.438 m | +12,4 a +15,5 seg/km |

> A Zona R **nunca** é ajustada para altitude — a curta duração não depende primariamente do transporte de oxigénio.

### Inclinação (GAP)

| Terreno | Ajuste |
|---------|--------|
| Subida | +18 a +24 seg por cada 10 m de ganho/km |
| Descida | −8 a −12 seg por cada 10 m de perda/km |
| Plano | Sem ajuste |

---

## Exportação PDF

O PDF é gerado inteiramente no browser via [jsPDF](https://github.com/parallax/jsPDF) (carregado sob demanda). Nenhum dado é enviado para qualquer servidor.

**Conteúdo do PDF:**
- Cabeçalho com score VDOT e dados de referência
- Tabela completa das 5 zonas de treino com paces e passagens de pista
- Rodapé com aviso legal

**Nome do ficheiro:** `vdot-[score]-[distância]-[data].pdf`
Exemplo: `vdot-52.4-10km-2026-03-27.pdf`

---

## Estrutura do Projeto

```
vdot-calculator/
├── vdot-calculator.html   # Aplicação completa (ficheiro único)
└── README.md              # Este ficheiro
```

A aplicação é intencionalmente um **ficheiro único** para a v1.0 — sem dependências, sem build step, máxima portabilidade. Basta abrir no browser.

---

## Roadmap

### v1.1 — Pós-lançamento
- [ ] Partilha de resultados via URL com querystring (sem servidor)
- [ ] Modo escuro (dark mode)
- [ ] Animações de transição entre estados
- [ ] Analytics via Plausible (privacy-first, sem cookies)

### v2.0 — App Mobile (React Native / Expo)
- [ ] Migração da lógica de cálculo para pacote `@vdot/core`
- [ ] Interface mobile nativa
- [ ] Integração com HealthKit e Google Fit
- [ ] Histórico de VDOT ao longo do tempo

### v3.0 — Planos de Treino
- [ ] Geração de plano semanal baseado no VDOT e objetivo de prova
- [ ] Exportação para Garmin Connect, Strava e Polar Flow
- [ ] Integração com Google Calendar e Apple Calendar

---

## Referências

- Daniels, J. (2014). *Daniels' Running Formula* (3.ª ed.). Human Kinetics.
- Daniels, J., & Gilbert, J. (1979). *Oxygen Power: Performance Tables for Distance Runners.*
- [VDOT O2 Official Calculator](https://vdoto2.com) — ferramenta de referência oficial
- [Jack Daniels Running Calculator](https://runsmartproject.com/calculator/) — implementação de referência
- [WCAG 2.1 Guidelines](https://www.w3.org/TR/WCAG21/) — requisitos de acessibilidade

---

## Aviso Legal

> **VDOT é uma marca registada da The Run SMART Project, LLC.** Esta calculadora é uma ferramenta educacional independente, criada utilizando fórmulas publicamente disponíveis e inspirada no trabalho do Dr. Jack Daniels. Esta página (ou aplicação) não é endossada nem afiliada à The Run SMART Project ou ao Dr. Jack Daniels.

Os ritmos e tempos apresentados são estimativas baseadas no sistema VDOT de Daniels & Gilbert (1979). São valores indicativos — consulte um treinador qualificado antes de alterar o teu plano de treino.

---

*Calculadora VDOT v1.0 · Português Europeu · Março 2026*
