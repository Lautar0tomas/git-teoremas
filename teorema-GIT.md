+ \\main\\ es una rama en Git es como una "linea de tiempo " de tu proyecto y 
main avanza automaticamente al nuevo commit 

+ \\HEAD\\ es como un puntero temporal que te permite ver el codigo en una version especifica
HEAD se puede mover pero la rama main sigue apuntando a su ultimo commit 
el estado "head detached" quiere decir "cabeza desprendida" o "desacoplado" , y te permite observar esa linea temporal 

[propiedades-a-tener-muy-en-cuenta]
{1ero} 
- HEAD -> main : "aqui HEAD esta apuntando a la rama ACTIVA , lo que significa que cualquier commit que hagas se agregara a esta rama(main) 
   avanzando la misma "

{2do}
- HEAD ,... , main : "aqui HEAD esta en estado DETACHED y su rama ACTIVA es otra , y estas observando el ultimo commit de la rama MAIN 
    esto se debe a que "main" es el nombre de la rama , y la rama avanza con cada nuevo commit automaticamente sobre la misma .

{3ro}
- (HEAD) " aqui HEAD apunta directamente a un commit no a una rama y si estas en estado DETACHED y no estas postrado sobre el
 ultimo commit de una rama , unicamente en git log te aparecera de esa forma " 

{4to} 
- en git status podras observar si estas "desacoplado" o estas en la rama activa 

{5to}
- si estas en estado DETACHED y haces un commit sobre un archivo no existente en esa linea temporal o simplemente cambios en el mismo archivo
  que estas observando , este commit se conderara como un commit "HUERFANO" .

  y este coomit huerfano si no lo quieres perder , tendras que hacer una NUEVA RAMA en ese instante con "switch -c nuevaRAMA " porque necesecitas 
  que el HEAD siga apuntando a ese commit huerfano de otro modo perderas los cambios que has implementado . 
  al crear la nuevaRAMA con ese comando automaticamente el commit huerfano pasara hacer tu primer commit de esa nuevaRAMA 
  con el historial limpio de dicha rama


+ \\commit HUERFANO\\ si quiero trabajar sobre un archivo en especifico sin afectar de ninguna manera ese archivo de esa rama en especifico 
  me sirve realizar cambios a ese archivo estando en estado DETACHED y commitear / guardar / crear una nueva rama sobre el . 
  de esta manera esa rama no tendra "padres" y tendra un historial limpio sin arrastrar los demas 


+ \\commit\\ al crear un commit ademas de guardar esa version de tu codigo estas marcando una 
linea de tiempo en ese main, que quiere decir?
    _ si decides regresar(ver o cambiar ) a una version anterior de tu archivo en especifico
no solo va a afectar a ese archivo si no a todos los commits que has guardado entre tu archivo
de la version actual y al archivo que quieras regresar (un archivo anterior logicamente)
porque estas volviendo al pasado donde no habias hecho los commits de otros archivos sean o
no ese archivo en concreto , por eso se crea otra "main(rama)"
[ EJ ]:
123Lautaro (head -> main)
567MIli
890Blas 
            "git reset 890Blas"

890Blas (head -> main) 
\\como ves , yo quise regresar del archivo "123Lautaro" a "890Blas" pero termine afectando a "567MIli"\\



// [] = uso obligatorio de argumentos 
// () = uso opcional de argumentos
// || = significa "o tambien se puede usar"
// && = significa "y tambien se puede usar con"

# git init 
    - inicializa un nuevo repositorio git en el directorio en el que se ejecuta 
    crea una carpeta oculta llamada .git 



# git status || (nombre-Del-Archivo)
    -muestra el estado de aquellos que no tienen seguimiento (los que no esten en git) 
        - te muestra si hay archivos para agregar al area de stage 
            - y/o si si estan listos para ser committeados


# git add [archivo.extencion] || ( . ) || git add -u 
    - agrega los archivos al area de preparacion (stage area) a partir de ah y estaran
    listos para ser incluidos 

        (1)- el modificador punto agrega todos los archivos que tengas pendientes al area 
        de stage 

            (2)- marca como elminados los archivos que ya no existen , si borraste un archivo y quieres commitear , este comando es 
            el que utilizaras , ya que para subir un push tiene que estar todo comiteado .


# git commit -m "" || commit (--amend -m "actualizar descripcion")
    - guarda los cambios creando una "version" no sobrescrita y agrega una descripcion.
    si se agregan varios archivos al stage el commit -m "" agregara una sola descripcion a todos esos archivos

        (1)- permite modificar la descripcion de tu ultimo commit teniendo en cuenta que 
        no se ah pusheado 



