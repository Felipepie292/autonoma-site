# LP_AUTONOMAAI — CLAUDE.md

## Sobre o Projeto

Landing page para **Autonoma** — agência de automação e IA que fornece "agentes de IA" para empresas.
Projeto é **HTML/CSS/JS puro** (arquivo único `index.html`), servido via `npx serve`.

---

## Stack

- **HTML estático** — arquivo único `index.html` (~5500 linhas)
- **Tailwind CSS** via CDN (classes utilitárias inline)
- **Vanilla JavaScript** para animações e interatividade
- **CSS `@keyframes`** para efeitos decorativos
- **IntersectionObserver** para scroll-triggered animations
- **SVG inline** para ícones
- **Sem frameworks JS** — tudo é HTML/CSS/JS puro

---

## Design System

**Consultar o arquivo `design-system.html` na raiz do projeto.** Ele contém a referência completa de paleta, tipografia, componentes, motion, ícones e layout. Sempre usar esse arquivo como fonte da verdade para decisões visuais.

Resumo rápido:
- **Background:** #050505 / #080808 / #0A0A0A
- **Accent:** #F97316 (laranja)
- **Fonte:** Inter (sans), monospace para labels
- **Estética:** brutalist — cantos retos, sem rounded, corner accents, beam borders

---

## Seções do Site (ordem real no HTML)

### VISÍVEIS:
1. **Nav** — logo oficial AUTONOMA_ (Inter 500, cursor laranja piscando), links, CTA "Ver funcionando". **Nav fixa** (`fixed top-0 left-0 right-0 z-50 bg-[#050505]/80 backdrop-blur-md`).
2. **Hero** — grid 12 colunas: col-5 texto + CTA laranja, col-5 cena isométrica animada (`.hi-wrap`), col-2 sidebar com 3 stats reais (+35 empresas / 47k leads / R$2.1M). `mt-28 lg:mt-32` para compensar nav fixa.
3. **Manifesto** (sem section ID) — letra scramble animada
4. **Metodologia** (id="metodologia-section") — sticky scroll, 3 steps
5. **Pilares** (id="pilares-section") — 4 cards de serviço
6. **Casos** (id="casos-section") — sticky horizontal scroll com 5 case cards
7. **WhatsApp Demo** (id="whatsapp-demo-section") — iPhone mockup + FAQ. **Visível.**
8. **Expert** (sem ID) — seção fullscreen `min-height:100vh`, `assets/foto_expert.png` como fundo (`object-fit:cover`), overlay `rgba(0,0,0,0.45)`. Texto grande (Inter 500, uppercase, `clamp(2.5rem,5vw,5rem)`) topo-esquerdo com `// ` laranja; subtexto topo-direito.
9. **Footer / Contato** (id="contato-section") — fundo `#ffffff`, grid 12 col. Formulário **substituído por Typebot embed** (ver seção abaixo).

### REMOVIDAS DO DOM (não recriar):
- **Diagnóstico** (`bg-[#080808]`) — seção "Seu negócio gera dados o tempo todo" removida completamente.
- **sys-scroll-outer** — seção "Seus funcionários de IA" (lixo do template). Removida do DOM em runtime por script inline. Não reativar.
- **cases-section** (id="cases-section") — seção antiga que continha mc-card e rc-card. Cards migrados para #cc-grid. Seção está `class="hidden"`, não usar.
- **compare-section** (id="compare-section") — comparativo operacional, `display:none`

---

## SEÇÃO DE CASE CARDS — ESTRUTURA ATUAL

Existem **5 case cards interativos**, todos dentro de `#cc-grid` na `casos-section`. Todos usam a mesma arquitetura `cc-wrap`.

### Localização:
- **casos-section** (id="casos-section", VISÍVEL): todos os 5 cards estão aqui dentro de `#cc-grid`

