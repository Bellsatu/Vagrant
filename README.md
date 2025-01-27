### Disciplina: *Administração de Sistemas Abertos*
### Aluno: *Maria Isabel Saturnino*  Matrícula: *20211380035*
### Aluno: *Samuel Araújo Cabral e Silva* Matrícula: *20242380040*
### Professor: *Pedro Filho*
#                                    
# Vagrant e Ansible

# 1. Introdução
Grandes avanços no mundo digital tornam evidente que o uso de ferramentas de automação e virtualização é essencial para aumentar a eficiência e reduzir significativamente os erros. O Vagrant e o Ansible são exemplos de tecnologias amplamente utilizadas nesse contexto.

Neste projeto, analisaremos a integração desses dois componentes, explicando de forma breve suas funcionalidades individuais e como trabalham juntos.

# 2. O que é Vagrant?
Vagrant é uma ferramenta de código aberto que permite a criação e configuração rápida de ambientes de desenvolvimento virtualizados. Com Vagrant, é possível:

Provisionar Ambientes: Criar máquinas virtuais de maneira automática e reprodutível.
Isolar Ambientes: Garantir que um ambiente de desenvolvimento não interfira em outro, usando máquinas virtuais isoladas.
Facilidade de Compartilhamento: Utilizando arquivos de configuração (Vagrantfile), outros desenvolvedores podem replicar o mesmo ambiente em suas máquinas com um simples comando.

# 3. O que é o Ansible?
O Ansible é uma ferramenta de automação open source escrita em Python. Ele é amplamente utilizado para o gerenciamento de configurações, permitindo a execução de múltiplas tarefas em diferentes ambientes. Trabalha sem a necessidade de agentes, utilizando o protocolo SSH para realizar os gerenciamentos.

Quase todos os parâmetros do Ansible são baseados em arquivos .yaml, como playbooks e roles. No entanto, alguns inventários não dependem exclusivamente desse formato e podem ser escritos em outras opções, como JSON.

A partir de palavras-chave e comandos (módulos e parâmetros), o Ansible possibilita o desenvolvimento de todos os processos de automação de forma eficiente e escalável.

# 4. Projeto
   ## Vagrantfile 
   O arquivo Vagrantfile configura uma máquina virtual baseada na box Debian Bookworm (64 bits) usando o VirtualBox como provedor. A VM é nomeada como "p01_isabel", possui 1 GB de memória RAM, e está configurada para usar uma rede privada com o IP 192.168.57.10. Além disso, são criados e conectados três discos adicionais de 10 GB cada, configurados na controladora SATA. O provisionamento da máquina é realizado com o Ansible, utilizando o arquivo playbook.yml para automatizar tarefas pós-criação.
#
## Playbook
O Playbook Ansible define a execução das tarefas, as mesmas são organizadas em roles provisionando e configurando o sistema.

        - hosts: all
        become: true
        roles:
          - atualizar_sistema
          - configurar_hostname
          - criar_usuarios
          - mensagem_saudacao
          - configurar_sudo
          - configurar_ssh
          - configurar_lvm
          - configurar_nfs
          - monitorar_acessos

As roles, são um agrupamento de tarefas que podem ser reutilizadas para a configuração de servidores ou de sistemas. No projeto foi solicitado os tipos de tarefas a serem desenvolvidas. 

### **Atualizar_sistema:** Atualiza os pacotes e o sistema operacional.
       
         - name: Atualizar Sistema Operacional  
        apt:  
          update_cache: yes  
          upgrade: dist
       
### **Configurar hostname** 
Define o nome do host para o sistema.

      - name: Configurar Hostname  
        hostname:  
          name: "p01-Isabel"

### **Criar usuários** 
Cria contas de usuários e define suas permissões, senhas ou chaves SSH.

      - name: Criar grupo aceso_ssh  
        ansible.builtin.group:  
          name: acesso_ssh  
          state: present  
      
      - name: Criar grupo "ifpb"  
        ansible.builtin.group:  
          name: ifpb  
          state: present  
      
      - name: Criar Usuários  
        ansible.builtin.user:  
          name: "{{ item }}"  
          groups: "ifpb,acesso_ssh"  
          state: present  
        loop:  
          - isabel  
          - ifpb


