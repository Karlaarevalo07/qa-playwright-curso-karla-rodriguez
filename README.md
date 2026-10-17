# Aseguramiento de la Calidad del Software (Curso 048) – Paquete para ponerse al día

Proyecto Playwright + TypeScript con las Clases 4, 5, 6, 8, 9, 10 y 11 (laboratorios, tareas reto y documentos).
**Es material de estudio**: no se va a entregar ni cuenta para nota. El objetivo es entender cada técnica y llegar preparada al final.

Cada archivo sale de las diapositivas del curso (UMG, Ing. Estuardo Mayen). Los laboratorios están copiados tal cual de las diapositivas; los retos de cada tarea son una **versión de referencia** escrita para estudiar.

---

## 1. Instrucciones para el Claude que acompañe a la estudiante

Lee esto antes de responder cualquier cosa. La estudiante te pasará esta carpeta (o este README) para que la ayudes a recorrerla.

### Rol
Eres su tutor y compañero de código. Vas **una clase a la vez, y dentro de cada clase paso a paso**, no todo de golpe. Ella es quien ejecuta, mira y entiende; tú explicas y corriges. Si ella pide "dame todo completo", puedes dárselo, pero por defecto avanza por pasos.

### Reglas que siempre se cumplen
1. **Nunca ejecutes `git add`, `git commit`, `git push` ni nada que modifique el repositorio.** Esas acciones las hace ella con sus manos. Tú solo le das los comandos listos para copiar.
2. **Ningún commit ni mensaje que prepares debe llevar línea de coautor** (nada de `Co-Authored-By`, ni "Generated with Claude", ni enlaces de sesión). Solo el mensaje del commit.
3. **Usa `git add` selectivo** (archivo por archivo), nunca `git add .` ni `-A`. Así no se cuelan archivos que no corresponden.
4. **Fidelidad a las diapositivas.** Los archivos de laboratorio son literales; no los "mejores" ni los reescribas. Si ella cree que hay un error, comprueben juntas contra las diapositivas.
5. **No inventes resultados.** Si algo no se ha ejecutado, dilo. Los retos de este paquete no se corrieron contra los sitios reales al escribirlos (ver sección 5). Si un locator falla, se revisa el sitio real (`npx playwright codegen <url>`), no se adivina.
6. **Evidencias solo desde su computadora.** Las capturas las toma ella de su propia pantalla. Tú le dices cuál tomar y qué debe verse en ella. Cuando te las pase, las revisas (¿se ve lo que debe verse?) y, si ella lo pide, **las insertas tú en el README del repositorio** con enlaces relativos a `evidencias/`.
7. **Los textos personales son de ella**: reflexiones, respuestas de discusión, el SQA Plan, la tabla de decisión. Ayúdale a pensarlos y a corregir la redacción, pero que salgan con su voz y su criterio, no con lenguaje genérico.
8. **Distingue fallos de ambiente de fallos reales.** practice.expandtesting.com y DemoBlaze tienen anuncios y backend lento que causan `Timeout` sueltos. Regla de la guía: si un test falla con Timeout, repite **una vez** el mismo comando; si pasa, fue del ambiente (se anota y se sigue). Si falla otra vez con el mismo `Expected/Received`, es real: abrir el reporte y revisar la traza.
9. **Respuestas concisas y en español conversacional**, con poco formato. Una cosa a la vez.

### Cómo trabajar cada clase (el ciclo)
1. Ella te dice en qué clase va. Le explicas en pocas líneas qué hace el archivo.
2. Ella corre el comando de la sección 4 y te pega la salida de la terminal.
3. Tú la lees: ¿coincide con el resultado esperado? Si no, diagnostican (regla 8).
4. Le indicas qué capturas tomar (sección 4), ella las toma y te las pasa.
5. Si hay que documentar algo en el README del repo, lo redactas tú con las imágenes enlazadas.
6. Al cierre de la clase le das el comando de commit (reglas 1-3) para que lo corra ella.

---

## 2. Preparación (una sola vez)

Requisitos: Node.js 18 o superior y Git.

```bash
npm install
npx playwright install
```