### Anatomia universal do card (todos os 5 seguem este padrão):
```html
<div class="cc-wrap min-w-0" id="[id-do-card]" onclick="[fn]Expand(event)">
  <div class="cc-ca cc-ca-tl"></div>  <!-- corner accent top-left -->
  <div class="cc-ca cc-ca-br"></div>  <!-- corner accent bottom-right -->
  <div class="cc-collapsed">
    <img src="assets/[foto].png" class="cc-photo" alt="" aria-hidden="true">
    <div class="cc-photo-gradient"></div>
    <div class="cc-photo-footer">
      <div class="cc-tag">// Segmento · Tipo</div>
      <h3 class="cc-title">Nome do agente</h3>
    </div>
    <div class="cc-specs-overlay">  <!-- aparece no hover -->
      <div class="cc-specs-inner">
        <p class="cc-sub">Descrição</p>
        <!-- logo pills opcionais (mc- e rc- têm, cc- não têm) -->
        <div class="cc-metrics">...</div>
        <div class="cc-cta">...</div>
      </div>
    </div>
  </div>
  <div class="cc-expanded">
    <!-- conteúdo interativo expandido (único por card, usa prefixo próprio internamente) -->
  </div>
</div>
```

**Importante:** mc-card e rc-card usam `cc-wrap` + `cc-collapsed` + `cc-expanded` igual aos outros 3. Internamente no expanded, o CSS ainda usa prefixos mc- e rc-.

### Logo pills no hover (cc-specs-overlay) — todos os 5 cards têm:
- **cc-card (Pneus):** WhatsApp, Bling (`logo_bling_ahM4Zg.png`), RD Station (`LOGORD.jpg`), Google Agenda, Dashboards — classe `.cc-logo-pill` com variantes `cc-lp-*`
- **cc-card2 (Social Media):** Notion (`Notion-logo.png`), OpenAI, Instagram (`instagram_logo.avif`), WhatsApp
- **cc-card3 (Imobiliária):** WhatsApp, Trello (`trelo_logo.webp`), Google Agenda, Google Sheets (`logo_sheets.png`), Meta Ads
- **mc-card (Meta Ads):** Meta Ads, WhatsApp, Google Sheets (`logo_sheets.png`) — classe `.mc-cc-logo-pill`
- **rc-card (Recepcionista):** WhatsApp, Clinicorp (`logo_clinicorp.png`), Mercado Pago (`logo_mercadopago.png`) — classe `.rc-cc-logo-pill`

### Fonte da descrição (cc-sub):
- `font-size: 15px` — aumentada de 12px original para melhor legibilidade

### Card 3 (Imobiliária) — layout expanded dois painéis:
- `.c3-main { display:flex; height:380px }` — wrapper flex
- `.activity-feed` — painel esquerdo 42%, chat de mensagens, `border-right` sutil
- `.c3-screen` — painel direito flex:1, `background:#0c0a08`, recebe sys cards via `addSysCard3()`
- `addSysCard3()` **substitui** (não empilha) o conteúdo do painel direito com fade de opacidade
- Reset no `cc3Collapse`: restaura `c3-screen` com placeholder `// aguardando integração...`

### Logos nos expanded bars (rc-card JS strings):
- Clinicorp bar: `<img src="assets/logo_clinicorp.png" style="height:16px;width:auto;">`
- Mercado Pago bar: `<img src="assets/logo_mercadopago.png" style="height:16px;width:auto;">`
- mc-card Google Sheets bar: `<img src="assets/logo_sheets.png" style="height:16px;width:auto;">`

### Prefixos CSS por card (IMPORTANTE — evita conflitos):

| Card | ID | Assets | Expand fn | Collapse fn | Prefixo interno (expanded) |
|------|----|--------|-----------|-------------|---------------------------|
| Pneus SDR | cc-card | card_pneu.png | ccExpand() | ccCollapse() | topbar-, chat-, sys- |
| Social Media | cc-card2 | foto_card_socialmedia.png | cc2Expand() | cc2Collapse() | sm- |
| Imobiliária | cc-card3 | imagem_case card imovel.png | cc3Expand() | cc3Collapse() | c3- |
| Meta Ads | mc-card | imagem_card meta ads.png | mcExpand() | mcCollapse() | mc- |
| Recepcionista | rc-card | imagem_card recepcionista.png | rcExpand() | rcCollapse() | rc- |

### Estado de expand/collapse:
- Classe usada: `cc-open` no card aberto (todos os 5 cards)
- Grid usa `cc-has-open` quando algum card está aberto
- CSS: card aberto → `flex: 0 0 min(calc(100vw - 96px - 2*160px - 3*24px), 760px)`; fechados → `flex: 0 0 160px`

### Coordenador JS (grid management):
- Script envolto em `document.addEventListener('DOMContentLoaded', ...)` (necessário pois mc/rc também usam DOMContentLoaded)
- Gerencia todos os 5 cards: fecha os outros quando um abre, controla `cc-has-open` no grid
- mc/rc scripts também usam `DOMContentLoaded` e definem `window.mcExpand`, `window.rcExpand` etc.

