# Examen-Final-de-Redes-II
https://github.com/csantillgar/Examen-Final-de-Redes-II.git
## Parte I – Misiones de Conocimiento Teórico (60%)
## Misión 1: Reconexión en la Base Eco (Hoth) – Direccionamiento IP y Subredes

**Situación:** La Base Eco en Hoth ha quedado incomunicada tras un bombardeo imperial. La General Leia Organa te encomienda diseñar un nuevo esquema de red para restablecer la comunicación interna y con la flota rebelde. Hoth alberga varios departamentos (mando, defensa, médico, hangar) y cada uno necesita su propia subred IP debido a requisitos de seguridad y cantidad de dispositivos. Disponéis de un bloque de direcciones limitado y es crucial usarlo eficientemente.

**Objetivo de la misión:** Diseñar un plan de direccionamiento IP para la Base Eco utilizando subnetting. Parte de una red asignada (por ejemplo, 172.16.0.0/24) y subdivide en subredes adecuadas para cada departamento, cumpliendo estos requisitos (ficticios):

* Comando Central: $\sim$50 hosts (dispositivos)
* Defensa Perimetral: $\sim$30 hosts
* Centro Médico: $\sim$20 hosts
* Hangar y Taller: $\sim$14 hosts
* Enlace Troncal: Subred adicional para la conexión con la antena.

**Pregunta:** ¿Cómo dividirías la red 172.16.0.0/24 en subredes para satisfacer las necesidades anteriores, asignando direcciones IP a cada segmento de la base? Indica las subredes obtenidas (con su notación de máscara /xx), la cantidad de hosts útiles en cada una, y especifica qué subred se destinaría al enlace troncal interplanetario.

**Paso a Paso del Subnetting:**

Partimos de la red base: **172.16.0.0/24** (máscara: 255.255.255.0). Tenemos 8 bits disponibles para subredes y hosts.

1.  **Determinar los bits de host necesarios para cada subred:**
    * Comando Central ($\sim$50 hosts): Necesita 6 bits de host ($2^6 - 2 = 62$ hosts útiles).
    * Defensa Perimetral ($\sim$30 hosts): Necesita 5 bits de host ($2^5 - 2 = 30$ hosts útiles).
    * Centro Médico ($\sim$20 hosts): Necesita 5 bits de host ($2^5 - 2 = 30$ hosts útiles).
    * Hangar y Taller ($\sim$14 hosts): Necesita 4 bits de host ($2^4 - 2 = 14$ hosts útiles).
    * Enlace Troncal (2 hosts): Necesita 2 bits de host ($2^2 - 2 = 2$ hosts útiles).

2.  **Calcular la máscara de subred para cada subred:** La máscara base es /24. Añadiremos bits a la parte de red para crear las subredes.
    * Comando Central (6 bits host): 8 bits totales - 6 bits host = 2 bits para subred. Nueva máscara: /24 + 2 = **/26**.
    * Defensa Perimetral (5 bits host): 8 bits totales - 5 bits host = 3 bits para subred. Nueva máscara: /24 + 3 = **/27**.
    * Centro Médico (5 bits host): 8 bits totales - 5 bits host = 3 bits para subred. Nueva máscara: **/27**.
    * Hangar y Taller (4 bits host): 8 bits totales - 4 bits host = 4 bits para subred. Nueva máscara: /24 + 4 = **/28**.
    * Enlace Troncal (2 bits host): 8 bits totales - 2 bits host = 6 bits para subred. Nueva máscara: /24 + 6 = **/30**.

3.  **Determinar la dirección de red, el rango de hosts y la dirección de broadcast para cada subred, secuencialmente desde el bloque 172.16.0.0/24:**

    * **Comando Central (/26 - bloque de 64 direcciones):**
        * Dirección de red: **172.16.0.0/26**
        * Rango de hosts: 172.16.0.1 - 172.16.0.62
        * Dirección de broadcast: 172.16.0.63

    * **Defensa Perimetral (/27 - bloque de 32 direcciones, comienza en la siguiente dirección disponible):**
        * Dirección de red: **172.16.0.64/27**
        * Rango de hosts: 172.16.0.65 - 172.16.0.94
        * Dirección de broadcast: 172.16.0.95

    * **Centro Médico (/27 - bloque de 32 direcciones, comienza en la siguiente dirección disponible):**
        * Dirección de red: **172.16.0.96/27**
        * Rango de hosts: 172.16.0.97 - 172.16.0.126
        * Dirección de broadcast: 172.16.0.127

    * **Hangar y Taller (/28 - bloque de 16 direcciones, comienza en la siguiente dirección disponible):**
        * Dirección de red: **172.16.0.128/28**
        * Rango de hosts: 172.16.0.129 - 172.16.0.142
        * Dirección de broadcast: 172.16.0.143

    * **Enlace Troncal (/30 - bloque de 4 direcciones, comienza en la siguiente dirección disponible):**
        * Dirección de red: **172.16.0.144/30**
        * Rango de hosts: 172.16.0.145 - 172.16.0.146
        * Dirección de broadcast: 172.16.0.147

**Resumen de las Subredes:**

| Departamento      | Hosts Requeridos | Bits de Host | Máscara de Subred | Dirección de Red   | Rango de Hosts        | Dirección de Broadcast |
| :---------------- | :---------------: | :----------: | :----------------: | :----------------- | :-------------------- | :--------------------- |
| Comando Central   |        $\sim$50       |      6       |        /26        | 172.16.0.0/26      | 172.16.0.1 - 172.16.0.62 | 172.16.0.63          |
| Defensa Perimetral |        $\sim$30       |      5       |        /27        | 172.16.0.64/27     | 172.16.0.65 - 172.16.0.94 | 172.16.0.95          |
| Centro Médico     |        $\sim$20       |      5       |        /27        | 172.16.0.96/27     | 172.16.0.97 - 172.16.0.126 | 172.16.0.127         |
| Hangar y Taller   |        $\sim$14       |      4       |        /28        | 172.16.0.128/28    | 172.16.0.129 - 172.16.0.142 | 172.16.0.143         |
| Enlace Troncal    |          2          |      2       |        /30        | 172.16.0.144/30    | 172.16.0.145 - 172.16.0.146 | 172.16.0.147         |

La subred destinada al **enlace troncal interplanetario** es **172.16.0.144/30**.
