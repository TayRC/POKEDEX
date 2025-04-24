
# Caso PokeDex: Despliegue y Configuración

## Paso 1: Realizar un Fork del Repositorio en GitHub

1. Accede al repositorio original en GitHub:  
   👉 [https://github.com/rcuello/ac4dem1a/tree/master/sistemas-distribuidos/poke-dex-lab](https://github.com/rcuello/ac4dem1a/tree/master/sistemas-distribuidos/poke-dex-lab)

2. Haz clic en el botón **“Fork”**, ubicado en la parte superior derecha.

3. Selecciona tu cuenta personal como destino y cambia el nombre del repositorio de `ac4dem1a` a `PokeDex`.

4. Haz clic en **“Create fork”** y espera a que GitHub complete el proceso.

---

## Paso 2: Crear una Static Web App en Azure

1. Ingresa al [Portal de Azure](https://portal.azure.com) y busca el servicio **“Web App”**.

2. Configura la aplicación con los siguientes datos:
   - **Nombre:** `swa-pokedex-portal-prod-yev`
   - **Grupo de recursos:** `(Nuevo) rg-pokedex-prod`
   - **Tipo de plan:** Gratuito
   - **Origen del código:** Repositorio fork en GitHub
   - **Organización:** `yamil2023`
   - **Repositorio:** `PokeDex`
   - **Rama:** `master`

3. Copia la ruta relativa desde GitHub:  
   `sistemas-distribuidos/poke-dex-lab/source/pokedex-angular`

4. En Azure, reemplaza el campo **Ubicación de la aplicación** con la ruta mencionada, respetando el `./`.

5. Haz clic en **“Revisar y crear”** y espera la validación y creación del recurso.

6. Luego del despliegue, haz clic en **“Ir al recurso”** y después en **“Visitar sitio”**.

---

## Paso 3: Solución al Error de Imágenes No Mostradas

**Problema:** Las imágenes no cargaban debido a rutas incorrectas.

### Solución:

1. Verifica errores 404 desde la consola del navegador (F12 → “Console”).

2. En GitHub, accede a:  
   `sistemas-distribuidos/poke-dex-lab/source/pokedex-angular`

3. Crea un archivo nuevo:  
   `staticwebapp.config.json`

4. Agrega el siguiente contenido:

```json
{
  "globalHeaders": {
    "Strict-Transport-Security": "max-age=31536000; includeSubDomains; preload",
    "X-Frame-Options": "DENY",
    "X-Content-Type-Options": "nosniff",
    "Referrer-Policy": "no-referrer",
    "Permissions-Policy": "geolocation=()",
    "Content-Security-Policy": "default-src 'self'; img-src 'self' https://raw.githubusercontent.com https://pokeapi.co https://assets.pokemon.com; script-src 'self' 'unsafe-inline'; style-src 'self' 'unsafe-inline' https://fonts.googleapis.com; font-src 'self' https://fonts.gstatic.com; connect-src 'self' https://beta.pokeapi.co;"
  },
  "navigationFallback": {
    "rewrite": "/index.html",
    "exclude": ["/images/*", "/css/*", "/js/*", "/favicon.ico"]
  }
}
```

5. Haz clic en **“Commit changes”**.

6. Accede a:  
   `sistemas-distribuidos/poke-dex-lab/source/pokedex-angular/src/environments/environment.prod.ts`

7. Modifica la línea:

```ts
imagesPath: 'pokedex-angular/assets/images',
```

a:

```ts
imagesPath: 'assets/images',
```

8. Espera a que se completen los despliegues.

9. Vuelve a Azure y abre la aplicación para verificar que las imágenes se muestren correctamente.

---

## Paso 4: Pruebas de Seguridad

1. Accede a [https://securityheaders.com](https://securityheaders.com)

2. Ingresa la URL del sitio y realiza el análisis.

3. El sitio debe obtener una calificación **“A”** como verificación de su configuración segura.