# git checkout [archive.extencion] || (ID-DEL-COMMIT) || ( -f nuevaRAMA )
    - vuelve al ultimo commit (version) descartando las modificaciones             
    actuales (si nos has hecho ya un commit). si es un archivo de otra rama te dara error 

        (1)-permite ver el codigo en una version especifica  

            (2)-si has hecho cambios exprimentales en archivos de otra rama o en archivos no existentes en ese commit (linea de tiempo)
            -f [rama] te permitira volver a tu commit descartando esos cambios , osea sobreescribiendo y borrando lo anterior  


# git log || (-- nombre-del-archivo) || (--oneline)
    - muestra historial de commits detallado 

        (1)- muestra los commits de ese archivo en especifico

                (2)- muestra un resumen de los commits 


# git diff || (nombre-del-archivo)  
    - nos muestra los cambios realizados antes del commit 
        - el menos (-) indica la linea cambiada , el (+) indica lo nuevo en la linea

                    
# git reset ID_COMMIT || (--hard ID_DEL_COMMIT)
    - regresa al commit del ID BORRANDO los commits mas actuales sin eliminar o cambiar lo hecho 
    en los archivos con seguimiento , te pedira guardar en el stage . 

        (1)-ademas de regresar etc ELIMINA los cambios que le has hecho a tus archivos si estan despues 
        de esa version del commit  regresando por completo ah esa linea de tiempo (commit )
            
            -si quieres regresar de vuelta a la vercion del commit actual vasta con encontrar su 
            ID en "git reflog" e insertar "git reset --hard ID_MAS_ACTUAL" porque el "git reset"
            te permite moverte tanto atras como adelante 


# git rm [ --cached ARCHIVO ]
    - elimina el archivo del control de versiones en GIT sin afectar su contenido 


# git tag (etiqueta) || (etiqueta ID_COMMIT) ||(git checkout etiqueta) || (-d etiqueta) || ( git tag )
    - te etiqueta puntos concretos de un commit(version) en especifico una , v1.0/clase/dia etc 

        (1)- etiqueta con texto sin comillas y sin espacios el commit actual 

            (2)- etiqueta un commit anteror 

                (3)- mueve el head al commit con esa etiqueta 

                     (4)- elimina un tag de ese commit de forma local (no remota )

                        (5)- lista los tag que has creado 

- los tags no se suben autometicamente a un repositorio tendras que enviarlos / tampoco se borran de forma remota 

                                    
                                    || RAMIFICACIONES || 
--------------------------------------------------------------------------------------------------------------------                    

+ \\BRANCH ("rama")\\ : crear otra rama fuera de la principal (main) a partir de una version de la misma
 permite trabajar en cambios sin afectar la rama principal , aislar el trabajo para experimentar / desarrollar 
 sin preocupaciones de romper codigo fuente , correcciones de codigo / pruebas y sobre todo sin manchar el historial 
 (git reflog) de la rama principal manteniendolo claro y logico . 

# git branch || [nombre-de-nueva-rama] || ( -d nombre-de-rama )
    (1)- te permite observar en que rama estas ubicado (denotado con " * ") y cuantas ramas hay .
    
        (2)- crea una nueva rama a partir del commit(linea temporal ) de la rama principal (main) pero no se reedirige a la misma 
        (HEAD -> main , nuevaRAMA) // el puntero sigue viendo a la rama principal 

            (3)- te permite eliminar dicha rama 


# git switch [nombre-de-la-rama] || ( -f cambiarRAMA ) || [ -c nuevaRAMA ]
    [1]- cambia a otra rama existente
 (HEAD -> nueva RAMA , main) y apartir de aqui todos los commits seran de la rama correspondiente 

        (2)- el modificador -f te permite cambiar de branch si has hecho modificaciones en un archivo perdiendo los cambios .

            (3)- utilizar para crear una nueva rama sobre un commit HUERFANO (esto te permite guardar el commit huerfano en esa rama creada)



