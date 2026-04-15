# Tacômetro Óptico com ESP32

## Descrição

Este projeto consiste no desenvolvimento de um tacômetro óptico utilizando o microcontrolador ESP32, capaz de medir a velocidade de rotação de eixos sem contato físico. O sistema detecta a passagem de marcas refletivas através de um sensor óptico, convertendo pulsos luminosos em dados de rotação (RPM).

A solução foi projetada com foco em aplicações experimentais e laboratoriais, oferecendo uma alternativa de baixo custo, confiável e de fácil implementação.

---

## Funcionamento

O princípio de operação baseia-se na detecção de pulsos gerados por um sensor óptico (infravermelho ou fototransistor). A cada passagem de uma marca refletiva no eixo, um pulso é gerado e contabilizado pelo ESP32.

A partir da frequência desses pulsos, o sistema calcula a velocidade angular utilizando a relação:

* RPM = (Número de pulsos por minuto) / (número de marcas no eixo)

O processamento é realizado em tempo real, permitindo leituras rápidas e precisas.

---

## Componentes Utilizados

* ESP32
* Sensor óptico (IR ou fototransistor)
* Resistores e componentes passivos
* Display de 7 segmentos (ou interface de saída equivalente)
* Fonte de alimentação

---

## Tecnologias

* Linguagem C
* Programação embarcada
* Temporizadores e interrupções
* Leitura de sinais digitais

---

## Aplicações

* Medição de velocidade de motores
* Bancadas de testes experimentais
* Projetos acadêmicos
* Sistemas de controle e automação

---

## 📷 Imagens do Projeto

![Tacômetro](imagens/IMG_20251213_124025.jpg)

---

## Possíveis Melhorias

* Implementação de filtro digital para redução de ruído
* Comunicação via Bluetooth ou Wi-Fi
* Interface gráfica para visualização dos dados
* Armazenamento de medições

---

## Licença

Este projeto é de uso acadêmico e pode ser utilizado livremente para fins educacionais.

