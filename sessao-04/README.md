# Relatório - Hardening de SSH e Auditoria de Sistemas Linux

| Parâmetro | Especificação Técnica |
|-----------|-----------------------|
| **Módulo / Projeto** | Reskilling — Linux & Cibersegurança |
| **Sessão / Laboratório** | Sessão 04 — Gestão e Fortalecimento de Acessos Remotos (SSH) |
| **Nível de Aprendizagem** | OA4 · Aplicar & Avaliar |
| **Ambiente Virtual** | TryHackMe — Linux Strength Training *(100% concluído)* |
| **Utilizador / Autor** | mbaptista |
| **Formador** | Péricles Borges |

---

# Sumário Executivo

O presente relatório documenta as atividades práticas realizadas durante a **Sessão 04 – Hardening de SSH e Auditoria de Sistemas Linux**, cujo principal objetivo consistiu na consolidação de competências de administração segura de sistemas Linux e na implementação de mecanismos de reforço da segurança do serviço **OpenSSH**.

As atividades desenvolveram-se em duas componentes complementares:

- **Resolução integral do laboratório Linux Strength Training na plataforma TryHackMe**, abrangendo operações de administração de sistemas, criptografia, manipulação de ficheiros, consultas SQL e auditoria de permissões.
- **Implementação de medidas de hardening do serviço SSH**, reduzindo significativamente a superfície de ataque através da utilização de autenticação baseada em chaves criptográficas e da restrição de acessos privilegiados.

No final da sessão foi possível validar uma configuração segura do serviço SSH, alinhada com as boas práticas atualmente utilizadas em ambientes empresariais.

---

# Objetivos

Durante esta sessão laboratorial foram definidos os seguintes objetivos:

- Consolidar conhecimentos avançados de administração Linux;
- Aplicar técnicas de pesquisa e manipulação do sistema de ficheiros;
- Compreender mecanismos de hash e criptografia;
- Trabalhar com bases de dados SQL em ambiente Linux;
- Aplicar técnicas básicas de auditoria de segurança;
- Reforçar a configuração do serviço OpenSSH;
- Implementar autenticação baseada em chaves assimétricas;
- Eliminar mecanismos inseguros de autenticação por palavra-passe;
- Validar a configuração final do servidor SSH.

---

# Ambiente de Trabalho

| Item | Descrição |
|------|-----------|
| Plataforma | TryHackMe |
| Laboratório | Linux Strength Training |
| Sistema Operativo | Ubuntu Linux |
| Serviço Configurado | OpenSSH Server |
| Algoritmo de Chaves | Ed25519 |
| Utilizador Administrativo | adminuser |

---

# Desenvolvimento das Atividades

## Tarefa 1 - Introdução e Configuração do Ambiente

Foi iniciada a máquina virtual disponibilizada pela plataforma **TryHackMe**, estabelecendo-se posteriormente a comunicação entre a máquina atacante (AttackBox/VPN) e o servidor alvo.

Esta fase teve como objetivo preparar o ambiente para a execução das restantes atividades, validando a conectividade e garantindo que todos os serviços necessários estavam operacionais.

![Evidência Tarefa 1](Tarefa%201.png)

---

## Tarefa 2 - Navegação e Localização no Sistema de Ficheiros

Nesta tarefa foram explorados diferentes comandos de pesquisa e localização de ficheiros existentes no sistema Linux.

Foram utilizados critérios como:

- nome do ficheiro;
- proprietário;
- grupo;
- dimensão;
- data de modificação;
- localização em diretórios específicos.

A utilização destas ferramentas permitiu compreender a organização hierárquica do sistema de ficheiros Linux e localizar rapidamente informação relevante.

![Evidência Tarefa 2](Tarefa%202.png)

---

## Tarefa 3 - Gestão e Manipulação de Ficheiros

Foram executadas operações fundamentais de administração do sistema de ficheiros, incluindo:

- criação de ficheiros;
- movimentação entre diretórios;
- cópia de documentos;
- renomeação de ficheiros;
- edição de conteúdo;
- manipulação de nomes contendo caracteres especiais.

Adicionalmente foram utilizadas ferramentas de transferência de ficheiros entre diferentes sistemas Linux.