# git merge ( nombre-rama ) || ( --abort )
    - merge (" unir ") permite fusionar / traer los cambios/historia de una rama en especifico hasta tu ( HEAD -> RAMA ) , los archivos 
    carpetas e historial se agregaran a tu rama 

        (1)- git crea automaticamente un commit de merge , lo que significa que no requiere de hacer un commit para guardar los cambios 
  
                (2)- te permite tener la opcion de abortar/cancelar el merge si tienes algun conflicto , o si queres podes decidir quedarte con 
                el " HEAD " que son tus cambios o con " nombre-RAMA " que son los cambios de la rama que quisiste traer , esto se refleja en los cuadros verdes/azules de vscode

 
# git stash || (list ) || ( pop ) || ( drop )

    (1)- guarda los cambios sin hacer commits , permitiendote seguir navegando por las ramas  

        (2)-|list| -> lista los stash disponibles en dicha rama 

            (3)- restaura los cambios disponibles del stash de dicha rama / archivo .

                (4)- borra unicamente el stash que has guardado 



                                            || GIT HUB ||
----------------------------------------------------------------------------------------------------------------------------------------------

\\REPOSITORIO\\: es un almacen de archivos que guarda y gestiona el codigo del proyecto junto con su historial de cambio .(git)

\\SSH\\: protocolo de autenticacion que me permitira subir mi repo de forma remota sin necesitar la autenticacion con contraseña {

  *si estas usando otra pc con las claves tanto privada como publica ssh asociadas a la cuenta Lautar0tomas , y si las tenes 
  y no te permite HACER un push mediante ssh , significa que tu clave ssh no esta cargada en tu git , proceder con :*

    + ssh-add -l
  
    + eval $(ssh-agent -s)  # Inicia el agente SSH
    ssh-add ~/.ssh/id_ed25519  # Agrega tu clave SSH {

        edita tu "~/.zshrc " para que ejecute automaticamente cada que inicies en wsl2 : 
        "   # Iniciar el agente SSH si no está corriendo
if ! pgrep -u "$USER" ssh-agent > /dev/null; then
    eval "$(ssh-agent -s)"
fi

# Agregar la clave SSH automáticamente
ssh-add ~/.ssh/id_ed25519 &> /dev/null


"
    }

    + ssh -T git@github.com

    + si estas usando https en vez de ssh cambialo por : "git remote set-url origin git@github.com:Lautar0tomas/ahk-blas.git
"
     a partir de aca podras usar ssh
}





## "git remote [ add <nombre-repo-remoto> <link-ssh> ] || git remote || git remote -v || git remote remove <n.-repo-remoto>
    -git remote se utiliza para gestionar los repositorios remotos asignandoles un nombre al mismo y listando/agregar/modificar /eliminar
     repositorios remotos asociados a tu proyecto local 

        (1) ""add <nombre-remoto> el nombre por defecto del repositorio remoto va a ser "origin" pero le puedes poner un nombre
        personalizado mas acorde a tu proyecto , este nombre servira como identificador para la conexcion . 
        y luego necesitaras el link-ssh de tu repositorio al que quieres subir tu proyecto  , para que entiendas , es como ponerle 
        un nombre al link-ssh y cada vez que hagas una accion como pushear / fetch etc , en vez de poner todo el link , pones el nombre 
        asociado a ese link (nombre asociado al repositorio remoto ); 

            (2) git remote " :  te mostrara el nombre del repositorio remoto al cual estas conectado

                (3) te mostrara el nombre de tu repositorio remoto y a que repositorio esta conectado para hacer un push o un fetch  

                    (4) elimina la conexion entre tu repositorio local y el remoto (no elimina nada de archivos/directorio), en otras 
                    palabras , git ya no sabra a que servidor remoto conectarse .

# git push ( origin <nombre-de-la-rama-principal> ) || ( -u origin <main> ) || git push 
    - este comando sube los cambios de tu repositorio local al repositorio remoto (github en este caso ) despues de indicarle el "git remote add origin https" que te da github la primera vez
    el push sube cambios al repositorio remoto en el que estas trabajando en local .  
  
      (1)- solo sube la rama principal ( generalmente main ) al remoto origin [origin sera el nombre que le agregaste al repo remoto] estandar, pero no establece un seguimiento automatico   

            (2)- el "-u" es el modificador que ademas de subir al repo remoto , le dice a git que , cada vez que se haga "git push " sin 
            especificar la rama , use automaticamente "origin main "

                (3)- despues de indicar el nombre del repositorio remoto y de la rama , si necesitas subir/cambiar el main de ese repo no nece
                sitaras especificar todo de nuevo . simplemente haz "git push "
            

