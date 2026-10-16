# Formula AI · AI MCLAC Championship — Votación anónima (mobile first)

Este MVP **no solicita login**, no envía iNumber, email o nombre, y no almacena votos individuales. En Firestore existe únicamente la colección `teamCounts`, con seis documentos por ID de equipo, cada uno con el campo numérico `count`.

## Configuración
1. Crear un proyecto en https://console.firebase.google.com/ (sujeto a la política interna SAP).
2. Activar **Firestore Database**.
3. Copiar el contenido de `firestore.rules` en Firestore Database > Rules y publicarlo.
4. En Project settings > Your apps, agregar una aplicación Web y copiar `apiKey`, `authDomain`, `projectId`, `appId` al objeto `firebaseConfig` en `index.html`.
5. Los seis equipos ya están configurados en el array `teams`.
6. Publicar `index.html` en Vercel, Netlify o Firebase Hosting vía HTTPS, si SAP autoriza su uso.
7. Abrir el enlace desde un celular y probar un voto; verificar en Firestore el contador.

## Funcionamiento y limitaciones
- Cada voto incrementa transaccionalmente el contador del equipo elegido.
- Al completarse se almacena la marca `votacion-sap-equipos-2026-final` en `localStorage` **solo en el navegador**.
- Un usuario puede votar otra vez desde otro dispositivo, otro navegador, incógnito o eliminando el almacenamiento local. El servidor **no puede comprobar unicidad** sin algún identificador, por lo que las reglas solo restringen escrituras a incrementos de una unidad; una persona técnica también podría simular más solicitudes. Es adecuado solo para votaciones informales y de bajo riesgo.
- Firebase/hosting pueden conservar registros técnicos (por ejemplo, IP), aunque este código no almacena datos personales en Firestore. Validar tratamiento y aprobación de proveedores con las normas de SAP.
- El sistema permite crear el primer contador en 1; no hace falta crear documentos a mano.
- El botón de votos se bloquea mientras la transacción está pendiente. Ante una interrupción de red justo después del commit, podría existir un voto confirmado sin marca local.
- Para reiniciar una votación, usar una nueva clave STORAGE_KEY, borrar/reiniciar `teamCounts` mediante la consola administrativa, y volver a desplegar. Nunca habilitar reglas generales de escritura.

## Equipos configurados
GCF, Demand Generation, CVG Ventas, O2I, Legales y Marketing. Los resultados se muestran después de votar, no antes.

**Importante:** esta versión usa IDs nuevos y una clave nueva de bloqueo local; si ya se utilizó una versión anterior con votos reales, hay que migrar los conteos antes de reemplazarla.

## Identidad visual
La interfaz está inspirada en los textos y la temática de competición del HTML del evento Formula AI proporcionado, no es una reproducción exacta de su CSS: el archivo original referenciaba hojas de estilo e imágenes externas que no estaban adjuntas. Se usa una marca tipográfica SAP estilizada, no el logo oficial.
