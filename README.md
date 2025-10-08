# 🛡️ Projeto de Laboratório: Análise de Ataques de Força Bruta com Kali Linux e Medusa

Este repositório documenta a execução de um desafio prático da [Digital Innovation One (DIO)](https://www.dio.me/), focado em segurança ofensiva. O projeto consiste em simular ataques de força bruta em um ambiente de laboratório controlado para entender suas mecânicas, identificar vulnerabilidades e propor medidas de mitigação eficazes.

## 🎯 1. Objetivo

O objetivo principal deste projeto é demonstrar a utilização da ferramenta **Medusa** no **Kali Linux** para realizar auditorias de segurança em diferentes serviços, explorando ambientes vulneráveis como o **Metasploitable 2** e o **DVWA**. O processo foi inteiramente documentado para servir como um portfólio técnico e evidência de aprendizado.

---

## ⚙️ 2. Configuração do Ambiente de Laboratório

Para garantir a segurança e o isolamento dos testes, foi configurado um ambiente de virtualização dedicado.

* **Sistema Operacional Atacante:** Kali Linux (`<VERSÃO DO KALI>`)
* **Sistema Operacional Alvo:** Metasploitable 2
* **Software de Virtualização:** Oracle VM VirtualBox (`<VERSÃO DO VIRTUALBOX>`)
* **Configuração de Rede:**
    * **Tipo:** Rede Apenas de Anfitrião (Host-Only)
    * **Justificativa:** Esta configuração cria uma rede privada e isolada, permitindo que as máquinas virtuais (Kali e Metasploitable 2) se comuniquem entre si, sem expor os serviços vulneráveis à rede local ou à internet.
* **Verificação de Conectividade:**
    * IP do Kali Linux: `192.168.56.101`
    * IP do Metasploitable 2: `192.168.56.102`
    * Teste de conectividade realizado com sucesso via comando `ping`.

_Imagem da topologia de rede:_
<p align="center">
<img 
    src="https://github.com/celloweb-ai/brute_force_attack/blob/main/images/network-setup.png"
    width="400"  
/>
</p>

---

## 💣 3. Execução dos Cenários de Ataque

Foram simulados três cenários de ataque distintos para avaliar a segurança de diferentes protocolos. As wordlists utilizadas foram criadas com base em credenciais comuns e podem ser encontradas neste repositório nos arquivos `usuarios.txt` e `senhas.txt`.

### Cenário 1: Força Bruta em Serviço FTP (vsftpd 2.3.4)

* **Objetivo:** Obter acesso não autorizado ao serviço FTP, que frequentemente contém arquivos sensíveis.
* **Ferramenta:** Medusa
* **Comando Executado:**
    ```bash
    medusa -h 192.168.56.102 -U usuarios.txt -P senhas.txt -M ftp -v 4
    ```
    * `-h`: Host alvo.
    * `-U`: Caminho para a lista de usuários.
    * `-P`: Caminho para a lista de senhas.
    * `-M`: Módulo do serviço a ser atacado (`ftp`).
    * `-v 4`: Nível de verbosidade para detalhar o processo.

* **Resultado:**
    * **Sucesso!** Acesso obtido com as credenciais: `usuário: msfadmin`, `senha: msfadmin`.
    * **Evidência:**
        `![Sucesso FTP](./images/ftp-success.png)`

---

### Cenário 2: Força Bruta em Formulário Web (DVWA)

* **Objetivo:** Comprometer uma aplicação web explorando um formulário de login com senhas fracas.
* **Ambiente Alvo:** Damn Vulnerable Web Application (DVWA) com nível de segurança **Low**.
* **Ferramenta:** Medusa
* **Comando Executado:**
    ```bash
    medusa -h 192.168.56.102 -u admin -P senhas.txt -M http -m DIR=/dvwa/login.php -m FORM-DATA="username=%USER%&password=%PASS%&Login=Login" -v 4
    ```
    * `-u admin`: Foca o ataque no usuário `admin`.
    * `-M http`: Módulo para o protocolo HTTP.
    * `-m DIR`: Diretório do script de login.
    * `-m FORM-DATA`: Define o payload da requisição POST, onde `%USER%` e `%PASS%` são as variáveis que o Medusa substitui a cada tentativa.

* **Resultado:**
    * **Sucesso!** Acesso obtido com as credenciais: `usuário: admin`, `senha: password`.
    * **Evidência:**
        `![Sucesso DVWA](./images/dvwa-success.png)`

---

### Cenário 3: Password Spraying em Serviço SMB

* **Objetivo:** Identificar contas com senhas fracas em um serviço de compartilhamento de arquivos (SMB), utilizando uma abordagem mais discreta que o brute force tradicional.
* **Técnica:** Password Spraying (uma única senha testada contra múltiplos usuários).
* **Ferramentas:** `nmap` (para enumeração) e `medusa`.

1.  **Passo 1: Enumeração de Usuários com Nmap**
    ```bash
    nmap --script smb-enum-users.nse -p 445 192.168.56.102
    ```
    * Este comando utilizou um script do Nmap para listar os usuários disponíveis no serviço SMB. A lista foi salva em `usuarios_smb.txt`.

2.  **Passo 2: Execução do Password Spraying com Medusa**
    ```bash
    medusa -h 192.168.56.102 -U usuarios_smb.txt -p msfadmin -M smbnt
    ```
    * `-U`: Lista de usuários enumerados.
    * `-p msfadmin`: Testa a **senha única** `msfadmin` contra todos os usuários.
    * `-M smbnt`: Módulo para o protocolo SMB.

* **Resultado:**
    * **Sucesso!** Acesso obtido para o usuário `msfadmin` com a senha `msfadmin`.
    * **Evidência:**
        `![Sucesso SMB](./images/smb-success.png)`

---

##  mitigation 4. Recomendações de Mitigação

Com base nas vulnerabilidades exploradas, as seguintes contramedidas são essenciais para fortalecer a segurança dos serviços:

1.  **Política de Senhas Fortes:** Implementar requisitos de complexidade (caracteres especiais, maiúsculas, minúsculas, números) e comprimento mínimo (12+ caracteres) para todas as credenciais.

2.  **Mecanismo de Bloqueio de Contas (Account Lockout):** Bloquear contas de usuários automaticamente após um número limitado de tentativas de login falhas (ex: 5 tentativas em 15 minutos).

3.  **Autenticação Multifator (MFA):** Habilitar MFA em todos os serviços críticos. Mesmo que uma senha seja comprometida, o acesso não autorizado é impedido pela exigência de um segundo fator.

4.  **Monitoramento e Alertas:** Utilizar sistemas de detecção de intrusão (IDS) e SIEM para monitorar logs de autenticação e gerar alertas para atividades suspeitas, como um alto volume de falhas de login de um único IP.

5.  **CAPTCHA para Aplicações Web:** Implementar CAPTCHA ou reCAPTCHA em formulários de login para diferenciar usuários humanos de bots automatizados, tornando ataques de força bruta em escala inviáveis.

6.  **Desabilitar Contas Padrão:** Nunca utilizar credenciais padrão em produção. Alterar todas as senhas de contas de serviço e de administrador durante a configuração inicial.

---

## 🎓 5. Conclusão e Aprendizados

A execução deste projeto prático foi extremamente valiosa para consolidar o conhecimento teórico sobre ataques de força bruta. Ficou evidente que senhas fracas e configurações padrão representam um risco de segurança crítico e são um dos vetores de ataque mais comuns e eficazes.

A principal lição aprendida é que a segurança é um processo em camadas. Uma única falha, como uma senha fraca, pode comprometer todo um sistema. Portanto, a implementação de múltiplas barreiras de defesa é fundamental para garantir a resiliência contra ameaças.

---

**Autor**

[Marcus Vasconcellos]

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/marcusvasconcellos/)
[![Perfil DIO](https://img.shields.io/badge/DIO-20232A?style=for-the-badge)](https://web.dio.me/users/seu-usuario-aqui)