# git fetch || (<nombre-del-repositorio-remoto>)
    - trae los cambios del repositorio remoto (como ramas y commits ) , al repo local , pero no los fusiona automaticamente 
    con tu rama de trabajo local , pudiendo revisar los cambios sin alteraciones inmediatas en tu repositorio local 

        (1) no necesitas obligatoriamente el nombre de tu repo-remoto si ya esta en conexion con tu local .

al ejecutar git fetch
Se conecta al repositorio remoto (por ejemplo, origin).
Descarga los últimos cambios, como nuevas ramas o commits.
No cambia tu rama actual ni te incorpora esos cambios automáticamente.
Si hay actualizaciones en la rama remota (como origin/main), podrás verlas, pero tu rama local no se actualiza hasta que hagas un git merge o git rebase.


# git pull [<nombre-de-repo-REMOTO>  <nombre-RAMA>] // " git config pull.rebase false " la primera vez .
    - hace un fetch + merge/rebase automaticamente , actualizando tu rama local con los cambios remotos , el nombre del repositorio 
  remoto generalmente es origin .

# git clone [link-SSH(git@github.....)]
    - es equivalente a apretar "dowlang zip" en github , y lo clonas a partir del link "local/ssh[link]", te bajara la rama main en cualquier
    - directorio sin necesidad de terner git en ese directorio 
    - y podras trabajar con ello a partir de ese punto . 


# fork [la bifurcacion se hace en github en el input "tenedor/fork"]
- tenedor/bifurcacion , es una copia completa de un repositorio x a tu propia cuenta (osea te crea ese repositorio pero en tu cuenta) 
- en vez de clonar el repo remoto a tu escritorio y luego hacerle un push en tu cuenta para tenerlo en tu repositorio de github
- simplemente creas esa bifurcacion en tu cuenta ., no descarga el codigo a tu maquina local . 
- se utiliza si necesitas modificar un proyecto de otro uuario y hacer cambios con clone link-ssh-de mi usuario . - push . 
-  [sincronizar-el-fork-con-el-original-para-el-pull-request]
        -  la opcion en github "sync fork" , sirve para o comparar los nuevos commits agregados ah ese repo remoto con tu repo local 
        o "update branch" para agregar los nuevos commits a tu repo local rama main .;


# pull request "pr" 
   -  si en el fork has hecho cambios (commits) y estan sincronizados puedes hacerle saber al usuario de ese repositorio tus cambios
   -  solicitando que esos cambios se añadan al repositorio principal de ese usuario (eso es el pull request) 
   -  tanto fork como pull request son acciones que se hacen en la platafora de github , no comandos de git . 




### Comandos linux de mucha ayuda 

\\tree\\ equivalente a "ls" con mejor distribucion , y el modificador -a para ver directorio oculto

\\cat \\ te permite leer / copiar , el contenido de un archivo 

\\history -"numero que quieres que muestre ('n')"\\ este comando te permite observar los 'n' ultimos comandos comandos 












# vim:ft=zsh ts=2 sw=2 sts=2
#
# agnoster's Theme - https://gist.github.com/3712874
# A Powerline-inspired theme for ZSH
#
# # README
#
# In order for this theme to render correctly, you will need a
# [Powerline-patched font](https://github.com/Lokaltog/powerline-fonts).
# Make sure you have a recent version: the code points that Powerline
# uses changed in 2012, and older versions will display incorrectly,
# in confusing ways.
#
# In addition, I recommend the
# [Solarized theme](https://github.com/altercation/solarized/) and, if you're
# using it on Mac OS X, [iTerm 2](https://iterm2.com/) over Terminal.app -
# it has significantly better color fidelity.
#
# If using with "light" variant of the Solarized color schema, set
# SOLARIZED_THEME variable to "light". If you don't specify, we'll assume
# you're using the "dark" variant.
#
# # Goals
#
# The aim of this theme is to only show you *relevant* information. Like most
# prompts, it will only show git information when in a git working directory.
# However, it goes a step further: everything from the current user and
# hostname to whether the last call exited with an error to whether background
# jobs are running in this shell will all be displayed automatically when
# appropriate.

### Segment drawing
# A few utility functions to make it easy and re-usable to draw segmented prompts

CURRENT_BG='NONE'

case ${SOLARIZED_THEME:-dark} in
    light) CURRENT_FG='white';;
    *)     CURRENT_FG='black';;
esac

# Special Powerline characters