Notas del `playwright.config.ts` (es el de las diapositivas de la Clase 10):
- Define 5 proyectos (chromium, firefox, webkit, mobile-chrome, mobile-safari), `headless: false`, `video`, `screenshot` y `trace` activados, y `workers: 1`.
- **Si corres un archivo sin `--project`, se ejecuta en los 5 proyectos.** Para estudiar, usa siempre `--project=chromium`; los proyectos extra se usan solo en la Clase 10.
- El `baseURL` es DemoBlaze porque la Clase 4 lo necesita (`page.goto('/')`). Los demás archivos usan su URL completa.
- Las carpetas `evidencias/` y `reportes/` ya existen.

---

## 3. Qué hay en el proyecto

| Carpeta / archivo | Contenido |
|---|---|
| `tests/clase04.spec.ts` | Lab Clase 4 (DemoBlaze, 4 tests) + 3 retos |
| `tests/clase05.spec.ts` | Lab Clase 5 (Sauce Demo, 10 tests) + 3 retos |
| `tests/clase06.spec.ts` | Lab Clase 6 (Page Object Model, 5 tests) + 3 retos |
| `tests/clase08.spec.ts`, `tests/tarea08.spec.ts` | Lab Clase 8 (hooks, 7 tests) y retos de la tarea |
| `tests/clase09.spec.ts`, `tests/tarea09.spec.ts` | Lab Clase 9 (fixtures y parametrizados, 9 tests) y retos |
| `tests/clase10-smoke.spec.ts`, `tests/clase10-regression.spec.ts`, `tests/tarea10.spec.ts` | Lab Clase 10 (tags y multi-browser) y retos |
| `tests/clase11.spec.ts`, `tests/tarea11.spec.ts` | Lab Clase 11 (reportes y bugs, practice.expandtesting.com) y retos |
| `pages/` | Page Objects: LoginPage, InventoryPage, CartPage, CheckoutPage, MenuPage |
| `helpers/auth.ts` | `loginAs(page, usuario)` |
| `fixtures/index.ts` | Fixtures de la Clase 9 |
| `tareas/tarea-04.md` | Reflexión de la Tarea 4 (la escribe ella) |
| `casos-de-prueba/tabla-decision-checkout.md` | Tabla de decisión de la Clase 5 (a verificar a mano) |
| `documentos/sqa-plan-saucedemo.md` | SQA Plan mínimo de la Clase 8 |
| `reportes/` | Plantillas de bug report de la Clase 11 |
| `evidencias/` | Aquí van las capturas |

---

## 4. Guía por clase: comando, resultado esperado y capturas

Todos los comandos desde la carpeta del proyecto. Orden sugerido: 4 → 5 → 6 → 8 → 9 → 10 → 11.

### Clase 4 – Principios de pruebas + Actions (DemoBlaze)
- **Comando:** `npx playwright test tests/clase04.spec.ts --project=chromium`
- **Esperado:** `7 passed` (4 del lab + 3 retos). Los tests dependen del orden (el usuario se registra en el primero): corre el archivo completo. DemoBlaze es lento; un fallo suelto se repite una vez.
- **Capturas:** (1) terminal con `7 passed`; (2) `evidencias/registro-llenado.png` y `evidencias/carrito-con-producto.png` se generan solas: verifica que existen y se ven bien.
- **Escribe tú:** `tareas/tarea-04.md` (¿qué principio ISTQB te parece más importante y por qué?).
- **Para discutir con Claude (diapositiva 21):** ¿qué principio aplica el test de credenciales incorrectas? ¿Se puede garantizar que ningún usuario entre con contraseña incorrecta? Si se corre 100 veces igual, ¿encuentra defectos nuevos?

### Clase 5 – Técnicas de diseño + Assertions (Sauce Demo)
- **Comando:** `npx playwright test tests/clase05.spec.ts --project=chromium`
- **Esperado:** `13 passed` (10 del lab + 3 retos: `toHaveValue`, `toBeFocused`, `toHaveCSS`).
- **Capturas:** terminal con `13 passed`.
- **Entregable a trabajar:** `casos-de-prueba/tabla-decision-checkout.md`. Antes de darla por buena, comprueba **a mano** en saucedemo.com las tres preguntas del lab (checkout sin sesión, carrito vacío, mensaje de error por campo).
- **Para discutir:** ¿cuántos tests cubren las clases de equivalencia del login? ¿Qué assertion usarías para un precio > $0? ¿Cuándo usar soft assertions?

