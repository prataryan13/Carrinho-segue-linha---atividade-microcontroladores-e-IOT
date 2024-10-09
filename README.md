#define pinSensorDir 9 // roxo
#define pinSensorEsq 2 // cinza

#define dirFrente 3 // branco
#define dirTras   6 // amarelo
#define esqFrente 11 // verde
#define esqTras   10 // laranja



#define LINHA HIGH // está ligado quando está na linha

#define FRENTE  1 
#define PARADO  0
#define TRAS   -1

void configMotor();
void motorEsq(int direcao, byte velocidade = 80);
void motorDir(int direcao, byte velocidade = 80);

bool leituraEsquerda;
bool leituraDireita;

void setup() {

  pinMode(pinSensorDir, INPUT);
  pinMode(pinSensorEsq, INPUT);

  configMotor();

}

void loop() {

  bool valE = digitalRead(pinSensorEsq);
  bool valD = digitalRead(pinSensorDir);

  if (valE == LINHA && valD == LINHA) { // quando os sensores estão na linha 
    motorEsq(FRENTE, 80);
    motorDir(FRENTE, 80);
  } else if (valD == LINHA) { // quando o sensor direito está na linha
    motorEsq(FRENTE, 80);
    motorDir(TRAS, 80);
  } else if (valE == LINHA) { // quando o sensor esquerdo está na linha
    motorEsq(TRAS, 80);
    motorDir(FRENTE, 80);
  } else { // quando nenhum sensor está na linha
    motorEsq(PARADO);
    motorDir(PARADO);
  }
}


void configMotor() {

  pinMode(dirFrente,  OUTPUT);
  pinMode(dirTras,    OUTPUT);
  pinMode(esqFrente,  OUTPUT);
  pinMode(esqTras,    OUTPUT);

  digitalWrite(dirFrente,  LOW);
  digitalWrite(dirTras,    LOW);
  digitalWrite(esqFrente,  LOW);
  digitalWrite(esqTras,    LOW);
}

void motorEsq(int direcao, byte velocidade = 80) {

  switch (direcao) {
    case -1: {
        digitalWrite(esqFrente,  LOW);
        analogWrite (esqTras,    velocidade);
        break;
      }
    case 0: {
        digitalWrite(esqFrente,  HIGH);
        digitalWrite(esqTras,    HIGH);
        break;
      }
    case 1: {
        analogWrite (esqFrente,  velocidade);
        digitalWrite(esqTras,    LOW);
        break;
      }
  }
}

void motorDir(int direcao, byte velocidade = 80) {

  switch (direcao) {
    case -1: {
        digitalWrite(dirFrente,  LOW);
        analogWrite (dirTras,    velocidade);
        break;
      }
    case 0: {
        digitalWrite(dirFrente,  HIGH);
        digitalWrite(dirTras,    HIGH);
        break;
      }
    case 1: {
        analogWrite (dirFrente,  velocidade);
        digitalWrite(dirTras,    LOW);
        break;
      }
  }
}
