#include <esp_timer.h>

// ---------------- CONFIGURAÇÃO DOS PINOS ----------------
#define SENSOR_PIN 34

// 7 segmentos (comum ANODO → nível baixo acende)
int segPins[7] = {4, 16, 17, 5, 18, 19, 21};

// 5 displays (ativação de cada dígito)
int digitPins[5] = {12, 14, 27, 26, 25};

// ---------------- TABELA DE SEGMENTOS ----------------
// 0b GFEDCBA   (0 = aceso no comum-anodo)
const uint8_t digits[10] = {
  0b0000001, // 0
  0b1001111, // 1
  0b0010010, // 2
  0b0000110, // 3
  0b1001100, // 4
  0b0100100, // 5
  0b0100000, // 6
  0b0001111, // 7
  0b0000000, // 8
  0b0000100  // 9
};

// ---------------- VARIÁVEIS DO SISTEMA ----------------
volatile uint32_t pulsoCount = 0;
volatile uint32_t lastTime = 0;
volatile uint32_t rpm = 0;

volatile int muxIndex = 0;
int displayBuffer[5] = {0,0,0,0,0};

// ------------------------------------------------------
//    INTERRUPÇÃO DO SENSOR ÓPTICO (1 pulso por volta)
// ------------------------------------------------------
void IRAM_ATTR sensorISR() {
  pulsoCount++;
}

// ------------------------------------------------------
//     TIMER DE 100 µs PARA MULTIPLEXAR OS DISPLAYS
// ------------------------------------------------------
void IRAM_ATTR muxTimerISR(void* arg) {
  for (int i=0; i<5; i++) digitalWrite(digitPins[i], HIGH);

  uint8_t seg = digits[ displayBuffer[muxIndex] ];

  for (int b=0; b<7; b++) {
    digitalWrite(segPins[b], (seg & (1 << b)) ? HIGH : LOW);
  }

  digitalWrite(digitPins[muxIndex], LOW);

  muxIndex++;
  if (muxIndex >= 5) muxIndex = 0;
}

// ------------------------------------------------------
//       TIMER DE 1 SEGUNDO PARA CALCULAR RPM
// ------------------------------------------------------
void IRAM_ATTR rpmTimerISR(void* arg) {
  uint32_t count = pulsoCount;
  pulsoCount = 0;

  rpm = count * 60;

  int n = rpm;
  for (int i=4; i>=0; i--) {
    displayBuffer[i] = n % 10;
    n /= 10;
  }
}

// ------------------------------------------------------
//                      SETUP
// ------------------------------------------------------
void setup() {
  Serial.begin(115200);

  pinMode(SENSOR_PIN, INPUT);

  for (int i=0; i<7; i++) pinMode(segPins[i], OUTPUT);
  for (int i=0; i<5; i++) pinMode(digitPins[i], OUTPUT);

  attachInterrupt(SENSOR_PIN, sensorISR, RISING);

  const esp_timer_create_args_t muxTimerArgs = {
    .callback = &muxTimerISR,
    .arg = NULL,
    .dispatch_method = ESP_TIMER_TASK,
    .name = "muxTimer"
  };

  esp_timer_handle_t muxTimer;
  esp_timer_create(&muxTimerArgs, &muxTimer);
  esp_timer_start_periodic(muxTimer, 100); // 100 µs

  const esp_timer_create_args_t rpmTimerArgs = {
    .callback = &rpmTimerISR,
    .arg = NULL,
    .dispatch_method = ESP_TIMER_TASK,
    .name = "rpmTimer"
  };

  esp_timer_handle_t rpmTimer;
  esp_timer_create(&rpmTimerArgs, &rpmTimer);
  esp_timer_start_periodic(rpmTimer, 1000000);
}

// ------------------------------------------------------
//                      LOOP
// ------------------------------------------------------
void loop() {
 static unsigned long ultima = 0;
 unsigned long agora = millis();
    
    if (agora - ultima >= 1000) {   // a cada 1 segundo
        noInterrupts();
        unsigned long p = pulsos;
        pulsos = 0;
        interrupts();
        
        rpm = p * 60;
        if (rpm > 99999) rpm = 99999;
        
        // atualizar os dígitos do display
      
}
// Atualiza os dígitos individuais SEM zeros à esquerda
unsigned long v = rpm;
for (int i = NUM_DIGITS - 1; i >= 0; i--) {
    dig[i] = v % 10;  
    v /= 10;
}

// Agora removemos os zeros à esquerda
bool encontrouDigito = false;

for (int i = 0; i < NUM_DIGITS; i++) {
    if (dig[i] != 0) {
        encontrouDigito = true;
    } else if (!encontrouDigito) {
        // apaga esse dígito antes do primeiro número real
        dig[i] = -1; 
    }
}

// Caso RPM seja zero: apagar todos os dígitos
if (rpm == 0) {
    for (int i = 0; i < NUM_DIGITS; i++) {
        dig[i] = -1;
    }
}

