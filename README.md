# Introdução

Bem vindos ao registro da minha (mais uma) tentativa de melhorar a bagunça que
são meus dotfiles. Depois de alguns anos mantendo-os ativamente, eu comecei a
perceber que algumas decisões feitas por eu mesmo no passado não foram muito
boas (te odeio, Renan do passado), e que removê-las sem quebrar meu fluxo de
trabalho seria trabalhoso demais.

Assim, me surgiu a ideia de refazer eles do zero e ir registrando o raciocínio
por trás de cada decisão, para que possa ajudar alguém a fazer seus próprios
dotfiles, e obviamente, pro Renan do futuro voltar aqui e reclamar das decisões
que estão sendo tomadas hoje sabendo os motivos que o levaram a isso.

Antes de começar a falar dos dotfiles em si, um pouco de contexto: Eu trabalho
utilizando Linux desde 2019, mas só comecei a efetivamente explorar o SO em 2020.
Um ano depois, comecei a manter os meus [dotfiles](https://github.com/nkzren/dotfiles)
e desde então, peguei gosto pela coisa e tenho sempre explorado novas formas de
customizar o meu SO de uma forma que seja interessante pra mim. Costumo falar
que acaba virando um hobby análogo a um "design de interiores" dos nerdolas.

Agora, em Julho de 2024, vejo que algumas decisões que eu tomei quando estava
estruturando meu ambiente não foram muito boas e difíceis de serem removidas.
Então, após algumas tentativas mal sucedidas de corrigir essas decisões, resolvi
investir um tempo extra e refazer de uma forma que eu acho satisfatória e mais
flexível para mudanças futuras, esperando que meus anos extras de experiência
contribuam com isso

Dito isso, vamos ao que interessa!

# Premissas

Essa seção explica alguns elementos por trás da estrutura atual dos meus
dotfiles, o que pretendo manter e o que será removido. A ideia aqui é ter um
guia para facilitar o entendimento de algumas decisões

## Multi-contexto

Um dos meus objetivos com meu dotfiles é ter uma configuração única para minha
estação de trabalho e meu computador pessoal. Mesmo estando ambientes
diferentes, ter a maior parte da configuração comum é algo interessante para
mim, então eu gostaria de algo que dê suporte para isso.

A ferramenta escolhida para esse propósito foi o [chezmoi](https://www.chezmoi.io/),
que me permite criar templates de dotfiles e fazer substituições condicionais.
Assim, eu tenho um único arquivo que pode variar de acordo com variáveis como
SO, configuração de monitores, etc...

## Window Manager

Utilizar um tiling window manager foi algo que mudou a forma como eu utilizo o
computador no dia a dia. A curva de aprendizado é maior no início, mas hoje eu
consigo ter uma produtividade muito maior por conta dele, principalmente quando
estou em um notebook somente com o touchpad. A maior parte das tarefas básicas é
feita somente com a parte principal do teclado, sem movimentar muito minhas
mãos.

Para esse propósito, utilizo o [i3](https://i3wm.org/), que é um dos mais
populares para o X11 e conta com um range bom de funcionalidades

## Terminal

Como a ideia é fazer o máximo possível sem sair do terminal, boa parte dos
dotfiles é voltada para configurá-lo. O ambiente de terminal tem vários
componentes, então vamos separá-lo por partes:

### Shell

Escolhi o ZSH na época por conta do [Oh My Zsh](https://ohmyz.sh/) (ou OMZ), que
facilitou bastante num primeiro momento em que eu tinha pouco conhecimento do
ambiente de terminal, mas gostaria de customizá-lo mais facilmente. Hoje, eu
vejo ele como algo que adiciona funcionalidades de mais, e gostaria de
removê-lo, mas reconheço que ele foi uma ótima ferramenta para iniciar minhas
customizações no terminal

### Multiplexador

Conseguir dividir o terminal é praticamente essencial quando estamos falando de
produtividade nele, logo escolhi o [tmux](https://github.com/tmux/tmux/wiki), e
posteriormente o [byobu](https://www.byobu.org/) para esse propósito.

### Emulador

Por muito tempo, utilizei o [kitty](https://sw.kovidgoyal.net/kitty/) como meu
emulador principal. Porém, ele conta com algumas funcionalidades extras que eu
não utilizava, porque já utilizava o byobu. Estou há algum tempo testando o
[alacritty](https://alacritty.org/) que é um emulador voltado para simplicidade.
Não notei nenhuma diferença, então a decisão de utilizar um ou outro está em
aberto ainda.

## Neovim

Continuando com a ideia de morar no terminal, o outro monstrinho dos meus
dotfiles são as configurações do neovim. Assim como o WM, é uma ferramenta com
uma curva de aprendizado um pouco íngreme, mas que hoje me dá uma produtividade
maior

# Resultados esperados

Aqui eu explico o que eu pretendo com essa reestruturação dos meus dotfiles e o
que eu **não** pretendo fazer nesse primeiro momento

## Objetivos

* Manter algumas premissas originais dos meus dotfiles, que são:
    * Utilizar ferramentas open-source sempre que possível
    * Gruvbox em tudo (ter cores consistentes em todo o meu ambiente me traz uma
      sensação de paz)
* Criar novos dotfiles que sejam fáceis de manter e fazer o setup a partir de
  uma instalação nova de Ubuntu e Arch Linux
    * Trocar os scripts de setup existentes por algo que seja um pouco mais
      fácil de manter (Ansible?)
* Remover o máximo de configurações adicionais que não são utilizadas ou foram
  copiadas de outro lugar sem o devido cuidado de entendê-las. Lista com algumas
  configs que entram nessa categoria:
    * OMZ
    * byobu/tmux
    * i3config
    * neovim

## Não-Objetivos

Essa seção está aqui pra me lembrar de manter o foco, uma vez que eu acabo me
empolgando mais vezes do que eu gostaria e desvio do propósito original da coisa

* Dar suporte pra Fedora, ou qualquer outra distro
* Trocar o i3 por sway (queria, porém sem tempo por enquanto)

