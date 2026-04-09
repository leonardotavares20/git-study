# Git

Git é um sistema de controle de versão, que permite que você rastreie e controle as alterações dos arquivos no seu projeto.

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

## Staging Area

O "Staging Area" (ou área de preparação) é um passo intermediário entre o seu diretório de trabalho (onde você edita os arquivos) e o repositório Git (onde os commits são armazenados). Quando você modifica arquivos, o Git não os inclui automaticamente no próximo commit. Primeiro, você deve adicioná-los ao staging area usando o comando `git add`.

 ```git add``` -- adiciona um arquivo ou diretório ao stage, ele marca o arquivo para ser incluído no próximo commit,  você pode passar um arquivo ou diretório como argumento para adicionar um arquivo específico. Você também pode somente passar logo após o comando ```git add``` um ```.``` pra adicionar todos os arquivos não trackeados, arquivos modificados e arquivos deletados. A resposta retornada pro exemplo: ```git status git.md```, após o comando ```git add git.md``` 

```
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   git.md

```

### Removendo um arquivo do Staging Area

Se você adicionou um arquivo ao staging area por engano (usando `git add`), mas ainda não fez o commit, você pode removê-lo de lá (fazer o *unstage*) sem perder as modificações que você fez no arquivo em si.

Para remover um arquivo do staging area, você pode usar o comando `git restore`:

```bash
git restore --staged <nome-do-arquivo>
```

*(Nota: O próprio comando `git status` costuma sugerir esse comando quando há arquivos no stage.)*

Isso tira o arquivo da área de preparação, mudando seu status de volta para modificado (ou untracked, se for um arquivo novo), mas **mantém todas as alterações** feitas no seu disco. 

Se você quiser descartar as alterações do arquivo de forma definitiva (apagar o que editou e voltar pro estado do último commit), você remove a flag `--staged` e o comando seria `git restore <nome-do-arquivo>`.

## Commit

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

## Git Log

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

- ```git log -p``` -- **mostra o historico de commits com as mudanças feitas em cada commit.**


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

## Reset

Um dos grandes benefícios do git é a capacidade de desfazer ações.  O comando ```git reset``` é uma das ferramentas mais poderosas para isso. Ele pode ser usado para desfazer o último commit ou qualquer mudança, seja ela no staging area ou no working directory.

### Git Reset Soft

A opção ``--soft``, é útil quando você quer desfazer seu último commit, mas ainda manter todas as mudanças que você fez no staging area. Mudanças que foram commitadas vão ser desfeitas e colocadas no staging area, enquando mudanças que ainda não foram commitadas, vão permanecer como staged ou unstaged.

```bash
git reset --soft HEAD~1
```

- HEAD~1 é uma referência do commit anterior ao commit atual.

Então você desfaz as mudanças, enquanto ainda as mantém disponíveis para serem commitadas novamente.

### Git Reset Hard

Diferente do ``--soft``, o ``--hard`` desfaz as mudanças dos commits, e remove as mudanças do staging area e do working directory que você tem. Então é como se você nunca tivesse feito as mudanças. Ele só não aplica o reset se em arquivos que não estão trackeados na worktree.

Então é útil se você simplismente quiser voltar pra um commit anterior e descartar todas as mudanças que você fez desde então. Ele vai limpar seu index e também seu worktree.

```bash
git reset --hard HEAD~1
```

ou

```bash
git reset <hash do commit>
```

``git reset --hard`` é um comando poderoso, mas também **perigoso** pois ele vai apagar todas as mudanças que você fez desde o commit que você está resetando, inclusive as que não foram commitadas ainda, fora os arquivos que não estão trackeados na worktree. Então é bom tomar cuidado e ter certeza que você quer fazer isso.

## Remote

Quando você está trabalhando em um projeto, é comum que você queira compartilhar seu trabalho com outras pessoas. Para isso, você pode usar o git remote. O git remote é uma ferramenta que permite que você gerencie as conexões entre o seu repositório local e os repositórios remotos. Eles são repositórios externos com o mesmo histórico do git do seu repositório local.
Esses repositórios não necessariamente precisam estar hospedados em um Github da vida, pode ser um repositório local, ou em um servidor próprio por exemplo.


