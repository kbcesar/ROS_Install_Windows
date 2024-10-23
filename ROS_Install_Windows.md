# Instalação ROS 2 Jazz

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