### **Mensagem de saudação:** 
Configura uma mensagem de boas-vindas (motd) exibida no terminal ao fazer login.

      - name: Configurar Mensagem de Saudação  
        copy:  
          content: |  
            Acesso restrito apenas a pessoas com autorização expressa.  
            Seu acesso está sendo monitorado !!!  
          dest: /etc/motd

### **Configurar sudo** 
Configura permissões de usuários para usar o comando sudo.
      
      - name: Permitir que o grupo "ifpb" tenha acesso SUDO  
        copy:  
          dest: /etc/sudoers.d/ifpb  
          content: "%ifpb ALL=(ALL) NOPASSWD:ALL"  
          mode: "0440"

### **Configurar ssh** 

A ação realiza a alteração da configuração do SSH para permitir apenas a autenticação por chaves públicas, desativando a autenticação por senha. Em seguida, modifica a configuração para bloquear o login do usuário root via SSH, garantindo maior segurança. Por fim, o serviço SSH é reiniciado para que as novas configurações entrem em vigor.

      - name: Permitir apenas chaves públicas  
        lineinfile:  
          path: /etc/ssh/sshd_config  
          state: present  
          regexp: "^PasswordAuthentication"  
          line: "PasswordAuthentication no"  
      
      - name: Bloquear root via SSH  
        lineinfile:  
          path: /etc/ssh/sshd_config  
          state: present  
          regexp: "^PermitRootLogin"  
          line: "PermitRootLogin no"  
      
      - name: Reiniciar serviço SSH  
        service:  
          name: ssh  
          state: restarted

### **Configurar lvm** 
A tarefa realiza a instalação dos pacotes necessários para utilizar a funcionalidade LVM (Logical Volume Management). Em seguida, cria um grupo de volumes chamado "dados" e um volume lógico chamado "sistema", com o tamanho especificado. Após a criação do volume lógico, ele é formatado no sistema de arquivos ext4. Finalmente, um diretório é criado para que o volume lógico seja montado corretamente no sistema.

      - name: Instalar pacotes necessários para LVM  
        apt:  
          name: lvm2  
          state: present  
      
      - name: Criar Volume Group chamado "dados"  
        lvg:  
          vg: dados  
          pvs:  
            - /dev/sdb  
            - /dev/sdc  
            - /dev/sdd  
      
      - name: Criar Logical Volume "sistema"  
        community.general.lvl:  
          vg: dados  
          lv: sistema  
          size: 15g  
      
      - name: Formatar o LV "sistema" como ext4  
        filesystem:  
          fstype: ext4  
          dev: "/dev/dados/sistema"  
      
      - name: Criar diretório "/dados" e montar a partição  
        file:  
          path: /dados  
          state: directory  
        notify:  
          - Montar partição

### **Configurar nfs** 
Configura o serviço NFS para compartilhamento de arquivos entre sistemas.
Esta tarefa realiza a instalação dos pacotes necessários para o servidor NFS, cria um usuário específico que não pode realizar login diretamente, configura o diretório compartilhado pelo NFS, adiciona as permissões adequadas no arquivo /etc/exports para permitir o acesso à rede local e reinicia o serviço do servidor NFS após as configurações.

      - name: Instalar o servidor NFS  
        apt:  
          name: nfs-kernel-server  
          state: present  
      
      - name: Criar usuário "nfs-ifpb" para NFS  
        user:  
          name: nfs-ifpb  
          shell: /usr/sbin/nologin  
      
      - name: Criar diretório NFS  
        file:  
          path: /dados/nfs  
          state: directory  
          owner: nfs-ifpb  
          group: nfs-ifpb  
      
      - name: Configurar diretório compartilhado NFS  
        blockinfile:  
          path: /etc/exports  
          block: |  
            /dados/nfs 192.168.57.0/24(rw,sync,no_root_squash)  
      
      - name: Reiniciar o servidor NFS  
        service:  
          name: nfs-kernel-server  
          state: restarted

### **Monitorar acessos** 

      - name: Configurar script de monitoramento de acessos  
        copy:  
          dest: /etc/profile.d/monitor_acesso.sh  
          content: |  
            #!/bin/bash  
            echo "$(date '+%Y-%m-%d %H:%M'); $USER; $SSH_TTY"  
          mode: "0755"

O conteúdo descreve a tarefa, que consiste em configurar um script de monitoramento de acessos. O módulo copy é utilizado para criar ou modificar arquivos no destino especificado. A diretiva dest define o caminho onde o script será salvo, que é /etc/profile.d/monitor_acesso.sh. 




