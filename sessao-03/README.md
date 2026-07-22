# Laboratório 3 - Hardening de Redes Linux e Configuração de Firewalls

**Curso:** Reskilling  
**Módulo:** Linux e Cibersegurança  
**Objetivo de Aprendizagem:** OA3 – Aplicar  
**Formador:** Péricles Borges  
**Aluno:** Melissa Simone Lima Baptista  
**Data:** 20/07/2026

---

# Relatório Técnico

## 1. Introdução

No contexto da cibersegurança, a implementação de mecanismos de controlo de acesso é um dos pilares fundamentais para garantir a proteção de sistemas informáticos. Entre esses mecanismos, os **firewalls** desempenham um papel essencial ao controlar o tráfego de rede, permitindo apenas comunicações autorizadas e bloqueando potenciais ameaças.

Nesta sessão laboratorial foi realizada a configuração de uma política de segurança defensiva num sistema Ubuntu Linux, recorrendo às ferramentas **UFW (Uncomplicated Firewall)** e **iptables**.

O principal objetivo consistiu em aplicar uma política de firewall baseada no princípio **Default Deny**, permitindo apenas o serviço SSH, simulando o bloqueio de um endereço IP malicioso e garantindo a persistência das regras implementadas.

Esta atividade permitiu consolidar conhecimentos sobre administração segura de sistemas Linux, boas práticas de hardening e gestão de regras de firewall.

---

# 2. Objetivos

Durante esta sessão prática foram definidos os seguintes objetivos:

- Verificar o estado atual do firewall UFW;
- Configurar políticas padrão de segurança;
- Permitir apenas o serviço SSH;
- Ativar o firewall;
- Criar uma regra de bloqueio utilizando iptables;
- Guardar as regras de forma persistente;
- Validar a configuração através dos comandos de verificação;
- Analisar a política de segurança implementada.

---

# 3. Ambiente de Trabalho

| Item | Descrição |
|------|-----------|
| Sistema Operativo | Ubuntu Linux |
| Ambiente Virtual | KillerCoda Ubuntu Playground |
| Firewall | UFW |
| Sistema de filtragem | iptables |
| Backend | nftables |

---

# 4. Desenvolvimento da Atividade

## 4.1 Verificação do Estado do Firewall

Inicialmente foi verificado o estado atual do firewall utilizando o seguinte comando:

```bash
sudo ufw status
```

### Output

```text
Status: inactive
```

### Análise

O firewall encontrava-se inicialmente desativado, o que significa que o sistema não possuía qualquer política ativa de filtragem de tráfego.

Antes da sua ativação foram definidas as políticas de segurança pretendidas.

---

## 4.2 Configuração das Políticas Padrão

Foi aplicada uma política baseada no princípio **Default Deny**, considerado uma das melhores práticas de segurança para servidores Linux.

Foram executados os seguintes comandos:

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

### Objetivo

Estas configurações estabelecem que:

- Todo o tráfego de entrada será bloqueado por defeito;
- Todo o tráfego de saída continuará autorizado.

Esta abordagem reduz significativamente a superfície de ataque do servidor, permitindo apenas os serviços explicitamente autorizados.

---

## 4.3 Configuração da Regra SSH

Para garantir a administração remota do servidor foi criada uma exceção para o protocolo SSH.

```bash
sudo ufw allow 22/tcp
```

Posteriormente o firewall foi ativado.

```bash
sudo ufw --force enable
```

---

# 5. Configuração do iptables

Após a configuração do UFW foi utilizada diretamente a framework **iptables** para implementar uma regra adicional de segurança.

Foi executado o comando:

```bash
sudo iptables -A INPUT -s 203.0.113.50 -j DROP
```

## Objetivo da Regra

Foi simulado o bloqueio de um endereço IP considerado malicioso.

Sempre que um pacote proveniente do endereço:

```text
203.0.113.50
```

tentar estabelecer comunicação com o servidor, será imediatamente descartado através da ação **DROP**, impedindo qualquer resposta ao atacante.

Este tipo de regra é frequentemente utilizado para:

- bloqueio de atacantes conhecidos;
- mitigação de ataques automatizados;
- prevenção de brute force;
- proteção contra scanners de rede.

---

# 6. Persistência das Regras

Para garantir que as regras possam ser reutilizadas posteriormente foi efetuada a exportação da configuração do iptables.

Foi utilizado o comando:

```bash
iptables-save | tee /etc/iptables/rules.v4
```

Este comando gera um ficheiro contendo todas as regras atualmente carregadas no kernel, incluindo:

- tabela Filter;
- tabela NAT;
- políticas padrão;
- regras do UFW;
- regras do Docker;
- regra de bloqueio criada manualmente.

---

# 7. Resultados Obtidos

## 7.1 Estado Final do UFW

Comando executado:

```bash
ufw status verbose
```

### Output

```text
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), deny (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    Anywhere
22/tcp (v6)                ALLOW IN    Anywhere (v6)
```

### Interpretação

A análise do resultado permite concluir que:

