# Simulação de Ataques de Força Bruta e Password Spraying

# 1. Introdução

Este projeto foi desenvolvido no âmbito de um desafio prático da DIO, com o objetivo de compreender e simular ataques relacionados com mecanismos de autenticação, nomeadamente ataques de Brute Force, Dictionary Attack e Password Spraying.

Para a realização das experiências, foi utilizado o Kali Linux em conjunto com ferramentas de segurança, como Medusa, Enum4linux e cURL, num ambiente laboratorial controlado.

Durante o desenvolvimento do projeto, foram explorados três cenários:

- Ataque de força bruta contra um serviço FTP.
- Análise de segurança e testes de autenticação numa aplicação Web através de HTTP.
- Enumeração de utilizadores e testes de autenticação contra o serviço SMB.

Além da execução das simulações, foram analisados os resultados obtidos e identificados problemas relacionados com a utilização de credenciais fracas e a exposição de informações sensíveis.

O projeto procura demonstrar não apenas o funcionamento das técnicas utilizadas, mas também a importância da implementação de mecanismos de proteção contra este tipo de ameaças.

# 2. Ambiente de Laboratório

## 2.1. Configuração do ambiente

Para realizar as experiências, foi utilizado um ambiente virtualizado através do Oracle VirtualBox.

O objetivo foi criar um laboratório isolado, permitindo a comunicação entre as máquinas virtuais sem expor os serviços vulneráveis à rede externa.

## 2.2. Componentes utilizados

Componente

Kali Linux - Máquina utilizada para executar os testes
Metasploitable 2 - Sistema intencionalmente vulnerável
DVWA / aplicação Web vulnerável - Ambiente para testes de autenticação Web
Oracle VirtualBox - Criação e gestão das máquinas virtuais
Rede Host-only - Comunicação isolada entre máquinas virtuais

# 3. Desenvolvimento Prático

# 3.1. Primeiro Cenário — Ataque de Força Bruta em FTP

## Objetivo

Realizar uma simulação de ataque de força bruta contra o serviço FTP da máquina Metasploitable 2, utilizando a ferramenta Medusa para testar combinações de utilizadores e palavras-passe.

## Etapa 1 — Enumeração de portas

Inicialmente, foi realizada uma análise das portas da máquina de destino.

O objetivo desta etapa foi identificar os serviços disponíveis e verificar quais poderiam ser utilizados durante os testes.

A enumeração permitiu identificar a presença do serviço FTP.

**Procedimento:**

- Identificação do endereço IP da máquina de destino.
- Análise das portas disponíveis.
- Identificação dos serviços associados às portas encontradas.
- Seleção do serviço FTP para a experiência.

**Comando utilizado:**

nmap -sV -p 22,21,23,445,80 192.168.50.20

**Resultado obtido:**

O serviço FTP foi identificado como um dos serviços disponíveis na máquina de destino.

## Etapa 2 — Primeira tentativa de autenticação

Depois de identificar o serviço FTP, foi realizada uma tentativa manual de autenticação.

Foram utilizadas credenciais aleatórias para verificar se seria possível estabelecer uma ligação ao serviço.

O resultado foi negativo, uma vez que as credenciais introduzidas não correspondiam a uma conta válida.

Esta etapa permitiu confirmar que o serviço estava acessível, mas que as credenciais utilizadas não permitiam a autenticação.

## Etapa 3 — Preparação das Wordlists

Para automatizar as tentativas de autenticação, foram preparados dois ficheiros:

**users.txt**

Ficheiro responsável por armazenar os nomes de utilizadores que seriam utilizados nos testes.

**pass.txt**

Ficheiro responsável por armazenar as palavras-passe que seriam testadas.

As listas foram criadas com combinações simples, exclusivamente para a realização das experiências no laboratório.

## Etapa 4 — Execução do Medusa

Depois de preparar as wordlists, foi utilizada a ferramenta Medusa para realizar tentativas automatizadas de autenticação contra o serviço FTP.

A ferramenta testou as combinações fornecidas através dos ficheiros de utilizadores e palavras-passe.

**Comando utilizado:**

medusa -h 192.168.50.20 -U users.txt -P pass.txt -M ftp -t 10

**Resultado obtido:**

Durante a execução, o Medusa identificou uma combinação de credenciais válida.

|   Campo       | Resultado                 |
| --------------|---------------------------|
| Serviço       | FTP                       |
| Utilizador    | `msfadmin`                |
| Palavra-passe | `msfadmin`                |
| Resultado     | Credenciais identificadas |

