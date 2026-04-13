# Blueprint — TechParts.BR Landing Page
> Criado por **João Antonio**  
> Loja varejista de placas de vídeo, memórias RAM e processadores — novos e usados.

---

## Visão geral do projeto

| Item | Detalhe |
|---|---|
| Tipo | Landing page + catálogo de produtos |
| Tecnologia | HTML5 · CSS3 · JavaScript puro (sem frameworks) |
| Hospedagem sugerida | Vercel · Netlify · GitHub Pages |
| Responsividade | Mobile-first, breakpoint em 768px |
| Integrações | WhatsApp Business API (wa.me) |
| Autor | João Antonio |

---

## Arquitetura de arquivos

```
techparts-br/
│
├── index.html              ← Landing page principal
├── produto.html            ← Página individual de produto
│
├── assets/
│   ├── css/
│   │   ├── reset.css
│   │   ├── global.css      ← variáveis, tipografia, cores
│   │   ├── nav.css
│   │   ├── hero.css
│   │   ├── catalog.css
│   │   ├── drawer.css      ← carrinho lateral
│   │   └── produto.css     ← página de produto individual
│   │
│   ├── js/
│   │   ├── products.js     ← array de dados dos produtos
│   │   ├── catalog.js      ← renderização, filtros, busca
│   │   ├── cart.js         ← lógica do carrinho e drawer
│   │   ├── checkout.js     ← montagem da mensagem WhatsApp
│   │   └── produto.js      ← galeria, tabs de specs
│   │
│   └── img/
│       ├── gpus/           ← fotos das placas de vídeo
│       ├── ram/            ← fotos das memórias
│       └── cpu/            ← fotos dos processadores
│
└── blueprint.md            ← este documento
```

---

## Design system

### Paleta de cores

| Token | Hex | Uso |
|---|---|---|
| `--bg-base` | `#060910` | fundo geral |
| `--bg-card` | `#0d1117` | cards de produto |
| `--bg-surface` | `#0a0e15` | hero, banner, drawer |
| `--accent` | `#378add` | botões primários, destaques |
| `--accent-hover` | `#185fa5` | hover nos botões |
| `--accent-muted` | `rgba(55,138,221,0.15)` | fundos sutis |
| `--border` | `#1e3a5f` | bordas de cards |
| `--text-primary` | `#ccddf0` | texto principal |
| `--text-muted` | `#556677` | texto secundário |
| `--text-white` | `#ffffff` | headings do hero |
| `--green` | `#5dcaa5` | condição ótima |
| `--amber` | `#fac775` | condição regular / estrelas |
| `--red` | `#f0997b` | badge "Em alta" |
| `--whatsapp` | `#25d366` | botão WhatsApp |

### Tipografia

```css
font-family: 'Segoe UI', system-ui, sans-serif;

h1  → 2.4rem / weight 600 / color #ffffff
h2  → 1.6rem / weight 600 / color #ccddf0
h3  → 1.1rem / weight 500 / color #ccddf0
p   → 1rem   / weight 400 / line-height 1.7 / color #8899aa
small → 11px / color #556677 / letter-spacing 0.8px
```

### Grid de produtos

```css
display: grid;
grid-template-columns: repeat(auto-fill, minmax(210px, 1fr));
gap: 16px;
```

---

## Seções da Landing Page (index.html)

### 1. Navbar fixa

**Elementos:**
- Logo `TECH PARTS.BR` à esquerda
- Links de navegação: GPUs · RAM · CPUs · Usados · Contato
- Ícone de busca (abre campo inline)
- Botão carrinho com contador de itens (abre drawer)

**Comportamento:**
- Ao rolar 80px, adiciona classe `.scrolled` → borda inferior sutil
- Campo de busca expande com animação ao clicar no ícone

---

### 2. Hero section

**Elementos:**
- Background: `#0a0a0f` com grid de linhas SVG (azul 6% opacidade, 40x40px)
- Badge animado: `"Hardware de segunda mão · 100% testado"`
- H1: `"Performance real por um preço justo"`
- Subtítulo explicativo
- 2 CTAs: `"Ver placas de vídeo"` (primário) · `"Como testamos?"` (ghost)
- Imagem decorativa à direita: placa de vídeo em perspectiva (opcional em desktop)

---

### 3. Barra de estatísticas

4 cards em grid horizontal:

| Número | Label |
|---|---|
| 340+ | Peças vendidas |
| 98% | Satisfação |
| 3 meses | Garantia |
| 48h | Envio |

---

### 4. Barra de busca + filtros

**Busca:**
```
[ 🔍  Buscar por nome, modelo ou especificação... ]
```
- Filtra em tempo real sobre o array `products`
- Debounce de 200ms para performance