- o firewall encontra-se ativo;
- o registo de eventos está ativado;
- todas as ligações de entrada são bloqueadas por defeito;
- apenas o protocolo SSH foi autorizado;
- existe igualmente proteção para IPv6.

Esta configuração segue o princípio de **Least Privilege**, permitindo apenas os serviços estritamente necessários.

---

## 7.2 Verificação das Regras do iptables

Comando executado:

```bash
iptables -L -v
```

Durante a análise foi possível observar:

```text
DROP all -- 203.0.113.50 anywhere
```

### Interpretação

Esta regra confirma que qualquer pacote proveniente do endereço IP fictício **203.0.113.50** será imediatamente descartado.

Também foi possível verificar que:

- a política da chain **INPUT** encontra-se definida como **DROP**;
- a política da chain **FORWARD** encontra-se igualmente em **DROP**;
- apenas a chain **OUTPUT** permanece configurada como **ACCEPT**.

Estas políticas garantem uma postura de segurança significativamente mais robusta.

---

# 8. Análise Técnica da Configuração

A política implementada segue o conceito de **Defense in Depth**, combinando diferentes mecanismos de proteção.

## Política Default Deny

Ao bloquear todo o tráfego de entrada por defeito, apenas os serviços explicitamente autorizados ficam acessíveis.

Esta estratégia reduz drasticamente a exposição do servidor.

---

## Controlo do Serviço SSH

Foi autorizada exclusivamente a porta:

```text
22/TCP
```

Desta forma, apenas administradores autorizados conseguem estabelecer sessões remotas.

---

## Bloqueio de Endereço IP

Foi criada uma regra específica para bloquear um endereço IP malicioso.

Embora tenha sido utilizado um endereço reservado para documentação (203.0.113.50), esta técnica é amplamente utilizada em ambientes reais para impedir:

- ataques de força bruta;
- scanners automáticos;
- acessos provenientes de endereços previamente identificados como maliciosos.

---

## Persistência das Configurações

A exportação das regras garante que a política implementada possa ser restaurada posteriormente, facilitando a recuperação do sistema após reinicializações ou migrações.

---

# 9. Boas Práticas de Hardening Observadas

Durante a realização desta atividade foram aplicadas diversas recomendações de segurança utilizadas em ambientes empresariais.

Entre elas destacam-se:

- utilização do princípio **Least Privilege**;
- aplicação da política **Default Deny**;
- autorização apenas dos serviços indispensáveis;
- utilização simultânea do UFW e iptables;
- ativação do logging do firewall;
- bloqueio explícito de endereços IP suspeitos;
- armazenamento persistente das regras.

Estas medidas reduzem significativamente o risco de acessos não autorizados e aumentam o nível geral de proteção do servidor.

---

# 10. Conclusão

A presente atividade permitiu compreender e aplicar técnicas fundamentais de hardening em sistemas Linux através da configuração do UFW e do iptables.

Foi implementada uma política de firewall baseada no princípio **Default Deny**, permitindo apenas o serviço SSH e bloqueando explicitamente um endereço IP considerado malicioso.

Os testes realizados demonstraram que:

- o firewall foi corretamente ativado;
- as políticas padrão foram aplicadas com sucesso;
- o acesso SSH permaneceu disponível;
- a regra DROP foi corretamente adicionada à chain INPUT;
- a configuração foi exportada com sucesso através do comando `iptables-save`;
- os comandos `ufw status verbose` e `iptables -L -v` confirmaram a correta implementação das regras.

Conclui-se que todos os objetivos definidos para esta sessão laboratorial foram cumpridos, demonstrando uma implementação consistente de práticas de segurança utilizadas em ambientes profissionais de administração de sistemas Linux.

---

# Anexo A — Comandos Executados

```bash
sudo ufw status

sudo ufw default deny incoming

sudo ufw default allow outgoing

sudo ufw allow 22/tcp

sudo ufw --force enable

sudo iptables -A INPUT -s 203.0.113.50 -j DROP

iptables-save | tee /etc/iptables/rules.v4

ufw status verbose

iptables -L -v
```

---

# Anexo B — Evidências Obtidas

## Estado do UFW

```text
Status: active
Logging: on (low)

Default:
deny (incoming)
allow (outgoing)
deny (routed)

22/tcp ALLOW IN Anywhere
22/tcp (v6) ALLOW IN Anywhere (v6)
```

---

## Regra criada no iptables

```text
DROP all -- 203.0.113.50 anywhere
```

---

## Resumo Executivo

| Requisito | Estado |
|-----------|--------|
| Verificação do UFW |  Concluído |
| Política Default Deny |  Aplicada |
| Política Allow Outgoing |  Aplicada |
| Permissão SSH |  Configurada |
| Ativação do Firewall |  Concluída |
| Regra DROP no iptables |  Configurada |
| Persistência das regras |  Efetuada |
| Validação com `ufw status verbose` |  Confirmada |
| Validação com `iptables -L -v` |  Confirmada |
| Explicação técnica da política |  Incluída |

---

**Resultado Final:** Todos os objetivos propostos pelo laboratório foram cumpridos com sucesso, evidenciando uma configuração segura do firewall e a aplicação de boas práticas de hardening em sistemas Linux.
