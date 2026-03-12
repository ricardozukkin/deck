# Zukkin Deck v3.1 — Guia de Referência para Claude

> **Propósito:** Este documento é o guia definitivo para o Claude gerar apresentações HTML da Zukkin, sempre alinhadas à identidade visual do sistema Deck v3.1.  
> **Showcase de referência:** https://zukkin-br.github.io/deck/showcase/showcase-themes.html  
> **Repositório:** github.com/zukkin-br/deck

---

## 1. Estrutura Base Obrigatória

Todo arquivo HTML de apresentação deve seguir esta estrutura:

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>[TÍTULO DA APRESENTAÇÃO]</title>
  <meta name="robots" content="noindex, nofollow">
  <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@300;400;500;600;700;800;900&family=Plus+Jakarta+Sans:wght@700;800&display=swap" rel="stylesheet">
  <style>
    /* CSS do sistema (ver seção 3) */
  </style>
</head>
<body>

<!-- BLOCO DE EXPIRAÇÃO (opcional) -->
<div class="zk-expiration" id="zkExpiration"></div>
<div class="zk-expired-block" id="zkExpiredBlock">
  <div class="zk-expired-icon">🔒</div>
  <h2>Apresentação expirada</h2>
  <p>Esta apresentação atingiu sua data de validade.</p>
  <div class="zk-expired-date" id="zkExpiredDate"></div>
</div>

<!-- COCKPIT (estrutura de navegação) -->
<div class="ck-viewport">
  <button class="ck-arrow ck-arrow-prev" id="ckArrowPrev">&#8249;</button>
  <div class="ck-frame">
    <div class="zk-deck" id="zkDeck">

      <!-- SLIDE 1: Cover (OBRIGATÓRIO — sempre primeiro) -->
      <div class="zk-slide cover active zk-animate">
        <div class="zk-logo zk-animate"></div>  <!-- logo auto-injetado pelo JS -->
        <h1 class="zk-animate">Título Principal</h1>
        <h2 class="zk-animate">Subtítulo</h2>
        <div class="zk-meta zk-animate">Nome Empresa · Mês Ano</div>
      </div>

      <!-- DEMAIS SLIDES -->

    </div><!-- /zk-deck -->
  </div><!-- /ck-frame -->
  <button class="ck-arrow ck-arrow-next" id="ckArrowNext">&#8250;</button>
</div><!-- /ck-viewport -->

<!-- BARRA INFERIOR -->
<div class="ck-bar">
  <div class="ck-bar-left">
    <button id="ckBtnPrev">‹</button>
    <span id="ckCurrent">1</span> / <span id="ckTotal">1</span>
    <button id="ckBtnNext">›</button>
  </div>
  <div class="ck-progress-wrap"><div class="ck-progress" id="ckProgress"></div></div>
  <div class="ck-bar-right">
    <span id="zkThemeLabel">TEMA: Pricing</span>
    <div id="zkThemeSwitcher"></div>
  </div>
</div>

<script>
  const METADATA = {
    theme: null,           // null = azul_aberto (padrão). Outros: 'light','warm','midnight','sand','silver','analytics','zgo','consulting','promo','robot','vermelho'
    title: "[TÍTULO]",
    expires_at: null       // ou "2026-06-30" para expiração
  };
  // ... (cockpit JS + brand JS — ver seção 5)
</script>
</body>
</html>
```

---

## 2. Temas Disponíveis

| ID | Nome | Cor Principal | Uso Recomendado |
|----|------|---------------|-----------------|
| `azul_aberto` (padrão) | Pricing | `#1C2F48` | ZPricing, uso geral Zukkin |
| `light` | Claro | `#F8FAFC` | Apresentações formais/corporativas |
| `warm` | Warm | `#1A1215` | ZAnalytics, destaque |
| `midnight` | Midnight | `#0A0E1A` | Tech, inovação |
| `sand` | Sand | `#F0EDE8` | Consultoria, leveza |
| `silver` | Silver | `#DFE3E8` | Corporativo neutro |
| `analytics` | Analytics | `#EE8625` | ZAnalytics |
| `zgo` | ZGo | `#78AE3F` | ZGO |
| `consulting` | Consulting | `#8C9BAB` | ZConsulting |
| `promo` | Promo | `#FF4D6A` | ZPromo |
| `robot` | Robot | `#6B2246` | ZRobot |
| `vermelho` | Vermelho | `#CF110D` | Destaques, urgência |

**Para ativar um tema no HTML:** definir `METADATA.theme = 'analytics'` e `document.documentElement.setAttribute('data-theme', 'analytics')`

