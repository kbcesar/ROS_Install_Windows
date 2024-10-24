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

Supondo que você o descompactou em C:\opencv, digite o seguinte no Prompt de Comando (requer privilégios de Admin):
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

### Instalação do QT5

Baixe a versão [neste link](https://www.qt.io/offline-installers). Execute o instalador e certifique-se de selecionar o componente MSVC 2017 64-bit sob a árvore Qt -> Qt 5.12.12.

Por fim, em uma janela cmd.exe com privilégios de administrador, defina essas variáveis de ambiente. Os comandos abaixo assumem que você o instalou no local padrão C:\Qt:
```
setx /m Qt5_DIR C:\Qt\Qt5.12.12\5.12.12\msvc2017_64
setx /m QT_QPA_PLATFORM_PLUGIN_PATH C:\Qt\Qt5.12.12\5.12.12\msvc2017_64\plugins\platforms
```

Este caminho pode mudar com base na versão do MSVC instalada, no diretório onde o Qt foi instalado e na versão do Qt instalada.

### Dependências do RQt

```
python -m pip install -U pydot PyQt5
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
