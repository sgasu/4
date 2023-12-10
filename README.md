# 4点击鼠标后，杂乱的粒子会回到有序的状态，呈现完整的图像

PImage targetImage;
ArrayList<Particle> particles;
boolean assembling = false;

void setup() {
  size(800, 600);
  targetImage = loadImage("portrait.jpg");  
  targetImage.resize(width, height);
  particles = new ArrayList<Particle>();
  for (int x = 0; x < targetImage.width; x += 5) {
    for (int y = 0; y < targetImage.height; y += 5) {
      int loc = x + y * targetImage.width;
      color c = targetImage.pixels[loc];
      particles.add(new Particle(x, y, c));
    }
  }
}

void draw() {
  background(255);

 
  for (Particle p : particles) {
    if (assembling) {
      p.assemble();
    }
    p.display();
  }

  if (assembling) {
    checkAssemblingCompletion();
  }
}

void mousePressed() {
  assembling = true;
}

void checkAssemblingCompletion() {
  boolean allAssembled = true;
  for (Particle p : particles) {
    if (!p.isAssembled()) {
      allAssembled = false;
      break;
    }
  }
  if (allAssembled) {
    assembling = false;
  }
}

class Particle {
  float x, y;
  float targetX, targetY;
  color col;
  float size = 6;  
  float easing = 0.05;
  boolean assembled = false;

  Particle(float x, float y, color col) {
    this.x = random(width);
    this.y = random(height);
    this.targetX = x;
    this.targetY = y;
    this.col = col;
  }

  void assemble() {
    float dx = targetX - x;
    float dy = targetY - y;
    x += dx * easing;
    y += dy * easing;

    float d = dist(x, y, targetX, targetY);
    if (d < 1.0) {
      assembled = true;
    }
  }

  void display() {
    noStroke();
    fill(col);
    ellipse(x, y, size, size);
  }

  boolean isAssembled() {
    return assembled;
  }
}