---

## 3. Variáveis CSS Obrigatórias (`:root`)

```css
:root {
  /* Tema padrão: Azul Aberto */
  --bg: #1C2F48;
  --bg-outer: #0D1B2A;
  --surface: #253A56;
  --surface-hover: #2D4462;
  --border: rgba(2, 56, 90, 0.28);
  --border-accent: rgba(2, 56, 90, 0.4);
  --text: #EEF4FA;
  --text-secondary: rgba(180, 210, 245, 0.6);
  --text-muted: rgba(180, 210, 245, 0.35);

  /* Cores da marca */
  --vermelho: #CF110D;
  --azul: #02385A;
  --branco: #FFFFFF;
  --cinza: #71869D;

  /* Cores por produto */
  --pricing: #02385A;
  --analytics: #EE8625;
  --promo: #FF4D6A;
  --robot: #6B2246;
  --go: #78AE3F;
  --consulting: #8C9BAB;

  /* Gradientes aprovados */
  --gradient-vibrant: linear-gradient(135deg, #7C6EF6, #00D4FF, #FF6B9D);
  --gradient-warm: linear-gradient(135deg, #EE8625, #FF4D6A);

  /* Semânticas */
  --positive: #10B981;
  --negative: #EF4444;
  --warning: #F59E0B;

  /* Layout */
  --slide-padding: 48px;
  --radius: 16px;
  --font: 'Montserrat', -apple-system, sans-serif;

  /* Safe Zones — NUNCA violar */
  --zk-safe-top: 72px;
  --zk-safe-bottom: 40px;
  --zk-safe-side: var(--slide-padding);
  --zk-title-gap: 32px;
}
```

---

## 4. Tipos de Slide Disponíveis

### Classes do `div.zk-slide`:

| Classe | Tipo | Alinhamento | Uso |
|--------|------|-------------|-----|
| `cover` | Capa principal | Centralizado | Sempre o primeiro slide |
| `cover-info` | Capa com metadados | Centralizado | Capa alternativa com badges |
| `content` | Conteúdo geral | Topo | Texto + elemento visual |
| `two-col` | Duas colunas | Topo | Comparativos, pilares |
| `kpi-grid` | Grid de KPIs | Topo | Métricas, resultados |
| `timeline` | Linha do tempo | Topo | Roadmap, histórico |
| `section-divider` | Divisor de seção | Centralizado | Transição entre blocos |
| `feature-list` | Lista de features | Topo | Funcionalidades do produto |
| `quote` | Citação | Centralizado | Depoimentos |
| `big-number` | Número grande | Centralizado | Destaque de métrica |
| `comparison` | Antes × Depois | Topo | Comparativos |
| `image-text` | Imagem + Texto | Split | Produto com screenshot |
| `checklist` | Checklist | Topo | Requisitos, critérios |
| `photo-gallery` | Galeria | Topo | Cases, cases visuais |
| `screenshot` | Screenshot | Topo | Demo do produto |
| `stats-row` | Linha de stats | Centralizado | Impacto em números |
| `process-steps` | Passos | Centralizado | Como funciona |
| `team-grid` | Time | Topo | Equipe |
| `dashboard` | Dashboard | Topo | Dados complexos |
| `analytics` | Analytics | Topo | Métricas de produto |
| `adoption-map` | Mapa de adoção | Topo | Status de ferramentas |
| `opportunities` | Oportunidades | Topo | Potencial de ganho |
| `workshop` | Workshop | Topo | Interativo |
| `closing` | Encerramento | Centralizado | Último slide |

### Backgrounds opcionais (`data-bg` no `.zk-slide`):
- `node_network` — rede de nós
- `scatter_plot` — pontos dispersos
- `concentric_rings` — anéis concêntricos
- `triangular_mesh` — malha triangular

---

## 5. JavaScript do Cockpit (copiar em toda apresentação)

O JS completo deve incluir:
1. `THEME_COCKPIT` — mapeamento de cores por tema
2. `ckApplyTheme()` — aplica cor ao cockpit
3. `ckNext()` / `ckPrev()` — navegação
4. `ckUpdateSlide()` — atualiza slide ativo + animações
5. `ckInit()` — inicialização
6. `ckCheckExpiration()` — controle de expiração
7. Logos SVG: `ZK_LOGO_RED` e `ZK_LOGO_WHITE`
8. `THEMES` array — lista de temas
9. Auto-inject logos nas `.zk-logo`
10. Brand Validator — garante cover + logos
11. `initThemeSwitcher()` — dots de seleção de tema
12. Safe Zone Enforcer — aplica espaçamentos mínimos

