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

## Misión 3: Los Nombres del Holonet – DNS y Resolución de Nombres

**Situación:** La flota rebelde ha interceptado transmisiones imperiales que mencionan distintos códigos de planeta y nombres en clave. Para coordinar un contraataque, la Alianza necesita entender cómo los nombres de dominio galácticos se traducen en localizaciones reales. En otras palabras, requieren restablecer un servicio DNS rebelde que asocie nombres de sistemas estelares con direcciones de la red. Sin DNS, comunicarse es tan difícil como encontrar un Ewok en la noche de Endor.

**Narrativa:** A bordo de la nave de mando *Home One*, los técnicos rebelde te muestran un panel donde parpadea la petición “Unknown host”. El Almirante Ackbar frunce el ceño mientras exclama: "¡Es una trampa... de nombres! Nuestros sistemas no reconocen las direcciones de destino." Mon Mothma asiente con gravedad: "Debemos reconstruir nuestro directorio de comunicaciones. Aprendiz, ¿cómo funciona este Sistema de Nombres de Dominio nuestro? ¿Por qué es tan importante?".

Tú recuerdas tus lecciones sobre cómo en la Red (o la HoloRed) se gestionan las direcciones simbólicas. Un nombre como “echo.base” debe traducirse a una dirección IP para establecer conexión. Ha llegado el momento de explicarlo claramente.

**Pregunta:** Explica el funcionamiento básico del sistema DNS y su importancia en la comunicación en redes. ¿Cómo realiza la red rebelde (o cualquier red TCP/IP) la resolución de nombres de dominio a direcciones IP? Incluye en tu explicación qué es un servidor DNS y un registro (por ejemplo, un registro A), ilustrando con un ejemplo simple (por ejemplo: traducir holonet.rebelion.org a una dirección IP). Además, menciona brevemente qué sucede si el servidor DNS no está disponible y cómo eso afectaría a las comunicaciones de la Alianza.

**Respuesta:**

Almirante Ackbar, Mon Mothma, la clave para traducir esos crípticos códigos de planeta a ubicaciones reales en la HoloRed reside en el **Sistema de Nombres de Dominio (DNS)**. Imaginen el DNS como una guía galáctica, un vasto directorio que asocia los nombres fáciles de recordar de los sistemas estelares y bases rebeldes con sus coordenadas numéricas exactas en la red, sus **direcciones IP**.

**Funcionamiento Básico del DNS:**

En esencia, el DNS funciona como un sistema distribuido de bases de datos jerárquicas. Cuando un miembro de la Alianza intenta acceder a un recurso en la HoloRed utilizando un nombre de dominio (por ejemplo, `echo.base`), su dispositivo realiza una consulta a un **servidor DNS**.

El proceso de **resolución de nombres de dominio** a direcciones IP generalmente sigue estos pasos:

1.  **Consulta del Cliente:** El dispositivo del usuario envía una consulta al servidor DNS configurado, preguntando por la dirección IP asociada al nombre de dominio.
2.  **Consulta Recursiva (si es necesario):** Si el servidor DNS inicial no tiene la respuesta, puede consultar a otros servidores DNS en la jerarquía para encontrar la información.
3.  **Respuesta del Servidor DNS:** Una vez que se encuentra la dirección IP, el servidor DNS envía una respuesta al cliente.
4.  **Establecimiento de la Conexión:** Con la dirección IP, el dispositivo puede establecer una conexión con el recurso deseado.

**Importancia del DNS en la Comunicación en Redes:**

El DNS es fundamental por las siguientes razones:

* **Facilita el Acceso:** Permite usar nombres de dominio fáciles de recordar en lugar de direcciones IP numéricas.
* **Abstracción de la Dirección IP:** Los nombres de dominio permanecen consistentes aunque las direcciones IP cambien.
* **Organización Jerárquica:** Permite una administración descentralizada y eficiente de los nombres de dominio.

**Servidor DNS y Registros (Registro A):**

* **Servidor DNS:** Un servidor especializado que almacena y gestiona la información de los nombres de dominio y sus IPs, respondiendo a las consultas.
* **Registro A:** Asocia un nombre de dominio (o subdominio) con una dirección IPv4.

    **Ejemplo:** Para `holonet.rebelion.org`, un registro A podría ser:

    ```
    holonet.rebelion.org.  IN  A   203.0.113.45
    ```

    Esto asocia el nombre `holonet.rebelion.org` con la IP `203.0.113.45`.

**Consecuencias de la Indisponibilidad del Servidor DNS:**

Si el servidor DNS no está disponible:

* **Fallo en la Resolución de Nombres:** Los nombres de dominio no se podrán traducir a direcciones IP.
* **Incomunicación:** Dificultad para acceder a recursos y comunicarse en la red.
* **Aislamiento:** Las bases y naves podrían quedar aisladas en la red.