### Adicionando um Remote

Quando adicinamos um remote, e tratamos ele como "a fonte da verdade", ele é chamado de **origin**.

Como "fonte da verdade", isso quer dizer que você e o seu time vão tratar ele como o repositório verdadeiro ou principal. É onde tem a maioria do código final e aceito pelo time.

Para adicionar um remote, você pode usar o comando ```git remote add```:

```bash
git remote add origin <uri>
```

OBS: se você adicionar a opção ``-u``, você vai estar dizendo para o git que você quer que essa branch seja rastreada pela branch remota. Ou seja, quando você rodar ``git pull`` ou ``git push``, o git vai saber qual branch remota ele deve usar. E você não vai precisar mais digitar ``git pull origin main`` ou ``git pull origin master``, basta digitar ``git pull`` ou ``git push``.

```bash
git remote add -u origin <uri>
```

### Fetch

Adicionar um repositório remoto é uma coisa, mas o que acontece se esse repositório remoto for atualizado? Você precisa atualizar o seu repositório local com as mudanças do repositório remoto. Para isso, você pode usar o comando ```git fetch```:

```bash
git fetch origin
```

Esse comando vai baixar todas as mudanças do repositório remoto de ```.git//objects``, mas não vai aplicar nenhuma mudança no seu repositório local. Ele só vai atualizar os ponteiros das branches remotas.

### Log Remote

O comando ``git log`` pode ser usado também pra mostrar o histórico de commits de um repositório remoto também. Por exemplo:

```bash
git log remote/branch
```

Por exemplo se você quiser ver os commits de uma branch chamada feat/login, você pode usar o comando:

```bash
git log origin/feat/login
```

### Merge

Assim como nós podemos mergear branches locais, nós também podemos mergear branches locais com branches remotas. Por exemplo:

```bash
git merge remote/branch
```

Mais uma vez com o exemplo da branch ```feat/login```:

```bash
git merge origin/feat/login
```

## Github

Enquanto o **Git** é a ferramenta (o motor) que gerencia as versões do seu código localmente, o **GitHub** é uma plataforma de hospedagem de código na nuvem construída em cima do Git. Ele permite que você armazene seus repositórios Git online, colabore com outros desenvolvedores, faça backup dos seus projetos e compartilhe seu código (de forma pública ou privada). 
Em resumo: o Git é o sistema de controle de versão, e o GitHub é o serviço onde você hospeda seus repositórios Git.

### Pull

Quando fazemos o fetch, nós baixamos as mudanças do repositório remoto, mas não aplicamos nenhuma mudança no nosso repositório local. Para aplicar as mudanças no nosso repositório local, nós precisamos usar o comando ```git pull```:

```bash
git pull origin
```

### Pull Requests

No github, uma pull request é uma solicitação de merge de uma branch para outra. Ou seja, você está pedindo para que as mudanças que você fez na sua branch sejam mergeadas na branch de destino.

É muito comum você criar uma branch, fazer as mudanças que você quer, e depois criar uma pull request para que o seu time possa revisar as mudanças e aprovar o merge. Elas permitem que o seu time possa discutir sobre as mudanças, sugerir melhorias, e garantir que as mudanças sejam feitas da melhor forma possível.

## Gitignore

É muito comum ter um workflow parecido como esse em um projeto:

```
git add .
```
```
git push origin main
```
```
git commit -m "alguma mensagem aqui"
```

Mas um problema aparece quando nós queremos colocar arquivos no diretório do seu projeto, mas não queremos que eles sejam rastreados ou trackeados pelo Git. O ``.gitignore`` resolve esse problema. Um exemplo é se você trabalha com Javascript, você provavelmente vai querer ignorar o diretório dos ``node_modules``. O ``.gitignore`` pode ignorar arquivos e diretórios. 
 
Se você adicionar no arquivo ``.gitignore`` o nome do diretório ``node_modules``, o Git vai ignorar:

- ``node_modules/code.js``
- ``src/node_modules/code.js``
- ``src/node_modules``

mas ele não iria ignorar algo como:

- ``src/node_modules_2/code.js``
- ``src/node_modules_4``

### Nested Gitignore

O seu ``.gitignore`` não necessariamente precisa estar no root do seu projeto. Ele pode estar em qualquer diretório do seu projeto. E ele vai ignorar todos os arquivos e diretórios que estiverem abaixo dele ou no caso os subdiretórios.

por exemplo:

```
src/
|--assets/
|    |--images/
|    |--styles/
|    |--.gitignore
|--node_modules/
|--.gitignore
```
Nesse caso o ``.gitignore`` na pasta ``src/assets`` vai ignorar todos os arquivos e diretórios que estiverem abaixo dele. o mesmo se aplica ao ``.gitignore`` na pasta ``src``.

### Patterns

Os arquivos ``.gitignore`` aceitam patterns, que são como "padrões" de arquivos e diretórios que devem ser ignorados. Por exemplo, se você quiser ignorar todos os arquivos com a extensão ``.log``, você pode adicionar no arquivo ``.gitignore``:

```
*.log
```

Isso vai ignorar todos os arquivos com a extensão ``.log``, como:

- ``file.log``
- ``src/file.log``
- ``src/node_modules/file.log``


#### Wildcards

O caracter ``*`` trigga qualquer quantidade de caracteres, exceto o separador ``/`` . Por exemplo se você quiser ignorar todos os arquivos com ``.txt``, você pode usar o pattern:

```
*.txt
```

#### Rooted Patterns

Patterns que começam com ``/`` são ancorados ao diretório onde o ``.gitignore`` está localizado. Por exemplo, se você quiser ignorar um arquivo chamado ```root.js`` no diretório root do projeto, mas não nos subdiretórios:

```
/root.js
```

#### Negation

Você pode negar um pattern prefixando ele com ``!``. Por exemplo se você quiser ignorar todos os arquivos com ``.txt``, mas não quiser ignorar o arquivo ``root.txt``:

#### Comentários

Os comentários no ``.gitignore`` começam com ``#``.

```
# Ignora todos os arquivos com .txt
*.txt

# Mas não ignora o arquivo root.txt
!root.txt
```

### O que Ignorar?

Vimos como nós podemos ignorar arquivos, mas o que exatamante você deve ignorar? Algumas regras que você pode seguir nos seus projetos são

- 1 - Ignore coisas que possam ser geradas(exemplo: código compilado, arquivos mimificados, etc.)
- 2 - Ignore dependências(exemplo: ``node_modules``, ``venv``, ``packages``, etc.)
- 3 - Ignore coisas que são pessoais ou da forma como você gosta de trabalhar(exemplo: configurações do seu editor)
- 4 - Ignore coisas que pode ser sensíveis ou perigosas(exemplo: arquivos ``.env``, senhas, chaves de API, etc.)

## Fork

Um **fork** é uma cópia de um repositório que fica na sua conta. Diferente de um clone (que é uma cópia local), o fork reside no servidor do provedor de hospedagem (como o GitHub).

### Para que serve?
- **Contribuir para projetos Open Source:** Você faz um fork, altera o código no seu repositório e depois envia um *Pull Request* para o projeto original.
- **Base para novos projetos:** Usar um projeto existente como ponto de partida para algo novo.
- **Experimentação:** Testar mudanças sem risco de afetar o repositório principal.

### Sincronizando um Fork
Para manter seu fork atualizado com o repositório original (upstream):

1. Adicione o repositório original como um remote:
   ```bash
   git remote add upstream https://github.com/usuario-original/repositorio.git
   ```
2. Busque as alterações:
   ```bash
   git fetch upstream
   ```
3. Faça o merge no seu branch principal:
   ```bash
   git merge upstream/main
   ```

## Git Reflog

O comando ``git reflog`` é parecido com o comando ``git log``, mas como o nome diz, (ref-log ou reference log - registro de referência) ele registra especificamente as mudanças nas **referências** que aconteceram ao longo do tempo.

Ele usa um formato diferente para mostrar o histórico de uma branch ou HEAD, mais focado na quantidade de passos e quais foram esses passos voltando no tempo.

Exemplo:

```bash
953e35f (HEAD -> main, origin/main) HEAD@{0}: commit: Explicação sobre Git Fork
5794c23 HEAD@{1}: commit: Configurando o arquivo .gitignore
48a5e1c HEAD@{2}: commit: Exemplos de patterns no Gitignore
dd40822 HEAD@{3}: commit: Início da documentação de Gitignore
793c68a HEAD@{4}: pull: Merge made by the 'ort' strategy.
b43a7eb HEAD@{5}: commit: Seção de Git Pull e GitHub
3c43bf7 HEAD@{6}: Branch: renamed refs/heads/master to refs/heads/main
3c43bf7 HEAD@{8}: commit: Documentando comandos de Remote
e48f394 HEAD@{9}: commit: Explicando Reset Soft e Hard
4ee31e6 HEAD@{10}: commit: Início da seção de Reset
376a4f1 HEAD@{11}: reset: moving to HEAD~1
```

Então ele mostra todo o histórico de movimentos que você fez nas referências locais(como HEAD), o que te permite até recuperar estados passados que não aparecem no ``git log``.

### Usando Reflog com Merge e Commit-ish

O Reflog não serve apenas para visualização. As entradas que você vê (como `HEAD@{n}`) são tipos de **commit-ish** (referências que apontam para um commit específico). Você pode usar essas referências diretamente em comandos como `merge`.

#### Exemplo: Recuperando um Merge Deletado

Imagine que você deletou uma branch após um merge, mas depois percebeu que precisava de um estado anterior daquela branch que não está mais no histórico do `git log`.

1.  **Encontre a referência no Reflog:**
    ```bash
    git reflog
    # Resultado:
    # abc1234 HEAD@{0}: merge feature-x: Merge made by the 'ort' strategy.
    # def5678 HEAD@{1}: checkout: moving from feature-x to main
    # 9876543 HEAD@{2}: commit: Finalizando feature-x antes do merge
    ```

2.  **Mergear um estado específico via Reflog:**
    Se você quiser "refazer" o merge ou trazer as alterações exatamente como elas estavam no commit `9876543` (que era o `HEAD@{2}` naquele momento):
    ```bash
    git merge HEAD@{2}
    ```

#### O que é Commit-ish?
Um **commit-ish** é qualquer termo que o Git consiga resolver para um ID de commit. Exemplos comuns:
- Hash completo: `1a2b3c4d...`
- Hash curto: `1a2b3c4`
- Nome de branch: `main`, `feature-login`
- Tags: `v1.0.2`
- Referências relativas: `HEAD~1` (um commit atrás), `main^^` (dois commits atrás)
- **Entradas do Reflog:** `HEAD@{5}`, `main@{yesterday}`

Isso torna o `git merge` extremamente flexível, permitindo que você combine estados temporários ou passados de forma cirúrgica.

### Merge Conflicts

Quando você está trabalhando em arquivos ou linhas diferentes que o seu time, provavelmente você não vai ter conflitos, mas quando você está trabalhando em uma features nas mesmas linhas de código ou no mesmo arquivo que alguém do seu time também está, aí surgem os conflitos.

Um merge conflict ocorre quando dois commits, modificam a mesma linha e o Git não pode automaticamente decidir qual das duas mudanças ele vai manter, e qual ele vai descartar.

Vamos dizer que você tem o seguinte histórico:

```bash
    C - feature
  /
A - B - main
```

A branch ``main`` tem um arquivo com a seguinte função:

```bash
function square(numero) {
  return numero * numero;
}
```

E na feature o mesmo arquivo e função:

```bash
function square(numero) {
  return numero + numero;
}
```

Mas note que o retorno da função da feature é diferente.

Se nós mergearmos a branch ``feature`` na branch ``main``, o Git vai detectar que a linha com o``return`` da função mudou nas duas branches de forma independente, o que acaba criando conflito.

Quando um conflito acontece(geralmente como resultado de um ``merge`` ou um ``rebase``) o Git vai te deixar decidir de forma manual, qual das alterações que você vai manter. E tudo vai bem quando a mesma linha é modificada em um commit, e de novo em outro commit. Mas o problema aparece quando a mesma linha é modificado em dois commits que não tem relação ``pai -> filho``.