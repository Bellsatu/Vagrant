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
### Vagrant
      # -*- mode: ruby -*-
   # vi: set ft=ruby :
   # Before execute this Vagrant File, Type the command below in your Linux Terminal
   #   export VAGRANT_EXPERIMENTAL="disks"
   
   # Vagrantfile 
  




