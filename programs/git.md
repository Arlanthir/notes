# GIT

## Instalar e configurar Git
```bash
sudo apt-get install git
git config --global user.name "<NAME>"
git config --global user.email <EMAIL>
# git config --global core.editor gedit
git config --global core.editor "atom --wait"
# git config --global color.ui auto
```

## Inicializar repositório para um projecto já existente
```bash
cd <pasta com as sources>
git init
echo -e '/bin/\n/gen/' > .gitignore
git add --all
git commit -m "Initial Commit"
```

## Workflow Típico Git, depois de tudo configurado e clonado
```bash
git pull              // = git fetch + git merge
<Trabalhar>
git add --all
git commit
git pull
<Resolver possíveis conflitos>
git push
```

## Workflow Típico Git, depois de tudo configurado e clonado (com branches que precisam pushar para o master)
```bash
git add --all
git commit
git checkout master
git pull

git merge <localbranch> --no-ff
git push
git checkout <localbranch>
```

## Remotes

```bash
git remote -v    # Ver remotes utilizados e os seus nomes
```

### Adicionar um projecto de um servidor
```bash
git clone git@arlan.servegame.org:<PROJECT_NAME>.git     # Isto tracka os branches (todos?)

git fetch origin   # Sacar nova informação do servidor
git pull                 # fetch + merge

git push origin <BRANCH_NAME>  # Enviar a nossa informação para o servidor. Branch-pai chama-se "master"
git push origin <MY_BRANCH>:<SERVER_BRANCH> # Enviar a nossa informação para o servidor, com controlo detalhado dos branches
git push --mirror <nome>   # Enviar todas as refs

git push origin :<BRANCH_NAME>  # Apagar branch no servidor

git checkout -b <BRANCH_NAME> origin/<BRANCH_NAME>  # Importar branch do servidor
```

### Trackar um branch
```bash
git checkout --track origin/<BRANCH>   # Agora push e pull funcionam sem argumentos (pull é um fetch para um branch)
```

### Adicionar remote
```bash
git remote add origin git@home.epicvortex.com:<PROJECT_NAME>.git
```

### Adicionar remote como mirror (push para lá vai sempre com --mirror)
```bash
git remote add --mirror=push <nome> <remote>
```

### Listar info/branches do remote
```bash
git remote show origin
```

### Actualizar branches do remote
```bash
git remote update origin
```

## Tags
```bash
git tag -a v1.0 -m 'Initial Public Release'    # Anotada
git tag v1.0                                                # Lightweight
git tag v1.0 <começo_da_hash_de_um_commit>   # Taggar um snapshot depois do commit estar feito
git push origin v1.0                                   # Pushar a tag para o remote
git push origin --tags                                # Pushar todas as tags para o remote
```

## Branches
```bash
git branch <BRANCH>   # Criar branch
git branch -d <BRANCH>   # Apagar branch
git checkout <BRANCH>   # Mudar vista activa para branch
git checkout <BRANCH> -- <FILE> # Sacar só determinado ficheiro
git checkout -b <BRANCH> # Criar e mudar vista activa para branch
git merge <BRANCH>   # Fazer merge de um branch para onde estamos
git merge --strategy-option ours|theirs <BRANCH> # Merge e decidir quem ganha os conflitos
git rebase <BRANCH>   # Aplicar commits de um branch nesta linha e fingir que foi aqui desde sempre. Não usar em commits remotos!!!
git branch -a # Listar branches (all, -r para remote)

git diff <branch1> <branch2> -- <FILE>  # Compare file between branches
```

## Corrigir último commit com um novo
```bash
git commit --ammend
```

## Reset mudanças desde o último commit
```bash
git reset HEAD --hard
git clean -fd
```

## Instalar e configurar servidor Git por SSH
```bash
sudo apt-get install openssh-server git
(consider chmod'ing your stuff to 711 (careful with enabling +x in everything), search around, example: chmod -R go-rwx /home/alice)
sudo adduser git
```

## Initialize a bare repository for a project
```bash
su git
cd ~
mkdir <PROJECT_NAME>.git
git init --bare    # Se for suposto mais users acederem ao repo, adicionar --shared
```

## Clone a project from the hard disk as a bare server repo
```bash
su git
cd ~
git clone --bare /home/user/path_to_proj <PROJECT_NAME>.git
```

## Outros comandos úteis
```bash
git status
git status -s             # Short version
git commit --amend -m "New commit message"
git log         # --stat para ver detalhes         -n3 para limitar aos ultimos 3

git diff <commit1> <commit2>   # Compare two commits

git blame <file>                         # Show git commit annotations in each line of a file
git blame <file> > <newfile>     # As above but in a file instead of the terminal
git blame -L 40,60 <file>          # Limit analyzed lines
```

## Branch logs with graph and colors:
```bash
git log --pretty=oneline
git log --graph --oneline --decorate --all

git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit
```

### Add to alias git graph:
```bash
git config --global alias.graph "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

### And to show log with changed lines:
```bash
git graph -p
```
More tips: https://csswizardry.com/2017/05/little-things-i-like-to-do-with-git/

## Receita para criar bare repo a partir de código existente
```bash
cd /my-project/
git init
echo -e '/bin/\n/gen/' > .gitignore
git add .
git commit -m 'Initial commit'
cd /place/where/repos/are/
git init --bare my-project.git
cd /my-project/
git remote add origin /place/where/repos/are/my-project.git
#example git remote add origin git@home.epicvortex.com:my-project.git
git push -u origin master                                                                # setting up "upstream" and pushing the first commit
```

## .gitignore (ignorar estes ficheiros)
```bash
/file                   # Ignore file in root
file                    # Ignore file in any directory
**/file                # Ignore file in any directory
folder/**            # Ignore everything in folder
!file                   # Don't ignore file
```

.gitignore local (não é commitado)

`.git/info/exclude`

## Backup git
```bash
git bundle create projectname.bundle --all         # all branches
git bundle create projectname.bundle master    # just master
To restore it:
git clone projectname.bundle projectname
```

## Recover popped stash (or unreachable commit)
```bash
git fsck --no-reflogs          # to list commits
git show <hash>                # to see info
git stash apply <hash>     # to apply stash
gitk --all $( git fsck --no-reflog | awk '/dangling commit/ {print $3}' )                                   # GUI log version
```
