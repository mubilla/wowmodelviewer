# WoW Model Viewer — contexto del fork e instrucciones de trabajo

### Publicación de eventos nativos — 23 de octubre de 2026

Publicados en `codex/native-animation-events`, commit `3533c814`, PR
https://github.com/wowmodelviewer/wowmodelviewer/pull/131 contra `develop` oficial.
Depende del PR #128 y parte de su head `8ae5f9b7`; el diff incluye temporalmente
esa dependencia. Integrar #128 primero. No incluye equipo independiente ni efectos.
El worktree aislado está limpio y sincronizado con el fork. Compilación Release x64,
28 comprobaciones sintéticas y los 13 informes del corpus aprobados. Exportación
oculta nueva de ambas posturas (tres takes por postura), FBX ordinario (un take),
lector independiente y reimportación correctos; también retirada de sidecar
obsoleto y rechazo de escritura fallida. Evidencia `native-events-20261023/pr-*`
en `C:/Users/mauri/WMVDev/`. Sin nueva revisión visual ni cambios del consumidor.
La instalación diaria conserva la versión verificada tras la revisión. El commit
del 24 de octubre guarda también los eventos nativos y su documentación local en
`my-develop`, por petición de Mauricio; no fusiona otras ramas.

### Eventos nativos de animación — 23 de octubre de 2026

Implementado en `my-develop` y guardado en el commit del 24 de octubre. Toda exportación FBX animada
incluye automáticamente `<archivo>.fbx.wmvevents.json` (`wmv.animation-events`,
versión 1). Cada catálogo de cuerpo/personalización, incluidas ambas posturas
del orco, referencia su propio sidecar. Conserva todos los códigos, payloads,
huesos, posiciones, timestamps y relojes globales; diferencia eventos ausentes
de errores de lectura. Sin cambios de UI ni modificaciones de WowSpellViewer.

PEDC contiene sus timestamps residentes incluso para clips con huesos externos;
MD21 puede requerir AFM2 para eventos externos. No confundirlos con AFSB.
PABC, alias, índices locales y límites se validan; herencia más profunda que el
padre inmediato se rechaza explícitamente. Alcance real: Retail `wow`
12.1.5.70391, layout 274. Globales y blacklist tienen cobertura sintética;
no se afirma soporte comprobado de Classic/TBC u otros builds.

Compilación Release x64 y 24 comprobaciones sintéticas aprobadas. Lector
independiente: 13 cuerpos, 1.965 clips, 3.802 timestamps. Paquete completo del
orco: 151 takes por postura, 403/331 timestamps y 28/35 clips sin eventos.
Además, 13 FBX normales con 103 takes. Reimportación comprobó takes, duraciones
y referencias a sidecars; regresión ósea: 3.133.460 muestras sin discrepancias.
La inspección de geometría, huesos, pesos y materiales coincide con los paquetes
previos; los defaults de apariencia son los de la nueva selección. Pasaron la
retirada de sidecar obsoleto al exportar sin animaciones y el rechazo de escritura
fallida. El host oculto puede devolver 255 con estado `OK`: comprobar el archivo
de estado y las verificaciones, no sólo el código del proceso.

Instalación diaria actualizada: dos archivos reemplazados y siete hashes
SHA-256 verificados, con respaldo/manifiesto en
`C:/Users/mauri/WMVDev/native-events-20261023/install/`. Se cerró WMV normalmente
y no se volvió a abrir. No hubo revisión visual ni entrada simulada. Los nuevos
recursos de prueba están separados en
`WowBattlegrounds/LocalAssets/Prepared/WoWModelViewer/native-events-20261023/`;
no se sobrescribieron paquetes compartidos anteriores. Evidencia en
`C:/Users/mauri/WMVDev/native-events-20261023/`. Contrato del consumidor y límites
en `docs/native-animation-events.md`; pruebas en
`scripts/tests/native-animation-events/`.

## Objetivo

Este proyecto es el fork personal de Mauricio (`mubilla`) de WoW Model Viewer.
Su objetivo es mejorar una herramienta que Mauricio utiliza activamente para explorar modelos
de World of Warcraft y extraer modelos, texturas y animaciones para realizar pruebas en Unity
y trabajar en su proyecto **WowBattlegrounds**.

Mauricio quiere contribuir al proyecto oficial mediante pull requests con arreglos y mejoras
acotados, revisados y probados. También quiere mantener una versión propia utilizable con todas
sus mejoras, independientemente de que el proyecto oficial las acepte o tarde en integrarlas.

Proyecto objetivo: `C:\Users\mauri\workspace\WowBattlegrounds`, en una carpeta al mismo nivel que este checkout.
La carpeta de destino de las exportaciones se determinará según la tarea.
No modificar ese proyecto ni sus assets salvo que la tarea lo requiera.

## Repositorios y remotos

| Remoto local | Repositorio | Función |
| --- | --- | --- |
| `origin` | https://github.com/wowmodelviewer/wowmodelviewer | Proyecto oficial; fuente de cambios y destino de las contribuciones por PR. |
| `fork` | https://github.com/mubilla/wowmodelviewer | Fork de Mauricio; destino habitual de los pushes de nuestro trabajo. |

**En este checkout `origin` es el proyecto oficial, no el fork.** Comprobar los remotos antes
de publicar. No enviar nuestros commits directamente a las ramas del repositorio oficial.

## Ramas y su significado

### Estado vigente — 22 de octubre de 2026

`develop` está actualizado a `origin/develop` (`678071f0`) y publicado también en
`fork/develop`. Por petición de Mauricio, `my-develop` se reconstruyó desde esa
base, con dos contribuciones independientes y los ajustes locales de compilación.
El PR #123 ya está integrado en la base oficial y no se reaplicó como cambio propio.

- `codex/fbx-orc-postures`, commit `93e68ecc`, PR
  https://github.com/wowmodelviewer/wowmodelviewer/pull/128: caras y dos posturas del
  orco masculino, cada una con su modelo y esqueleto; catálogo v2 y caché esquema 23.
