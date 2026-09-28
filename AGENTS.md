# WoW Model Viewer — contexto del fork e instrucciones de trabajo

## Objetivo

Este proyecto es el fork personal de Mauricio (`mubilla`) de WoW Model Viewer.
Su objetivo es mejorar una herramienta que Mauricio utiliza activamente para explorar modelos
de World of Warcraft y extraer modelos, texturas y animaciones para realizar pruebas en Unity
y trabajar en su proyecto **Song of War**.

Mauricio quiere contribuir al proyecto oficial mediante pull requests con arreglos y mejoras
acotados, revisados y probados. También quiere mantener una versión propia utilizable con todas
sus mejoras, independientemente de que el proyecto oficial las acepte o tarde en integrarlas.

Proyecto relacionado: `C:\Users\mauri\workspace\Song of War`.
Exportaciones de personajes: `C:\Users\mauri\workspace\Song of War\wow_models\characters`.
No modificar ese proyecto ni sus assets salvo que la tarea lo requiera.

## Repositorios y remotos

| Remoto local | Repositorio | Función |
| --- | --- | --- |
| `origin` | https://github.com/wowmodelviewer/wowmodelviewer | Proyecto oficial; fuente de cambios y destino de las contribuciones por PR. |
| `fork` | https://github.com/mubilla/wowmodelviewer | Fork de Mauricio; destino habitual de los pushes de nuestro trabajo. |

**En este checkout `origin` es el proyecto oficial, no el fork.** Comprobar los remotos antes
de publicar. No enviar nuestros commits directamente a las ramas del repositorio oficial.

## Ramas y su significado

Inventario local comprobado el **28 de septiembre de 2026**. Revisar Git antes de actuar;
esta tabla describe la organización acordada, no garantiza que las referencias sigan iguales.

| Rama | Propósito y seguimiento |
| --- | --- |
| `develop` | Espejo de `origin/develop`. Mantenerla como referencia del código oficial, sin mejoras propias. También está publicada como `fork/develop`. |
| `my-develop` | Rama de integración y uso diario de Mauricio. Reúne las mejoras propias; sigue `fork/my-develop`. |
| `fix/equipment-remove-buttons` | Corrección de la retirada individual de equipo. Publicada en el fork e integrada en `my-develop`. |
| `codex/animation-list-improvements` | Ordenación del listado de animaciones por nombre, ID y duración. Publicada en el fork e integrada en `my-develop`. |
| `codex/fbx-animation-id-range` | Selección y búsqueda de animaciones para exportar a FBX mediante IDs de WoW. Publicada en el fork e integrada en `my-develop`. |

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

La creación de estos PR no significa que estén aceptados; consultar su estado cuando sea relevante.

### Cambios guardados en `my-develop` el 28 de septiembre

No confundir trabajo implementado e instalado con trabajo publicado en un PR. Estos cambios
ya tienen commits propios en `my-develop`; la separación de los dos arreglos de NPC en ramas
de contribución está en curso:

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
  del visor Unity. Ambos se publicarán de forma acumulativa: `codex/npc-equipment-1`
  desde `origin/develop` y `codex/npc-equipment-2` desde la primera, aplicando únicamente
  sus commits y las dependencias mínimas necesarias; los dos PR apuntarán a `develop`.
  Compilado e instalado el 28 de septiembre: seis casos de exportación y cuatro casos
  de rechazo aprobados. Tyrande exporta cuerpo, arco y 156 clips; se verificaron hueso,
  textura y movimiento numéricamente, sin revisión visual del FBX en Unity. Los FBX
  originales de Song of War no se sobrescribieron.

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
  de trabajo para Song of War, no una enumeración universal de todas las animaciones necesarias.
- No commitear, publicar PR ni integrar ramas solo por haber terminado una implementación;
  seguir la autorización de la tarea y las instrucciones personales de Git de Mauricio.

Actualizar este documento cuando cambien acuerdos, ramas o hitos importantes, evitando que
los apartados históricos se interpreten como el estado actual del árbol de trabajo.
