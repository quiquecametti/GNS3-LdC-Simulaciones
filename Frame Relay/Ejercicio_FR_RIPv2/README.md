# 18/09/2020
# Ejercicio Frame Relay
Se conectan los routers como se muestran en la siguiente figura

![Topology](FR_topo.png)

Se separan las interfaces en subinterfaces lógicas para poder comunicarme con distintas redes a través la misma interfaz física. En cada subinterfaz voy a tener una IP y un DLCI.

***Defino tantas subinterfaces lógicas como DLCI tengo***
## Configuración Router ARRIBA
```
ARRIBA#config t
ARRIBA(config)#int s1/0
ARRIBA(config-if)#clock rate 64000
ARRIBA(config-if)#encapsulation frame-relay ietf
ARRIBA(config-if)#frame-relay intf-type dte
ARRIBA(config-if)#no shutdown
ARRIBA(config-if)#exit

ARRIBA(config)#int s1/0.1 point-to-point
ARRIBA(config-subif)#ip address 192.168.1.1 255.255.255.252
ARRIBA(config-subif)#frame-relay interface-dlci 101
ARRIBA(config-fr-dlci)#exit
ARRIBA(config-subif)#exit

ARRIBA(config)#line vty 0 4
ARRIBA(config-line)#password uca
ARRIBA(config-line)#transport input all
ARRIBA(config-line)#login
ARRIBA(config-line)#exit
ARRIBA(config)#exit

ARRIBA#write
```
## Configuración Router SWITCH
```
SWITCH#config t
SWITCH(config)#frame-relay switching 
SWITCH(config)#int s1/0
SWITCH(config-if)#encapsulation frame-relay ietf
SWITCH(config-if)#frame-relay intf-type dce 
SWITCH(config-if)#frame-relay route 101 interface s1/1 100
SWITCH(config-if)#clock rate 64000
SWITCH(config-if)#no shutdown 
SWITCH(config-if)#exit

SWITCH(config)#int s1/1
SWITCH(config-if)#encapsulation frame-relay ietf
SWITCH(config-if)#frame-relay intf-type dce 
SWITCH(config-if)#frame-relay route 100 interface s1/0 101
SWITCH(config-if)#clock rate 64000
SWITCH(config-if)#no shutdown 
SWITCH(config-if)#exit
SWITCH(config)#exit

SWITCH#write
```
## Configuración Router ABAJO
```
ABAJO(config)#int s1/0
ABAJO(config-if)#clock rate 64000
ABAJO(config-if)#encapsulation frame-relay ietf
ABAJO(config-if)#frame-relay intf-type dte
ABAJO(config-if)#no shutdown
ABAJO(config-if)#exit

ABAJO(config)#int s1/0.1 point-to-point
ABAJO(config-subif)#ip address 192.168.1.2 255.255.255.252
ABAJO(config-subif)#frame-relay interface-dlci 100
ABAJO(config-fr-dlci)#exit
ABAJO(config-subif)#exit

ABAJO(config)#line vty 0 4
ABAJO(config-line)#password uca
ABAJO(config-line)#transport input all
ABAJO(config-line)#login
ABAJO(config-line)#exit
ABAJO(config)#exit

ABAJO#write
```

