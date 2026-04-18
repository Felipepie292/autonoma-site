# Seção WhatsApp Demo + FAQ — Temporariamente oculta

**Data da ocultação:** 2026-04-18
**Motivo:** Apresentação para cliente. Seção estava com problema de responsividade
no iOS Safari mobile (iPhone mockup quebrando). Removida do fluxo visível até
ajuste final. Nada foi deletado — apenas escondido via CSS.

---

## O que foi escondido

1. **Section `#whatsapp-demo-section`** (index.html, linha ~4639)
   - iPhone mockup com conversa WhatsApp animada
   - FAQ com 9 perguntas ao lado
2. **Link de nav desktop "FAQ"** (index.html, linha ~418)
3. **Link de nav mobile "FAQ"** (index.html, linha ~442)

---

## Como reativar (3 alterações simples)

### 1. Section principal — remover `style="display:none;"`

**Localizar** (linha ~4639):
```html
<section id="whatsapp-demo-section" aria-label="Demonstração de atendimento via WhatsApp" class="relative w-full bg-[#050505] py-20 lg:py-32 overflow-hidden" style="display:none;">
```

**Trocar por:**
```html
<section id="whatsapp-demo-section" aria-label="Demonstração de atendimento via WhatsApp" class="relative w-full bg-[#050505] py-20 lg:py-32 overflow-hidden">
```

### 2. Link nav desktop — remover `hidden` e `data-hidden-faq-link`

**Localizar** (linha ~418):
```html
<a class="hover:text-white transition-colors hidden" href="#whatsapp-demo-section" data-hidden-faq-link>FAQ</a>
```

**Trocar por:**
```html
<a class="hover:text-white transition-colors" href="#whatsapp-demo-section">FAQ</a>
```

### 3. Link nav mobile — remover `hidden` e `data-hidden-faq-link`

**Localizar** (linha ~442):
```html
<a class="text-2xl font-medium text-white tracking-tight border-b border-white/5 pb-5 hidden" href="#whatsapp-demo-section" onclick="closeNavMenu()" data-hidden-faq-link>FAQ</a>
```

**Trocar por:**
```html
<a class="text-2xl font-medium text-white tracking-tight border-b border-white/5 pb-5" href="#whatsapp-demo-section" onclick="closeNavMenu()">FAQ</a>
```

---

## Pendência conhecida ao reativar

O iPhone mockup ainda tem problema de responsividade no iOS Safari mobile
(mockup vaza pra fora do contêiner). Ver `CLAUDE222.md` para o contexto completo.

Commits relacionados ao fix em andamento:
- `6c9c3b8` — fix: responsividade do iPhone mockup (revertido parcialmente)
- `ea73900` — fix: scroll via CSS transform (atual — ainda com bug)

Antes de reativar, resolver o problema de layout no iOS testando em device
real (não só DevTools).

---

## Referências no código

- Section: [index.html:4639](index.html#L4639) até [index.html:5044](index.html#L5044)
- CSS da section: linhas ~5048 a ~5076 (continua no arquivo, sem efeito com section hidden)
- Script JS da section: linhas ~5083 a ~5290 (continua no arquivo, usa IntersectionObserver que só dispara se visível)
- IDs internos: `wa-iphone-wrap`, `wa-chat-area`, `wa-messages`, `faq-list`, `faq-column`
