# AWS Auto Scaling com EC2, Application Load Balancer e Alta Disponibilidade

## 📌 Sobre o laboratório

Neste laboratório, configurei uma arquitetura de alta disponibilidade na AWS utilizando **Amazon EC2, Amazon Machine Image (AMI), Application Load Balancer (ALB), Target Group e EC2 Auto Scaling**.

O objetivo foi criar servidores web distribuídos em diferentes Availability Zones e configurar o Auto Scaling para aumentar automaticamente a quantidade de instâncias quando a utilização de CPU ultrapassasse o limite definido.

---

## 🎯 Objetivos

* Criar uma instância EC2 utilizando AWS CLI.
* Criar uma AMI a partir da instância.
* Criar um Application Load Balancer.
* Criar um Launch Template.
* Configurar um Auto Scaling Group.
* Distribuir o tráfego entre diferentes Availability Zones.
* Configurar uma política de Target Tracking baseada em CPU.
* Testar o Scale Out automático.

---

## 🏗️ Arquitetura

A arquitetura criada utiliza:

```text
                    Internet
                       │
                       ▼
             Application Load Balancer
                  WebServerELB
                       │
              ┌────────┴────────┐
              ▼                 ▼
         WebApp EC2         WebApp EC2
        us-west-2a          us-west-2b
              │                 │
              └────────┬────────┘
                       │
                Auto Scaling
                 Min: 2
                 Desired: 2
                 Max: 4
                       │
                CPU Target: 50%
```

O Application Load Balancer distribui as requisições entre as instâncias saudáveis do Auto Scaling Group.

---

## ☁️ Serviços AWS utilizados

| Serviço                   | Utilização                                     |
| ------------------------- | ---------------------------------------------- |
| Amazon EC2                | Servidores web                                 |
| Amazon AMI                | Imagem utilizada para criar novas instâncias   |
| Application Load Balancer | Distribuição de tráfego                        |
| Target Group              | Controle das instâncias disponíveis para o ALB |
| EC2 Auto Scaling          | Escalabilidade automática                      |
| Amazon CloudWatch         | Monitoramento utilizado pelo Auto Scaling      |
| AWS CLI                   | Criação e configuração de recursos             |

---

## 🔧 Etapas realizadas

### 1. Criação da instância EC2

Utilizei o AWS CLI para criar uma instância EC2 com:

* Instance Type: `t3.micro`
* AMI: `ami-01477f93b365aa11a`
* Security Group: `HTTPAccess`
* Subnet pública
* Public IP habilitado

A instância criada foi:

`WebServer`

---

### 2. Criação da AMI

Após configurar o servidor web, criei uma Amazon Machine Image:

`WebServerAMI`

Essa imagem foi utilizada posteriormente pelo Launch Template.

---

### 3. Application Load Balancer

Criei o Application Load Balancer:

`WebServerELB`

Configurações principais:

* Tipo: Application
* Scheme: Internet-facing
* IP: IPv4
* Listener: HTTP : 80
* Availability Zones:

  * us-west-2a
  * us-west-2b

O ALB encaminha o tráfego para o Target Group:

`webserver-app`

Health Check:

`/index.php`

---

### 4. Launch Template

Criei o Launch Template:

`web-app-launch-template`

Configurações principais:

* AMI: `WebServerAMI`
* Instance Type: `t3.micro`
* Security Group: `HTTPAccess`

O Launch Template define como novas instâncias serão criadas pelo Auto Scaling.

---

### 5. Auto Scaling Group

Criei o Auto Scaling Group:

`Web App Auto Scaling Group`

Configuração:

* Minimum: **2**
* Desired: **2**
* Maximum: **4**
* CPU Target: **50%**
* Health Check: EC2 + ELB
* Availability Zones:

  * us-west-2a
  * us-west-2b

As instâncias foram distribuídas entre diferentes Availability Zones para aumentar a disponibilidade da aplicação.

---

## 📈 Teste de Scale Out

Para testar o Auto Scaling, utilizei a aplicação de geração de carga disponível no servidor web.

Ao aumentar a utilização de CPU, o Auto Scaling identificou que a média estava acima do objetivo configurado de **50%**.

Como resultado, uma nova instância EC2 foi iniciada automaticamente.

### Resultado

**Scale Out realizado com sucesso.**

O laboratório demonstrou na prática o fluxo:

```text
Aumento da carga
       ↓
Aumento da utilização de CPU
       ↓
CloudWatch / Target Tracking
       ↓
Auto Scaling identifica a necessidade
       ↓
Nova instância EC2 é criada
       ↓
Instância entra no Target Group
       ↓
ALB pode distribuir tráfego
```

---

## 📸 Evidências

As evidências do laboratório estão disponíveis na pasta:

`evidencias/`

### Principais evidências

1. Criação da instância EC2
2. Criação da AMI
3. Application Load Balancer
4. Launch Template
5. Auto Scaling Group
6. Target Group com instâncias saudáveis
7. Scale Out realizado pelo Auto Scaling

---

## 🧠 O que aprendi

Neste laboratório, aprendi que o **Auto Scaling não é simplesmente criar mais servidores**.

Ele permite que a infraestrutura responda automaticamente à demanda da aplicação.

Também compreendi na prática a relação entre:

**EC2 → AMI → Launch Template → Auto Scaling → Target Group → ALB**

e como esses serviços trabalham juntos para criar uma aplicação mais **disponível, escalável e resiliente**.

---

## 💡 Principais conceitos praticados

* Alta disponibilidade
* Availability Zones
* Load Balancing
* Health Checks
* Auto Scaling
* Scale Out
* Target Tracking
* CPU utilization
* AMI
* Launch Template
* Target Group
* AWS CLI
* Arquitetura distribuída

---

## 🚀 Resultado final

Laboratório concluído com sucesso, demonstrando uma arquitetura capaz de distribuir tráfego entre múltiplas instâncias EC2 e aumentar automaticamente a capacidade computacional conforme a demanda.

**Status: ✅ Concluído**
