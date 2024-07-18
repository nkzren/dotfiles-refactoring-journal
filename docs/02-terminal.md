# Terminal

Feitos os preparativos, podemos efetivamente começar a customizar o terminal e
adicionar os dotfiles correspondentes

Partindo da config inicial do OMZ, a primeira customização a ser feita foi
alterar o `ZSH_CUSTOM` para um caminho que não seja um subpath do
`~/.oh-my-zsh`. Dessa forma, conseguimos rodar o setup do chezmoi mesmo em
cenários onde já temos o OMZ instalado. Na minha experiência, isso tende a dar
alguns conflitos que precisam ser resolvidos manualmente e é exatamente isso que
não queremos já que a premissa aqui é facilitar o setup dos dotfiles em qualquer
ambiente

E como um dos problemas que eu tinha antes era o tempo de startup do terminal,
nada mais justo que monitorar esse tempo. Dá pra fazer isso deixando um terminal
aberto rodando o seguinte comando:

```sh
watch 'time zsh -i -c exit'
```

## Tmux (e adeus byobu :sad:)

O byobu que eu utilizava antes nada mais é do que um wrapper sobre o tmux
adicionando algumas funcionalidades, como keybinds um pouco mais intuitivas
pra quem não está acostumado com o tmux e alguns menus e utilitários extras de
configuração. Ele me ajudou a transicionar para um multiplexador de terminal no
começo, mas eu não vejo mais necessidade para ele uma vez que eu uso minha
própria customização do tmux

O tmux é configurado pelo arquivo `.tmux.conf`. A config completa fica
[aqui](https://github.com/nkzren/dotfiles/blob/main/dot_tmux.conf) se você
quiser ver em detalhes, mas ressalto alguns plugins abaixo

### Tmux Plugin Manager (TPM)

Praticamente obrigatório pra qualquer pessoa que customiza o tmux, o tpm
facilita bastante a instalação de outros plugins, então geralmente é o primeiro
que você deveria instalar. Você pode instalar ele manualmente seguindo o [README
do repositório](https://github.com/tmux-plugins/tpm?tab=readme-ov-file#installation),
mas como eu estou usando o chezmoi, basta adicionar o bloco abaixo no
`.chezmoiexternal.yaml`:

```yaml
.tmux/plugins/tpm:
  type: archive
  url: 'https://github.com/tmux-plugins/tpm/archive/master.tar.gz'
  exact: true
  stripComponents: 1
  refreshPeriod: 168h # 7 days
```

Em seguida, basta adicionar o bloco abaixo no final do `.tmux.conf`

```tmux
# List of plugins
set -g @plugin 'tmux-plugins/tpm'

# Initialize TMUX plugin manager (keep this line at the very bottom of tmux.conf)
run '~/.tmux/plugins/tpm/tpm'
```

Feito isso, você pode recarregar a config do tmux com `tmux source
~/.tmux.conf` e adicionar novos plugins

### [tmux-plugins/tmux-sensible](https://github.com/tmux-plugins/tmux-sensible)

A ideia desse plugin é ter um conjunto de configurações que sejam aceitáveis
para qualquer usuário de tmux. Você pode conferir a lista completa no readme do
repo

### [alexwforsythe/tmux-which-key](https://github.com/alexwforsythe/tmux-which-key)

Esse plugin te dá um menu para navegar e manipular as sessões, janelas e painéis
do tmux. Recomendo principalmente pra quem está começando a utilizar o tmux
agora, porque evita a necessidade de memorizar diversos conjuntos de keybinds
que talvez você nem use

### [tmux-plugins/tmux-open](https://github.com/tmux-plugins/tmux-open)

Facilita a abertura de arquivos e URLs enquanto você navega pelo vi-mode do
tmux. Com ele você também consegue selecionar texto e buscar direto em algum
mecanismo de busca de sua preferência

## OMZ

```zsh
plugins=(
	aliases
	asdf
	autojump
	docker
	docker-compose
	fzf
	git
	kube-ps1
	kubectl
	ripgrep
	sudo
	tmux
	z
	zsh-vi-mode
)
```

