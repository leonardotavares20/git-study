# Git

git é um sistema de controle de versão, que permite que você rastreie e controle as alterações dos arquivos no seu projeto.

## Repositórios

git init -- inicializa um repositório vazio, no diretório atual, ele cria a pasta .git que armazena todos os dados do repositório, commits, branches, histórico e etc.

git status -- mostra o status atual de um arquivo, podendo ser untracked, staged ou committed, você pode passar um arquivo ou diretório como argumento para ver o status de um arquivo específico. A resposta retornada pro exemplo: ``git status git.md``

```
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	git.md

nothing added to commit but untracked files present (use "git add" to track)
```

git add -- adiciona um arquivo ou diretório ao stage, ele marca o arquivo para ser incluído no próximo commit,  você pode passar um arquivo ou diretório como argumento para adicionar um arquivo específico. Você também pode somente passar logo após o comando ```git add``` um ```.``` pra adicionar todos os arquivos não trackeados. A resposta retornada pro exemplo: ```git status git.md```, após o comando ```git add git.md``` 

```
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   git.md

```

git commit -- um commit é como uma fotografia do seu repositorio, em um determinado ponto no tempo. Git não armazena as diferenças, o git guarda todo histórico por commit. Pra realizar um commit, você deve executar o comando ```git commit -m "mensagem do commit"```. A mensagem retornada é:

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

git commit --amend -- se você errou a mensagem do seu ultimo commit, você pode corrigir com o comando ```git commit --amend -m "nova mensagem"```
