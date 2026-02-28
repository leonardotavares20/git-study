# Git

git é um sistema de controle de versão, que permite que você rastreie e controle as alterações dos arquivos no seu projeto.

## Repositórios

- ```git init``` -- inicializa um repositório vazio, no diretório atual, ele cria a pasta .git que armazena todos os dados do repositório, commits, branches, histórico e etc.

- ```git status``` -- mostra o status atual de um arquivo, podendo ser untracked, staged ou committed, você pode passar um arquivo ou diretório como argumento para ver o status de um arquivo específico. A resposta retornada pro exemplo: ``git status git.md``

```bash
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	git.md

nothing added to commit but untracked files present (use "git add" to track)
```

- ```git add``` -- adiciona um arquivo ou diretório ao stage, ele marca o arquivo para ser incluído no próximo commit,  você pode passar um arquivo ou diretório como argumento para adicionar um arquivo específico. Você também pode somente passar logo após o comando ```git add``` um ```.``` pra adicionar todos os arquivos não trackeados. A resposta retornada pro exemplo: ```git status git.md```, após o comando ```git add git.md``` 

```
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   git.md

```

- ```git commit -m``` -- um commit é como uma fotografia do seu repositorio, em um determinado ponto no tempo. Git não armazena as diferenças, o git guarda todo histórico por commit. Pra realizar um commit, você deve executar o comando ```git commit -m "mensagem do commit"```. A mensagem retornada é:

```bash
[master (root-commit) 8379dc0] add mod git.md
 1 file changed, 17 insertions(+)
 create mode 100644 git.md
```

logo após o commit, rodando o comando ```git status```

```bash
On branch master
nothing to commit, working tree clean
```

logo após o commit, rodando o comando ```git log --oneline```

```bash
38d407a (HEAD -> master) add mod git.md
8379dc0 add git.md
```

- ```git commit --amend``` -- se você errou a mensagem do seu ultimo commit, você pode corrigir com o comando ```git commit --amend -m "nova mensagem"```

- ```git log``` -- em um projeto provavelmente você vai ter uma longa lista de commits. Pra visualizar esse historico de commits, você pode usar o comando ```git log```, que mostra uma lista de commits com informações como data, autor e mensagem. Você também pode usar o comando ```git log --oneline``` para mostrar apenas a mensagem e o hash do commit.

```bash
commit 33ff827d4c7414a93a9f03e8cd29016cd256bb2f (HEAD -> master)
Author: Leonardo Tavares <leo@gmail.com>
Date:   Tue Jan 20 15:31:02 2026 -0300

    add mod two git.md

commit 38d407a67582cdaeac5fcc7436957e9fc813d72c
Author: Leonardo Tavares <leo@gmail.com>
Date:   Tue Jan 20 15:14:00 2026 -0300

    add mod git.md

commit 8379dc0a3524a934c1cb4483a6ac8406d18d3bd0
Author: Leonardo Tavares <leo@gmail.com>
Date:   Tue Jan 20 15:12:28 2026 -0300

    add git.md
:
```

### Variações:

- ```git log --no-pager log -n 10``` -- limita o maximo de commits a serem exibidos, - por exemplo, ```git --no-pager log -n 10``` limita a exibição de 10 commits.

- ```git log --no-pager log -n 10 --oneline``` -- mostra apenas a mensagem e o hash do commit com a limitação de 10 commits.

- ```git log --no-pager log -n 10 --oneline --graph``` -- mostra apenas a mensagem e o hash do commit com a limitação de 10 commits e um grafico de commits.

- ```git log --no-pager log -n 10 --oneline --parents``` -- mostra apenas a mensagem e o hash do commit com a limitação de 10 commits e os pais do commit.

## Internals

todos os dados em um repositorio git são armazenados em uma object database, localizada dentro do diretório ```.git```.
Nesse diretório tem um conjunto de arquivos que representam os objetos do Git, como blobs(arquivos), trees(diretorios), commits e tags.

- ```git cat-file -p <hash>``` - permite visualizar o conteudo de um commit diretamente.

### Trees e Blobs:

- tree é a maneira do git armazenar um diretório.
- blob é a maneira do git armazenar um arquivo.

o fluxo pra você ver o conteudo de um arquivo com ``git cat-file`` é:

```bash
git log --oneline
git cat-file -p <hash do tree do commit>
git cat-file -p <hash do blob do arquivo>
```


### Storing Data

Git armazena um snapshot inteiro dos arquivos `per-commit` level. E não somente as mudanças que você fez em cada commit. E sim como que um status do repositorio como um todo no momento do commit. Mas ele não armazenas os arquivos em si, ele armazena um ponteiro para o arquivo, ele armazena todas as referencias daquele arquivo.

