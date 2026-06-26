# Avance Examen

Agustín Bastidas - Pablo Godoy 

![storyboard-1](./img/storyboard-1.jpeg)

![storyboard-1](./img/storyboard-2.jpeg)
```
let page = 0;

// Mapa de navegación
let pages = [
  { right: 1 }, // Página 1
  { right: 2 }, // Página 2
  { down: 3 },  // Página 3
  { right: 4 }, // Página 4
  { right: 5 }, // Página 5
  { right: 6 }, // Página 6
  { down: 7 },  // Página 7
  {}            // Página 8
];

let canTrigger = true;

let rewinding = false;
let rewindTimer = 0;
let rewindDuration = 300; // ~5 segundos

let secretPage = false;

function setup() {
  createCanvas(windowWidth, windowHeight);
  rectMode(CORNER);
}

function draw() {

  background(0);

  if (secretPage) {
    drawSecretPage();
    return;
  }

  if (rewinding) {
    drawRewind();
    return;
  }

  checkNavigation();

  drawPage(page);

  if (page === 7) {
    drawRewindButton();
  }
}

// --------------------------------------------------
// NAVEGACIÓN
// --------------------------------------------------

function checkNavigation() {

  let current = pages[page];

  let rightZone = width * 0.75;
  let bottomZone = height * 0.75;

  // Para volver a activar navegación,
  // el mouse debe regresar a la zona central.
  if (
    mouseX < width * 0.5 &&
    mouseY < height * 0.5
  ) {
    canTrigger = true;
  }

  if (!canTrigger) return;

  if (
    current.right !== undefined &&
    mouseX > rightZone
  ) {
    page = current.right;
    canTrigger = false;
  }

  else if (
    current.down !== undefined &&
    mouseY > bottomZone
  ) {
    page = current.down;
    canTrigger = false;
  }
}

// --------------------------------------------------
// DIBUJO DE PÁGINAS
// --------------------------------------------------

function drawPage(p) {

  fill(255);
  textAlign(CENTER);
  textSize(24);

  text(
    "Página " + (p + 1),
    width / 2,
    60
  );

  switch (p) {

    // Página 1
    case 0:

      drawBox(
        width / 2 - 60,
        height / 2 - 60,
        120,
        120
      );

      break;

    // Página 2
    case 1:

      drawBox(
        80,
        height / 2 - 60,
        120,
        120
      );

      drawBox(
        width - 200,
        height / 2 - 60,
        120,
        120
      );

      break;

    // Página 3
    case 2:

      drawBox(
        width / 2 - 60,
        height / 2 - 60,
        120,
        120
      );

      break;

    // Página 4
    case 3:

      drawBox(
        80,
        height / 2 - 60,
        120,
        120
      );

      drawBox(
        width / 2 - 60,
        height / 2 - 60,
        120,
        120
      );

      drawBox(
        width - 200,
        height / 2 - 60,
        120,
        120
      );

      break;

    // Página 5
    case 4:

      drawBox(
        width / 2 - 60,
        height / 2 - 60,
        120,
        120
      );

      break;

    // Página 6
    case 5:

      drawBox(
        width / 2 - 60,
        height / 2 - 60,
        120,
        120
      );

      break;

    // Página 7
    case 6:

      drawBox(
        width / 2 - 60,
        height / 2 - 60,
        120,
        120
      );

      break;

    // Página 8
    case 7:

      drawBox(
        width / 2 - 60,
        height / 2 - 60,
        120,
        120
      );

      break;
  }

  drawDirectionHints();
}

// --------------------------------------------------
// CUADROS CON HOVER
// --------------------------------------------------

function drawBox(x, y, w, h) {

  let hover =
    mouseX > x &&
    mouseX < x + w &&
    mouseY > y &&
    mouseY < y + h;

  noStroke();

  if (hover) {
    fill(0, 255, 255);
  } else {
    fill(255);
  }

  rect(x, y, w, h);
}

// --------------------------------------------------
// INDICADORES VISUALES
// --------------------------------------------------

function drawDirectionHints() {

  let current = pages[page];

  noStroke();

  if (current.right !== undefined) {

    fill(255, 50);

    rect(
      width * 0.75,
      0,
      width * 0.25,
      height
    );

    fill(255);
    textSize(20);

    text(
      "→",
      width * 0.88,
      height / 2
    );
  }

  if (current.down !== undefined) {

    fill(255, 50);

    rect(
      0,
      height * 0.75,
      width,
      height * 0.25
    );

    fill(255);
    textSize(20);

    text(
      "↓",
      width / 2,
      height * 0.9
    );
  }
}

// --------------------------------------------------
// BOTÓN REWIND
// --------------------------------------------------

function drawRewindButton() {

  rectMode(CENTER);

  fill(255);

  rect(
    width / 2,
    height - 80,
    220,
    60,
    10
  );

  fill(0);

  textSize(20);

  text(
    "REWIND",
    width / 2,
    height - 73
  );

  rectMode(CORNER);
}

// --------------------------------------------------
// REWIND
// --------------------------------------------------

function drawRewind() {

  background(0);

  let progress =
    rewindTimer / rewindDuration;

  let current =
    floor(
      map(
        progress,
        0,
        1,
        7,
        0
      )
    );

  current = constrain(current, 0, 7);

  push();

  let shake = random(-5, 5);

  translate(shake, shake);

  drawPage(current);

  pop();

  fill(255, 80, 80);

  textSize(42);

  text(
    "⏪ REWIND",
    width / 2,
    height - 120
  );

  rewindTimer++;

  if (rewindTimer >= rewindDuration) {

    rewinding = false;
    secretPage = true;
  }
}

// --------------------------------------------------
// PÁGINA SECRETA
// --------------------------------------------------

function drawSecretPage() {

  background(0);

  fill(255, 255, 0);

  rectMode(CENTER);

  rect(
    width / 2,
    height / 2,
    260,
    260
  );

  fill(255);

  textSize(36);

  text(
    "PÁGINA SECRETA",
    width / 2,
    120
  );

  textSize(20);

  text(
    "Easter Egg desbloqueado",
    width / 2,
    height - 150
  );

  fill(255);

  rect(
    width / 2,
    height - 70,
    220,
    60,
    10
  );

  fill(0);

  textSize(20);

  text(
    "RESTART",
    width / 2,
    height - 63
  );

  rectMode(CORNER);
}

// --------------------------------------------------
// CLICKS
// --------------------------------------------------

function mousePressed() {

  // BOTÓN RESTART

  if (secretPage) {

    if (
      mouseX > width / 2 - 110 &&
      mouseX < width / 2 + 110 &&
      mouseY > height - 100 &&
      mouseY < height - 40
    ) {

      page = 0;
      rewinding = false;
      rewindTimer = 0;
      secretPage = false;
      canTrigger = true;

      return;
    }
  }

  // BOTÓN REWIND

  if (page === 7) {

    let bx = width / 2;
    let by = height - 80;

    if (
      mouseX > bx - 110 &&
      mouseX < bx + 110 &&
      mouseY > by - 30 &&
      mouseY < by + 30
    ) {

      rewinding = true;
      rewindTimer = 0;
    }
  }
}

// --------------------------------------------------

function windowResized() {
  resizeCanvas(windowWidth, windowHeight);
}

```
-----------------------------------------------------
# Codigos Bocetos De Paginas
----------------------------------------------------- 
# Pagina 1