En resumen, el DNS es vital para la comunicación en la HoloRed, permitiendo el uso de nombres significativos para acceder a los recursos. Sin él, la coordinación de la Alianza se vería gravemente afectada.
## Misión 4: “Es una trampa… de protocolos!” – TCP vs UDP en las transmisiones

**Situación:** Durante la batalla espacial sobre Endor, los ingenieros de comunicación rebelde notan comportamientos distintos en las transmisiones de datos. Algunas comunicaciones deben ser rápidas aunque ocasionalmente se pierda información (por ejemplo, un stream de vídeo de una cámara X-Wing), mientras que otras deben llegar íntegras y en orden aunque tarden un poco más (por ejemplo, la transferencia de los planos de la Estrella de la Muerte). Estas diferencias corresponden al uso de distintos protocolos de transporte: UDP y TCP. Luke Skywalker, ahora piloteando su X-Wing y ejerciendo de líder en el ataque, te pregunta por qué percibe lagos de datos en unas transmisiones y retrasos en otras.

**Narrativa:** En medio del fragor de la batalla, ves cómo R2-D2 proyecta diagramas de paquetes dentro del X-Wing de Luke. "Algunas de estas tramas van rápidas como el Halcón Milenario, pero otras llegan seguras como Yoda al Consejo," comenta Luke por el comunicador, intentando comprender. Tú, desde la sala de control, le explicas que siente la diferencia entre los dos grandes protocolos de la capa de transporte.

**Pregunta:** Compara los protocolos TCP y UDP y sus características en contexto de la transmisión de datos. ¿Por qué TCP se considera un protocolo confiable y orientado a conexión, y qué implica eso en cuanto a rendimiento? ¿Por qué UDP es no confiable y sin conexión, y en qué casos su rapidez resulta ventajosa? En tu respuesta, menciona ejemplos de aplicaciones o situaciones galácticas para cada protocolo: por ejemplo, qué tipo de datos enviarías mediante UDP durante una misión crítica, y cuál vía TCP en comunicaciones rutinarias.

**Respuesta:**

Luke, la diferencia que percibes en las transmisiones se debe a los dos principales protocolos de la capa de transporte: **TCP (Transmission Control Protocol)** y **UDP (User Datagram Protocol)**. Cada uno tiene sus propias características y es adecuado para diferentes tipos de datos y situaciones.

**TCP (Protocolo de Control de Transmisión): Confiable y Orientado a Conexión**

* **Confiable:** TCP garantiza la entrega de datos de forma **completa y ordenada**. Utiliza un sistema de acuse de recibo (acknowledgment) para verificar que cada segmento de datos enviado ha llegado correctamente al destino. Si un segmento se pierde o llega corrupto, el remitente lo retransmite.
* **Orientado a Conexión:** Antes de que los datos puedan ser transferidos, TCP establece una **conexión** entre el emisor y el receptor a través de un proceso de "handshake" de tres vías. Esta conexión virtual asegura que ambos extremos estén listos para comunicarse. Una vez finalizada la transmisión, la conexión se cierra.
* **Rendimiento:** La confiabilidad y la orientación a conexión de TCP introducen una **sobrecarga** en términos de tiempo y recursos. El establecimiento de la conexión, el seguimiento de los segmentos, los acuses de recibo y las posibles retransmisiones pueden generar **retrasos** y reducir la velocidad de transmisión en comparación con UDP.

**Ejemplos Galácticos para TCP:**

* **Transferencia de los planos de la Estrella de la Muerte:** La integridad de estos datos es crucial. Cada bit debe llegar en orden y sin errores para que la Alianza pueda encontrar su punto débil. TCP sería el protocolo ideal para asegurar que los planos se transmitan de forma confiable.
* **Comunicaciones rutinarias y estratégicas:** El envío de informes de misión detallados, órdenes de la General Organa o cualquier información donde la pérdida o el desorden de los datos podría tener consecuencias negativas se beneficiaría de la confiabilidad de TCP.
* **Acceso a bases de datos de inteligencia:** Cuando los analistas rebeldes acceden a información almacenada en servidores remotos, TCP asegura que los datos se recuperen de manera precisa y completa.

**UDP (Protocolo de Datagramas de Usuario): No Confiable y Sin Conexión**

* **No Confiable:** UDP no garantiza la entrega de los datos. Los paquetes (llamados datagramas en UDP) se envían sin acuse de recibo ni retransmisión en caso de pérdida. No hay mecanismos incorporados para asegurar que los datos lleguen al destino, ni mucho menos en el orden correcto.
* **Sin Conexión:** UDP no establece una conexión antes de enviar los datos. Simplemente envía los datagramas al destino sin verificar si el receptor está listo o incluso existe.
* **Rendimiento:** La falta de mecanismos de confiabilidad y de establecimiento de conexión hace que UDP sea **mucho más rápido** que TCP. La sobrecarga es mínima, lo que permite una transmisión de datos más ágil con menor latencia.

