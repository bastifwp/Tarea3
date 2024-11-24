# Tarea3-SD

**¿Qué ocurre cuando n=1 en ambos sistemas?**  
R: En ambos sistemas sólo se genera una go routina, por lo tanto este proceso siempre tendrá el token, realizando la tarea secuancialmente por si sólo (leer solicitud, modificar estructura de datos e inscribir en inscripciones).

**¿Qué ocurre cuando n=número de solicitudes en ambos sitemas?**  
R: En el sistema A cada go routina leera una solicitud de solicitudes.txt, posteriormente esperará su turno para acceder a la estructura de datos que almacenan los paralelos, provocando un cuello de botella, una vez haya encontrado un paralelo con cupos disponibles lo modifica y pasa a inscibirlo en inscipciones.txt.    

En el sistema B cada go routina leerá una solicitud de solicitudes.txt, e intentará conseguir el token del paralelo correspondiente según la preferencia indicada, en caso que todas las solicitudes tengan como preferencia el mismo paralelo, se provocará un cuello de botella similar al sistema A, sin embargo, si existen preferencias de paralelos distintas se podrán realizar modificaciones en paralelo a la estructura de datos, cambiando sólo el paralelo correspondiente, posterior a la modificacion de la edd cada proceso realizará la inscripción en inscripciones.

**¿Cómo podría mejorar el sistema A para que sea más eficiente?**   
Cada vez que un proceso acceda a la memoria compartida podría almacenar localmente los cupos de cada paralelo y funcionar con un estilo de hash memory que sea válida sólo en caso que el paralelo que se desea consultar cumpla un umbral de cupos, en caso que cumpla con el umbral de cupos la memoria local será válida para una ventana de tiempo que dependerá de la cantidad de procesos, la fiabilidad deseada del sistema y la velocida de las go routines. Cada vez que se realiza una inscripción usando la memoria local se almacenan las operaciones de tal forma que cuando se acceda nuevamente a la memoria compartida esta se actualice correctamente.

Lo anterior ayudaría a solucionar en cierta medida el cuello de botella. Funcionaría bien en un caso donde los paralelos tengan bastantes cupos y se trabajen con pocas go routinas (si se tienen 100 paralelos con 200 cupos y asumiendo un tiempo de inscripcion = 1[s] es poco probable que por ejemplo 3 go routinas terminen el trabajo rápidamente, por lo cual no es necesario consultar a la memoria compartida cada vez).

**¿Cómo podría mejorar el sistema B para que sea más eficiente?**  
El problema del sistema B es la carga que implica en la red, por cada solicitud en solicitudes.txt se enviará una request a todos los nodos por el token, además, al existir un token por cada paralelo el tráfico de la red es mucho mayor. Para mejorar lo anterior se propone que cada go routine pueda leer más solicitudes a la vez (pero no demasiadas para evitar esperar por el acceso a paralelos), de esta forma una go routine podría realizar inscripciones simultáneamente asegurando confiabilidad.

**Escriba una analogía de los sistemas A y B con un sistema de inscripción presencial**  
Para realizar una analogía consideraremos las go routines como personas que se mueven a través de los distintos módulos: solicitudes sería similar a una caja (clientes esperando a ser llamados), paralelos sería la universidad (conjunto de salas, en cada sala hay un profesor que sabe cuantos cupos le quedan a su paralelo), e inscipciones se llevaría acabo por un grupo de encargados afuera de la universidad.  

Para el sistema A, la persona que simula la go routine atiende al cliente que se encuentre en la caja para esuchar su solicitud, posteriormente, se dirije a la universidad y cierra la puerta de la entrada, se dirije a las salas indicadas por la solicitud en orden hasta el primer profesor que le indique que quedan cupos, en ese momento abre la puerta de la universidad (así otra persona podrá entrar a la universidad) y se dirige al encargado de inscribir dicho paralelo, el proceso se repite hasta que no hayan clientes en la caja.  

Para el sistema B serían las mismas analogías, sin embargo, ahora al entrar a la universidad no se cierra la puerta, sin embargo, cuando se ingresa a una sala para preguntarle al profesor esta debe ser cerrada (también puede ser interpretado como que el profesor sólo puede hablar con una persona a la vez) una vez termine de preguntarle acerca de los cupos se abre la puerta de la sala y se dirigue a inscripciones.

Como diferencias podemos observar que en el sistema A solo una persona puede estar en la universidad al mismo tiempo, en cambio, en el sistema B muchas personas pueden estar en la universidad simultáneamente.   

Como mejora para el sistema A, al ingresar a la universidad se podría aprovechar para consultar a varios profesores acerca de sus cupos, así al esuchar otra solicitud podría saber inmediatamente si se puede llevar a cabo la inscripcion e ir directamente a inscripciones (para esto es necesario saber cuantos trabajadores hay y que tan rápido trabajan para aseugrar la confiabilidad).  

Como mejora para el sistema B sería que cada persona maneje varias solicitudes a la vez (pero no demasiadas), aprovechando que puede ingresar en la universidad, de esta forma una puede realizar varias inscripciones a la vez de forma confiable.  



