# Roteiro de Apresentação e Guia de Estudos: Rede Industrial DeviceNet

**Apresentador:** Bruno Saldanha Poltosi  
**Curso:** Eletrônica Industrial  
**Disciplina:** Redes de Comunicação  
**Tema:** Rede Industrial DeviceNet (Arquitetura, Especificações Elétricas, Protocolo CIP e Diagnóstico de Falhas)

---

## 1. Revisão e Síntese Técnica do Material das Fontes

* **Conceito Fundamental e Inversão de Prioridade:** Nas redes de TI corporativas (Níveis 4 e 5 da pirâmide de automação), a prioridade principal é a vazão de dados (*throughput*) e a transferência de arquivos pesados. No chão de fábrica (Níveis 1 e 2), a prioridade absoluta é o **determinismo**: mover pequenos pacotes de telemetria e comando no tempo exato, garantindo que o ciclo de varredura (*scan*) do CLP não atue sobre dados desatualizados.
* **Origem e Padronização ODVA:** Desenvolvida em 1994 pela Allen-Bradley/Rockwell Automation, a tecnologia teve suas especificações abertas e geridas globalmente pela **ODVA** (*Open DeviceNet Vendors Association*). O DeviceNet adota a camada física e a camada de enlace da norma **CAN (ISO 11898)** desenvolvida pela Bosch, combinada à camada de aplicação **CIP** (*Common Industrial Protocol*).
* **Atuação na Pirâmide de Automação:** Opera na interconexão entre o **Nível 1 (Campo)** — onde ficam sensores físicos, atuadores pneumáticos e ilhas de válvulas — e o **Nível 2 (Controle)**, onde operam os Controladores Lógicos Programáveis (CLPs) e IHMs.
* **Camada Física e Fiação de 5 Condutores:** O barramento utiliza um cabo blindado com 5 condutores com funções padronizadas:
  * **V+ (Vermelho):** Alimentação positiva (+24V DC).
  * **V- (Preto):** Referência de alimentação (0V DC / GND).
  * **CAN_H (Branco) e CAN_L (Azul):** Par trançado para transmissão diferencial de dados.
  * **Shield (Bare):** Malha de blindagem aterrada em um único ponto para drenar interferências eletromagnéticas.
  * **Tipos de Cabo:** Linha principal de tronco em cabo grosso (**Thick Cable**) e derivações secundárias em cabo fino e flexível (**Thin Cable**) de até 6 metros.
* **Sinalização Elétrica e Terminação de Barramento:** A transmissão de dados ocorre por sinalização diferencial sobre o par trançado CAN:
  * **Estado Dominante (Bit 0):** $V_{CANH} \approx 3,75\text{ V}$ e $V_{CANL} \approx 1,25\text{ V}$ (diferença de potencial $V_{diff} \approx 2,5\text{ V}$).
  * **Estado Recessivo (Bit 1):** Ambas as linhas permanecem em repouso próximo a $2,5\text{ V}$ ($V_{diff} \approx 0\text{ V}$).
  * **Terminação:** Para impedir a reflexão de ondas elétricas nas extremidades do cabo principal, é obrigatório aplicar **resistores de terminação de 121 Ω** (1/4 W) nas duas pontas da linha tronco.
* **Velocidades e Distâncias Máximas:** Opera em três taxas de transmissão padronizadas com limite físico rígido de extensão:
  * **125 kbps:** Alcance máximo de **500 metros**.
  * **250 kbps:** Alcance máximo de **250 metros**.
  * **500 kbps:** Alcance máximo de **100 metros**.
* **Capacidade de Nós e Endereçamento:** Suporta até **64 nós por segmento de rede**, identificados pelos endereços **MAC ID de 0 a 63**. O endereço 00 é tradicionalmente atribuído à placa Scanner do CLP e o endereço 63 é reservado para o comissionamento de novos nós de fábrica.
* **Alimentação em 24V DC e Queda de Tensão:** A fonte de 24V DC injetada no barramento deve garantir que a tensão no nó mais distante nunca caia abaixo de **11,0V DC** (limite operacional estabelecido pela ODVA), permitindo uma queda de tensão máxima de $\Delta V_{max} = 13,0\text{ V DC}$.
* **Análise de Falhas e Diagnósticos:** O DeviceNet substitui o cabeamento ponto a ponto tradicional (reduzindo até 70% dos cabos nas calhas) e elimina ruídos seriais antigos. Em caso de defeitos: a falta do resistor de 121 Ω gera reflexão e falhas intermitentes; a divergência de *baud rate* derruba o nó para o estado *Bus Off*; e testes com multímetro no barramento desligado devem indicar **60 Ω** entre CAN_H e CAN_L.

