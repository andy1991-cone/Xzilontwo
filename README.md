// ===== ARIES Quantum Symmetric RGB Engine =====

const int L_R = 3;
const int L_G = 5;
const int L_B = 6;

const int R_R = 9;
const int R_G = 10;
const int R_B = 11;

float hue = 0;

void setup() {
  pinMode(L_R, OUTPUT);
  pinMode(L_G, OUTPUT);
  pinMode(L_B, OUTPUT);
  pinMode(R_R, OUTPUT);
  pinMode(R_G, OUTPUT);
  pinMode(R_B, OUTPUT);
}

// Konversi HSV ke RGB
void HSVtoRGB(float h, float s, float v, int &r, int &g, int &b) {
  int i = int(h * 6);
  float f = h * 6 - i;
  float p = v * (1 - s);
  float q = v * (1 - f * s);
  float t = v * (1 - (1 - f) * s);

  float r_f, g_f, b_f;

  switch (i % 6) {
    case 0: r_f = v; g_f = t; b_f = p; break;
    case 1: r_f = q; g_f = v; b_f = p; break;
    case 2: r_f = p; g_f = v; b_f = t; break;
    case 3: r_f = p; g_f = q; b_f = v; break;
    case 4: r_f = t; g_f = p; b_f = v; break;
    case 5: r_f = v; g_f = p; b_f = q; break;
  }

  r = r_f * 255;
  g = g_f * 255;
  b = b_f * 255;
}

void loop() {

  int r1, g1, b1;
  int r2, g2, b2;

  // Warna kiri
  HSVtoRGB(hue, 1.0, 1.0, r1, g1, b1);

  // Warna kanan (complementary 180°)
  float complementaryHue = fmod(hue + 0.5, 1.0);
  HSVtoRGB(complementaryHue, 1.0, 1.0, r2, g2, b2);

  // Efek gelombang brightness
  float wave = (sin(millis() * 0.002) + 1) / 2.0;

  analogWrite(L_R, r1 * wave);
  analogWrite(L_G, g1 * wave);
  analogWrite(L_B, b1 * wave);

  analogWrite(R_R, r2 * wave);
  analogWrite(R_G, g2 * wave);
  analogWrite(R_B, b2 * wave);

  hue += 0.002;
  if (hue > 1) hue = 0;

  delay(10);
}