---

## STICKY HORIZONTAL SCROLL — casos-section

A seção de cases usa scroll sticky horizontal: ao rolar a página para baixo, os cards se movem para a lateral até mostrar todos os 5.

### Estrutura HTML:
```html
<div id="cc-scroll-outer" style="position:relative;">          <!-- cria espaço vertical para o scroll -->
  <div id="cc-scroll-sticky" style="position:sticky;top:0;overflow:hidden;..."> <!-- fica fixo -->
    <section id="casos-section" ...>
      <div class="max-w-screen-xl mx-auto px-6 lg:px-12">
        <!-- header -->
        <div id="cc-grid" class="flex flex-row flex-nowrap gap-6 items-start" style="will-change:transform;">
          <!-- 5 cc-wrap cards -->
        </div>
      </div>
    </section>
    <div id="cc-scroll-dots">  <!-- 5 pontos de progresso, class="cc-sdot" -->
  </div><!-- /cc-scroll-sticky -->
</div><!-- /cc-scroll-outer -->
```

### Como funciona o JS (script após cc-scroll-outer):
- `setOuterHeight()` — calcula e define a altura do outer = `100vh + (trackWidth - visibleWidth)`
- `update()` — converte progresso do scroll vertical em `translateX` no cc-grid; pausa quando `cc-has-open`
- Ao expandir card: recalcula altura do outer
- Ao colapsar card: restaura `translateX` salvo (`savedTx`) após 600ms
- Fallback `prefers-reduced-motion`: desativa sticky, usa `overflow-x:auto`

### Largura dos cards:
- Base: `flex: 0 0 clamp(300px, 32vw, 440px)` → ~3 cards visíveis no desktop

---

## HERO ISOMETRIC SCENE (.hi-wrap)

Cena 3D isométrica que ocupa a coluna central do hero (col-span-5). CSS prefixado com `.hi-wrap` para evitar conflitos.

- **Arquivo fonte:** `autonoma-brand-system.html` (não usar diretamente — conteúdo integrado ao index.html)
- **Scale:** `0.95` aplicado via `transform: rotateX(55deg) rotateZ(-45deg) scale(0.95)` na `.hi-wrap .scene`
- **Elementos animados:** monitor (chat), phone (WhatsApp msgs), 2 dashboard cards (conversões + performance), agenda, 6 notificações voando, 8 partículas laranja, glows ambientes, linhas de conexão
- **IDs JS:** `hiMonChat`, `hiPhoneMsgs`, `hiConvVal`, `hiConvBars`, `hiLeadsVal`, `hiCalNewEvent`
- **Script:** IIFE inline logo após o `</main>` do hero (não usa DOMContentLoaded — roda após os elementos existirem no DOM)
- **Keyframes:** todos prefixados com `hi-` (ex: `hi-float1`, `hi-bubbleIn`, `hi-notifFly`)

---

## LOGO OFICIAL (Nav)

Substituída a logo SVG + JetBrains Mono por HTML puro seguindo o brand system (`autonoma-brand-system.html`):
```html
<span style="font-family:'Inter',sans-serif;font-weight:500;text-transform:uppercase;letter-spacing:-0.05em;font-size:1.35rem;color:#fff;white-space:nowrap;line-height:1">
  AUTONOMA<span style="color:#F97316;animation:logo-blink 1.2s step-end infinite">_</span>
</span>
```
Referência: Seção "01 — Wordmark principal · fundo escuro" → variante "Clean + cursor pra navbar".

---

## HERO — BOTÃO CTA

Botão "Agendar Demonstração" com fundo e borda laranja `#F97316`, texto preto. Corner accents brancos no hover. Não tem pattern de pontos no hover (removido junto com o hover-bg da versão anterior).

---

## HERO — SIDEBAR STATS

3 métricas reais da Autonoma (substituíram dados do template Canvas):
- `+35` — Empresas automatizadas
- `47k` — Leads processados/mês
- `R$2.1M` — Economizados em folha

---

## SEÇÃO EXPERT (fullscreen foto)