```
let teclas = {
  up: false,
  down: false,
  left: false,
  right: false
};

// Medidas de la imagen que me enviaste
let baseW = 1228;
let baseH = 689;

function setup() {
  createCanvas(1920, 1080);
  pixelDensity(1);
}

function draw() {
  background(0);
  noStroke();

  // Escala para pasar de tu boceto a 1920x1080
  let sx = width / baseW;
  let sy = height / baseH;

  // -------------------------------
  // POSICIONES SEGÚN TU IMAGEN
  // -------------------------------

  let mouseBox = {
    x: 137,
    y: 119,
    w: 71,
    h: 71
  };

  let textoMouse = {
    x: 51,
    y: 198,
    w: 247,
    h: 118
  };

  let bloqueAmarillo = {
    x: 368,
    y: 76,
    w: 488,
    h: 535
  };

  let teclaUp = {
    x: 139,
    y: 377,
    w: 71,
    h: 71
  };

  let teclaLeft = {
    x: 60,
    y: 455,
    w: 71,
    h: 71
  };

  let teclaDown = {
    x: 139,
    y: 455,
    w: 71,
    h: 71
  };

  let teclaRight = {
    x: 218,
    y: 455,
    w: 70,
    h: 71
  };

  let textoTeclado = {
    x: 51,
    y: 540,
    w: 247,
    h: 71
  };

  let textoDerechaGrande = {
    x: 931,
    y: 76,
    w: 223,
    h: 418
  };

  let textoDerechaAbajo = {
    x: 931,
    y: 522,
    w: 223,
    h: 88
  };

  // -------------------------------
  // MOUSE
  // -------------------------------

  let hoverMouse = estaEncima(mouseBox, sx, sy);

  fill(hoverMouse ? 140 : 255);
  rect(
    mouseBox.x * sx,
    mouseBox.y * sy,
    mouseBox.w * sx,
    mouseBox.h * sy
  );

  dibujarCursorMouse(mouseBox, sx, sy);

  // -------------------------------
  // CUADRO BLANCO SUPERIOR IZQUIERDO
  // -------------------------------

  fill(255);
  rect(
    textoMouse.x * sx,
    textoMouse.y * sy,
    textoMouse.w * sx,
    textoMouse.h * sy
  );

  // -------------------------------
  // BLOQUE AMARILLO CENTRAL
  // -------------------------------

  let hoverBloque = estaEncima(bloqueAmarillo, sx, sy);

  if (hoverBloque) {
    fill(175, 165, 0);
  } else {
    fill(240, 225, 55);
  }

  rect(
    bloqueAmarillo.x * sx,
    bloqueAmarillo.y * sy,
    bloqueAmarillo.w * sx,
    bloqueAmarillo.h * sy
  );

  // -------------------------------
  // TECLAS
  // -------------------------------

  dibujarTecla(teclaUp, "up", teclas.up, sx, sy);
  dibujarTecla(teclaLeft, "left", teclas.left, sx, sy);
  dibujarTecla(teclaDown, "down", teclas.down, sx, sy);
  dibujarTecla(teclaRight, "right", teclas.right, sx, sy);

  // -------------------------------
  // CUADRO BLANCO INFERIOR IZQUIERDO
  // -------------------------------

  fill(255);
  rect(
    textoTeclado.x * sx,
    textoTeclado.y * sy,
    textoTeclado.w * sx,
    textoTeclado.h * sy
  );

  // -------------------------------
  // CUADROS DERECHA
  // -------------------------------

  fill(255);

  rect(
    textoDerechaGrande.x * sx,
    textoDerechaGrande.y * sy,
    textoDerechaGrande.w * sx,
    textoDerechaGrande.h * sy
  );

  rect(
    textoDerechaAbajo.x * sx,
    textoDerechaAbajo.y * sy,
    textoDerechaAbajo.w * sx,
    textoDerechaAbajo.h * sy
  );
}

// -------------------------------
// FUNCIONES
// -------------------------------

function estaEncima(obj, sx, sy) {
  return (
    mouseX > obj.x * sx &&
    mouseX < (obj.x + obj.w) * sx &&
    mouseY > obj.y * sy &&
    mouseY < (obj.y + obj.h) * sy
  );
}

function dibujarTecla(obj, direccion, activa, sx, sy) {
  fill(activa ? 120 : 255);

  rect(
    obj.x * sx,
    obj.y * sy,
    obj.w * sx,
    obj.h * sy
  );

  fill(0);

  let x = obj.x;
  let y = obj.y;
  let w = obj.w;
  let h = obj.h;

  if (direccion === "up") {
    triangle(
      (x + w / 2) * sx,
      (y + 8) * sy,
      (x + 16) * sx,
      (y + 45) * sy,
      (x + w - 16) * sx,
      (y + 45) * sy
    );

    rect(
      (x + w / 2 - 8) * sx,
      (y + 38) * sy,
      16 * sx,
      24 * sy
    );
  }

  if (direccion === "down") {
    triangle(
      (x + w / 2) * sx,
      (y + h - 8) * sy,
      (x + 16) * sx,
      (y + 26) * sy,
      (x + w - 16) * sx,
      (y + 26) * sy
    );

    rect(
      (x + w / 2 - 8) * sx,
      (y + 12) * sy,
      16 * sx,
      28 * sy
    );
  }

  if (direccion === "left") {
    triangle(
      (x + 8) * sx,
      (y + h / 2) * sy,
      (x + 45) * sx,
      (y + 16) * sy,
      (x + 45) * sx,
      (y + h - 16) * sy
    );

    rect(
      (x + 36) * sx,
      (y + h / 2 - 8) * sy,
      24 * sx,
      16 * sy
    );
  }

  if (direccion === "right") {
    triangle(
      (x + w - 8) * sx,
      (y + h / 2) * sy,
      (x + 26) * sx,
      (y + 16) * sy,
      (x + 26) * sx,
      (y + h - 16) * sy
    );

    rect(
      (x + 12) * sx,
      (y + h / 2 - 8) * sy,
      28 * sx,
      16 * sy
    );
  }
}

function dibujarCursorMouse(obj, sx, sy) {
  let x = obj.x;
  let y = obj.y;

  fill(0);

  beginShape();

  vertex((x + 17) * sx, (y + 13) * sy);
  vertex((x + 59) * sx, (y + 29) * sy);
  vertex((x + 44) * sx, (y + 39) * sy);
  vertex((x + 58) * sx, (y + 56) * sy);
  vertex((x + 47) * sx, (y + 62) * sy);
  vertex((x + 34) * sx, (y + 45) * sy);
  vertex((x + 23) * sx, (y + 57) * sy);

  endShape(CLOSE);
}

// -------------------------------
// INTERACCIÓN TECLADO
// -------------------------------

function keyPressed() {
  if (keyCode === UP_ARROW) {
    teclas.up = true;
  }

  if (keyCode === DOWN_ARROW) {
    teclas.down = true;
  }

  if (keyCode === LEFT_ARROW) {
    teclas.left = true;
  }

  if (keyCode === RIGHT_ARROW) {
    teclas.right = true;
  }

  return false;
}

function keyReleased() {
  if (keyCode === UP_ARROW) {
    teclas.up = false;
  }

  if (keyCode === DOWN_ARROW) {
    teclas.down = false;
  }

  if (keyCode === LEFT_ARROW) {
    teclas.left = false;
  }

  if (keyCode === RIGHT_ARROW) {
    teclas.right = false;
  }

  return false;
}

```
--------------------------
# Pagina 2
--------------------------

