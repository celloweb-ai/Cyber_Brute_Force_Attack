# 🛡️ Projeto de Laboratório: Análise de Ataques de Força Bruta com Kali Linux e Medusa

Este repositório documenta a execução de um desafio prático da [Digital Innovation One (DIO)](https://www.dio.me/), focado em segurança ofensiva. O projeto consiste em simular ataques de força bruta em um ambiente de laboratório controlado para entender suas mecânicas, identificar vulnerabilidades e propor medidas de mitigação eficazes.

---

## ⚠️ AVISO LEGAL E DISCLAIMER

> ### 🚨 **ATENÇÃO: LEIA ANTES DE PROSSEGUIR** 🚨
>
> **Este repositório documenta técnicas de teste de penetração e segurança ofensiva. O uso inadequado pode violar leis locais, nacionais e internacionais.**
>
> ### 📋 Termos de Uso Obrigatórios:
>
> 1. **FINALIDADE EDUCACIONAL EXCLUSIVA**: Este projeto destina-se SOMENTE a fins educacionais, pesquisa de segurança, certificações profissionais (CEH, OSCP, etc.) e testes de penetração autorizados.
>
> 2. **PROIBIÇÕES EXPRESSAS**:
>    - ❌ É **ESTRITAMENTE PROIBIDO** realizar testes em sistemas sem autorização expressa, formal e por escrito do proprietário
>    - ❌ É **ESTRITAMENTE PROIBIDO** usar estas técnicas para acesso não autorizado, roubo de dados ou qualquer atividade ilegal
>    - ❌ É **ESTRITAMENTE PROIBIDO** executar ataques contra infraestrutura de terceiros sem contrato de pentest vigente
>    - ❌ É **ESTRITAMENTE PROIBIDO** usar este conhecimento para fins criminosos, antiéticos ou maliciosos
>
> 3. **RESPONSABILIDADE LEGAL**:
>    - O autor **NÃO SE RESPONSABILIZA** por qualquer uso indevido, dano a sistemas, violação de privacidade, perda de dados ou consequências legais decorrentes do uso destas técnicas
>    - O usuário assume **TOTAL E EXCLUSIVA RESPONSABILIDADE** por suas ações ao replicar ou adaptar este material
>    - Violações podem resultar em **PROCESSOS CRIMINAIS E CIVIS** conforme legislação brasileira (Lei 12.737/2012 - Lei Carolina Dieckmann, Lei 12.965/2014 - Marco Civil da Internet) e internacional
>
> 4. **REQUISITOS PARA USO LEGÍTIMO**:
>    - ✅ Execute APENAS em ambientes de laboratório controlados e completamente isolados da internet
>    - ✅ Utilize máquinas virtuais dedicadas (Metasploitable, DVWA, HackTheBox, TryHackMe, etc.)
>    - ✅ Obtenha autorização formal **POR ESCRITO** antes de testar qualquer sistema real
>    - ✅ Documente todos os testes com timestamps, escopo e autorizações para fins de auditoria
>    - ✅ Siga o código de ética de pentesters profissionais (EC-Council, SANS, OWASP)
>
> 5. **LEGISLAÇÃO APLICÁVEL**:
>    - **Brasil**: 
>      - Lei 12.737/2012 (Crimes Cibernéticos - "Lei Carolina Dieckmann")
>      - Lei 12.965/2014 (Marco Civil da Internet)
>      - Lei 13.709/2018 (LGPD - Proteção de Dados)
>      - Código Penal - Arts. 154-A e 154-B (Invasão de dispositivo informático)
>    - **Internacional**: 
>      - Computer Fraud and Abuse Act - CFAA (EUA)
>      - Computer Misuse Act (Reino Unido)
>      - Convenção de Budapeste sobre Cibercrime
>      - GDPR (União Europeia)
>
> 6. **PENALIDADES POSSÍVEIS**:
>    - Invasão não autorizada: **3 meses a 1 ano de detenção + multa** (Lei 12.737/2012)
>    - Obtenção de dados sem autorização: **6 meses a 2 anos de reclusão + multa**
>    - Interrupção de serviços: **1 a 3 anos de detenção + multa**
>    - Penas aumentam em caso de prejuízo econômico ou divulgação de dados
>
> ### ⚖️ ISENÇÃO DE RESPONSABILIDADE:
> 
> AO ACESSAR, BAIXAR OU UTILIZAR ESTE REPOSITÓRIO, VOCÊ DECLARA E CONCORDA QUE:
> - Leu e compreendeu integralmente este aviso legal e suas implicações
> - Compromete-se a usar o material apenas para fins educacionais e éticos legítimos
> - Possui conhecimento adequado sobre as leis de cibersegurança aplicáveis em sua jurisdição
> - Assume total responsabilidade civil e criminal por quaisquer consequências do uso destas técnicas
> - Reconhece que o autor não fornece suporte, consultoria ou assistência para atividades ilegais
> - Concorda em indenizar o autor de quaisquer reclamações decorrentes do seu uso deste material
>
> **Se você não concorda com TODOS estes termos, FECHE ESTE REPOSITÓRIO IMEDIATAMENTE e não utilize seu conteúdo de forma alguma.**
>
> ### 👮 PARA PROFISSIONAIS DE SEGURANÇA:
> 
> Se você é um profissional de segurança ou pentester:
> - Assegure-se de ter um **contrato formal de pentesting** ou **autorização por escrito** antes de realizar qualquer teste
> - Defina claramente o **escopo, cronograma e limites** dos testes com o cliente
> - Mantenha **documentação completa** de todas as atividades para fins de compliance
> - Siga as diretrizes do **NIST, OWASP, PTES** (Penetration Testing Execution Standard)
> - Preserve a **confidencialidade** de todas as informações descobertas durante os testes

---

## 🎯 1. Objetivo

O objetivo principal deste projeto é demonstrar a utilização da ferramenta **Medusa** no **Kali Linux** para realizar auditorias de segurança em diferentes serviços, explorando ambientes vulneráveis como o **Metasploitable 2** e o **DVWA** **EM AMBIENTE CONTROLADO E ISOLADO**. O processo foi inteiramente documentado para servir como um portfólio técnico e evidência de aprendizado.

---

## ⚙️ 2. Configuração do Ambiente de Laboratório

### ⚠️ IMPORTANTE: Ambiente Isolado Obrigatório

**TODOS os testes foram realizados em ambiente 100% isolado da internet e de redes de produção.**

Para garantir a segurança e o isolamento dos testes, foi configurado um ambiente de virtualização dedicado.

* **Sistema Operacional Atacante:** [Kali Linux](https://www.kali.org/get-kali/)
* **Sistema Operacional Alvo:** [Metasploitable 2](https://sourceforge.net/projects/metasploitable/)
* **Software de Virtualização:** [Oracle VM VirtualBox](https://www.virtualbox.org/wiki/Downloads)
* **Configuração de Rede:**
    * **Tipo:** Rede Apenas de Anfitrião (Host-Only)
    * **Justificativa:** Esta configuração cria uma rede privada e **COMPLETAMENTE ISOLADA**, permitindo que as máquinas virtuais ([Kali Linux](https://www.kali.org/get-kali/) e [Metasploitable 2](https://sourceforge.net/projects/metasploitable/)) se comuniquem entre si, **SEM EXPOR** os serviços vulneráveis à rede local ou à internet.
    * **Validação de Isolamento:** Verificado que as VMs NÃO possuem acesso externo via testes de ping para IPs públicos
* **Verificação de Conectividade:**
    * IP do Kali Linux: `192.168.56.101`
    * IP do Metasploitable 2: `192.168.56.102`
    * Teste de conectividade realizado com sucesso via comando `ping` **APENAS ENTRE AS VMs**

_Imagem da topologia de rede:_
<p align="center">
<img 
    src="https://github.com/celloweb-ai/brute_force_attack/blob/main/images/network-setup.png"
    width="400"  
/>
</p>

### 🔒 Medidas de Segurança Implementadas:
1. ✅ Rede configurada em modo **Host-Only** (sem acesso externo)
2. ✅ Firewall da máquina host configurado para bloquear tráfego das VMs
3. ✅ Snapshots criados antes de cada teste para fácil recuperação
4. ✅ Máquinas vulneráveis **NUNCA** conectadas à internet ou rede corporativa
5. ✅ Documentação completa de todos os testes com timestamps

---

## 💣 3. Execução dos Cenários de Ataque

### ⚠️ Nota Importante sobre Testes de Penetração:

Todos os cenários descritos abaixo foram executados **EXCLUSIVAMENTE** em:
- Ambientes de laboratório isolados
- Máquinas intencionalmente vulneráveis (Metasploitable 2, DVWA)
- Sem conexão com sistemas reais ou de produção
- Para fins educacionais e de demonstração de conceitos de segurança

Foram simulados três cenários de ataque distintos para avaliar a segurança de diferentes protocolos. As wordlists utilizadas foram criadas com base em credenciais comuns e podem ser encontradas neste repositório nos arquivos `usuarios.txt` e `senhas.txt`.

### Cenário 1: Força Bruta em Serviço FTP (vsftpd 2.3.4)

* **Objetivo:** Demonstrar como ataques de força bruta funcionam contra serviços FTP mal configurados
* **Ferramenta:** Medusa
* **Comando Executado:**
    ```bash
    medusa -h 192.168.56.102 -U usuarios.txt -P senhas.txt -M ftp -v 4
    ```
    * `-h`: Host alvo (Metasploitable 2 - **AMBIENTE DE LAB**)
    * `-U`: Caminho para a lista de usuários
    * `-P`: Caminho para a lista de senhas
    * `-M`: Módulo do serviço a ser atacado (`ftp`)
    * `-v 4`: Nível de verbosidade para detalhar o processo

* **Resultado:**
    * **Sucesso no ambiente de lab!** Acesso obtido com as credenciais padrão: `usuário: msfadmin`, `senha: msfadmin`.
    * **Evidência:**
        ![Sucesso FTP](https://github.com/celloweb-ai/Cyber_Brute_Force_Attack/blob/main/images/ftp-success.png)

---

### Cenário 2: Força Bruta em Formulário Web (DVWA)

* **Objetivo:** Demonstrar vulnerabilidades em autenticação web sem proteções adequadas
* **Ambiente Alvo:** Damn Vulnerable Web Application (DVWA) com nível de segurança **Low** - **APLICATIVO INTENCIONALMENTE VULNERÁVEL**
* **Ferramenta:** Medusa
* **Comando Executado:**
    ```bash
    medusa -h 192.168.56.102 -u admin -P senhas.txt -M http -m DIR=/dvwa/login.php -m FORM-DATA="username=%USER%&password=%PASS%&Login=Login" -v 4
    ```
    * `-u admin`: Foca o ataque no usuário `admin`
    * `-M http`: Módulo para o protocolo HTTP
    * `-m DIR`: Diretório do script de login
    * `-m FORM-DATA`: Define o payload da requisição POST, onde `%USER%` e `%PASS%` são as variáveis que o Medusa substitui a cada tentativa

* **Resultado:**
    * **Sucesso no ambiente de lab!** Acesso obtido com as credenciais padrão: `usuário: admin`, `senha: password`.
    * **Evidência:**
        ![Sucesso DVWA](https://github.com/celloweb-ai/Cyber_Brute_Force_Attack/blob/main/images/dvwa-success.png)

---

### Cenário 3: Password Spraying em Serviço SMB

* **Objetivo:** Demonstrar técnicas de password spraying que são mais discretas que brute force tradicional
* **Técnica:** Password Spraying (uma única senha testada contra múltiplos usuários)
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
    * `-U`: Lista de usuários enumerados
    * `-p msfadmin`: Testa a **senha única** `msfadmin` contra todos os usuários
    * `-M smbnt`: Módulo para o protocolo SMB

* **Resultado:**
    * **Sucesso no ambiente de lab!** Acesso obtido para o usuário `msfadmin` com a senha `msfadmin`.
    * **Evidência:**
        ![Sucesso SMB](https://github.com/celloweb-ai/Cyber_Brute_Force_Attack/blob/main/images/smb-success.png)

---

## 🛡️ 4. Recomendações de Mitigação

Com base nas vulnerabilidades exploradas **EM AMBIENTE CONTROLADO**, as seguintes contramedidas são essenciais para fortalecer a segurança dos serviços:

### Defesas Contra Ataques de Força Bruta:

1.  **Política de Senhas Fortes:** 
    - Complexidade mínima: 12+ caracteres, maiúsculas, minúsculas, números e símbolos
    - Verificar contra listas de senhas comprometidas (Have I Been Pwned)
    - Implementar validação de entropia de senha

2.  **Mecanismo de Bloqueio de Contas (Account Lockout):** 
    - Bloquear contas após 3-5 tentativas falhas consecutivas
    - Implementar bloqueio progressivo (aumentar tempo a cada tentativa)
    - Alertar administradores sobre bloqueios suspeitos

3.  **Autenticação Multifator (MFA/2FA):** 
    - **Implementar MFA em TODOS os serviços críticos**
    - Utilizar TOTP, push notifications ou tokens de hardware
    - MFA torna ataques de força bruta praticamente inúteis

4.  **Rate Limiting e Throttling:**
    - Limitar número de tentativas por IP (ex: 5 por minuto)
    - Implementar delays progressivos entre tentativas
    - Bloquear IPs com comportamento suspeito

5.  **Monitoramento e Detecção:**
    - IDS/IPS para detectar padrões de força bruta
    - SIEM para correlacionar eventos de segurança
    - Alertas automáticos para tentativas de login suspeitas
    - Análise de logs com ferramentas como ELK Stack

6.  **CAPTCHA para Aplicações Web:** 
    - Implementar reCAPTCHA v3 em formulários de login
    - Ativar desafios adicionais após falhas de login
    - Dificultar automação de ataques

7.  **Segurança de Credenciais:**
    - **NUNCA** utilizar credenciais padrão em produção
    - Alterar TODAS as senhas de fábrica imediatamente
    - Usar gerenciadores de senhas corporativos
    - Implementar rotação periódica de senhas para contas críticas

8.  **Controles de Rede:**
    - Segmentação de rede (VLANs)
    - Firewall com whitelist de IPs autorizados
    - VPN obrigatória para acesso remoto
    - Desabilitar serviços desnecessários

9.  **Auditoria e Compliance:**
    - Auditorias regulares de segurança
    - Testes de penetração periódicos **AUTORIZADOS**
    - Conformidade com frameworks (NIST, ISO 27001, CIS Controls)

---

## 🎯 5. Conclusão e Aprendizados

A execução deste projeto prático **EM AMBIENTE CONTROLADO E ISOLADO** foi extremamente valiosa para consolidar o conhecimento teórico sobre ataques de força bruta. Ficou evidente que senhas fracas e configurações padrão representam um risco de segurança crítico e são um dos vetores de ataque mais comuns e eficazes.

### Principais Lições:

1. **Defesa em Profundidade**: Uma única falha (senha fraca) pode comprometer todo um sistema. Múltiplas camadas de segurança são essenciais.

2. **Credenciais Padrão**: Sistemas com credenciais de fábrica são extremamente vulneráveis e facilmente explorados.

3. **MFA como Barreira**: Autenticação multifator é a defesa mais eficaz contra ataques de força bruta.

4. **Monitoramento É Crucial**: Detectar e responder rapidamente a tentativas de ataque é tão importante quanto preveni-las.

5. **Ética e Legalidade**: Sempre obter autorização formal antes de realizar testes de segurança. Conhecimento de segurança ofensiva deve ser usado exclusivamente para fins legítimos e éticos.

### Aplicabilidade Profissional:

Este conhecimento é fundamental para:
- Profissionais de segurança da informação
- Pentesters e auditores de segurança
- Administradores de sistemas e redes
- Desenvolvedores focados em secure coding
- Analistas de SOC (Security Operations Center)

---

## 📚 Recursos Educacionais Recomendados

### Plataformas de Prática Legal:
- [HackTheBox](https://www.hackthebox.eu/) - Ambientes legais de hacking
- [TryHackMe](https://tryhackme.com/) - Laboratórios guiados de cibersegurança
- [OverTheWire](https://overthewire.org/) - War games para aprendizado
- [VulnHub](https://www.vulnhub.com/) - VMs vulneráveis para prática
- [PentesterLab](https://pentesterlab.com/) - Exercícios de pentesting

### Certificações Profissionais:
- CEH (Certified Ethical Hacker) - EC-Council
- OSCP (Offensive Security Certified Professional)
- GPEN (GIAC Penetration Tester) - SANS
- CompTIA PenTest+

### Documentação e Frameworks:
- [OWASP Testing Guide](https://owasp.org/www-project-web-security-testing-guide/)
- [PTES - Penetration Testing Execution Standard](http://www.pentest-standard.org/)
- [NIST Cybersecurity Framework](https://www.nist.gov/cyberframework)
- [Kali Linux Documentation](https://www.kali.org/docs/)

### Legislação Brasileira:
- [Lei 12.737/2012](http://www.planalto.gov.br/ccivil_03/_ato2011-2014/2012/lei/l12737.htm) - Crimes Cibernéticos
- [Lei 12.965/2014](http://www.planalto.gov.br/ccivil_03/_ato2011-2014/2014/lei/l12965.htm) - Marco Civil da Internet
- [Lei 13.709/2018](http://www.planalto.gov.br/ccivil_03/_ato2015-2018/2018/lei/l13709.htm) - LGPD

---

## 📧 Relatar Uso Indevido

Se você identificar uso malicioso ou não autorizado das técnicas descritas neste repositório:

1. **No Brasil**: Reporte ao [CERT.br](https://www.cert.br/) ou autoridades competentes
2. **Empresas**: Entre em contato com equipes de segurança corporativas
3. **Internacional**: Reporte às autoridades locais de crimes cibernéticos

---

**Autor**

Marcus Vasconcellos  
Profissional de Segurança da Informação e Automação Industrial

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/marcusvasconcellos/)
[![Perfil DIO](https://img.shields.io/badge/DIO-20232A?style=for-the-badge)](https://web.dio.me/users/marcus_60890)

---

### 🔒 Nota Final de Segurança

Este repositório foi criado com propósitos estritamente educacionais. O conhecimento de segurança ofensiva é uma ferramenta poderosa que deve ser usada com responsabilidade e ética. 

**"Com grandes poderes vêm grandes responsabilidades."**

Sempre priorize a ética, legalidade e segurança em suas atividades profissionais.

---

**Última atualização**: Janeiro 2026  
**Versão do documento**: 2.0 (com aviso legal completo)