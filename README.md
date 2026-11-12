# Plataforma de Auditorías HPM

Lobby de auditorías de Hyundai Polytech México: **IATF 16949:2016**, **Hyundai SQ-Mark (inyección)** y **VDA 6.3**, en un solo archivo (`index.html`) publicado con GitHub Pages.

Sitio: https://calidadbackupsistema-collab.github.io/Auditorias-Automotrices/

## Archivos

| Archivo | Uso |
|---|---|
| `index.html` | Plataforma completa (Lobby + 3 módulos). Incluye embebida la herramienta IATF. |
| `firebase-config.js` | Configuración del proyecto Firebase `sistema-de-auditorias`. Si no existe, la plataforma trabaja en modo local. |
| `HPM_Auditoria_IATF16949_v4.html` | Versión independiente de la auditoría IATF (se conserva como referencia). |

## Sincronización en la nube (Firebase)

Todos los equipos que escriban el **mismo código de sala** (por ejemplo `HPM-2026`) ven y editan las mismas tres auditorías en tiempo real. Los datos se guardan en Firestore en `salas/{CÓDIGO}` y además en el navegador de cada equipo.

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

1. Abrir el sitio, escribir el código de sala y **Conectar**.
2. Elegir la auditoría en el carrusel y **Abrir**.
3. Al cerrar el día, **Exportar respaldo** (JSON) desde la tarjeta Respaldo.


## SQ-Mark (Ver 5.1 oficial)

47 ítems · 1,000 pts · 6 niveles de calificación · grado S/A/B/C/D. Cada ítem admite fotos de evidencia (cámara o galería). **Imprimir informe** genera: informe principal, hoja de evaluación, hoja de hallazgos y anexo fotográfico opcional (con el formato del Excel oficial).