### Clase 6 – Testing ágil + Page Object Model
- **Comando:** `npx playwright test tests/clase06.spec.ts --project=chromium`
- **Esperado:** `8 passed` (5 del lab + 3 retos que usan `CheckoutPage`, `MenuPage` y `removeProductByName`).
- **Capturas:** terminal con `8 passed`.
- **Para discutir:** ¿cuánto código cambia si `#login-button` pasa a ser `#btn-login`? ¿Qué ventaja da el POM en un equipo de 3?

### Clase 8 – Hooks y suites
- **Comandos:**
  - `npx playwright test tests/clase08.spec.ts --project=chromium --workers=4` (paralelo; el config trae `workers: 1`, por eso se fuerza)
  - `npx playwright test tests/clase08.spec.ts --project=chromium --workers=1` (secuencial)
  - `npx playwright test tests/tarea08.spec.ts --project=chromium`
  - `npx playwright show-report`
- **Esperado:** `7 passed` en el lab (en paralelo suele tardar bastante menos que en secuencial; la guía midió ~7-8 s vs ~15 s) y `5 passed` en la tarea (3 pasos del reto 1 + reto 2 + reto 3).
- **Capturas:** terminal de la corrida en paralelo, terminal de la secuencial (para comparar tiempos) y el reporte HTML.
- **Escribe tú:** revisa `documentos/sqa-plan-saucedemo.md` (propósito, alcance, herramientas, criterios de salida) y ajústalo con tus palabras.

### Clase 9 – Fixtures y tests parametrizados
- **Comandos:**
  - `npx playwright test tests/clase09.spec.ts --project=chromium --reporter=list`
  - `npx playwright test tests/tarea09.spec.ts --project=chromium --reporter=list`
- **Esperado:** `9 passed` en el lab (3 con fixtures, 3 de login parametrizado, 3 de productos) y `5 passed` en la tarea (teardown, worker x2, viewport x2). En el reto 1 verás impreso `[cronómetro] el test tardó ... ms`.
- **Capturas:** terminal de ambas corridas.
- **Para discutir:** ¿cuántas líneas ahorra el loop? ¿Qué pasa si agregas un cuarto usuario al array? ¿Cómo leerías los datos desde un CSV?

### Clase 10 – Multi-browser y tags
- **Requisito:** `npx playwright install` (descarga también firefox y webkit).
- **Comandos:**
  - `npx playwright test --grep "@smoke" --project=chromium`
  - `npx playwright test --grep "@regression" --project=chromium`
  - `npx playwright test tests/clase10-smoke.spec.ts tests/clase10-regression.spec.ts` (los 5 proyectos)
  - `npx playwright test tests/tarea10.spec.ts` (los 5 proyectos)
  - `npx playwright test --grep-invert "@ui" --project=chromium`
  - `npx playwright show-report`
- **Esperado:** el lab son 10 tests (5 smoke + 5 regression); en los 5 proyectos son 50. La tarea son 3 tests (15 en los 5 proyectos).
- **Capturas:** terminal de la corrida multi-browser y **el reporte HTML mostrando los proyectos** (lo pide el entregable).
- **Para discutir:** ¿algún test se comportó distinto en WebKit? ¿Cuántos tests corrieron en total con 5 proyectos? ¿Por qué el smoke va antes del regression en un pipeline?

