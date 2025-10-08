# redes-corporativa-security
 Como configurar uma rede corporativa simples com VLANs, trunk, port security e BPDU Guard  para proteger sua rede contra acessos indevidos e loops no packet tracer.
Este projeto simula uma infraestrutura de rede corporativa segmentada para três departamentos ADM VLAN-10,VENDAS VLAN-20 e GERENCIA VLAN-99

Objetivo Principal: Garantir a comunicação segura entre diferentes departamentos (VLANs) usando o método Router-on-a-Stick, ao mesmo tempo que protege as portas do switch contra vulnerabilidades comuns.

Topologia e Endereçamento
(Aqui você incluiria o Screenshot da Topologia do Packet Tracer)
ADM VLAN-10	192.168.10.0/24	
VENDAS VLAN-20	192.168.20.0/24	
GERENCIA VLAN-99	192.168.99.0/24	
DESABILITADA VLAN-999 (Medida de segurança usada para desativar portar não usadas)

Detalhes da Configuração
1. Roteamento  (Router-on-a-Stick)
O roteador foi configurado com sub-interfaces para atuar como o gateway para cada VLAN.
2. Segurança na Camada 2
Foram implementadas duas políticas de segurança nas portas de acesso (Fa0/2-Fa0/4) para proteger a rede.

2.1. Port Security
Propósito: Impedir que um usuário mal-intencionado conecte um novo PC à rede e obtenha um endereço IP.

Regra Aplicada: A porta só aceita o primeiro endereço MAC (usando sticky) e limita o máximo a um MAC. Se houver violação, a porta desliga (shutdown).

Switch(config-if)# switchport port-security
Switch(config-if)# switchport port-security maximum 1
Switch(config-if)# switchport port-security mac-address sticky
Switch(config-if)# switchport port-security violation shutdown
2.2. BPDU Guard
Propósito: Evitar loops acidentais na rede. Bloqueia imediatamente qualquer porta de acesso que receba pacotes BPDU (que são enviados por outros switches).

Switch(config)# interface range FastEthernet 0/2 - 4
Switch(config-if-range)# spanning-tree portfast
Switch(config-if-range)# spanning-tree bpduguard enable
 Verificação e Prova de Funcionamento
O teste final demonstra que o roteamento está funcionando e que as regras de segurança estão ativas.

Teste 1: Comunicação VLAN
Teste: Ping de um PC da VLAN 10 (ADM) para um PC da VLAN 20 (VENDAS).

Resultado Esperado: Ping bem-sucedido.

(Aqui você incluiria um Screenshot do Ping Bem-sucedido)

Teste 2: Prova de Port Security
Teste: Tentativa de conectar um quarto PC (não autorizado) a uma porta configurada com Port Security.

Resultado Esperado: A porta do switch entra no estado err-disabled e não permite mais tráfego.
