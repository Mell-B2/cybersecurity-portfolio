# Relatório Técnico: Mapeamento de Redes e Enumeração (Sessão 01)

Este relatório compõe a entrega oficial da **Sessão 01**, estruturado em tarefas para detalhar as atividades práticas de reconhecimento e varredura realizadas no laboratório.

---

## 1. Auditoria do Ambiente Local (KillerCoda)

Para mapear a nossa própria infraestrutura de origem antes de qualquer auditoria externa, foram executados os comandos `ip a` e `ss -tuln`.

### A. Output Completo do Comando `ip a`
Este comando identifica a interface de rede ativa e o endereço IP privado atribuído à nossa máquina de ataque:

```text
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: enp1s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 02:42:ac:1e:01:02 brd ff:ff:ff:ff:ff:ff
    inet 172.30.1.2/24 brd 172.30.1.255 scope global enp1s0
       valid_lft forever preferred_lft forever
    inet6 fe80::2ec2:3706:c647:fc14/64 scope link 
       valid_lft forever preferred_lft forever

Netid State  Recv-Q Send-Q  Local Address:Port   Peer Address:Port Process
tcp   LISTEN 0      4096          0.0.0.0:40200       0.0.0.0:*            
tcp   LISTEN 0      4096          0.0.0.0:40205       0.0.0.0:*            
tcp   LISTEN 0      4096        127.0.0.1:35977       0.0.0.0:*            
tcp   LISTEN 0      4096      127.0.0.54%:53          0.0.0.0:*            
tcp   LISTEN 0      128           0.0.0.0:22          0.0.0.0:*            
tcp   LISTEN 0      4096             [::]:40300          [::]:*            
tcp   LISTEN 0      4096             [::]:40305          [::]:*            
udp   UNCONN 0      0           172.30.1.2:68          0.0.0.0:*            
udp   UNCONN 0      0      [fe80::2ec2:3706:c647:fc14]%enp1s0:546          [::]:*