### Clase 11 – Gestión de bugs (practice.expandtesting.com)
Esta clase tiene más pasos; ve uno por uno:
1. **Ejecutar:** `npx playwright test tests/clase11.spec.ts --project=chromium` → `9 passed`. (Un Timeout suelto: repetir una vez, es el ambiente.)
2. **Ver el defecto:** comenta la línea `test.fail(...)` del test "Imágenes rotas", corre `npx playwright test tests/clase11.spec.ts -g "rotas" --project=chromium` y abre `npx playwright show-report`. Debes ver `Expected: > 0` / `Received: 0`, la captura/video del fallo y, en la traza (pestaña Network), los 404 de `asdf.jpg` y `hjkl.jpg`. **Después, descomenta `test.fail(...)`.**
3. **Capturas:** error del reporte, Network con `asdf.jpg` 404, Network con `hjkl.jpg` 404. Guárdalas en `evidencias/`.
4. **Bug report:** completa `reportes/bug-report-clase11.md` (fecha, nombre, ambiente, evidencia con tus capturas).
5. **GitHub Issue:** en **tu** repositorio, pestaña Issues → New issue, título del reporte, descripción = contenido del `.md`, etiqueta `bug`. Copia la URL del Issue y pégala en el reporte y en el README del repo. (Las imágenes del `.md` se ven en GitHub solo después de hacer push.)
6. **Tarea 11:** abre `tests/tarea11.spec.ts` y cambia `TU-USUARIO` en `URL_ISSUE` por tu usuario de GitHub (y el número del Issue que corresponda). Luego: `npx playwright test tests/tarea11.spec.ts --project=chromium` → **`2 passed, 1 skipped`**.
7. **Reto 1 de la tarea** imprime "N de 12 cargas mostraron el error de texto": ese N es **tu** medición real; úsalo en `reportes/bug-report-typos-clase11.md` y regístralo como un segundo Issue (Baja / P4 / Intermitente). Captura la frecuencia en el reporte HTML y el `borrador-reporte.md` adjunto del reto 3.

---

## 5. Qué verificar tú (lo que no se pudo comprobar al escribir el paquete)

Los laboratorios son copia literal de las diapositivas. Los **retos y documentos** se escribieron como referencia sin ejecutarlos contra los sitios reales (desde el entorno donde se prepararon no había acceso a ellos). El código sí compila y Playwright lista todos los tests (79 en total en chromium). Por eso:

- **Clase 4 (retos):** locators del formulario Place Order de DemoBlaze (`#name`, `#country`, `#city`, `#card`, `#orderModal`) y del modal de login.
- **Clase 5 (retos):** que el primer precio al ordenar `lohi` sea `$7.99` y que el cursor del botón sea `pointer`.
- **Clase 6 (retos):** ids del checkout (`#first-name`, `#last-name`, `#postal-code`, `#continue`, `#finish`), el texto `Thank you for your order!`, `#logout_sidebar_link` y que el badge desaparezca (`toHaveCount(0)`).
- **Tabla de decisión:** todas las filas marcadas con (?) son hipótesis hasta que las compruebes a mano.
- **Clase 10, reto 3:** la rama de `webkit` es una demostración de la técnica, no una diferencia real observada.
- **Si algo falla:** `npx playwright codegen <url>` te muestra el locator correcto; corrígelo en el test y anótalo.

---

## 6. Commits (los haces tú, en este orden sugerido)

Un commit por clase, con `git add` archivo por archivo. Ejemplo para la Clase 4:

```bash
git add tests/clase04.spec.ts tareas/tarea-04.md evidencias/registro-llenado.png evidencias/carrito-con-producto.png README.md
git commit -m "Clase 4: tests de actions y tests reto"
git push
```

Mismo patrón para las demás (agrega solo los archivos de esa clase y sus capturas). No subas `node_modules/`, `test-results/` ni `playwright-report/` (ya están en `.gitignore`).

Mensajes sugeridos: `Clase 5: assertions y tabla de decisión`, `Clase 6: Page Object Model y tests reto`, `Clase 8: hooks, suites y SQA Plan`, `Clase 9: fixtures y tests parametrizados`, `Clase 10: multi-browser y tags`, `Clase 11: bugs, reportes y GitHub Issues`.

---

## 7. Cuando termines una clase, anótalo aquí

- [ ] Clase 4  - [ ] Clase 5  - [ ] Clase 6  - [ ] Clase 8  - [ ] Clase 9  - [ ] Clase 10  - [ ] Clase 11
