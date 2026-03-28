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
4. **Diagnóstico** (sem ID, `bg-[#080808]`) — split 6/6: texto à esquerda (`lg:px-16`, `justify-center`) + código à direita. **Visível.**
5. **Metodologia** (id="metodologia-section") — sticky scroll, 3 steps
6. **Pilares** (id="pilares-section") — 4 cards de serviço
7. **Casos** (id="casos-section") — sticky horizontal scroll com 5 case cards
8. **WhatsApp Demo** (id="whatsapp-demo-section") — iPhone mockup + FAQ. **Visível.**
9. **Expert** (sem ID) — seção fullscreen `min-height:100vh`, `assets/foto_expert.png` como fundo (`object-fit:cover`), overlay `rgba(0,0,0,0.45)`. Texto grande (Inter 500, uppercase, `clamp(2.5rem,5vw,5rem)`) topo-esquerdo com `// ` laranja; subtexto topo-direito.
10. **Footer / Contato** — fundo `#ffffff`, grid 12 col. **SUBSTITUÍDO COMPLETAMENTE** (ver seção abaixo).

### HIDDEN / REMOVIDOS DO DOM:
- **sys-scroll-outer** — seção "Seus funcionários de IA" (lixo do template). **REMOVIDA DO DOM em runtime** por script inline logo após seu fechamento. Não reativar.
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

Footer antigo ("Enquanto você avalia...") foi **completamente substituído** por uma seção de contato com formulário.

### Estrutura:
- **Fundo:** `#ffffff`, `border-top: 1px solid #E5E7EB`
- **Grid:** 12 colunas, `col-span-7` esquerda + `col-span-5` direita
- **Padding:** `py-24 lg:py-32` equivalente (6rem top, inline)

### Coluna esquerda:
- Tag `// ENTRE EM CONTATO` (JetBrains Mono, 10px, laranja, com barra laranja à esquerda)
- Headline: "Pronto pra ver o que a IA pode fazer pelo seu negócio?" (`clamp(2rem,4vw,3.5rem)`, Inter 500)
- Descrição: 14px, `#6B7280`
- **Formulário** `id="ft-form"`: dois blocos lado a lado
  - Esquerdo: Nome, Telefone (com máscara BR `(XX) 9XXXX-XXXX`), Email
  - Direito: Nome da Empresa, Segmento (select), Porte (select)
- Inputs: `.ft-input` — bg transparente, só `border-bottom: 1px solid #E5E7EB`, focus laranja
- Botão `id="ft-btn"`: `SOLICITAR DIAGNÓSTICO`, bg `#F97316`, cantos retos
- Feedback `id="ft-msg"`: sucesso (verde `#166534` / `#f0fdf4`) ou erro (vermelho `#991b1b` / `#fef2f2`)
- Separador `1px #E5E7EB` + copyright `© 2025 AUTONOMA`

### Coluna direita:
- `A_` gigante centralizado: `clamp(280px,22vw,320px)`, Inter 500
  - `A`: `rgba(0,0,0,0.28)`
  - `_`: `rgba(249,115,22,0.6)` + `animation: logo-blink 1.2s step-end infinite`
- Corner accent topo-esquerdo: 24px, `border-top/left: 2px solid #F97316`
- Corner accent inferior-direito: 24px, `border-bottom/right: 2px solid rgba(249,115,22,0.3)`
- `overflow:hidden` no container

### Submit JS (script inline no footer):
- POST fetch para `https://script.google.com/macros/s/AKfycbyQO4a4YsRq_QqaZg0ik8kIn3JF1pazmShybbuYsNncxxspaSa5Gp2ZY9YyAiOiy9oi/exec`
- `mode: 'no-cors'` (resolve CORS do Google Apps Script)
- Body JSON: `{nome, telefone, email, empresa, segmento, porte}`
- Durante envio: botão desabilitado, texto `ENVIANDO...`
- Sucesso: formulário `display:none`, mensagem verde
- Erro: botão reabilitado, mensagem vermelha

### Máscara telefone (script inline no footer):
- Campo `id="ft-telefone"`, `maxlength="16"`
- Formata em tempo real: `(XX) 9XXXX-XXXX`, só aceita 11 dígitos
- Validação no blur: `setCustomValidity` se incompleto

---

## COPYS ATUALIZADAS

### Seção Diagnóstico (bg-[#080808]):
- **Parágrafo 1:** "Cada conversa com cliente esconde padrão de comportamento... porque tá todo mundo ocupado apagando incêndio."
- **Parágrafo 2 + destaque:** "Os Agentes da Autonoma capturam tudo isso enquanto trabalham... devolvem pra você os insights que sua equipe nunca teve tempo de extrair."
- **Barra laranja:** "Sua operação melhora toda semana. Sem você precisar contratar mais ninguém."

### Pilares — card "TREINADO POR PESSOAS":
- **Headline:** "Cada agente é construído do zero pro seu negócio."
- **Descrição:** "Mapeamos seus processos, treinamos os agentes com seus dados reais... é um funcionário digital que já chega sabendo como sua empresa funciona."

### WhatsApp Demo — subtítulo:
- "Enquanto sua equipe lida com retrabalho, esquecimento e sobrecarga... O gargalo que trava seu crescimento não é falta de gente. É excesso de processo manual."

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
- Sidebar de stats (`lg:col-span-2`) está `hidden lg:flex` — oculta no mobile
- Strip de stats abaixo do CTA: `flex lg:hidden` com 3 métricas em linha (`border-t`)
- Cena isométrica (`.hi-wrap`) oculta em mobile, visível a partir de `lg`

### Diagnóstico (mobile)
- Grid `grid-cols-1 lg:grid-cols-12`
- Coluna de código (`lg:col-span-6`) está `hidden lg:block` — oculta no mobile
- Coluna de texto ocupa largura total com `px-6 py-12`

### Metodologia (mobile)
- CSS block: `#metodologia-section { height: auto !important; }` e sticky container → `position:relative; height:auto; overflow:visible`
- `#meth-mobile` (fallback JS para mobile) já existia; o CSS desfaz o sticky container que o impedia de aparecer

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