![Evidência Tarefa 3](Tarefa%203.png)

---

## Tarefa 4 - Introdução aos Hashes Criptográficos

Esta atividade incidiu sobre funções de hash criptográfico, nomeadamente:

- MD5;
- SHA-1;
- SHA-256.

Foi demonstrado que estas funções produzem valores unidirecionais utilizados para garantir a integridade da informação.

Posteriormente foram aplicadas técnicas de força bruta e ataques por dicionário para recuperar palavras-passe a partir dos respetivos hashes.

![Evidência Tarefa 4](Tarefa%204.png)
![Evidência Tarefa 4.1](Tarefa%204.1.png)

---

## Tarefa 5 - Descodificação em Base64

Nesta fase foi estudado o mecanismo Base64.

Foram realizados exercícios de:

- codificação;
- descodificação;
- interpretação dos dados obtidos.

Foi igualmente reforçada a diferença entre:

- **Codificação**, utilizada para representação de dados;
- **Criptografia**, utilizada para garantir confidencialidade.

![Evidência Tarefa 5](Tarefa%205.png)

---

## Tarefa 6 - Criptografia Simétrica com GPG

Foi utilizada a ferramenta **GNU Privacy Guard (GPG)** para proteger documentos através de criptografia simétrica.

Durante a atividade foram executadas operações de:

- encriptação;
- desencriptação;
- validação da frase-passe.

A cifra utilizada foi baseada no algoritmo **AES-128**, permitindo proteger eficazmente os ficheiros contra acessos não autorizados.

![Evidência Tarefa 6](Tarefa%206.png)

---

## Tarefa 7 - Auditoria e Quebra de Ficheiros GPG

Nesta tarefa foi simulado um processo de auditoria de segurança.

Foram utilizadas as ferramentas:

- John the Ripper;
- utilitários de extração de hashes.

O objetivo consistiu em recuperar a palavra-passe utilizada para proteger um ficheiro encriptado através de ataques por dicionário.

A atividade permitiu compreender a importância da utilização de palavras-passe fortes e resistentes a ataques automatizados.

![Evidência Tarefa 7](Tarefa%207.png)

---

## Tarefa 8 - Administração de Bases de Dados SQL

Foi efetuada a administração básica de uma base de dados MySQL/MariaDB.

Entre as operações realizadas destacam-se:

- arranque do serviço;
- autenticação local;
- seleção de bases de dados;
- consulta de tabelas;
- execução de comandos SQL;
- extração de informação através de instruções `SELECT`.

Esta atividade demonstrou a integração entre administração Linux e gestão de bases de dados.

![Evidência Tarefa 8](Tarefa%208.png)
![Evidência Tarefa 8.1](Tarefa%208.1.png)

---

## Tarefa 9 - Desafio Final e Elevação de Privilégios

A última atividade integrou todos os conhecimentos adquiridos durante o laboratório.

Foram realizadas operações de:

- análise de ficheiros de registo;
- identificação de credenciais;
- autenticação remota;
- exploração de privilégios;
- obtenção de acesso administrativo (root).

A conclusão desta tarefa demonstrou a capacidade de aplicar múltiplas técnicas de administração e auditoria de sistemas Linux de forma encadeada.

![Evidência Tarefa 9](Tarefa%209.png)

---

# Procedimento de Hardening do Serviço OpenSSH

Após a conclusão das atividades práticas procedeu-se ao reforço da configuração do serviço SSH.

O objetivo principal consistiu em reduzir a superfície de ataque do servidor e aumentar o nível de segurança das ligações remotas.

---

## Criação de um Utilizador Administrativo

Foi criado um utilizador dedicado denominado:

```text
adminuser
```

Esta abordagem evita a utilização direta da conta **root**, permitindo uma maior rastreabilidade das ações administrativas.

---

## Implementação de Chaves Ed25519

A autenticação baseada em palavra-passe foi substituída por um par de chaves criptográficas assimétricas **Ed25519**.

Foram igualmente aplicadas permissões restritivas:

| Diretório/Ficheiro | Permissões |
|--------------------|-----------:|
| ~/.ssh | 700 |
| authorized_keys | 600 |

Estas permissões impedem acessos não autorizados às credenciais do utilizador.

---