---

## 2. Roteiro de Apresentação Slide a Slide (Com Capa Oficial)

**Duração Estimada:** 12 a 14 minutos.

### [Slide 1: Capa do Trabalho]
* **Visual do Slide:** Título principal em destaque, identificação acadêmica oficial e diagrama conceitual da rede DeviceNet.
* **Texto no Slide:**
  * **Título:** Rede Industrial DeviceNet: Arquitetura, Especificações Elétricas e Diagnóstico de Falhas
  * **Apresentador:** Bruno Saldanha Poltosi
  * **Curso:** Eletrônica Industrial
  * **Disciplina:** Redes de Comunicação
* **Script de Fala (O que falar):**
  > "Olá a todos, meu nome é Bruno Saldanha Poltosi, sou aluno do curso de Eletrônica Industrial e apresento hoje o trabalho da disciplina de Redes de Comunicação sobre a **Rede Industrial DeviceNet**. O objetivo desta apresentação é detalhar a arquitetura, o funcionamento elétrico, a camada de aplicação CIP e as práticas de diagnóstico da rede DeviceNet. Veremos como essa tecnologia substitui quilômetros de fiação no chão de fábrica por um único cabo de 5 condutores que combina energia e dados com determinismo em tempo real."
* **Dica do Orador:** Mantenha postura firme e faça contato visual. Use a introdução para situar a importância da automação e da fiação simplificada.

---

### [Slide 2: Origem, Histórico e Padronização ODVA]
* **Visual do Slide:** Linha do tempo: Rockwell Automation (1994) $\rightarrow$ Gestão ODVA $\rightarrow$ Pilha CAN (Bosch) + CIP.
* **Script de Fala:**
  > "Desenvolvido originalmente pela Allen-Bradley/Rockwell Automation em 1994, o protocolo tornou-se um padrão aberto sob a gestão da **ODVA** (*Open DeviceNet Vendors Association*). Isso permite a interoperabilidade entre fabricantes: um CLP da Rockwell, uma ilha de válvulas da Festo e um inversor da Schneider conectam-se na mesma rede. O DeviceNet adota a camada física e de enlace do protocolo automotivo **CAN (ISO 11898)** e a camada de aplicação **CIP** (*Common Industrial Protocol*)."
* **Dica do Orador:** Destaque o papel da ODVA na garantia de interoperabilidade entre diferentes marcas.

---

### [Slide 3: Aplicação no Chão de Fábrica e Pirâmide de Automação]
* **Visual do Slide:** Diagrama da Pirâmide de Automação destacando os Níveis 1 (Campo) e 2 (Controle) com fotos de CLPs, inversores PowerFlex 525 e blocos de I/O Turck.
* **Script de Fala:**
  > "Na hierarquia industrial, o DeviceNet faz a integração direta entre o **Nível 1 (Campo)** — onde ficam sensores fotoelétricos, chaves de fim de curso e atuadores — e o **Nível 2 (Controle)**, onde opera o módulo Scanner do CLP. Em uma esteira transportadora, em vez de passar cabos individuais para cada motor e sensor, um único barramento DeviceNet conecta o inversor e os módulos de I/O, substituindo até 80% do cabeamento tradicional."
* **Dica do Orador:** Cite os componentes de campo (inversores, ilhas de válvulas) para demonstrar visão prática de instrumentação.

---

### [Slide 4: Detalhamento do Protocolo: Objetos CIP e Mensagens]
* **Visual do Slide:** Esquema do Modelo de Objetos CIP, mensagens Explícitas vs. Mensagens de I/O (COS - *Change of State*) e arquivo EDS (*Electronic Data Sheet*).
* **Script de Fala:**
  > "Na camada de aplicação, o protocolo CIP organiza os dados do dispositivo na forma de **Objetos**. Cada nó possui um arquivo **EDS**, que funciona como seu dicionário digital lido pelo CLP no comissionamento. A comunicação divide-se em **Mensagens Explícitas**, usadas para configuração e diagnósticos sem urgência, e **Mensagens de I/O**, usadas no controle em tempo real. Destaca-se o modo **Change of State (COS)**, onde o nó só transmite quando há alteração de estado no sensor, economizando banda no barramento."
* **Dica do Orador:** Explique como o modo COS evita tráfego desnecessário na rede.

---

