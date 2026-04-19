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

## Arquivos do Projeto

| Arquivo | Descrição |
|---|---|
| `index.html` | Site principal (~5500 linhas) |
| `design-system.html` | Referência visual completa — fonte da verdade para decisões visuais |
| `autonoma-brand-system.html` | Brand system da Autonoma (wordmark, cores, hero scene) |
| `slides-apresentacao.html` | **Deck de slides de negócios** (ver seção abaixo) |
| `DOCS_whatsapp-demo.md` | Instruções para reativar a seção whatsapp-demo (temporariamente oculta) |
| `CLAUDE222.md` | Análise e plano de fix para o iPhone mockup quebrado no iOS |

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
7. **Expert** (sem ID) — seção fullscreen `min-height:100vh`, `assets/foto_expert.png`
8. **Footer / Contato** (id="contato-section") — fundo branco `#ffffff`, Typebot embed

### OCULTAS TEMPORARIAMENTE (display:none — não deletar):
- **WhatsApp Demo** (id="whatsapp-demo-section", linha ~4639) — `style="display:none;"` adicionado para apresentação ao cliente. iPhone mockup + FAQ. **Ver `DOCS_whatsapp-demo.md` para reativar.** Problema de responsividade no iOS Safari mobile pendente de resolução.
- **Links "FAQ"** no nav (desktop linha ~418 e mobile linha ~442) — classe `hidden` adicionada; atributo `data-hidden-faq-link`.

### REMOVIDAS DO DOM (não recriar):
- **Diagnóstico** (`bg-[#080808]`) — seção "Seu negócio gera dados o tempo todo" removida completamente.
- **sys-scroll-outer** — seção "Seus funcionários de IA" (lixo do template). Removida do DOM em runtime por script inline. Não reativar.
- **cases-section** (id="cases-section") — seção antiga que continha mc-card e rc-card. Cards migrados para #cc-grid. Seção está `class="hidden"`, não usar.
- **compare-section** (id="compare-section") — comparativo operacional, `display:none`

---

## SLIDES DE APRESENTAÇÃO — slides-apresentacao.html

Arquivo `slides-apresentacao.html` na raiz do projeto. Deck de negócios baseado no design system da Autonoma.

### Estrutura (9 slides):
1. **Capa** — AUTONOMA_ + tagline + 3 stats principais
2. **O Problema** — contexto de mercado, 4 dores operacionais
3. **A Solução** — agentes 24/7, métricas de qualidade
4. **Metodologia** — 3 passos: Identificar → Educar → Desenvolver
5. **Agentes** — 4 tipos (Atendimento, Análise, Automação, Tráfego)
6. **Cases** — Valetão Pneus, Meta Ads, Imobiliária
7. **Por que Autonoma** — 4 diferenciais
8. **CTA** — diagnóstico gratuito
9. **Obrigado** — contato

### Navegação:
- **Teclado:** → / ↓ / Espaço avança, ← / ↑ volta, Home/End para primeiro/último
- **Clique:** lado direito avança, lado esquerdo volta
- **Toque/Swipe:** swipe esquerdo avança, direito volta
- **Barra de progresso:** bottom da tela, laranja
- **Indicador:** `01 / 09` canto inferior direito

### Design tokens usados:
- Background `#050505`, accent `#F97316`, Inter 500
- Corner accents, beam borders animados, glow radial laranja
- Sem border-radius em nenhum elemento (estética brutalist)

---

## SEÇÃO DE CASE CARDS — ESTRUTURA ATUAL

Existem **5 case cards interativos**, todos dentro de `#cc-grid` na `casos-section`. Todos usam a mesma arquitetura `cc-wrap`.

### Localização:
- **casos-section** (id="casos-section", VISÍVEL): todos os 5 cards estão aqui dentro de `#cc-grid`

### Anatomia universal do card (todos os 5 seguem este padrão):
```html
<div class="cc-wrap min-w-0" id="[id-do-card]" onclick="[fn]Expand(event)">
  <div class="cc-ca cc-ca-tl"></div>
  <div class="cc-ca cc-ca-br"></div>
  <div class="cc-collapsed">
    <img src="assets/[foto].png" class="cc-photo" alt="" aria-hidden="true">
    <div class="cc-photo-gradient"></div>
    <div class="cc-photo-footer">
      <div class="cc-tag">// Segmento · Tipo</div>
      <h3 class="cc-title">Nome do agente</h3>
    </div>
    <div class="cc-specs-overlay">
      <div class="cc-specs-inner">
        <p class="cc-sub">Descrição</p>
        <div class="cc-metrics">...</div>
        <div class="cc-cta">...</div>
      </div>
    </div>
  </div>
  <div class="cc-expanded">
    <!-- conteúdo interativo expandido (único por card) -->
  </div>
</div>
```

### Prefixos CSS por card (IMPORTANTE — evita conflitos):

