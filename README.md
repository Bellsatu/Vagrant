### Disciplina: Administração de sistemas Abertos
### Aluno: Maria Isabel Saturnino
### Matricula: 20211380035
### Professor: Pedro Filho
#                                    
# Vagrant e Ansible

# 1. Introdução
No mundo da administração de sistemas, o uso de ferramentas de automação e virtualização é essencial para aumentar a eficiência e minimizar erros. Duas das ferramentas mais utilizadas nesse contexto são o Vagrant e o Ansible. Este documento explicará cada uma dessas ferramentas, suas características e como elas interagem para facilitar a criação e a gestão de ambientes de desenvolvimento.

# 2. O que é Vagrant?
Vagrant é uma ferramenta de código aberto que permite a criação e configuração rápida de ambientes de desenvolvimento virtualizados. Com Vagrant, é possível:

Provisionar Ambientes: Criar máquinas virtuais de maneira automática e reprodutível.
Isolar Ambientes: Garantir que um ambiente de desenvolvimento não interfira em outro, usando máquinas virtuais isoladas.
Facilidade de Compartilhamento: Utilizando arquivos de configuração (Vagrantfile), outros desenvolvedores podem replicar o mesmo ambiente em suas máquinas com um simples comando.

# 3. O que é o Ansible
Ansible é uma ferramenta de automação open source escrita em Python. Ela foi criada para automatizar processos de provisionamento, implantação de aplicações, orquestração, gerenciamento de configurações, entre outros. 
De modo geral, ela pode ser usada em qualquer operação administrativa que antes era realizada manualmente. Imagine que você tem vários servidores, sejam eles locais, em nuvem ou híbridos. Então, você precisa remover um usuário, realizar um deploy ou configurar um novo pacote nas máquinas. 
