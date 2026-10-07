# PokeMMO PS5 — 1.0

Proyecto educativo no oficial de compatibilidad para ejecutar el cliente original de Windows de PokeMMO en PS5 mediante una adaptación de prospero-win/Wine y OpenGL para PS5.

El trabajo se realiza sobre el lanzador y la capa de compatibilidad. El ejecutable original de PokeMMO no se modifica. Este proyecto no es una versión oficial de PokeMMO ni está afiliado con sus desarrolladores, Sony o Nintendo.

## Estado de la versión 1.0

Probada por el usuario en una PS5 con firmware **12.40**, en un entorno con **etaHEN, kstuff y ShadowMount Plus**. Esta es la combinación utilizada en la prueba; no constituye una matriz de compatibilidad para otros firmwares o versiones de esas herramientas.

- La interfaz de PokeMMO se muestra correctamente tras corregir la presentación gráfica.
- Las ROMs se detectan al colocarlas manualmente en la carpeta del cliente.
- El usuario ha configurado los controles para utilizar el cliente con el mando.
- El rendimiento comunicado por el usuario es de aproximadamente **50–60 FPS**, con pequeños bajones. No es un benchmark ni una garantía de rendimiento.
- No se garantiza el funcionamiento de todas las regiones, funciones, actualizaciones o sesiones prolongadas.

La 1.0 conserva el menú de selección de Wine, el nombre actual del título y el icono anterior. El inicio directo, el nuevo nombre y el nuevo icono pertenecen a una futura **1.1**, todavía no implementada.

## Requisitos

- PS5 con un entorno capaz de ejecutar y montar el paquete homebrew `.ffpfsc`; la configuración probada es la indicada arriba.
- ShadowMount Plus configurado para detectar la carpeta donde se colocará el paquete.
- Espacio disponible para el paquete y los datos persistentes del cliente.
- Mando; teclado USB opcional para introducir texto cuando sea necesario.
- Conexión a internet y cuenta de PokeMMO para las funciones que requieren sus servidores.
- Copias de ROMs obtenidas legalmente por el usuario. **No se incluyen ni se facilitan ROMs.**

