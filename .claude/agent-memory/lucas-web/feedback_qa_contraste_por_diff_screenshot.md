---
name: feedback-qa-contraste-por-diff-screenshot
description: QA de contraste de elemento decorativo atras do texto por diff de screenshots exige scroll instantaneo e filtro de franja de antialiasing
metadata:
  type: feedback
---

Para provar que um ornamento de fundo (glow, simbolo, marca de agua) nao estraga a
legibilidade, medir por diff de dois screenshots — um com o ornamento visivel e outro
com `visibility:hidden` — e comparar a cor do texto contra o pixel de fundo alterado.
Tres armadilhas que dao numeros falsos:

1. **`html{scroll-behavior:smooth}`** — um `scrollTo` de 15000px demora varios segundos
   e os dois screenshots saem em pontos diferentes do percurso, dando "meia pagina
   alterada". Injetar `html{scroll-behavior:auto!important}` antes de medir.
2. **Animacoes** — congelar tudo com
   `*,*::before,*::after{animation-play-state:paused!important;transition:none!important}`,
   senao o marquee e os gradientes conicos entram no diff.
3. **Franjas de antialiasing** — os pixeis da borda das letras tambem "mudam" e ai
   compara-se o texto contra ele proprio, dando sempre ~1:1. Aceitar so pixeis de fundo
   plano: vizinhanca 3x3 na imagem base com spread do canal maximo <= 6.

Restringir a varredura a interseccao entre a caixa do ornamento (mais 2x o raio do blur)
e a caixa dos elementos-folha que contem texto — assim o numero e "contraste real sob
texto", nao "pior pixel da regiao", que e conservador de mais e reprova sem motivo.

**Why:** sem estes tres filtros o mesmo simbolo mediu 1.0:1, 3.0:1 e 4.7:1 em corridas
diferentes; so a versao filtrada bateu certo com o calculo analitico
(`fundo + opacidade * (cor - fundo)`).
**How to apply:** sempre que se acrescentar um decorativo por tras do conteudo numa LP
escura. Tecto pratico medido: laranja `#F7931A` sobre `#0B0B0B` passa AA com
`--texto2` (#9A93A8) ate opacidade .20; a .24 ja falha. Ver [[feedback-qa-provar-desktop-intacto]].
