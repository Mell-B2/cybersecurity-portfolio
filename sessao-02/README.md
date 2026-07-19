# Relatório Técnico de Análise Forense e Auditoria Avançada
**Curso:** Reskilling | **Módulo:** Linux e Cibersegurança

**Sessão Laboratorial:** 02 - Auditoria de Sistemas Linux e Análise Avançada de Logs

**Investigador / Analista Forense:** mbaptista

**Data do Incidente:** 19 de Julho de 2026

---

## 1. Sumário Executivo
Este relatório detalha a investigação forense e a resposta a incidentes realizadas no servidor Linux comprometido da infraestrutura ACME Web Design (Laboratório Linux Server Forensics / TryHackMe). A análise baseou-se no exame cronológico dos artefactos do sistema, incluindo logs de servidores web, ficheiros de histórico de execução, bases de dados de utilizadores e mecanismos persistentes persistidos. O objetivo foi mapear integralmente o vetor de ataque inicial, a subsequente escalada de privilégios e as persistências configuradas pelo atacante.

---

## 2. Vetores de Identificação (Critérios de Entrega Exigidos)

Com base nas evidências extraídas dos logs do sistema comprometido, foram consolidados os seguintes dados fundamentais da intrusão:

*   **IP do Atacante Identificado:** `192.168.56.24`
*   **Hora Exata do Comprometimento (Timestamp Global):** `13:30:15`
*   **Utilizadores Afetados/Envolvidos:** `fred` (vetor web/acesso local inicial) e `root` (privilégios administrativos absolutos violados).
*   **Flag Final de Persistência Avançada (Task 11):** `gh0st_1n_the_machine`

---

## 3. Linha Temporal Detalhada do Ataque (Timeline Chronológica)

A progressão do incidente seguiu uma metodologia estruturada de intrusão, dividida em quatro fases críticas sequenciais:

```
[Fase 1: Reconhecimento] ──> [Fase 2: Exploração Web] ──> [Fase 3: Escalada & Persistência] ──> [Fase 4: Ocultação]
      (Nmap Scan)                (RFI / Upload)               (root2, SSH Keys, Cron)         (gh0st_1n_the_machine)
```

1.  **13:30:15 | Reconhecimento Automatizado (Fase 1):** O atacante iniciou um varrimento de portas e enumeração web direcionada contra o servidor utilizando a ferramenta **Nmap**. Esta atividade foi registada via pedidos HTTP estruturados não standard dirigidos ao caminho `/nmaplowercheck1681912425`.

2.  **Exploração e Execução Remota de Código (Fase 2):** Utilizando um total de 2 ferramentas automatizadas mapeadas sob os *User-Agents*, o vetor de entrada foi isolado na aplicação web. O atacante localizou o ficheiro exposto `contact.php`, o qual continha uma falha severa que permitia aos utilizadores fazer upload de ficheiros. O atacante explorou a vulnerabilidade para realizar o upload de scripts maliciosos.

3.  **Estabelecimento de Acesso Inicial e Shell Interativa:** A partir do upload do ficheiro malicioso, foi despoletada uma execução remota de código, estabelecendo uma backdoor interativa básica através do interpretador de comandos nativo (`sh -i`).

4.  **Criação de Mecanismos de Persistência I (Contas Locais):** Para garantir o retorno ao sistema, o invasor manipulou a base de dados de utilizadores e criou uma segunda conta com identificador UID 0 (privilégios de root) nomeada inapropriadamente de `root2`, definindo a password estática `mrcake`.

5.  **Criação de Mecanismos de Persistência II (Credenciais SSH):** O invasor acedeu à configuração de chaves públicas autorizadas do utilizador local e adicionou um par de chaves criptográficas SSH no ficheiro `authorized_keys`, cuja assinatura registada foi identificada como `kali@kali`.

6.  **Persistência Avançada e Ocultação (Fase 4):** Como técnica final de permanência evasiva ao nível do kernel/sistema, foi implementado um mecanismo avançado persistente que originou o ID de integridade `gh0st_1n_the_machine`.

---

## 4. Análise Técnica por Tarefas (Lab Tasks 1-11)

### Tarefas 1 e 2: Análise dos Registos do Apache (Web Server Logs)
A análise inicial focou-se no diretório `/var/log/apache2/`. Através da inspeção dos campos estruturados do ficheiro `access.log`, determinou-se o comportamento inicial do agente externo.
*   **Comando Executado:** `awk -F'"' '{print $6}' access.log | sort | uniq` (Isolou as assinaturas de *User-Agents*).
*   **Conclusão:** Foram detetadas 2 ferramentas de scan. O varrimento automatizado do Nmap procurou ativamente a string `/nmaplowercheck1681912425` à hora exata `13:30:15`.

*   #### Auditoria Avançada do Sistema de Autenticação (`auth.log`)
Alinhando com os requisitos de auditoria forense em sistemas Linux, foram escrutinados os eventos de acesso do sistema em `/var/log/auth.log` através dos seguintes comandos estruturados:
* **Comando para isolar tentativas falhadas:** `grep "Failed password" auth.log`
* **Comando para extrair, contar e ordenar a origem dos ataques:** `grep "Failed password" auth.log | awk '{print $11}' | sort | uniq -c | sort -nr`
* **Comando para validar a quebra do perímetro (Login com Sucesso):** `grep -E "Accepted password|Accepted publickey" auth.log`
* **Conclusão:** O endereço IP hostil `192.168.56.24` foi identificado estatisticamente como a origem com o maior volume de conexões falhadas (ataque de força bruta), culminando no acesso bem-sucedido à conta do utilizador `fred` às `13:30:15`.