**Ejemplos Galácticos para UDP:**

* **Stream de vídeo de una cámara X-Wing:** Durante un combate, la velocidad es primordial. Aunque ocasionalmente se pierda algún fotograma (causando un "lago" de datos), es más importante tener una transmisión en tiempo real de la situación que esperar a que cada fotograma llegue perfectamente pero con un retraso significativo.
* **Comunicaciones de voz en tiempo real durante una misión crítica:** Al igual que el vídeo, la rapidez de la comunicación por voz es esencial, incluso si se pierden algunas palabras o frases cortas. Un pequeño "lag" es preferible a una conversación entrecortada por retransmisiones.
* **Transmisión de telemetría de naves en combate:** Datos como la velocidad, la posición y el estado de los escudos de un caza X-Wing necesitan ser transmitidos rápidamente a la nave nodriza. La pérdida ocasional de un paquete de telemetría es menos crítica que la latencia en la recepción de la información.
* **Consultas DNS:** Las consultas al servidor de nombres de dominio suelen utilizar UDP debido a su rapidez. Una respuesta rápida es importante para la navegación en la red.

En resumen, Luke, TCP es como un carguero estelar que asegura que cada caja de suministros llegue a su destino, incluso si tiene que tomar una ruta más larga. UDP, en cambio, es como un caza rápido que entrega mensajes rápidamente, sin preocuparse demasiado si alguno se pierde en el camino. La elección entre TCP y UDP depende de las prioridades de la comunicación: integridad y orden versus velocidad y baja latencia.
## Misión 5: Comunicación Segura o lado oscuro – Criptografía y Seguridad de la Red

**Situación:** La victoria rebelde depende de que sus comunicaciones permanezcan secretas. Se rumorea que espías imperiales (e incluso usuarios del lado oscuro) intentan interceptar los mensajes de la Alianza. La líder Mon Mothma te encomienda diseñar un protocolo de comunicación cifrado para que los mensajes entre bases rebeldes no puedan ser leídos por el Imperio aunque sean capturados.

**Narrativa:** En la sala de guerra de la base rebelde, Mon Mothma se dirige a ti con semblante serio: "Hemos logrado infiltrar agentes en Coruscant, pero comunicarse con ellos es arriesgado. El Imperio intercepta cualquier señal. Aprendiz, necesitamos que nuestras transmisiones sean indescifrables para el enemigo." A tu lado, C-3PO comenta nerviosamente en más de seis millones de formas de comunicación: "Si caemos en manos imperiales, prefiero que apaguen mis circuitos a revelar nuestros mensajes."

Recuerdas que cifrar es esencial: usar claves secretas para que sólo los rebeldes autorizados puedan leer un mensaje. También piensas en las dos formas en que esto se puede lograr: con clave simétrica (una misma clave secreta compartida) o con claves públicas/privadas (criptografía asimétrica). Es hora de demostrar tu conocimiento en seguridad.

**Pregunta:** Explica brevemente la diferencia entre cifrado simétrico y cifrado asimétrico en el contexto de las comunicaciones de la Alianza. ¿Cómo funciona cada esquema y qué ventajas ofrece? Por ejemplo, si Leia y Luke comparten una frase clave secreta para cifrar sus holomensajes, ¿qué tipo de cifrado es ese? En cambio, si la Alianza quiere enviar un mensaje a un nuevo aliado sin haber intercambiado una clave secreta previamente, ¿cómo podría ayudar el cifrado asimétrico (claves pública/privada) en ese caso? Comenta también sobre la importancia de autenticación y no repudio en los mensajes (cómo podemos estar seguros de que el mensaje no ha sido alterado y proviene realmente de quien dice ser). Finalmente, menciona por qué utilizar un protocolo seguro (como SSH en lugar de Telnet) es crucial al administrar remotamente los equipos de la red rebelde.

**Respuesta:**

Mon Mothma, la seguridad de nuestras comunicaciones es primordial para evitar que el Imperio descubra nuestros planes. Existen dos métodos principales de cifrado que podemos emplear: el **cifrado simétrico** y el **cifrado asimétrico**.

**Cifrado Simétrico:**

* **Funcionamiento:** En el cifrado simétrico, **una única clave secreta** se utiliza tanto para cifrar (codificar) el mensaje original (texto plano) como para descifrar (decodificar) el mensaje cifrado (texto cifrado). Tanto el emisor como el receptor deben conocer y compartir esta clave secreta.
* **Ventajas:**
    * **Rapidez:** Los algoritmos de cifrado simétrico suelen ser computacionalmente eficientes, lo que permite cifrar y descifrar grandes cantidades de datos rápidamente.
    * **Simplicidad:** El concepto es relativamente sencillo de entender e implementar.