| Card | ID | Assets | Expand fn | Collapse fn | Prefixo interno (expanded) |
|------|----|--------|-----------|-------------|---------------------------|
| Pneus SDR | cc-card | card_pneu.png | ccExpand() | ccCollapse() | topbar-, chat-, sys- |
| Social Media | cc-card2 | foto_card_socialmedia.png | cc2Expand() | cc2Collapse() | sm- |
| Imobiliária | cc-card3 | imagem_case card imovel.png | cc3Expand() | cc3Collapse() | c3- |
| Meta Ads | mc-card | imagem_card meta ads.png | mcExpand() | mcCollapse() | mc- |
| Recepcionista | rc-card | imagem_card recepcionista.png | rcExpand() | rcCollapse() | rc- |

---

## STICKY HORIZONTAL SCROLL — casos-section

A seção de cases usa scroll sticky horizontal: ao rolar a página para baixo, os cards se movem para a lateral até mostrar todos os 5.

### Estrutura HTML:
```html
<div id="cc-scroll-outer" style="position:relative;">
  <div id="cc-scroll-sticky" style="position:sticky;top:0;overflow:hidden;...">
    <section id="casos-section" ...>
      <div class="max-w-screen-xl mx-auto px-6 lg:px-12">
        <div id="cc-grid" class="flex flex-row flex-nowrap gap-6 items-start" style="will-change:transform;">
          <!-- 5 cc-wrap cards -->
        </div>
      </div>
    </section>
    <div id="cc-scroll-dots">  <!-- 5 pontos de progresso -->
  </div>
</div>
```

---

## HERO ISOMETRIC SCENE (.hi-wrap)

Cena 3D isométrica na coluna central do hero (col-span-5). CSS prefixado com `.hi-wrap`.

- **Elementos:** monitor (chat), phone (WhatsApp msgs), 2 dashboard cards, agenda, 6 notificações, 8 partículas laranja
- **IDs JS:** `hiMonChat`, `hiPhoneMsgs`, `hiConvVal`, `hiConvBars`, `hiLeadsVal`, `hiCalNewEvent`
- **Script:** IIFE inline após `</main>` (não usa DOMContentLoaded)
- **Keyframes:** prefixados com `hi-`

---

## LOGO OFICIAL (Nav)

```html
<span style="font-family:'Inter',sans-serif;font-weight:500;text-transform:uppercase;letter-spacing:-0.05em;font-size:1.35rem;color:#fff;white-space:nowrap;line-height:1">
  AUTONOMA<span style="color:#F97316;animation:logo-blink 1.2s step-end infinite">_</span>
</span>
```

---

## HERO — SIDEBAR STATS

3 métricas reais da Autonoma:
- `+35` — Empresas automatizadas
- `47k` — Leads processados/mês
- `R$2.1M` — Economizados em folha

---

## FOOTER / SEÇÃO DE CONTATO

- **Fundo:** `#ffffff`, `border-top: 1px solid #E5E7EB`
- **Grid:** 12 colunas, `col-span-7` esquerda + `col-span-5` direita
- **Typebot embed** (formulário): `<typebot-standard style="width:100%;height:600px;">`

---

## COPYS ATUALIZADAS

### Metodologia — headlines dos steps:
- **01 IDENTIFICAR:** "Mapeamos onde o tempo some e onde está o dinheiro parado."
- **02 EDUCAR:** "Sua equipe entende o que delegar. Você decide o que construir."
- **03 DESENVOLVER:** "Agentes treinados com seus dados, prontos pra operar no dia um."

### Case cards — descrições (cc-sub, 15px):
- **cc-card (Pneus):** "Cliente manda 'quanto custa o pneu 205/55?' e em 8 segundos..."
- **cc-card2 (Social):** "Recebe o briefing do cliente, gera copy e arte via IA..."
- **cc-card3 (Imobiliária):** "Lead chegou do Meta Ads? Em 2 minutos o agente já perguntou..."
- **mc-card (Meta Ads):** "Às 3h da manhã, enquanto o gestor dorme..."
- **rc-card (Recepcionista):** "Paciente manda 'quero marcar uma limpeza' às 23h..."

---

## MOBILE RESPONSIVENESS (375px–430px)

- **Nav:** hamburger mobile com overlay fullscreen
- **Hero:** cena isométrica oculta no mobile; strip de stats abaixo do CTA
- **Metodologia:** layout vertical com barra 01→02→03 no rodapé
- **Cases:** carousel 1 card por tela com swipe; JS `if (window.innerWidth < 1024) return`
- **Footer:** grid 1 coluna, coluna decorativa oculta

### Armadilha crítica — flex:1 sem pai com altura explícita:
Elementos filhos com `flex:1` dentro de containers flex sem altura explícita resolvem para 0 no mobile. Sempre dar `height` explícito ao pai OU mudar filho para `flex:none` com `min-height`.

---

## REGRAS

1. **Arquivo único:** tudo vive em `index.html`. Não criar arquivos separados (exceto utilitários como slides).
2. **Não criar seções duplicadas.** Se já existe uma seção de cards, não criar outra.
3. **Não mover cards pra fora do cc-grid.** Todos os 5 case cards ficam dentro de `div#cc-grid` na `casos-section`.
4. **Respeitar prefixos CSS internos.** Cada card tem seu prefixo interno (mc-, rc-, sm-, cc-, c3-) no expanded.
5. **Não reativar sys-scroll-outer.** Essa seção é removida do DOM no runtime por script inline.
6. **Ao fazer qualquer alteração estrutural, atualizar este arquivo (CLAUDE.md)** com o que mudou.
