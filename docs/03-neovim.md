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


