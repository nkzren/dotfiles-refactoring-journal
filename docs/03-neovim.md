# Neovim

Com nosso terminal configurado, podemos seguir customizando a próxima ferramenta
que vai dar trabalho: o Neovim. E seguindo o padrão dos meus dotfiles antigos,
ele também tem configurações demais e explicações de menos. No entanto, o
problema de performance que meu terminal tinhha não existe aqui, logo meu
objetivo é apenas um: simplificar uma configuração que ficou complexa demais

## Setup inicial

Como estamos começando zerados, nada mais justo que iniciar com o primeiro
arquivo de config: `~/.config/nvim/init.lua`. E como a config do neovim é um
pouco mais complexa que a do terminal, vamos manter o init simples, e
modularizá-lo de uma forma que faça sentido.

```lua
vim.cmd 'colorscheme desert'

local o = vim.opt

o.mouse='a'

-- line numbers
o.nu=true
o.rnu=false

-- whitespace
o.listchars = { trail = '␣', tab = '|-', eol = '↵', extends = '>', precedes = '<'  }
```

O `<Leader>` no (neo)vim é uma tecla muito útil para iniciar comandos. Quando
apertada, você tem 1 segundo para apertar outra sequencia de teclas para
executar um comando, então ela é bastante utilizada para customizar seus atalhos
sem afetar o comportamento padrão do vim.

A tecla padrão para isso é o `\`, que não é muito conveniente uma vez que você
vai usá-la o tempo todo, então eu geralmente coloco ela no espaço com as linhas
abaixo:

```lua
vim.g.mapleader=' '
vim.g.maplocaleader=' '
```

A seguir, algumas options para deixar a busca case insensitive. O `smartcase`
serve para torná-la case sensitive somente quando você digitar uma letra
maiúscula na busca. Notem que eu criei um local para o `vim.opt`, assim eu não
preciso ficar repetindo o caminho todo várias vezes. Vai ser bem útil para as
options seguintes

```lua
local o = vim.opt

o.ignorecase=true
o.smartcase=true
```

As próximas options são as line `nu`mbers e `r`elative line `nu`mbers. Diferente
da maior parte dos usuários de vim, eu prefiro manter as linhas relativas
desabilitadas. As linhas absolutas facilitam a comunicação em situações de pair
programming, então eu acabo preferindo mantê-las por padrão e fazer o cálculo na
cabeça quando preciso navegar X linhas com `j/k`, já que é algo que eu preciso
fazer com menos frequência

```lua
o.nu=true
o.rnu=false
```

Agora, configuramos o tamanho padrão dos tabs. Com esses 3 valores, os tabs
ficam com 4 espaços e os espaços acompanham o mesmo comportamento. A explicação
completa desses valores do `shiftwidth` e `softtabstop` e o que eles fazem está
[aqui nessa pergunta do StackOverflow](https://stackoverflow.com/q/51995128)

```lua
o.tabstop=4
o.shiftwidth=0
o.softtabstop=-1
```

Por fim, como interagir com o clipboard não é muito prático com a configuração
padrão, eu criei alguns atalhos pra isso. O primeiro argumento do
[`nvim_set_keymap`](https://neovim.io/doc/user/api.html#nvim_set_keymap()) é o
modo do vim em que o atalho é válido (`n`ormal, `v`isual, `i`nsert, etc...), e
os seguintes são o atalho, comando que ele executa e os parâmetros. O `noremap`
serve para sobrescrever o comando, caso já exista um igual, e o `silent` oculta
a mensagem do prompt do vim quando o comando é executado

```lua
local map = vim.api.nvim_set_keymap
local opts = { noremap=true, silent=true }

map('n', '<leader>y', '"+y', opts)
map('n', '<leader>Y', '"+yy', opts)
map('v', '<leader>y', '"+y', opts)


