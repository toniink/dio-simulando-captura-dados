# RELATORIO DE ESTUDO E SIMULACAO DE MALWARES EM PYTHON

---

## OBJETIVO DO PROJETO
Este projeto tem como objetivo documentar o estudo e a simulação do comportamento de malwares (Keylogger e Ransomware) desenvolvidos em Python. Com finalidade estritamente educacional, buscando compreender a anatomia destas ameaças no nivel do codigo, como elas exfiltram e sequestram dados, e, principalmente, estruturar o pensamento defensivo para detectar e mitigar esses ataques em ambientes corporativos.

## ARQUITETURA DO AMBIENTE DE TESTES
- Sistema Operacional Base: Kali Linux (Ambiente Virtualizado).
- Linguagem Utilizada: Python 3 com ambientes virtuais (.venv) isolados.
- Bibliotecas Principais: pynput (para interacao com hardware/teclado) e cryptography (estudo teorico de criptografia simetrica).

## DESENVOLVIMENTO E ANALISE TECONOLOGICA

### PARTE 1: RANSOMWARE 
O Ransomware ataca a disponibilidade dos dados da vitima. O estudo teórico da anatomia deste malware em Python revelou que ele opera em três fases distintas:
- Geração da Chave: Utilizando a biblioteca cryptography (modulo Fernet), o malware gera uma chave de criptografia simétrica.
- Varredura (Mapeamento): O script utiliza a biblioteca nativa "os" para percorrer os diretórios do sistema buscando arquivos críticos baseados em suas extensões (.pdf, .docx, .jpg).
- Criptografia: O malware abre os arquivos originais, aplica a chave Fernet embaralhando os bytes, e salva a versão sobrescrita, tornando a leitura humana impossível sem a chave de resgate.

### PARTE 2: KEYLOGGER
A implementação prática focou na construção de um Keylogger local. Utilizando a biblioteca pynput, o script foi programado para instanciar um Listener que captura as interrupções de hardware do teclado e registrando cada tecla pressionada pela vítima em um arquivo de log local.

Para simular táticas de evasão e furtividade, explorou-se a execucao do processo em background. Em ambientes Windows, isso é possível alterando a extensão do arquivo para .pyw. No laboratorio em Linux, a furtividade foi alcancada manipulando o processo via terminal para rodar de forma invisivel e desvinculada da sessão ativa do usuario usando o nohup.

#### Exfiltracao de Dados (Estudo Teorico):
A etapa de exfiltração foi apenas aprofundada conceitualmente. Malwares tradicionais utilizam a biblioteca smtplib para envio via E-mail (SMTP). No entanto, em cenários de ataques modernos, cibercriminosos utilizam Webhooks via requisicoes HTTP POST (porta 443 - HTTPS). Como firewalls corporativos raramente bloqueiam trafego HTTPS de saida, o malware consegue enviar os dados roubados disfarçados de trafego web comum, burlando as defesas de perimetro.

## ESTRATÉGIAS DE DEFESA E MITIGAÇÃO
A compreensão de como as ameaças sao construídas permite arquitetar defesas sólidas. As principais contramedidas documentadas neste estudo são:

- Antivirus Tradicional e EDR: Antivírus clássicos operam baseados em "Assinaturas" (hashes conhecidos). Como um malware feito em Python no laboratorio e um codigo "inedito", ele passaria despercebido por essa camada. A defesa corporativa moderna é feita com o uso de EDR (Endpoint Detection and Response). O EDR atua por analise heuristica e de comportamento. Ele seria capaz de detectar que um processo desconhecido do Python começou a ler o buffer do teclado (Keylogger) ou começou a alterar arquivos em massa (Ransomware), isolando a máquina da rede instantaneamente.

- Políticas de Backup Offline: A unica defesa absoluta contra a perda de dados por Ransomware é a implementação da regra de backup 3-2-1, garantindo que pelo menos uma copia dos dados essenciais esteja armazenada de forma totalmente offline e desconectada da rede principal.

- O Fator Humano (Conscientização): A barreira de seguranca mais forte e, simultaneamente, a mais frágil, é o usuario. Treinamentos constantes de conscientização sobre engenharia social e phishing são vitais, pois o melhor sistema de seguranca existente pode ser burlado se o usuario decidir executar voluntariamente um arquivo desconhecido baixado da internet.
