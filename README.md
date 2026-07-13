# Relatório de Laboratório: Mapeamento de Redes e Enumeração com Nmap

## Informações Gerais
* **Curso/Contexto:** Segurança de Redes & Pentesting
* **Plataformas Utilizadas:** KillerCoda (Ambiente Local) & TryHackMe (Ambiente Alvo)
* **Quarto Concluído:** Further Nmap
* **Data de Execução:** 13 de Julho de 2026

---

## 1. Introdução e Objetivos
Este repositório documenta as atividades práticas de auditoria de segurança, mapeamento de ativos e enumeração de serviços efetuadas no ambiente de laboratório. O objetivo principal foi analisar a postura de segurança de um host local e de um host remoto, aplicando conceitos de varreduras de rede, evasão de firewalls e execução de scripts automatizados através do **Nmap**.

---

## 2. Análise do Ambiente Local (KillerCoda)
Antes de interagir com o sistema alvo, realizou-se um reconhecimento básico da própria infraestrutura de ataque para validar as interfaces e os portos que já se encontravam em escuta.

### 2.1. Identificação do Endereço IP
Através do comando `ip a`, mapeou-se a seguinte configuração:
* **Interface Ativa:** `enp1s0`
* **Endereço IP Local:** `172.30.1.2`

### 2.2. Portas e Serviços em Escuta (Output do `ss -tuln`)
A execução do utilitário de estatísticas de sockets revelou o seguinte mapeamento de portas ativas na máquina local:

| Protocolo | Estado | Endereço Local:Porta | Descrição do Serviço / Uso Estimado |
| :---: | :---: | :---: | :--- |
| **TCP** | `LISTEN` | `0.0.0.0:40200` | Serviço Customizado de Laboratório |
| **TCP** | `LISTEN` | `0.0.0.0:40205` | Serviço Customizado de Laboratório |
| **TCP** | `LISTEN` | `127.0.0.1:35977` | Serviço Interno Local |
| **TCP** | `LISTEN` | `127.0.0.54:53` | Serviço de Resolução de Nomes (DNS Local) |
| **TCP** | `LISTEN` | `0.0.0.0:22` | Serviço de Acesso Remoto Seguro (SSH) |
| **TCP** | `LISTEN` | `*:40300` | Porta de Aplicação em Escuta Global |
| **TCP** | `LISTEN` | `*:40305` | Porta de Aplicação em Escuta Global |
| **UDP** | `UNCONN` | `172.30.1.2:68` | Cliente DHCP (Configuração Dinâmica de Rede) |
| **UDP** | `UNCONN` | `[fe80::...]:546` | Cliente DHCPv6 |

> **Nota:** A presença do SSH na porta padrão (22) e múltiplos serviços utilitários em portas altas é característica de ecossistemas controlados ou laboratórios virtuais de teste baseados em containers.

---

## 3. Atividade Prática: Enumeração do Alvo Remoto
A segunda fase consistiu em auditar o host remoto fornecido pelo TryHackMe (IP: `10.128.171.118`).

### 3.1. Deteção de Mecanismos de Firewall e Evasão
* **Comportamento ICMP:** O host alvo **não respondeu** às solicitações padrão de eco ICMP (ping). Isto indica a presença de uma firewall configurada para dropar pacotes ICMP, simulando que a máquina está offline.
* **Estratégia de Evasão:** Foi obrigatório o uso do parâmetro `-Pn` em todas as chamadas do Nmap, instruindo a ferramenta a saltar a fase de ping e tratar o host como permanentemente ativo.

### 3.2. Análise Comparativa de Varreduras (Scans)
* **Varredura de Natal (Xmas Scan - `-sX`):** Ao testar as primeiras 999 portas, o resultado retornou incorretamente as **999 portas como "abertas ou filtradas"**. Isto acontece porque sistemas baseados em **Microsoft Windows** violam a RFC 793 e enviam uma resposta `RST` para qualquer pacote TCP malformado recebido, impedindo o Xmas Scan de distinguir o estado real das portas.
* **Varredura TCP SYN (SYN Scan - `-sS`):** Ao executar uma varredura semiaberta (SYN) focada nas primeiras 5000 portas do dispositivo, os mecanismos de proteção foram contornados com sucesso, identificando **5 portas abertas**.

### 3.3. Enumeração Avançada com Nmap Scripting Engine (NSE)
Com as portas abertas mapeadas, utilizou-se o script especializado `ftp-anon.nse` direcionado à porta 21 (FTP). O resultado confirmou de forma inequívoca que o **acesso anónimo está ativo (Y)**, permitindo conexões sem credenciais explícitas, o que representa uma falha crítica de configuração de segurança.

---

## 4. Conclusão
O laboratório demonstrou com sucesso a importância das etapas de reconhecimento e enumeração em auditorias de infraestrutura. Foi possível identificar a topologia local, contornar com sucesso as restrições de filtragem ICMP de uma firewall restritiva através do parâmetro `-Pn` e diagnosticar um host remoto Windows exposto com 5 portas abertas e uma falha grave de configuração de login anónimo no serviço FTP.

---
_Relatório gerado para fins educacionais e de composição de portfólio de segurança._
