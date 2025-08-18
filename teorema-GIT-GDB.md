+ \\main\\ es una rama en Git es como una "linea de tiempo " de tu proyecto y 
main avanza automaticamente al nuevo commit 

+ \\HEAD\\ es como un puntero temporal que te permite ver el codigo en una version especifica
HEAD se puede mover pero la rama main sigue apuntando a su ultimo commit 
el estado "head detached" quiere decir "cabeza desprendida" o "desacoplado" , y te permite observar esa linea temporal 

[propiedades-a-tener-muy-en-cuenta]{
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
}


[ORDEN-DE-GIT]{
    tenes lo siguiente : "~/proyecto/d1-d2 y en d1/d3 y d2/d4" en "proyecto" se encuentra el ".git" 
    el "add" y "commit" afectan diferentes a cada directorio: 

**1ero**para definir lo que hace (git add .) hay que entender los (_directorios_) , el directorio que tiene subdirectorios 
es (_directorio padre_) [por ejemplo el "d1"] asi asu vez , este tiene hermano [" d2 "] y la subcarpeta "d3" seria el (_directorio hijo_)
de "d1" y asu vez estos directorios son subcarpetas de "proyecto", osea que este directorio , es el (_directorio padre de esos directorios_)
como das a entender , ningun directorio es "padre definitivo de todos" pero si son "padres de algunos" entonces : 

 el "." del " add "  osea al hacer "git add ." staguea archivos que sean hijos del directorio donde te encuentres ,
 si hiciste cambios en "proyecto/archivo.txt y proyecto/d1/archivo.txt" si te encontras en "proyecto/d1"
y haces un "git add ." staguea unicamente a los hijos (archivos y subcarpetas) del directorio "d1" asi no a sus (_hermanos_ ni _padres_)
(ni a d2/archivo.txt ni a "proyecto/archivo.txt") , agregaria al stague a (d1/archivo.txt y d1/d3 si hay cambios ah y ).

por eso podes hacer un (git add .) desde "proyecto" si tienes cambios en diferentes repositorios , debido a que es el directorio padre que 
contiene los cambios de cada subdirectorio y necesitarias un solo "git add ." "."-> (archivos/directorios hijos_) y se commitearia lo
que hay en el area de (_stage_) osea , todos los cambios del "proyecto y sus subdirectorios" , asi no su hermano si es que lo tiene 

o puedes unir en el (_stage_) con (git add .) en cada directorio y luego commitear una sola vez en cualquier repositorio donde estes 
barado , ya que el "commit -m".." " se registra en el repo definido por el (.git) osea el (_~/proyecto_)

si haces varios cambios en diferentes directorios de un proyecto y haces un "git status " para ver el area de (_stage_)
aparecera algo como "modificados ../../../archivo" es porque este archivo ah sido modificado pero no stagueado entonces 
necesitaras "subir" 3 directorios (1/2/3) 

pero ojo con el termino "subir" porque en realidad se refiere a subir de jerarquia (de hijo a padre y del padre al padre) ya 
que el "add ." significa directorio actual hacia abajo (sus hijos _subcarpetas_), no hacia arriba sus padres ni a hermanos.
entonces al decir subir se piensa en esto "1 -> 2 -> 3" y enrealidad se refiere a esot "1 <- 2 <- 3 " en el caso de los directorios


**2do** si tenes ~/proyecto/d1/d3/otroPROYECTo [(.git en proyecto y otro .git en otroPROYECTo)] y hacer un "git add ." en "~/proyecto" auque (_otroPROYECTO_) sea hijo de "poryecto" , 
los cambios que hayan en (_otroPROYECTO_) necesitaran su propio "git add ." y su propio "git commit -m "" "
Dicho de otra manera:
proyecto y otroPROYECTO son repos distintos
Uno no ve los cambios del otro, aunque estén anidados en carpetas.
    
}


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
            - y/o si si estan listos para ser committeados , de lo contrario , no se podran (en verde se pueden,  y en rojo no estan en stage)



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
{
con cada commit , su descripcion puesta en el ( -m "" ) , sera visible en github unicamente en los archivo modificado ,
ej _ si modificaste "fundamentos.c" y tenes mas archivos en el repo "d1" , "d2" etc , y hacer "add . " y "commit -m" ,
al subirlo "push" , la descripcion sera vinculada al o los archivos modificados .

eso si , si modificaste muchos archivos , y necesitas una descripcion para cada uno , entonces , o adieres al 
area de stage de a uno (git add archivo.extencion) y lo commiteas de a uno , o a medida que vas modificando archivos
vas adiriendolos y commiteandolos (no hace falta pushear en el momento)
                                                                        }



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

\\helpp\\ comando alias que me permite ver los comandos mas utilizados por mi , para editar , en /home/blas "~" , dentro del archivo ".comandos-mi-guia.txt"
editalo con "nano .. .. . "
                                          
\\tree\\ equivalente a "ls" con mejor distribucion , y el modificador -a para ver directorio oculto

\\cat \\ te permite leer / copiar , el contenido de un archivo 

\\history -"numero que quieres que muestre ('n')"\\ este comando te permite observar los 'n' ultimos comandos comandos 


==========================================================================================================================================

                                                [GDB DEPURADOR] - comandos

para utilizar el depurador gdb hay que coompilarl el archivo con \\[gcc -g archivo.c -o nuevo-nombre.exe]\\ el modificador "-g" genera 
una copia de tu programa ejecutable para poder depurarlo .

                                            📌 Iniciar y preparar la depuración: 
##  gdb ./programa.ejecutable    
    - Inicia GDB con tu programa compilado o usa "./programa" para simplemente ejecutarlo
##  run ||                 
    - Ejecuta el programa en gdb --  hasta el primer breakpoint
# set args arg1 arg2
    - Pasa argumentos al programa  

                                            🛠️ Control del flujo de ejecución:

##  BREAK POINT 
    - 









