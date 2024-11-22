# Instalação ROS 2 Jazzy

## Sistema Operacional utilizado
Windows 11

## Instalação seguindo o tutorial da [documentação do ROS 2](https://docs.ros.org/en/foxy/Installation/Windows-Install-Binary.html)

## Instalando pré-requisitos

### Chocolatey

Instalação realizada pelo [site oficial](https://chocolatey.org/)

### Python

```
choco install -y python --version 3.8.3
```

Obs: O ROS 2 espera que a instalação do Python esteja disponível no diretório C:\python38.

### Visual C++

```
choco install -y vcredist2013 vcredist140
```

### OpenSSL

```
choco install -y openssl --version 1.1.1.2100
```

Este comando define uma variável de ambiente que persiste entre as sessões: 
```
setx /m OPENSSL_CONF "C:\Program Files\OpenSSL-Win64\bin\openssl.cfg"
```

Será necessário adicionar às variáveis de ambiente em PATH: ```C:\Program Files\OpenSSL-Win64\bin\```

### Visual Studio

Baixar a versão 2019 do Visual Studio Code. [Você pode baixar o instalador diretamente através deste link](https://visualstudio.microsoft.com/pt-br/vs/older-downloads/). 

### OpenCV

Você pode baixar uma versão pré-compilada do OpenCV 3.4.6 [aqui](https://github.com/ros2/ros2/releases/download/opencv-archives/opencv-3.4.6-vc16.VS2019.zip).

Supondo que você o descompactou em C:\opencv, digite o seguinte no Prompt de Comando (requer privilégios de Administrador):
```
setx /m OpenCV_DIR C:\opencv
```

Como você está usando uma versão pré-compilada do ROS, precisamos informar onde encontrar as bibliotecas do OpenCV. Você deve estender a variável PATH para ```C:\opencv\x64\vc16\bin```.

## Instalando dependências

Existem algumas dependências que não estão disponíveis no banco de dados de pacotes do Chocolatey. Como alguns pacotes do Chocolatey dependem dele, começamos instalando o CMake:

```
choco install -y cmake
```

Você precisará adicionar a pasta bin do CMake ```C:\Program Files\CMake\bin``` ao seu PATH.

Será necessário baixar os seguintes pacotes [deste repositório do GitHub](https://github.com/ros2/choco-packages/releases/tag/2022-03-15): 

- asio.1.12.1.nupkg
- bullet.2.89.0.nupkg
- cunit.2.1.3.nupkg
- eigen-3.3.4.nupkg
- tinyxml-usestl.2.6.2.nupkg
- tinyxml2.6.0.0.nupkg
- log4cxx.0.10.0.nupkg

Uma vez que esses pacotes estejam baixados, abra um shell administrativo e execute o seguinte comando:

```
choco install -y -s <PATH\TO\DOWNLOADS> asio cunit eigen tinyxml-usestl tinyxml2 log4cxx bullet
```

Obs: substitua <PATH\TO\DOWNLOADS> pela pasta onde foram baixados os pacotes.

Você também deve instalar algumas dependências do Python para ferramentas de linha de comando: 
```
python -m pip install -U catkin_pkg cryptography empy ifcfg lark-parser lxml netifaces numpy opencv-python pyparsing pyyaml setuptools rosdistro
```

### Instalação do QT6

Baixe a versão 6 [neste link](https://www.qt.io/offline-installers). Execute o instalador e certifique-se de selecionar o componente MSVC 2022 64-bit sob a árvore Qt -> Qt 6.8.0.

Por fim, em uma janela de comando com privilégios de administrador, defina essas variáveis de ambiente. Os comandos abaixo assumem que você o instalou no local padrão C:\Qt:
```
setx /m Qt6_DIR C:\Qt\Qt6.8.0\6.8.0\msvc2022_64
setx /m QT_QPA_PLATFORM_PLUGIN_PATH C:\dev\ros2-jazzy\bin\platforms
```

Este caminho pode mudar com base na versão do MSVC instalada, no diretório onde o Qt foi instalado e na versão do Qt instalada.

### Dependências do RQt

```
python -m pip install -U pydot PyQt6
```

```
choco install graphviz
```

Você precisará adicionar a pasta bin do Graphviz C:\Program Files\Graphviz\bin ao seu PATH, navegando até 'Editar as variáveis de ambiente do sistema'.

## Instalando o ROS 2

Vá até a [página de releases](https://github.com/ros2/ros2/releases)

Faça o download do pacote para windows ```ros2-jazzy-20240919-windows-release-amd64.zip```

 Obs: caso tenha alguma versão mais recente do ROS 2, realize o download da mesma.

 - Descompacte o arquivo zip em algum lugar (vamos assumir C:\dev\ros2_jazzy).

## Configuração do ambiente

Inicie um shell de comando e carregue o arquivo de configuração do ROS 2 para configurar o espaço de trabalho:
```
call C:\dev\ros2_jazzy\local_setup.bat
```

Obs: garanta que o arquivo esteja no caminho correto informado. Caso esteja diferente, verifica e substitua após call no comando anterior.

## Configurando o ambiente do micro-ROS

O micro-ROS é normalmente instalado usando ferramentas como o micro-ROS Agent.

### 1. Instale o Python e dependências

Certifique-se de que o Python 3.8 ou superior está instalado e configurado no PATH.
Instale o colcon, que é usado para compilar pacotes do ROS 2:
```
pip install colcon-common-extensions
```

### 2. Configure o repositório micro_ros_setup

No terminal do Windows (PowerShell ou CMD com suporte ao ROS 2):

```
mkdir micro_ros_ws
cd micro_ros_ws
git clone -b foxy https://github.com/micro-ROS/micro_ros_setup.git src/micro_ros_setup
colcon build
```

Após a construção, ative o ambiente:

```
call install/setup.bat
```

### 3. Configure um agente micro-ROS

Inicialize um workspace do micro-ROS: 

```
ros2 run micro_ros_setup create_agent_ws.sh
colcon build
```

## Execute o micro-ROS Agent

O agente do micro-ROS permite comunicação com dispositivos embarcados. Para iniciar o agente:

```
ros2 run micro_ros_agent micro_ros_agent serial --dev [porta_serial]
```

Obs: Substitua ```[porta_serial]``` pela porta COM usada pelo dispositivo.

## Desenvolvimento no dispositivo embarcado

Use a biblioteca micro-ROS Client para configurar o firmware do dispositivo embarcado.
Ferramentas como o micro-ROS CLI ajudam na depuração e no gerenciamento.

## Caso haja problema com a execução do colcon build (VisualStudioVersion is not set)

Baixe e instale o Visual Studio Community Edition

### Durante a instalação, selecione:
- Desktop Development with C++.
- Certifique-se de incluir o C++ CMake tools for Windows (nas opções do instalador).

Abra um prompt de comando como Admin e execute call C:\dev\ros2_jazzy\local_setup.bat (certifique-se de que o caminho esteja correto)
Direcione para C:\Users\```seu-usuario```\micro_ros_ws
Execute o comando: 

```
call install/setup.bat
```

Retorne para a etapa de execução do micro-ROS Agent