- Git comprime e embala(faz um pack dos arquivos) para armazenar esses arquivos de uma forma mais eficiente.
- Git duplica arquivos que tem mais de um commit que referencia eles. Se um arquivos não tem uma mudança entre os commits, o Git armazena apenas uma cópia do arquivo.

## Git config

Git armazena informações sobre o autor, então quando você faz um commit, o Git pode trackear quem fez aquela mudança ou commit. Para atualizar suas configurações globais do git você pode usar: 
- ```git config --add global user.name "Seu Nome"```
- ```git config --add global user.email "Seu Email```

### Significado das configurações

- ```git config ``` - o comando para interagir com suas configurações do Git.
- ```--add``` - Flag que marca que você quer adicionar uma nova configuração.
- ```--global``` - Flag que marca que você quer adicionar uma nova configuração global no seu diretório ```~/.gitconfig```. Em contrario disso se você utilizar a flag ```local```, as configurações vão ser armazenadas somente no repositório atual.
- ```user``` - a seção
- ```name``` - a chave
- ```email``` - a chave

### List

- Você pode usar o comando ```--list``` - para listar todas as configurações do Git. 
```bash
git config --list
```
- Você também pode adicionar a flag ```--local``` para listar as configurações locais do repositório atual.
```bash
git config --list --local
```

### GET

Você pode usar o comando ```--list``` para ver as configurações de todos os valores, mas a flag ```--get``` é util para ver um valor específico de uma configuração.

- ```--get``` - Flag que retorna o valor de uma configuração específica.
- ```git config --get --local <key>```

Exemplo: ```git config --get --local user.name```

### UNSET

Você pode usar o comando ```--unset``` para remover uma configuração específica.

- ```--unset``` - Flag que remove uma configuração específica.
- ```git config --unset --local <key>```

Exemplo: ```git config --unset --local user.name```

- ```bash--unset-all <key>``` - Flag que remove todas as instâncias de uma key da sua configuração.
- exemplo: ```bash--unset-all example.key```

### Duplicates

Geralmente, em um key/value pair, como [Python Dictionaries](https://www.w3schools.com/python/python_dictionaries.asp), você não pode ter chaves duplicadas. Mas estranhamente o GIT não se importa com isso e permite chaves duplicadas.

Então mesmo se você já tiver uma chave com um valor, você pode adicionar outra chave com o mesmo nome e um valor diferente.

Exemplo:
```bash
git config --add --local user.name "Dev 001"
git config --add --local user.name "001 Dev"
```
A sua saída para ```git config --list --local``` seria algo como:

```bash
core.repositoryformatversion=0
core.filemode=false
core.bare=false
core.logallrefupdates=true
core.ignorecase=true
user.name=001 Dev
user.name=Dev 001
```

Então ele vai sempre pegar o último valor da chave configurada.

### Sections

Uma seção é basicamente um grupo de configurações relacionadas, como [user, core, alias, remote].

Diferentemente de uma key que é uma variável específica, que você pode configurar com um value dentro de uma seção.

```bash
[user]                     <-- Isso é a SECTION (Seção)
    name = Maria Silva     <-- "name" é a KEY (Chave), "Maria Silva" é o Valor
    email = maria@email.com<-- "email" é a KEY (Chave)

[core]                     <-- Isso é outra SECTION
    editor = vim           <-- "editor" é a KEY
```

Você pode remover uma seção inteira da sua configuração usando o comando: 
```bash
git config --remove-section section
```

### Locations

Existem vários lugares onde o Git pode ser configurado, desde o arquivo de configuração global até o arquivo de configuração local do repositório.

- **system**: ```etc\gitconfig```, um arquivo que configura o Git para todos os usuários do sistema.
- **global**: ```~/.gitconfig```, um arquivo qu configura o Git para todos os projetos de um usuários específico 
- **local**: ```.git/config```, um arquivo que configura o Git para um repositório específico.
- **worktree**: ```.git/config.worktree```, um arquivo que configura o Git para uma parte de um projeto.

Provavelmente em 90% do tempo você deva usar o arquivo de configuração global(```--local```) para configurar suas preferências pessoais como nome e email, e 9% para o arquivo de configuração local(```--local```) para configurar suas preferências de repositório na maioria do tempo. 1% para o arquivo de configuração worktree(```--worktree```), mas é bem raro.

Se você configurar um arquivo numa location específica, essa configuração vai substituir a configuração de uma location geral. Por exemplo, se você configurar ```user.name``` na configuração local, ele vai substituir o ```user.name``` na configuração global. Ou seja quanto mais específico, mais ele substitui a configuração geral anterior.

![Representação visual das locations e overrides do git](./img/config.png)