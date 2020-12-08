# Plantalandia
Jogo no P5js Editor

//link vídeo explicativo do código e imagens utilizadas: https://drive.google.com/drive/u/0/folders/1k748CraEAasqEo9-VavW_6F2Dnun9qQK

var tela = 1;
var largura = 200;
var altura = 50;
var xMenu = 160;
var yMenu1 = 55;
var yMenu2 = 125;
var yMenu3 = 195;
var x = 170;
var y = 320;
var a = 0;
var l_x = -70;
var l_y = -90;
var l_x1 = 550;
var l_y1 = -90;
var velocidade = 4;
var v = 2;
var v2 = 2;
var w = 1;
var w2 = 1;
var vida = 3;
var pegou = 0;
var pegou2 = 0;
var x_a = 0;
var y_a = 0;
var x_t = 60;
var y_t = 70;
var score = 0;
var figura = [];
var fig = 0
var nivel = 1;
var n = 1;
var reg = 0;
var msc = 0

function preload() {
  plt = loadImage('planta.png');
  land = loadImage('land_6.png');
  fundo = loadImage('girassol.jpg');
  foto = loadImage('credito.jpeg');
  largata = loadImage('largata.png');
  core = loadImage('vida.png');
  figura[0] = loadImage('aguinha.png');
  figura[1] = loadImage('sol.png');
  reg = loadImage('aguinha.png');
  msc = createAudio('musica.mp3');
}

function setup() {
  createCanvas(500, 400);
  x_a = random(0, 450);
  y_a = random(0, 350);
  
}