- `codex/standalone-equipment-export`, commit `1581dfb8`, PR
  https://github.com/wowmodelviewer/wowmodelviewer/pull/129: exportación independiente
  de equipo, carpetas portables por set/arma/pieza, sugerencia de nombre y validación
  mediante aviso flotante. Incluye documentación y pruebas. No depende del PR #128.

Ambas ramas parten de `678071f0` y están publicadas en el fork. `my-develop` integra
las dos; sus ajustes CMake y este documento son personales y quedan fuera de los PR.
Los apartados históricos que indican cambios pendientes de commit quedan superados
para estas funcionalidades. Los directorios locales de tareas no forman parte de los PR.

El estado anterior completo, incluidos los cambios pendientes, está respaldado
localmente y en `fork/backup/my-develop-2026-10-22` (`aea592e9`). También se conserva
`backup/my-develop-2026-10-13`; no borrar ninguno sin autorización. Las copias de los
archivos anteriores y la evidencia nueva están en
`C:/Users/mauri/WMVDev/rebuild-my-develop-20261022/`.

Las futuras contribuciones independientes deben partir de `develop` actualizado;
`my-develop` sigue siendo la rama de uso diario y no la base de PR acotados.
Las rutas particulares permanecen en `build64/CMakeCache.txt` y
`ThirdParty/wxWidgets33` enlaza a la dependencia local instalada.

Inventario local comprobado el **28 de septiembre de 2026**. Revisar Git antes de actuar;
esta tabla describe la organización acordada, no garantiza que las referencias sigan iguales.

| Rama | Propósito y seguimiento |
| --- | --- |
| `develop` | Espejo de `origin/develop`. Mantenerla como referencia del código oficial, sin mejoras propias. También está publicada como `fork/develop`. |
| `my-develop` | Rama de integración y uso diario de Mauricio. Reúne las mejoras propias; sigue `fork/my-develop`. |
| `fix/equipment-remove-buttons` | Corrección de la retirada individual de equipo. Publicada en el fork e integrada en `my-develop`. |
| `codex/animation-list-improvements` | Ordenación del listado de animaciones por nombre, ID y duración. Publicada en el fork e integrada en `my-develop`. |
| `codex/fbx-animation-id-range` | Selección y búsqueda de animaciones para exportar a FBX mediante IDs de WoW. Publicada en el fork e integrada en `my-develop`. |
| `codex/npc-equipment-1` | Visualización de armas en NPC exclusivos en Unity. Parte de `origin/develop`, publicada en el fork; PR #64. |
| `codex/npc-equipment-2` | Exportación FBX del equipo de NPC. Parte de `codex/npc-equipment-1` y contiene ambos arreglos; publicada en el fork; PR #65. |

Para una contribución independiente, partir de `develop` actualizado y trabajar en una rama
específica del arreglo o funcionalidad. El PR debe comparar esa rama del fork contra `develop`
del proyecto oficial. Evitar incluir otras mejoras o preferencias personales en el mismo PR.

Integrar las mejoras en `my-develop` cuando Mauricio lo solicite. Los cambios exclusivamente
personales pueden desarrollarse allí si así lo pide. No usar `my-develop` como origen de un PR
acotado cuando arrastre modificaciones ajenas a la contribución.

Antes de cambiar de rama o integrar, comprobar el estado del árbol y los worktrees: una rama
puede estar abierta en otro checkout. Preservar los cambios pendientes; no descartarlos ni
mezclarlos con otra tarea. Para commits, seguir el skill personal `mau-commit` de Mauricio.

## Trabajo realizado

### Conjuntos y alternativas de transmog � 3 de octubre de 2026

El filtro por armadura est� publicado en `codex/item-set-armor-filters`, commit
`32215a2f`, PR https://github.com/wowmodelviewer/wowmodelviewer/pull/73.
La ampliaci�n con `TransmogSet` y panel de alternativas por ItemID/apariencia est�
implementada **sin commit ni push** en ese worktree y aplicada al �rbol de trabajo
local de `my-develop` para su compilaci�n diaria; se preservaron los cambios previos.
No interpretar esta aplicaci�n local como un merge o publicaci�n del PR actualizado.
Ambas compilaciones Release x64 pasaron y la instalaci�n local se actualiz� con 13
archivos verificados por hash. Pruebas ocultas: 5.838 selecciones de conjuntos,
18.697 cambios de objeto, 1.196 de apariencia, regresiones del selector individual
y base sin transmog. Esquema 14: se reconstruir� la cach� al cargar el juego.
La revisi�n visual y de assets renderizados sigue pendiente. Criterios, l�mites y
pruebas en `docs/item-set-filters.md`; evidencia local en
`C:/Users/mauri/WMVDev/item-set-filters/`. No se abri� la aplicaci�n tras instalar.

### Animaciones: variantes, alias y archivos — 2 de octubre de 2026

Arreglo revisado e integrado en `my-develop`: identidad por secuencia/variante,
resolución coherente de alias, tiempos y buffers, cierre de archivos propios sin
alterar consumidores compartidos, y muestreo FBX de la secuencia seleccionada.
La contribución independiente está en `codex/animation-source-resolution`, basada
en `origin/develop` (`7b518d3b`), commit `259de366`, PR
https://github.com/wowmodelviewer/wowmodelviewer/pull/69. Incluye la corrección mínima
de selección de clips también presente en #63; no depende del resto de su interfaz.

Compilación Release x64 e instalación local verificadas. La rama independiente
también compiló y pasó las regresiones: 14 clips de HumanMale, 284.472 muestras
comparadas con un lector y muestreador independientes, y 15 selecciones ocultas del
visor sin bloqueos. Evidencia local: `C:/Users/mauri/WMVDev/animation-source-fix-20261002/pr/`.
El FBX original usado en la prueba se conserva. La exportación validada contiene
esqueleto y animaciones; la exportación con malla/materiales falla en PixelBuffer
también con los binarios previos. La revisión visual de las poses sigue pendiente.
Se deben reconstruir juntos los componentes C++ porque cambia el tamaño de `GameFile`.
Unity y el protocolo no cambian por este arreglo.