### [Slide 5: Camada Física e Cabeamento de 5 Condutores]
* **Visual do Slide:** Corte transversal do cabo DeviceNet destacando os 5 condutores e conectores Mini-Change (Thick) e Micro-Change (Thin).
* **Script de Fala:**
  > "A infraestrutura física utiliza um **cabo blindado de 5 condutores**. O fio **Vermelho (V+)** e o **Preto (V-)** conduzem alimentação contínua em 24V DC para a eletrônica dos nós. O par trançado **Branco (CAN_H)** e **Azul (CAN_L)** realiza a transmissão diferencial de dados. O quinto condutor é o **Shield (Bare)**, aterrado em um único ponto contra interferências. A linha principal utiliza o cabo grosso (*Thick Cable*) e as derivações para os nós utilizam o cabo fino (*Thin Cable*) de até 6 metros."
* **Dica do Orador:** Enfatize que a rede entrega energia de 24V DC + dados no mesmo cabo.

---

### [Slide 6: Sinalização Elétrica Diferencial e Terminação]
* **Visual do Slide:** Formas de onda das tensões $V_{CANH}$ e $V_{CANL}$ ilustrando os estados Dominante e Recessivo e o esquema dos resistores de 121 Ω.
* **Script de Fala:**
  > "Para operar com alta imunidade a ruídos de motores elétricos, a rede emprega **sinalização diferencial CAN**. No **Bit 0 (Dominante)**, a linha CAN_H sobe para 3,75V e a CAN_L desce para 1,25V (diferença de 2,5V). No **Bit 1 (Recessivo)**, ambas permanecem em 2,5V (diferença nula). Ruídos eletromagnéticos afetam ambas as linhas de forma idêntica e se anulam na subtração matemática. Além disso, é obrigatório aplicar **resistores de terminação de 121 Ω** (1/4 W) nas duas extremidades do barramento tronco para impedir a reflexão do sinal elétrico."
* **Dica do Orador:** Mostre a importância dos resistores de 121 Ω para evitar falhas físicas na transmissão.

---

### [Slide 7: Taxas de Dados Suportadas e Determinismo]
* **Visual do Slide:** Tabela de velocidades (125 kbps, 250 kbps, 500 kbps) e diagrama do tempo do ciclo de varredura (*scan*) do CLP.
* **Script de Fala:**
  > "O DeviceNet opera nas taxas padronizadas de **125 kbps, 250 kbps e 500 kbps**. O foco das redes de campo não é transmitir arquivos pesados, mas assegurar o **determinismo**: garantir que o ciclo de varredura do CLP leia sensores e atue nos equipamentos em tempos previsíveis (geralmente abaixo de 5 ms) através da arbitragem por prioridade nativa do barramento CAN."
* **Dica do Orador:** Reforce que redes industriais buscam pontualidade e previsibilidade, não alto volume de megabytes.

---

### [Slide 8: Capacidade da Rede e Endereçamento de Nós]
* **Visual do Slide:** Diagrama de barramento com 64 nós numerados de 0 a 63 (Scanner no MAC ID 00 e novos nós no MAC ID 63).
* **Script de Fala:**
  > "Cada segmento suporta até **64 nós ou dispositivos conectados**, identificados por endereços **MAC ID de 0 a 63**. O endereço **MAC ID 00** é tradicionalmente reservado para a placa Scanner do CLP (ex: 1756-DNB). Os endereços de 01 a 62 são atribuídos aos sensores, válvulas e inversores, enquanto o **MAC ID 63** é a configuração padrão de fábrica para comissionar novos equipamentos via software."
* **Dica do Orador:** Mencionar o MAC ID 63 como padrão de comissionamento mostra experiência prática.

---

### [Slide 9: Distâncias Máximas vs. Velocidade e Alimentação 24V DC]
* **Visual do Slide:** Gráfico comparativo de velocidade vs. comprimento do cabo tronco e tabela de limites de tensão em 24V DC.
* **Script de Fala:**
  > "A distância máxima do barramento principal diminui com o aumento da velocidade: em **125 kbps** atinge **500 metros**; em **250 kbps** atinge **250 metros**; e em **500 kbps** fica limitada a **100 metros**. Em relação à energia de 24V DC, a norma da ODVA exige que a tensão no nó mais distante nunca caia abaixo de **11,0V DC**, garantindo o funcionamento contínuo de toda a eletrônica de campo."
* **Dica do Orador:** Relacione os cenários de aplicação (silo de grãos de 500m vs envasadora de 100m).

---

### [Slide 10: Análise de Falhas e Diagnósticos de Campo]
* **Visual do Slide:** Painel de teste: Multímetro medindo 60 Ω no barramento e display digital indicando os códigos de erro 78 e 91.
* **Script de Fala:**
  > "O DeviceNet corrige as falhas clássicas do cabeamento antigo, eliminando a bagunça de fios e a vulnerabilidade das conexões seriais comuns. Quanto a falhas próprias da rede: a ausência do resistor de 121 Ω causa reflexão de sinal e erros intermitentes; a inserção de um nó com baud rate incorreto gera quadros de erro e derruba a porta para *Bus Off*. Para diagnosticar, medimos **60 Ω** entre CAN_H e CAN_L no cabo desalimentado e consultamos os códigos no scanner, como o **Code 78 (Node Missing)**."
