# ¡Bienvenido a la práctica de Git!
Este repositorio es un lugar seguro para practicar los conceptos básicos de Git. ¡Prueba una rebase sin arriesgar ningún código importante! ¡Aprende a sincronizar desde un repositorio anterior! ¡Resolver conflictos de fusión de forma práctica!
Esta guía supone que tienes un conocimiento cómodo de los comandos de shell de Unix. Si quieres un repaso, [prueba aquí](https://github.com/you-dont-need/You-Dont-Need-GUI). Esta guía también supone que tienes una cuenta de Github existente y que has [agregado tu clave SSH a ella](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/)
Esta guía está diseñada como un espacio para practicar los comandos de Git, no solo para enseñarlos. Si solo buscas una referencia rápida, te recomiendo http://rogerdudler.github.io/git-guide/

## ¿Qué son Git y GitHub?
Git es un sistema de control de versiones. Realiza un seguimiento de los cambios en los archivos y permite que las personas colaboren en proyectos. Git se utiliza a menudo para proyectos de software, pero se puede utilizar para cualquier proyecto y es especialmente potente para proyectos basados ​​en archivos de texto. Un proyecto en Git se denomina "repositorio" (o repositorio para abreviar). Un repositorio es un conjunto de archivos y directorios. Cada repositorio incluye un directorio .git que contiene información sobre cada cambio realizado en el repositorio.

GitHub es un servicio de alojamiento en línea para Git. Si bien la mayor parte de su trabajo con Git se lleva a cabo en su propia computadora, a menudo es útil sincronizar su repositorio local con un repositorio "remoto" (en GitHub), lo que le permite trabajar en varias computadoras o colaborar con otros en un proyecto.
Hay muchas alternativas a GitHub, como Stash, GitLab o el alojamiento propio de sus repositorios remotos.

## Init
Para crear un nuevo repositorio, cree un directorio y use `git init` dentro de el.
```bash
mkdir ISpractica
cd ISpractica
git init
```
Eso es todo, muy simple. Deberías ver un `.git` directorio dentro.
```bash
ls -a # los directorios con inicial . son ocultos, por lo que se usa -a para mostrarlos
```
No utilizaremos este nuevo repositorio, así que puedes continuar y eliminarlo.
```bash
rm -rf ISpractica
```
Bifurca este repositorio en tu cuenta de GitHub presionando el botón Bifurcar en GitHub (arriba a la derecha de la página de GitHub para este repositorio). Esto creará una bifurcación (copia) de este repositorio en tu cuenta de GitHub, creando un nuevo repositorio remoto que tú controlas. La bifurcación es útil cuando quieres usar un repositorio existente como base para tu propio proyecto. Un repositorio bifurcado también se puede usar para proponer cambios al repositorio original.

Si no estás usando GitHub para administrar tu repositorio remoto, entonces crea tu propia copia remota de este repositorio en cualquier servicio de alojamiento remoto que estés usando.

## Clone
`clone` Crea una copia local de un repositorio remoto. Clonemos el repositorio bifurcado del paso anterior. 
```bash
git clone https://github.com/rgfernan/git-practice.git
```

## Add y Commit
Ahora que tenemos un clon local del repositorio, haremos un cambio en él.
```bash
echo "Este es un ejercicio de prueba de Git" > crawl.txt
git status
```
El comando `git status` muestra que tenemos un cambio no preparado. Para registrar un cambio en Git, primero tenemos que preparar ese cambio en el índice y luego confirmar todos los cambios preparados.
```bash
git add crawl.txt # preparar el cambio
git status # demuestra que nuestro cambio está preparado y listo para commit
git commit -m "Hice un cambio" # Confirmar los cambios programados con un mensaje
```
El paso de preparación parece innecesario en este caso, pero es útil en proyectos complejos en los que se modifican varios archivos. Probemos con un ejemplo
```bash
echo "Las naves espaciales rebeldes, que atacan desde una base oculta, han conseguido su primera victoria contra el malvado Imperio Galáctico." >> crawl.txt # modificamos el primer archivo
echo "Me gustan los tomates" > newFile.txt # creamos un archivo nuevo
git status
```
Ahora tenemos dos cambios y queremos confirmarlos por separado, lo que podemos hacer con el comando staging. Prueba a ejecutar `git status` después de cada comando a continuación para ver los cambios que estás realizando.
```bash
git add crawl.txt
git commit -m "Iniciando crawl"
git add newFile.txt
git commit -m "iniciando blog de comida"
```

## `HEAD` y el git tree
En Git, el `HEAD` es el puntero a la referencia de la rama actual. Eso significa que apunta a la confirmación actual en la rama actual. Mover el encabezado es la forma en que nos movemos a confirmaciones más nuevas o más antiguas o nos movemos entre ramas. Puedes pensar en el encabezado como un marcador de "estás aquí". Echemos un vistazo a dónde estamos
```bash
git log
```
Puedes ver "HEAD" en la parte superior, pero no es una visualización particularmente clara del árbol de Git. Si tienes un programa gráfico de Git, puedes usarlo para examinar el árbol de Git, pero yo prefiero quedarme en la línea de comandos.
```bash
git log --graph --decorate --pretty=oneline --abbrev-commit --all # Muestra un registro mucho más bonito.
```
¡Eso está mucho mejor! Podemos ver `HEAD` claramente en la parte superior, apuntando a la rama `master`. También se ven otras ramas, pero ignórelas por ahora.

Guardemos ese comando para usarlo más tarde. Este paso es opcional, pero se recomienda. Gracias a [Conrad Parker](http://blog.kfish.org/2010/04/git-lola.html) por el comando lola.
```bash
git config --global alias.lola 'log --graph --decorate --pretty=oneline --abbrev-commit --all'
git lola
```

## diff
`diff` te permite comparar dos commits

## Branches
Como viste en `git lola`, nuestro árbol git tiene múltiples ramas. Vamos a enumerarlas todas:
```bash
git branch -l
```
Cada rama es un puntero a una instantánea de su proyecto. Si está trabajando en varias funciones diferentes, utilice siempre una rama nueva para cada una.

### New branch
Vamos a crear una nueva rama
```bash
git branch myFirstBranch # crear una nueva rama
git status
```
¿Qué ha sucedido? No mucho. Hemos creado una nueva rama (una instantánea de nuestro código), pero eso es todo. `HEAD` todavía apunta a la rama `master`, por lo que cualquier cambio de código que hagamos se realizará en `master`, no en nuestra nueva rama. Para mover `HEAD` a una rama diferente, use el comando `checkout`.
```bash
git checkout myFirstBranch
git status
```
Ahora, podemos ver que `HEAD` apunta a nuestra nueva rama. A menudo, querrás ir a una rama inmediatamente después de crearla. Puedes hacerlo con un solo comando:
```bash
git checkout -b "foodBlog" #Crea una nueva rama y cámbiate a ella inmediatamente
git status
```

Si confirmamos los cambios en nuestra nueva rama, veremos que se vuelve diferente a nuestra rama maestra:
```bash
echo "Me has fallado por última vez." >> crawl.txt
git add crawl.txt
git commit -m "Se agregó una cita de Vader"
git lola
git diff master # compara dos ramas
```

## reset
### restablecer un commit
Ahora, intentemos modificar el árbol con `reset`. Este comando mueve la rama a la que apunta `HEAD` (y mueve `HEAD` junto con ella). El comando se llama "reset" porque "restablece" o "deshace" los cambios.

El comando `reset` tiene varios modos, así que comencemos con `--soft`. Restableceremos el padre de la confirmación actual usando el atajo `HEAD~`. También puedes moverte a una confirmación específica especificando el hash de la confirmación (visible en `git log` o `git lola`) en lugar de `HEAD~`.
```bash
git checkout master
echo "limpiar" >> mess.txt
git add tamagotchi.txt
git commit -m "error agregado"
git reset --soft HEAD~
git lola
```
Puedes ver que la rama maestra se ha movido a la confirmación anterior, y `HEAD` se ha movido junto con ella. Sin embargo, ninguno de los archivos ha cambiado, y si ejecutamos `git status` vemos que mess.txt está preparado para ser confirmado. `git reset --soft HEAD~` básicamente ha deshecho nuestra última confirmación.

A continuación, se muestra el comportamiento predeterminado para restablecer: `--mixed`. Esto no cambiará ningún archivo, pero deshará los cambios realizados y deshará las confirmaciones. Ejecute `git lola` y `git status` después de cada comando a continuación para ver los cambios que está realizando.
```bash
git commit -m "Confirmar los cambios que acabamos de deshacer"
git reset HEAD~
```
Después de ejecutar el `reset` predeterminado (`--mixed`), puedes ver que `mess.txt` está presente, pero no está preparado. Hemos deshecho la confirmación y hemos sacado el archivo del modo preparado.

Por último, la opción más peligrosa: `reset --hard`. Este comando deshará las confirmaciones, deshabilitará los cambios y eliminará esos cambios de todos los archivos. Los cambios se borrarán para siempre, así que úselo con precaución.
\*see Reflog
```bash
git add . # preparamos todo lo que acabamos de deshacer
git commit -m "Confirmar los cambios que acabamos de deshacer"
git reset --hard HEAD~
```
Si miras tu directorio de trabajo, podrás ver que `mess.txt` no se encuentra en ninguna parte y `git status` tampoco muestra nada.

En resumen:
`reset --soft`: Mueve la rama a la que apunta `HEAD` (deshace confirmaciones)
`reset`: (`--mixed`) Mueve la rama `HEAD` y hace que el índice de staging se parezca a la nueva ubicación de HEAD (deshace confirmaciones y deshace cambios de staging)
`reset --hard`: Mueve la rama `HEAD`, hace que el índice de staging se parezca a HEAD y haz que el directorio de trabajo se parezca al índice (deshace confirmaciones, deshace cambios de staging y elimina todos los cambios. Úsalo con precaución).

En el código anterior, hemos estado usando `reset` para mover confirmaciones en la misma rama, pero no hay nada que te impida usar confirmaciones en diferentes ramas.

#### Squashing commits
El botón de reset es útil como botón para deshacer, pero probemos un ejemplo más interesante. Imaginemos que somos un desarrollador ocupado y hemos creado un montón de mensajes de confirmación inútiles.
```bash
git checkout -b butternut
touch pumpkin.txt
git add pumpkin.txt
git commit -m "vegetariano"
echo "querido diario" >> pumpkin.txt
git add pumpkin.txt
git commit -m "si"
echo "Hoy comi un rabano" >> pumpkin.txt
git add pumpkin.txt
git commit -m "oops"
echo "O sea, me comi una calabaza. Que dia" >> pumpkin.txt
git add pumpkin.txt
git commit -m "max"
git lola
```
Esos mensajes de confirmación son inútiles. Reemplácelos con un único mensaje de confirmación.
```bash
git reset HEAD~4
git add pumpkin.txt
git commit -m "Comence un registro de mis habitos alimentarios."
```
Ahora volvamos a la rama master para el resto de nuestro trabajo.
```bash
git checkout master
```

### Resetting a file
`reset` También se puede utilizar en un archivo específico. Si haces esto, `HEAD` no se moverá, pero se llevarán a cabo todas las demás acciones de `reset`. Vamos a intentarlo. Ejecuta `git status` mientras avanzas para ver los cambios.
```bash
git checkout master
echo "El imperio no hizo nada malo" > protest.txt # haz un cambio
git add protest.txt # prepara el archivo
git reset protest.txt # quita archivo de staging
```

## checkout
### checking out a commit
Al igual que `reset`, el comando `checkout` manipula el árbol git y mueve `HEAD` a un commit específico. `git checkout [commit]` es similar a `git reset --hard [commit]`, pero `checkout` solo mueve `HEAD`, no la rama a la que apunta `HEAD`. Me gusta pensar en `checkout` como una forma de moverse por el árbol, mientras que `reset` es una forma de cambiar el árbol. Probémoslo.

```bash
git checkout HEAD~
git lola
```
Después de la verificación, ahora estamos en un estado de "detached head"; ya no estamos trabajando en una rama. A diferencia de antes, cuando hicimos "git reset", todavía podemos ver todas nuestras confirmaciones anteriores en el árbol de git y podemos regresar a nuestra rama anterior fácilmente.
```bash
git checkout master
git lola
```

`checkout` También nos detendrá si tenemos cambios no confirmados (a diferencia de `reset --hard`, que simplemente destruye cualquiera de nuestros cambios sin verificar)
```bash
echo "alto" > crawl.txt
git status
git checkout HEAD~
```
Git no nos permite realizar el checkout porque tenemos cambios no confirmados

### checking out a file
También puedes usar `checkout` en una ruta de archivo. Esto no moverá `HEAD`, pero desmarcará cualquier cambio en ese archivo en el índice y también sobrescribirá ese archivo. `git checkout [branch] file` es equivalente a `git reset --hard [branch] file`, excepto que el último comando no existe.

Usemos checkout para eliminar nuestros cambios no deseados para rastrear
```bash
git checkout master crawl
```

Reset y checkout son dos herramientas potentes, y un gran poder conlleva una gran complejidad. Recomiendo encarecidamente [leer más](https://git-scm.com/book/en/v2/Git-Tools-Reset-Demystified) sobre estos dos comandos.

## Reflog
Cuando dije que `reset --hard` eliminaría un commit para siempre, no estaba diciendo la verdad. `git reflog` está disponible como una opción de último recurso. Este comando muestra un reflog (historial) de cada vez que se actualizó la punta de una rama. Si tienes buena memoria y buenos mensajes de commit, puedes usar reflog para recuperar commits que ya no están en el árbol git.
```bash
git reflog # muestra reflog para HEAD
git reflog show --all # muestra reflog para todas las ramas

git checkout blizzard
git reset --hard HEAD~ # acabo de perder un commit que quería
git reflog show blizzard # muestra registro de referencias de la rama de Blizzard y busca commits perdidos
git reset <commit-hash> # Coloque el hash de commit aquí, no <>
git status # comprobar que los cambios sean los esperados
git reset --hard <commit-hash> # Coloque el hash de commit aquí
```

## Stash and pop
A veces, desea almacenar temporalmente algún trabajo pero no confirmarlo (por ejemplo, si desea verificar rápidamente otra rama o si se da cuenta de que comenzó a trabajar en la rama incorrecta). `git stash` le permitirá hacer esto. `git stash pop` recuperará los cambios almacenados más recientemente (los cambios almacenados se almacenan usando una pila).
```bash
git checkout master
echo "mcTempFace temporal" >> crawl.txt
git stash # Almacenamos temporalmente nuestro trabajo porque esta es la rama incorrecta.
git checkout blizzard # pasar a la rama correcta
git stash pop # recuperar trabajo almacenado
git add .
git commit -m "agrego algo temporal"
```
Nunca utilices stash para almacenar a largo plazo, porque te garantizo que olvidarás lo que habías almacenado y en qué rama debía ir. Si quieres almacenar algo durante más de un día y no lo quieres en ninguna de tus ramas, simplemente colócalo en una nueva rama en el lugar correspondiente.

## Merging
Cuando quieres incorporar código de una rama a otra, es momento de realizar un "merge".

### Auto-resolved merge
La mayoría de los merges son asuntos sencillos y Git las gestiona de forma completamente automática.
```bash
git checkout merge-target
git merge simple-feature
git lola
```

### Merge conflicts
#### Resolucion manual
En ocasiones, un merge requiere intervencion manual
```bash
git checkout merge-target
git merge complex-feature
git status
```
No hay problema. El merge ha generado algunos conflictos y Git no está seguro de cómo resolverlos. `git status` nos dice que estos conflictos están en `crawl.txt`, así que abre ese archivo en tu editor favorito.

Cuando abras `crawl.txt`, verás X e Y. X es de la rama X e Y de Y. Para resolver la fusión, edita el archivo como creas conveniente y luego elimina los marcadores. En este caso, deberíamos hacer Z. Cuando hayas terminado, guarda tu trabajo y regresa a la terminal.
```bash
git add crawl.txt
git merge --continue
```

#### Ours vs theirs
A veces, cuando tienes conflictos de merge, simplemente querrás tomar todos los cambios de una rama específica siempre que haya un conflicto.
```bash
git checkout merge-target
git merge complex-feature-2
```

Al fusionar, "ours" significa la rama en la que se está fusionando, mientras que "theirs" significa la rama de características desde la que se está fusionando. En nuestro caso, estamos fusionando en `merge-target` y estamos fusionando desde `complex-feature-2`. Hemos decidido que solo queremos los cambios de `complex-feature-2`, también conocido como "theirs", para que podamos evitar hacer un merge manual completo.
```bash
git checkout --theirs funky.txt # toma los cambios de la rama feature-2
git add funky.txt
git merge --continue
```

## Remotes
Un repositorio remoto es una copia remota de un repositorio. En la mayoría de los proyectos, existe un único repositorio remoto que se utiliza como fuente central de información.

### Pull
`pull` sincroniza tu rama local con una rama remota. Funciona realizando una `fetch` (descarga los ultimos datos del remoto) y despues `merge`s los datos obtenidos en la rama actual. En su caso, no habrá nada que actualizar, ya que no se han realizado cambios en el remoto.
```bash
git checkout master
git pull origin master
```

### Push
`push` sincroniza la rama remota con su rama local.
```bash
git checkout master
echo "porgos" >> crawl.txt
git add crawl.txt
git commit -m "agrega algo lindo de animal."
git push origin master
```

### checkout branch from remote
Cuando clonaste este repositorio, se creó con un único control remoto, llamado "origin". Probemos eso ahora.
```bash
git fetch # actualizar información remota
git checkout -b origin/check-me-out # Crea una nueva rama local para realizar un seguimiento de la rama remota
```

### Adding remotes
Dado que ha bifurcado este repositorio, desea mantenerlo actualizado con el repositorio original del que lo bifurcó. Para ello, agregue el repositorio original como un nuevo repositorio remoto llamado "upstream:
```bash
git remote add upstream https://github.com/rgfernan/git-practice.git# agregar nuevo remoto
```

### Sync upstream (fetch and merge)
Ahora que ha agregado un nuevo upstream, puede sincronizar las ramas upstream con las ramas locales
```bash
git checkout master
git fetch upstream
git merge upstream/master
```
Nuevamente, este ejemplo específico no hará nada a menos que haya actualizado el repositorio original.


## Rebasing
El rebase mueve uno o más commits a una nueva confirmación base. Si desea incorporar cambios de una rama principal en su rama de trabajo (pero no desea volver a fusionarla con la rama principal), entonces la rebase es lo que necesita. Advertencia: [no rebase ramas públicas](https://benmarshall.me/git-rebase/)
### Auto-resolved rebase
Al igual que con el merge, la mayoría de los rebase son triviales.
```bash
git checkout rebel-alliance
git lola
git rebase hoth
git lola
```
Al examinar el árbol git, puedes ver que el inicio de la rama `rebel-alliance` se ha movido al final de la rama hoth.
### Merge conflicts while rebasing
Alguns rebase provocarán conflictos de merge
```bash
git checkout yoda
git lola
git rebase hoth
git lola
```
Como hiciste en la sección de merge, abre tu editor favorito y resuelve los conflictos de merge. Usa `git status` para ver los archivos conflictivos. Cuando hayas terminado, agrega los archivos y continúa con el rebase.
```bash
git add .
git rebase --continue
```

#### ours vs theirs
```bash
git checkout palp
git rebase hoth
```
Durante un rebase, Git comprueba la rama en la que estás rebasando y reproduce los cambios de tu rama sobre ella. Por lo tanto, "ours" significa la rama en la que estás rebasando y "theirs" significa la rama que se está rebasando. En este caso, estamos rebasando palp ("theirs") en hoth ("ours"). Al igual que con el merge, podemos hacer uso de estos términos para evitar un merge complejo. Digamos que queremos mantener los cambios en la rama hoth siempre que haya un conflicto
```bash
git checkout --ours padawan.txt
git add padawan.txt
git rebase --continue
```

## Cherry-picking
Cherry pick le permite elegir un commit de una rama y aplicarlo a otra. Digamos que queremos aplicar el commit con el mensaje "PICK ME" de la rama `cherries` a la rama `farm`.
```bash
git checkout farm
git cherry-pick <commit-hash> # inserta el commit correcto
git lola
```
