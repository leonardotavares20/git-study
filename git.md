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

- ```git add``` -- adiciona um arquivo ou diretório ao stage, ele marca o arquivo para ser incluído no próximo commit,  você pode passar um arquivo ou diretório como argumento para adicionar um arquivo específico. Você também pode somente passar logo após o comando ```git add``` um ```.``` pra adicionar todos os arquivos não trackeados, arquivos modificados e arquivos deletados. A resposta retornada pro exemplo: ```git status git.md```, após o comando ```git add git.md``` 

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

- ```git --no-pager log -n 10``` -- limita o maximo de commits a serem exibidos, - por exemplo, ```git --no-pager log -n 10``` limita a exibição de 10 commits.

- ```git --no-pager log -n 10 --oneline``` -- mostra apenas a mensagem e o hash do commit com a limitação de 10 commits.

- ```git --no-pager log -n 10 --oneline --graph``` -- mostra apenas a mensagem e o hash do commit com a limitação de 10 commits e um grafico de commits.

- ```git --no-pager log -n 10 --oneline --parents``` -- mostra apenas a mensagem e o hash do commit com a limitação de 10 commits e os pais do commit.

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

Git armazena um snapshot inteiro dos arquivos `per-commit` level. E não somente as mudanças que você fez em cada commit. E sim como que um status do repositorio como um todo no momento do commit. Mas ele não armazena múltiplas cópias do mesmo arquivo se ele não mudou.

- Git comprime e embala(faz um pack dos arquivos) para armazenar esses arquivos de uma forma mais eficiente.
- Se os arquivos não tem mudanças entre os commits, o Git armazena apenas uma cópia do arquivo.

## Git config

Git armazena informações sobre o autor, então quando você faz um commit, o Git pode trackear quem fez aquela mudança ou commit. Para atualizar suas configurações globais do git você pode usar: 
- ```git config --add --global user.name "Seu Nome"```
- ```git config --add --global user.email "Seu Email"```

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

- ```git config --unset-all <key>``` - Flag que remove todas as instâncias de uma key da sua configuração.
- exemplo: ```git config --unset-all <key>```

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

Provavelmente em 90% do tempo você deva usar o arquivo de configuração global(```--global```) para configurar suas preferências pessoais como nome e email, e 9% para o arquivo de configuração local(```--local```) para configurar suas preferências de repositório na maioria do tempo. 1% para o arquivo de configuração worktree(```--worktree```), mas é bem raro.

Se você configurar um arquivo numa location específica, essa configuração vai substituir a configuração de uma location geral. Por exemplo, se você configurar ```user.name``` na configuração local, ele vai substituir o ```user.name``` na configuração global. Ou seja quanto mais específico, mais ele substitui a configuração geral anterior.

![Representação visual das locations e overrides do git](./img/config.png)

## HEAD

No Git, o `HEAD` é um ponteiro especial que indica qual é o commit atual no seu diretório de trabalho. Você pode pensar nele como a sua "posição atual" ou "onde você está agora" no histórico do repositório.

Em situações normais, o `HEAD` aponta para a branch local em que você fez checkout (por exemplo, `main` ou `master`). Esta branch, por sua vez, aponta para o commit mais recente feito nela.

### Detached HEAD (HEAD Desanexado)

Se você usar o comando de alternância diretamente para um commit específico (usando o hash desse commit) em vez de usar o nome de uma branch, você entrará em um estado conhecido como **Detached HEAD**.

```bash
# Alternando diretamente para um commit
git switch --detach 33ff827
# Ou no método clássico:
git checkout 33ff827
```

Nesse estado de "HEAD desanexado", o `HEAD` aponta diretamente para um commit específico e não para uma branch. Qualquer novo commit que você fizer a partir daqui não pertencerá a nenhuma branch visível e poderá ser perdido (apagado pelo "garbage collector" do Git) futuramente quando você mudar para outra branch ativa.

Caso você faça modificações nesse estado e deseje salvá-las, precisará criar uma nova branch apontando para o seu `HEAD` atual:

```bash
git switch -c nome-da-nova-branch
```

## Branches

Uma branch no git te permite manter o track de diferentes mudanças no seu projeto separadamente.

Por exemplo, vamos dizer que você tem um grande projeto, no qual você quer experimentar uma nova paleta de cores. Em vez de mudar o projeto inteiro diretamente, você pode criar uma nova branch chamada ```nova-paleta``` e fazer as alterações lá.  Isso permite que você teste a nova paleta sem afetar o código principal do projeto. Quando você terminar, se estiver satisfeito com as mudanças, você pode mesclar (```merge```) a branch ```nova-paleta``` com a branch principal do projeto. Se você não estiver satisfeito, você pode simplesmente descartar a branch ```nova-paleta``` e continuar trabalhando na branch principal.