### Integrado en `my-develop`

- **Retirada individual de equipo** (`a908c92c`): habilitar los botones de eliminación por
  slot y actualizar correctamente el estado del equipo. La atenuación corresponde al botón
  de eliminar cuando no hay pieza; el texto `--- None ---` debe mantener un aspecto consistente.
- **Inicio maximizado respetando la barra de tareas** (`efa7361c`): utilizar el comportamiento
  normal de ventana maximizada de Windows, sin fullscreen que tape la barra de tareas.
- **Ordenación de animaciones** (`a637013b`): clic en las cabeceras de nombre, ID o duración;
  otro clic invierte el orden. Mauricio probó y aprobó la interfaz.
- **Opciones de exportación FBX** (`e4765c31`): listado con checkboxes, búsqueda, contador,
  columnas de nombre e ID y ordenación por cabeceras; selección por rango de IDs de WoW.
  El rango inicial es 0–225. Mantener la selección asociada a la animación original al buscar
  u ordenar. También se corrigió el mapeo de los clips seleccionados hacia el exportador.
  Mauricio probó y aprobó la vista y realizó una exportación de Rehgar.

Se crearon estas contribuciones al proyecto oficial:

- Equipo individual: https://github.com/wowmodelviewer/wowmodelviewer/pull/61
- Ordenación de animaciones: https://github.com/wowmodelviewer/wowmodelviewer/pull/62
- Selección de animaciones FBX: https://github.com/wowmodelviewer/wowmodelviewer/pull/63
- Equipo de NPC, parte 1 (Unity): https://github.com/wowmodelviewer/wowmodelviewer/pull/64
- Equipo de NPC, parte 2 (FBX): https://github.com/wowmodelviewer/wowmodelviewer/pull/65

La creación de estos PR no significa que estén aceptados; consultar su estado cuando sea relevante.

### Cambios guardados en `my-develop` el 28 de septiembre

Estos cambios tienen commits propios en `my-develop`. Los arreglos de NPC también se aislaron
mediante cherry-pick sobre la base oficial, sin arrastrar los cambios personales ni los PR #62/#63:

- Seleccionar inicialmente el perfil de cliente más reciente (`7f5384f5`) disponible en el diálogo de
  versiones (`ClientChoiceDialog.cpp`); actualmente corresponde a Midnight.
- **Armas de NPC exclusivos en Unity** (`37c4b7cc`): el emisor C++ envía una escena de equipo separada
  (`attachmentsOnly`, protocolo 7) y Unity la vincula a la carga correspondiente. El NPC conserva
  su ruta habitual de apariencia y animación; no se clasifica como personaje racial.
  Los modelos de equipo siguen el hueso y desplazamiento de su punto de sujeción. Si falta
  un punto válido, se omite la pieza, se retira cualquier pieza obsoleta y se informa del problema,
  sin colocar el arma arbitrariamente en el origen.
- Adaptaciones locales de dependencias en `CMakeLists.txt` (`98418d04`). No incluir rutas particulares
  del equipo de Mauricio en contribuciones al proyecto oficial.
- **Equipo de NPC exclusivos en FBX** (`319519b6`): el proceso separado conserva
  `-mo`/`-npc` y recibe además `-fbxequipment` con una instantánea temporal de las dos manos.
  Restaura IDs, apariencias y ranuras vacías sin aplicar personalización racial al cuerpo.
  Los personajes raciales mantienen su `.chr` completo. Documentación y pruebas repetibles:
  `docs/fbx-npc-equipment.md` y `scripts/tests/fbx-equipment/`. Es un arreglo independiente
  del visor Unity. Ambos se publicaron de forma acumulativa: `codex/npc-equipment-1`
  desde `origin/develop` y `codex/npc-equipment-2` desde la primera. Los commits aislados
  son `13080587` y `981dea37`; el segundo añade únicamente el helper `WoWItem::isEquipped()`
  como dependencia mínima de #61. Los dos PR tienen como destino `develop`; revisar e integrar
  primero la parte 1 y después la parte 2, que inicialmente incluye ambos commits.
  Tras integrar #64, actualizar la base de la segunda rama y verificar que su diff solo conserva FBX.
  Compilado e instalado el 28 de septiembre: seis casos de exportación y cuatro casos
  de rechazo aprobados. Tyrande exporta cuerpo, arco y 156 clips; se verificaron hueso,
  textura y movimiento numéricamente, sin revisión visual del FBX en Unity. Los FBX
  originales usados en las pruebas no se sobrescribieron.