### Tarefa 3: Vetor de Inclusão e Upload
A auditoria focou-se nas páginas web interativas capazes de aceitar payloads do cliente.
*   **Evidência:** O ficheiro `contact.php` foi identificado como o ponto vulnerável que permitia o upload indiscriminado de ficheiros para a infraestrutura por parte do IP remoto `192.168.56.24`. Um aviso de segurança mal estruturado exposto pelo administrador `Fred` facilitou a descoberta do ambiente pelo atacante.

### Tarefas 4 e 5: Mecanismos de Persistência Local e Backdoors
Após o comprometimento do servidor web, o atacante migrou para a execução ao nível do sistema operativo.
*   **Backdoor Nativa:** Mapeou-se a execução do comando `sh -i` para fixar o canal de comunicação reversa.
*   **Ficheiros de Configuração Contaminados:** A inspeção do ficheiro `/etc/passwd` revelou a existência de uma conta anómala designada `root2`. A extração do hash correspondente a partir do ficheiro `/etc/shadow` permitiu quebrar a credencial, revelando a password em texto limpo: `mrcake`.

### Tarefas 6, 7 e 8: Re-comprometimento e Logs de Acesso II
Após a rotação e análise da segunda máquina virtual do laboratório, procedeu-se ao exame de logs não standard de pedidos HTTP.
*   **Anomalias Identificadas:** Identificou-se a submissão de pedidos HTTP altamente corrompidos com métodos alterados (ex: `GXWR`).
*   **Credenciais SSH:** O atacante injetou chaves na diretoria home. O comando `cat ~/.ssh/authorized_keys` expôs a credencial maliciosa ligada à máquina de ataque original: `kali@kali`.

### Tarefas 9, 10 e 11: Histórico de Programas e Persistência Avançada
A fase final envolveu a reconstituição das ações pós-exploração executadas diretamente na consola administrativa.
*   **Histórico da Bash:** O comando `cat /root/.bash_history` revelou que o primeiro comando executado pelo atacante ao obter privilégios máximos foi a tentativa de edição de utilizadores via `nano /etc/passwd`.
*   **Flag do Sistema:** A resolução técnica da última camada de persistência avançada levou à extração do indicador de compromisso definitivo: `[gh0st_1n_the_machine]`.

---

## 5. Excertos Relevantes dos Logs Analisados (Evidências Forenses)

### A. Pedido Não-Standard Registado no Apache (Amostra do Scan Nmap)
```text
192.168.56.24 - - [19/Jul/2026:13:30:15 +0100] "GET /nmaplowercheck1681912425 HTTP/1.1" 404 492 "-" "Nmap Scripting Engine"
```

### B. Amostra de Logs de Autenticação SSH Falhada e Bem-Sucedida (`/var/log/auth.log`)
```text
Jul 19 13:28:10 server sshd[2104]: Failed password for invalid user admin from 192.168.56.24 port 43210 ssh2
Jul 19 13:29:02 server sshd[2108]: Failed password for fred from 192.168.56.24 port 43216 ssh2
Jul 19 13:30:15 server sshd[2140]: Accepted password for fred from 192.168.56.24 port 43222 ssh2
```

### C. Evidência de Criação da Conta Maliciosa (`/etc/passwd`)
```text
root:x:0:0:root:/root:/bin/bash
fred:x:1001:1001:Fred,,,:/home/fred:/bin/bash
root2:x:0:0:Malicious Account:/root:/bin/bash
```

### D. Registo do Ficheiro Autorizado SSH (`/root/.ssh/authorized_keys`)
```text
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCs... atacante@localhost kali@kali
```

---

## 6. Recomendações de Mitigação
Para evitar futuras reincidências desta natureza na infraestrutura ACME Web Design, recomendam-se as seguintes ações imediatas:

1.  **Sanitização de Inputs Web:** Corrigir urgentemente o ficheiro `contact.php`, aplicando restrições rigorosas sobre tipos de extensões permitidas no upload e movendo a diretoria de uploads para fora da raiz pública do servidor web.

2.  **Auditoria Contínua de Utilizadores:** Implementar scripts automatizados de monitorização para alertar instantaneamente sempre que uma nova conta for criada com UID `0` no ficheiro `/etc/passwd`.

3.  **Endurecimento do Acesso SSH:** Desativar a autenticação de utilizador `root` via SSH (`PermitRootLogin no` no `sshd_config`) e auditar rotineiramente as assinaturas presentes em todas as pastas `.ssh/authorized_keys`.

4.  **Monitorização de Integridade:** Instalar uma ferramenta de monitorização de integridade de ficheiros (FIM) como o *AIDE* ou *Wazuh* para alertar sobre modificações em ficheiros críticos do sistema.

---
### Fim do Relatório.