## Reconfiguração do ficheiro `sshd_config`

Foram alteradas diversas diretivas críticas do serviço SSH.

### Alteração da Porta

```text
Port 2222
```

A alteração da porta padrão reduz significativamente o volume de ataques automáticos provenientes de scanners e botnets.

---

### Desativação do Login Root

```text
PermitRootLogin no
```

Esta configuração impede qualquer autenticação direta através da conta root.

Todas as ações administrativas passam obrigatoriamente por um utilizador intermédio.

---

### Desativação da Autenticação por Palavra-passe

```text
PasswordAuthentication no
```

Esta medida elimina ataques de:

- força bruta;
- password spraying;
- ataques por dicionário.

A autenticação passa a ser efetuada exclusivamente através de chaves privadas.

---

## Validação da Configuração

Antes do reinício do serviço foi efetuada a validação da sintaxe do ficheiro de configuração.

Posteriormente o serviço SSH foi reiniciado e testado com sucesso utilizando:

- novo utilizador;
- nova porta;
- autenticação baseada em chave Ed25519.

---

# Matriz de Impacto na Segurança

| Controlo de Segurança | Ameaça Mitigada | Impacto |
|------------------------|-----------------|----------|
| Chaves Ed25519 | Ataques de força bruta e dicionário |  Crítico |
| PermitRootLogin no | Comprometimento direto da conta root |  Alto |
| PasswordAuthentication no | Roubo ou adivinhação de palavras-passe |  Crítico |
| Porta 2222 | Scanners automáticos e botnets |  Médio |
| Utilizador administrativo dedicado | Auditoria e rastreabilidade |  Alto |

---

# Boas Práticas Implementadas

Durante a realização da atividade foram aplicadas diversas recomendações de segurança utilizadas em ambientes profissionais.

Entre elas destacam-se:

- utilização de autenticação baseada em chaves criptográficas;
- eliminação da autenticação por palavra-passe;
- proibição do login direto do utilizador root;
- alteração da porta padrão do SSH;
- utilização de um utilizador administrativo dedicado;
- aplicação de permissões restritivas ao diretório `.ssh`;
- validação das configurações antes da ativação do serviço.

Estas medidas reduzem significativamente a superfície de ataque do servidor e aumentam a sua resiliência perante ameaças comuns.

---

# Conclusão

A realização da **Sessão 04 — Hardening de SSH e Auditoria de Sistemas Linux** permitiu consolidar conhecimentos fundamentais de administração segura de sistemas Linux.

A resolução integral do laboratório **Linux Strength Training**, disponibilizado pela plataforma TryHackMe, confirmou a capacidade de manipular o sistema de ficheiros, trabalhar com mecanismos criptográficos, administrar bases de dados e compreender técnicas básicas de auditoria de segurança.

![Conclusão da Sala](Conclusão.png)

Posteriormente foi efetuado o endurecimento do serviço **OpenSSH**, através da implementação de mecanismos modernos de autenticação e da eliminação de configurações consideradas inseguras.

As medidas implementadas encontram-se alinhadas com as boas práticas recomendadas para servidores Linux em ambientes de produção e demonstram a importância da configuração adequada dos serviços de acesso remoto na proteção da infraestrutura informática.

---

# Resumo Executivo

| Requisito | Estado |
|-----------|--------|
| Linux Strength Training |  Concluído |
| Navegação em Linux |  Concluída |
| Gestão de Ficheiros |  Concluída |
| Hashes Criptográficos |  Concluído |
| Base64 |  Concluído |
| GPG |  Concluído |
| John the Ripper |  Concluído |
| SQL |  Concluído |
| Elevação de Privilégios |  Concluída |
| Hardening SSH |  Implementado |
| Chaves Ed25519 |  Configuradas |
| Login Root Desativado |  Implementado |
| PasswordAuthentication Desativado |  Implementado |
| Porta SSH Alterada |  Implementada |
| Validação Final |  Concluída |

---

> **Resultado Final:** Todos os objetivos definidos para a Sessão 04 foram alcançados com sucesso. O ambiente foi configurado segundo práticas modernas de hardening do OpenSSH, garantindo maior segurança, rastreabilidade e resistência a ataques sobre serviços de acesso remoto.