Entre `whatsapp-demo-section` e o footer. Sem section ID.
- **Foto:** `assets/foto_expert.png`, `object-fit:cover`, sem `transform:scale`
- **Overlay:** `rgba(0,0,0,0.45)`
- **Texto principal (topo-esquerdo):** Inter 500, uppercase, `clamp(2.5rem,5vw,5rem)`, `line-height:1.05`, `letter-spacing:-0.05em`, branco puro `#ffffff`, sem text-shadow. Prefixado com `// ` laranja.
- **Texto secundário (inferior-direito):** Inter 400, `1.1rem`, `rgba(255,255,255,0.75)`, `max-width:500px`, `text-align:right`
- Sem border-radius em nenhum elemento

---

## FOOTER / SEÇÃO DE CONTATO — ESTADO ATUAL

Footer substituído por seção de contato com **Typebot embed** (formulário removido).

### Estrutura:
- **Fundo:** `#ffffff`, `border-top: 1px solid #E5E7EB`
- **Grid:** 12 colunas, `col-span-7` esquerda (`id="ft-left"`) + `col-span-5` direita (`id="ft-right"`)

### Coluna esquerda:
- Tag `// ENTRE EM CONTATO`
- Headline: "Pronto pra ver o que a IA pode fazer pelo seu negócio?"
- Descrição: "Responda algumas perguntas e a gente te retorna com um diagnóstico real do seu negócio."
- **`<typebot-standard style="width:100%;height:600px;">`** — embed inline do Typebot
- Separador `1px #E5E7EB` + copyright `© 2025 AUTONOMA`

### Coluna direita:
- `A_` gigante centralizado, corner accents laranja

### Typebot (script inline no footer):
```js
const typebotInitScript = document.createElement("script");
typebotInitScript.type = "module";
typebotInitScript.innerHTML = `import Typebot from 'https://cdn.jsdelivr.net/npm/@typebot.io/js@0/dist/web.js'
Typebot.initStandard({
  typebot: "meu-typebot-rdqvtm8",
  apiHost: "https://view.autonomaai.tech",
});`;
document.body.append(typebotInitScript);
```
- `initStandard` sem `container` — o Typebot detecta o elemento `<typebot-standard>` automaticamente
- Formulário antigo (Google Apps Script) e scripts de máscara/submit foram removidos

---

## COPYS ATUALIZADAS

### Metodologia — headlines dos steps:
- **Step 1 (IDENTIFICAR):** "Mapeamos onde o tempo some e onde está o dinheiro parado."
- **Step 2 (EDUCAR):** "Sua equipe entende o que delegar. Você decide o que construir."
- **Step 3 (DESENVOLVER):** "Agentes treinados com seus dados, prontos pra operar no dia um."

### Pilares — header da seção:
- **Descrição:** "Cada agente é construído pro seu negócio — não é instalado, é treinado."

### WhatsApp Demo — subtítulo:
- "O problema não é o tamanho do time. É o quanto do time ainda faz coisa que máquina faz melhor."

### Footer / Contato — descrição:
- "Responda algumas perguntas e a gente te retorna com um diagnóstico real do seu negócio."

### Case cards — descrições (cc-sub, 15px):
- **cc-card (Pneus):** "Cliente manda 'quanto custa o pneu 205/55?' e em 8 segundos..."
- **cc-card2 (Social):** "Recebe o briefing do cliente, gera copy e arte via IA..."
- **cc-card3 (Imobiliária):** "Lead chegou do Meta Ads? Em 2 minutos o agente já perguntou..."
- **mc-card (Meta Ads):** "Às 3h da manhã, enquanto o gestor dorme..."
- **rc-card (Recepcionista):** "Paciente manda 'quero marcar uma limpeza' às 23h..."

---

## iPhone Demo + FAQ (whatsapp-demo-section)

Seção com layout split horizontal. **Visível.**
- **Esquerda:** iPhone mockup com conversa WhatsApp animada + cards de sistema com skeleton/typewriter
- **Direita:** FAQ accordion com 9 perguntas

IDs importantes: `wa-iphone-wrap`, `wa-chat-area`, `wa-messages`, `faq-list`, `faq-column`

---

## MOBILE RESPONSIVENESS (375px–430px)

O site é totalmente responsivo. Principais implementações:

### Nav (mobile)
- Hamburger `id="nav-hamburger"` substitui links no mobile (`md:hidden`)
- Menu overlay fullscreen `id="nav-mobile-menu"` com animação de abertura/fechamento
- Botão vira X ao abrir; `body overflow:hidden` enquanto aberto

