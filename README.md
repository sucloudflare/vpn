
<h1>Configuração de VPN com WireGuard</h1>
<p>Este guia explica como configurar um servidor WireGuard e adicionar clientes à sua rede VPN. O WireGuard é uma VPN moderna, rápida e fácil de configurar. Abaixo estão as instruções para configurar o servidor e os clientes.</p>

<div class="section">
<h2>Pré-requisitos</h2>
<ul>
<li>Sistema operacional: Linux (Debian, Ubuntu, Kali)</li>
<li>Acesso root ou sudo</li>
<li>WireGuard instalado no servidor e nos clientes</li>
</ul>
</div>

<div class="section">
<h2>Instalando o WireGuard</h2>
<p>Se ainda não tiver o WireGuard instalado, use os seguintes comandos para instalá-lo:</p>
<pre><code># Instalar WireGuard no Debian/Ubuntu/Kali
sudo apt update
sudo apt install wireguard wireguard-tools</code></pre>
</div>

<div class="section">
<h2>Configuração do Servidor</h2>
<h3>1. Gerar as chaves</h3>
<p>Primeiro, gere as chaves privadas e públicas para o servidor.</p>
<pre><code># Gerar chave privada do servidor
sudo wg genkey | sudo tee /etc/wireguard/privatekey
sudo chmod 600 /etc/wireguard/privatekey

# Gerar chave pública do servidor
sudo cat /etc/wireguard/privatekey | sudo wg pubkey | sudo tee /etc/wireguard/publickey</code></pre>

<h3>2. Configuração do arquivo wg0.conf</h3>
<p>Crie o arquivo de configuração do servidor (/etc/wireguard/wg0.conf):</p>
<pre><code>sudo nano /etc/wireguard/wg0.conf</code></pre>
<p>Conteúdo do arquivo wg0.conf:</p>
<pre><code>[Interface]
Address = 10.0.0.1/24         # Endereço IP do servidor na VPN
PrivateKey = SUA_CHAVE_PRIVADA_AQUI
ListenPort = 51820            # Porta de escuta do servidor

[Peer]
PublicKey = CHAVE_PUBLICA_DO_CLIENTE
AllowedIPs = 10.0.0.2/32      # IP do cliente</code></pre>
<p>Substitua SUA_CHAVE_PRIVADA_AQUI pela chave privada gerada anteriormente.</p>
<p>Substitua CHAVE_PUBLICA_DO_CLIENTE pela chave pública do cliente (gerada no processo de configuração do cliente).</p>

<h3>3. Subir a interface</h3>
<p>Suba a interface WireGuard com o seguinte comando:</p>
<pre><code>sudo wg-quick up wg0</code></pre>
<p>Para verificar se a interface está ativa:</p>
<pre><code>sudo wg show</code></pre>
</div>

<div class="section">
h2>Configuração do Cliente</h2>
<h3>1. Gerar as chaves no cliente</h3>
<p>Repita o processo de geração de chaves no cliente:</p>
<pre><code># Gerar chave privada do cliente
sudo wg genkey | sudo tee /etc/wireguard/privatekey
sudo chmod 600 /etc/wireguard/privatekey

# Gerar chave pública do cliente
sudo cat /etc/wireguard/privatekey | sudo wg pubkey | sudo tee /etc/wireguard/publickey</code></pre>

<h3>2. Configuração do arquivo wg0.conf no cliente</h3>
<p>Crie o arquivo de configuração do cliente:</p>
<pre><code>sudo nano /etc/wireguard/wg0.conf</code></pre>
<p>Conteúdo do arquivo wg0.conf do cliente:</p>
<pre><code>[Interface]
Address = 10.0.0.2/32         # Endereço IP do cliente na VPN
PrivateKey = SUA_CHAVE_PRIVADA_AQUI
DNS = 1.1.1.1                 # (Opcional) Defina um servidor DNS