```
let baseW = 817;
let baseH = 456;

// Estados de opacidad
let opacidadIzq = 70;
let objetivoOpacidadIzq = 70;

let opacidadDer = 70;
let objetivoOpacidadDer = 70;

// Estados de escala
let escalaIzq = 1;
let objetivoEscalaIzq = 1;

let escalaDer = 1;
let objetivoEscalaDer = 1;

// Cambio de color del bloque amarillo derecho
let cambioColorDer = 0;
let objetivoCambioColorDer = 0;

// Opacidad de flechas
let opacidadFlecha1 = 255;
let objetivoOpacidadFlecha1 = 255;

let opacidadFlecha2 = 255;
let objetivoOpacidadFlecha2 = 255;

let opacidadFlecha3 = 255;
let objetivoOpacidadFlecha3 = 255;

// Control de avance
let pasoActual = 0;

function setup() {
  createCanvas(1920, 1080);
  pixelDensity(1);
}

function draw() {
  background(0);
  noStroke();

  let sx = width / baseW;
  let sy = height / baseH;

  // Transiciones suaves
  opacidadIzq = lerp(opacidadIzq, objetivoOpacidadIzq, 0.08);
  opacidadDer = lerp(opacidadDer, objetivoOpacidadDer, 0.08);

  escalaIzq = lerp(escalaIzq, objetivoEscalaIzq, 0.08);
  escalaDer = lerp(escalaDer, objetivoEscalaDer, 0.08);

  cambioColorDer = lerp(cambioColorDer, objetivoCambioColorDer, 0.08);

  opacidadFlecha1 = lerp(opacidadFlecha1, objetivoOpacidadFlecha1, 0.08);
  opacidadFlecha2 = lerp(opacidadFlecha2, objetivoOpacidadFlecha2, 0.08);
  opacidadFlecha3 = lerp(opacidadFlecha3, objetivoOpacidadFlecha3, 0.08);

  // -------------------------------
  // OBJETOS SEGÚN TU BOCETO
  // -------------------------------

  let bloqueAmarilloIzq = {
    x: 57,
    y: 39,
    w: 291,
    h: 195
  };

  let textoIzq = {
    x: 57,
    y: 249,
    w: 291,
    h: 130
  };

  let flecha1 = {
    x: 61,
    y: 395,
    w: 49,
    h: 48,
    dir: "right"
  };

  let flecha2 = {
    x: 178,
    y: 395,
    w: 49,
    h: 48,
    dir: "right"
  };

  let flecha3 = {
    x: 296,
    y: 395,
    w: 49,
    h: 48,
    dir: "up"
  };

  let bloqueAmarilloDer = {
    x: 527,
    y: 39,
    w: 231,
    h: 287
  };

  let textoDer = {
    x: 526,
    y: 337,
    w: 233,
    h: 42
  };

  let flechaDer = {
    x: 760,
    y: 395,
    w: 49,
    h: 48,
    dir: "down"
  };

  // -------------------------------
  // COLORES
  // -------------------------------

  let amarilloIzq = color(240, 225, 55, opacidadIzq);

  let amarilloDer = color(240, 225, 55, opacidadDer);
  let naranjoDer = color(255, 130, 20, opacidadDer);

  let colorBloqueDer = lerpColor(
    amarilloDer,
    naranjoDer,
    cambioColorDer
  );

  // -------------------------------
  // LADO IZQUIERDO
  // -------------------------------

  dibujarRectEscalado(
    bloqueAmarilloIzq,
    sx,
    sy,
    amarilloIzq,
    escalaIzq
  );

  dibujarRectEscalado(
    textoIzq,
    sx,
    sy,
    color(255, 255, 255, opacidadIzq),
    escalaIzq
  );

  // -------------------------------
  // LADO DERECHO
  // -------------------------------

  dibujarRectEscalado(
    bloqueAmarilloDer,
    sx,
    sy,
    colorBloqueDer,
    escalaDer
  );

  dibujarRectEscalado(
    textoDer,
    sx,
    sy,
    color(255, 255, 255, opacidadDer),
    escalaDer
  );

  // -------------------------------
  // FLECHAS VISUALES
  // -------------------------------

  dibujarFlechaBoton(flecha1, sx, sy, opacidadFlecha1);
  dibujarFlechaBoton(flecha2, sx, sy, opacidadFlecha2);
  dibujarFlechaBoton(flecha3, sx, sy, opacidadFlecha3);
  dibujarFlechaBoton(flechaDer, sx, sy, 255);
}

// --------------------------------
// INTERACCIÓN CON TECLADO
// --------------------------------

function keyPressed() {

  // Primera flecha visual: derecha
  if (pasoActual === 0 && keyCode === RIGHT_ARROW) {
    objetivoOpacidadIzq = 255;
    objetivoEscalaIzq = 1.05;

    objetivoOpacidadFlecha1 = 0;

    pasoActual = 1;
  }

  // Segunda flecha visual: derecha
  else if (pasoActual === 1 && keyCode === RIGHT_ARROW) {
    objetivoOpacidadDer = 255;
    objetivoEscalaDer = 1.05;

    objetivoOpacidadFlecha2 = 0;

    pasoActual = 2;
  }

  // Tercera flecha visual: arriba
  // Cambia el cuadro amarillo DERECHO a naranjo
  else if (pasoActual === 2 && keyCode === UP_ARROW) {
    objetivoCambioColorDer = 1;

    objetivoOpacidadFlecha3 = 0;

    pasoActual = 3;
  }

  return false;
}

// --------------------------------
// FUNCIONES DE DIBUJO
// --------------------------------

function dibujarRectEscalado(obj, sx, sy, col, escala) {
  let x = obj.x * sx;
  let y = obj.y * sy;
  let w = obj.w * sx;
  let h = obj.h * sy;

  let cx = x + w / 2;
  let cy = y + h / 2;

  push();

  translate(cx, cy);
  scale(escala);

  fill(col);

  rect(
    -w / 2,
    -h / 2,
    w,
    h
  );

  pop();
}

function dibujarFlechaBoton(obj, sx, sy, alpha) {
  if (alpha < 1) {
    return;
  }

  fill(255, alpha);

  rect(
    obj.x * sx,
    obj.y * sy,
    obj.w * sx,
    obj.h * sy
  );

  fill(0, alpha);

  let x = obj.x;
  let y = obj.y;
  let w = obj.w;
  let h = obj.h;

  if (obj.dir === "right") {
    triangle(
      (x + w - 8) * sx,
      (y + h / 2) * sy,
      (x + 17) * sx,
      (y + 10) * sy,
      (x + 17) * sx,
      (y + h - 10) * sy
    );

    rect(
      (x + 8) * sx,
      (y + h / 2 - 7) * sy,
      25 * sx,
      14 * sy
    );
  }

  if (obj.dir === "up") {
    triangle(
      (x + w / 2) * sx,
      (y + 8) * sy,
      (x + 12) * sx,
      (y + h - 17) * sy,
      (x + w - 12) * sx,
      (y + h - 17) * sy
    );

    rect(
      (x + w / 2 - 7) * sx,
      (y + 24) * sy,
      14 * sx,
      20 * sy
    );
  }

  if (obj.dir === "down") {
    triangle(
      (x + w / 2) * sx,
      (y + h - 8) * sy,
      (x + 12) * sx,
      (y + 17) * sy,
      (x + w - 12) * sx,
      (y + 17) * sy
    );

    rect(
      (x + w / 2 - 7) * sx,
      (y + 6) * sy,
      14 * sx,
      24 * sy
    );
  }
}

```

