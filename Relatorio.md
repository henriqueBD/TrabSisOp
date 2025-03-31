# Relatório sobre a Instalação Manual do Arch Linux com GNOME em um PC Focado em Jogos

Este relatório descreve o processo de instalação do Arch Linux com o ambiente de desktop GNOME em um PC focado em jogos.

## 1. Preparação do Sistema

**Passos para Configuração da Máquina Virtual**

### 1.1 **Criação da Máquina Virtual**
- **Tipo**: Linux
- **Versão**: Arch Linux (64-bit)
- **Memória**: 4 GB
- **Disco Rígido**: Aproximadamente 20 GB
- **Processador**: 2 Nucleos
- (obs) Essas foram as configurações que usamos no nosso exemplo, mas para jogos pesados, é necessario maior espaço de disco e nucleos

### 1.2 **Configuração de Rede**
- **Adaptador de Rede**: Configurar para usar NAT.

### 1.3 **Adição da Imagem ISO**
- **Configuração de Inicialização**: Inserir a imagem ISO do Arch Linux no drive de CD/DVD da VM.

### 1.3 **Baixar a ISO do Arch Linux**
   - disponivel em https://www.archlinux.org/download/.
   
### 1.4 **Configurar a ISO como Disco de Inicialização**
   - No software de virtualização, colocamos a máquina virtual para inicializar a partir da ISO do Arch Linux baixada

## 2. Particionamento e Formatação

### Decisão:
- O sistema de arquivos **ext4** foi escolhido pela sua robustez e desempenho.
- **Particionamento**: Para a instalação do Arch, criamos duas partições principais:
  - Uma partição **/boot** para o carregador de inicialização.
  - Uma partição **/ (root)** para o sistema base, onde o Arch e todos os jogos serão instalados.
  - Uma partição **/home** para armazenar dados pessoais e configurações do usuário, com o restante do espaço disponível.
  
  A criação de partições dedicadas para /boot e /home melhora a organização e facilita a reinstalação do sistema, sem perda de dados.

- **SWAP**: É recomendado criar uma partição swap para melhorar o gerenciamento de memória, especialmente ao rodar jogos pesados.

## 3. Instalação do Arch Linux

```bash
timedatectl set-ntp true
pacstrap /mnt base linux linux-firmware
genfstab -U /mnt >> /mnt/etc/fstab
arch-chroot /mnt
```

Configuração do **hostname**, **timezone** e **locale**:

```bash
echo "Hudfilho" > /etc/hostname
ln -sf /usr/share/zoneinfo/Brazil/East /etc/localtime
locale-gen
```

### Decisão:
- **Instalação mínima**: Começar com uma instalação mínima é crucial para garantir que apenas os pacotes essenciais sejam instalados, evitando a sobrecarga do sistema.
  
## 4. Instalação do GNOME

O GNOME foi escolhido devido ao seu design moderno e bom suporte a jogos:

```bash
pacman -S gnome gnome-extra
systemctl enable gdm.service
```

- Instalar o GNOME completo (gnome-extra) garante que todos os componentes do ambiente de desktop estejam disponíveis.

## 5. Instalação de Drivers Gráficos e de Hardware

- **Drivers Gráficos**:
  - utilizamos o pacote `nvidia`:
    ```bash
    pacman -S nvidia nvidia-utils
    ```

- **Drivers de Som**: Para o áudio, instalamos o `pulseaudio` e o `pavucontrol`, que oferecem uma configuração de som ideal para a maioria dos jogos.

- **Drivers de Rede**: Para garantir que a conectividade de rede seja eficiente, instalamos `networkmanager`:
    ```bash
    pacman -S networkmanager
    systemctl enable NetworkManager
    ```

## 6. Configurações para Jogos

- **Instalação de Wine e Proton**: Esses programas são essenciais para rodar jogos feitos para Windows em um sistema Linux.
    ```bash
    pacman -S wine wine-gecko wine-mono
    ```
- **Configurações de CPU**: O uso do **cpupower** pode ser configurado para garantir que a CPU tenha um bom desempenho durante os jogos:
    ```bash
    pacman -S cpupower
    systemctl enable cpupower.service
    ```

## 7. Instalação de Softwares para Jogos

### Decisão:
- **Steam**: O Steam é a plataforma de distribuição de jogos mais popular e oferece varios jogos nativos para linux.
- A steam tambem oferece o proton, mencionado anteriormente, que permite jogos de windows rodarem no linux
    ```bash
    pacman -S steam
    ```