### Hero (mobile)
- `<main>` usa `lg:min-h-[80vh]` (não `min-h-[80vh]`) — sem altura mínima no mobile, evita espaço vazio abaixo do CTA
- Sidebar de stats (`lg:col-span-2`) está `hidden lg:flex` — oculta no mobile
- Strip de stats abaixo do CTA: `flex lg:hidden` com 3 métricas em linha (`border-t`)
- Cena isométrica (`.hi-wrap`) oculta em mobile, visível a partir de `lg`

### Diagnóstico (mobile)
- Grid `grid-cols-1 lg:grid-cols-12`
- Coluna de código (`lg:col-span-6`) está `hidden lg:block` — oculta no mobile
- Coluna de texto ocupa largura total com `px-6 py-12`

### Metodologia (mobile)
O sticky scroll é mantido no mobile com layout vertical, mas com a barra 01→02→03 na parte **inferior** (não no topo).

**Layout atual (flex column, top → bottom):**
1. `#meth-content` (`order:1; flex:1`) — texto animado (headline + parágrafo de cada step) ocupa o topo
2. `#meth-sidebar` (`order:2`) — barra horizontal 01→02→03 fixada no rodapé com `border-top` sutil

**CSS key:**
- `sticky top: 4rem` — container sticky inicia abaixo da nav fixa (~64px), evitando que a barra fique escondida atrás da nav
- `height: calc(100vh - 4rem)` — altura compensada pelo offset do nav
- Sidebar: `padding: 0.75rem 1.25rem 1rem` (padding bottom, não top)
- `.meth-step { position:absolute; top:0; bottom:0; padding: 2rem 1.5rem; align-items: flex-start }` — preenche toda a área de conteúdo

**JS — progresso one-way com reset:**
- `maxProgress` acumula o progresso máximo atingido (animação não reverte ao subir)
- Reset: `if (rawProgress < 0.01) maxProgress = 0` — quando usuário sobe acima da seção, animação replay do começo
- `cachedInnerH = window.innerHeight` capturado no init — evita jank do iOS (address bar aparece/some alterando `window.innerHeight`)
- Beams animam `width` (horizontal) no mobile e `height` (vertical) no desktop

**`#meth-mobile`** (fallback JS estático) fica `display:none !important` — JS usa os `.meth-step` animados em todos os viewports

### Casos / Cards (mobile) — **Carousel 1 card por tela**
- CSS: `@media (max-width: 1023px)` logo antes de `@keyframes beam-drop-h`
- `#cc-scroll-sticky`: `position:relative !important; height:auto !important; overflow:visible !important; display:block !important` — desfaz sticky + overflow do desktop
- `#cc-grid`: `display:block !important; transform:none !important` — sai do flex, JS controla visibilidade por classe
- JS sticky scroll: `if (window.innerWidth < 1024) return;` (early return, não inicializa)
- JS carousel: script separado após o sticky scroll IIFE, roda em `DOMContentLoaded`, também tem `if (window.innerWidth >= 1024) return;`

**Abordagem de visibilidade — `display:none/block` (NÃO `translateX`):**
- CSS: `.cc-wrap { display: none !important }` por padrão; `.cc-wrap.cc-mob-active { display: block !important }` mostra o card ativo
- `goTo(idx, dir)` remove `cc-mob-active`/`cc-mob-prev` de todos os cards e adiciona ao card atual
- Animação direcional: `cc-mob-fadein` (slide da direita, padrão ao avançar) e `cc-mob-fadein-back` (slide da esquerda, ao voltar via `cc-mob-prev`)
- **IMPORTANTE:** Nunca mudar isso de volta para `translateX` — overflow-x:hidden/clip no iOS Safari conflita com card expandido crescendo verticalmente

**Botões / dots:**
- Botões `#cc-prev-btn` / `#cc-next-btn` (class `cc-carousel-btn`) injetados via JS no `#cc-scroll-sticky` (position absolute, `top:160px`, `left:6px`/`right:6px`)
- `#cc-scroll-dots` reutilizados como indicadores: `cc-sdot-active` = `opacity:1; background:#F97316`
- Swipe horizontal: touch listeners com threshold de 50px

