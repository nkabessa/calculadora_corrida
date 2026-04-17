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
- [Demo e Instalação](#demo-e-instalação)
- [Como Usar](#como-usar)
- [Motor de Cálculo](#motor-de-cálculo)
- [Zonas de Treino](#zonas-de-treino)
- [Exportação PDF](#exportação-pdf)
- [Integração no WordPress](#integração-no-wordpress)
- [Estrutura do Projeto](#estrutura-do-projeto)
- [Roadmap](#roadmap)
- [Aviso Legal](#aviso-legal)

---

## Visão Geral

A **Calculadora VDOT** é uma aplicação web estática de página única (SPA) que implementa fielmente o Sistema VDOT desenvolvido pelo Dr. Jack Daniels e pelo matemático Jimmy Gilbert.

Toda a lógica de cálculo é executada **100% no browser do utilizador** — não existe servidor de aplicação, base de dados, cookies de rastreamento ou envio de dados para servidores externos.

### Princípios de Design

- **Zero dependências de servidor** — lógica totalmente client-side, sem npm, sem build step
- **Ficheiro único** — toda a aplicação num único `.html`, máxima portabilidade
- **Privacidade por defeito** — nenhum dado é transmitido ou armazenado
- **Métricas SI** — quilómetros, metros, graus Celsius
- **Idioma fixo** — Português Europeu (pt-PT)

---

## Funcionalidades

### Cálculo Principal
- Introdução de resultado de prova (distância + tempo) com validação em tempo real
- Cálculo do **score VDOT** via fórmula original de Daniels-Gilbert (velocidade em m/min, tempo em minutos)
- Classificação automática do nível do corredor (Iniciante → Elite Nacional)

### Aba 1 — Tempos Equivalentes
- Previsão de tempos para **10 km, 15 km, Meia-Maratona e Maratona**
- Pace médio por distância em min:ss/km
- Destaque visual da distância de referência ("Âncora")

### Aba 2 — Ritmos de Treino
- **5 zonas de treino** (E, M, T, I, R) com paces calibrados contra as tabelas publicadas por Daniels
- Zona E apresenta intervalo de pace (lento–rápido); M, T, I, R apresentam pace único
- Passagens de pista (200m, 400m, 800m, 1km, 1,2km) para zona I
- Passagens de pista (200m, 400m) para zona R
- Botão ⓘ por zona com descrição fisiológica detalhada

### Score VDOT
- Faixa compacta no fundo dos resultados com score, referência, nível e botão ⓘ informativo

### Exportação PDF
- Geração de página de impressão sem dependências externas — abre nova janela pronta para "Guardar como PDF"
- Inclui ritmos de treino, tempos equivalentes e aviso legal

---

## Demo e Instalação

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
python -m http.server 8080   # Python
npx serve .                  # Node.js
```

---

## Como Usar

1. **Seleciona a distância** da prova de referência (10 km, 15 km, Meia-Maratona ou Maratona)
2. **Introduz o tempo** realizado no formato `HH:MM:SS` — a validação ocorre em tempo real
3. Clica em **Calcular VDOT** — os resultados aparecem imediatamente
4. Navega pelas duas abas: **Tempos Equivalentes** e **Ritmos de Treino**
5. Clica em **Exportar PDF** na Aba 2 para guardar os ritmos (abre janela de impressão)

### Limites de Tempo Válidos

| Distância | Tempo Mínimo | Tempo Máximo |
|-----------|-------------|-------------|
| 10 km | 0:26:00 | 1:30:00 |
| 15 km | 0:41:00 | 2:15:00 |
| Meia-Maratona | 1:03:00 | 3:30:00 |
| Maratona | 2:10:00 | 7:00:00 |

---

## Motor de Cálculo

### Fórmula de Daniels-Gilbert

O VDOT é calculado usando a fórmula original de Daniels & Gilbert (1979), com velocidade em **metros/minuto** e tempo em **minutos**:

```
VO2(V, T) = (-4,60 + 0,182258 × V + 0,000104 × V²)
            ÷ (0,8 + 0,1894393 × e^(-0,012778 × T) + 0,2989558 × e^(-0,1932605 × T))
```

Os ritmos de treino são derivados pela aproximação de steady-state (T→∞, denominador→0,8) com fracções de intensidade calibradas empiricamente contra as tabelas publicadas por Daniels. A inversão VDOT → tempo previsto é feita por busca binária com tolerância de ±0,05 segundos.

### Valores de Referência

| Input | Tempo | VDOT calculado | Referência vdoto2.com |
|-------|-------|---------------|----------------------|
| 10 km | 45:00 | 45,3 | 45,3 ✓ |
| 10 km | 40:00 | 51,9 | 52,4 (~±0,5) |
| Meia-Maratona | 1:30:00 | 51,0 | 52,1 (~±1,1) |
| Maratona | 3:30:00 | 44,6 | 46,9 (~±2,3) |

> As pequenas diferenças para Meia e Maratona são esperadas: a fórmula contínua diverge ligeiramente das tabelas de lookup usadas pelo site de referência para provas mais longas.

---

## Zonas de Treino

| Zona | Nome | Fracção calibrada | Objetivo |
|------|------|-------------------|----------|
| E | Easy / Fácil | 0,775–0,872 do VDOT | Base aeróbica, recuperação |
| M | Marathon Pace | 1,015 do VDOT | Adaptação ao ritmo de maratona |
| T | Threshold / Limiar | 1,097 do VDOT | Clearance de lactato |
| I | Intervalos | 1,215 do VDOT | Maximização do VO₂Max |
| R | Repetições | 1,310 do VDOT | Velocidade pura, economia de corrida |

As fracções foram derivadas por calibração contra as tabelas publicadas em *Daniels' Running Formula* (2014) e validadas contra o calculador oficial vdoto2.com.

---

## Exportação PDF

Ao clicar em **Exportar PDF dos Ritmos**, a calculadora gera uma página HTML formatada numa nova janela e abre automaticamente o diálogo de impressão do browser. Para guardar em PDF, selecciona "Guardar como PDF" na lista de impressoras.

A exportação não usa qualquer biblioteca externa — funciona sem ligação à internet e sem enviar dados para servidores.

---

## Integração no WordPress

A forma mais simples de mostrar a calculadora no teu WordPress é através de um `<iframe>` a apontar para a URL do GitHub Pages. Não precisas de instalar plugins especiais nem de modificar o WordPress.

### Passo 1 — Publicar no GitHub Pages

Se ainda não tens o GitHub Pages activo:

1. Faz upload de `vdot-calculator.html` para um repositório GitHub
2. No repositório, vai a **Settings → Pages**
3. Em *Source*, selecciona **Deploy from a branch** → `main` → `/ (root)` → **Save**
4. Aguarda 1–2 minutos
5. A calculadora fica disponível em:
   ```
   https://[teu-utilizador].github.io/[nome-do-repositório]/vdot-calculator.html
   ```

Testa a URL no browser antes de continuar.

### Passo 2 — Criar a página no WordPress

1. No painel WordPress, vai a **Páginas → Adicionar nova**
2. No editor de blocos (Gutenberg), adiciona um bloco **HTML Personalizado**
3. Cola o código seguinte, substituindo a URL pela tua:

```html
<iframe
  src="https://[teu-utilizador].github.io/[repo]/vdot-calculator.html"
  width="100%"
  height="900"
  style="border: none; border-radius: 12px; display: block;"
  title="Calculadora VDOT — Jack Daniels Running Formula"
  loading="lazy">
</iframe>
```

4. Publica a página e verifica o resultado no frontend.

### Passo 3 — Redimensionamento automático (opcional)

A altura fixa de `900px` funciona bem em desktop. Para que a altura se ajuste automaticamente ao conteúdo em todos os ecrãs, adiciona estes dois snippets.

**No `vdot-calculator.html`**, antes de `</body>`:

```html
<script>
function notifyHeight() {
  window.parent.postMessage({ vdotHeight: document.body.scrollHeight }, '*');
}
new ResizeObserver(notifyHeight).observe(document.body);
</script>
```

**No WordPress**, a seguir ao `<iframe>` no mesmo bloco HTML Personalizado:

```html
<script>
(function() {
  var iframe = document.querySelector(
    'iframe[title="Calculadora VDOT — Jack Daniels Running Formula"]'
  );
  if (!iframe) return;
  window.addEventListener('message', function(e) {
    if (e.data && e.data.vdotHeight) {
      iframe.style.height = e.data.vdotHeight + 'px';
    }
  });
})();
</script>
```

### Passo 4 — Alternativa via Shortcode

Se preferires uma solução reutilizável, adiciona este código ao `functions.php` do teu tema filho (ou num plugin de snippets como o *Code Snippets*):

```php
function vdot_calculator_shortcode( $atts ) {
    $atts = shortcode_atts( array(
        'height' => '900',
        'url'    => 'https://[teu-utilizador].github.io/[repo]/vdot-calculator.html',
    ), $atts );

    return '<iframe src="' . esc_url( $atts['url'] ) . '"
        width="100%"
        height="' . intval( $atts['height'] ) . '"
        style="border:none;border-radius:12px;display:block;"
        title="Calculadora VDOT — Jack Daniels Running Formula"
        loading="lazy"></iframe>';
}
add_shortcode( 'vdot_calculator', 'vdot_calculator_shortcode' );
```

Depois usa em qualquer página ou post:

```
[vdot_calculator]
[vdot_calculator height="1100"]
```

### Resolução de Problemas

**O iframe aparece em branco ou não carrega**
O teu tema ou plugin de segurança pode estar a bloquear iframes externos. Verifica se tens plugins como *WP Cerber*, *iThemes Security* ou *Wordfence* com CSP activa e adiciona `github.io` à lista de domínios permitidos.

**A calculadora fica cortada em mobile**
Aumenta o valor de `height`, usa o snippet de redimensionamento automático (Passo 3), ou adiciona CSS em **Aparência → Personalizar → CSS Adicional**:

```css
iframe[title="Calculadora VDOT — Jack Daniels Running Formula"] {
  min-height: 800px;
}

@media (max-width: 600px) {
  iframe[title="Calculadora VDOT — Jack Daniels Running Formula"] {
    min-height: 1100px;
  }
}
```

**O WordPress remove o HTML do bloco**
Alguns temas e plugins filtram HTML personalizado. Instala o plugin gratuito [**Raw HTML**](https://wordpress.org/plugins/raw-html/) para inserir o iframe sem filtros, ou usa o shortcode (Passo 4).

---

## Estrutura do Projeto

```
vdot-calculator/
├── vdot-calculator.html   # Aplicação completa (ficheiro único, ~46 KB)
└── README.md              # Este ficheiro
```

---

## Roadmap

### v1.1
- [ ] Partilha de resultados via URL com querystring (sem servidor)
- [ ] Modo escuro (dark mode)
- [ ] Ajustes avançados: temperatura/humidade, altitude, inclinação (GAP)
- [ ] Analytics via Plausible (privacy-first, sem cookies)

### v2.0 — App Mobile (React Native / Expo)
- [ ] Migração da lógica de cálculo para pacote `@vdot/core`
- [ ] Interface mobile nativa com Expo
- [ ] Integração com HealthKit (iOS) e Google Fit (Android)
- [ ] Histórico de VDOT ao longo do tempo

### v3.0 — Planos de Treino
- [ ] Geração de plano semanal baseado no VDOT e objetivo de prova
- [ ] Exportação para Garmin Connect, Strava e Polar Flow
- [ ] Integração com Google Calendar e Apple Calendar

---

## Referências

- Daniels, J. (2014). *Daniels' Running Formula* (3.ª ed.). Human Kinetics.
- Daniels, J., & Gilbert, J. (1979). *Oxygen Power: Performance Tables for Distance Runners.*
- [VDOT O2 Official Calculator](https://vdoto2.com) — calculador de referência oficial
- [Jack Daniels Running Calculator](https://runsmartproject.com/calculator/) — implementação de referência

---

## Aviso Legal

> **VDOT é uma marca registada da The Run SMART Project, LLC.** Esta calculadora é uma ferramenta educacional independente, criada utilizando fórmulas publicamente disponíveis e inspirada no trabalho do Dr. Jack Daniels. Esta página (ou aplicação) não é endossada nem afiliada à The Run SMART Project ou ao Dr. Jack Daniels.

Os ritmos e tempos apresentados são estimativas baseadas no sistema VDOT de Daniels & Gilbert (1979). São valores indicativos — consulte um treinador qualificado antes de alterar o teu plano de treino.

---

*Calculadora VDOT v1.0 · Português Europeu · Abril 2026*