### O que é uma branch?

Na tradução literal, branch significa "ramo". No desenvolvimento de software ela tem o mesmo significado, branch é uma ramificação(ramo) do seu projeto.

Uma branch nada mais é, que um ponteiro nomeado para um commit específico. Quando você cria uma branch, você está criando um novo ponteiro para um commit específico.

Devido a branch ser só um ponteiro de um commit, criar uma nova branch é algo que não tem um custo elevado em performace, pelo contrário, elas são "leves" e "baratas". Quando você cria 10 branches, você não está criando 10 copias do seu projeto no disco do seu computador, apenas um ponteiros para um commit específico.

```branch tips``` - é o último commit de uma branch, ou seja o ponto mais recente no qual você estava trabalhando.

![git branch](./img/branch.png)

### Trabalhando com branches

- Default branch

A branch padrão é a branch que é criada quando você inicializa um repositório com ```git init```. Ela é chamada de ```main``` ou ```master```. Se você estiver trabalhando com github, é recomendado usar ```main``` como branch padrão.

- Checando a branch atual

Para checar qual a sua branch atual, você pode usar o comando ```git branch``` sem nenhum argumento. Isso mostrará todas as branches existentes no seu repositório local, com a branch atual marcada com um asterisco.

```bash
git branch
```
- Criando uma nova branch

Para criar uma nova branch, você pode usar o comando ```git branch``` seguido do nome da branch que você deseja criar. Por exemplo:

```bash
git branch nova-branch
```

Isso criará uma nova branch chamada ```nova-branch``` que aponta para o mesmo commit que a branch atual.

**Se você quiser criar uma branch e ir diretamente pra ela, você pode simplismente usar o comando:**

```bash
git switch -c nova-branch
```

Quando você cria uma nova branch, ela usa o commit atual como ponto de partida. Por exemplo, se você estiver na branch ```main``` e tiver commits ```A```, ```B``` e ```C```, e você criar uma nova branch ```nova-branch```, ela apontará para o commit ```C```.

Exemplo de como sua branch deve ficar:

```
A - B - C
         \
          nova-branch
```

- Alternando para uma branch existente

Para alternar para uma branch existente, você pode usar o comando ```git switch``` seguido do nome da branch que você deseja alternar. Por exemplo:

```bash
git switch nova-branch
```

Isso alternará para a branch ```nova-branch``` e atualizará seu diretório de trabalho para refletir as mudanças nessa branch.

- Excluindo uma branch

Para excluir uma branch, você pode usar o comando ```git branch -d``` seguido do nome da branch que você deseja excluir. Por exemplo:

```bash
git branch -d nova-branch
```

Isso excluirá a branch ```nova-branch``` do seu repositório local.

- Renomeando uma branch

Para renomear uma branch, você pode usar o comando ```git branch -m``` seguido do nome antigo e do novo nome da branch. Por exemplo:

```bash
git branch -m nome-antigo-branch novo-nome-branch
```

Isso renomeará a branch ```nova-branch``` para ```branch-renomeada```.

### Visualizando branches

O git tem a sua forma de visualizar branches por meio de texto. Por exemplo:

```bash
A - B - C  main
```

Isso significa que o commit ```C``` é o último commit na branch ```main```, e os commits ```A``` e ```B``` são seus commits anteriores.

Para representar multiplas branches, o git usa uma notação de linha de tempo. Por exemplo:

```bash
A - B - C  main
     \
      D - E  feature
```

Isso significa que a branch ```feature``` foi criada a partir da branch ```main```, tendo como ponteiro o commit ```B```, e tem dois commits adicionais ```D``` e ```E```.

- Git Files

Lembrando que o Git guarda todas as informações do seu projeto, incluindo branches, commits e arquivos, no subdiretório ```.git``` no root do seu projeto. Os "heads" das branches são armazenados no diretório ```.git/refs/heads/```. Se você acessar um desses arquivos, você verá o hash do commit que é o ponteiro da branch.

## Merge

Quando você está trabalhando em uma branch, seja uma nova feature, a correção de um bug ou qualquer outro tipo de coisa, em algum momento se estiver satisfeito com o resultado, você provavelmente vai querer mesclar (merge) essa branch com a branch principal (geralmente ```main``` ou ```master```). O merge é o processo de unir as alterações de uma branch com outra.

