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