## Etapa 5 — Validação das credenciais

Após a identificação das credenciais, foi realizada uma nova tentativa manual de autenticação no serviço FTP.

Desta vez, foram utilizadas as credenciais identificadas pelo Medusa.

O acesso foi estabelecido com sucesso, confirmando que a combinação encontrada permitia autenticar-se no serviço.

**Resultado final:** Autenticação FTP bem-sucedida.

## Análise do cenário FTP

A experiência demonstrou como a utilização de palavras-passe fracas e previsíveis pode facilitar a descoberta de credenciais através de ferramentas automatizadas.

Neste caso, a palavra-passe era igual ao nome de utilizador, o que representa uma prática inadequada de segurança.

Também foi possível compreender a importância da enumeração inicial, da preparação de wordlists e da validação manual dos resultados obtidos.

# 6.2. Segundo Cenário — Análise de Segurança e Testes de Autenticação Web

## Objetivo

Analisar a segurança de uma aplicação Web acessível através da porta 80 (HTTP), procurando identificar informações sensíveis expostas e realizar testes automatizados de autenticação utilizando a ferramenta Medusa.

## Etapa 1 — Acesso à aplicação Web

Inicialmente, foi identificado o serviço HTTP na porta 80 da máquina de destino.

Através do navegador, foi possível aceder à aplicação Web e analisar o seu conteúdo.

Esta etapa permitiu identificar o ponto de entrada utilizado para a realização dos testes seguintes.

## Etapa 2 — Análise do conteúdo com cURL

Durante a análise da aplicação, foi utilizada a ferramenta cURL para consultar o conteúdo disponibilizado pelo servidor Web.

Através da resposta HTTP recebida, foi possível examinar o código-fonte enviado ao cliente.

Durante esta análise, foram identificadas credenciais expostas diretamente no conteúdo da aplicação.

**Ferramenta utilizada:** cURL.

**Comando utilizado:**

curl 192.168.50.20/dvwa/login.php

**Resultado obtido:**

Foram encontradas credenciais no código disponibilizado pela aplicação Web.

Esta descoberta demonstrou uma falha de segurança relacionada com a exposição de informações sensíveis.

## Etapa 3 — Teste automatizado com Medusa

Apesar de as credenciais já terem sido identificadas através da análise do conteúdo Web, foi também realizada uma simulação de tentativas automatizadas de autenticação.

Para isso, foi utilizada a ferramenta Medusa, com recurso a wordlists.

O objetivo foi verificar se as credenciais identificadas através da análise do código também seriam reconhecidas durante os testes automatizados.

**Ferramenta utilizada:** Medusa.

**Comando utilizado:**

medusa -h 192.168.50.20 -U users.txt -P pass.txt -M http \ -m PAGE:'dvwa/login.php' \ -m FORM:'username=^USER^&password=^PASS^&Login=Login' \ -m 'Login failed' -t 10 -f

**Resultado obtido:**

Durante a execução, o Medusa identificou uma combinação de credenciais que permitiu obter autenticação bem-sucedida.

Uma das combinações identificadas correspondia às credenciais anteriormente encontradas no conteúdo da aplicação Web.

## Etapa 4 — Comparação e validação dos resultados

Após a realização dos dois procedimentos, foi possível comparar os resultados.

|   Método         | Resultado                 |
| -----------------|---------------------------|
| Análise com cURL | Credenciais expostas      |
| Teste com Medusa | Credenciais identificadas |
| Comparação       | Credenciais coincidentes  |
| Autenticação     | Bem-sucedida              |

Esta comparação demonstrou que a aplicação apresentava uma exposição de informação sensível e que as mesmas credenciais podiam ser utilizadas para autenticação.

## Análise do cenário Web

Este cenário foi particularmente interessante porque permitiu identificar uma fragilidade sem depender exclusivamente de tentativas automatizadas.

A análise do conteúdo disponibilizado pela aplicação revelou credenciais que não deveriam estar acessíveis ao utilizador.

A utilização do Medusa permitiu complementar a experiência e demonstrar que as mesmas credenciais também podiam ser identificadas através de testes automatizados.

É importante destacar que o cURL consulta o conteúdo disponibilizado pelo servidor ao cliente. Não permite, por si só, consultar ficheiros internos ou o código privado executado no servidor.

A experiência demonstrou a importância de proteger informações sensíveis e de nunca disponibilizar credenciais em HTML, JavaScript ou outros conteúdos enviados ao navegador.