Então vamos dizer que você está em um estado que você tem duas branches, cada uma com commits únicos.

```bash
A - B - C - main
     \
      D - E  feature
```

Se você mergear a branch ```feature``` com a branch ```main```, o Git combina as duas branches, criando um novo commit que tem o histórico das duas branches, no diagrama ```F``` é um ````merge commit````, que tem ```C``` e ```E``` como parentes. O commit ```F``` traz todas as mudanças dos commits ```D``` e ```E``` para a branch ```main```:

```bash
A - B - C - F main
     \     /
      D - E  feature
```

### Merge Commits

Quando estamos mergeando duas branches com históricos divergentes, assim como no exemplo abaixo

```bash
A - B - C - main
     \
      D - E  feature
```

o Git cria um merge commit que é o único com dois parentes. 
- **1** - Primeiro é necessário encontrar o ```merge base``` aka ```best common ancestor```, é o ancestral mais recente que as duas branches tem em comum no exemplo acima é o commit ```B```. 
- **2** - Em seguida o Git coloca os commits da branch ```main``` no ```merge commit```, e após isso os commits da branch ```feature``` são adicionados. 
- **3** - Quando todas as mudanças são incorporadas, o merge commit é criado e colocado na branch ```main```. Nesse caso seria o commit ```F```. Ele tem dois parentes apontando para os commits ```C``` e ```E```.

```bash
A - B - C - F main
     \     /
      D - E  feature
```

Para mergear a branch  ```feature``` com a branch ```main```, você pode usar o comando ```git merge```:

```bash
git merge feature
```

### Merge Log

Se você quiser visualizar o log de um merge commit, de forma mais detalhada, você pode utilizar o comando ```git log```, e usar os parâmetros ```--oneline --graph --decorate --parents```:

- ```--oneline``` - retorna uma visão condensada do histórico de commits, os hashes dos commits são abreviados para 7 caracteres, que é a quantia mínima que o git requer para especificar um hash.
- ```--graph``` - desenha todas as linhas do grafo de commits, mostrando como os commits divergiram, e retornaram a linha principal por meio de um merge commit.
- ```--decorate``` - mostra rótulos de branches e tags
- ```--parents``` - mostra os parentes de cada commit

```bash
git log --oneline --graph --decorate --parents
```

### Fast Forward Merge

O tipo mais simples de merge é o "fast forward merge".

Vamos dizer que você vai adicionar uma feature no projeto:

```bash
A - B - main
     \    
      C -  feature
```

E você rodar o comando:

```bash
git merge feature
```

Por causa que a branch `feature` tem todos os commits que a branch `main` tem, o Git pode simplesmente avançar a branch `main` para o commit mais recente da branch `feature`, sem criar um merge commit. Isso é chamado de "fast forward merge".

Ele automaticamente faz um ```fast forward merge```. Ele simplismente move o ponteiro da base da branch `main` para o commit mais recente da branch `feature`.

Quando o merge é feito em uma branch que está atualizada com a branch de destino, o Git pode simplesmente avançar a branch para o commit mais recente da branch de destino, sem criar um merge commit. Isso é chamado de "fast forward merge".

Então o diagrama final seria:

```bash
A - B - C - main
```

Porque o Git fez um fast forward merge, ele não criou um merge commit.

## Rebase

O rebase é uma operação do git que te permite reaplicar os commits de uma branch, para o ```tip```, ou a ponta da branch de destino. Diferente do ``merge``, o rebase não cria um merge commit. Então o histórico da branch de destino fica mais linear, justamente porque ele não adiciona uma nova camada de commits, ele simplesmente "reescreve" o histórico  de commits. O histórico ficaria parecido com isso:

Antes:

```bash
A - B - C - main
     \    
      D - E  feature
```

Depois de rodar `git rebase main` na branch `feature`:

```bash
A - B - C - main
          \ - D' - E'  feature
```

Nesse processo o git:
- 1 - encontra o ponto em comum entre as duas branches(merge base)
- 2 - pega os commits da branch atual
- 3 - move temporariamente o ponteiro
- 4 - reaplica esses commits para o topo da branch de destino

Uma vantagem do merge é que ele preserva o histórico verdadeiro do projeto. Ele mostra quando, onde e como as branches foram mergeadas. Uma desvantagem é que isso cria uma grande quantidade de commits, o que pode tornar o histórico mais difícil de ler e entender.

Um histórico linear geralmente é mais facil de ler, entender e trabalhar com.