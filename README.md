# Examen-Final-de-Redes-II
https://github.com/csantillgar/Examen-Final-de-Redes-II.git
## Parte I – Misiones de Conocimiento Teórico (60%)
## Misión 1: Reconexión en la Base Eco (Hoth) – Direccionamiento IP y Subredes

**Situación:** La Base Eco en Hoth ha quedado incomunicada tras un bombardeo imperial. La General Leia Organa te encomienda diseñar un nuevo esquema de red para restablecer la comunicación interna y con la flota rebelde. Hoth alberga varios departamentos (mando, defensa, médico, hangar) y cada uno necesita su propia subred IP debido a requisitos de seguridad y cantidad de dispositivos. Disponéis de un bloque de direcciones limitado y es crucial usarlo eficientemente.

**Objetivo de la misión:** Diseñar un plan de direccionamiento IP para la Base Eco utilizando subnetting. Parte de una red asignada (por ejemplo, 172.16.0.0/24) y subdivide en subredes adecuadas para cada departamento, cumpliendo estos requisitos (ficticios):

* Comando Central: 50 hosts (dispositivos)
* Defensa Perimetral: 30 hosts
* Centro Médico: 20 hosts
* Hangar y Taller: 14 hosts
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
| Comando Central   |          50       |      6       |        /26        | 172.16.0.0/26      | 172.16.0.1 - 172.16.0.62 | 172.16.0.63          |
| Defensa Perimetral |         30       |      5       |        /27        | 172.16.0.64/27     | 172.16.0.65 - 172.16.0.94 | 172.16.0.95          |
| Centro Médico     |          20       |      5       |        /27        | 172.16.0.96/27     | 172.16.0.97 - 172.16.0.126 | 172.16.0.127         |
| Hangar y Taller   |          14       |      4       |        /28        | 172.16.0.128/28    | 172.16.0.129 - 172.16.0.142 | 172.16.0.143         |
| Enlace Troncal    |        2          |      2       |        /30        | 172.16.0.144/30    | 172.16.0.145 - 172.16.0.146 | 172.16.0.147         |

La subred destinada al **enlace troncal interplanetario** es **172.16.0.144/30**.
## Misión 2: Sabiduría de Yoda – Algoritmos de Enrutamiento y Rutas

**Situación:** Completando tu entrenamiento en Dagobah, el Maestro Yoda evalúa tu comprensión de cómo circula la información por la galaxia. Las rutas que toman los paquetes a través de los nodos de la HoloRed pueden establecerse de distintas formas: estáticas o dinámicas, simples o inteligentes. Yoda quiere saber si distingues la filosofía detrás de los algoritmos de enrutamiento clave.

**Narrativa:** En la neblina de los pantanos de Dagobah, Yoda cierra los ojos y enuncia en su peculiar forma: "En tus redes, joven padawan, dos caminos existen para los datos: uno *fijo* y otro *en movimiento constante*. El primero, estático es – manual, control total brinda; el segundo, dinámico – se adapta y vive la red. Grandes diferencias hay... ¿Comprenderlas tú las puedes?".

A su lado, luces parpadeantes de un viejo router imperial recuperado iluminan tu rostro. Debes explicarle a Yoda las diferencias que percibes en la Fuerza… de la red.

**Respuesta:**

Maestro Yoda, percibo claramente las dos fuerzas que guían el flujo de datos en la HoloRed: el **enrutamiento estático** y el **enrutamiento dinámico**.

**Enrutamiento Estático:**

* **Filosofía:** Como un camino fijo trazado en un mapa antiguo, el enrutamiento estático se basa en la configuración manual de las rutas en cada router por un administrador de la red. El camino que deben seguir los datos hacia un destino específico se define explícitamente y permanece inalterable hasta que un ser lo modifique.
* **Ventajas:**
    * **Control Total:** El administrador tiene control absoluto sobre las rutas, lo que puede ser útil para redes pequeñas o para definir rutas específicas por seguridad o calidad de servicio.
    * **Simplicidad:** En redes muy pequeñas con pocas rutas, la configuración es sencilla.
    * **Menor Sobrecarga:** No hay tráfico adicional generado por protocolos de enrutamiento.