Verificación de las ramas aisladas el 28 de septiembre: ambas compilaron en Release x64.
La parte 1 pasó una nueva secuencia de ocho pasos con Tyrande (ambas manos, retirada y recarga).
La parte 2 pasó de nuevo seis exportaciones y cuatro rechazos de instantáneas inválidas.
Evidencia local: `C:\Users\mauri\WMVDev\fbx-npc-pr\`. La instalación diaria conserva el ejecutable
ya verificado; las pruebas de aislamiento utilizaron el runtime del worktree separado.

Caso verificado del arreglo de NPC: `creature/tyrande3/tyrande3.m2` (FileDataID `4198151`),
con `Kaldorei Moon Bow` (ItemID `213160`) en la mano izquierda; modelo del arco `524474`.
Se compiló C++ y Unity, se aprobaron 577 comprobaciones del runtime y se hicieron pruebas
con archivos reales de ambas manos, cambio/retirada de armas, recarga del NPC, personaje racial
y montaje/desmontaje. Se actualizó la instalación local. Esto describe la validación del
27 de septiembre de 2026, no una garantía para cambios futuros.

## Arquitectura y entorno local

- La aplicación principal está en C++ y wxWidgets. El visor integrado actual es Unity.
- WMV proporciona assets y metadatos a Unity mediante IPC; el visor no utiliza una exportación
  FBX intermedia. Revisar emisor y receptor cuando un cambio atraviese esa frontera.
- El canvas OpenGL permanece como servicio interno heredado; no reintroducirlo como visor.
- Código fuente del receptor: `Tools/UnityRendererProject/Assets/Scripts`.
- Documentación del visor: `docs/unity-renderer/README.md`.
- El exportador FBX tiene una ruta independiente. Un fallo visual de Unity no demuestra un
  fallo de exportación; comprobarlos por separado cuando corresponda.
- Checkout principal: `C:\Users\mauri\workspace\WowModelViewer`.
- Instalación utilizada por Mauricio: `C:\Users\mauri\Applications\WoW Model Viewer`.
- Compilación C++ local: directorio `build64`, configuración `Release`, Visual Studio 2026.
- Proyecto Unity de compilación local: `C:\Users\mauri\WMVDev\WmvUnityRenderer`.
- Editor usado para las últimas compilaciones: Unity `6000.3.25f1`.
- Script local de compilación de Unity: `C:\Users\mauri\WMVDev\build-unity.ps1`.
  Sincroniza los scripts y recursos del repositorio al proyecto local antes de compilar.

Estos paths describen esta máquina. Verificar que existan y no convertirlos en requisitos
del repositorio oficial ni versionar los assets extraídos del juego.

## Forma de trabajar con Mauricio

- Comunicarse en español y dar actualizaciones breves durante el trabajo. Distinguir lo que
  se ha inspeccionado, implementado, compilado, probado, instalado y publicado.
- Tras compilar y verificar una mejora, **actualizar la instalación local**: es una preferencia
  explícita de Mauricio. Si hace falta cerrarla para reemplazar archivos, se puede cerrar sin
  volver a pedir permiso. Verificar los archivos copiados; actualizar ambos componentes si
  cambió el protocolo entre WMV y Unity.
- **No abrir, enfocar ni tomar control de la aplicación automáticamente.** Abrirla cuando
  Mauricio lo solicite. Preferir pruebas automáticas ocultas que no interrumpan su escritorio.
- Mantener las configuraciones necesarias para este equipo separadas de las contribuciones
  portables. Evitar que un arreglo personal rompa la configuración del proyecto oficial.
- Revisar y probar según el alcance. En un PR, describir fielmente las pruebas automáticas y
  las pruebas manuales realizadas por Mauricio; no decir que algo no se probó si él ya lo probó,
  ni presentar inspección de código como validación visual.
- Usar IDs reales de animaciones de WoW para selección compartida entre modelos. No confundirlos
  con posiciones de lista, índices internos o subanimaciones. El rango 0–225 es una selección
  de trabajo de Mauricio, no una enumeración universal de todas las animaciones necesarias.
- No commitear, publicar PR ni integrar ramas solo por haber terminado una implementación;
  seguir la autorización de la tarea y las instrucciones personales de Git de Mauricio.

Actualizar este documento cuando cambien acuerdos, ramas o hitos importantes, evitando que
los apartados históricos se interpreten como el estado actual del árbol de trabajo.

### Agrupaciones parciales de transmogs � 4 de octubre de 2026

La ampliaci�n pendiente de Item Sets ahora usa `TransmogSetGroupID` y `ClassMask`
para omitir �nicamente fragmentos sin grupo que est�n cubiertos por fuentes exactas
de variantes completas de una misma familia/grupo. Astral Chain pasa de 21 registros
a 6 variantes completas; se conservan piezas exclusivas y datos incompletos.
Esquema 15: reconstrucci�n de cach� al siguiente inicio. Ambas compilaciones Release
pasaron, se verific� la extracci�n de los campos desde DB2 y se actualizaron 11
archivos de la instalaci�n local con verificaci�n de hash. Los cambios siguen sin
commit ni push en el worktree del PR #73 y en el checkout diario. Ver criterios y
pruebas en `docs/item-set-filters.md`.

### Publicación de conjuntos transmog — 4 de octubre de 2026

La ampliación y la corrección de fragmentos se publicaron en
`codex/transmog-item-sets`, commit `c534546f`, PR
https://github.com/wowmodelviewer/wowmodelviewer/pull/74 contra `develop`.
El PR #73 sigue abierto: #74 depende de él e incluye temporalmente su commit
`32215a2f`. El worktree aislado usa ahora la nueva rama, está limpio y sigue
`fork/codex/transmog-item-sets`; la rama del #73 se conserva sin cambios.
Las copias locales de estos cambios en `my-develop` continúan pendientes, sin merge
ni commit en esa rama. Esta publicación reemplaza las referencias anteriores a
«sin commit ni push» únicamente para la contribución aislada. La instalación local
ya contiene la versión verificada con esquema 15. Mauricio confirmó las alternativas
de Sabellian y aprobó la UI; el conflicto visual de pecho/piernas de Southsea Cruise
continúa pendiente, como documenta el PR.
### Fragmentos con nombres distintos — 4 de octubre de 2026

Corrección local posterior al PR #74: la cobertura de fuentes exactas ya no exige
el mismo nombre localizado. Resuelve Draconic Plate frente a Verdant Plate y
Dread Vestment frente a Dread Plate. Conserva las restricciones de grupo y ClassMask.
Ambas compilaciones pasaron, 242.436 comprobaciones ocultas y 194 regresiones;
ejecutable diario instalado y verificado por hash. Pendiente la revisión visual
de estas familias. Cambio aplicado en ambos checkouts, todavía sin commit ni push.

La corrección de nombres distintos quedó publicada en el PR #74 mediante el commit
`a684813d` de `codex/transmog-item-sets`. Rama aislada limpia y sincronizada;
descripción del PR actualizada con la regla general y las pruebas finales.
Las copias de trabajo en `my-develop` se conservan pendientes.

### Settings y búsqueda de equipo por ID — 10 de octubre de 2026

Los cambios previos de conjuntos transmog del checkout diario quedaron guardados
por petición de Mauricio en `e2f6b978` de `my-develop`. Esto reemplaza las referencias
históricas a copias pendientes en esa rama.

La corrección de Settings (tamaño inicial, redimensionado y desplazamiento), búsqueda
por ItemID independiente de su visualización y objetos sin nombre quedó publicada
en `codex/settings-item-id`, commit `0df7dc58`, PR
https://github.com/wowmodelviewer/wowmodelviewer/pull/90 contra `develop`.
Parte de `origin/develop` actualizado (`d3643c06`) y no depende de los PR de transmog.
El worktree aislado compiló en Release x64 con wxWidgets 3.3.3 y pasó las pruebas
ocultas. La integración en `my-develop` conserva la misma corrección verificada e
instalada, adaptada a su esquema `Item.SheathType`; el PR usa `Item.SheatheType`.

El bastón 153575/185141 cargó el modelo 1717765, se equipó y se exportó para
verificación. Se actualizó la instalación diaria: solo cambió `wowmodelviewer.exe`
y se verificaron los hashes de los siete binarios C++. Copia previa y evidencia en
`C:/Users/mauri/WMVDev/settings-item-id-20261010/`. No se abrió WMV automáticamente.
Mauricio probó la instalación y confirmó que funcionó bien. Ver pruebas y límites
en `docs/settings-item-id.md`; otros DPI y animación del bastón no están cubiertos.

### Paquetes FBX de personalización — 17 de octubre de 2026

Implementación terminada en `my-develop`, todavía **sin commit ni push** y pendiente
 de revisión formal y QA. El diálogo FBX incorpora Appearance e Include equipment.
All customization options exporta recursos reutilizables, catálogo de opciones y
reglas de composición, equipo actual opcional y ajustes faciales vinculados al
esqueleto. Requiere un consumidor del catálogo; no incorpora un creador de personajes
en el proyecto consumidor ni modifica sus assets.

La aplicación de correcciones de huesos con animación se comprobó contra el visor
independiente de Wowhead: 16.000 muestras, 30 archivos y 1.000 correcciones. No se
presenta como captura numérica del cliente Blizzard. El Night Elf masculino de
12.1.5.70391 exportó 20 opciones, 127 elecciones, 201 texturas y 11 conjuntos de
ajustes. Pasaron reimportación FBX, referencias trasladables, cuatro clips, equipo
opcional, rechazo de destino existente y ausencia de paquete final al cancelar.
La variante femenina se rechaza por el archivo 1053758 ausente en esta instalación.
El fallo previo de PixelBuffer en exportación habitual con texturas combinadas se
reprodujo también con los binarios anteriores; Current appearance en modo raw pasó.
No se realizó validación visual del nuevo paquete ni de la nueva fila del diálogo.

Compilación Release x64 correcta. Instalación diaria actualizada con 12 archivos
verificados por SHA-256; respaldo y manifiesto en
`C:/Users/mauri/WMVDev/customization-install-20261017/`. Configuración y cachés
conservadas; el esquema 19 reconstruirá la caché al cargar el juego. No se abrió ni
enfocó WMV. Detalle y estado por criterio en `docs/fbx-customization.md` y
`tasks/in_progress/fbx-customization-options/implementation-status.md`.

### Corrección de exportación Night Elf femenina — 17 de octubre de 2026

Resuelto el bloqueo descrito arriba. La cara ordinaria 825 usa la forma base sin
corrección ósea; la referencia de archivo 1053758 no figura en CASC ni en BFID y su
FacePose no tiene ajustes por secuencia. Se contrastó el reinicio de correcciones
con el renderizador independiente. El exportador reconoce de forma acotada las
referencias neutras verificadas y mantiene el rechazo de otros archivos ausentes
o dañados. No se omite la cara. Las elecciones DH/DK permanecen excluidas.

También se evita exportar la superficie auxiliar de tipo de textura 9 sin material
para este contexto del modelo femenino. El catálogo registra esta exclusión.
La exportación femenina y reimportación FBX pasaron con nueve caras ordinarias,
137 elecciones, 276 texturas y tres clips. La prueba negativa de otra cara con
archivo ausente sigue rechazándose sin publicar paquete. La regresión masculina
conserva las 127 elecciones y 80 entradas de geometría. No hay validación visual
nueva del FBX ni cambios en el proyecto consumidor. Sin commit ni push.

### Rama aislada de personalización FBX — 17 de octubre de 2026

Preparada `codex/fbx-customization-options` en
`C:/Users/mauri/.codex/worktrees/fbx-customization-options/WowModelViewer`, desde
`origin/develop` actualizado (`8f4e791f`). Contiene los cambios de exportación,
pruebas y documentación, incluida la corrección femenina, sin ajustes personales,
objetos compilados ni el directorio local de tareas. La aplicación del parche no
presentó conflictos y se verificaron las copias y el diff; esta base nueva aún no
se ha compilado ni probado. La validación e instalación anteriores corresponden a
`my-develop`. Ambas copias conservan cambios sin commit; todavía no hay push ni PR
para esta funcionalidad. El checkout diario y su instalación se conservan.

### Publicación de personalización FBX — 18 de octubre de 2026

La contribución independiente está publicada en `codex/fbx-customization-options`,
commit `b2efb519`, PR https://github.com/wowmodelviewer/wowmodelviewer/pull/123 contra
`develop` oficial (`8f4e791f`). Revisión normal terminada: se corrigió la conservación
de armas envainadas, visibilidad de adjuntos y preferencias de casco en el snapshot.
Compilación Release y regresiones ocultas con reimportación FBX correctas. Sin nueva
validación visual ni QA formal. El worktree del PR está limpio y sincronizado con el
fork. Las adaptaciones a grupos de geosets siguen la semántica de la base oficial.
La implementación del checkout diario sigue pendiente de commit; no se ha fusionado
la base oficial nueva en `my-develop`.

Las correcciones compatibles de snapshots quedaron aplicadas también al árbol de
`my-develop`, compiladas y verificadas con las tres variantes de equipo, exportación
femenina y Current appearance raw (tres clips). Se actualizaron dos archivos de la
instalación diaria y se verificaron los doce hashes del paquete C++/esquema. Respaldo
y manifiesto: `C:/Users/mauri/WMVDev/customization-review-install-20261018/`.
No se abrió ni enfocó la aplicación. Estos cambios locales siguen sin commit.

### Exportación independiente de equipo — 19 de octubre de 2026

Implementada en `my-develop`, sin commit ni push. `File > Export equipment...`
elige primero la carpeta contenedora y luego permite seleccionar armadura, armas
y cuerpos por raza/sexo. La lista de razas sigue el orden de expansiones sin
cabeceras intermedias. Las piezas siempre conservan carpetas individuales; el
modo conjunto las agrupa bajo el nombre editable del set y añade reglas combinadas.
Las armas quedan fuera de esa carpeta, siempre por separado. El selector de sets
aporta una sugerencia de nombre, invalidada si cambia la armadura por otra vía.

El paquete incluye geometría de equipo y ropa suplementaria, esqueletos de
referencia, materiales, texturas compartidas y reglas de composición; no incluye
superficie del personaje ni animaciones. Requiere un consumidor de catálogos en
Unity. No se modificaron los proyectos consumidores. La conservación del
equipo al cambiar de personaje, comentada durante la investigación, sigue siendo
un cambio separado pendiente.

Compilación Release x64 y pruebas ocultas correctas. Battlelord's Plate exportó
ocho piezas más arco para Night Elf masculino y femenino (18 variantes). Pasaron
también exportación individual, solo armas, solo armadura, humano de ambos sexos,
gnomo masculino, goblin femenino y tauren masculino, capas/tabardos, nombres con
acentos, traslado de paquete, peticiones inválidas, cancelación y protección de
destino existente. La reimportación independiente verificó referencias, geometría,
ausencia de vértices sobrantes y pesos de huesos. Se comprobaron el diálogo oculto
y la propagación/invalidez de nombres de sets; el selector pasó 102.105 controles.
Las regresiones de FBX normal y personalización femenina conservaron tres clips.
No se realizó validación visual de estos nuevos paquetes en Unity ni de la nueva UI.

Instalación diaria actualizada: siete archivos reemplazados y doce hashes
verificados, conservando configuración y cachés. Se cerró WMV para actualizarlo;
no se volvió a abrir. Respaldo previo y manifiesto final en
`C:/Users/mauri/WMVDev/equipment-install-20261019/`; evidencia de pruebas en
`C:/Users/mauri/WMVDev/equipment-export-20261019/`. Contrato, límites y pruebas
repetibles en `docs/equipment-export.md` y `scripts/tests/equipment-export/`.

### Carpetas de equipo y validación de nombres — 20 de octubre de 2026

Actualizado en `my-develop`, sin commit ni push. La carpeta elegida es la
contenedora: el set se publica directamente con el nombre escrito en la interfaz,
y cada arma con su nombre en una carpeta hermana. Se elimina el nivel
`Equipment.equipment` del flujo de la interfaz. Cada set o arma contiene sus
catálogos y texturas, y puede trasladarse por separado. Las piezas de armadura
siguen teniendo carpetas individuales dentro del set. Los logs y estados se
guardan en `export-logs` de los datos locales de WMV.

El nombre del set conserva espacios, tildes y guiones bajos. Export se desactiva
con nombres inválidos, mostrando `Set name contains invalid characters:` y la
lista completa `< > : " / \ | ? *`. Los nombres vacíos, reservados por Windows,
con espacio/punto final o de más de 80 caracteres tienen mensajes específicos.
No se sustituyen caracteres silenciosamente ni se sobrescriben destinos existentes.