[Peer]
PublicKey = CHAVE_PUBLICA_DO_SERVIDOR
Endpoint = IP_DO_SERVIDOR:51820
AllowedIPs = 0.0.0.0/0        # Roteia todo o tráfego via VPN
PersistentKeepalive = 25      # Mantém a conexão ativa</code></pre>
<p>Substitua SUA_CHAVE_PRIVADA_AQUI pela chave privada gerada para o cliente.</p>
<p>Substitua CHAVE_PUBLICA_DO_SERVIDOR pela chave pública do servidor.</p>
<p>Substitua IP_DO_SERVIDOR pelo IP público ou local do servidor WireGuard.</p>

<h3>3. Subir a interface</h3>
<p>Suba a interface WireGuard no cliente:</p>
<pre><code>sudo wg-quick up wg0</code>
<code>sudo systemctl enable wg-quick@wg0
</code></pre>
</div>

<div class="section">
<h2>Comandos Úteis</h2>
<ul>
<li><code>sudo wg-quick up wg0</code> - Subir a interface</li>
<li><code>sudo wg-quick down wg0</code> - Descer a interface</li>
<li><code>sudo wg show</code> - Verificar status da interface</li>
<li><code>sudo journalctl -u wg-quick@wg0</code> - Verificar logs do WireGuard</li>
</ul>
</div>

<div class="section">
<h2>Resumo</h2>
<p>Servidor: Crie chaves, configure o arquivo /etc/wireguard/wg0.conf, suba a interface.</p>
<p>Cliente: Crie chaves, configure o arquivo /etc/wireguard/wg0.conf, suba a interface.</p>
<p>Conecte-se à VPN e verifique se o tráfego está sendo roteado corretamente.</p>
<p>Agora, o seu servidor WireGuard deve estar configurado e pronto para uso, com os clientes podendo se conectar à rede VPN usando as chaves e configurações adequadas.</p>
</div>

<h1>/Atualização:</h1>

<h1>Configuração Completa do WireGuard VPN (Servidor e Cliente)</h1>

  <nav>
    <a href="#requisitos">Requisitos</a>
    <a href="#instalacao">Instalação do WireGuard</a>
    <a href="#config-servidor">Configuração do Servidor</a>
    <a href="#config-cliente">Configuração do Cliente</a>
    <a href="#ativando">Ativando a VPN</a>
    <a href="#testando">Testando a Conexão</a>
    <a href="#boot">Ativar Automaticamente no Boot</a>
    <a href="#comandos">Comandos Úteis</a>
    <a href="#solucao">Solução de Problemas</a>
  </nav>

  <section id="requisitos">
    <h2>Requisitos</h2>
    <ul>
      <li>Sistema operacional Linux (Debian, Ubuntu, Kali, etc)</li>
      <li>Acesso root ou sudo no servidor e cliente</li>
      <li>Conexão à internet para instalar pacotes</li>
    </ul>
  </section>

  <section id="instalacao">
    <h2>Instalação do WireGuard</h2>
    <p>Execute os comandos abaixo no servidor e no cliente:</p>
    <pre><code>sudo apt update
sudo apt install wireguard wireguard-tools
</code></pre>
  </section>

  <section id="config-servidor">
    <h2>Configuração do Servidor WireGuard</h2>

  <h3>1. Gerar chaves no servidor</h3>
    <pre><code>sudo wg genkey | sudo tee /etc/wireguard/privatekey
sudo chmod 600 /etc/wireguard/privatekey

sudo cat /etc/wireguard/privatekey | sudo wg pubkey | sudo tee /etc/wireguard/publickey
</code></pre>

   <h3>2. Criar o arquivo de configuração <code>/etc/wireguard/wg0.conf</code></h3>
    <pre><code>sudo nano /etc/wireguard/wg0.conf
</code></pre>
    <p>Cole e edite substituindo as chaves geradas:</p>
    <pre><code>[Interface]
Address = 10.0.0.1/24          # IP da VPN para o servidor
PrivateKey = &lt;CHAVE_PRIVADA_DO_SERVIDOR&gt;
ListenPort = 51820             # Porta UDP que o WireGuard usará

# Cliente 1
[Peer]
PublicKey = &lt;CHAVE_PUBLICA_DO_CLIENTE&gt;
AllowedIPs = 10.0.0.2/32       # IP que o cliente terá na VPN
</code></pre>

  <h3>3. Habilitar encaminhamento de pacotes (IP forwarding)</h3>
    <pre><code>sudo sysctl -w net.ipv4.ip_forward=1
