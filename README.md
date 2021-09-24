# Criando um Ambiente de Desenvolvimento no Windows

O tutorial abaixo tem por objetivo auxilar a criação de um ambiente completo de desenvolvimento utiliando Windows + WSL2 (Ubuntu Linux).

- [WSL2](#wsl2)
- [Fira Code Font](#fira-code-font)
- [Windows Terminal](#windows-terminal)
- [Criando um usuário no Linux](#criando-um-usuário-no-linux)
- [Zsh e Oh My Zsh](#zsh-e-oh-my-zsh)
- [Alias](#alias)
- [Chaves SSH Github](#chaves-ssh-github)
- [VSCode e Extensions](#vscode-e-extensions)
- [NVM](#nvm)
- [XServer](#xserver)
- [Google Chrome](#google-chrome)
- [Postman](#postman)
- [.NET SDK](#net-sdk)


## WSL2
No PowerShell execute o comando abaixo:

`wsl --install`

Esse comando irá habilitar o que é necessário para rodar o WSL, irá fazer o download do kernel linux mais recente, setar o WSL2 como padrão e instalar uma distribuição linux, por padrão o *Ubuntu*

Configure otimização de processador e memória para o WSL2

Abra pelo explorer o arquivo %UserProfile%/.wslconfig e defina as propriedades conforme abaixo:

```
[wsl2]
processors=2
memory=4GB
swap=4GB
```

Fonte: https://docs.microsoft.com/en-us/windows/wsl/install

## Fira Code Font

Faça o download da Fonte Fira Code em https://github.com/tonsky/FiraCode.

Descompacte os arquivos, clique com botão direito e instalar.

## Windows Terminal

Na loja de aplicativos do Windows, busque pelo Windows Terminal e o instale.

Copie o conteúdo do arquivo win-terminal-settings.json para o settings.json do windows terminal.

## Criando um usuário no Linux

Dentro do Linux (Ubuntu) digite:

`$ adduser <NOME_USUARIO>`

Configure o usuário criado para fazer parte do grupo de administradores:

`$ usermod -a -G sudo <NOME_USUARIO>`

Configure para o novo usuário seja o default no linux. No PowerShell digite:

`$ ubuntu<VERSAO_INSTALADA> config --default-user <NOME_USUARIO>`

## Zsh e Oh My Zsh

Para instalar o Zsh:

`$ sudo apt install zsh`

Para instalar o Oh My Zsh, vá no site oficial https://ohmyz.sh e procure pelo comando de instalação utilizando o curl.

Editando o tema do zsh:

Edite o arquivo .zshrc e altere a variável ZSH_THEME para o thema desejado, você pode escolher algum tema disponível em https://github.com/ohmyzsh/ohmyzsh/wiki/Themes

Para aplicar as modificações, recarregue o arquivo .zshrc:

`$ source .zshrc`

Para configurar o Zsh Auto Suggestions, clone o repo:

`$ git clone https://github.com/zsh-users/zsh-autosuggestions ~/.oh-my-zsh/custom/plugins/zsh-autosuggestions`

Edite o .zshrc e adicione o novo plugin na variável plugins. Ex:

`plugins=(git zsh-autosuggestions)`

Recarregue as configurações:

`$ source .zshrc`

## Alias

Edite o .zshrc e adicione seus alias (atalhos) preferidos. Ex

`alias dev="cd ~/Development"`

## Chaves SSH Github

No terminal, digite:

`$ ssh-keygen -t ed25519 -C "your_email@example.com"`

Edite .zshrc o plugin ssh-agent. Ex:

`plugins=(git ssh-agent zsh-autosuggestions)`

Recarregue as configurações

`$ source .zshrc`

Copiando a chave pública no Github:

`$ cd ~/.ssh`

`$ more id_rsa.pub`

Copie a chave mostrada no terminal e cole a chave:

Github > Settings > SSH and GPG keys > New SSH key

Fonte: https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent

## VSCode e Extensions

Instale o Visual Studio Code https://code.visualstudio.com/download

Após a instalação do VSCode, instale as seguintes extensões:

- Remote WSL

Feche o VSCode e abra novamente

Continue instalando as extensões no WSL:

- .NET Core Add Reference (Adrian Wilczyński)
- .NET Core Test Explorer (Jun Han)
- .NET Core User Secrets (Adrian Wilczyński) 
- Angular Extensions Pack 
- C# (Microsoft)

## NVM

O NVM é um gerenciador de versões do NodeJS que permite instalarmos e usarmos uma ou mais versões do NodeJS no mesmo ambiente. Para instalar o NVM faça o clone do repo:

`$ git clone https://github.com/lukechilds/zsh-nvm.git ~/.zsh-nvm`

`$ source ~/.zsh-nvm/zsh-nvm.plugin.zsh`

Fonte: https://github.com/nvm-sh/nvm

### Instalando o NodeJS

`$ nvm install --lts`

## XServer

Faça o download do VcXsrv https://sourceforge.net/projects/vcxsrv/

Crie um atalho com o seguinte comando:

`"C:\Program Files\VcXsrv\vcxsrv.exe" :0 -ac -terminate -lesspointer -multiwindow -clipboard -wgl -dpi auto`

Edite o .zshrc:

`export DISPLAY=$(cat /etc/resolv.conf | grep nameserver | awk '{print $2; exit;}'):0.0`
`export LIBGL_ALWAYS_INDIRECT=1`

## Google Chrome

Para instalar o Google Chrome no Linux (WSL), execute os passos abaixo:

`$ wget -q -O - https://dl.google.com/linux/linux_signing_key.pub | sudo apt-key add -`

`$ sudo sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list'`

`$ sudo apt-get update`

`$ sudo apt install google-chrome-stable`

## Postman

Para instalar o Postman no Linux (WSL), execute os passos abaixo:

`$ wget https://dl.pstmn.io/download/latest/linux64 -O postman-linux-x64.tar.gz`

`$ sudo tar -xvzf postman-linux-x64.tar.gz -C /opt`

`$ sudo ln -s /opt/Postman/Postman /usr/bin/postman`

Para atualizar o Postman

`$ rm -rf /opt/Postman`

`$ wget https://dl.pstmn.io/download/latest/linux64 -O postman-linux-x64.tar.gz`

`$ sudo tar -xvzf postman-linux-x64.tar.gz -C /opt`

## .NET SDK

Para instalar o .NET SDK é importante saber qual versão do Ubuntu está sendo utilizada e seguir o passo a passo da documentação da Microsoft. Ex da versão Ubuntu 20.04:

Adicione o repo:

```
wget https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
rm packages-microsoft-prod.deb
```

Instale o SDK

```
sudo apt-get update; \
  sudo apt-get install -y apt-transport-https && \
  sudo apt-get update && \
  sudo apt-get install -y dotnet-sdk-5.0
```

Instale o runtime do ASP.NET Core:

```
sudo apt-get update; \
  sudo apt-get install -y apt-transport-https && \
  sudo apt-get update && \
  sudo apt-get install -y aspnetcore-runtime-5.0
```