**Comportamento ao expandir/fechar:**
- Ao expandir card (`cc-has-open` no grid): botões nav ocultos via `.cc-nav-hidden`, `scrollTo` suave para topo do card (offset 80px nav fixa)
- Ao colapsar: botões reativados, `cc-mob-active` re-aplicado ao card atual via MutationObserver
- Card expandido: **sem `max-height`**, cresce verticalmente — usuário scrolla a página
- Outros cards enquanto um está aberto: `display:none !important` (via `#cc-grid.cc-has-open .cc-wrap:not(.cc-open)`)

**Alturas explícitas dos cards no mobile — crítico para animações visíveis:**
- **Card 1 (Pneus):** `.main-split → 1fr`, `.chat-panel height:280px` (necessário para `flex:1` no chat-body funcionar), `.chat-body min-height:0; overflow-y:auto`, `.sys-panel height:300px; overflow-y:auto`
- **Card 2 (Social):** `.sm-pipeline → 1fr`, `.sm-pipe-body flex:none; min-height:160px; max-height:220px` (flex:none necessário pois sm-pipe-col não tem altura explícita)
- **Card 3 (Imob):** `.c3-main → column`, `.activity-feed height:280px`, `.c3-screen height:260px`
- **Card 4 (Meta):** `.mc-metrics-bar → repeat(2,1fr)`, `.mc-action-feed max-height:none; overflow-y:visible`
- **Card 5 (RC):** `.rc-timeline max-height:none; overflow-y:visible`, padding ajustado, `.rc-ficha-fields → 1fr`, `.rc-pay-box → column`

**Armadilha do flex:1 sem pai com altura explícita:**
Cards 1 e 2 usam `flex:1` em elementos filhos (chat-body, sm-pipe-body) dentro de containers flex sem altura explícita. Sem `height` no pai, `flex:1` resolve para 0 — conteúdo fica invisível mesmo com JS adicionando elementos. Solução: sempre dar `height` explícito ao pai OU mudar filho para `flex:none` com `min-height`.

### Footer / Contato (mobile)
- `#ft-grid` → `grid-template-columns:1fr`, `padding:2.5rem 1.5rem`
- `#ft-right` (coluna decorativa com A_) → `display:none`
- `#ft-form-grid` → `grid-template-columns:1fr`
- `#ft-btn` → `width:100%`

### Expert section (mobile)
- Padding via `clamp()`: `clamp(2rem,6vw,4rem) clamp(1.5rem,5vw,3rem)`
- Fonte do subtexto via `clamp(0.9rem,2.5vw,1.1rem)`

---

## REGRAS

1. **Arquivo único:** tudo vive em `index.html`. Não criar arquivos separados.
2. **Não criar seções duplicadas.** Se já existe uma seção de cards, não criar outra.
3. **Não mover cards pra fora do cc-grid.** Todos os 5 case cards ficam dentro de `div#cc-grid` na `casos-section`.
4. **Respeitar prefixos CSS internos.** Cada card tem seu prefixo interno (mc-, rc-, sm-, cc-, c3-) no expanded. O collapsed usa sempre classes cc- universais.
5. **Não reativar sys-scroll-outer.** Essa seção é removida do DOM no runtime por script inline.
6. **Ao fazer qualquer alteração estrutural, atualizar este arquivo (CLAUDE.md)** com o que mudou — seções adicionadas/removidas, IDs novos, mudanças de ordem, lógica JS nova.

---

## HISTÓRICO DE ALTERAÇÕES (Abril 2026)

### ✅ IMPLEMENTADAS COM SUCESSO

1. **Metodologia scroll fix — maxProgress reset + cachedInnerH**
   - Problema: No mobile, o scroll da seção Metodologia ficava muito pra cima, cortando a animação do card
   - Solução: Adicionado `maxProgress` para acumular progresso máximo (não reverte ao subir) + `cachedInnerH` capturado no init para evitar jank do iOS (address bar aparece/some)
   - Commit: Integrado no fluxo de correções

2. **Hero mobile fix — remove empty space below CTA**
   - Problema: Espaço vazio preto abaixo do CTA no mobile
   - Solução: Alterado `<main>` de `min-h-[80vh]` para `lg:min-h-[80vh]` (altura mínima só em desktop)
   - Commit: Integrado

3. **Metodologia mobile layout — bar at bottom**
   - Problema: Barra de steps (01→02→03) ficava grudada no topo, tampando a animação
   - Solução: CSS mobile `order:2` no sidebar + `order:1` no content, positioning sticky ajustado com `top:4rem` e `height: calc(100vh - 4rem)`
   - Commit: Integrado