* **Ejemplo en la Alianza:** Si la General Leia y Luke Skywalker comparten una **frase clave secreta** para cifrar sus holomensajes, este sería un ejemplo de **cifrado simétrico**. Ambos utilizan la misma frase (la clave secreta) para codificar sus mensajes antes de enviarlos y para decodificarlos al recibirlos.

**Cifrado Asimétrico (de Clave Pública/Privada):**

* **Funcionamiento:** El cifrado asimétrico utiliza un **par de claves relacionadas matemáticamente**: una **clave pública** y una **clave privada**.
    * La **clave pública** puede compartirse libremente con cualquier persona. Los mensajes cifrados con la clave pública solo pueden ser descifrados utilizando la **clave privada** correspondiente, que debe mantenerse en secreto por su propietario.
    * La **clave privada** es única para cada entidad y nunca debe ser compartida.
* **Ventajas:**
    * **Comunicación Segura sin Intercambio Previo de Claves:** Permite la comunicación segura con entidades con las que no se ha intercambiado una clave secreta previamente. Alguien que quiera enviar un mensaje seguro a un nuevo aliado puede cifrarlo utilizando la clave pública del destinatario. Solo el destinatario, con su clave privada, podrá descifrar el mensaje.
    * **Autenticación y No Repudio (con firmas digitales):** La clave privada también se puede utilizar para crear firmas digitales, que permiten al receptor verificar la autenticidad del remitente y asegurar que el remitente no puede negar haber enviado el mensaje (no repudio).
* **Ejemplo en la Alianza:** Si la Alianza quiere enviar un mensaje a un nuevo aliado sin haber intercambiado una clave secreta previamente, podría funcionar así:
    1. El nuevo aliado genera un par de claves: una clave pública y una clave privada.
    2. El nuevo aliado comparte su **clave pública** con la Alianza (a través de un canal potencialmente inseguro, ya que la clave pública no es secreta).
    3. La Alianza cifra el mensaje utilizando la **clave pública** del nuevo aliado.
    4. El mensaje cifrado se envía al nuevo aliado.
    5. Solo el nuevo aliado, que posee la **clave privada** correspondiente, puede descifrar el mensaje.

**Importancia de la Autenticación y el No Repudio:**

* **Autenticación:** Es crucial asegurarse de que un mensaje realmente proviene de la fuente que dice ser. Sin autenticación, un espía imperial podría enviar mensajes haciéndose pasar por un líder rebelde, sembrando confusión o dando órdenes falsas. La autenticación se puede lograr mediante el uso de firmas digitales (con criptografía asimétrica) o mediante el uso de claves compartidas (en sistemas simétricos, asumiendo que la clave solo la conocen las partes legítimas).
* **No Repudio:** El no repudio proporciona la prueba de que un mensaje fue enviado y recibido por las partes involucradas. Esto evita que un remitente niegue haber enviado un mensaje. Las firmas digitales basadas en criptografía asimétrica son una forma de proporcionar no repudio, ya que la clave privada utilizada para firmar el mensaje es única para el remitente.

**Importancia de Protocolos Seguros (SSH vs. Telnet):**

Al administrar remotamente los equipos de la red rebelde, utilizar un protocolo seguro como **SSH (Secure Shell)** en lugar de Telnet es crucial por las siguientes razones:

* **Cifrado:** SSH cifra todo el tráfico entre el cliente (el administrador) y el servidor remoto (el equipo de red). Esto incluye las credenciales de inicio de sesión (nombre de usuario y contraseña) y todos los comandos y la salida de los comandos. Incluso si un espía imperial intercepta la comunicación, no podrá entender los datos transmitidos.
* **Autenticación Segura:** SSH admite métodos de autenticación más seguros que Telnet, como la autenticación basada en claves públicas, que es más resistente a ataques de fuerza bruta que las contraseñas simples.
* **Integridad:** SSH puede verificar la integridad de los datos transmitidos, asegurando que no hayan sido alterados en tránsito.

En contraste, **Telnet** transmite todos los datos, incluyendo las contraseñas, en **texto plano**. Si un espía imperial intercepta el tráfico de Telnet, podría obtener fácilmente las credenciales de acceso a nuestros equipos de red, comprometiendo toda la infraestructura rebelde. Utilizar SSH es, por lo tanto, una medida fundamental para proteger la administración de nuestra red de las miradas indiscretas del Imperio.

Con estos métodos de cifrado y protocolos seguros, Mon Mothma, podemos fortalecer nuestras comunicaciones y dificultar enormemente que el lado oscuro obtenga información sobre nuestros movimientos.