# 6.3. Terceiro Cenário — Enumeração e Ataque de Dicionário em SMB

## Objetivo

Identificar contas existentes na máquina Metasploitable 2 e realizar testes controlados de autenticação contra o serviço SMB, utilizando Enum4linux e Medusa.

Neste cenário, foi utilizada uma abordagem baseada em wordlists, com uma quantidade reduzida de utilizadores e uma lista maior de palavras-passe.

## Etapa 1 — Enumeração de utilizadores com Enum4linux

Antes de realizar os testes de autenticação, foi necessário identificar quais as contas existentes no sistema de destino.

Para isso, foi utilizada a ferramenta Enum4linux.

A enumeração permitiu recolher informações sobre as contas disponíveis através do serviço SMB.

**Ferramenta utilizada:** Enum4linux.

**Comando utilizado:**

Enum4linux -a 192.168.50.20 ! tee enum4_output.txt

**Resultado obtido:**

A ferramenta executou a enumeração com sucesso e apresentou informações relativas às contas de utilizadores existentes no sistema.

Esta informação foi posteriormente utilizada na preparação dos testes de autenticação.

Após obter as informações sobre as contas existentes, foram preparados dois ficheiros:

**smb_users.txt**

Contém os nomes de utilizadores selecionados para a realização dos testes.

**senhas_spray.txt**

Contém uma lista de palavras-passe utilizadas nas tentativas de autenticação.

Neste cenário, foi utilizada uma quantidade reduzida de utilizadores e uma quantidade superior de palavras-passe.

As listas foram preparadas exclusivamente para o laboratório.

## Etapa 3 — Execução dos testes com Medusa

Depois da preparação das wordlists, foi utilizada a ferramenta Medusa para realizar tentativas automatizadas de autenticação contra o serviço SMB.

**Comando utilizado:**

medusa -h 192.168.50.20 -U smb_users.txt -P senhas_spray.txt -M smbnt -t 2 -T 50

## Etapa 4 — Resultados obtidos

Durante a execução, o Medusa apresentou um resultado positivo e identificou uma combinação de credenciais válida.

|  Campo                     | Resultado                 |
| ---------------------------|---------------------------|
| Serviço                    | SMB                       |
| Ferramenta de enumeração   | Enum4linux                |
| Ferramenta de autenticação | Medusa                    |
| Utilizador                 | `msfadmin`                |
| Palavra-passe              | `msfadmin`                |
| Resultado                  | Credenciais identificadas |

## Etapa 5 — Análise da técnica utilizada

A atividade realizada no serviço SMB correspondeu a uma simulação de Password Spraying. Foram enumeradas várias contas de utilizador através do Enum4linux e utilizadas wordlists distintas para os nomes de utilizador e palavras-passe.

O Medusa foi utilizado para testar as credenciais identificadas. Durante os testes, foi encontrada uma combinação válida: msfadmin:msfadmin.

Este resultado demonstra como a utilização de palavras-passe fracas ou previsíveis pode permitir a autenticação não autorizada numa conta, mesmo quando o serviço SMB está exposto na rede.

## Análise do cenário SMB

A experiência permitiu compreender a importância da enumeração de utilizadores durante uma avaliação de segurança.

A utilização do Enum4linux demonstrou como é possível obter informações sobre contas existentes num sistema através do serviço SMB.

Posteriormente, a utilização do Medusa permitiu testar as combinações presentes nas wordlists e identificar credenciais válidas.

Tal como aconteceu no cenário FTP, a utilização de uma palavra-passe igual ao nome de utilizador demonstrou os riscos associados à utilização de credenciais fracas e previsíveis.

# 7. Resultados Geraisz

Após a realização dos três cenários, foram obtidos os seguintes resultados:

| Cenário | Ferramentas                   | Resultado                                        |
|---------|-------------------------------|--------------------------------------------------|
| FTP     | Enumeração de portas e Medusa | Credenciais identificadas e acesso validado      |
| Web     | cURL e Medusa                 | Credenciais expostas e autenticação bem-sucedida |
| SMB     | Enum4linux e Medusa           | Contas enumeradas e credenciais identificadas    |

## 7.1. Principais descobertas

Durante as experiências, foram identificados diferentes problemas e comportamentos relacionados com a segurança da autenticação:

- Utilização de palavras-passe iguais aos nomes de utilizador.
- Possibilidade de identificar credenciais através de wordlists.
- Exposição de credenciais no conteúdo disponibilizado por uma aplicação Web.
- Possibilidade de enumerar contas existentes através do serviço SMB.
- Importância de validar os resultados apresentados pelas ferramentas.

## 7.2. Aprendizagens adquiridas

A realização deste projeto permitiu desenvolver conhecimentos práticos em diferentes áreas da segurança informática.

Entre as principais aprendizagens destacam-se:

- Utilização de ferramentas de enumeração.
- Identificação de serviços disponíveis numa máquina.
- Preparação e utilização de wordlists.
- Utilização do Medusa em diferentes serviços.
- Análise de respostas HTTP com cURL.
- Identificação de credenciais expostas.
- Compreensão dos riscos associados a palavras-passe fracas.
- Diferenciação entre técnicas de força bruta, ataques de dicionário e Password Spraying.

# 8. Medidas de Prevenção e Mitigação

Com base nas vulnerabilidades e comportamentos observados durante os testes, existem várias medidas que podem ser implementadas para reduzir os riscos associados a ataques de autenticação.

## 8.1. Utilização de palavras-passe fortes

As palavras-passe devem ser suficientemente longas, únicas e difíceis de adivinhar.

Deve ser evitada a utilização de palavras-passe iguais ao nome de utilizador, nomes próprios ou combinações previsíveis.

## 8.2. Autenticação Multifator (MFA)

A implementação de autenticação multifator acrescenta uma camada adicional de proteção.

Mesmo que uma palavra-passe seja descoberta, o atacante terá de ultrapassar um segundo mecanismo de autenticação.

## 8.3. Limitação de tentativas de autenticação

Os sistemas devem implementar mecanismos que limitem tentativas repetidas de login.

Esta medida dificulta a execução de ataques automatizados e reduz a possibilidade de descoberta de credenciais.

## 8.4. Monitorização e registo de eventos

É importante monitorizar tentativas de autenticação falhadas e identificar padrões suspeitos.

A análise dos registos pode ajudar a detetar comportamentos associados a ataques de força bruta e Password Spraying.

## 8.5. Proteção de informações sensíveis

As credenciais nunca devem ser disponibilizadas diretamente no código HTML, JavaScript ou em outros conteúdos enviados ao cliente.

As informações sensíveis devem ser armazenadas e geridas exclusivamente no lado do servidor, através de mecanismos adequados de proteção.

## 8.6. Proteção do serviço SMB

Devem ser aplicadas políticas de autenticação fortes, restrições de acesso e monitorização das ligações SMB.

Também é importante desativar versões e funcionalidades inseguras que já não sejam necessárias.

## 8.7. Utilização de protocolos seguros

Serviços que transmitam credenciais devem utilizar mecanismos de proteção adequados.

Por exemplo, deve evitar-se a utilização de FTP sem encriptação para transmitir credenciais, privilegiando alternativas seguras como SFTP ou FTPS.

# 9. Conclusão Geral

Ao longo deste projeto, foram realizadas experiências práticas relacionadas com a segurança dos mecanismos de autenticação nos serviços FTP, HTTP e SMB.

A utilização do Kali Linux e de ferramentas como Medusa, Enum4linux e cURL permitiu explorar diferentes métodos de identificação de credenciais e análise de vulnerabilidades num ambiente controlado.

No cenário FTP, foi possível identificar credenciais através de testes automatizados e validar posteriormente o acesso ao serviço.

No cenário Web, a análise do conteúdo disponibilizado pela aplicação permitiu identificar credenciais expostas. A utilização do Medusa complementou a experiência, demonstrando que as mesmas credenciais também podiam ser identificadas através de tentativas automatizadas.

No cenário SMB, a enumeração de utilizadores através do Enum4linux permitiu recolher informações sobre as contas existentes, que foram posteriormente utilizadas nos testes de autenticação com Medusa.

As experiências demonstraram que palavras-passe fracas, informações sensíveis expostas e mecanismos de autenticação sem proteção adequada podem representar riscos significativos para a segurança dos sistemas.

Além da componente prática, este projeto permitiu aprofundar conhecimentos sobre enumeração, autenticação, análise de serviços, utilização de ferramentas de segurança e medidas de mitigação.

Concluo que a compreensão destas técnicas é importante não apenas para identificar vulnerabilidades, mas também para desenvolver estratégias de prevenção e melhorar a segurança dos sistemas informáticos.

**Projeto desenvolvido como parte de um desafio prático da Digital Innovation One (DIO).**
