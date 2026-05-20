# Packet-Tracer---Corporative-Network-Segmenting-Departments-
Segmenting organizational departments through VLSM &amp; VLAN


  Objetivos 
* Implementar divisão e otimização de espaço IP utilizando **VLSM (Variable Length Subnet Mask)**.
* Garantir o isolamento de tráfego entre setores através de **VLANs**.
* Permitir a comunicação controlada inter-departamentos via **Inter-VLAN Routing (Router-on-a-Stick)**.
* Automatizar a distribuição de rede com múltiplos escopos **DHCP** no roteador.
* Configurar acesso sem fios seguro para visitantes utilizando o padrão **WPA2-Enterprise com autenticação RADIUS**.
* Habilitar gerência remota segura do Switch principal através de uma **SVI dedicada**.
  

## Scripts de Configuração (CLI)

### 1. Configuração do Switch Principal (`2960`)
```text
enable
configure terminal

# Criação das VLANs
vlan 10
 name Engenharia
vlan 20
 name Financeiro
vlan 30
 name RH
vlan 40
 name TI
vlan 50
 name Visitantes
vlan 100
 name Servidores
exit

# Atribuição de Portas de Acesso para os Departamentos
interface range fa0/1-5
 switchport mode access
 switchport access vlan 10
exit

interface range fa0/6-10
 switchport mode access
 switchport access vlan 20
exit

interface range fa0/11-15
 switchport mode access
 switchport access vlan 30
exit

interface range fa0/16-20
 switchport mode access
 switchport access vlan 40
exit

# Porta do Access Point (VLAN Visitantes)
interface fa0/21
 switchport mode access
 switchport access vlan 50
exit

# Porta do Servidor RADIUS (VLAN Servidores)
interface fa0/22
 switchport mode access
 switchport access vlan 100
exit

# Configuração da Porta Trunk para o Roteador
interface Gig0/1
 switchport mode trunk
 description Conexao_Trunk_Roteador
exit

# Interface Virtual de Gerência (SVI) para Acesso Remoto
interface vlan 40
 ip address 192.168.10.130 255.255.255.240
 no shutdown
exit
ip default-gateway 192.168.10.129

#user Exec access
line console 0
  password armindo
  exit

#privileged Exec access
  enable secret armindo
  exit

# Ativação das Linhas de Acesso Virtual (SSH)
line vty 0 15
 username armindo password armindo
 login local
exit
