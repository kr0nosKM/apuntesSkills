#cisco #vlan

# VLANs

## INTRODUCCIÓN

Las VLANs se configuran siempre en los SWITCHES.

## TIPOS DE INTERFACES EN VLANs

- ACCESS (puerto de acceso)
- TRUNK   (puerto troncal)

### ACCESS
- Hay que **indicar** a **que interfaz** pertenecen
- Todas las PDUs que lleguen a un puerto access, serán **etiquetadas** con la VLAN configurada en el **puerto / interfaz**
- Las PDUs que salgan por un puerto de **ACCESO** serán **des**etiquetadas. Se quedarán según el formato estándar (sin VLAN) para que los dispositivos finales la puedean procesar
- Peticiones **UNICAST**: Cuando llega a un puerto de acceso, la PDU se etiqueta con la VLAN corrrespondiente
- Peticiones **BROADCAST**: Cuando llega a un puerto de acceso, se etiqueta con la VLAN correspondiente.

### TRUNK
- Hay que indicar que PDUs podrán salir a través de la interfaz configurada, en función de la VLAN indicada en la PDU
- **PETICIONES** (mismo proceso en los dos casos):
- Peticiones **UNICAST**: 
	- Cuando llega por el puerto troncal, no se realiza ningún cambio en la PDU
	- La PDU es reenviada por todos los puertos ASOCIADOS a la VLAN de acceso y troncales que estén asociados o permitan la VLAN de la PDU
- Peticiones **BROADCAST**:
	- Cuando llega por el puerto troncal, no se hace nigún cambio en la PDU
	- La PDU es reenviada por todos los puertos asociados a la VLAN de la PDU, de acceso y troncales que estén asociados o permital la VLAN de la pdu