</code></pre>
    <p>Para tornar permanente, adicione no arquivo <code>/etc/sysctl.conf</code> (caso exista):</p>
    <pre><code>sudo nano /etc/sysctl.conf
</code></pre>
    <p>Adicione ou descomente a linha:</p>
    <pre><code>net.ipv4.ip_forward=1
</code></pre>

  <h3>4. Configurar firewall para liberar UDP na porta 51820</h3>
    <p>Se usar <code>ufw</code>:</p>
    <pre><code>sudo ufw allow 51820/udp
</code></pre>
    <p>Se usar <code>iptables</code>:</p>
    <pre><code>sudo iptables -A INPUT -p udp --dport 51820 -j ACCEPT
</code></pre>
  </section>

  <section id="config-cliente">
    <h2>Configuração do Cliente WireGuard</h2>
    <h3>1. Gerar chaves no cliente</h3>
    <pre><code>sudo wg genkey | sudo tee /etc/wireguard/privatekey
sudo chmod 600 /etc/wireguard/privatekey

sudo cat /etc/wireguard/privatekey | sudo wg pubkey | sudo tee /etc/wireguard/publickey
</code></pre>

  <h3>2. Criar arquivo de configuração <code>/etc/wireguard/wg0.conf</code></h3>
    <pre><code>sudo nano /etc/wireguard/wg0.conf
</code></pre>
    <p>Exemplo de conteúdo, substitua as chaves e IP do servidor:</p>
    <pre><code>[Interface]
Address = 10.0.0.2/32          # IP do cliente na VPN
PrivateKey = &lt;CHAVE_PRIVADA_DO_CLIENTE&gt;
DNS = 1.1.1.1                 # Opcional: DNS para usar

[Peer]
PublicKey = &lt;CHAVE_PUBLICA_DO_SERVIDOR&gt;
Endpoint = &lt;IP_PÚBLICO_DO_SERVIDOR&gt;:51820
AllowedIPs = 0.0.0.0/0        # Roteia todo o tráfego pela VPN
PersistentKeepalive = 25      # Mantém conexão ativa atrás de NAT
</code></pre>
  </section>

  <section id="ativando">
    <h2>Ativando a VPN</h2>
    <p>No servidor e no cliente, suba a interface:</p>
    <pre><code>sudo wg-quick up wg0
</code></pre>
    <p>Para derrubar a interface:</p>
    <pre><code>sudo wg-quick down wg0
</code></pre>
  </section>

  <section id="testando">
    <h2>Testando a Conexão</h2>
    <p>No cliente, rode:</p>
    <pre><code>ping 10.0.0.1
</code></pre>
    <p>No servidor, rode:</p>
    <pre><code>sudo wg show
</code></pre>
    <p>Para ver as conexões ativas.</p>
  </section>

  <section id="boot">
    <h2>Configuração para Ativar Automaticamente no Boot</h2>
    <p>No servidor e no cliente, execute:</p>
    <pre><code>sudo systemctl enable wg-quick@wg0
</code></pre>
    <p>Isso fará com que o WireGuard levante a interface <code>wg0</code> automaticamente ao iniciar o sistema.</p>
  </section>

  <section id="comandos">
    <h2>Comandos Úteis</h2>
    <ul>
      <li><strong>Ver status da interface WireGuard:</strong>
        <pre><code>sudo wg show</code></pre>
      </li>
      <li><strong>Ver logs do serviço WireGuard:</strong>
        <pre><code>sudo journalctl -u wg-quick@wg0</code></pre>
      </li>
      <li><strong>Reiniciar serviço WireGuard:</strong>
        <pre><code>sudo systemctl restart wg-quick@wg0</code></pre>
      </li>
      <li><strong>Desativar interface WireGuard:</strong>
        <pre><code>sudo wg-quick down wg0</code></pre>
      </li>
    </ul>
  </section>

  <section id="solucao">
    <h2>Solução de Problemas</h2>
    <ul>
      <li><strong>Erro ao subir wg0:</strong> Verifique se o arquivo <code>/etc/wireguard/wg0.conf</code> existe e está correto.</li>
      <li><strong>Firewall bloqueando conexão:</strong> Certifique-se de liberar a porta UDP 51820.</li>
      <li><strong>IP forwarding não funcionando:</strong> Verifique se <code>net.ipv4.ip_forward=1</code> está ativo com <code>sysctl net.ipv4.ip_forward</code>.</li>
      <li><strong>Cliente não conecta:</strong> Confira se o IP do servidor e as chaves estão corretas no cliente.</li>
    </ul>
  </section>



