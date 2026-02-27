
Este documento descreve a arquitetura de software, padrões de design e lógica de implementação utilizados no projeto, servindo como material de consulta técnica e estudo de desenvolvimento Frontend.

---

## 1. ARQUITETURA ESTRUTURAL (HTML5 SEMÂNTICO)

A estrutura foi projetada para maximizar a acessibilidade e o SEO, utilizando a árvore do DOM de forma lógica:

*Identidade e Navegação (`<header>`, `<nav>`): Diferente de usar apenas <div>, o uso de <header> define a região de introdução. A tag <nav> informa aos motores de busca que ali reside o mapa do site. O uso de `aria-label` é um padrão de acessibilidade para garantir que leitores de tela identifiquem a navegação principal.

*Divisão por Contexto (`<section>`, `<article>`):** Cada seção possui um `id` único que serve como âncora para a URL. O uso de `<article>` nos cards de projetos e habilidades indica que cada item possui um contexto independente (pode ser entendido sozinho fora da página).

*Otimização de Assets: As imagens de projetos estão encapsuladas em elementos que permitem o controle de proporção via CSS, garantindo que o layout não quebre durante o carregamento.

---

## 2. ENGENHARIA DE ESTILIZAÇÃO (CSS3 ADVANCED)

O CSS foi escrito seguindo princípios de escalabilidade e responsividade:

*Box Model & Reset Global: O seletor universal `*` define `box-sizing: border-box`, o que altera o cálculo padrão do navegador. Aqui, o `padding` e a `border` são incluídos na largura total do elemento, evitando o erro clássico onde elementos "transbordam" a tela.

*Sistemas de Layout (Grid vs Flexbox):
*CSS Grid:** Utilizado no container `.grid`. A instrução `grid-template-columns: repeat(3, 1fr)` no desktop cria três frações iguais da largura total. É um sistema bidimensional para controle de colunas e linhas.

*Flexbox:** Utilizado na `.nav ul` e `.contact-list`. É um sistema unidimensional ideal para distribuir espaço entre itens em uma linha e alinhar o conteúdo ao centro de forma dinâmica.

*Posicionamento e Camadas (Z-Index): O botão `#backToTop` utiliza `position: fixed` com um `z-index: 1000`. Isso garante que o elemento seja removido do fluxo de renderização comum e "flutue" sobre todos os outros componentes, independente da rolagem.

*Media Queries (Mobile-First): O código foi pensado para telas menores primeiro, e as regras para desktop são injetadas apenas quando a largura da tela ultrapassa 768px, economizando processamento em dispositivos móveis.

---

## 3. LÓGICA DE COMPORTAMENTO (JAVASCRIPT / MANIPULAÇÃO DE DOM)

O JavaScript gerencia o estado da interface em tempo real através de Event Listeners e APIs nativas:

*Manipulação de Estilos via Scroll:*
* O listener de `scroll` verifica a propriedade `window.scrollY`.
*Lógica: Quando o valor ultrapassa 50 pixels, o JS acessa o objeto `style` do header e altera o `boxShadow`. Isso demonstra como manipular propriedades de CSS dinamicamente sem a necessidade de bibliotecas externas como jQuery.
* Intersection Observer API (O Coração da Interatividade):
Em vez de calcular posições de scroll constantemente (o que consome muita CPU), o `IntersectionObserver` funciona de forma assíncrona.
Ele observa quando a `.section` atinge 20% de visibilidade (`threshold: 0.2`).
Ao disparar, ele injeta os valores de `opacity: 1` e `translateY(0)`. Isso é engenharia de performance aplicada a animações.

*Scroll Programático:
* O evento de clique no ` #backToTop` utiliza o método `window.scrollTo`.
* O parâmetro `behavior: "smooth"` instrui o motor do navegador a realizar a interpolação de movimento entre a posição atual e o topo (Y=0), em vez de um salto abrupto.

---

## 4. FLUXO DE EXECUÇÃO (STUDY GUIDE)

1. Parsing: O navegador lê o HTML e monta o DOM.
2. CSSOM: O CSS é lido e aplicado à árvore do DOM.
3. JS Execution: O script.js é carregado ao final do body para não bloquear a renderização inicial.
4. Observer Setup: O JavaScript inicializa o observador de interseção, preparando as animações de entrada.
5. Event Loop: O navegador fica em estado de espera, aguardando interações de scroll ou cliques do usuário para disparar as funções programadas.

---
© 2026 - Fernando Morata | Documentação Técnica de Software
