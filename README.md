# Formula AI — Votación + Race Control (una sola página)

## Para publicar
1. En el repositorio `zeroser1/Formula-AI`, reemplazá **index.html** por el archivo de este ZIP.
2. En Firebase Console → Firestore Database → **Reglas**, pegá `firestore.rules` y hacé clic en **Publicar**. El archivo en GitHub no publica automáticamente las reglas.
3. El deploy de Vercel debería comenzar automáticamente al hacer commit en GitHub. La web sigue en `https://formula-ai-sepia.vercel.app/`.
4. Probá con **nombres ficticios** antes del evento. Los votos y nombres se guardan juntos al confirmar.
5. Acceso al panel: enlace **Race Control** al pie de la misma página, luego correo y contraseña del usuario Firebase Authentication cuyo UID es `gDXk8m5xE0Yw9K3d8eOQAPS8usx2`.
6. El sorteo exige al menos 20 inscriptos y registra de forma persistente los 20 ganadores antes de mostrar la animación.
7. Después del evento, desde Race Control usá **Eliminar datos personales del evento**. Verificá los documentos borrados en Firestore. Los contadores de votos no se eliminan.

## Advertencias importantes
- Los participantes deben completar nombre, equipo y consentimiento: este formulario NO es anónimo respecto del nombre. No guarda iNumber, correo, ni el equipo junto al nombre.
- La prohibición de nombres duplicados se aplica a la versión normalizada (sin tildes, sin diferencias de mayúsculas ni espacios extra) mediante un identificador SHA-256. Diferentes personas con el mismo nombre deberán usar nombres distinguibles. Esto no verifica identidad real.
- El bloqueo por `localStorage` y las reglas de escritura pública **no impiden ataques deliberados ni votos fraudulentos**. La base no es apta para una elección sensible. Firestore Rules no puede verificar hashes arbitrarios del lado del servidor. Una persona con conocimientos técnicos puede escribir identificadores arbitrarios; para prevenirlo se requiere un backend validado, autenticación o tokens únicos.
- El nombre se almacena temporalmente en Firestore, y los ganadores en otro documento; los servidores pueden conservar registros técnicos o respaldos. Revisá la política de datos SAP y conservá el consentimiento.
- Si tras borrar los datos querés reiniciar el evento con los mismos navegadores, tendrás que cambiar `STORAGE_KEY` y decidir también cómo reiniciar los contadores.
- La clave pública de Firebase incluida aquí no es una credencial de administración. **Nunca** pongas contraseñas o claves privadas dentro del HTML.
- Si Firebase rechaza la transacción en la creación de un contador inexistente, inicializá los documentos `teamCounts` con `count:0` desde la consola (sólo administradores) antes de abrir al público.