--------------------------
# Pagina 3
--------------------------

```
let baseW = 818;
let baseH = 460;

// -------------------------------
// OPACIDADES
// -------------------------------

let opacidadBlancos = 60;
let objetivoOpacidadBlancos = 60;

let opacidadGrupo1 = 0;
let objetivoOpacidadGrupo1 = 0;

let opacidadGrupo2 = 0;
let objetivoOpacidadGrupo2 = 0;

// -------------------------------
// ESCALAS
// -------------------------------

let escalaBlancos = 1;
let objetivoEscalaBlancos = 1;

let escalaGrupo1 = 0.96;
let objetivoEscalaGrupo1 = 0.96;

let escalaGrupo2 = 0.96;
let objetivoEscalaGrupo2 = 0.96;

// -------------------------------
// FLECHAS
// -------------------------------

let opacidadFlecha1 = 255;
let objetivoOpacidadFlecha1 = 255;

let opacidadFlecha2 = 255;
let objetivoOpacidadFlecha2 = 255;

let opacidadFlecha3 = 255;
let objetivoOpacidadFlecha3 = 255;

let opacidadFlecha4 = 255;

// Control de avance
let pasoActual = 0;

function setup() {
  createCanvas(1920, 1080);
  pixelDensity(1);
}

function draw() {
  background(0);
  noStroke();

  let sx = width / baseW;
  let sy = height / baseH;

  // -------------------------------
  // TRANSICIONES SUAVES
  // -------------------------------

  opacidadBlancos = lerp(opacidadBlancos, objetivoOpacidadBlancos, 0.08);
  opacidadGrupo1 = lerp(opacidadGrupo1, objetivoOpacidadGrupo1, 0.08);
  opacidadGrupo2 = lerp(opacidadGrupo2, objetivoOpacidadGrupo2, 0.08);

  escalaBlancos = lerp(escalaBlancos, objetivoEscalaBlancos, 0.08);
  escalaGrupo1 = lerp(escalaGrupo1, objetivoEscalaGrupo1, 0.08);
  escalaGrupo2 = lerp(escalaGrupo2, objetivoEscalaGrupo2, 0.08);

  opacidadFlecha1 = lerp(opacidadFlecha1, objetivoOpacidadFlecha1, 0.08);
  opacidadFlecha2 = lerp(opacidadFlecha2, objetivoOpacidadFlecha2, 0.08);
  opacidadFlecha3 = lerp(opacidadFlecha3, objetivoOpacidadFlecha3, 0.08);

  // -------------------------------
  // OBJETOS SEGÚN TU BOCETO
  // -------------------------------

  let cuadroBlancoIzq = {
    x: 57,
    y: 51,
    w: 134,
    h: 309
  };

  let cuadroBlancoCentro = {
    x: 309,
    y: 151,
    w: 200,
    h: 209
  };

  // Primeros 2 recuadros que rodean el cuadro blanco
  // Estos aparecen en el segundo paso.
  let recuadroInterior1 = {
    x: 264,
    y: 136,
    w: 290,
    h: 224
  };

  let recuadroInterior2 = {
    x: 251,
    y: 117,
    w: 316,
    h: 243
  };

  // Últimos 2 recuadros exteriores
  // Estos aparecen en el tercer paso.
  let recuadroExterior1 = {
    x: 235,
    y: 98,
    w: 348,
    h: 262
  };

  let recuadroExterior2 = {
    x: 215,
    y: 73,
    w: 388,
    h: 287
  };

  let flecha1 = {
    x: 58,
    y: 395,
    w: 49,
    h: 48,
    dir: "right"
  };

  let flecha2 = {
    x: 176,
    y: 395,
    w: 49,
    h: 48,
    dir: "right"
  };

  let flecha3 = {
    x: 293,
    y: 395,
    w: 49,
    h: 48,
    dir: "right"
  };

  let flecha4 = {
    x: 760,
    y: 395,
    w: 49,
    h: 48,
    dir: "right"
  };

  // -------------------------------
  // RECUADROS EXTERIORES
  // Se dibujan primero para quedar detrás.
  // -------------------------------

  dibujarRectEscalado(
    recuadroExterior2,
    sx,
    sy,
    color(178, 112, 112, opacidadGrupo2),
    escalaGrupo2
  );

  dibujarRectEscalado(
    recuadroExterior1,
    sx,
    sy,
    color(145, 45, 45, opacidadGrupo2),
    escalaGrupo2
  );

  // -------------------------------
  // RECUADROS INTERIORES
  // -------------------------------

  dibujarRectEscalado(
    recuadroInterior2,
    sx,
    sy,
    color(185, 85, 85, opacidadGrupo1),
    escalaGrupo1
  );

  dibujarRectEscalado(
    recuadroInterior1,
    sx,
    sy,
    color(174, 112, 112, opacidadGrupo1),
    escalaGrupo1
  );

  // -------------------------------
  // CUADROS BLANCOS
  // -------------------------------

  dibujarRectEscalado(
    cuadroBlancoIzq,
    sx,
    sy,
    color(255, 255, 255, opacidadBlancos),
    escalaBlancos
  );

  dibujarRectEscalado(
    cuadroBlancoCentro,
    sx,
    sy,
    color(255, 255, 255, opacidadBlancos),
    escalaBlancos
  );

  // -------------------------------
  // FLECHAS VISUALES
  // -------------------------------

  dibujarFlechaBoton(flecha1, sx, sy, opacidadFlecha1);
  dibujarFlechaBoton(flecha2, sx, sy, opacidadFlecha2);
  dibujarFlechaBoton(flecha3, sx, sy, opacidadFlecha3);

  // Esta queda para avanzar después a la siguiente página
  dibujarFlechaBoton(flecha4, sx, sy, opacidadFlecha4);
}

// -------------------------------
// INTERACCIÓN CON TECLADO
// -------------------------------

function keyPressed() {

  // Primera flecha: aparecen los 2 cuadros blancos
  if (pasoActual === 0 && keyCode === RIGHT_ARROW) {
    objetivoOpacidadBlancos = 255;
    objetivoEscalaBlancos = 1.04;

    objetivoOpacidadFlecha1 = 0;

    pasoActual = 1;
  }

  // Segunda flecha: aparecen los primeros 2 recuadros
  else if (pasoActual === 1 && keyCode === RIGHT_ARROW) {
    objetivoOpacidadGrupo1 = 255;
    objetivoEscalaGrupo1 = 1.03;

    objetivoOpacidadFlecha2 = 0;

    pasoActual = 2;
  }

  // Tercera flecha: aparecen los últimos 2 recuadros
  else if (pasoActual === 2 && keyCode === RIGHT_ARROW) {
    objetivoOpacidadGrupo2 = 255;
    objetivoEscalaGrupo2 = 1.03;

    objetivoOpacidadFlecha3 = 0;

    pasoActual = 3;
  }

  return false;
}

// -------------------------------
// FUNCIONES DE DIBUJO
// -------------------------------

function dibujarRectEscalado(obj, sx, sy, col, escala) {
  let x = obj.x * sx;
  let y = obj.y * sy;
  let w = obj.w * sx;
  let h = obj.h * sy;

  let cx = x + w / 2;
  let cy = y + h / 2;

  push();

  translate(cx, cy);
  scale(escala);

  fill(col);

  rect(
    -w / 2,
    -h / 2,
    w,
    h
  );

  pop();
}

function dibujarFlechaBoton(obj, sx, sy, alpha) {
  if (alpha < 1) {
    return;
  }

  fill(255, alpha);

  rect(
    obj.x * sx,
    obj.y * sy,
    obj.w * sx,
    obj.h * sy
  );

  fill(0, alpha);

  let x = obj.x;
  let y = obj.y;
  let w = obj.w;
  let h = obj.h;

  if (obj.dir === "right") {
    triangle(
      (x + w - 8) * sx,
      (y + h / 2) * sy,
      (x + 17) * sx,
      (y + 10) * sy,
      (x + 17) * sx,
      (y + h - 10) * sy
    );

    rect(
      (x + 8) * sx,
      (y + h / 2 - 7) * sy,
      25 * sx,
      14 * sy
    );
  }
}
```
--------------------------
# Pagina 4
--------------------------

