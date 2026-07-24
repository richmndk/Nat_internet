# Nat_internet

objectif: connecter mon réseau local à internet


j'ai ajouté un serveur web 
un routeur ici qui est mon FAI

je configure le serveur web 

ip 8.8.8.8
masque :255.0.0.0
passerlle:8.1.1.1

je configure le routeur FAI 

enable
configure terminal
hostname FAI

je configure l'interface vers le routeur principal
interface gigabitethernet O/1
ip address 200.100.100.2 255.255.255.252 
no shutdown
exit

je configure l'interface vers le serveur web public

interface gigabitethernet 0/1 
ip address 8.1.1.1 255.0.0.0
no shutdown
exit



je configure le NAT sur le routeur principal de l'entreprise 

en 
conf t
je configure IP interface WAN

interface gigabiteternet 0/1
ip address 200.100.100.1 255.255.255.252
no shutdown
exit

interface externe pour NAT 

interaface gigabitethernet 0/1
ip nat outside
exit

interface interne pour NAT

ip nat inside
exit
interface gigabitethernet 0/0.10
ip nat inside 
exit

interface gigabitethernet 0/0.20
ip nat inside 
exit


je crée les accès ACl pour le trafic à traduire

access-list 1 permit 192.168.10.0 0.0.0.255
access-list 1 permit 192.168.20.0 0.0.0.255

activer la traduction dynamique overload

ip nat inside source list 1 interface gigabitethernet 0/1 overload

je configure la route de la passerelle

ip route 0.0.0.0 0.0.0.0 200.100.100.2
end 
write



