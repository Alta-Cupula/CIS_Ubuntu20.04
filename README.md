# Guia de hardenização e execução de playbook CIS Ubuntu 20.04

## Instalações
### SO
Decidimos utilizar ubuntu 20.04 para evitar problemas com a versão 22.04
[Instalação](https://releases.ubuntu.com/18.04.6/ubuntu-18.04.6-live-server-amd64.iso)
1 - Ative o SSH durante a instalação
2 - Crie 2 placas de rede, uma nat e outra

### Dependências 

Ansible
```
sudo apt install ansible
```
Python 3.6+
```
sudo apt-get install python3.10
```


### Alterações necessarias

#### Criar senha para user root
```
sudo passwd root
password: (digite a senha criada na instalação)  
New Password Unix: (digite a senha que será do root)  
Repeat Password Unix: (repita a senha que será do root)
```
#### Definir seu usuario como Sudoer
Nesse momento vamos ter criar um arquivo e adicionar uma linha dentro dele:
```
sudo vim /etc/sudoers.d/<Seu_Usuario>

Seu_Usuario ALL=(ALL) NOPASSWD: ALL
```

### Ansible

Vamos utilizar o playbook: https://galaxy.ansible.com/florianutz/ubuntu2004_cis
Unico motivo é por ser da mesma versão do SO utilizado.

### Excussão

Para rodar os arquivos de configuração baixados pelo comando :
```
ansible-galaxy install florianutz.ubuntu2004_cis
```
vamos criar um arquivos chamado playbook.yml na home do nosso usuario, nele contendo :
~vim playbook.yml
```
- hosts: all
  roles:
         - { role: florianutz.ubuntu2004_cis }  
```
Tambem vamos criar nosso arquivos Hosts com com a tag all:
~vim hosts
```
[all]
192.168.0.1
```
Você deve trocar o ip para o que você configurou na outra maquina

se todos passos foram seguidos corretamente basta rodar o comando:
```
ansible-playbook playbook -i hosts
```

---
Referencias:

https://www.ansiblepilot.com/articles/ansible-troubleshooting-missing-sudo-password/
