# Ejercicio RIPv2
## Topología
![Topology](topo_ripv2.png)
## Configuración IP de los routers
Se deben configurar las interfaces asignandoles su dirección IP junto con la Subnet Mask y después levantarlas adminitrativamente
#### Router MIRAGE
```
MIRAGE#config t
MIRAGE(config)#int fa0/0
MIRAGE(config-if)#ip address 192.168.1.1 255.255.255.0
MIRAGE(config-if)#no shutdown
MIRAGE(config-if)#exit

MIRAGE(config)#int fa1/0
MIRAGE(config-if)#ip address 192.168.4.1 255.255.255.252
MIRAGE(config-if)#no shutdown
```
#### Router VERTIGO
```
VERTIGO#config t
VERTIGO(config)#int fa 0/0
VERTIGO(config-if)#ip address 192.168.3.1 255.255.255.0
VERTIGO(config-if)#no shutdown
VERTIGO(config-if)#exit

VERTIGO(config)#int fa 2/0
VERTIGO(config-if)#ip address 192.168.4.5 255.255.255.252
VERTIGO(config-if)#no shutdown
```
#### Router OVERPASS
```
OVERPASS#config t
OVERPASS(config)#int fa0/0
OVERPASS(config-if)#ip address 192.168.2.1 255.255.255.0
OVERPASS(config-if)#no shutdown
OVERPASS(config-if)#exit

OVERPASS(config)#int fa1/0
OVERPASS(config-if)#ip address 192.168.4.2 255.255.255.252
OVERPASS(config-if)#no shutdown
OVERPASS(config-if)#exit

OVERPASS(config)#int fa2/0
OVERPASS(config-if)#ip address 192.168.4.6 255.255.255.252
OVERPASS(config-if)#no shutdown
```

## Tablas de Ruteo
#### Router MIRAGE
```
MIRAGE#show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       + - replicated route, % - next hop override

Gateway of last resort is not set

      192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks
C        192.168.1.0/24 is directly connected, FastEthernet0/0
L        192.168.1.1/32 is directly connected, FastEthernet0/0
      192.168.4.0/24 is variably subnetted, 2 subnets, 2 masks
C        192.168.4.0/30 is directly connected, FastEthernet1/0
L        192.168.4.1/32 is directly connected, FastEthernet1/0
```
#### Router VERTIGO
```
VERTIGO#show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       + - replicated route, % - next hop override

Gateway of last resort is not set

      192.168.3.0/24 is variably subnetted, 2 subnets, 2 masks
C        192.168.3.0/24 is directly connected, FastEthernet0/0
L        192.168.3.1/32 is directly connected, FastEthernet0/0
      192.168.4.0/24 is variably subnetted, 2 subnets, 2 masks
C        192.168.4.4/30 is directly connected, FastEthernet2/0
L        192.168.4.5/32 is directly connected, FastEthernet2/0
```
#### Router OVERPASS
```
OVERPASS#show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       + - replicated route, % - next hop override

Gateway of last resort is not set

      192.168.2.0/24 is variably subnetted, 2 subnets, 2 masks
C        192.168.2.0/24 is directly connected, FastEthernet0/0
L        192.168.2.1/32 is directly connected, FastEthernet0/0
      192.168.4.0/24 is variably subnetted, 4 subnets, 2 masks
C        192.168.4.0/30 is directly connected, FastEthernet1/0
L        192.168.4.2/32 is directly connected, FastEthernet1/0
C        192.168.4.4/30 is directly connected, FastEthernet2/0
L        192.168.4.6/32 is directly connected, FastEthernet2/0
```

## Configuración RIPv2 de los routers
#### Router MIRAGE
```
MIRAGE#config t
MIRAGE(config)#router rip
MIRAGE(config-router)#version 2
MIRAGE(config-router)#network 192.168.1.0
MIRAGE(config-router)#network 192.168.4.0
```
#### Router VERTIGO
```
VERTIGO#config t
VERTIGO(config)#router rip
VERTIGO(config-router)#version 2
VERTIGO(config-router)#network 192.168.3.0
VERTIGO(config-router)#network 192.168.4.4
```

#### Router OVERPASS
```
OVERPASS#config t
OVERPASS(config)#router rip
OVERPASS(config-router)#version 2
OVERPASS(config-router)#network 192.168.2.0
OVERPASS(config-router)#network 192.168.4.0
OVERPASS(config-router)#network 192.168.4.4
```
## Tabla de Ruteo con RIPv2 en funcionamiento
#### Router MIRAGE
```
MIRAGE#sh ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       + - replicated route, % - next hop override

Gateway of last resort is not set

      192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks
C        192.168.1.0/24 is directly connected, FastEthernet0/0
L        192.168.1.1/32 is directly connected, FastEthernet0/0
R     192.168.2.0/24 [120/1] via 192.168.4.2, 00:00:16, FastEthernet1/0
R     192.168.3.0/24 [120/2] via 192.168.4.2, 00:00:06, FastEthernet1/0
      192.168.4.0/24 is variably subnetted, 3 subnets, 2 masks
C        192.168.4.0/30 is directly connected, FastEthernet1/0
L        192.168.4.1/32 is directly connected, FastEthernet1/0
R        192.168.4.4/30 [120/1] via 192.168.4.2, 00:00:16, FastEthernet1/0
```
#### Router VERTIGO
```
VERTIGO#sh ip route                               
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       + - replicated route, % - next hop override

Gateway of last resort is not set

R     192.168.1.0/24 [120/2] via 192.168.4.6, 00:00:31, FastEthernet2/0
R     192.168.2.0/24 [120/1] via 192.168.4.6, 00:00:31, FastEthernet2/0
      192.168.3.0/24 is variably subnetted, 2 subnets, 2 masks
C        192.168.3.0/24 is directly connected, FastEthernet0/0
L        192.168.3.1/32 is directly connected, FastEthernet0/0
      192.168.4.0/24 is variably subnetted, 3 subnets, 2 masks
R        192.168.4.0/30 [120/1] via 192.168.4.6, 00:00:31, FastEthernet2/0
C        192.168.4.4/30 is directly connected, FastEthernet2/0
L        192.168.4.5/32 is directly connected, FastEthernet2/0
```

#### Router OVERPASS
```
OVERPASS#sh ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       + - replicated route, % - next hop override

Gateway of last resort is not set

R     192.168.1.0/24 [120/1] via 192.168.4.1, 00:00:03, FastEthernet1/0
      192.168.2.0/24 is variably subnetted, 2 subnets, 2 masks
C        192.168.2.0/24 is directly connected, FastEthernet0/0
L        192.168.2.1/32 is directly connected, FastEthernet0/0
R     192.168.3.0/24 [120/1] via 192.168.4.5, 00:00:19, FastEthernet2/0
      192.168.4.0/24 is variably subnetted, 4 subnets, 2 masks
C        192.168.4.0/30 is directly connected, FastEthernet1/0
L        192.168.4.2/32 is directly connected, FastEthernet1/0
C        192.168.4.4/30 is directly connected, FastEthernet2/0
L        192.168.4.6/32 is directly connected, FastEthernet2/0


```
