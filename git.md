# Git

git é um sistema de controle de versão, que permite que você rastreie e controle as alterações dos arquivos no seu projeto.

## Repositórios

```git init``` -- inicializa um repositório vazio, no diretório atual, ele cria a pasta .git que armazena todos os dados do repositório, commits, branches, histórico e etc.

```git status``` -- mostra o status atual de um arquivo, podendo ser untracked, staged ou committed, você pode passar um arquivo ou diretório como argumento para ver o status de um arquivo específico. A resposta retornada pro exemplo: ``git status git.md``

```
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	git.md

nothing added to commit but untracked files present (use "git add" to track)
```

```git add``` -- adiciona um arquivo ou diretório ao stage, ele marca o arquivo para ser incluído no próximo commit,  você pode passar um arquivo ou diretório como argumento para adicionar um arquivo específico. Você também pode somente passar logo após o comando ```git add``` um ```.``` pra adicionar todos os arquivos não trackeados. A resposta retornada pro exemplo: ```git status git.md```, após o comando ```git add git.md``` 

```
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   git.md

```

```git commit -m``` -- um commit é como uma fotografia do seu repositorio, em um determinado ponto no tempo. Git não armazena as diferenças, o git guarda todo histórico por commit. Pra realizar um commit, você deve executar o comando ```git commit -m "mensagem do commit"```. A mensagem retornada é:

```
[master (root-commit) 8379dc0] add mod git.md
 1 file changed, 17 insertions(+)
 create mode 100644 git.md
```

logo após o commit, rodando o comando ```git status```

```
On branch master
nothing to commit, working tree clean
```

logo após o commit, rodando o comando ```git log --oneline```

```
38d407a (HEAD -> master) add mod git.md
8379dc0 add git.md
```

```git commit --amend``` -- se você errou a mensagem do seu ultimo commit, você pode corrigir com o comando ```git commit --amend -m "nova mensagem"```

```git log``` -- em um projeto provavelmente você vai ter uma longa lista de commits. Pra visualizar esse historico de commits, você pode usar o comando ```git log```, que mostra uma lista de commits com informações como data, autor e mensagem. Você também pode usar o comando ```git log --oneline``` para mostrar apenas a mensagem e o hash do commit.

```
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

```git log --no-pager log -n 10``` -- limita o maximo de commits a serem exibidos, por exemplo, ```git --no-pager log -n 10``` limita a exibição de 10 commits.

```git log --no-pager log -n 10 --oneline``` -- mostra apenas a mensagem e o hash do commit com a limitação de 10 commits.

```git log --no-pager log -n 10 --oneline --graph``` -- mostra apenas a mensagem e o hash do commit com a limitação de 10 commits e um grafico de commits.

```git log --no-pager log -n 10 --oneline --parents``` -- mostra apenas a mensagem e o hash do commit com a limitação de 10 commits e os pais do commit.

## Internals

todos os dados em um repositorio git são armazenados em uma object database, localizada dentro do diretório ```.git```.
Nesse diretório tem um conjunto de arquivos que representam os objetos do Git, como blobs(arquivos), trees(diretorios), commits e tags.

```git cat-file -p <hash>``` - permite visualizar o conteudo de um commit diretamente.

### Trees e Blobs:

- tree é a maneira do git armazenar um diretório.
- blob é a maneira do git armazenar um arquivo.