```
let baseW = 1228;
let baseH = 689;

// -------------------------------
// OPACIDADES
// -------------------------------

let opacidadIzq = 70;
let objetivoOpacidadIzq = 70;

let opacidadDer = 70;
let objetivoOpacidadDer = 70;

// -------------------------------
// ESCALAS
// -------------------------------

let escalaIzq = 1;
let objetivoEscalaIzq = 1;

let escalaDer = 1;
let objetivoEscalaDer = 1;

// -------------------------------
// COLOR DEL BLOQUE DERECHO
// 0 = amarillo original
// 1,2,3 = cambios sucesivos
// -------------------------------

let estadoColorDer = 0;
let objetivoColorDer = 0;

// -------------------------------
// OPACIDAD DE FLECHAS
// -------------------------------

let opacidadF1 = 255;
let objetivoOpacidadF1 = 255;

let opacidadF2 = 255;
let objetivoOpacidadF2 = 255;

let opacidadF3 = 255;
let objetivoOpacidadF3 = 255;

let opacidadF4 = 255;
let objetivoOpacidadF4 = 255;

let opacidadF5 = 255;
let objetivoOpacidadF5 = 255;

let opacidadF6 = 255; // flecha hacia abajo, queda por ahora

// -------------------------------
// CONTROL DE PASOS
// -------------------------------

let pasoActual = 0;

function setup() {
  createCanvas(1920, 1080);
  pixelDensity(1);
}

function draw() {
  background(0);
  noStroke();

  let sx = width / baseW;
  let sy = height / baseH;

  // -------------------------------
  // TRANSICIONES SUAVES
  // -------------------------------

  opacidadIzq = lerp(opacidadIzq, objetivoOpacidadIzq, 0.08);
  opacidadDer = lerp(opacidadDer, objetivoOpacidadDer, 0.08);

  escalaIzq = lerp(escalaIzq, objetivoEscalaIzq, 0.08);
  escalaDer = lerp(escalaDer, objetivoEscalaDer, 0.08);

  estadoColorDer = lerp(estadoColorDer, objetivoColorDer, 0.08);

  opacidadF1 = lerp(opacidadF1, objetivoOpacidadF1, 0.08);
  opacidadF2 = lerp(opacidadF2, objetivoOpacidadF2, 0.08);
  opacidadF3 = lerp(opacidadF3, objetivoOpacidadF3, 0.08);
  opacidadF4 = lerp(opacidadF4, objetivoOpacidadF4, 0.08);
  opacidadF5 = lerp(opacidadF5, objetivoOpacidadF5, 0.08);

  // -------------------------------
  // OBJETOS SEGÚN TU BOCETO
  // -------------------------------

  let bloqueAmarilloIzq = {
    x: 88,
    y: 48,
    w: 348,
    h: 404
  };

  let textoIzq = {
    x: 88,
    y: 469,
    w: 348,
    h: 88
  };

  let bloqueAmarilloDer = {
    x: 791,
    y: 48,
    w: 347,
    h: 404
  };

  let textoDer = {
    x: 791,
    y: 469,
    w: 347,
    h: 88
  };

  let flecha1 = {
    x: 60,
    y: 594,
    w: 71,
    h: 71,
    dir: "right"
  };

  let flecha2 = {
    x: 263,
    y: 594,
    w: 71,
    h: 71,
    dir: "right"
  };

  let flecha3 = {
    x: 439,
    y: 594,
    w: 71,
    h: 71,
    dir: "up"
  };

  let flecha4 = {
    x: 614,
    y: 594,
    w: 71,
    h: 71,
    dir: "up"
  };

  let flecha5 = {
    x: 789,
    y: 594,
    w: 71,
    h: 71,
    dir: "up"
  };

  let flecha6 = {
    x: 1143,
    y: 594,
    w: 71,
    h: 71,
    dir: "down"
  };

  // -------------------------------
  // COLORES BLOQUE DERECHO
  // 3 cambios sucesivos
  // -------------------------------

  let colorBase = color(232, 220, 67, opacidadDer);   // amarillo
  let colorPaso1 = color(255, 160, 40, opacidadDer);  // naranjo
  let colorPaso2 = color(255, 95, 70, opacidadDer);   // coral rojizo
  let colorPaso3 = color(220, 80, 120, opacidadDer);  // rosado rojizo

  let colorActualDer;

  if (estadoColorDer <= 1) {
    colorActualDer = lerpColor(colorBase, colorPaso1, estadoColorDer);
  } else if (estadoColorDer <= 2) {
    colorActualDer = lerpColor(colorPaso1, colorPaso2, estadoColorDer - 1);
  } else {
    colorActualDer = lerpColor(colorPaso2, colorPaso3, estadoColorDer - 2);
  }

  // -------------------------------
  // LADO IZQUIERDO
  // -------------------------------

  dibujarRectEscalado(
    bloqueAmarilloIzq,
    sx,
    sy,
    color(232, 220, 67, opacidadIzq),
    escalaIzq
  );

  dibujarRectEscalado(
    textoIzq,
    sx,
    sy,
    color(255, 255, 255, opacidadIzq),
    escalaIzq
  );

  // -------------------------------
  // LADO DERECHO
  // -------------------------------

  dibujarRectEscalado(
    bloqueAmarilloDer,
    sx,
    sy,
    colorActualDer,
    escalaDer
  );

  dibujarRectEscalado(
    textoDer,
    sx,
    sy,
    color(255, 255, 255, opacidadDer),
    escalaDer
  );

  // -------------------------------
  // FLECHAS
  // -------------------------------

  dibujarFlechaBoton(flecha1, sx, sy, opacidadF1);
  dibujarFlechaBoton(flecha2, sx, sy, opacidadF2);
  dibujarFlechaBoton(flecha3, sx, sy, opacidadF3);
  dibujarFlechaBoton(flecha4, sx, sy, opacidadF4);
  dibujarFlechaBoton(flecha5, sx, sy, opacidadF5);
  dibujarFlechaBoton(flecha6, sx, sy, opacidadF6);
}

// -------------------------------
// INTERACCIÓN CON TECLADO
// -------------------------------

function keyPressed() {

  // 1) aparece lado izquierdo
  if (pasoActual === 0 && keyCode === RIGHT_ARROW) {
    objetivoOpacidadIzq = 255;
    objetivoEscalaIzq = 1.04;
    objetivoOpacidadF1 = 0;
    pasoActual = 1;
  }

  // 2) aparece lado derecho
  else if (pasoActual === 1 && keyCode === RIGHT_ARROW) {
    objetivoOpacidadDer = 255;
    objetivoEscalaDer = 1.04;
    objetivoOpacidadF2 = 0;
    pasoActual = 2;
  }

  // 3) primer cambio de color
  else if (pasoActual === 2 && keyCode === UP_ARROW) {
    objetivoColorDer = 1;
    objetivoOpacidadF3 = 0;
    pasoActual = 3;
  }

  // 4) segundo cambio de color
  else if (pasoActual === 3 && keyCode === UP_ARROW) {
    objetivoColorDer = 2;
    objetivoOpacidadF4 = 0;
    pasoActual = 4;
  }

  // 5) tercer cambio de color
  else if (pasoActual === 4 && keyCode === UP_ARROW) {
    objetivoColorDer = 3;
    objetivoOpacidadF5 = 0;
    pasoActual = 5;
  }

  return false;
}

// -------------------------------
// FUNCIONES DE DIBUJO
// -------------------------------

function dibujarRectEscalado(obj, sx, sy, col, escala) {
  let x = obj.x * sx;
  let y = obj.y * sy;
  let w = obj.w * sx;
  let h = obj.h * sy;

  let cx = x + w / 2;
  let cy = y + h / 2;

  push();
  translate(cx, cy);
  scale(escala);
  fill(col);
  rect(-w / 2, -h / 2, w, h);
  pop();
}

function dibujarFlechaBoton(obj, sx, sy, alpha) {
  if (alpha < 1) return;

  fill(255, alpha);
  rect(
    obj.x * sx,
    obj.y * sy,
    obj.w * sx,
    obj.h * sy
  );

  fill(0, alpha);

  let x = obj.x;
  let y = obj.y;
  let w = obj.w;
  let h = obj.h;

  if (obj.dir === "right") {
    triangle(
      (x + w - 10) * sx,
      (y + h / 2) * sy,
      (x + 22) * sx,
      (y + 12) * sy,
      (x + 22) * sx,
      (y + h - 12) * sy
    );

    rect(
      (x + 10) * sx,
      (y + h / 2 - 8) * sy,
      28 * sx,
      16 * sy
    );
  }

  if (obj.dir === "up") {
    triangle(
      (x + w / 2) * sx,
      (y + 8) * sy,
      (x + 14) * sx,
      (y + h - 22) * sy,
      (x + w - 14) * sx,
      (y + h - 22) * sy
    );

    rect(
      (x + w / 2 - 8) * sx,
      (y + 30) * sy,
      16 * sx,
      24 * sy
    );
  }

  if (obj.dir === "down") {
    triangle(
      (x + w / 2) * sx,
      (y + h - 8) * sy,
      (x + 14) * sx,
      (y + 22) * sy,
      (x + w - 14) * sx,
      (y + 22) * sy
    );

    rect(
      (x + w / 2 - 8) * sx,
      (y + 14) * sy,
      16 * sx,
      24 * sy
    );
  }
}

```
--------------------------
# Pagina 5
--------------------------

