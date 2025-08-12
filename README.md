
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