Compilación Release x64 y pruebas ocultas del diálogo correctas. Pasaron las
exportaciones reales de set y arma, piezas individuales, solo armas, nombres con
tildes, rechazos de nombres y colisiones con destinos existentes. La reimportación
FBX comprobó referencias internas, geometría y pesos; set y arma se trasladaron y
verificaron independientemente. No hubo revisión visual nueva de la UI ni en Unity.
Evidencia: `C:/Users/mauri/WMVDev/equipment-folders-20261020/`.

Instalación diaria actualizada con dos archivos reemplazados y doce hashes SHA-256
verificados. Respaldo previo y manifiesto en
`C:/Users/mauri/WMVDev/equipment-folders-install-20261020/`. Se cerró WMV de forma
normal para actualizarlo y no se volvió a abrir. Configuración y cachés conservadas.

El aviso de nombre inválido se cambió posteriormente por una ventana flotante
junto al campo. Se cierra al pulsar el textbox o el aviso; Export permanece
bloqueado mientras el nombre sea inválido. No ocupa espacio en el diseño ni mueve
los demás controles. El título Bodies quedó en negrita. Compilación Release x64
y pruebas ocultas de validación, eventos de cierre y posiciones/tamaños de los
controles correctas; no se hizo revisión visual nueva. Ejecutable diario actualizado
y verificado por SHA-256, sin abrir WMV. Respaldo y manifiesto en
`C:/Users/mauri/WMVDev/equipment-popup-install-20261020/`.

