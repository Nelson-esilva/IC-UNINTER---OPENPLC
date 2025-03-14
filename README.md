<img src="/img/top_openplc.png" width="100%">

# Índice 
 
* [Introdução](#Introdução)
* [A IEC 6113](#A-IEC-6113)
* [OpenPLC](#OpenPLC)
* [OpenPLC Editor](#OpenPLC-Editor)
* [OpenPLC Runtime](#OpenPLC-Runtime)
* [Plataformas de Hardware para o OpenPLC](#Plataformas-de-Hardware-para-o-OpenPLC)
* [Endereçamento de Entrada, Saída e Memória](#Endereçamento-de-Entrada-Saída-e-Memória)
* [Endereçamento Físico](#Endereçamento-Físico)
* [Instalando o OpenPLC em um Raspberry](#Instalando-o-OpenPLC-em-um-Raspberry)
* [Biblioteca WiringPi](#Biblioteca-WiringPi)
* [Criando o primeiro projeto no OpenPLC Editor](https://github.com/Epaminondaslage/OpenPLC/tree/master/primeirodiagrama)
* [Carregando programas para o OpenPLC Runtime](https://github.com/Epaminondaslage/OpenPLC/blob/master/primeirodiagrama/README.md)
* [Status do Projeto](#Status-do-Projeto)
* [Referências](#Referências)

# Introdução 

Um controlador monitora o estado real do processo de uma planta através de um número de transdutores, definido de acordo com a aplicação. Os transdutores convertem as grandezas físicas em sinais normalmente elétricos, os quais são conectados com as entradas dos controladores. Transdutores digitais (discretos) medem variáveis com estados distintos, tais como ligado/desligado ou alto/baixo, enquanto os transdutores analógicos medem variáveis com uma faixa contínua, tais como pressão, temperatura, vazão ou nível.

<table border="0">
<tbody>
<tr>
<td style="width: 50%;"><img src="/img/sistemas industriais.png" width="50%" /></td>
</tr>
<tr>
<td style="text-align: center;">Figura 1 - Sistemas Industriais.</td>
</tr>
</tbody>
</table>

Com base nos estados das suas entradas (variáveis de processo - PV), o controlador utiliza um algoritmo de controle embutido para calcular os estados das suas saídas (variáveis manipuladas - MV). Os sinais elétricos das saídas são convertidos para o processo através dos atuadores. Muitos atuadores geram movimentos como válvulas, motores, bombas e outros e utilizam a energia potencial pneumática para o acionamento.

O operador interage com o controlador através dos parâmetros de controle (ex: Set Point, Kp, Ki, Kd). Alguns controladores podem mostrar os estados do processo através de um display ou tela, que é chamado de IHM (Interface Homem Máquina).

Atualmente, os Controladores Programáveis aplicam-se tanto ao controle discreto (I/O) quanto para o controle de malhas analógicas. Diferentes computadores são conectados via rede local (LAN), redes de longas distâncias (WAN) a um computador supervisório central, o qual gerencia os alarmes, receitas e relatórios.

## Motivação para Sistemas Abertos

Os PLC's são um dos componentes mais críticos da indústria atual. Com a utilização dos sistemas de controle na maioria das indústrias, incluindo aplicações que exigem segurança, é muito importante que os programas possam ser facilmente entendidos por uma grande parte dos profissionais do chão de fábrica. Além do programador, o programa de controle deve ser de fácil entendimento para todos os técnicos, engenheiros e gerentes de processo.

Por décadas o mercado tem sido dominado por poucos fabricantes que oferecem soluções muito parecidas, porém com particularidades nos dialetos de programação. Muitos usuários têm decidido eleger no mínimo três fornecedores, com o objetivo principal de minimizar o risco. Em aplicações reais, isto implica em um maior custo devido ao retrabalho e problemas de comunicação entre produtos de diferentes fabricantes.

# A IEC 6113
  
A International Electrotechnical Commission (Comissão Eletrotécnica Internacional), normalmente conhecida como IEC, é o organismo de normalização internacional não lucrativo independente líder mundial para as tecnologias elétrica, eletrónica e relacionadas. A IEC 61131 traz requisitos de hardware e software para sistemas que envolvam CLPs é dividida em cinco partes:
  
* Parte 1: IEC 61131-1 Informações gerais. Definição da informação geral, da terminologia básica e dos conceitos; Publicado em 1992 está na Versão 2.0 desde 2003.;
* Parte 2: IEC 61131-2 Requisitos de hardware . Exigências de equipamento e testes eletrônicos e testes mecânicos de construção e verificação; Publicado em 1992 encontra-se na Versão 4.0 desde 2017.;
* Parte 3: IEC 61131-3 Linguagens de programação. Estrutura do Software do CLP, execução do programa e linguagens de programação; Publicado em 1993 está na Versão 3.0 desde 2013.;
* Parte 4: IEC 61131-4 Guia de orientação ao usuário. Guia de orientação ao usuário na seleção, instalação e manutenção de CLP's. Publicado em 1995 está na Versão 2.0 from 2004.;
* Parte 5: IEC 61131-5 Comunicação.Facilidade do Software em especificação de mensagens de serviços a comunicar-se com outros dispositivos usando as comunicações baseadas em MAP (Manufacturing Messaging Services). Publicado em 1998 apresenta-se naVersão 1.0 desde 2000.;
* Parte 6: IEC 61131-6 Segurança Funcional.Comunicação via facilidade do Software fieldbus para comunicação de PLC s utilizando IEC fieldbus. Edição 1.0 - 2012.;
* Parte 7: IEC 61131-7 Programação de Controle Fuzzy  Programação utilizando Lógica Nebulosa (Fuzzy).Edição 1.0 - 2000;
* Parte 8: IEC 61131-8 Guia para implementação das linguagens: Diretrizes para aplicação e implementação de linguagens de programação. Edição 3.0 - 2017.
* Parte 9: IEC 61131-9: Interface de comunicação digital single-drop para pequenos sensores e atuadores (SDCI).Especifica uma tecnologia de interface de comunicação digital single-drop para pequenos sensores e atuadores SDCI.A edição atual é 1.0 de 2013.
* Parte 10: IEC 61131-10: PLC aberto XML Exchange Format.Este novo padrão IEC é baseado na especificação original PLCopen XML. Com o lançamento da 3ª edição da IEC 61131-3 em 2013, uma grande reformulação foi necessária para incluir as mudanças e extensões como recursos orientados a objetos. Lançado em abril de 2019.
  
## IEC 61131-3 – Linguagens de Programação
  
Foco de nosso objeto de trabalho, a norma IEC 61131 em sua parte 3, tem por objetivo:
  
Fornecer metodologias de construção de lógicas de programação de forma estruturada e modular, permitindo a quebra dos programas em partes gerenciáveis;

Definir 5 linguagens de programação, cada uma com suas características, de forma a cobrir a maioria das necessidades de controle atuais;
Permite o uso de outras linguagens de programação, desde que obedecidas as mesmas formas de chamadas e trocas de dados (Visual Basic, Flow Chart, C++, etc) e possui uma abordagem e estruturação top-down e bottom-up, fundamentada em 3 princípios:

* Modularização;
* Estruturação;
* Reutilização.

Dentro destes aspectos, a IEC 61131-3 define cinco linguagens de programação:

* ST (Structured Text) Texto Estruturado
* IL (Instruction List) Lista de Instruções
* LD (Ladder) Linguagem ladder
* FBD (Function Block Diagram) Diagrama de bloco
* SFC (Sequential Flow Chart) Diagrama de Fluxo

As duas primeiras linguagens acima (ST e IL) são ditas textuais por conterem instruções na forma de texto. As duas seguintes (LD e FBD) são ditas gráficas por possuírem representação na forma de símbolos. A linguagem SFC é normalmente tida como linguagem gráfica, porém também permite programações textuais.

É comum em alguns ambientes de programação que atendem à IEC 61131-3 como o CODESYS, a presença de uma sexta linguagem de programação, conhecida como CFC (do inglês Continuous Function Chart) que não faz parte das definições da norma. O software de programação  compatíveis IEC 61131-3 permite que os usuários criem programas em um ambiente padrão global compatível com IEC. Projetos podem ser criados com uma variedade de linguagens de programação em qualquer combinação.

* Ambiente de programação aberto e flexível com portabilidade de código.
* Versões Express (gratuita) e Pro (paga) disponíveis.
* Biblioteca de modelos de programação para aplicativos de processo comuns, reduzindo o tempo de lançamento no mercado
* A biblioteca de blocos de funções para dispositivos e instrumentos MKS permite "plug & play"

# OpenPLC 

O projeto do OpenPLC  é um ambiente de desenvolvimento de programas, é compatível com praticamente qualquer software SCADA existente, utiliza o protocolo Modbus/TCP para comunicação e inclui em seu Editor uma Interface de usuário intuitiva e fácil de de utilizar. Outro ponto interessante a se destacar condiz a compatibilidade do OpenPLC Editor, sendo esse um software que permite escrever programas para CLP de acordo com a IEC 61131-3, estando em conformidade com o PLCopen XML (https://beremiz.org/doc). 

O Projeto OpenPLC consiste em duas partes: Runtime e Editor. O Runtime é um software portátil projetado para rodar desde o menor de todos os microcontroladores (compatível com Arduino) até poderosos servidores nas nuvens. Ele é responsável por executar os programas PLC que você cria usando o Editor.

 A programação do hardware é realizada por meio do Editor, onde são gerados arquivos ST. O aplicativo OpenPLC possui um servidor Web baseado em NodeJs (https://nodejs.org/en/) que controla se o OpenPLC está de fato sendo executado ou não, e permite que o usuário faça upload do arquivo ST. Durante a execução do servidor, basta abrir o navegador, que haverá uma interface Web, possibilitando o envio de novos programas ao OpenPLC.
 
 ## Porque usar este software? 
  
Por ser uma ferramenta totalmente aberta, o OpenPLC possibilita que qualquer pessoa tenha acesso a todos os arquivos e informações relativas ao projeto, o que resulta em uma colaboração significativa para disseminação de conhecimentos voltados principalmente para aplicações industriais que utilizam CLPs. Se comparado a um CLP tradicional, o OpenPLC  apresenta componentes relativamente baratos, o que abre muitas portas dentro do cenário de automação.

# OpenPLC Editor

O OpenPLC Editor, projeto criado por Thiago Rodrigues Alves (estudante de doutorado na Universidade do Alabama), surgiu através do objetivo de encontrar vulnerabilidade em PLCs (Programmable Logic Controller ou Controlador Lógico Programável - CLP). Entretanto, dificilmente algum fabricante de CLP disponibilizaria seu código fonte para que o estudante pudesse realizar uma análise mais profunda, a fim de validar seus estudos. Devido a isto, ele resolveu criar o seu próprio CLP de hardware e software livres, que pode ser programado nas 5 principais linguagens definidas conforme a norma IEC 61131-3 (https://pt.wikipedia.org/wiki/IEC_61131-3), que estabelece a arquitetura básica de software e as linguagens de programação para CLPs. As linguagens suportadas pelo OpenPLC Editor estão apresentadas na figura 02.

<table border="0">
<tbody>
<tr>
<td style="width: 50%;"><img src="./img/ling_plc.jpg" width="50%" /></td>
</tr>
<tr>
<td style="text-align: center;">Figura 02 - Linguagens de programação disponibilizadas no OpenPLC Editor.</td>
</tr>
</tbody>
</table>

* Diagrama Ladder (LD), Gráfica.
* Diagrama de Blocos (FBD), Gráfica.
* Texto Estruturado (ST), textual.
* Lista de Instruções (IL), textual.
* Diagrama de Funções Sequenciais (SFC).
  
A figura 3 ilustra a linguagem Ladder sendo aplicada sobre o OpenPLC Editor. 

<table border="0">
<tbody>
<tr>
<td style="width: 50%;"><img src="./img/Figura_1.png" width="50%" /></td>
</tr>
<tr>
<td style="text-align: center;">Figura 03 - Linguagem Ladder sendo aplicada sobre o OpenPLC Editor.</td>
</tr>
</tbody>
</table>

Em resumo, o Editor OpenPLC é um editor PLC compatível com IEC 61131-3 totalmente gratuito e de código aberto. Você pode usá-lo para fazer upload de código PLC diretamente para qualquer placa ou sistema executando OpenPLC Runtime.

# OpenPLC Runtime

O OpenPLC Runtime permite executar programas PLC criados no Editor OpenPLC. Esta runtime, instalada em um SBC possui um servidor web integrado que permite configurar vários parâmetros da runtime. Microimplementações do OpenPLC Runtime (ou seja, versões da runtime que vão em microcontroladores e placas Arduino) não possuem o servidor web embutido. Em vez disso, todas as configurações de tempo de execução para o micro runtime são feitas diretamente da caixa de diálogo de upload do OpenPLC Editor.

O servidor web runtime n SBC está disponível em seu endereço IP de destino na porta 8080. Por exemplo, se você instalou o OpenPLC Runtime em Raspberry Pi e seu endereço IP é 172.16.10.143, então você pode acessar o OpenPLC Runtime abrindo seu navegador web e apontando para http://172.16.10.143:8080.

Se você estiver recebendo erros de página, certifique-se de que seu computador possa acessar o Raspberry Pi em sua rede. Se você não sabe o endereço IP da sua placa, utilize uma ferramenta pas escanear sua rede e descobrir o IP dele.

Depois de acessar o servidor web OpenPLC, você deverá ver na janela do seu navegador uma página de login como a apresentada na figura 04.


<table border="0">
<tbody>
<tr>
<td style="width: 50%;"><img src="./img/diag11.png" width="50%" /></td>
</tr>
<tr>
<td style="text-align: center;">Figura 04 - Tela inicial da Runtime do OpenPLC Editor.</td>
</tr>
</tbody>
</table>

O nome de usuário e senha padrão é openplc (login) e openplc (senha). Isso significa que a primeira coisa que você deve fazer após o login pela primeira vez é alterar o nome de usuário e a senha padrão! É muito fácil fazer isso. Basta ir ao menu Users à esquerda e clicar no OpenPLC User para alterar as informações do usuário como desejar.
  
  * Endereço IP do Rasperry Pi que contém a runtime
  * http://172.16.10.143:8080 (por exemplo)
  * user=openplc
  * passwd=openplc

## Habilitando o acesso de E/S de hardware

Por padrão, o tempo de execução do OpenPLC é instalado com um driver em branco. Isso significa que ele não poderá controlar seus pinos GPIO de hardware imediatamente com o OpenPLC. Primeiro, você terá que habilitar o driver de hardware correto para sua plataforma. No menu à esquerda, clique em “Hardware” e escolha o driver apropriado no menu pop-up. Certifique-se de escolher o driver correto para sua placa, caso contrário, o OpenPLC Runtime falhará ao compilar o núcleo de tempo de execução.

<table border="0">
<tbody>
<tr>
<td style="width: 50%;"><img src="./img/diag12.png" width="50%" /></td>
</tr>
<tr>
<td style="text-align: center;">Figura 05 - Escolha do hardware que está instalado o OpenPLC Runtime.</td>
</tr>
</tbody>
</table>

# Plataformas de Hardware para o OpenPLC

O OpenPLC run time é compativel com algumas plataformas livres, como Arduino, Raspberry Pi e ESP8266. É oficialmente suportada nas seguintes plataformas:
  
* Arduino Uno / Nano / Leonardo / Micro
* Arduino Mega / Due
* Arduino Nano Every / IoT / BLE
* Arduino RB2040 Connect
* Arduino Mkr / Zero / WiFi
* Arduino Pro (Machine Control and EDGE)
* Controllino Maxi / Automation / Mega / Mini
* Productivity Open P1AM
* ESP8266 (nodemcu)
* ESP32
* Raspberry Pi 2 / 3 / 4
* PiXtend
* UniPi Industrial Platform
* Neuron PLC
* FreeWave Zumlink
* FreeWave ZumIQ
* Windows (generic target as a soft-PLC)
* Linux (generic target as a soft-PLC)

# Endereçamento de Entrada, Saída e Memória

As aplicações PLC interagem com o mundo externo através de módulos de entrada e saída e/ou protocolos de comunicação SCADA. Ao projetar suas aplicações de CLP, você decide quais variáveis devem ser conectadas aos módulos de E/S e comunicação, rotulando a variável com um endereço de CLP.

O OpenPLC Runtime usa a nomenclatura IEC 61131-3 para endereçar as localizações de entrada, saída e memória. O endereçamento das localizações de E/S é feito através do uso de sequências de caracteres especiais. Essas sequências são uma concatenação do sinal de porcentagem “%”, um prefixo de localização, um prefixo de tamanho e um ou mais números naturais separados por espaços em branco. Os seguintes prefixos de local são suportados:

   * I para entrada
   * O para saída
   * M para memória

Os seguintes prefixos de tamanho são suportados:

   * X para bit (1 bit)
   * B para byte (8 bits)
   * W para palavra (16 bits)
   * D para palavra dupla (32 bits)
   * L para palavra longa (64 bits)

Por exemplo, se você deseja ler o estado da primeira entrada digital em uma variável BOOL, deve declarar sua variável localizada em: %IX0.0. Se você quiser escrever o conteúdo de uma variável UINT na segunda saída analógica, você deve declarar sua variável UINT localizada em %QW2.

Nota: O mapeamento de PLC para E/S física depende da plataforma. Para mais informações sobre mapeamento de E/S do CLP para cada plataforma suportada.

<table border="0">
<tbody>
<tr>
<td style="width: 50%;"><img src="./img/endereçamento IEC 61131-3.png" width="30%" /></td>
</tr>
<tr>
<td style="text-align: center;">Figura 06 - Endereçamento IEC 61131-3.</td>
</tr>
</tbody>
</table>

Como você deve ter notado, os endereços PLC bit (X) possuem um endereço hierárquico de duas partes. A parte menos significativa (mais à direita) pode ser interpretada como uma posição em um byte e deve estar no intervalo de 0 a 7. A parte mais significativa (mais à esquerda) não deve ser maior que 1023. As partes são separadas por um único período. Tamanhos de dados diferentes de X têm um endereço hierárquico de uma parte. Eles não devem conter um ponto (.) e não devem ser maiores que o endereço máximo de localização de memória para sua plataforma.

Os seguintes são exemplos inválidos de endereços PLC no OpenPLC pelo motivo declarado:

   * %IX0.8 O índice menos significativo é maior que 7.
   * %QX0.0.1 A hierarquia de três partes não é um endereço permitido.
   * %IB1.1 Hierarquia de duas partes só é permitida para tamanho de dados X

# Endereçamento Físico

O OpenPLC Runtime é compatível com várias plataformas de hardware diferentes com diferentes configurações de módulos de E/S. Internamente, todas as variáveis de E/S estão associadas a um Endereço do PLC, conforme explicado em Endereçamento de Entradas, Saídas e Memória. A camada de hardware é o componente responsável por traduzir as variáveis de endereço interno do PLC em localizações físicas do hardware. Cada plataforma suportada pelo OpenPLC deve ter uma camada de hardware diferente. A tabela apresentada na figura 07 apresenta a equivalência dos pinos entre o Raspberry 3B e sua equivalência no OpenPLC.   

<table border="0">
<tbody>
<tr>
<td style="width: 50%;"><img src="https://github.com/Epaminondaslage/Automacao-industrial-e-residencial-Ecossistema-didatico/blob/main/img/GPIO_HEADER.png" width="80%" /></td>
</tr>
<tr>
<td style="text-align: center;">Figura 07 - Disposição de Pinos no RaspberryPi 3B e OpenPLC.</td>
</tr>
</tbody>
</table>


## Placas baseadas em microcontroladores

<table border="0">
<tbody>
<tr>
<td style="width: 50%;"><img src="./img/diagarduinomegaedue.png" width="50%" /></td>
</tr>
<tr>
<td style="text-align: center;">Figura 08 - Endereçamento físico para Arduino Mega e Due.</td>
</tr>
</tbody>
</table>


<table border="0">
<tbody>
<tr>
<td style="width: 50%;"><img src="./img/diagarduinouno.png" width="50%" /></td>
</tr>
<tr>
<td style="text-align: center;">Figura 09 - Endereçamento físico para Arduino Uno, Leonard, Nano, Micro e Zero.</td>
</tr>
</tbody>
</table>


<table border="0">
<tbody>
<tr>
<td style="width: 50%;"><img src="./img/diagesp32.png" width="50%" /></td>
</tr>
<tr>
<td style="text-align: center;">Figura 10 - Endereçamento físico para ESP32.</td>
</tr>
</tbody>
</table>


<table border="0">
<tbody>
<tr>
<td style="width: 50%;"><img src="./img/diagesp8266.png" width="50%" /></td>
</tr>
<tr>
<td style="text-align: center;">Figura 11 - Endereçamento físico para ESP8266.</td>
</tr>
</tbody>
</table>

<table border="0">
<tbody>
<tr>
<td style="width: 50%;"><img src="./img/diagraspberrypi.png" width="50%" /></td>
</tr>
<tr>
<td style="text-align: center;">Figura 12 - Endereçamento físico para Raspberry Pi.</td>
</tr>
</tbody>
</table>

# Instalando o OpenPLC em um Raspberry

Instalando OpenPLC Runtime no Linux

Instruções passo a passo sobre como instalar o OpenPLC Runtime em seu sistema. O OpenPLC Runtime pode rodar em uma variedade de sistemas Linux, mas funciona melhor em distribuições baseadas em Debian como Ubuntu e Raspbian.

A melhor maneira de obter o OpenPLC Runtime em seu dispositivo é usando git. Normalmente, o git vem pré-instalado na maioria das distribuições Linux. Se por algum motivo você não tiver o git instalado em seu sistema, você pode instalá-lo abrindo o terminal e digitando:

  sudo apt-get install git

Para instalar o OpenPLC, digite estas linhas no terminal:

  git clone https://github.com/thiagoralves/OpenPLC_v3.git
  CD OpenPLC_v3
  ./install.sh linux


Se você estiver instalando o OpenPLC em um hardware Linux específico, como o Raspberry Pi por exemplo, você deve substituir o parâmetro ‘linux’ pelo argumento específico da sua plataforma:

  ./install.sh rpi

Abaixo estão os argumentos válidos para o instalador:

* win – Instalar OpenPLC no Windows sobre Cygwin
* linux – Instalar OpenPLC em uma distribuição Linux baseada em Debian
* docker – Instalar o OpenPLC em um container Docker
* rpi – Instalar o OpenPLC em um Raspberry Pi
* neuron – Instalar o OpenPLC em um UniPi Neuron PLC
* personalizado – Ignora toda a instalação de pacotes específicos e tenta instalar o OpenPLC assumindo que seu sistema já possui todas as dependências atendidas
 
O processo de instalação levará algum tempo (até 1 hora, dependendo do seu sistema). Depois que o OpenPLC estiver instalado, basta reiniciar o dispositivo e ele iniciará automaticamente após a inicialização.

# Biblioteca WiringPi

Você também deve ter a biblioteca WiringPi instalada para poder executar o OpenPLC. WiringPi é responsável por controlar os pinos Raspberry Pi GPIO. Sem ele, você não conseguirá ativar o driver Raspberry Pi e o OpenPLC não conseguirá controlar os pinos da placa. 

A biblioteca WiringPi é uma biblioteca de software escrita em C para controlar os pinos GPIO (General Purpose Input/Output) em placas Raspberry Pi. Ela foi desenvolvida por Gordon Henderson e fornece uma interface fácil de usar para interagir com os pinos GPIO da Raspberry Pi usando a linguagem de programação C.

Com a biblioteca WiringPi, você pode controlar os pinos GPIO de várias maneiras, como definir o modo dos pinos como entrada ou saída, ler o estado dos pinos de entrada e escrever dados em pinos de saída. Ela também suporta temporizadores (delays) e PWM (Pulse Width Modulation) para controlar a intensidade de dispositivos como LEDs e motores.

Além de C, a biblioteca WiringPi também possui interfaces para outras linguagens de programação, incluindo Python, Perl, Ruby e Java. Isso permite que desenvolvedores de diferentes backgrounds utilizem a biblioteca em seus projetos, independentemente da linguagem escolhida.

Para utilizar a biblioteca WiringPi, você precisa instalá-la no seu Raspberry Pi. Ela geralmente vem pré-instalada em distribuições Raspberry Pi OS mais recentes, mas você pode instalá-la manualmente caso necessário.

Para instalar o WiringPi, obtenha a versão .deb mais recente em:

https://github.com/WiringPi/WiringPi/releases/

O arquivo -armhf.deb deve ser usado em sistemas operacionais de 32 bits (Raspberry Pi 3 e inferior) e o arquivo -arm64.deb é destinado a sistemas operacionais de 64 bits (Raspberry Pi 4 e superior). Baixe o arquivo apropriado para sua arquitetura em seu Raspberry Pi e instale-o com o comando dpkg:

  dpkg -i wirepi-[versão]-armhf.deb
ou
  dpkg -i wirepi-[versão]-arm64.deb

Teste se a instalação do WiringPi foi concluída com sucesso com o comando:

  gpio -v


# Referências

* FREITAS, C. M. Conheça o OpenPLC - O primeiro CLP de Código Aberto Padronizado. Disponível em: https://www.embarcados.com.br/openplc-o-primeiro-clp-de-codigo-aberto/. Acesso em Março de 2019.
* ALVES, T.R. What is OpenPLC?. OpenPLC Project, 2017. Disponível em: http://www.openplcproject.com/. Acesso em novembro de 2017.
* ALVES, T.R.; BURATTO, M.; SOUZA, F.M.; RODRIGUES, T, V. OpenPLC: An Open Source Alternative to Automation. In IEEE 2014 Global Humanitarian Technology Conference (GHTC 2014), San Jose, CA, 2014, pp. 585-589.
* JOHN, K.H.; TIEGELKAMP, M. IEC 61131-3: Programming Industrial Automation Systems,” 2nd ed. Springer, 2010 pp.147-168.
* "Goals and Benefits of the IEC 61131 Standard (PLCs – Programmable Logic Controllers)." crushtymks.com, 2010. Disponível em: https://crushtymks.com/pt/industrial-automation/1102-goals-and-benefits-of-the-iec-61131-standard-plcs-8211-programmable-logic-controllers.html. Acesso em Março/2022.
* "Use Cases." beremiz.org, [2021]. Disponível em: https://beremiz.org/usecases. Acesso em: Março 2021.
* GUIMARÃES, Hugo Casati Ferreira. Norma IEC 61131-3 para Programação de Controladores Programáveis: Estudo e Aplicação. Projeto de Graduação - Universidade Federal do Espírito Santo, Vitória ES, Setembro de 2005.
 * EASWARAN, E. V. et al. Programmable Logic Controller: Open Source Hardware and Software for Massive Training. In: IECON 2018 - 44th Annual Conference of the IEEE Industrial Electronics Society, 2018, pp. 2422-2427, doi: 10.1109/IECON.2018.8592772.
* SCADA Software. Inductive Automation, [ano de publicação]. Disponível em: https://inductiveautomation.com/scada-software/?gclid=Cj0KCQjwxIOXBhCrARIsAL1QFCbenQ37JpOXbF0VmaAw2WL0hdSYVrTHSdwi_yRkBgwWPkQzpkb-aH0aAhIREALw_wcB. Acesso em Fevereiro 2022.
* Site do Projeto openplc. OpenPLC Project, [2021]. Disponível em: https://openplcproject.com/. Acesso em: Dezembro 2020.
* Projeto OpenPLC Runtime no Github. Thiago R. Alves, [2021]. Disponível em: https://github.com/thiagoralves/OpenPLC_v3. Acesso em Dezembro 2020. 
* Projeto OpenPLC Editor no Github. Thiago R. Alves, [ano de publicação]. Disponível em: https://github.com/thiagoralves/OpenPLC_Editor. Acesso em Abril 2022  
* Site do PLC Open. PLC Open. Disponível em: https://plcopen.org/. Acesso em Abril 2021.
* Site do Editor Beremiz. Beremiz. Disponível em: https://beremiz.org/. Acesso em Abril de 2021.
* Site do OpenPLC Runtime. OpenPLC Project. Disponível em: https://openplcproject.com/docs/2-1-openplc-runtime-overview. Acesso em Abril de 2021.
* Instalação do runtime do OpenPLC. OpenPLC Project. Disponível em: https://openplcproject.com/docs/installing-openplc-runtime-on-linux-systems/. Acesso em Abril de 2021.

# Status do Projeto

![Badge em Desenvolvimento](http://img.shields.io/static/v1?label=STATUS&message=EM%20DESENVOLVIMENTO&color=GREEN&style=for-the-badge)