```
let baseW = 818;
let baseH = 460;

// -------------------------------
// CUADRO BLANCO INFERIOR
// -------------------------------

let opacidadTexto = 70;
let objetivoOpacidadTexto = 70;

let escalaTexto = 1;
let objetivoEscalaTexto = 1;

// -------------------------------
// ESTADO SWITCHES
// -------------------------------

let switch1Activo = false;
let switch2Activo = false;
let switch3Activo = false;

function setup() {
  createCanvas(1920, 1080);
  pixelDensity(1);
}

function draw() {
  background(0);
  noStroke();

  let sx = width / baseW;
  let sy = height / baseH;

  // -------------------------------
  // TRANSICIÓN SUAVE SOLO DEL TEXTO
  // -------------------------------

  opacidadTexto = lerp(opacidadTexto, objetivoOpacidadTexto, 0.08);
  escalaTexto = lerp(escalaTexto, objetivoEscalaTexto, 0.08);

  // -------------------------------
  // OBJETOS SEGÚN TU BOCETO
  // -------------------------------

  let bloque1 = {
    x: 78,
    y: 63,
    w: 153,
    h: 246
  };

  let bloque2 = {
    x: 332,
    y: 61,
    w: 153,
    h: 247
  };

  let bloque3 = {
    x: 584,
    y: 61,
    w: 153,
    h: 247
  };

  let switch1 = {
    x: 43,
    y: 181,
    w: 25,
    h: 32
  };

  let switch2 = {
    x: 298,
    y: 178,
    w: 25,
    h: 32
  };

  let switch3 = {
    x: 550,
    y: 178,
    w: 25,
    h: 33
  };

  let mouseIcon1 = {
    x: 39,
    y: 216,
    w: 11,
    h: 11
  };

  let mouseIcon2 = {
    x: 294,
    y: 215,
    w: 11,
    h: 11
  };

  let mouseIcon3 = {
    x: 546,
    y: 215,
    w: 11,
    h: 11
  };

  let cuadroTexto = {
    x: 57,
    y: 319,
    w: 232,
    h: 63
  };

  let flechaAbajo = {
    x: 760,
    y: 396,
    w: 49,
    h: 49,
    dir: "down"
  };

  // -------------------------------
  // CUADROS AMARILLOS
  // Aparecen de inmediato, sin fade
  // -------------------------------

  if (switch1Activo) {
    dibujarRectEscalado(
      bloque1,
      sx,
      sy,
      color(240, 225, 55, 255),
      1
    );
  }

  if (switch2Activo) {
    dibujarRectEscalado(
      bloque2,
      sx,
      sy,
      color(240, 225, 55, 255),
      1
    );
  }

  if (switch3Activo) {
    dibujarRectEscalado(
      bloque3,
      sx,
      sy,
      color(240, 225, 55, 255),
      1
    );
  }

  // -------------------------------
  // CUADRO BLANCO INFERIOR
  // -------------------------------

  dibujarRectEscalado(
    cuadroTexto,
    sx,
    sy,
    color(255, 255, 255, opacidadTexto),
    escalaTexto
  );

  // -------------------------------
  // SWITCHES
  // -------------------------------

  dibujarSwitch(switch1, sx, sy, switch1Activo);
  dibujarSwitch(switch2, sx, sy, switch2Activo);
  dibujarSwitch(switch3, sx, sy, switch3Activo);

  // -------------------------------
  // ÍCONOS PEQUEÑOS DE MOUSE
  // -------------------------------

  dibujarMousePequeno(mouseIcon1, sx, sy);
  dibujarMousePequeno(mouseIcon2, sx, sy);
  dibujarMousePequeno(mouseIcon3, sx, sy);

  // -------------------------------
  // FLECHA ABAJO
  // Por ahora solo visual
  // -------------------------------

  dibujarFlechaBoton(flechaAbajo, sx, sy, 255);
}

// -------------------------------
// INTERACCIÓN CON MOUSE
// -------------------------------

function mousePressed() {
  let sx = width / baseW;
  let sy = height / baseH;

  let switch1 = {
    x: 43,
    y: 181,
    w: 25,
    h: 32
  };

  let switch2 = {
    x: 298,
    y: 178,
    w: 25,
    h: 32
  };

  let switch3 = {
    x: 550,
    y: 178,
    w: 25,
    h: 33
  };

  if (estaEncima(switch1, sx, sy)) {
    switch1Activo = !switch1Activo;
    actualizarTexto();
  }

  if (estaEncima(switch2, sx, sy)) {
    switch2Activo = !switch2Activo;
    actualizarTexto();
  }

  if (estaEncima(switch3, sx, sy)) {
    switch3Activo = !switch3Activo;
    actualizarTexto();
  }
}

function actualizarTexto() {
  let algunSwitchActivo =
    switch1Activo ||
    switch2Activo ||
    switch3Activo;

  if (algunSwitchActivo) {
    objetivoOpacidadTexto = 255;
    objetivoEscalaTexto = 1.05;
  } else {
    objetivoOpacidadTexto = 70;
    objetivoEscalaTexto = 1;
  }
}

// -------------------------------
// FUNCIONES
// -------------------------------

function estaEncima(obj, sx, sy) {
  return (
    mouseX > obj.x * sx &&
    mouseX < (obj.x + obj.w) * sx &&
    mouseY > obj.y * sy &&
    mouseY < (obj.y + obj.h) * sy
  );
}

function dibujarRectEscalado(obj, sx, sy, col, escala) {
  let x = obj.x * sx;
  let y = obj.y * sy;
  let w = obj.w * sx;
  let h = obj.h * sy;

  let cx = x + w / 2;
  let cy = y + h / 2;

  push();

  translate(cx, cy);
  scale(escala);

  fill(col);

  rect(
    -w / 2,
    -h / 2,
    w,
    h
  );

  pop();
}

function dibujarSwitch(obj, sx, sy, activo) {
  if (activo) {
    fill(150);
  } else {
    fill(255);
  }

  rect(
    obj.x * sx,
    obj.y * sy,
    obj.w * sx,
    obj.h * sy
  );
}

function dibujarMousePequeno(obj, sx, sy) {
  fill(255);

  rect(
    obj.x * sx,
    obj.y * sy,
    obj.w * sx,
    obj.h * sy
  );

  fill(0);

  beginShape();

  vertex((obj.x + 2) * sx, (obj.y + 2) * sy);
  vertex((obj.x + 8) * sx, (obj.y + 5) * sy);
  vertex((obj.x + 6) * sx, (obj.y + 6) * sy);
  vertex((obj.x + 9) * sx, (obj.y + 9) * sy);
  vertex((obj.x + 7) * sx, (obj.y + 10) * sy);
  vertex((obj.x + 5) * sx, (obj.y + 7) * sy);
  vertex((obj.x + 3) * sx, (obj.y + 10) * sy);

  endShape(CLOSE);
}

function dibujarFlechaBoton(obj, sx, sy, alpha) {
  if (alpha < 1) return;

  fill(255, alpha);

  rect(
    obj.x * sx,
    obj.y * sy,
    obj.w * sx,
    obj.h * sy
  );

  fill(0, alpha);

  let x = obj.x;
  let y = obj.y;
  let w = obj.w;
  let h = obj.h;

  if (obj.dir === "down") {
    triangle(
      (x + w / 2) * sx,
      (y + h - 7) * sy,
      (x + 10) * sx,
      (y + 15) * sy,
      (x + w - 10) * sx,
      (y + 15) * sy
    );

    rect(
      (x + w / 2 - 7) * sx,
      (y + 7) * sy,
      14 * sx,
      25 * sy
    );
  }
}

```
--------------------------
# Pagina 6
--------------------------