() {
  local LC_ALL="" LC_CTYPE="en_US.UTF-8"
  # NOTE: This segment separator character is correct.  In 2012, Powerline changed
  # the code points they use for their special characters. This is the new code point.
  # If this is not working for you, you probably have an old version of the
  # Powerline-patched fonts installed. Download and install the new version.
  # Do not submit PRs to change this unless you have reviewed the Powerline code point
  # history and have new information.
  # This is defined using a Unicode escape sequence so it is unambiguously readable, regardless of
  # what font the user is viewing this source code in. Do not replace the
  # escape sequence with a single literal character.
  # Do not change this! Do not make it '\u2b80'; that is the old, wrong code point.
  SEGMENT_SEPARATOR=$'\ue0b0'
}

# Begin a segment
# Takes two arguments, background and foreground. Both can be omitted,
# rendering default background/foreground.
prompt_segment() {
  local bg fg
  [[ -n $1 ]] && bg="%K{$1}" || bg="%k"
  [[ -n $2 ]] && fg="%F{$2}" || fg="%f"
  if [[ $CURRENT_BG != 'NONE' && $1 != $CURRENT_BG ]]; then
    echo -n " %{$bg%F{$CURRENT_BG}%}$SEGMENT_SEPARATOR%{$fg%} "
  else
    echo -n "%{$bg%}%{$fg%} "
  fi
  CURRENT_BG=$1
  [[ -n $3 ]] && echo -n $3
}

# End the prompt, closing any open segments
prompt_end() {
  if [[ -n $CURRENT_BG ]]; then
    echo -n " %{%k%F{$CURRENT_BG}%}$SEGMENT_SEPARATOR"
  else
    echo -n "%{%k%}"
  fi
  echo -n "%{%f%}"
  CURRENT_BG=''
}

### Prompt components
# Each component will draw itself, and hide itself if no information needs to be shown

# Context: user@hostname (who am I and where am I)
prompt_context() {
  if [[ "$USERNAME" != "$DEFAULT_USER" || -n "$SSH_CLIENT" ]]; then
    prompt_segment 218 magenta "%(!.%{%F{yellow}%}.)%n@%m"
  fi
}

# Git: branch/detached head, dirty status
prompt_git() {
  (( $+commands[git] )) || return
  if [[ "$(command git config --get oh-my-zsh.hide-status 2>/dev/null)" = 1 ]]; then
    return
  fi
  local PL_BRANCH_CHAR
  () {
    local LC_ALL="" LC_CTYPE="en_US.UTF-8"
    PL_BRANCH_CHAR=$'\ue0a0'         # 
  }
  local ref dirty mode repo_path

   if [[ "$(command git rev-parse --is-inside-work-tree 2>/dev/null)" = "true" ]]; then
    repo_path=$(command git rev-parse --git-dir 2>/dev/null)
    dirty=$(parse_git_dirty)
    ref=$(command git symbolic-ref HEAD 2> /dev/null) || \
    ref="◈ $(command git describe --exact-match --tags HEAD 2> /dev/null)" || \
    ref="➦ $(command git rev-parse --short HEAD 2> /dev/null)"
    if [[ -n $dirty ]]; then
      prompt_segment yellow black
    else
      prompt_segment green $CURRENT_FG
    fi

    local ahead behind
    ahead=$(command git log --oneline @{upstream}.. 2>/dev/null)
    behind=$(command git log --oneline ..@{upstream} 2>/dev/null)
    if [[ -n "$ahead" ]] && [[ -n "$behind" ]]; then
      PL_BRANCH_CHAR=$'\u21c5'
    elif [[ -n "$ahead" ]]; then
      PL_BRANCH_CHAR=$'\u21b1'
    elif [[ -n "$behind" ]]; then
      PL_BRANCH_CHAR=$'\u21b0'
    fi

    if [[ -e "${repo_path}/BISECT_LOG" ]]; then
      mode=" <B>"
    elif [[ -e "${repo_path}/MERGE_HEAD" ]]; then
      mode=" >M<"
    elif [[ -e "${repo_path}/rebase" || -e "${repo_path}/rebase-apply" || -e "${repo_path}/rebase-merge" || -e "${repo_path}/../.dotest" ]]; then
      mode=" >R>"
    fi

    setopt promptsubst
    autoload -Uz vcs_info

    zstyle ':vcs_info:*' enable git
    zstyle ':vcs_info:*' get-revision true
    zstyle ':vcs_info:*' check-for-changes true
    zstyle ':vcs_info:*' stagedstr '✚'
    zstyle ':vcs_info:*' unstagedstr '±'
    zstyle ':vcs_info:*' formats ' %u%c'
    zstyle ':vcs_info:*' actionformats ' %u%c'
    vcs_info
    echo -n "${${ref:gs/%/%%}/refs\/heads\//$PL_BRANCH_CHAR }${vcs_info_msg_0_%% }${mode}"
  fi
}