<h1>WireGuard — Instalação e Configuração Permanente</h1>
    <p class="lead">Guia passo a passo para instalar, configurar e garantir que a VPN WireGuard suba no boot e tente reconectar automaticamente se cair. Funciona em Debian/Ubuntu/Kali.</p>

   <nav>      <a href="#install">Instalação</a>
      <a href="#config">Configurar wg0</a>
      <a href="#firewall">Firewall</a>
      <a href="#autostart">Auto-start</a>
      <a href="#autorestart">Auto-restart</a>
      <a href="#test">Testes</a>
    </nav>

   <section id="install">
      <h2>1. Instalar WireGuard e dependências</h2>
      <p>Rode no servidor e no cliente:</p>
      <pre><code>sudo apt update
sudo apt install wireguard wireguard-tools netfilter-persistent iptables-persistent -y</code></pre>
      <p class="note"><strong>Nota:</strong> O pacote <code>iptables-persistent</code> permite salvar regras de iptables entre reboots.</p>
    </section>
    <section id="config">
      <h2>2. Configuração da interface (<code>/etc/wireguard/wg0.conf</code>)</h2>
      <h3>2.1 Gerar chaves</h3>
      <pre><code>sudo wg genkey | sudo tee /etc/wireguard/privatekey
sudo chmod 600 /etc/wireguard/privatekey
sudo cat /etc/wireguard/privatekey | sudo wg pubkey | sudo tee /etc/wireguard/publickey</code></pre>
      <h3>2.2 Exemplo de arquivo servidor</h3>
      <pre><code># /etc/wireguard/wg0.conf (servidor)
[Interface]
Address = 10.0.0.1/24
ListenPort = 51820
PrivateKey = SUA_CHAVE_PRIVADA_DO_SERVIDOR_AQUI

# Peer: cliente 1
[Peer]
PublicKey = CHAVE_PUBLICA_DO_CLIENTE
AllowedIPs = 10.0.0.2/32</code></pre>

  <h3>2.3 Exemplo de arquivo cliente</h3>
      <pre><code># /etc/wireguard/wg0.conf (cliente)
[Interface]
Address = 10.0.0.2/32
PrivateKey = CHAVE_PRIVADA_DO_CLIENTE_AQUI
DNS = 1.1.1.1

[Peer]
PublicKey = CHAVE_PUBLICA_DO_SERVIDOR
Endpoint = IP_PUBLICO_DO_SERVIDOR:51820
AllowedIPs = 0.0.0.0/0
PersistentKeepalive = 25</code></pre>

   <p class="note"><strong>Importante:</strong> Substitua as chaves e o <code>IP_PUBLICO_DO_SERVIDOR</code> pelos valores reais.</p>
      <h3>2.4 Permissões</h3>
      <pre><code>sudo chmod 600 /etc/wireguard/wg0.conf</code></pre>
    </section>

   <section id="firewall">
      <h2>3. Firewall — abrir porta UDP</h2>
      <p>No servidor (abrir porta WireGuard):</p>
      <pre><code>sudo iptables -A INPUT -p udp --dport 51820 -j ACCEPT
sudo netfilter-persistent save</code></pre>
      <p class="note">Se você usa <code>ufw</code> (não instalado por padrão no Kali): <code>sudo ufw allow 51820/udp</code>.</p>

   <h3>Habilitar IP forwarding (servidor)</h3>
      <pre><code>sudo sysctl -w net.ipv4.ip_forward=1