* **Dica do Orador:** Explique o teste dos 60 Ω como uma verificação prática indispensável em campo.

---

### [Slide 11: Conclusão e Resumo Técnico (ROI)]
* **Visual do Slide:** Síntese de benefícios: Redução de 70% de fiação, redução do tempo de reparo (MTTR) e confiabilidade determinística.
* **Script de Fala:**
  > "Em suma, a rede DeviceNet une a robustez física do CAN à estrutura do protocolo CIP. Ela reduz até **70% do volume de cabos nas calhas** e corta pela metade o tempo médio de reparo (MTTR) graças ao diagnóstico preciso dos nós. É a solução perfeita entre economia de infraestrutura e controle em tempo real."
* **Dica do Orador:** Encerre reforçando os três pilares: Economia, Confiabilidade e Diagnóstico Rápido.

---

### [Slide 12: Referências Bibliográficas]
* **Visual do Slide:** Relação das normas ISO 11898, especificações ODVA, material de aula e manuais Rockwell Automation.
* **Script de Fala:**
  > "Estas são as referências que fundamentam nosso trabalho: a norma ISO 11898 do protocolo CAN, as especificações oficiais da ODVA, o material didático de Redes Industriais da Aula 9 e os manuais técnicos da Rockwell Automation. Muito obrigado a todos e estou aberto a perguntas!"
* **Dica do Orador:** Permaneça à disposição da banca com segurança.

---

## 3. Guia de Estudo e Embasamento Técnico para Defesa

### Fórmulas e Parâmetros Chave
1. **Resistência Equivalente de Terminação:**
   $$R_{eq} = \frac{121\ \Omega \times 121\ \Omega}{121\ \Omega + 121\ \Omega} \approx \mathbf{60\ \Omega}$$
   *(Medido com multímetro entre o fio Branco CAN_H e o fio Azul CAN_L com a rede desligada).*
2. **Queda de Tensão Máxima Permitida (ODVA):**
   $$\Delta V_{max} = V_{fonte} - V_{min\_no} = 24,0\text{ V} - 11,0\text{ V} = \mathbf{13,0\text{ V DC}}$$
   Fórmula da queda de tensão em loop:
   $$\Delta V = I_{total} \times R_{loop} \times L_{efetivo}$$
   *(Onde $R_{loop\_thick} \approx 0,029\ \Omega/\text{m}$).*

---

### Guia de Respostas Rápidas para a Banca (Defesa Exclusiva)

* **Pergunta 1: *"Por que o DeviceNet opera em velocidades de até 500 kbps enquanto a Ethernet opera em Gigabits?"***
  * **Sua Resposta:** *"No chão de fábrica (Níveis 1 e 2), a prioridade não é enviar arquivos grandes, mas sim o determinismo e a imunidade a ruídos. Os pacotes de I/O têm poucos bytes e o limite de 500 kbps é uma exigência física da arbitragem bit a bit do barramento CAN para garantir que paradas e leituras cheguem em menos de 5 ms sem tolerar colisões."*
* **Pergunta 2: *"Qual a função do resistor de 121 Ω no barramento DeviceNet?"***
  * **Sua Resposta:** *"Os resistores de 121 Ω (1/4 W) aplicados nas duas extremidades do cabo principal casam a impedância da linha e eliminam a reflexão das ondas elétricas de alta frequência. Sem eles, o sinal reflete nas pontas abertas e corrompe as mensagens do CLP."*
* **Pergunta 3: *"Por que escolher a tecnologia DeviceNet na camada de campo?"***
  * **Sua Resposta:** *"O diferencial do DeviceNet é entregar alimentação de 24V DC e dados diferenciais no mesmo cabo de 5 condutores com gestão aberta da ODVA. Ele substitui centenas de ligações ponto a ponto por um único barramento inteligente, reduzindo até 70% do cabeamento e permitindo diagnósticos centralizados no CLP."*
* **Pergunta 4: *"Qual a diferença entre a camada física CAN e a camada de aplicação CIP?"***
  * **Sua Resposta:** *"O CAN (norma ISO 11898) define o meio físico, os níveis elétricos diferenciais e a camada de enlace de dados. O CIP (Common Industrial Protocol) opera na camada de aplicação, organizando os dados em objetos padronizados (como o arquivo EDS). Essa separação garante que equipamentos de fabricantes diferentes se entendam nativamente na mesma rede."*

---
*Documento exportado e publicado no Studio.*
