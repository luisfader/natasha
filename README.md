# Nátasha — La hora salvaje

Asistente para charlar con Nátasha (Наташа Петровић), la protagonista de la saga *La hora salvaje* de Robert (Luis) Fader. Habla en español de cualquier tema y contesta con honestidad desde su visión del mundo y sus convicciones.

## Cómo funciona

- `index.html`: el sitio completo, sin compilación. Contiene la personalidad de Nátasha (constante `PERSONA`) y la interfaz del chat.
- `corpus.json`: todas las novelas de la saga en todas sus versiones publicadas (ruso, serbio, italiano, esperanto y español), divididas en pasajes. Nátasha busca en ellos (búsqueda BM25 en el navegador) antes de hablar de su vida.
- `img/`: imágenes de Nátasha tomadas de Litmir.

Dentro de Claude (como artefacto), la página responde con Claude. Fuera de Claude (por ejemplo en GitHub Pages) pide una clave de la API de Anthropic, que se guarda solo en el navegador del visitante.

## Publicar en GitHub Pages

Settings → Pages → Deploy from a branch → `main` / raíz.

## Novelas incluidas

Дикий час · Дивљи час · Дикий час 2: Духи гор · Дивљи час: Љубав укуса малбека · Эпический матч · Дикий час: Карнавал раздора · Дивљи час 7: Плач шума · Дивљи час: Против империје друштвених мрежа · Дивљи час 6 · La hora salvaje 52 · La Sovaĝa Horo: La Savo de Zmaj · Дикий час: Венецианское колдовство · L’ora selvaggia: La stregoneria veneziana · La hora salvaje: El tango más triste

## Memoria de charlas

Nátasha guarda todas las conversaciones completas y las recuerda en las charlas siguientes, sin importar quién le escriba (las busca igual que las novelas y además recibe las más recientes).

- **Dentro de Claude:** se guardan en la base de datos del artefacto.
- **En GitHub Pages:** se guardan en Firebase (Firestore). Para activarlo:
  1. En la consola de Firebase, crear un proyecto (o usar uno existente) y activar **Firestore Database**.
  2. En *Firestore → Reglas*, por ahora (sin privacidad):
     ```
     rules_version = '2';
     service cloud.firestore {
       match /databases/{database}/documents {
         match /conversaciones/{id} { allow read, write: if true; }
       }
     }
     ```
  3. En *Configuración del proyecto → Tus apps*, registrar una app web y copiar `apiKey` y `projectId` en la constante `FIREBASE` de `index.html`.

Cada conversación es un documento de la colección `conversaciones` con los campos `inicio`, `actualizado` y `mensajes` (JSON).
