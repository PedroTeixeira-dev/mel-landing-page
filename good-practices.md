# Boas Práticas — Projeto Site da Mel

Documento de referência para manter consistência em projetos futuros de landing page / portfólio.

---

## Layout & Responsividade

- **Sempre criar versão desktop e mobile.** O layout desktop usa `max-width: 1440px` centralizado com `margin: 0 auto`. No mobile, colunas viram linhas (`flex-direction: column`), padding reduz para `24px` e font-sizes escalam para baixo.
- **Breakpoints usados:** `1024px` (tablet) e `768px` (mobile). O mobile é o mais crítico — testar sempre.
- **`overflow-x: hidden`** no `html` e no `body` para evitar scroll horizontal indesejado, especialmente em mobile.
- **Padding lateral responsivo:** desktop `80–130px`, tablet `40px`, mobile `24px`.
- **Seções com `padding` vertical generoso** (70–90px desktop, 40–60px mobile) para dar respiro ao conteúdo.

---

## Navegação

- **Navbar sticky** (`position: sticky; top: 0; z-index: 300`) com fundo colorido para manter visibilidade ao rolar.
- **Menu hambúrguer no mobile** (`max-width: 768px`): três linhas animam para um **X** ao abrir (`rotate(45deg)` / `rotate(-45deg)` / `opacity: 0` na linha do meio).
- **Menu mobile fullscreen:** ocupa toda a tela abaixo da navbar, centralizado, com links grandes e bem espaçados (`gap: 44px`). Ao clicar em qualquer link, o menu fecha automaticamente.
- **Fechar com Escape:** adicionar listener `keydown` que fecha o menu se `e.key === 'Escape'`.
- **Focus trap:** quando o menu mobile está aberto, Tab e Shift+Tab ficam presos dentro dos links do menu.
- **Active state na nav via IntersectionObserver:** highlight no link da seção visível na viewport (`rootMargin: '-40% 0px -55% 0px'`).

---

## Animações

- **Scroll reveal com `IntersectionObserver`:** elementos entram na tela com fade + movimento. Três direções: `fade-up` (sobe), `fade-left` (vem da esquerda), `fade-right` (vem da direita).
- **Como aplicar:** `data-anim="fade-up"` no elemento HTML. Usar `data-delay="150"` (ms) para escalonar elementos em sequência dentro da mesma seção.
- **CSS das animações:**
  ```css
  .sc-hidden { opacity: 0 !important; }
  .sc-up      { transform: translateY(52px) !important; }
  .sc-left    { transform: translateX(-64px) !important; }
  .sc-right   { transform: translateX(64px) !important; }
  .sc-transition { transition: opacity 0.75s ease-out, transform 0.75s ease-out !important; }
  ```
- **Threshold:** `0.12` — o elemento começa a animar quando 12% dele entra na viewport.
- **Animações reversas:** ao sair da viewport, o elemento volta ao estado oculto (permite rever a animação ao rolar para cima).
- **Hover nos botões:** `opacity: .85` + `translateY(-2px)` com `transition: opacity .2s, transform .15s`.
- **Hover nos links de nav:** underline animado via `::after` com `width: 0 → 100%` + leve `scale(1.1)`.

---

## Acessibilidade

- **`skip-link`** no topo do body: link invisível que aparece ao focar via teclado, permitindo pular direto para o conteúdo.
- **`aria-label`** em todos os elementos interativos sem texto visível (logo, botão hambúrguer).
- **`aria-expanded` / `aria-hidden` / `aria-modal`** no menu mobile atualizados via JS conforme estado.
- **`aria-labelledby`** nas sections apontando para o `h2` correspondente.
- **`:focus-visible`** em todos os links e botões (outline visível só para teclado, não para mouse).
- **`loading="lazy"`** em todas as imagens que não estão na viewport inicial.
- **`rel="noopener noreferrer"`** em todos os links `target="_blank"`.

---

## Estrutura HTML

- **Arquivo único (`index.html`):** CSS e JS inline. Elimina dependências externas e simplifica o deploy.
- **Tokens CSS no `:root`** para cores — facilita manutenção e consistência.
- **`page-wrap`** como classe de container com `max-width: 1440px` reutilizável em todas as seções.
- **Seções semânticas** com `<section id="...">` e `<nav>`, `<footer>` corretamente marcados.
- **Favicon SVG:** `<link rel="icon" type="image/svg+xml" href="assets/logo-header.svg">` — escalável em qualquer resolução.

---

## Paleta de Cores

| Token       | Hex       | Uso principal                                      |
|-------------|-----------|---------------------------------------------------|
| `--yellow`  | `#F1CD79` | Navbar, seção "Sobre", footer, menu mobile         |
| `--dark`    | `#291E06` | Seção "Story" (fundo escuro)                       |
| `--blue`    | `#6C9DBD` | Seção "Contato" (fundo), botão orçamento           |
| `--white`   | `#FFFFFF` | Fundo padrão do body                               |
| `--text`    | `#2d1f04` | Texto principal (marrom escuro quente)             |

**Regra geral:** fundo claro → texto `--text`; fundo escuro (`--dark`) → texto branco com opacidade (`.78` corpo, `.92` destaque).

---

## Tipografia

**Fonte única:** `SUSE` (variable font do Google Fonts, eixo `wght` 100–800)

| Elemento                  | Peso  | Estilo  | Tamanho desktop | Tamanho mobile |
|---------------------------|-------|---------|-----------------|----------------|
| Hero `h1`                 | 400   | italic  | 68px            | 40px           |
| Hero subtítulo            | 300   | italic  | 20px            | 14px           |
| "Olá, prazer!" (`h2`)     | 250   | italic  | 62px            | 46px           |
| Contato `h2`              | 300   | italic  | 52px            | 34px           |
| Projetos `h2`             | —     | normal  | 32px            | 26px           |
| Serviços `h3`             | 700   | normal  | 18px            | —              |
| Corpo de texto (`p`)      | 400   | normal  | 14px            | 14px           |
| Destaque italic (`p`)     | 600   | italic  | 14px            | —              |
| Nav links                 | 500   | normal  | 16px            | 22px (mobile)  |
| Botões CTA                | 600–700 | normal | 15px           | 15px           |
| Footer copyright          | 400   | normal  | 10px            | 10px           |

**Carregamento:** `preconnect` nos domínios do Google Fonts antes do `<link>` da fonte.  
**Padrão:** `letter-spacing: 0` em toda a tipografia — a fonte já tem tracking equilibrado.

---

## Assets & Imagens

- **SVGs para ícones, logos e elementos gráficos** — nunca PNGs para elementos que precisam escalar.
- **Imagens compostas feitas no Figma** (foto + ícones decorativos num único SVG) — simplifica o HTML e garante alinhamento perfeito.
- **Botões como `background-image` SVG** (polígonos hexagonais) com dimensões fixas no CSS — criativo e sem dependência de canvas.
- Pasta `assets/` para todos os arquivos de mídia.

---

## Performance

- **CSS e JS inline** no HTML único — zero round trips extras.
- **`loading="lazy"`** em todas as imagens fora do hero.
- **`scroll-behavior: smooth`** no `html` para navegação âncora suave.
- **IntersectionObserver** para animações e active nav — sem scroll listeners custosos.
- **Variable font** carrega um único arquivo de fonte para todos os pesos.
