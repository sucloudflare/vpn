
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

