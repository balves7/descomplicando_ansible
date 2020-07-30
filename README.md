# Projeto k8s cluster com Ansible day5

Criação do projeto de um cluster k8s com ansible para o módulo do treinamento de Ansible da LinuxTips.

## Ordem de execução.

Abaixo segue a ordem para a execução dos playbooks.

```
provisioning => Cria instâncias na AWS
install_k8s => Criação do cluster e nós do k8s
deploy-app-v1 => Versão 1 da aplicação de exemplo
deploy-app-v2 => Versão 2 da aplicação de exemplo
```

## Instalação

```bash
cd provisioning
ansible-playbook -i hosts main.yml

```
Após rodar o playbook do provisioning, editar o arquivo hosts dos demais playbooks com os endereços que foram gerados no hosts do provisioning. Onde a instancia com tag ansible-1 será o master e as demais workers. 

```
[administrador@mycentos install_k8s]$ cat hosts 

[k8s-master] # IP Publico da instancia ansible-1
3.237.27.45

[k8s-workers] # IP Publico das instancias ansible-2 e 3
3.85.224.63
35.153.143.173

[k8s-workers:vars]
K8S_MASTER_NODE_IP=172.31.71.245 # IP Privado da instancia ansible-1
K8S_API_SECURE_PORT=6443

```

Seguindo para instalação da aplicação k8s
```bash
cd ../install_k8s
ansible-playbook -i hosts main.yml
```

Após rodar o playbook  copiar o arquivo hosts para a pasta deploy-app-v1 e v2
```bash
cp hosts ../deploy-app-v1
cp hosts ../deploy-app-v2
```

Com o arquivo de hosts copiado podemos rodar o playbook dos deploys v1 e v2 respectivamente.

```bash
cd ../deploy-app-v1
ansible-playbook -i hosts main.yml

cd ../deploy-app-v2
ansible-playbook -i hosts main.yml

```

* No deploy do app-v2 há um pause de 2 minutos para confirmação da remoção do app v1 para deploy do app-v2


Para visualizar a aplicação de exemplo rodando utilize o IP publico da instancia na porta 32222
Por exemplo: 

```bash
[administrador@mycentos deploy-app-v2]$  curl 3.237.27.45:32222
Giropops App - Version 2.0.0
```