### Caras y posturas del orco masculino en FBX — 22 de octubre de 2026

Corregido en `my-develop`, sin commit ni push. El rechazo inicial de combinaciones
óseas ocultaba que Upright usa un modelo distinto de Hunched. El exportador resuelve
`ChrCustomizationCondModel` desde los datos del cliente y genera ambas posturas,
cada una con su propio modelo, esqueleto, animaciones y ajustes faciales. Se mantienen
los rechazos de correcciones incompatibles o recursos ausentes. La compatibilidad
nueva de modelos condicionales se limita al orco masculino; no implica soporte de
todas las transformaciones o efectos display-info.

Estos paquetes usan versión 2 del catálogo y requieren un consumidor que entienda
`conditional-body-models-v1`. `bodyVariants` selecciona el catálogo de cada postura;
los paquetes de un solo cuerpo conservan versión 1. La selección de clips entre
modelos usa AnimationID/SubAnimationID y registra los pares ausentes sin sustituirlos
por índices de lista. No se modificó WowBattlegrounds ni el visor Unity.

Compilación Release x64 correcta. Reimportación FBX y verificación contra la base
del cliente: nueve caras por postura (18 combinaciones), 135 elecciones, 367 texturas,
220 huesos y tres clips por cuerpo; también pasó equipo opcional con arco en ambas
posturas y traslado del paquete. La regresión Night Elf femenina conservó 137
opciones de elección, nueve conjuntos óseos y tres clips. Pasaron los rechazos de
asociación ósea incorrecta y archivo de la postura erguida ausente, sin paquete final;
la apariencia actual del orco exportó y reimportó tres clips. Las modificaciones de
la base privada usadas para las pruebas negativas se restauraron. Sin revisión
visual nueva de las poses en Unity.

