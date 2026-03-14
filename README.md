# Guia de Configuração: Expo via Cabo USB em Rede Corporativa

Este documento descreve o passo a passo para rodar projetos Expo em ambientes com restrições de rede (firewall/Wi-Fi bloqueado) utilizando a comunicação via cabo USB (ADB) e forçando o tráfego via localhost.

## 1. Criação do Projeto (SDK 54)
Para garantir compatibilidade com a versão do Expo Go instalada no dispositivo mobile (que não pode ser atualizada), crie o projeto fixando o template na versão 54.

No PowerShell, execute:

npx create-expo-app meuProjeto --template expo-template-blank@54

## 2. Preparação do Celular (Depuração USB)
1. No aparelho Android, acesse **Configurações > Sobre o telefone**.
2. Toque 7 vezes em **Número da Versão** (ou Versão da MIUI) para habilitar o modo desenvolvedor.
3. Volte, acesse **Opções do Desenvolvedor** e ative a **Depuração USB**.
4. Conecte o celular ao computador via cabo USB (certifique-se de que é um cabo de transferência de dados).
5. Na barra de notificações do celular, altere o modo USB para **Transferência de Arquivos (MTP)**.

## 3. Configuração do ADB (Platform-Tools) no VS Code
Como o Android Studio não está instalado, utilizamos o utilitário leve do Google (ADB) para criar o túnel USB.

1. Baixe o [SDK Platform-Tools](https://dl.google.com/android/repository/platform-tools-latest-windows.zip) e extraia no computador.
2. No terminal do VS Code com seu projeto expo aberto, abra um novo terminal e adicione o caminho da pasta extraída às variáveis de ambiente da sessão atual:
powershell

$env:PATH += ";C:\Users\paulo.hgdias\Downloads\platform-tools-latest-windows\platform-tools"

3. Verifique se o computador reconheceu o celular:
Mesmo terminal anterior:

adb devices

> **Atenção:** Se o retorno for `unauthorized`, olhe para a tela do celular, marque a opção "Sempre permitir deste computador" e toque em **Permitir**. Rode o comando novamente até o status mudar para `device`.

## 4. Iniciando o Servidor (Modo Localhost)
Para evitar que o Expo tente usar o IP da rede Wi-Fi corporativa (o que causaria bloqueio e tela azul), force a inicialização pelo IP local (`127.0.0.1`).

No terminal do projeto, execute:
PowerShell

npx expo start --localhost


## 5. Execução no Dispositivo
Com o servidor rodando:
1. Pressione a tecla **`a`** no terminal para abrir o aplicativo no Android via ADB.
2. O terminal identificará a divergência de versões do Expo Go e perguntará se deseja instalar a versão recomendada (`Install the recommended Expo Go version?`).
3. **Digite `n` e pressione Enter.**
> Recusar a atualização é fundamental, pois a rede bloqueia o download e travaria o processo em 0%. O projeto abrirá normalmente na versão atual do aplicativo.