prompt_bzr() {
  (( $+commands[bzr] )) || return

  # Test if bzr repository in directory hierarchy
  local dir="$PWD"
  while [[ ! -d "$dir/.bzr" ]]; do
    [[ "$dir" = "/" ]] && return
    dir="${dir:h}"
  done

  local bzr_status status_mod status_all revision
  if bzr_status=$(command bzr status 2>&1); then
    status_mod=$(echo -n "$bzr_status" | head -n1 | grep "modified" | wc -m)
    status_all=$(echo -n "$bzr_status" | head -n1 | wc -m)
    revision=${$(command bzr log -r-1 --log-format line | cut -d: -f1):gs/%/%%}
    if [[ $status_mod -gt 0 ]] ; then
      prompt_segment yellow black "bzr@$revision ✚"
    else
      if [[ $status_all -gt 0 ]] ; then
        prompt_segment yellow black "bzr@$revision"
      else
        prompt_segment green black "bzr@$revision"
      fi
    fi
  fi
}

prompt_hg() {
  (( $+commands[hg] )) || return
  local rev st branch
  if $(command hg id >/dev/null 2>&1); then
    if $(command hg prompt >/dev/null 2>&1); then
      if [[ $(command hg prompt "{status|unknown}") = "?" ]]; then
        # if files are not added
        prompt_segment red white
        st='±'
      elif [[ -n $(command hg prompt "{status|modified}") ]]; then
        # if any modification
        prompt_segment yellow black
        st='±'
      else
        # if working copy is clean
        prompt_segment green $CURRENT_FG
      fi
      echo -n ${$(command hg prompt "☿ {rev}@{branch}"):gs/%/%%} $st
    else
      st=""
      rev=$(command hg id -n 2>/dev/null | sed 's/[^-0-9]//g')
      branch=$(command hg id -b 2>/dev/null)
      if command hg st | command grep -q "^\?"; then
        prompt_segment red black
        st='±'
      elif command hg st | command grep -q "^[MA]"; then
        prompt_segment yellow black
        st='±'
      else
        prompt_segment green $CURRENT_FG
      fi
      echo -n "☿ ${rev:gs/%/%%}@${branch:gs/%/%%}" $st
    fi
  fi
}

# Dir: current working directory
prompt_dir() {
  prompt_segment blue $CURRENT_FG '%~'
}

# Virtualenv: current working virtualenv
prompt_virtualenv() {
  if [[ -n "$VIRTUAL_ENV" && -n "$VIRTUAL_ENV_DISABLE_PROMPT" ]]; then
    prompt_segment blue black "(${VIRTUAL_ENV:t:gs/%/%%})"
  fi
}

# Status:
# - was there an error
# - am I root
# - are there background jobs?
prompt_status() {
  local -a symbols

  [[ $RETVAL -ne 0 ]] && symbols+="%{%F{red}%}✘"
  [[ $UID -eq 0 ]] && symbols+="%{%F{yellow}%}⚡"
  [[ $(jobs -l | wc -l) -gt 0 ]] && symbols+="%{%F{cyan}%}🦔"   #🦔🌗🌌卐☭( ͡❛ 👅 ͡❛)

  [[ -n "$symbols" ]] && prompt_segment default default "$symbols" #color de fondo
}

#AWS Profile:
# - display current AWS_PROFILE name
# - displays yellow on red if profile name contains 'production' or
#   ends in '-prod'
# - displays black on green otherwise
prompt_aws() {
  [[ -z "$AWS_PROFILE" || "$SHOW_AWS_PROMPT" = false ]] && return
  case "$AWS_PROFILE" in
    *-prod|*production*) prompt_segment red yellow  "AWS: ${AWS_PROFILE:gs/%/%%}" ;;
    *) prompt_segment green black "AWS: ${AWS_PROFILE:gs/%/%%}" ;;
  esac
}

## Main prompt
build_prompt() {
  RETVAL=$?
  prompt_status
  prompt_virtualenv
  prompt_aws
  prompt_context
  prompt_dir
  prompt_git
  prompt_bzr
  prompt_hg
  prompt_end
}

PROMPT='%{%f%b%k%}$(build_prompt) '














