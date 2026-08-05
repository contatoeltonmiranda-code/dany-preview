---
name: banco-imagens-video-gratis-como-buscar
description: Como encontrar foto e video de banco gratuito neste ambiente - API do Pexels quase nao pesquisa, Unsplash e Mixkit resolvem por HTML
metadata:
  type: reference
---

Para procurar imagem ou video de banco gratuito a partir do Bash deste ambiente:

- **Unsplash** - raspar `https://unsplash.com/s/photos/<query com espacos>` com `curl -sL`.
  O HTML traz um JSON escapado (`\"` -> `"`) com `slug`, `alt_description` e o objeto do
  autor (`"username":"...","name":"..."`). Da para pesquisar qualquer termo. O ficheiro
  descarrega-se por `https://images.unsplash.com/photo-<id>?w=1800&q=82&fm=jpg&fit=max`.
  Licenca Unsplash: comercial livre, atribuicao opcional.
- **Mixkit** (video) - `https://mixkit.co/free-stock-video/<tema>/` responde 200. Os mp4
  estao no HTML como `https://assets.mixkit.co/videos/<id>/<id>-720.mp4`. So 360 e 720
  descarregam; 1080 da 403. Licenca Mixkit: comercial livre, sem atribuicao.
- **Pexels** - `api.pexels.com/v1/search` responde sem chave, mas **so devolve resultados
  para um punhado de termos** (bitcoin, laptop, technology, business, office); o resto vem
  com `photos:[]`. `api.pexels.com/videos/search` pede chave (401) e o HTML de pesquisa da
  403. Nao contar com o Pexels para procurar.
- **Pixabay** - API pede chave (400) e o HTML da 403. Nao serve.

**Como aplicar:** ir direto ao Unsplash para fotos e ao Mixkit para video; nao gastar
tempo a tentar a API do Pexels. Confirmar sempre a foto a olho (montar um contact sheet
com Pillow) antes de a montar na pagina - varias trazem texto em ingles ou tema errado
apesar do `alt` parecer certo. Registar autor e link em `_brain/CREDITOS-IMAGENS.md`.