* **Inconvenientes:**
    * **Administración Compleja a Gran Escala:** Configurar y mantener las rutas manualmente en redes grandes con muchos nodos se vuelve una tarea ardua y propensa a errores.
    * **Falta de Adaptabilidad:** Si un nodo o un enlace falla, las rutas estáticas no se ajustan automáticamente. Los datos destinados a través de esa ruta se perderán hasta que un administrador intervenga y modifique la configuración.
    * **Escalabilidad Limitada:** Añadir nuevos nodos o redes requiere una reconfiguración manual en todos los routers involucrados.

**Enrutamiento Dinámico:**

* **Filosofía:** Como las corrientes de la Fuerza que fluyen y se adaptan, el enrutamiento dinámico permite a los routers aprender y ajustar las rutas automáticamente en respuesta a los cambios en la topología de la red. Utilizan protocolos especiales para comunicarse entre sí e intercambiar información sobre la red.
* **Ventajas:**
    * **Adaptabilidad:** Las rutas se ajustan automáticamente ante fallos de nodos o enlaces, asegurando una mayor resiliencia de la red.
    * **Escalabilidad:** La adición de nuevos nodos o redes se propaga automáticamente a través de la red, simplificando la administración en entornos grandes.
    * **Menor Carga Administrativa (a largo plazo):** Aunque la configuración inicial puede ser más compleja, el mantenimiento continuo se reduce significativamente en comparación con el enrutamiento estático.
* **Inconvenientes:**
    * **Mayor Sobrecarga:** Los protocolos de enrutamiento dinámico generan tráfico adicional en la red al intercambiar información de enrutamiento.
    * **Mayor Complejidad:** La configuración inicial y la comprensión de los protocolos dinámicos pueden ser más complejas que el enrutamiento estático.
    * **Convergencia:** Puede haber un período de tiempo (conocido como convergencia) después de un cambio en la topología donde las rutas aún no se han actualizado completamente en todos los routers, lo que podría causar problemas temporales.

**Protocolo de Enrutamiento Dinámico: OSPF (Open Shortest Path First)**

Un ejemplo de protocolo de enrutamiento dinámico es **OSPF**. OSPF es un protocolo de **estado de enlace**.

**Protocolos de Vector de Distancia vs. Estado de Enlace:**

Las diferencias entre los protocolos de **vector de distancia** (como RIP - Routing Information Protocol) y los protocolos de **estado de enlace** (como OSPF) son significativas en términos de rendimiento y complejidad:

* **Protocolos de Vector de Distancia:**
    * **Funcionamiento:** Los routers de vector de distancia anuncian periódicamente su tabla de enrutamiento completa a sus vecinos directos. Esta tabla contiene información sobre la distancia (en saltos o una métrica similar) y la dirección del "vector" (el siguiente router) para alcanzar cada destino.
    * **Rendimiento:** Pueden ser propensos a problemas como bucles de enrutamiento y una convergencia lenta, especialmente en redes grandes.
    * **Complejidad:** Generalmente son más sencillos de configurar inicialmente.

* **Protocolos de Estado de Enlace:**
    * **Funcionamiento:** Los routers de estado de enlace descubren a sus vecinos y luego anuncian información sobre el estado de sus propios enlaces (interfaces, direcciones IP, métrica del enlace) a todos los demás routers de la misma área. Cada router construye un mapa topológico completo de la red y luego utiliza un algoritmo (como Dijkstra) para calcular la ruta más corta a cada destino.
    * **Rendimiento:** Convergen más rápidamente y son menos propensos a bucles de enrutamiento, lo que los hace más adecuados para redes grandes y complejas.
    * **Complejidad:** La configuración inicial y los algoritmos subyacentes son más complejos que los protocolos de vector de distancia.

En resumen, Maestro Yoda, mientras que el enrutamiento estático ofrece control en entornos pequeños, el enrutamiento dinámico, con protocolos como OSPF, proporciona la adaptabilidad y escalabilidad necesarias para mantener la HoloRed galáctica en constante movimiento y capaz de superar los obstáculos impuestos por el Imperio. La visión completa de la red que ofrecen los protocolos de estado de enlace los hace más poderosos para las vastas distancias de la galaxia.