Instalación diaria actualizada: ocho archivos cambiados y trece hashes SHA-256
verificados, incluida la definición nueva de la tabla. Esquema 20: se reconstruirá
la caché al cargar el cliente. Se cerró normalmente el diálogo FBX y WMV para instalar;
no se volvió a abrir. Respaldo/manifiesto en
`C:/Users/mauri/WMVDev/orc-customization-install-20261022/`; evidencia en
`C:/Users/mauri/WMVDev/orc-customization-20261022/`. Contrato y pruebas en
`docs/fbx-customization.md` y `scripts/tests/customization-bone-selection/`.

### Reconstrucción y dos contribuciones independientes — 22 de octubre de 2026

Se actualizó `develop` a `678071f0` y se reconstruyó `my-develop` con los cambios
publicados en los PR #128 (orco) y #129 (equipo). El PR #123 ya pertenece a la base.
Se conservaron las reglas oficiales de grupos de geosets, el contexto de sexo del
cuerpo y los cambios posteriores del visor. El nuevo esquema de caché es 23.
Los cambios funcionales anteriores quedaron guardados; las referencias históricas
a cambios sin commit no describen ya estas funcionalidades.

Ambas contribuciones compilaron por separado en Release x64. El orco pasó las
18 combinaciones de cara/postura, equipo en ambos cuerpos, traslado, rechazos de
asociación incorrecta y recurso ausente, y regresiones de Current appearance y
Night Elf femenina con tres clips. El exportador de equipo pasó el diálogo oculto,
102.105 controles del selector, exportación del conjunto Battlelord y arco para
ambos cuerpos Night Elf (18 variantes), reimportación FBX, traslado independiente,
colisiones, nombres inválidos, piezas individuales y solo armas. La integración
final volvió a compilar y pasó exportaciones/reimportaciones de orco y equipo.
Unity 6000.3.25f1 también compiló sobre la nueva base. No hubo validación visual
nueva ni cambios en WowBattlegrounds.

Instalación diaria actualizada con 187 archivos reemplazados y 283 hashes SHA-256
verificados. Configuración y cachés conservadas; WMV no se abrió ni enfocó.
Respaldo de instalación: `C:/Users/mauri/WMVDev/rebuild-my-develop-20261022/previous-install/`.
Manifiesto: `C:/Users/mauri/WMVDev/rebuild-my-develop-20261022/install-manifest.csv`.
Los dos objetos temporales se conservaron fuera del checkout en `local-only/`;
las tareas locales permanecen sin versionar y no forman parte de los PR.

### Corrección de animaciones Upright y sets del orco — 22 de octubre de 2026

Corrección directa en `my-develop`, sin Spec, commit, push ni actualización de los
PR existentes. El cargador C++ y el parser del visor Unity integrado descartaban
SKS1/SKB1/AFID del esqueleto Upright 1969462 al encontrar su padre 1964411. Ahora
las secuencias propias tienen prioridad por AnimationID/SubAnimationID, con sus
alias, tiempos, interpolación y relojes globales; las restantes se heredan. Cada
cuerpo conserva sus pivotes y su esqueleto. Los esqueletos parciales con otro
espacio de índices conservan el tratamiento anterior. La opción condicional de
Appearance sigue sin cambiar el modelo en la UI; las pruebas cargaron los modelos
reales 917116 y 1968587. No confundir ese toggle con una vista previa de Upright.

El exportador `wmv-fbx-posture-2` conserva personalización v2 y sus rutas. El
equipo añade `bodyVariants` a la entrada Orc male de los índices v1, con geometría,
esqueleto y reglas combinadas resueltos por postura. El consumidor debe elegir
modelo tanto en piezas como en reglas; no se elimina la validación. No se modificó
WowCharacterViewer. Los cambios nuevos todavía no están en las ramas de PR #128/#129.

Retail `wow` 12.1.5.70391: 151 clips por cuerpo, 3.133.460 muestras FBX comparadas
con lector independiente sin discrepancias; Upright usa 73 clips propios y 78
heredados dentro del rango seleccionado. Pasaron las 18 asociaciones cara/postura,
199.320 comparaciones de pistas del parser Unity y la regresión Night Elf femenina.
Oathclaw Wargarb y Battlelords Plate: 30 variantes de piezas, ambas reglas por set,
18 evaluaciones de geometría Stand/Walk/Run y 11.928 vínculos/adjuntos con diferencia
de bind 0. Se revisaron renders nativos FBX y capturas de seis recorridos ocultos
de WMV, todos aprobados. No hubo captura visual del cliente Blizzard ni ejecución
de WowCharacterViewer. Las láminas FBX usan materiales neutros; no prueban un nuevo
consumidor completo de texturas o correcciones faciales.