map('n', '<leader>p', '"+p', opts)
map('n', '<leader>P', '"+P', opts)
map('v', '<leader>p', '"+p', opts)
```

Eu poderia configurar o restante dos mappings agora, mas por enquanto podemos
seguir somente com essas, já que algumas mappings dependem de plugins estarem
instalados.

## Filetypes

### `filetype.lua`

A detecção de tipo de arquivo do neovim geralmente funciona, mas em algumas
situações específicas, queremos estender o comportamento para que ele reconheça
mais casos (Um exemplo comum são arquivos de template, como `.env.sample` ou
`some.conf.template`). Para isso, podemos adicionar um `filetype.lua` na nossa
config.

Tem algumas formas de configurar ele (filename, extension e pattern), que você
pode ver em detalhes lendo a doc do neovim (`:help vim.filetype.add()`), mas no
meu
[dotfiles](https://github.com/nkzren/dotfiles/blob/main/dot_config/nvim/filetype.lua)
tem alguns exemplos

### `ftplugin`

Às vezes queremos adicionar configurações específicas para certos tipos de
arquivos. Pra isso, podemos adicionar arquivos na pasta `ftplugin`, seguindo o
padrão `<filetype>.(vim|lua)`. Diferente do restante dos arquivos de config, eu
prefiro utilizar `vimscript` pra esses caras, já que eu geralmente não modifico
muita coisa além de algumas opts e é mais simples fazer isso com `vimscript`.

## Plugin Manager

Agora que temos algumas configs básicas feitas, podemos começar a estender o
comportamento do Neovim. E para isso vamos utilizar um gerenciador de plugins.
Apesar de ser possível instalar plugins manualmente, um gerenciador facilita
nosso trabalho.

A minha escolha aqui foi o [Lazy](https://github.com/folke/lazy.nvim), mas
existem outros como o [vim-plug](https://github.com/junegunn/vim-plug) ou o
[packer](https://github.com/wbthomason/packer.nvim).

O setup do Lazy foi feito seguindo a [doc](https://lazy.folke.io/installation)
no modo estruturado. Dessa forma, mantemos o mínimo possível no nosso `init.lua`
e facilita a remoção/troca do Lazy (basta remover o `require('config.lazy')`).
Caso você não ligue pra isso, o setup com um único arquivo é mais simples e
direto.

Com o Lazy instalado, podemos estruturar nossas configs da seguinte forma:

```sh
.
├── init.lua
└── lua
    ├── config
    │   ├── lazy.lua
    │   └── mappings.lua
    └── plugins
        ├── some.lua
        ├── plugins.lua
        └── by-category.lua
```

A doc do Lazy tem alguns [exemplos](https://lazy.folke.io/spec/examples) de como
adicionar e configurar novos plugins. Por padrão, ele vai importar todos os
plugins retornados nos arquivos da pasta `plugins`, mas minha recomendação é
inicialmente adicioná-los somente em um arquivo.

Um dos problemas dos meus dotfiles anteriores era justamente nos plugins do
Neovim: Era difícil de encontrar onde estava cada plugin porque os agrupamentos
foram feitos de forma prematura. Deixar todos os plugins inicialmente em um
único arquivo evita esse problema inicialmente quando não temos muitos plugins

## Plugins

Com o plugin manager instalado, podemos adicionar alguns plugins. Esta seção
será separada em 3 subseções:

1. Core: Plugins que eu acredito serem muito úteis mesmo para quem não quer
   utilizar o Neovim como editor principal (:sad:). A maior parte dos plugins
aqui são para facilitar edições e navegação por arquivos de texto (o propósito
principal do Neovim, afinal de contas)

2. Extra: Plugins para quem pretende utilizar o neovim como editor principal.
   Aqui vão ter plugins para otimizar o fluxo de trabalho com código adicionando
ferramentas como linting, formatação e autocomplete

3. UI: Plugins pra incrementar a interface do neovim. Porque ter pontos de
   estilo também é importante

As listas não serão extensivas e em sua maior parte vão ter somente um
comentário resumindo o propósito do plugin. Consulte a documentação de cada um
para saber como configurá-los (Basta adicionar `github.com/` na frente do nome
completo do plugin)

## Core

- "RRethy/vim-illuminate": Ilumina palavras repetidas. Bem útil para localizar
variáveis
- "tpope/vim-commentary": Adiciona atalhos para comentar/descomentar código
- "tpope/vim-sleuth": Identifica automaticamente o padrão de tabs/espaços dos
arquivos abertos e altera a config de acordo