```
let baseW = 818;
let baseH = 460;

// -------------------------------
// OPACIDADES PRINCIPALES
// -------------------------------

let opacidadPrincipal = 70;
let objetivoOpacidadPrincipal = 70;

let escalaPrincipal = 1;
let objetivoEscalaPrincipal = 1;

// -------------------------------
// CUADROS DERECHA
// -------------------------------

let opacidadDerecha = 0;
let objetivoOpacidadDerecha = 0;

let escalaDerecha = 0.96;
let objetivoEscalaDerecha = 0.96;

// -------------------------------
// COLOR BLOQUE AMARILLO
// -------------------------------

let estadoColor = 0;
let objetivoColor = 0;

// -------------------------------
// FLECHAS
// -------------------------------

let opacidadF1 = 255;
let objetivoOpacidadF1 = 255;

let opacidadF2 = 255;
let objetivoOpacidadF2 = 255;

let opacidadF3 = 255;
let objetivoOpacidadF3 = 255;

let opacidadF4 = 255;
let objetivoOpacidadF4 = 255;

// -------------------------------
// CONTROL DE PASOS
// -------------------------------

let pasoActual = 0;

function setup() {
  createCanvas(1920, 1080);
  pixelDensity(1);
}

function draw() {
  background(0);
  noStroke();

  let sx = width / baseW;
  let sy = height / baseH;

  // -------------------------------
  // TRANSICIONES SUAVES
  // -------------------------------

  opacidadPrincipal = lerp(opacidadPrincipal, objetivoOpacidadPrincipal, 0.08);
  escalaPrincipal = lerp(escalaPrincipal, objetivoEscalaPrincipal, 0.08);

  opacidadDerecha = lerp(opacidadDerecha, objetivoOpacidadDerecha, 0.08);
  escalaDerecha = lerp(escalaDerecha, objetivoEscalaDerecha, 0.08);

  estadoColor = lerp(estadoColor, objetivoColor, 0.08);

  opacidadF1 = lerp(opacidadF1, objetivoOpacidadF1, 0.08);
  opacidadF2 = lerp(opacidadF2, objetivoOpacidadF2, 0.08);
  opacidadF3 = lerp(opacidadF3, objetivoOpacidadF3, 0.08);
  opacidadF4 = lerp(opacidadF4, objetivoOpacidadF4, 0.08);

  // -------------------------------
  // OBJETOS SEGÚN TU BOCETO
  // -------------------------------

  let cuadroBlancoIzq = {
    x: 57,
    y: 38,
    w: 143,
    h: 335
  };

  let bloqueCentral = {
    x: 233,
    y: 38,
    w: 350,
    h: 335
  };

  let cuadroDerechaGrande = {
    x: 613,
    y: 218,
    w: 147,
    h: 101
  };

  let cuadroDerechaBoton = {
    x: 613,
    y: 330,
    w: 147,
    h: 42
  };

  let flecha1 = {
    x: 57,
    y: 397,
    w: 49,
    h: 48,
    dir: "right"
  };

  let flecha2 = {
    x: 175,
    y: 397,
    w: 49,
    h: 48,
    dir: "down"
  };

  let flecha3 = {
    x: 292,
    y: 397,
    w: 49,
    h: 48,
    dir: "down"
  };

  let flecha4 = {
    x: 409,
    y: 397,
    w: 49,
    h: 48,
    dir: "down"
  };

  // -------------------------------
  // COLOR DEL BLOQUE CENTRAL
  // -------------------------------

  let colorBase = color(240, 225, 55, opacidadPrincipal);
  let colorCambio1 = color(255, 165, 35, opacidadPrincipal);
  let colorCambio2 = color(255, 95, 65, opacidadPrincipal);
  let colorCambio3 = color(180, 70, 130, opacidadPrincipal);

  let colorActual;

  if (estadoColor <= 1) {
    colorActual = lerpColor(colorBase, colorCambio1, estadoColor);
  } else if (estadoColor <= 2) {
    colorActual = lerpColor(colorCambio1, colorCambio2, estadoColor - 1);
  } else {
    colorActual = lerpColor(colorCambio2, colorCambio3, estadoColor - 2);
  }

  // -------------------------------
  // CUADRO IZQUIERDO
  // -------------------------------

  dibujarRectEscalado(
    cuadroBlancoIzq,
    sx,
    sy,
    color(255, 255, 255, opacidadPrincipal),
    escalaPrincipal
  );

  // -------------------------------
  // BLOQUE CENTRAL
  // -------------------------------

  dibujarRectEscalado(
    bloqueCentral,
    sx,
    sy,
    colorActual,
    escalaPrincipal
  );

  // -------------------------------
  // CUADROS DERECHA
  // Aparecen SOLO después de presionar
  // las 3 flechas hacia abajo.
  // -------------------------------

  dibujarRectEscalado(
    cuadroDerechaGrande,
    sx,
    sy,
    color(255, 255, 255, opacidadDerecha),
    escalaDerecha
  );

  dibujarRectEscalado(
    cuadroDerechaBoton,
    sx,
    sy,
    color(255, 255, 255, opacidadDerecha),
    escalaDerecha
  );

  // -------------------------------
  // FLECHAS
  // -------------------------------

  dibujarFlechaBoton(flecha1, sx, sy, opacidadF1);
  dibujarFlechaBoton(flecha2, sx, sy, opacidadF2);
  dibujarFlechaBoton(flecha3, sx, sy, opacidadF3);
  dibujarFlechaBoton(flecha4, sx, sy, opacidadF4);
}

// -------------------------------
// INTERACCIÓN CON TECLADO
// -------------------------------

function keyPressed() {

  // 1) Primera interacción:
  // aparece cuadro izquierdo + bloque amarillo
  if (pasoActual === 0 && keyCode === RIGHT_ARROW) {
    objetivoOpacidadPrincipal = 255;
    objetivoEscalaPrincipal = 1.04;

    objetivoOpacidadF1 = 0;

    pasoActual = 1;
  }

  // 2) Primera flecha abajo:
  // primer cambio de color
  else if (pasoActual === 1 && keyCode === DOWN_ARROW) {
    objetivoColor = 1;

    objetivoOpacidadF2 = 0;

    pasoActual = 2;
  }

  // 3) Segunda flecha abajo:
  // segundo cambio de color
  else if (pasoActual === 2 && keyCode === DOWN_ARROW) {
    objetivoColor = 2;

    objetivoOpacidadF3 = 0;

    pasoActual = 3;
  }

  // 4) Tercera flecha abajo:
  // tercer cambio de color
  // y recién aquí aparecen los cuadros derechos
  else if (pasoActual === 3 && keyCode === DOWN_ARROW) {
    objetivoColor = 3;

    objetivoOpacidadDerecha = 255;
    objetivoEscalaDerecha = 1.04;

    objetivoOpacidadF4 = 0;

    pasoActual = 4;
  }

  return false;
}

// -------------------------------
// FUNCIONES DE DIBUJO
// -------------------------------

function dibujarRectEscalado(obj, sx, sy, col, escala) {
  let x = obj.x * sx;
  let y = obj.y * sy;
  let w = obj.w * sx;
  let h = obj.h * sy;

  let cx = x + w / 2;
  let cy = y + h / 2;

  push();
  translate(cx, cy);
  scale(escala);
  fill(col);
  rect(-w / 2, -h / 2, w, h);
  pop();
}

function dibujarFlechaBoton(obj, sx, sy, alpha) {
  if (alpha < 1) return;

  fill(255, alpha);

  rect(
    obj.x * sx,
    obj.y * sy,
    obj.w * sx,
    obj.h * sy
  );

  fill(0, alpha);

  let x = obj.x;
  let y = obj.y;
  let w = obj.w;
  let h = obj.h;

  if (obj.dir === "right") {
    triangle(
      (x + w - 8) * sx,
      (y + h / 2) * sy,
      (x + 17) * sx,
      (y + 10) * sy,
      (x + 17) * sx,
      (y + h - 10) * sy
    );

    rect(
      (x + 8) * sx,
      (y + h / 2 - 7) * sy,
      25 * sx,
      14 * sy
    );
  }

  if (obj.dir === "down") {
    triangle(
      (x + w / 2) * sx,
      (y + h - 8) * sy,
      (x + 12) * sx,
      (y + 17) * sy,
      (x + w - 12) * sx,
      (y + 17) * sy
    );

    rect(
      (x + w / 2 - 7) * sx,
      (y + 6) * sy,
      14 * sx,
      24 * sy
    );
  }
}
```
--------------------------
# Pagina 4
--------------------------