function draw() {
  textStyle(NORMAL);
  msc.loop();

  //Tela de Menu
  if (tela == 1) {
    background(fundo);


    //Iniciar Jogo
    textAlign(CENTER);
    textSize(26);
    if (mouseX > xMenu && mouseX < xMenu + largura && mouseY > yMenu1 && mouseY < yMenu1 + altura) {
      stroke(300);
      fill(264, 164, 96);
      rect(xMenu, yMenu1, largura, altura, 25);
      if (mouseIsPressed) {
        tela = 6;
      }
    }
    fill(139, 69, 19);
    noStroke();
    text("Iniciar", 260, 90);

    //Opção "Informações"
    if (mouseX > xMenu && mouseX < xMenu + largura && mouseY > yMenu2 && mouseY < yMenu2 + altura) {
      stroke(300);
      fill(264, 164, 96);
      rect(xMenu, yMenu2, largura, altura, 25);
      if (mouseIsPressed) {
        tela = tela + 2;
      }
    }
    fill(139, 69, 19);
    noStroke();
    text("Informações", 260, 160);

    //Opção "Créditos"
    if (mouseX > xMenu && mouseX < xMenu + largura && mouseY > yMenu3 && mouseY < yMenu3 + altura) {
      stroke(300);
      fill(264, 164, 96);
      rect(xMenu, yMenu3, largura, altura, 25);
      if (mouseIsPressed) {
        tela = tela + 3;
      }

    }
    fill(139, 69, 19);
    noStroke();
    text("Créditos", 260, 230);
  }

  //Informações sobre o jogo
  if (tela == 3) {
    background(fundo);
    image(fundo, 0, 0, 500, 400);
    fill(255);
    rect(70, 80, 355, 240, 20, 15, 10, 5);
    fill(20);
    textSize(14);
    noStroke();
    textAlign(CENTER);
    text("Oi, pessoal! Sabe quais são as necessidades de uma planta? Elas precisam de água e luz para viver. Ajude a nossa plantinha a coletar esses elementos e desvie das lagartas que aparecerem para tentar lhe impedir. Prontos para uma aventura?", 80, 95, 340, 170);
    text("Público alvo: 2° ano do Ensino Fundamental", 80, 195, 340, 100);
    text("Matéria: Ciências", 80, 225, 340, 100);
    text("Habilidade: (EF02CI05) Investigar a importância da água e da luz para a manutenção da vida de plantas em geral.", 80, 255, 340, 170);
  }

  //Créditos do jogo
  if (tela == 4) {
    background(fundo);
    image(fundo, 0, 0, 500, 400);
    fill(255);
    rect(75, 80, 365, 248, 20, 15, 10, 5);
    image(foto, 150, 90, 210, 150);
    fill(20);
    textSize(16);
    noStroke();
    textAlign(CENTER);
    text("UFRN - Projeto Educacional, Lógica de Programação (LOP) . Programadora: Quelita Míriam Nunes Ferraz.", 80, 260, 360, 70);
  }



  //Jogo em ação - Nível 1
  else if (tela == 2) {
    background(100, 200, 250);
    noStroke();
    fill(20);
    image(land, 0, 0);
    image(land, 0, 255);
    image(land, 255, 0);
    image(land, 255, 255);
    image(core, 90, 57, 15, 15)
    image(plt, x, y, x_t, y_t);
    image(largata, a, 100, 50, 70);
    a = a + velocidade;
    fill(255);
    text('Vidas: ' + vida, 60, 70);
    text('Score: ' + score, 60, 50);
    text('Nível: ' + nivel, 60, 30)
    if (a > 440) {
      velocidade = -4;
    }
    if (a < -10) {
      velocidade = 4;
    }

    if (keyIsDown(LEFT_ARROW)) {
      if (x > -10) {
        x = x - 5;
      }
    }
    if (keyIsDown(RIGHT_ARROW)) {
      if (x < 520-x_t) {
        x = x + 5;
      }
    }
    if (keyIsDown(UP_ARROW)) {
      if (y > -15) {
        y -= 5;
      }
    }

    if (keyIsDown(DOWN_ARROW)) {
      if (y < 410-y_t) {
        y += 5;
      }
    }

    //Game Over
    if (dist(a + 25, 100 + 35, x + 25, y + 35) <= 25 + 35) {
      vida = vida - 1;
      x = 170;
      y = 320;
      if (vida == 0) {
        tela = 5;
      }
    }

    //Regador e Sol (Água e Luz)
    if (pegou < 5) {
      image(figura[fig], x_a, y_a, 50, 50);
    }


    //Nível 2
    if (score >= 100) {
      image(largata, l_x, l_y, 50, 70);
      l_x = l_x + v;
      l_y = l_y + v2;
      if (l_x > 450) {
        v = -2
      }
      if (l_x < 0) {
        v = 2
      }
      if (l_y > 350) {
        v2 = -2
      }
      if (l_y < 0) {
        v2 = 2
      }
      image(figura[fig], x_a, y_a, 50, 50);
      if (score == 500) {
        tela = 7
      }

    }

    if (dist(l_x + 25, l_y + 35, x + x_t / 2, y + y_t / 2) <= 25 + y_t / 2) {
      vida = vida - 1;
      x = 170;
      y = 320;
      if (vida == 0) {
        tela = 5;
      }
    }
    if (dist(x_a + 25, y_a + 25, x + x_t / 2, y + y_t / 2) <= 25 + y_t / 2) {
      x_a = random(50, 450);
      y_a = random(50, 350);

      if (pegou <= 4) {
        pegou = pegou + 1;
        score = score + 10
      } else if (pegou == 5) {
        pegou = 0;
        if (fig == 0) {
          fig = 1
        } else {
          fig = 0;
        }
        if (score == n*100) {
          nivel += 1;
          n += 1;
          x_t += 10;
          y_t += 10;
        }
      }
    }
    //Nivel 3
    if (nivel >= 3){
      if (dist(l_x1 + 25, l_y1 + 35, x + x_t / 2, y + y_t / 2) <= 25 + y_t / 2) {
      vida = vida - 1;
      x = 170;
      y = 320;
      if (vida == 0) {
        tela = 5;
      }
      }
      image(largata, l_x1, l_y1, 50, 70);
      l_x1 = l_x1 + w;
      l_y1 = l_y1 + w2;
      if (l_x1 > 450) {
        w = -7/2
      }
      if (l_x1 < 0) {
        w = 7/2
      }
      if (l_y1 > 350) {
        w2 = -7/2
      }
      if (l_y1 < 0) {
        w2 = 7/2
      }
      image(figura[fig], x_a, y_a, 50, 50);
      if (score == 500) {
        tela = 7
      }
      }


    //Voltar para a tela de início (Menu);
    if (tela == 2) {
      if (mouseX > 412 && mouseX < 412 + 70 && mouseY > 20 && mouseY < 20 + 70) {
        stroke(139, 69, 19);
        fill(222, 184, 135);
        rect(412, 20, 70, 50, 15);
        if (mouseIsPressed) {
          tela = 1;
          vida = 3;
          score = 0;
          nivel = 1;
          n = 1;
          l_x = -70;
          l_y = -90;
          pegou = 0;
          fig = 0;
          x = 170;
          y = 320;
          x_t = 60;
          y_t = 70;
          l_x1 = 550;
          l_y1 = -90;
        }
      }
      fill(255);
      noStroke();
      textSize(14);
      noStroke();
      textAlign(CENTER);
      text("Menu", 320, 40, 260, 260);
      
    }
  }

  if (tela == 3 || tela == 4) {
    if (mouseX > 213 && mouseX < 213 + 70 && mouseY > 20 && mouseY < 20 + 70) {
      stroke(300);
      fill(264, 164, 96);
      rect(213, 20, 70, 50, 20);
      if (mouseIsPressed) {
        tela = 1;
      }
    }
    fill(139, 69, 19);
    noStroke();
    textSize(14);
    noStroke();
    textAlign(CENTER);
    text("Menu", 120, 40, 260, 260);
  }

  if (tela == 5) {
    if (mouseX > 412 && mouseX < 412 + 70 && mouseY > 20 && mouseY < 20 + 70) {
      stroke(139, 69, 19);
      fill(222, 184, 135);
      rect(412, 20, 70, 50, 15);
      if (mouseIsPressed) {
        tela = 1;
        vida = 3;
        score = 0;
        nivel = 1;
        n = 1;
        l_x = -70;
        l_y = -90;
        pegou = 0;
        fig = 0;
        x = 170;
        y = 320;
        x_t = 60;
        y_t = 70;
        l_x1 = 550;
        l_y1 = -90;
      }
    }
    fill(255);
    noStroke();
    textSize(14);
    textAlign(CENTER);
    text("Menu", 320, 40, 260, 260);

    //Tela == 5 (Game Over)
    fill(255);
    rect(185, 150, 150, 100, 20);
    strokeWeight(2);
    fill(20);
    text("Game Over!", 185, 195, 150, 150);
    
  }

  //Tela == 6 (Instruções para o jogo)
  if (tela == 6) {
    if (mouseX > 213 && mouseX < 213 + 70 && mouseY > 269 && mouseY < 269 + 70) {
      stroke(139, 69, 19);
      fill(222, 184, 135);
      rect(213, 269, 70, 50, 15);
      if (mouseIsPressed) {
        tela = 2;
      }
    }
    fill(255);
    noStroke();
    textSize(14);
    textAlign(CENTER);
    text("Jogar", 120, 290, 260, 260);

    fill(264, 164, 96);
    rect(115, 50, 270, 200, 20);
    strokeWeight(2);
    fill(20);
    text("Ooi, aventureiros! Ajude o nosso girassol a crescer coletando os elementos essenciais para a sobrevivencia deles: água e luz solar. Se coletar 5 de cada você consegue avançar de nível e fazer a plantinha crescer. Boa sorte!", 150, 70, 200, 170);
  }

  //Tela == 7 (Fim de Jogo)
  if (tela == 7) {
    if (mouseX > 412 && mouseX < 412 + 70 && mouseY > 20 && mouseY < 20 + 70) {
      stroke(139, 69, 19);
      fill(222, 184, 135);
      rect(412, 20, 70, 50, 15);
      if (mouseIsPressed) {
        tela = 1;
        tela = 1;
        vida = 3;
        score = 0;
        nivel = 1;
        n = 1;
        l_x = -70;
        l_y = -90;
        pegou = 0;
        fig = 0;
        x = 170;
        y = 320;
        x_t = 60;
        y_t = 70;
        l_x1 = 550;
        l_y1 = -90;
      }
    }
    fill(255);
    noStroke();
    textSize(14);
    textAlign(CENTER);
    text("Menu", 320, 40, 260, 260);

    fill(255);
    rect(185, 150, 150, 100, 20);
    strokeWeight(2);
    fill(20);
    text("Fim de Jogo! Parabéns! Ajude as plantinhas no mundo", 185, 175, 150, 150);
    
  }
}
