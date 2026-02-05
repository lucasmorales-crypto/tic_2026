# CICD de APPScript

## Configuración del repositorio y Codespace

- Configurar el entorno
- Instalaciones necesarias:
    - Node.js (LTS)
    - npm o yarn
    - git
    - `clasp` (instalación: `npm install -g @google/clasp`)
- Habilitar la Google Apps Script API en tu configuración de usuario de Google: https://script.google.com/home/settings

## Autenticación de clasp

- Instala y autentica `clasp`:
    - `npm install -g @google/clasp`
    - `clasp login --no-localhost` (o `clasp login` según tu entorno)
- Archivo de credenciales de usuario
    - Puedes usar un archivo `credentials.json` en la raíz del proyecto o almacenar las credenciales como secretos de GitHub (por ejemplo `CLASP_CREDENTIALS`).
    - Si usas OAuth/client secrets, coloca el JSON de credenciales en `credentials.json` (no subir al repositorio público).

## Estructura del proyecto

- Estructura sugerida:

```
.
├─ src/
│  └─ code.js
├─ .clasp.json
└─ appsscript.json
```

## Ejemplo de código para generar el documento (`src/code.js`)

```javascript
function createDocument() {
    const doc = DocumentApp.create('Documento generado desde CI');
    const body = doc.getBody();
    body.appendParagraph('Hola desde Google Apps Script y CI/CD!');
    body.appendParagraph('Fecha: ' + new Date());
    return doc.getId();
}
```

## Pipeline de CI/CD (GitHub Actions)

- Pasos para el workflow
    - `actions/checkout` — obtener el código.
    - Configurar Node.js (`actions/setup-node`).
    - Instalar dependencias y `clasp` (`npm install` / `npm install -g @google/clasp`).
    - Autenticación: cargar credenciales desde secretos de GitHub (por ejemplo `CLASP_CREDENTIALS` o `SERVICE_ACCOUNT_KEY`).
    - Ejecutar pruebas/compilación (si aplica).
    - `clasp push` para desplegar los cambios a Google Apps Script.

- Recomendación de secretos de GitHub: `CLASP_CREDENTIALS`, `SERVICE_ACCOUNT_KEY`, `GCP_PROJECT_ID`.