Olá esse script é base para outros quaisquer playbook que vc necessita,
neste caso vamos instalar o zabbix-agent em servidores da familia Debian e CentOS.

Adicionando os repos necessários para cada distro.


Testado usando Ansible 2.5.1 e Vagrant 2.0.2

Caso ainda não esteja instalado, instale os pacotes (Debian like);

apt install ansible vagrant vagrant-libvirt virtualbox virtualbox-dkms virtualbox-guest-utils virtualbox-qt

Adicione ao /etc/ansible/hosts a secão [maquinas]

[maquinas]
#192.168.33.160 ansible_user=vagrant ansible_ssh_private_key_file="/home/barizom/.vagrant/machines/debian1/virtualbox/private_key"
#192.168.33.159 ansible_user=vagrant ansible_ssh_private_key_file="/home/barizom/.vagrant/machines/centos1/virtualbox/private_key"
192.168.33.10 ansible_user=vagrant ansible_ssh_private_key_file="/home/barizom/.vagrant/machines/db/virtualbox/private_key"
192.168.33.161 ansible_user=vagrant ansible_ssh_private_key_file="/home/barizom/.vagrant/machines/debian2/virtualbox/private_key"

Esse é meu exemplo para você, terá que trocar o diretorio correto para seu "..../.vagrant/machines/.... "


No diretorio onde baixar o git, 

inicie o vagrant 

$vagrant init

nesse diretorio será criado o Vagrantfile, onde estará os stament das maquinas virtuais que vamos precisar para esse exercício.

Apague o conteudo ou comento todo o conteudo de Vagrantfile e adicione o seguinte

VAGRANTFILE_API_VERSION = "2"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

#Maquina de teste centos7 
config.vm.define :centos1 do |centos_config|
config.vm.box = "centos/7"
centos_config.vm.network "private_network", ip: "192.168.33.159"

end

#Máquina de testes Debian 7 
config.vm.define :debian1 do |debian_config|
  config.vm.box = "debian/wheezy64"
  debian_config.vm.network "private_network", ip: "192.168.33.160"
end

:wq! se estiver usando o vi/vim

Inicie uma de cada vez para poder dar "yes" para a ssh key

$vagrant up centos1 

E apos suba a outra

$vagrant up debian1

rode
$vagrant status

Ambas devem estar "running" 

Rode 
$ansible-playbook -l maquinas tasks/main.yml

Apos para checar se logue em casa maquina para ver o pacote instalado

debian dpkg -l | grep -i zabbix
centos rpm -qa | grep -i zabbix

Se foi util, fico feliz.

Agradecimento ao site onde usei a referência, recomendo. 

http://www.linuxsysadmin.com.br/index.php/2016/09/22/ansible-automacao-e-provisionamento-agil/ 

Grato.
