# Plataforma de Auditorías HPM

Lobby de auditorías de Hyundai Polytech México: **IATF 16949:2016**, **Hyundai SQ-Mark (inyección)** y **VDA 6.3**, en un solo archivo (`index.html`) publicado con GitHub Pages.

Sitio: https://calidadbackupsistema-collab.github.io/Auditorias-Automotrices/

## Archivos

| Archivo | Uso |
|---|---|
| `index.html` | Plataforma completa (Lobby + 3 módulos). Incluye embebida la herramienta IATF. |
| `firebase-config.js` | Configuración del proyecto Firebase `sistema-de-auditorias`. Si no existe, la plataforma trabaja en modo local. |
| `HPM_Auditoria_IATF16949_v4.html` | Versión independiente de la auditoría IATF (se conserva como referencia). |

## Acceso y salas

Al abrir la plataforma se pide acceso:

- **Administrador** (contraseña de administrador): edita todo, ve el Lobby y, **dentro de cada auditoría**, configura la **Sala** y los **accesos** (botón «Sala y accesos · Plan PDF» en los datos de la auditoría). Cada acceso tiene nombre/rol, permiso (*puede editar* o *solo lectura*) y un código propio. Desde ahí imprime el **Plan de Auditoría + contraseñas** en PDF para la empresa auditada.
- **Participante**: entra con *Sala + código personal*; solo ve la auditoría de esa sala, con el permiso asignado.

Cada auditoría tiene su propia sala en Firestore (`salas/{SALA}`, con la auditoría y un bloque `meta` con los códigos **en hash SHA-256**; los códigos en claro solo viven en el equipo del administrador y en el PDF impreso).

**Importante (seguridad):** la validación de contraseña y los permisos se aplican en la interfaz del navegador. Frena el uso normal, pero no es seguridad fuerte: quien conozca el proyecto Firebase y la sala puede leer/escribir saltándose la interfaz. Para protección real, usar Firebase Auth con usuarios y reglas de Firestore por sala/rol.

Configuración necesaria en la consola de Firebase (solo una vez):

1. **Authentication → Sign-in method → Anónimo: habilitar.**
2. **Authentication → Settings → Authorized domains:** agregar `calidadbackupsistema-collab.github.io`.
3. **Firestore Database → crear base (modo producción) → Rules:**

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // incluye la subcolección de fotos de evidencia (salas/{sala}/fotos/{id})
    match /salas/{sala}/{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

Sin estos tres pasos la barra de nube mostrará "Error de nube" y la plataforma seguirá funcionando en modo local.

## Uso

1. Abrir el sitio y entrar como Administrador (o como Participante con sala y código).
2. Administrador: elegir la auditoría, capturar datos iniciales, **Sala y accesos** → *Guardar y conectar* → *Imprimir PDF*.
3. Al cerrar el día, **Exportar respaldo** (JSON) desde la tarjeta Respaldo.


## SQ-Mark (Ver 5.1 oficial)

47 ítems · 1,000 pts · 6 niveles de calificación · grado S/A/B/C/D. Cada ítem admite fotos de evidencia (cámara o galería). **Imprimir informe** genera: informe principal, hoja de evaluación, hoja de hallazgos y anexo fotográfico opcional (con el formato del Excel oficial).