```
let baseW = 617;
let baseH = 314;

function setup() {
  createCanvas(1920, 1080);
  pixelDensity(1);
}

function draw() {
  background(0);
  noStroke();

  let sx = width / baseW;
  let sy = height / baseH;

  // -------------------------------
  // OBJETOS SEGÚN TU BOCETO
  // -------------------------------

  let cuadroAmarillo = {
    x: 209,
    y: 39,
    w: 187,
    h: 250
  };

  let cuadroTexto = {
    x: 441,
    y: 36,
    w: 130,
    h: 161
  };

  let botonReinicio = {
    x: 441,
    y: 211,
    w: 130,
    h: 36
  };

  // -------------------------------
  // CUADRO AMARILLO
  // -------------------------------

  fill(240, 225, 55);

  rect(
    cuadroAmarillo.x * sx,
    cuadroAmarillo.y * sy,
    cuadroAmarillo.w * sx,
    cuadroAmarillo.h * sy
  );

  // -------------------------------
  // CUADRO BLANCO DE TEXTO
  // -------------------------------

  fill(255);

  rect(
    cuadroTexto.x * sx,
    cuadroTexto.y * sy,
    cuadroTexto.w * sx,
    cuadroTexto.h * sy
  );

  // -------------------------------
  // BOTÓN VISUAL
  // Por ahora no tiene función.
  // Después servirá para reiniciar la historia.
  // -------------------------------

  fill(255);

  rect(
    botonReinicio.x * sx,
    botonReinicio.y * sy,
    botonReinicio.w * sx,
    botonReinicio.h * sy
  );
}
```
