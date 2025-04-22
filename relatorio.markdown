# Relatório de Atividade — Topologia de Rede no Mininet

## 1. Descrição da Topologia

A topologia implementada consiste em 4 hosts (h1 a h4) e 3 roteadores (r1 a r3), conectados em diferentes sub-redes. Os roteadores fazem o papel de gateways entre os hosts e também entre si, utilizando links ponto-a-ponto para o roteamento entre redes.

### Esquema da Rede


### Tabela de IPs

| Dispositivo | Interface     | IP              | Sub-rede         |
|-------------|---------------|------------------|------------------|
| h1          | -             | 192.168.1.10/24  | LAN1             |
| h2          | -             | 192.168.2.10/24  | LAN2             |
| h3          | -             | 192.168.3.10/24  | LAN3             |
| h4          | -             | 192.168.4.10/24  | LAN4             |
| r1          | r1-eth0       | 192.168.1.1/24   | LAN1             |
| r1          | r1-eth1       | 10.0.0.1/30      | Link r1-r2       |
| r2          | r2-eth0       | 192.168.2.1/24   | LAN2             |
| r2          | r2-eth1       | 10.0.0.2/30      | Link r1-r2       |
| r2          | r2-eth2       | 10.0.1.1/30      | Link r2-r3       |
| r3          | r3-eth0       | 10.0.1.2/30      | Link r2-r3       |
| r3          | r3-eth1       | 192.168.3.1/24   | LAN3             |
| r3          | r3-eth2       | 192.168.4.1/24   | LAN4             |

---

## 2. Configuração

### Trecho do Script Python

```python

# Exemplo de criação de roteador

r1 = self.addNode('r1', cls=LinuxRouter, ip='192.168.1.1/24')
self.addLink(h1, r1, intfName2='r1-eth0', params2={'ip': '192.168.1.1/24'})

# Ativando roteamento IP
r1.cmd('sysctl -w net.ipv4.ip_forward=1')

# Rota padrão nos hosts
h1.cmd('ip route add default via 192.168.1.1')

# Rotas estáticas nos roteadores
r1.cmd('ip route add 192.168.2.0/24 via 10.0.0.2')


## 3. Rotas Estáticas

#Tabela de rotas por roteador
R1:

192.168.2.0/24 via 10.0.0.2

192.168.3.0/24 via 10.0.0.2

192.168.4.0/24 via 10.0.0.2

R2:

192.168.1.0/24 via 10.0.0.1

192.168.3.0/24 via 10.0.1.2

192.168.4.0/24 via 10.0.1.2

R3:

192.168.1.0/24 via 10.0.1.1

192.168.2.0/24 via 10.0.1.1

# Explicação:
Permitir que os pacotes sejam encaminhados corretamente entre as diferentes redes, utilizando os roteadores intermediários.