PokeMMO requiere una ROM compatible de **Black/White 1**. Para acceder al contenido de Kanto también se necesita **FireRed**. Consulte los requisitos actuales en la [web oficial](https://pokemmo.com/en/downloads/).

## Instalación

1. Obtenga el paquete de la versión 1.0, identificado como `PokeMMO_PS5_1.0.ffpfsc`. El ZIP de fuentes no es el paquete que se monta en la consola. Los archivos se publicarán en Releases cuando estén preparados y se haya revisado su contenido.
2. Cierre por completo el título si ya está abierto.
3. Copie el `.ffpfsc` a la carpeta que escanea ShadowMount Plus. En la instalación utilizada se emplea `/data/homebrew/`.
4. Evite conservar varios paquetes del mismo título **PPSA99995** en las carpetas escaneadas.
5. Vuelva a montar o actualizar la detección del título en ShadowMount Plus y abra la aplicación desde el menú de PS5.
6. En el menú de Wine seleccione **PokeMMO** y pulse **X** para iniciarlo. En la primera ejecución se preparan los datos persistentes.
7. Cierre el título antes de copiar las ROMs. Colóquelas en esta carpeta de la consola:

   ```text
   /data/pokemmo-ps5/prefixes/pokemmo/drive_c/Games/PokeMMO/roms/
   ```

8. Abra de nuevo la aplicación y seleccione PokeMMO. La colocación manual de las ROMs es el procedimiento que funcionó en esta instalación.
9. Configure los controles dentro del cliente según sus preferencias.

**No borre `/data/pokemmo-ps5/` al reemplazar el paquete.** Allí se conservan el cliente instalado, las ROMs, los perfiles y la configuración.

## Controles iniciales del perfil

Estos son los controles del perfil incluido; los cambios personales realizados dentro del juego pueden alterar su uso.

| Entrada | Acción inicial |
| --- | --- |
| Cruceta | Flechas del teclado |
| X | Tecla Z |
| Círculo | Tecla X |
| Cuadrado | Enter |
| Triángulo / Options | Escape |
| Stick derecho | Mover cursor |
| R2 | Clic izquierdo |
| L2 | Clic derecho |
| Mantener Options + Create | Solicitar el cierre del cliente y volver al lanzador |

## Problemas conocidos

### Select File

En la prueba realizada, pulsar **Select File** para elegir una ROM congeló el cliente. Utilice la carpeta `roms` descrita en la instalación. El selector no se considera resuelto en la 1.0.

### Teclado y mouse USB simultáneos

El usuario observó que el teclado empezó a escribir al desconectar el mouse USB. La causa del conflicto no está confirmada. Si ocurre, desconecte el mouse y utilice el stick derecho y R2 para manejar el cursor; conecte el teclado antes de iniciar el título.

### Actualizaciones de PokeMMO

Las correcciones de compatibilidad están en el lanzador y Wine, no en el ejecutable original del juego. Una actualización del cliente no las sobrescribe directamente, pero el actualizador o una nueva versión podrían requerir funciones todavía no soportadas.

Antes de actualizar, guarde una copia completa de `/data/pokemmo-ps5/` y conserve el paquete que funcionó. Recuperar una instalación anterior no garantiza poder conectarse: el servicio puede exigir una versión reciente.

## Configuración técnica y trabajo realizado

- Adaptación del arranque, preparación inicial del cliente y directorios persistentes.
- Ajustes de memoria para superar el fallo de arranque: `-Xmx1024m` y `-XX:ReservedAddressSpaceSize=2048m`.
- Correcciones de transferencia del control de la salida de vídeo entre el lanzador y la ruta OpenGL.
- Correcciones del compilador gráfico para la correspondencia de variables entre shaders.
- Presentación de las texturas de ventana y cursor mediante un shader con coordenadas UV genéricas, revisión interna **GL4 / generic-uv-4**.
- Ruta alternativa de presentación y comprobaciones gráficas limitadas para diagnosticar fallos de salida.
- Integración de controles del mando y entrada USB, con las limitaciones indicadas.

La secuencia observada durante el desarrollo pasó por retorno al lanzador, parpadeos de colores, pantalla blanca y cursor cuadrado, hasta obtener una interfaz visible. La versión 1.0 corresponde a la revisión gráfica que funcionó; las anteriores son intentos de desarrollo.

El máximo de 1 GB corresponde al **heap de Java**, no a toda la RAM consumida por Wine y el cliente. La reserva de 2 GB es espacio de direcciones, no una medición de RAM física. La salida configurada es 1920 × 1080. No se ha medido el presupuesto total de memoria del título ni el consumo durante una sesión completa.

## Registros y reporte de errores

- Wine: `/data/pokemmo-ps5/logs/`
- Cliente: `/data/pokemmo-ps5/prefixes/pokemmo/drive_c/Games/PokeMMO/log/`

Indique versión, firmware, pasos y resultado. Los números de los archivos de sesión no indican necesariamente el orden cronológico porque los registros rotan. Revise y elimine información privada antes de publicar logs; no adjunte contraseñas, credenciales ni ROMs.

## Licencias, derechos y condiciones de uso

El cliente de PokeMMO es propietario. Usarlo sin modificar **no convierte al cliente en software libre ni garantiza que cualquier forma de distribución o uso esté autorizada**. Su distribución y uso están sujetos a los [términos de PokeMMO](https://pokemmo.com/en/tos/) y a su [código de conducta](https://pokemmo.com/en/code_of_conduct/).

Wine, prospero-win y las bibliotecas y herramientas utilizadas conservan sus licencias respectivas. Al publicar fuentes y binarios deberán acompañarse los avisos, atribuciones y fuentes correspondientes cuando sus licencias lo requieran. Este README no sustituye esa revisión y no concede derechos sobre software de terceros.

PokeMMO, Pokémon y PlayStation/PS5 son nombres o marcas de sus respectivos titulares. La finalidad educativa del proyecto no supone autorización de dichos titulares ni una excepción general a sus condiciones.

## Aviso de responsabilidad

El proyecto se ofrece con fines educativos y de investigación de compatibilidad, sin garantías de funcionamiento, soporte oficial o seguridad de una cuenta. **No se garantiza que su uso esté permitido por PokeMMO ni que esté libre de suspensiones o baneos.**

Los responsables del proyecto no asumen responsabilidad por baneos, pérdida de datos, daños o uso indebido, en la medida permitida por la legislación aplicable. Cada usuario debe revisar las condiciones de los servicios y las licencias y contar con los derechos necesarios sobre los archivos que utilice. Este aviso no elimina las obligaciones legales de ninguna de las partes.

No se proporcionan ROMs, credenciales, herramientas de automatización de juego ni métodos para eludir controles del servicio.