4. **Typebot integration — replace form**
   - Problema: Formulário antigo (Google Apps Script) precisava ser removido
   - Solução: Substituído por `<typebot-standard>` embed com script `Typebot.initStandard()` apontando para `meu-typebot-rdqvtm8` / `https://view.autonomaai.tech`
   - Localização: Seção contato (#contato-section), coluna esquerda (#ft-left)
   - Commit: Integrado

5. **Diagnóstico section removal**
   - Problema: Seção "Seu negócio gera dados o tempo todo" precisava ser removida
   - Solução: Removida completamente do HTML (foi uma seção sem ID antes de Manifesto)
   - Commit: Integrado

6. **Copy updates**
   - **Metodologia steps:**
     - Step 1: "Mapeamos onde o tempo some e onde está o dinheiro parado."
     - Step 2: "Sua equipe entende o que delegar. Você decide o que construir."
     - Step 3: "Agentes treinados com seus dados, prontos pra operar no dia um."
   - **Pilares header:** "Cada agente é construído pro seu negócio — não é instalado, é treinado."
   - **WhatsApp Demo subtitle:** "O problema não é o tamanho do time. É o quanto do time ainda faz coisa que máquina faz melhor."
   - **Contato description:** "Responda algumas perguntas e a gente te retorna com um diagnóstico real do seu negócio."
   - Commit: Integrado

7. **Valetão Pneus naming**
   - Problema: Case card "Pneus" usava "Disk Pneus" em 3 locais
   - Solução: Trocado para "Valetão Pneus" em: topbar do expanded (cc-card), chat header (cc-card), iPhone mockup header (whatsapp-demo-section)
   - Commit: `9411f1a`

### ❌ TENTADAS E REVERTIDAS

1. **WhatsApp demo section iPhone fix — Attempt 1 (sticky overflow-x: clip)**
   - Objetivo: iPhone mockup ESTÁTICO no mobile (não descer com scroll), mensagens animadas dentro
   - Tentativa: CSS `overflow-x: clip` (em vez de `overflow-hidden`), `position:sticky; top:100px; align-self:start` no iPhone, mobile `align-self:center`
   - Resultado: Ficou péssimo — user feedback: "volte como estava"
   - Revert: `git revert` simples, voltou ao estado anterior

2. **WhatsApp demo section iPhone fix — Attempt 2 (sticky + viewport math)**
   - Objetivo: Mesmo — iPhone estático
   - Tentativa: `overflow: clip` ambos eixos + sticky + cálculo viewport-aware `min(80vw, calc((100svh - 80px) * 300/648))`
   - Resultado: iPhone continuava descendo com scroll, pior ainda — user: "a tela não scrolla pra baixo, o celular estica, está péssimo"
   - Revert: `git revert`

3. **WhatsApp demo section iPhone fix — Attempt 3 (sizing reduction only)**
   - Objetivo: Mesmo
   - Tentativa: Reduzir tamanho do iPhone no mobile apenas com `max-width: 60vw`, sem sticky
   - Resultado: Ainda ficou péssimo — user: "volte antes de todas essas alterações, ficou péssimo"
   - Revert: `git revert` de todos os 5 commits de tentativas em um comando único
   - **Status atual:** Seção whatsapp-demo voltou ao estado original (antes de qualquer tentativa). iPhone mockup segue padrão desktop: `width:300px; max-width:85vw` com responsive media query (lg: 380px)
   - **Problema não resolvido:** iPhone ainda desce com scroll no mobile. Comportamento desejado explicitado pelo user: "um mockup de iphone ESTÁTICO, com as conversas descendo, mas a tela está estática."

---

## NOTAS DE DESENVOLVIMENTO

- **Seção whatsapp-demo:** Aguardando nova abordagem ou clarificação. Todas as 3 tentativas CSS falhou. Possível necessidade de redesign do layout (talvez iPhone não deveria estar em scroll absoluto, ou a seção precisa estrutura diferente).
- **Git revert chain:** Os 5 reverts de whatsapp foram feitos em um comando (`git revert efbbe21 4e5751c c92aa4f d5b83e4 914ded3 --no-edit`) criando 5 commits de revert sequenciais. Histórico Git é ligeiramente complexo, mas funcional.
- **Typebot API:** Endpoint customizado `https://view.autonomaai.tech` em vez de padrão `https://api.typebot.io`
