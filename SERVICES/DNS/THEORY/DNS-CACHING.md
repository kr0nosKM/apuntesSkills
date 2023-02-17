TAGS: #dns #caching #theory

## EXPLANATION

Normalmente también es conocido como (DNS resolver cache). 

La función principal del caché DNS es que después de que el cliente resuelva por primera vez una dirección DNS, el caché lo que hará será almacenar esa dirección DNS con la ip pública correspondiente.

> Normalmente los equipos ya tienen un DNS caché local instalado, para mejorar el rendimiento de la resolución de DNSs

TTL (time to live) es el límite de saltos (hops) entre nodos que tiene un paquete. También indica la vida de este paquete, si el contador llegase a cero, los paquetes correspondientes serían descartados, normalmente es únicamente un paquete, pero pueden llegar a ser más.



_FUNCIONAMIENTO_:

![[Pasted image 20230120224549.png]]

LINKS FOR INFO:
https://www.cloudns.net/blog/dns-cache-explained/
https://netbeez.net/blog/how-dns-cache-works/
