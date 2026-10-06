# AWS Route 53 — DNS Failover e Alta Disponibilidade

## 📌 Objetivo

Neste laboratório, configurei o **Amazon Route 53** para implementar um mecanismo de **failover**, permitindo que o tráfego seja direcionado automaticamente para um servidor secundário quando o servidor principal ficar indisponível.

## 🏗️ Arquitetura

A solução utiliza:

* **Amazon Route 53** — gerenciamento de DNS e failover
* **Amazon EC2** — servidores web principal e secundário
* **Route 53 Health Check** — monitoramento da disponibilidade do servidor principal
* **Availability Zones diferentes** — maior disponibilidade da aplicação

### Servidor principal

* Availability Zone: `us-west-2a`
* IP público: `16.146.137.5`
* Health Check: `Primary-Website-Health`

### Servidor secundário

* Availability Zone: `us-west-2b`
* IP público: `52.32.26.253`

## 🔄 Configuração do Failover

Foi criado um registro DNS do tipo **A** com política de roteamento **Failover**.

### Principal

```text
Tipo: A
Política: Failover
Função: Principal
Health Check: Primary-Website-Health
```

### Secundário

```text
Tipo: A
Política: Failover
Função: Secundário
```

## ❤️ Health Check

O Route 53 monitora o servidor principal através de:

```text
http://16.146.137.5:80/cafe
```

Quando o servidor principal está funcionando, o Health Check permanece como **Íntegro**.

## 🧪 Teste de Failover

Para testar a alta disponibilidade, o servidor EC2 principal foi **parado intencionalmente**.

Após a parada:

1. O Health Check identificou que o servidor principal estava indisponível.
2. O status mudou para **Não íntegro**.
3. O Route 53 deixou de direcionar o tráfego para o servidor principal.
4. O DNS passou a direcionar o acesso para o servidor secundário.
5. O site continuou acessível através do segundo servidor.

## ✅ Resultado

O failover funcionou corretamente.

O acesso inicialmente apresentou informações do servidor:

```text
16.146.137.5
us-west-2a
```

Após a indisponibilidade do servidor principal, passou para:

```text
52.32.26.253
us-west-2b
```

Isso comprovou o funcionamento do mecanismo de **DNS Failover e alta disponibilidade**.

## 📚 Principais aprendizados

* Como funciona o **Amazon Route 53**.
* Como utilizar **Health Checks**.
* Como configurar uma política de roteamento **Failover**.
* Diferença entre servidor **Principal** e **Secundário**.
* Importância das **Availability Zones** para disponibilidade.
* Como testar uma situação de indisponibilidade.
* Como o DNS pode direcionar o tráfego para outro servidor automaticamente.

## 🛠️ Serviços AWS utilizados

* Amazon Route 53
* Amazon EC2
* AWS Health Check
* Availability Zones

## 🎯 Conclusão

Este laboratório demonstrou, na prática, como utilizar o Amazon Route 53 para aumentar a disponibilidade de uma aplicação web através de **monitoramento de saúde e failover automático entre servidores EC2**.