# Tornar persistente:
echo "net.ipv4.ip_forward=1" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p</code></pre>
    </section>

   <section id="autostart">
      <h2>4. Habilitar para iniciar no boot</h2>
      <p>Ative o serviço systemd que executa o <code>wg-quick</code> e sobe a interface no boot:</p>
      <pre><code>sudo systemctl enable wg-quick@wg0
# iniciar agora:
sudo systemctl start wg-quick@wg0</code></pre>
      <p>Verificar status:</p>
      <pre><code>sudo systemctl status wg-quick@wg0</code></pre>
      <p class="note">O serviço <code>wg-quick@wg0</code> normalmente aparece como <code>active (exited)</code> — isto é normal; ele configura a interface e encerra. O importante é que a interface exista (<code>ip a show wg0</code>).</p>
    </section>

  <section id="autorestart">
      <h2>5. Fazer o serviço tentar reconectar automaticamente (se cair)</h2>
      <p>Adicione opções de reinício via <code>systemd</code>:</p>
      <pre><code>sudo systemctl edit wg-quick@wg0</code></pre>
      <p>No editor que abrir, cole:</p>
      <pre><code>[Service]
Restart=always
RestartSec=10</code></pre>
      <p>Salvar e aplicar:</p>
      <pre><code>sudo systemctl daemon-reexec
sudo systemctl restart wg-quick@wg0</code></pre>
      <p class="note">Com isto, se o processo de subida do túnel falhar, o systemd tentará novamente a cada 10 segundos.</p>
    </section>

   <section id="test">
      <h2>6. Testes e comandos úteis</h2>

   <ul>
        <li><strong>Ver interface e IP</strong>:
          <pre><code>ip a show wg0</code></pre>
      </li>
        <li><strong>Ver status e peers do WireGuard</strong>:
          <pre><code>sudo wg show</code></pre>
        </li>
        <li><strong>Subir / Descer manualmente</strong>:
          <pre><code>sudo wg-quick up wg0
sudo wg-quick down wg0</code></pre>
        </li>
        <li><strong>Ver logs do serviço</strong>:
          <pre><code>sudo journalctl -u wg-quick@wg0 -e</code></pre>
        </li>
        <li><strong>Testar IP público (antes/depois)</strong>:
          <pre><code>curl ifconfig.me
curl ipinfo.io/ip</code></pre>
        </li>
      </ul>
    </section>
    <section id="troubleshoot">
      <h2>7. Solução de problemas (comuns)</h2>
      <ul>
        <li><strong>Interface não aparece:</strong> rode <code>sudo wg-quick up wg0</code> e verifique logs (<code>journalctl</code>).</li>
        <li><strong>Chaves inválidas:</strong> confirme conteúdo de <code>/etc/wireguard/privatekey</code> e publickey correspondentes.</li>
        <li><strong>Roteamento/Internet não funciona:</strong> verifique <code>net.ipv4.ip_forward=1</code> e regra NAT (mas atenção se for servidor de produção — configurar <code>iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE</code> se necessário).</li>
        <li><strong>Cliente cai frequentemente:</strong> adicione <code>PersistentKeepalive = 25</code> no cliente.</li>
        <li><strong>Porta bloqueada:</strong> verifique com <code>nmap -sU -p 51820 IP_DO_SERVIDOR</code> e libere no firewall/ISP/roteador.</li>
      </ul>
    </section>

   <section id="final">
      <h2>8. Script opcional (automatizar tudo)</h2>
      <p>Se quiser, eu posso gerar um script que:</p>
      <ol>
        <li>Gera chaves automaticamente;</li>
        <li>Cria <code>/etc/wireguard/wg0.conf</code> do servidor;</li>
        <li>Configura ip_forward e regras iptables;</li>
      <li>Habilita e reinicia o serviço com restart automático.</li>
      </ol>
      <p>Diga se quer que eu gere esse script pronto para você executar.</p>
    </section>

  <footer>
      <p>Se preferir, posso transformar este HTML em um arquivo para download (pronto para salvar) ou gerar o script automático mencionado acima. Quer que eu gere o script também?</p>
      <p style="margin-top:8px;">Criado para: <strong>WireGuard on Kali/Debian/Ubuntu</strong> — substitua chaves e IPs pelo seus valores reais antes de usar em produção.</p>
    </footer>
