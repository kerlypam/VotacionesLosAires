# Votaciones Los Aires de Magdalena, gestión 2026-2028

Página de votación para elegir la Junta Directiva de Propietarios del Edificio Multifamiliar Los Aires de Magdalena (Jr. Tacna 1492-1494, Magdalena del Mar). Es un único archivo, `index.html`, sin servidor ni instalación.

## Qué hace

- **Registro del votante:** elige su departamento e ingresa su DNI; su nombre se completa desde el padrón y puede corregirlo. El DNI se valida contra el padrón cargado por el comité.
- **Cédula:** una opción por cargo (Presidente, Tesorero y Administrador General, según el Reglamento Interno) o voto en blanco.
- **Un solo voto por departamento (regla fija).** Si un departamento tiene dos o más DNI registrados, vota el primero que ingrese. Desde ese momento el departamento aparece como "ya votó" y queda bloqueado para los demás DNI.
- **Registro del voto:** guarda departamento, DNI, nombre, la elección de cada cargo, fecha, hora y un código de verificación (por ejemplo `LAM-7K3P9Q`).
- **Constancia descargable (PNG):** "El departamento 1404 emitió su voto el domingo 25 de octubre de 2026 a las 6:01 p. m.", con código de verificación. No muestra la elección del votante.
- **Panel del comité con clave:** resultados en vivo, registro de votantes, padrón, candidatos, horario y ajustes.
- **Horario programado:** fecha y hora de inicio y de cierre (hora de Lima). Fuera de ese rango la cédula no acepta votos.
- **Carga de padrón desde Excel (.xlsx, .xls o .csv):** usa las columnas "Número de documento de identidad", "Apellidos y nombres completos" y la de departamento; ignora el resto.
- **Contacto del Comité Electoral** por WhatsApp visible para los votantes.
- **Exportaciones CSV:** resultados, registro de votos y padrón. Respaldo completo en JSON.

## Publicar en GitHub Pages

1. Cree un repositorio nuevo en GitHub (por ejemplo `votaciones-los-aires-de-magdalena`).
2. Suba `index.html`, `README.md`, `plantilla-padron.xlsx`, `.nojekyll` y `.gitignore`.
3. En el repositorio, vaya a **Settings > Pages**.
4. En **Source**, elija **Deploy from a branch**, rama `main`, carpeta `/ (root)`, y pulse **Save**.
5. En uno o dos minutos la página quedará en `https://<su-usuario>.github.io/votaciones-los-aires-de-magdalena/`.

> **No suba el padrón real ni los CSV exportados al repositorio.** Contienen DNI y están protegidos por la Ley N.° 29733. El padrón se carga desde el panel del comité, en el navegador, y nunca se envía a GitHub. El archivo `.gitignore` ya bloquea esos nombres de archivo.

## Modo de funcionamiento: mesa de votación

Publicada en GitHub Pages, la página guarda los datos **en el navegador del equipo donde se abre** (almacenamiento local). Esto significa:

- Funciona como **mesa de votación**: una laptop o tablet en el SUM donde los propietarios votan por turno.
- Los votos **no se comparten entre equipos.** Si dos laptops abren la página, cada una lleva su propio conteo. Use un solo equipo por elección.
- Borrar el historial o los datos del navegador borra los votos. Al cerrar la votación, descargue el **respaldo** y el **registro de votos (CSV)** desde el panel.
- No use una ventana de incógnito: al cerrarla se pierden los datos.

Para que cada propietario vote desde su propio celular hace falta una base de datos en línea (por ejemplo Firebase). Esa versión requiere configurar reglas de seguridad para proteger los DNI y no está incluida en este paquete.

## Uso paso a paso

### 1. Crear la clave del comité
Pulse **Acceso del comité** al pie de la página. La primera vez se crea la clave (mínimo 6 caracteres). Compártala solo con los miembros del comité. El panel se bloquea solo tras 10 minutos sin uso.

### 2. Cargar el padrón
En **Padrón > Elegir archivo Excel o CSV**. Puede subir directamente el Excel de respuestas del formulario de actualización de datos, aunque tenga muchas columnas. La página usa solo tres:

| Columna del Excel | Para qué se usa |
|---|---|
| **Número de documento de identidad** | DNI o carné de extranjería que valida al votante |
| **Apellidos y nombres completos** | Se precarga en el registro cuando el votante escribe su DNI; puede corregirlo |
| **Departamento** (o "Número de departamento", "Dpto.") | Departamento al que pertenece ese documento |
| **Habilitado** (obligatoria) | Solo vota quien tenga **SI**. Quien tenga **NO** (o la celda vacía) aparece como **INHABILITADO** y no puede votar |

- Las demás columnas (marca temporal, correo, celular, tipo de documento, estado civil, postulación) se ignoran.
- Los DNI no habilitados no se eliminan: quedan en el padrón con estado INHABILITADO. Si alguien de ese departamento intenta votar, la página lo indica y bloquea el registro. Un departamento sin ningún DNI habilitado aparece en la lista como "(inhabilitado)".
- Si el Excel no tiene la columna "Habilitado", la importación se detiene y pide agregarla.
- En **Padrón**, cada DNI tiene un enlace **Habilitar / Inhabilitar** para corregir el estado en la mesa, por ejemplo si un propietario se pone al día con sus cuotas.
- La participación se calcula sobre los departamentos habilitados.
- Si no hay columna de nombres, el votante lo escribe al registrarse.
- Los DNI que Excel guardó sin el cero inicial (7 dígitos) se completan a 8.
- Un departamento puede tener varios DNI (copropietarios), en filas separadas.
- Si una persona respondió el formulario dos veces, se conserva un solo registro.
- Antes de importar se muestra una vista previa con las columnas detectadas, las filas omitidas, los números que no coinciden con la estructura del edificio (70 departamentos) y los departamentos que faltan.

### Contacto para votantes con problemas
Debajo del registro aparece el bloque **Contacto del Comité Electoral**, con enlaces directos a WhatsApp:

| Nombre | WhatsApp |
|---|---|
| Kerly Mantilla Díaz | 933 982 010 |
| Jose Luis Bujaico Carlos | 964 479 291 |

Los mensajes de error (DNI no encontrado, departamento que ya votó) remiten a este bloque, y la constancia incluye ambos números. Para cambiarlos, edite la línea `const CONTACTOS=` en `index.html`.

### 3. Registrar candidatos
En **Candidatos**, agregue nombre y departamento para cada cargo.

### 4. Programar el horario
En **Horario y ajustes**, indique inicio y cierre y pulse **Guardar horario**. El interruptor **Votación habilitada** permite cerrar la mesa en cualquier momento. La regla de un voto por departamento no se puede desactivar.

### 5. Votar
El votante elige su departamento, ingresa DNI y nombre, marca la autorización de datos, vota y confirma. Al terminar puede descargar su constancia; la pantalla vuelve al inicio en 2 minutos o con **Siguiente votante**.

### 6. Cerrar y reportar
- **Resultados:** votos por cargo, participación por departamentos y CSV.
- **Votantes:** registro completo (fecha, hora, departamento, nombre, DNI, elección por cargo y código) en CSV, y la constancia de cualquier departamento.
- **Respaldo:** archivo JSON con todo lo anterior.

### 7. Reiniciar
**Horario y ajustes > Reiniciar** borra todos los votos (pide escribir REINICIAR). El padrón, los candidatos, el horario y la clave se conservan. Use **Padrón > Vaciar padrón** para retirar el padrón.

## Consideraciones

- **Secreto del voto:** esta versión registra la elección de cada votante junto con su DNI, a pedido del comité. Solo es visible con la clave, pero el voto no es secreto para el comité. Confirme que esto es compatible con el Reglamento Interno antes de usarla.
- **La clave protege la pantalla, no los datos:** quien tenga acceso al equipo y conocimientos técnicos podría leer el almacenamiento del navegador. Mantenga el equipo bajo custodia del comité.
- **Constancia:** acredita que el departamento votó, con fecha, hora y código; el código se puede cruzar con el registro de votantes.