**Filtros de categoria (pills toggle):**
```
[ Todos ]  [ Placas de vídeo ]  [ Memória RAM ]  [ Processadores ]  [ Usados ]
```

**Filtro de preço (range slider duplo):**
```
R$ 0 ————●————————●———— R$ 3.000
         R$ 200        R$ 1.800
```
- Dois inputs `type="range"` sobrepostos via CSS
- Atualiza em tempo real ao arrastar

**Ordenação (select):**
```
[ Relevância ▾ ]  →  Menor preço · Maior preço · Mais novo · Melhor avaliado
```

---

### 5. Grid de produtos

**Card de produto — estrutura:**

```
┌─────────────────────────┐
│  [Em alta]  [Condição]  │  ← badges absolutos
│                         │
│      [ IMAGEM ]         │  ← foto real do produto, object-fit: contain
│                         │  ← zoom suave no hover
├─────────────────────────┤
│  RTX 3060 Ti            │  ← nome
│  8GB GDDR6 · TDP 200W   │  ← spec resumida
│                         │
│  R$ 1.190  ~~R$ 1.800~~ │  ← preço + riscado
│  ★★★★★                 │  ← avaliação
│               [+ Cart]  │  ← botão adicionar
└─────────────────────────┘
```

**Clique no card** → abre `produto.html?id=1`

**Badges de condição:**
- `Ótima` → verde
- `Boa` → azul
- `Regular` → âmbar

---

### 6. Banner "Venda sua peça"

```
┌────────────────────────────────────────────────────┐
│  Quer vender sua peça?                             │
│  Compramos GPUs, RAMs e CPUs. Avaliação em 24h.   │
│                              [ Enviar para avaliação → ] │
└────────────────────────────────────────────────────┘
```
Botão abre WhatsApp com mensagem pré-formatada.

---

### 7. Seção "Por que comprar aqui"

4 cards de feature:

| Ícone | Título | Descrição |
|---|---|---|
| 🔬 | Teste completo em bancada | Stress test, temperatura, benchmark em todas as peças |
| 🛡️ | Garantia real de 3 meses | Troca sem burocracia se defeito aparecer |
| 📦 | Embalagem antiestática | Proteção profissional no envio |
| 💬 | Suporte via WhatsApp | Dúvidas de compatibilidade com quem entende |

---

### 8. Footer

- Logo + tagline
- Links rápidos: Início · Produtos · Vender peça · Contato
- Redes sociais (ícones SVG): Instagram · WhatsApp · Facebook
- Copyright: `© 2025 TechParts.BR · Criado por João Antonio · Cacoal, RO`

---

## Carrinho lateral — Drawer (cart.js + drawer.css)

### Comportamento

- Abre deslizando da direita ao clicar no ícone do carrinho na navbar
- Overlay escurecido no fundo (clicável para fechar)
- Animação: `transform: translateX(100%)` → `translateX(0)` em 280ms

### Estrutura do drawer

```
┌──────────────────────────────┐
│  Carrinho          [✕ fechar]│
├──────────────────────────────┤
│  ┌────────────────────────┐  │
│  │ [img]  RTX 3060 Ti     │  │
│  │        8GB GDDR6       │  │
│  │        R$ 1.190  [-][+]│  │
│  │                  [🗑️] │  │
│  └────────────────────────┘  │
│                              │
│  ┌────────────────────────┐  │
│  │ [img]  Ryzen 5 5600X   │  │
│  │        6C/12T          │  │
│  │        R$ 520    [-][+]│  │
│  └────────────────────────┘  │
├──────────────────────────────┤
│  Subtotal:       R$ 1.710    │
│                              │
│  [ Finalizar via WhatsApp ] │  ← botão verde
└──────────────────────────────┘
```

### Dados mantidos em memória (array `cart`)

```js
cart = [
  { id: 1, name: 'RTX 3060 Ti', price: 1190, qty: 1, img: '...' },
  { id: 7, name: 'Ryzen 5 5600X', price: 520, qty: 1, img: '...' }
]
```

### Persistência

- `localStorage.setItem('cart', JSON.stringify(cart))` a cada alteração
- Recuperado ao carregar a página

---

## Checkout via WhatsApp (checkout.js)

### Lógica de montagem da mensagem

```js
function buildWhatsAppMessage(cart) {
  const lines = cart.map(item =>
    `▸ ${item.name} (x${item.qty}) — R$ ${(item.price * item.qty).toLocaleString('pt-BR')}`
  );
  const total = cart.reduce((acc, i) => acc + i.price * i.qty, 0);

  return [
    'Olá! Tenho interesse nos seguintes produtos:',
    '',
    ...lines,
    '',
    `*Total: R$ ${total.toLocaleString('pt-BR')}*`,
    '',
    'Poderia confirmar disponibilidade e frete?'
  ].join('\n');
}

function openWhatsApp(cart) {
  const msg = encodeURIComponent(buildWhatsAppMessage(cart));
  window.open(`https://wa.me/5569XXXXXXXXX?text=${msg}`, '_blank');
}
```

### Mensagem gerada (exemplo)

```
Olá! Tenho interesse nos seguintes produtos:

▸ RTX 3060 Ti (x1) — R$ 1.190
▸ Ryzen 5 5600X (x1) — R$ 520

*Total: R$ 1.710*

Poderia confirmar disponibilidade e frete?
```

---

## Página de produto individual (produto.html)

### URL

```
produto.html?id=1
```

O script lê o parâmetro `id`, busca no array `products` e renderiza dinamicamente.

### Layout da página

```
NAVBAR
──────────────────────────────────────────────────────

  ┌──────────────────────┐   ┌──────────────────────┐
  │                      │   │  RTX 3060 Ti          │
  │   [IMAGEM PRINCIPAL] │   │  ★★★★★  (48 avaliações)│
  │                      │   │                      │
  │  ┌──┐ ┌──┐ ┌──┐ ┌──┐│   │  R$ 1.190            │
  │  │  │ │  │ │  │ │  ││   │  ~~R$ 1.800~~  -34%  │
  │  └──┘ └──┘ └──┘ └──┘│   │                      │
  │  (miniaturas)        │   │  Condição: Ótima      │
  └──────────────────────┘   │  Vendidos: 12         │
                             │                      │
                             │  Quantidade: [-] 1 [+]│
                             │                      │
                             │  [+ Adicionar ao cart]│
                             │  [💬 Comprar WhatsApp]│
                             └──────────────────────┘

──────────────────────────────────────────────────────

  [ Especificações ] [ Descrição ] [ Compatibilidade ]
  ─────────────────────────────────
  Memória:        8GB GDDR6
  Barramento:     256-bit
  Clock base:     1410 MHz
  Clock boost:    1665 MHz
  TDP:            200W
  Conectores:     2x 8-pin
  Saídas:         3x DisplayPort, 1x HDMI 2.1
  Dimensões:      285 x 112 x 50mm
  Condição:       Usada — Ótima
  Garantia:       3 meses

──────────────────────────────────────────────────────

  Produtos relacionados
  [card] [card] [card] [card]
```

### Galeria de fotos

- Imagem principal grande (clicável para lightbox)
- 4 miniaturas abaixo: frente, traseira, conectores, detalhe PCB
- Troca ao clicar na miniatura (sem reload)
- Lightbox: overlay escuro + navegação por setas

### Tabs de conteúdo

Implementadas via JS puro — sem `display:none` no carregamento:

- **Especificações** — tabela de dois campos chave/valor
- **Descrição** — texto livre explicando o histórico da peça
- **Compatibilidade** — lista de soquetes, slots, fontes recomendadas

---

## Dados dos produtos (products.js)

```js
const products = [
  {
    id: 1,
    name: 'RTX 3060 Ti',
    shortSpec: '8GB GDDR6 · TDP 200W',
    price: 1190,
    oldPrice: 1800,
    category: 'gpu',
    condition: 'otimo',   // 'otimo' | 'bom' | 'regular'
    isUsed: true,
    hot: true,
    stars: 5,
    soldCount: 12,
    imgs: [
      'assets/img/gpus/rtx3060ti-front.jpg',
      'assets/img/gpus/rtx3060ti-back.jpg',
      'assets/img/gpus/rtx3060ti-ports.jpg',
      'assets/img/gpus/rtx3060ti-pcb.jpg',
    ],
    specs: {
      'Memória':       '8GB GDDR6',
      'Barramento':    '256-bit',
      'Clock base':    '1410 MHz',
      'Clock boost':   '1665 MHz',
      'TDP':           '200W',
      'Conectores':    '2x 8-pin',
      'Saídas':        '3x DP, 1x HDMI 2.1',
      'Garantia':      '3 meses',
    },
    description: 'Placa retirada de sistema de trabalho...',
    compatibility: ['PCIe 4.0 x16', 'Fonte mín. 550W', 'Qualquer CPU moderno'],
    whatsappMsg: 'Olá! Tenho interesse na RTX 3060 Ti (id:1). Está disponível?',
  },
  // ... demais produtos seguem o mesmo schema
];
```

---

## Sistema de busca (catalog.js)

```js
function searchProducts(query, products) {
  const q = query.toLowerCase().trim();
  if (!q) return products;
  return products.filter(p =>
    p.name.toLowerCase().includes(q) ||
    p.shortSpec.toLowerCase().includes(q) ||
    p.category.includes(q)
  );
}
```

Pipeline de filtragem completo (executado sempre que qualquer filtro muda):

```
products (fonte)
  → filterByCategory()
  → filterByCondition()   ← se "Usados" estiver ativo
  → filterByPrice()       ← range slider
  → searchByText()        ← campo de busca
  → sortBy()              ← select de ordenação
  → renderGrid()          ← injeta no DOM