# 24/09/2020
## **sh frame pvc**
Muestra las interfaces activas muestra datos de la transmision y la recepcion. En ARRIBA y ABAJO son locales en SWITCH son switcheados
```
ARRIBA#sh frame pvc

PVC Statistics for interface Serial1/0 (Frame Relay DTE)

              Active     Inactive      Deleted       Static
  Local          1            0            0            0
  Switched       0            0            0            0
  Unused         0            0            0            0

DLCI = 101, DLCI USAGE = LOCAL, PVC STATUS = ACTIVE, INTERFACE = Serial1/0.1

  input pkts 3             output pkts 6            in bytes 1044      
  out bytes 2094           dropped pkts 0           in pkts dropped 0         
  out pkts dropped 0                out bytes dropped 0         
  in FECN pkts 0           in BECN pkts 0           out FECN pkts 0         
  out BECN pkts 0          in DE pkts 0             out DE pkts 0         
  out bcast pkts 6         out bcast bytes 2094      
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
  pvc create time 00:00:44, last time pvc status changed 00:00:25
```
```
SWITCH#sh frame pvc

PVC Statistics for interface Serial1/0 (Frame Relay DCE)

              Active     Inactive      Deleted       Static
  Local          0            0            0            0
  Switched       1            0            0            0
  Unused         0            0            0            0

DLCI = 101, DLCI USAGE = SWITCHED, PVC STATUS = ACTIVE, INTERFACE = Serial1/0

  input pkts 8             output pkts 5            in bytes 2792      
  out bytes 1740           dropped pkts 0           in pkts dropped 0         
  out pkts dropped 0                out bytes dropped 0         
  in FECN pkts 0           in BECN pkts 0           out FECN pkts 0         
  out BECN pkts 0          in DE pkts 0             out DE pkts 0         
  out bcast pkts 0         out bcast bytes 0         
  30 second input rate 0 bits/sec, 0 packets/sec
  30 second output rate 0 bits/sec, 0 packets/sec
  switched pkts 5         
  Detailed packet drop counters:
  no out intf 0            out intf down 0          no out PVC 0         
  in PVC down 0            out PVC down 0           pkt too big 0         
  shaping Q full 0         pkt above DE 0           policing drop 0         
  connected to interface Serial1/1 100
  pvc create time 00:01:59, last time pvc status changed 00:01:49

PVC Statistics for interface Serial1/1 (Frame Relay DCE)

              Active     Inactive      Deleted       Static
  Local          0            0            0            0
  Switched       1            0            0            0
  Unused         0            0            0            0

DLCI = 100, DLCI USAGE = SWITCHED, PVC STATUS = ACTIVE, INTERFACE = Serial1/1

  input pkts 8             output pkts 5            in bytes 2784      
  out bytes 1745           dropped pkts 0           in pkts dropped 0         
  out pkts dropped 0                out bytes dropped 0         
  in FECN pkts 0           in BECN pkts 0           out FECN pkts 0         
  out BECN pkts 0          in DE pkts 0             out DE pkts 0         
  out bcast pkts 0         out bcast bytes 0         
  30 second input rate 0 bits/sec, 0 packets/sec
  30 second output rate 0 bits/sec, 0 packets/sec
  switched pkts 5         
  Detailed packet drop counters:
  no out intf 0            out intf down 0          no out PVC 0         
  in PVC down 0            out PVC down 0           pkt too big 0         
  shaping Q full 0         pkt above DE 0           policing drop 0         
  connected to interface Serial1/0 101
  pvc create time 00:01:59, last time pvc status changed 00:01:49
```

## **debug frame-relay packet** 
Permite ver paquetes que envia y recibe. Con el ping muestra 5 salientes (ECHO REQUEST) y 5 entrantes (ECHO REPLY).

Se ve el tráfico de DATOS pero no de STATUS porque esto no muestra tráfico del protocolo LMI

ping (ICMP) -> IP (0x3CC) -> Frame-Relay (0x1851) [El hexa es el identificador de protocolo]

telnet -> TCP -> IP (0x3CC) -> Frame-Relay (0x1851)

```
ARRIBA#debug frame-relay packet 
ARRIBA#ping 192.168.1.2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.1.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 56/73/84 ms

*Sep 24 20:26:44.635: Serial1/0.1(o): dlci 101(0x1851), NLPID 0x3CC(IP), datagramsize 104
*Sep 24 20:26:44.715: Serial1/0(i): dlci 101(0x1851), NLPID 0x3CC(IP), datagramsize 104
*Sep 24 20:26:44.719: Serial1/0.1(o): dlci 101(0x1851), NLPID 0x3CC(IP), datagramsize 104
*Sep 24 20:26:44.771: Serial1/0(i): dlci 101(0x1851), NLPID 0x3CC(IP), datagramsize 104
*Sep 24 20:26:44.779: Serial1/0.1(o): dlci 101(0x1851), NLPID 0x3CC(IP), datagramsize 104
*Sep 24 20:26:44.847: Serial1/0(i): dlci 101(0x1851), NLPID 0x3CC(IP), datagramsize 104
*Sep 24 20:26:44.851: Serial1/0.1(o): dlci 101(0x1851), NLPID 0x3CC(IP), datagramsize 104
*Sep 24 20:26:44.927: Serial1/0(i): dlci 101(0x1851), NLPID 0x3CC(IP), datagramsize 104
*Sep 24 20:26:44.931: Serial1/0.1(o): dlci 101(0x1851), NLPID 0x3CC(IP), datagramsize 104
*Sep 24 20:26:45.003: Serial1/0(i): dlci 101(0x1851), NLPID 0x3CC(IP), datagramsize 104
```
## **debug frame-relay lmi** 
Muestra los paquetes STATUS

## **frame-relay lmi-type**
Parmite cambiar el protocolo LMI. Se utiliza el comando en la interfaz física.

## **no keepalive**
No manda los keepalive. Se utiliza el comando en la interfaz física. Tiene que ser igual en el lado usuario (ARRIBA/ABAJO) y en el lado red (SWITCH)

# Consigna
Crear otro PVC entre ARRIBA y SWITCH