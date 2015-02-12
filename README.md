//# INTERACTIVIDAD
//juego ping ball
boolean gameOver=true;

PImage imagen;
PImage fondo;

PFont fuente;

int puntaje1;
int puntaje2;

Bola bola;
Barra barra1;
Barra2 barra2;

void setup() {
  size(880, 426);
  fondo = loadImage ("fondo.jpg");
  imagen = loadImage ("mesa.jpg");
  fuente= loadFont ("Courier-Bold-48.vlw");

  bola = new Bola();
  barra1 = new Barra(0.95*width, height/2);
  barra2 = new Barra2(0.05*width, height/2);
  puntaje1=0;
  puntaje2=0;
}



void draw() {



  if (gameOver==true) {
    image (fondo, 0, 0);
    noStroke ();
    fill (149, 0, 2);
    ellipse(width/2, height/2, 100, 100);

    textAlign (CENTER);
    textFont (fuente, 25);
    fill (0);
    text("JUGAR", width/2, height/2);
  } else {
  image(imagen, 0, 0);
  barra1.moverY(mouseY);
  barra1.display();
  barra2.display();
  bola.detectarChoqueDesdeIzquierda(barra1.posX, barra1.posY);
  bola.detectarChoqueDesdeDerecha(barra2.pX, barra2.pY);
  bola.mover();
  bola.display();
  GameOver();

  fill (31, 202, 216);
  text("JUGADOR1", width*0.8, 0.18*height);
  text (puntaje1, width*0.8, height/4);


  fill (4, 106, 211);
  text("JUGADOR2", width/4, 0.18*height);
  text (puntaje2, width/4, height/4);
}
}

void GameOver() {

  if ( puntaje1 == 5 ) {
    image (fondo, 0, 0);
    noStroke ();
    fill (149, 0, 2);
    ellipse(width/2, height/2, 100, 100);

    textAlign (CENTER);
    textFont (fuente, 25);
    fill (0);
    text("JUGAR", width/2, height/2);
    bola.detener();
  }

  if ( puntaje2 == 5 ) {
    image (fondo, 0, 0);
    noStroke ();
    fill (149, 0, 2);
    ellipse(width/2, height/2, 100, 100);

    textAlign (CENTER);
    textFont (fuente, 25);
    fill (0);
    text("JUGAR", width/2, height/2);
    bola.detener();
  }
}
void mouseClicked () {

  if (mouseX >= width/2 -50 && mouseX <= width/2+50
    && mouseY >= height/2 -50
    && mouseY <= height/2+50 ) {
    gameOver=false;
    puntaje1 = 0;
    puntaje2 = 0;
  }
}



void keyPressed() {

  if ( keyCode == UP ) {  
    barra2.moverParaArriba();
  } 
  if (keyCode == DOWN ) {
    barra2.moverParaAbajo();
  }
}



// Barra
class Barra {

  float posX;
  float posY;
  color relleno;

  Barra() {
    posX = width/2;
    posY = height/2;
  }

  Barra(float posX_, float posY_) {
    posX = posX_;
    posY = posY_;
  }

  void moverY(float posY_) {
    posY = posY_;
  }



  void display () {
    rectMode(CENTER);
    fill (31, 202, 216);
    rect(posX, posY, 10, 80);
  }
}


//barra2
class Barra2 {

  float pX;
  float pY;
  color relleno;

  Barra2() {
    pX = width/2;
    pY = height/2;
  }

  Barra2(float pX_, float pY_) {
    pX = pX_;
    pY = pY_;
  }



  void moverParaArriba() {
    pY = pY-80;
  }

  void moverParaAbajo() {
    pY = pY+80;
  }

  void display () {
    rectMode(CENTER);
    fill (4, 106, 211);
    rect(pX, pY, 10, 80);
  }
}



//bola

class Bola {

  float posX;
  float posY;
  float speedX;
  float speedY;
  color relleno;


  Bola () {
    posX = width/2;
    posY = height/2;
    speedX = random(2, 5);
    speedY = random(2, 5);
    relleno= color((int)random(255), (int)random(255), (int)random(255));
  }
  void display () {
    fill(relleno);
    ellipse(posX, posY, 20, 20);
  }

  void detener() {
    posX = width/2;
    posY = height/2;
  }

  void mover() {
    if (posY>height) {
      speedY = -1*speedY;
      posY = height;
    } else if (posY<0) {
      speedY = -1*speedY;
      posY = 0;
    } else if (posX>width) {
      puntaje2=puntaje2+1;
      posX = width/2;
      posY=height/2;
    } else if (posX<0) {
      puntaje1=puntaje1+1;
      posX = width/2;
      posY=height/2;
    }
    posY += speedY;
    posX += speedX;
  }


  void detectarChoqueDesdeIzquierda(float posXbarra1_, float posYbarra1_) {
    if (posX >= posXbarra1_-5 && posX <= posXbarra1_+5 && posY < posYbarra1_+40 && posY > posYbarra1_-40) {
      speedX = -1*speedX;
      speedY = speedY;
      posX = posXbarra1_-5;
    }
  }


  void detectarChoqueDesdeDerecha(float pXbarra2_, float pYbarra2_) {
    if (posX >= pXbarra2_-5 && posX <= pXbarra2_+5 && posY < pYbarra2_+40 && posY > pYbarra2_-40) {
      speedX = -1*speedX;
      speedY = speedY;
    }
  }
}

