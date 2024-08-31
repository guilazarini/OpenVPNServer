# OpenVPNServer
Instalar OpenVPN em uma VPS
A ideia dessa vpn é que ele "tunele" apenas os IPs especificados no config para que você economize banda e não tenha problema ao assistir videos do youtube ou netflix por exemplo.

Fazer Update do sistema CentOS:
sudo yum update -y
Fazer Update do sistema Ubuntu:
sudo apt update 
Baixar o script instalador
Script no GitHub:

wget https://raw.githubusercontent.com/Angristan/openvpn-install/master/openvpn-install.sh -O openvpn-install.sh
Dar poder de execução para o script
chmod +x openvpn-install.sh
Executar o script para instalar
Usar porta random, o resto pode ser default:

./openvpn-install.sh
Editar o config do OpenVPN
Comentar as linhas que possuem "push" e adicionar as linhas abaixo:

vi /etc/openvpn/server.conf
Adicionar a abaixo no config:

push "route-nopull"
Agora aqui adicione todos os IPs dos sites que você quer que a VPN "tunele":

push "route 104.244.42.0 255.255.255.0"
push "route 199.59.148.0 255.255.255.0"
No caso acima coloquei os IPs do Twitter que eu peguei aqui https://www.netify.ai/resources/applications/twitter

Reiniciar o servidor e verificar status
Se ele não tiver sido reiniciado, matar o servidor com o comando kill:

systemctl restart openvpn@server
systemctl status openvpn@server
Instalar Firewalld para segurança caso você esteja no Ubuntu
sudo apt -y install firewalld
Desabilitar o firewall ufw caso exista
sudo ufw disable
Configurar Firewalld
Adicionando a porta escolhida pelo OpenVPN(Não esqueça de substituir "PORTA" pela porta que o instalador ou você escolheram na hora da instalacão do OpenVPNServer):

sudo systemctl enable firewalld
sudo systemctl start firewalld
sudo firewall-cmd --zone=public --permanent --add-port=PORTA/udp
sudo firewall-cmd --zone=public --permanent --add-masquerade
sudo firewall-cmd --reload
Para criar/remover usuários
Usar o mesmo script de instalação:

./openvpn-install.sh
Copie o arquivo gerado .ovpn de cliente para a sua máquina
Na sua máquina
Baixe o cliente de acordo com o seu OS https://openvpn.net/client/

Conecte na sua VPN usando o arquivo .ovpn gerado
Navegue feliz