```

---

## Filtro de preço — range slider duplo

### HTML

```html
<div class="price-range">
  <div class="range-track">
    <div class="range-fill" id="range-fill"></div>
    <input type="range" id="min-price" min="0" max="3000" value="0" step="50" />
    <input type="range" id="max-price" min="0" max="3000" value="3000" step="50" />
  </div>
  <div class="range-labels">
    <span id="label-min">R$ 0</span>
    <span id="label-max">R$ 3.000</span>
  </div>
</div>
```

### CSS (faixa preenchida entre os dois thumbs)

```css
.range-track { position: relative; height: 4px; background: #1e3a5f; border-radius: 2px; }
.range-fill  { position: absolute; height: 4px; background: #378add; border-radius: 2px; }
input[type="range"] { position: absolute; width: 100%; pointer-events: none; appearance: none; background: transparent; }
input[type="range"]::-webkit-slider-thumb { pointer-events: all; width: 18px; height: 18px; ... }
```

### JS

```js
[minInput, maxInput].forEach(input => {
  input.addEventListener('input', () => {
    let min = +minInput.value, max = +maxInput.value;
    if (min > max) [min, max] = [max, min];
    updateFill(min, max);
    applyFilters();
  });
});
```

---

## Responsividade

| Breakpoint | Comportamento |
|---|---|
| `< 480px` | Grid 1 coluna · Drawer 100% largura · Hero sem imagem decorativa |
| `480–768px` | Grid 2 colunas · Filtros em scroll horizontal |
| `768–1024px` | Grid 3 colunas · Navbar completa |
| `> 1024px` | Grid 4 colunas · Página de produto em 2 colunas lado a lado |

---

## Checklist de desenvolvimento

### Fase 1 — estrutura base
- [ ] Criar `index.html` com estrutura semântica
- [ ] Criar `global.css` com variáveis e reset
- [ ] Popular `products.js` com 8–12 produtos e imagens reais
- [ ] Renderizar grid de produtos via JS

### Fase 2 — filtros e busca
- [ ] Implementar filtro por categoria (pills)
- [ ] Implementar busca em tempo real com debounce
- [ ] Implementar range slider de preço duplo
- [ ] Implementar select de ordenação
- [ ] Conectar todos os filtros em pipeline único

### Fase 3 — carrinho
- [ ] Criar drawer lateral com CSS + animação
- [ ] Implementar `cart.js`: add, remove, alterar qty
- [ ] Persistir carrinho no `localStorage`
- [ ] Mostrar contador no ícone da navbar

### Fase 4 — checkout
- [ ] Implementar `buildWhatsAppMessage()`
- [ ] Botão "Finalizar via WhatsApp" no drawer
- [ ] Substituir número do WhatsApp pelo real

### Fase 5 — página de produto
- [ ] Criar `produto.html` com leitura de `?id=`
- [ ] Galeria de imagens com troca por miniatura
- [ ] Lightbox simples (overlay + setas)
- [ ] Tabs: Especificações · Descrição · Compatibilidade
- [ ] Botão WhatsApp direto da página do produto
- [ ] Seção de produtos relacionados (mesma categoria)

### Fase 6 — polimento
- [ ] Testar responsividade em mobile (360px)
- [ ] Adicionar `loading="lazy"` nas imagens
- [ ] Adicionar `meta` tags SEO no `<head>`
- [ ] Revisar acessibilidade (alt nas imagens, aria-label nos botões)
- [ ] Hospedar no Vercel ou Netlify

---

## Hospedagem — deploy rápido

### Vercel (recomendado)

```bash
npm i -g vercel
cd techparts-br
vercel
# URL gerada: techparts-br.vercel.app
```

### Netlify (drag & drop)

1. Acesse [netlify.com](https://netlify.com)
2. Arraste a pasta `techparts-br/` para a área de deploy
3. URL gerada instantaneamente

### GitHub Pages

```bash
git init
git add .
git commit -m "initial commit"
git remote add origin https://github.com/seu-usuario/techparts-br.git
git push -u origin main
# Ativar Pages em: Settings → Pages → Branch: main
```

---

## Créditos

| Item | Detalhe |
|---|---|
| Autor | João Antonio |
| Versão | 1.0.0 |
| Data | 2025 |
| Localização | Cacoal, RO — Brasil |
| Contato sugerido | WhatsApp Business |

---

*Blueprint gerado para o projeto TechParts.BR — todos os direitos reservados a João Antonio.*
