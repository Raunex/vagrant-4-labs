# vagrant-4-labs

Vagrantfile para criação de laboratórios com Hashicorp Vagrant.

Incluí personalizações para:

* criar automaticamente chaves de SSH para o usuário root.
~~criar arquivo /etc/hosts para resolução de nomes entre VM's.~~
* definição de máquinas virtuais baseada em arquivo YAML.

O objetivo deste projeto é usar as funcionalidades aplicadas ao arquivo Vagrantfile para montar laboratórios virtuais de forma simples e dinâmica.

## Para iniciar o laboratório

Faça o download dos arquivos disponíveis neste projeto através do git:

```
git clone https://github.com/jonathandalves/vagrant-4-labs.git
```

Acesse o diretório vagrant-4-labs e construa as máquinas virtuais através do Vagrant:

```
cd vagrant-4-labs
vagrant up
```

Repare que, no mesmo diretório, serão criados os seguintes arquivos.

Chaves de SSH:

* id_rsa
* id_rsa.pub

# Informações do Autor

Criado por [Jonathan Dantas Alves](https://www.linkedin.com/in/jonathandantasalves/).
