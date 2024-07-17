# Purge

O primeiro passo pra fazer esse refactor todo é remover todos os dotfiles da
minha máquina. Isso normalmente é resolvido com uma formatação, PORÉM eu tenho o
costume de criar volumes separados para o `/` (root), que tem todos os arquivos
de sistema e a `/home`, que tem meus arquivos pessoais (incluindo meus dotfiles)

A consequência disso é que eu consigo formatar meu sistema sem perder meus
arquivos pessoais, mas nesse caso específico que eu quero remover os dotfiles,
eu teria que formatar a partição da home também, o que também não é legal uma
vez que eu salvo alguns arquivos importantes nela, como meu histórico de
comandos. O jeito é remover tudo manualmente mesmo antes de formatar `¯\_(ツ)_/¯`

Aqui eu tive que tomar um certo cuidado para remover o suficiente para eu
conseguir fazer meu reset, mas ainda ter um conjunto de ferramentas minimamente
funcional (um terminal, um browser e o i3, basicamente). Removi bastante lixo
que foi se acumulando com os anos de reaproveitamento da minha home, mas a parte
mais importante foi remover todas as configs relacionadas a:

* zsh e Oh-My-Zsh
* Byobu + Tmux
* Neovim

As configs relacionadas ao i3 foram mantidas por enquanto, mas serão refeitas
futuramente. Eu não queria ter o trabalho de reconfigurar tudo nesse primeiro
momento.

# Fresh Start e mudança de planos

Depois de fazer a instalação nova do Arch seguindo o guia de 
[instalação](https://wiki.archlinux.org/title/Installation_guide) e
[pós-instalação](https://wiki.archlinux.org/title/General_recommendations),
as primeiras coisas foram instalar o mínimo necessário pra conseguir fazer login
com o meu usuário e cair direto no i3. Os detalhes de como fazer isso são bem
interessantes, mas vamos deixar isso para um momento futuro.

Depois de instalar os pacotes mínimos, temos algo parecido com isso:
![Um setup mínimo de i3](img/minimal-i3.png)

Como dá pra ver na imagem acima, o zsh está sem nenhum tipo de customização.
Acabei optando pelo kitty como emulador de terminal ao invés do Alacritty
simplesmente porque é mais fácil de instalar o kitty. A diferença de performance
entre os 2 não é grande a ponto de ser perceptível para mim.

Depois de alguns ajustes pequenos e algumas tentativas frustradas de customizar
o zsh sem o OMZ, percebi que daria um trabalho enorme até chegar no estado
anterior do meu terminal. E como eu não queria investir muito tempo nisso,
resolvi mudar a estratégia de remover o Oh-My-Zsh.

# Porque remover o Oh-My-Zsh?

Um dos problemas da configuração do meu terminal anterior era o tempo de
startup terrivelmente alto (> 1s). Com o tempo, fui adicionando plugins no OMZ
sem muito pudor e em algum momento o conjunto de plugins que eu utilizava
acabou causando esse resultado.

A solução inicial que pensei foi remover o OMZ e reinstalar somente o necessário
sem ele, mas como dito acima, daria mais trabalho do que eu estava disposto a
investir. Então, mudei a abordagem: Ao invés de removê-lo, instalar novamente e
readicionar as features aos poucos.

# Chezmoi

Estamos quase prontos pra começar a readicionar os dotfiles, e para
gerenciá-los, o chezmoi foi o escolhido, então precisamos fazer o setup inicial.
Seguindo o quickstart da própria
[documentação](https://www.chezmoi.io/quick-start/) dele, temos nosso estado
(quase) inicial:

```sh
$ tree -a -C -I '.git'
.
├── dot_zshrc
├── executable_dot_xprofile
└── .gitignore

1 directory, 3 files
```

Notem que não está completamente vazio porque eu incluí 2 dotfiles de exemplo,
mas o setup inicial deve te deixar com uma pasta vazia. Feito isso, temos mais
um problema para resolver

# Dependência externa

Quando instalamos o OMZ, nosso `.zshrc` foi modificado, criando uma dependência
entre os dois. Ou seja, nosso `.zshrc` não vai mais funcionar de forma adequada
se não tivermos o OMZ nos nossos dotfiles. O chezmoi consegue resolver esse
problema pra nós [incluindo arquivos
externos](https://www.chezmoi.io/user-guide/include-files-from-elsewhere/)
durante o setup. Assim, ao rodar o setup inicial em outra máquina, ele
instala o OMZ diretamente do repo no GitHub. Bem conveniente, então vamos criar
um arquivo `.chezmoiexternal.yaml`:

```yaml
.oh-my-zsh:
  type: archive
  url: 'https://github.com/ohmyzsh/ohmyzsh/archive/master.tar.gz'
  exact: true
  stripComponents: 1
  refreshPeriod: 720h # 30 days
```

# Finalizando

Feito todo esse setup, finalmente estamos prontos para adicionar os dotfiles. O
próximo passo será customizar nosso [terminal](./02-terminal.md)
