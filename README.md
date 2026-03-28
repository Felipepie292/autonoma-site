# AUTONOMA — Landing Page

Landing page para **Autonoma**, agência de automação e IA que fornece agentes de IA para empresas.

## Tech Stack

- **HTML/CSS/JS puro** — arquivo único (`index.html`)
- **Tailwind CSS** via CDN
- **Vanilla JavaScript** para animações e interatividade
- **Responsive design** (mobile-first, 375px+)
- **Brutalist design system** (dark theme, orange accents)

## Projeto

### Seções
1. **Nav** — Logo + Links + CTA
2. **Hero** — Cena isométrica + Stats reais
3. **Manifesto** — Letra animada scramble
4. **Diagnóstico** — Problema + Solução
5. **Metodologia** — 3 passos (sticky scroll)
6. **Pilares** — 4 cards de serviço
7. **Casos** — 5 case cards interativos com carousel mobile
8. **WhatsApp Demo** — iPhone mockup + FAQ
9. **Expert** — Seção fullscreen com foto
10. **Contato** — Formulário com integração Google Apps Script

### Cards Interativos
- **Pneus SDR** — Chat em tempo real
- **Social Media** — Kanban pipeline
- **Imobiliária** — Sistema de activity feed
- **Meta Ads** — Monitoramento de métricas
- **Recepcionista** — Timeline de agendamentos

## Setup Local

### Servir o site
```bash
npx serve
```
Acessa em `http://localhost:3000`

### Ou com Python
```bash
python3 -m http.server 8000
```

## Deploy

### Vercel (automático)
1. Acessa [Vercel Dashboard](https://vercel.com/dashboard)
2. Clica em **Add New Project**
3. Seleciona repositório `autonoma-site`
4. Clica **Deploy**

O site fará deploy automático a cada push na `main`.

### Configuração
- **Framework:** Static site
- **Output Directory:** `./` (root)
- **Build Command:** Não necessário (static files)

## Documentação

Ver **CLAUDE.md** para:
- Stack detalhado
- Seções do site (ordem, IDs, CSS)
- Case cards (arquitetura, animações)
- Mobile responsiveness (carousel, media queries)
- Regras do projeto

## Arquivo Principal

Tudo está em **`index.html`** (~5500 linhas):
- HTML + CSS inline + JavaScript
- Nenhum arquivo separado de assets (exceto imagens/fonts)
- Tailwind via CDN

## Contato

**Autonoma** — Automação & IA
- [Website](https://autonoma-site.vercel.app) (quando deployer)
- Email: contato@autonoma.ai (formulário integrado)

---

**Última atualização:** Março 2026
**Design System:** Brutalist, Dark Theme, Accent Laranja (#F97316)
