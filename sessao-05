# Laboratório – Sessão 05
## Análise de Vulnerabilidades em Linux com Lynis

**Curso:** Reskilling – Linux & Cibersegurança  
**Módulo:** Linux e Cibersegurança  
**Sessão:** 05 – Análise de Vulnerabilidades em Linux e Ferramentas de Auditoria  
**Objetivo de Aprendizagem:** OA5 – Criar  
**Ferramenta de Auditoria:** Lynis 3.0.9  
**Ambiente:** KillerKoda – Ubuntu Playground  
**Sistema Operativo:** Ubuntu 24.04 LTS (Kernel 6.8.0)

---

# 1. Objetivo

O presente laboratório teve como objetivo realizar uma auditoria de segurança ao sistema operativo Linux utilizando a ferramenta **Lynis**, permitindo identificar vulnerabilidades, configurações inseguras e oportunidades de melhoria de acordo com as boas práticas de hardening recomendadas pela CISOfy e pelos CIS Benchmarks.

---

# 2. Ambiente de Testes

| Componente | Especificação |
|------------|---------------|
| Plataforma | KillerKoda Ubuntu Playground |
| Sistema Operativo | Ubuntu 24.04 LTS |
| Kernel | Linux 6.8.0 |
| Ferramenta | Lynis 3.0.9 |
| Utilizador | root |

---

# 3. Procedimentos Executados

Foram executados os seguintes comandos durante a realização da auditoria:

```bash
sudo apt update
sudo apt install lynis -y
lynis --version
sudo lynis audit system
```

Após a instalação, foi iniciada uma auditoria completa ao sistema utilizando o comando:

```bash
sudo lynis audit system
```

O Lynis analisou os principais componentes do sistema, incluindo:

- Kernel
- Serviços
- Sistema de ficheiros
- Autenticação
- Utilizadores
- Firewall
- SSH
- Rede
- Permissões
- Hardening
- Integridade do sistema

---

# 4. Resultados da Auditoria

## Hardening Score

**60 / 100**

## Warnings

**1**

### Warning identificado

- Presença de pacotes vulneráveis no sistema (PKGS-7392).

## Suggestions

**50**

O Lynis apresentou diversas recomendações de melhoria relacionadas com autenticação, sistema de ficheiros, serviços, SSH, permissões, integridade e hardening do sistema.

---

# 5. Análise das Vulnerabilidades (Filesystem)

Foram selecionadas duas recomendações da categoria **Filesystem** para análise técnica.

---

## Vulnerabilidade 1 – Partição dedicada para /home

### Identificação

```
FILE-6310
To decrease the impact of a full /home file system,
place /home on a separate partition.
```

### Análise

O diretório **/home** armazena os ficheiros e dados dos utilizadores. Quando partilha a mesma partição do sistema operativo, existe o risco de esgotamento do espaço em disco, comprometendo o funcionamento do sistema.

### Medida Corretiva

Criar uma partição exclusiva para **/home**, permitindo:

- isolamento dos dados dos utilizadores;
- maior facilidade na realização de backups;
- reinstalação do sistema sem perda de dados;
- redução do impacto provocado por utilização excessiva do espaço em disco.

### Benefícios

- Maior disponibilidade do sistema.
- Melhor gestão do armazenamento.
- Aumento da segurança operacional.

---

## Vulnerabilidade 2 – Partição dedicada para /tmp

### Identificação

```
FILE-6310
To decrease the impact of a full /tmp file system,
place /tmp on a separate partition.
```

### Análise

O diretório **/tmp** é utilizado para armazenamento de ficheiros temporários por aplicações e utilizadores.

Quando partilha a mesma partição do sistema, pode facilitar ataques através da execução de ficheiros temporários ou provocar indisponibilidade caso fique completamente cheio.

### Medida Corretiva

Criar uma partição dedicada para **/tmp** utilizando as opções de montagem:

```text
nodev
nosuid
noexec
```

Estas opções impedem:

- criação de dispositivos especiais;
- execução de programas temporários;
- utilização de privilégios elevados através de ficheiros temporários.

### Benefícios

- Redução da superfície de ataque.
- Maior proteção contra malware.
- Melhoria do hardening do sistema.

---

# 6. Avaliação Geral

A auditoria demonstrou que o sistema apresenta uma configuração funcional, mas necessita de várias melhorias para cumprir boas práticas de segurança.

Embora apenas tenha sido identificado um Warning crítico, foram apresentadas cinquenta recomendações que permitem aumentar significativamente o nível de hardening do sistema.

As recomendações relacionadas com o sistema de ficheiros representam medidas preventivas importantes, reduzindo riscos associados à disponibilidade, integridade e segurança dos dados.

---

# 7. Conclusão

A utilização da ferramenta Lynis permitiu realizar uma avaliação detalhada da segurança do sistema Ubuntu.

O sistema obteve um **Hardening Score de 60**, indicando que existem diversas oportunidades de melhoria para alcançar um nível de segurança mais elevado.

Entre as recomendações analisadas, destacou-se a necessidade de separar os diretórios **/home** e **/tmp** em partições independentes, uma prática amplamente recomendada pelos CIS Benchmarks por aumentar a resiliência do sistema, melhorar a gestão do armazenamento e reduzir potenciais vetores de ataque.

Este laboratório permitiu compreender a importância das auditorias de segurança em sistemas Linux e demonstrou como ferramentas automáticas podem apoiar a implementação de medidas de hardening e conformidade com boas práticas internacionais.

---

# Referências

- CISOfy. *Lynis Security Auditing Tool*. https://cisofy.com
- CIS Benchmarks. *Linux Security Guidelines*. https://www.cisecurity.org/cis-benchmarks
- Ubuntu Documentation. https://ubuntu.com/server/docs