> **Importante:** O JS completo está no showcase de referência. Sempre copiar o bloco `<script>` completo do showcase para novas apresentações.

---

## 6. Regras de Design

### Typography
- **H1 (cover):** Plus Jakarta Sans, 800, clamp(36px, 6vw, 72px)
- **H2 (slides):** Montserrat, 800, clamp(24px, 3vw, 36px), margin-bottom: var(--zk-title-gap)
- **Body:** Montserrat, 400/500, clamp(14px, 1.5vw, 18px)
- **Labels/caps:** Montserrat, 600, 11px, uppercase, letter-spacing: 0.08em

### Componentes principais

```html
<!-- KPI card -->
<div class="zk-kpi">
  <div class="label">MÉTRICA</div>
  <div class="value">94%</div>
  <div class="trend up">↑ 12%</div>
</div>

<!-- Timeline item -->
<div class="zk-timeline-item">
  <div class="phase">Q1</div>
  <strong>Título</strong>
  <div class="desc">Descrição</div>
</div>

<!-- Surface card -->
<div style="background:var(--surface); border:1px solid var(--border); border-radius:var(--radius); padding:24px;">
  Conteúdo
</div>

<!-- Accent bar (topo do card) -->
<div style="border-top: 3px solid var(--vermelho);">

<!-- Badge de meta -->
<div class="zk-cover-meta-badge">
  <span>📅</span><span>Texto</span>
</div>
```

### Safe Zones — NUNCA violar
- Slides de conteúdo: padding-top mínimo de `72px`
- Todos os slides: padding-bottom mínimo de `40px`
- Laterais: `48px`
- Espaço abaixo de H2: `32px`
- Slides cover/closing/section-divider: padding centralizado com `var(--zk-safe-bottom)`

---

## 7. Publicação no GitHub

### Repositório: `zukkin-br/deck`

Estrutura de pastas sugerida:
```
deck/
├── showcase/
│   └── showcase-themes.html        ← criado pelo Bruno, referência
├── propostas/
│   ├── [cliente]-[produto]-[data].html
│   └── ...
├── eventos/
│   ├── abras-2026.html
│   └── ...
└── README.md
```

### Fluxo para publicar nova apresentação:
1. Claude gera o arquivo `.html` completo
2. Ricardo faz upload para o repositório `zukkin-br/deck` via GitHub.com (interface web) ou GitHub Desktop
3. A apresentação fica disponível em: `https://zukkin-br.github.io/deck/[pasta]/[arquivo].html`

### Nomeação de arquivos:
- Propostas: `proposta-[cliente]-[produto]-[aaaamm].html`  
  Ex: `proposta-stmarche-zpricing-202603.html`
- Eventos: `[evento]-[ano].html`  
  Ex: `abras-2026.html`
- Internos: `[assunto]-[aaaamm].html`

---

## 8. Checklist para o Claude ao gerar apresentações

Antes de entregar qualquer apresentação, verificar:

- [ ] Fonte Montserrat + Plus Jakarta Sans carregada via Google Fonts
- [ ] `METADATA` definido com `theme`, `title` e opcionalmente `expires_at`
- [ ] Primeiro slide é `cover` com `.zk-logo`
- [ ] Último slide é `closing`
- [ ] Safe zones respeitadas (top 72px, bottom 40px, laterais 48px)
- [ ] H2 de cada slide tem `margin-bottom: var(--zk-title-gap)`
- [ ] Cockpit JS completo copiado do showcase
- [ ] Cores usando variáveis CSS (não hardcoded)
- [ ] Tema escolhido adequado ao produto/contexto
- [ ] Logo auto-injetado (não precisa SVG manual — o JS injeta)
- [ ] Navegação teclado (←→) e touch funcionando

---

## 9. Produtos e Temas Recomendados

| Produto | Tema Recomendado | Cor Accent |
|---------|------------------|------------|
| ZPricing | `azul_aberto` | `#02385A` |
| ZAnalytics | `analytics` | `#EE8625` |
| ZPromo | `promo` | `#FF4D6A` |
| ZRobot | `robot` | `#6B2246` |
| ZGO | `zgo` | `#78AE3F` |
| ZConsulting | `consulting` | `#8C9BAB` |
| Institucional Zukkin | `azul_aberto` | `#CF110D` |
| Evento/ABRAS | `midnight` ou `azul_aberto` | variável |

---

*Guia gerado em março 2026 · Zukkin Deck v3.1*
