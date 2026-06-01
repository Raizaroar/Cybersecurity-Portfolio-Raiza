# Reconocimiento de las caracteristicas de la topologia de la red


## Hay 3 topologias de red básicas 

- RING, STAR, BUS

![networkfundamentals](../../../../assets/screenshots/09-CiscoIT/Networking/NetworkingFundamentals/NetworkFundamentals1.png)

## Bus

- Se conectan todos los dispositvos entre si

- Hay una capacidad limitada antes de que el trafico se congestione o caiga   

Se utiliza en redes inalambricas, en los AP inalambricos 

## Ring

- El trafico de red actua hacia un solo sentido

-Su limitacion es que el costo es Alto

## Star 

- Puede conectarse a un dispositivo que necesite sin molestar a ninguno de los demas

- da mas ancho de banda

## Topología en malla

Diseño: Cada dispositivo se conecta con varios otros, creando múltiples rutas.

Ejemplo: Redes de telecomunicaciones y backbone de Internet.

Ventaja: Alta redundancia y confiabilidad.

Desventaja: Costosa y compleja de implementar.
- Permite la comunicacion de la manera que necesitamos para tener la velocidad maxima

## Topología híbrida

Diseño: Combinación de varias topologías (estrella + bus, estrella + malla).

Ejemplo: Redes empresariales modernas que mezclan switches, routers y enlaces redundantes.

Ventaja: Flexibilidad y escalabilidad.

Desventaja: Puede ser difícil de administrar.

1. **Physical Topology** es como los equipos de estan realmente conectados entre si  

2. **Logical Topology** es como funciona la red para enviar datos


# Tipos de Redes

1. **LAN (Local Area Network)**
- Qué es: Red local, limitada a un espacio físico pequeño (casa, oficina, escuela).

*Ejemplo: La red Wi-Fi de tu casa que conecta tu laptop, celular y Smart TV.

*Ventaja: Alta velocidad y bajo costo.

2. **WAN (Wide Area Network)**
- Qué es: Red que conecta varias LAN a gran escala, incluso países.

- Ejemplo: Internet es la WAN más grande.

- Ventaja: Permite comunicación global.

3. **MAN (Metropolitan Area Network)**

- Qué es: Red que cubre una ciudad o área metropolitana.

- Ejemplo: La red de fibra óptica que conecta universidades y oficinas en CDMX.

- Ventaja: Mayor alcance que una LAN, pero más controlada que una WAN.

4. **PAN (Personal Area Network)**
- Qué es: Red personal, de corto alcance.

- Ejemplo: Conexión Bluetooth entre tu celular y audífonos.

- Ventaja: Muy práctica para dispositivos portátiles.

5. **WLAN (Wireless LAN)**
- Qué es: Variante de LAN, pero inalámbrica.

- Ejemplo: Wi-Fi en cafeterías o aeropuertos.

- Ventaja: Movilidad sin cables.

6. **VPN (Virtual Private Network)**

- Qué es: Red virtual que crea un túnel seguro sobre Internet.

- Ejemplo: Usar una VPN para acceder a recursos de tu empresa desde casa.

- Ventaja: Seguridad y privacidad.

7. **SAN (Storage Area Network)**

- Qué es: Red especializada en almacenamiento de datos.

- Ejemplo: Servidores de una empresa que comparten grandes volúmenes de información.

- Ventaja: Alta velocidad para manejar bases de datos y respaldos.

8. **CAN (Campus Area Network)**

- Qué es: Red que conecta varias LAN dentro de un campus (universidad, empresa).

- Ejemplo: La red de una universidad que conecta facultades, bibliotecas y laboratorios.

- Ventaja: Organización centralizada y eficiente.

# Topologias de diseño de Red

![DesignTopolgy](../../../../assets/screenshots/09-CiscoIT/Networking/NetworkingFundamentals/NetworkFundamentals2.png)

En este ejemplo tenemos las tres capas que comunmente se ponen en empresas grandes

- Access Layer

-Distribuition Layer

-Core Layer

y este es el tipo que combina las 2 capas ( Core y Distribuition Layer) y se utiliza en medianas empresas

- Collapsed Core Layer

- Access Layer

![DesignTopology](../Networking/../../../../assets/screenshots/09-CiscoIT/Networking/NetworkingFundamentals/NetworkFundamentals3.png)

Hay un nueva topologia de diseño de red que se llama 

## **Spine leaf topology** 

-    Leaf (hojas): Son los switches de acceso donde se conectan los servidores, PCs o dispositivos finales.

-   Spine (tronco): Son los switches centrales que interconectan todos los leaf.

No hay conexión directa entre leafs ni entre spines: todo pasa por la estructura tronco-hoja.

**Ventajas**

- Baja latencia: Todos los dispositivos están a máximo 2 saltos de distancia.

- Escalabilidad: Puedes agregar más hojas o más troncos sin rediseñar todo.

- Balanceo de tráfico: El tráfico se distribuye de manera uniforme entre los spines.

- Alta disponibilidad: Si un spine falla, los demás mantienen la comunicación.


![SpineLeafTopolgy](../../../../assets/screenshots/09-CiscoIT/Networking/NetworkingFundamentals/NetworkFundamentals4.png)


NOTA: > Si quiero ampliar mi ancho de banda para mis ENDPOINTS , debo agregar un SPINE SWITCH

![SpineLeafTopolgy](../../../../assets/screenshots/09-CiscoIT/Networking/NetworkingFundamentals/NetworkFundamentals5.png)

NOTA: > Si agrego Leaf switches eso significa que puedo agregar mas ENDPOINTS

![SpineLeafTopolgy](../../../../assets/screenshots/09-CiscoIT/Networking/NetworkingFundamentals/NetworkFundamentals6.png)


## Cableado

como cabeza tenemos un RJ45 que es el que nos permite conectar a un dispositivo

en el cableado tenemmos dos formas de hacer que se conecten el

568A UTP

568B UTP

![568ABy568Butp](../../../../assets/screenshots/09-CiscoIT/Networking/NetworkingFundamentals/NetworkFundamentals7.png)

## Straight-through vs Crossover

1. Straight-through: dispositivos distintos — PC a switch, switch a router.

2. Crossover: dispositivos iguales — PC a PC, switch a switch, router a router.

3. Auto-MDIX: los switches modernos detectan automáticamente el tipo de cable. Ya casi no importa cuál uses físicamente, pero el examen pregunta la regla clásica.

![568ABy568Butp](../../../../assets/screenshots/09-CiscoIT/Networking/NetworkingFundamentals/NetworkFundamentals8.png)


## Fiber Optic

1. Fibra single mode

la monomodo se usa para largas distancias y altas velocidades (hasta más de 100 km)

se usa en: infraestructura de telecomunicaciones, enlaces troncales de Internet, redes metropolitanas, transmisión de datos en larga distancia.


2. Fibra multimode 
multimodo es más económica y práctica para distancias cortas (hasta 1 km aprox.), como en LAN y centros de datos.

se usa en:redes locales (LAN), campus universitarios, centros de datos, sistemas de videovigilancia.

![568ABy568Butp](../../../../assets/screenshots/09-CiscoIT/Networking/NetworkingFundamentals/NetworkFundamentals10.png)

> Nota: al ver los cables por fuera no podemos distinguir a simple vista cual es el de single mode o multimode entonces tenemos las leyendas que traen los cables y con eso sabemos cual es cual