Compilaciones Release x64 y Unity 6000.3.25f1 aprobadas. Instalación diaria
actualizada con 9 archivos reemplazados y 176 hashes verificados, sin abrirla en
el escritorio. Respaldo y manifiesto: `C:/Users/mauri/WMVDev/upright-20261022/install/`.
Paquetes compartidos actualizados exclusivamente en `WowBattlegrounds/LocalAssets`:
102 archivos cambiados, 2.081 `.meta` preservados y comprobados contra el respaldo,
3.675 archivos existentes restantes sin cambios. Se conservaron las otras razas,
preferencias y apariencias predeterminadas. Los originales están archivados en
`LocalAssets/Prepared/WoWModelViewer/upright-20261022/original-packages.zip`, sin
duplicar GUIDs como archivos importables. Recursos de prueba y evidencia visual
permanecen en ese mismo directorio local compartido; logs y métricas en
`C:/Users/mauri/WMVDev/upright-20261022/`. Contrato y alcance de validación en
`docs/orc-upright-export.md`. No añadir estos assets a Git.


### Actualización de los PR de posturas y equipo — 23 de octubre de 2026

Mauricio confirmó que la corrección instalada funciona y autorizó actualizar ambos PR.
El #128 (`codex/fbx-orc-postures`) quedó publicado en `8ae5f9b7`, con el arreglo
funcional `8e7d0423`: conserva las animaciones nativas de Upright en C++ y Unity.
El #129 (`codex/standalone-equipment-export`) quedó publicado en `3ab50d28`, con el
arreglo de equipo `1b185d0c`. Contiene la rama publicada del #128 como antecesora;
ya no es independiente. Integrar primero #128 y después #129. Ambos siguen abiertos,
con títulos, descripciones, contrato del consumidor y pruebas actualizados.

Ambas ramas aisladas compilaron Release x64. El #128 repitió la exportación completa:
151 clips por postura, 3.133.460 muestras sin discrepancias y 18 combinaciones faciales
correctas; también pasaron las regresiones sintéticas y 199.320 comparaciones del
lector integrado. El #129 reexportó y reimportó ambos sets (14 y 16 variantes), y
pasaron de nuevo 18 casos de poses, 11.928 uniones y 89.412 puntos evaluados, con
error máximo de matrices 0. Las capturas visuales previas y la confirmación de Mauricio
se distinguen de estas nuevas comprobaciones automáticas. No se afirma captura del
cliente Blizzard ni prueba dentro de WowCharacterViewer.

Evidencia nueva: `C:/Users/mauri/WMVDev/upright-pr-update-20261023/`.
Los recursos de esta comprobación permanecen en `LocalAssets/Prepared/WoWModelViewer/
upright-20261022/pr128` y `pr129`. No se publicaron recursos de WoW, archivos `.meta`,
tareas ni ajustes locales de compilación. Los dos worktrees de PR quedaron limpios
y sincronizados con el fork. Las copias pendientes de `my-develop` se conservan;
esta actualización de PR no es un commit ni una integración nueva en esa rama.

### Efectos M2 sin malla — 23 de octubre de 2026

Corregido en `my-develop`, sin commit ni push. El visor Unity rechazaba
`spells/cfx_mage_fireball_missile.m2` (1327009) por no tener vértices, aunque
contiene cuatro emisores de partículas. Ahora admite efectos con emisores y skin
vacío, conserva esqueleto/reloj y calcula el encuadre desde una simulación breve
que reinicia antes de reproducir. Los modelos realmente vacíos siguen rechazados.

Compilación Unity correcta; 1.078 comprobaciones internas aprobadas, incluidas
28 nuevas para partículas/cintas sin superficie. Las pruebas IPC de Fireball,
Precast Hands y Arcane Missiles pasaron, con pausa, selección de animación,
cambio a una criatura y recarga. Capturas confirmaron que Fireball se dibuja;
no se comparó su apariencia con el cliente Blizzard. La primera ejecución conjunta
superó el tiempo de inicio del arnés al ejecutar todas las pruebas internas;
estas terminaron sin fallos y la ejecución IPC separada de Fireball pasó.
El alcance es el visor: no añade exportación FBX de partículas.

Instalación diaria actualizada: cuatro archivos reemplazados y 164 hashes SHA-256
verificados. Se cerró normalmente WMV y no se volvió a abrir. Respaldo, manifiesto
y logs: `C:/Users/mauri/WMVDev/fireball-effects-20261023/`. Recursos y capturas:
`C:/Users/mauri/workspace/WowBattlegrounds/LocalAssets/Prepared/WoWModelViewer/fireball-effects-20261023/`.
Se conservaron los demás cambios pendientes del checkout.

### PR de efectos sin malla — 23 de octubre de 2026

La corrección de Fireball quedó publicada de forma independiente en
`codex/emitter-only-effects`, commit `a2b60303`, PR
https://github.com/wowmodelviewer/wowmodelviewer/pull/130 contra `develop` oficial
(`678071f0`). Incluye únicamente el visor, sus regresiones y documentación; no
incluye Flight, exportación FBX, assets del juego ni cambios de orcos/equipo.

El player Unity de esa rama aislada compiló con 6000.3.25f1 y pasó las 1.078
comprobaciones internas. Las pruebas IPC ocultas con ese player y el host local
existente pasaron para Fireball, Precast Hands y Arcane Missiles. Se inspeccionó
una captura nueva de Fireball; no se afirma fidelidad visual al cliente Blizzard.
Evidencia: `C:/Users/mauri/WMVDev/fireball-effects-20261023/pr-validation/`.
El worktree `C:/Users/mauri/.codex/worktrees/emitter-only-effects/WowModelViewer`
quedó limpio y sincronizado con el fork. La instalación diaria conserva la versión
integrada ya verificada; las copias de trabajo en `my-develop` siguen pendientes.
