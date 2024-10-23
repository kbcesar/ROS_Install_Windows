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


