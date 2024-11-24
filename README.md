# Tarea3-SD

**¿Qué ocurre cuando n=1 en ambos sistemas?**
R: En ambos sistemas sólo se genera una go routina, por lo tanto este proceso siempre tendrá el token, realizando la tarea secuancialmente por si sólo (leer solicitud, modificar estructura de datos e inscribir en inscripciones).

**¿Qué ocurre cuando n=número de solicitudes en ambos sitemas?**
R: En el sistema A cada go routina leera una solicitud de solicitudes.txt, posteriormente esperará su turno para acceder a la estructura de datos que almacenan los paralelos, provocando un cuello de botella, una vez haya encontrado un paralelo con cupos disponibles lo modifica y pasa a inscibirlo en inscipciones.txt

En el sistema B cada go routina leerá una solicitud de solicitudes.txt, e intentará conseguir el token del paralelo correspondiente según la preferencia indicada, en caso que todas las solicitudes tengan como preferencia el mismo paralelo, se provocará un cuello de botella similar al sistema A, sin embargo, si existen preferencias de paralelos distintas se podrán realizar modificaciones en paralelo a la estructura de datos, cambiando sólo el paralelo correspondiente, posterior a la modificacion de la edd cada proceso realizará la inscripción en inscripciones.

**¿Cómo podría mejorar el sistema A para que sea más eficiente?**
Cada vez que un proceso acceda a la memoria compartida podría almacenar localmente los cupos de cada paralelo y funcionar con un estilo de hash memory que sea válida sólo en caso que el paralelo que se desea consultar cumpla un umbral de cupos, en caso que cumpla con el umbral de cupos la memoria local será válida para una ventana de tiempo que dependerá de la cantidad de procesos, la fiabilidad deseada del sistema y la velocida de las go routines. Cada vez que se realiza una inscripción usando la memoria local se almacenan las operaciones de tal forma que cuando se acceda nuevamente a la memoria compartida esta se actualice correctamente.

Lo anterior ayudaría a solucionar en cierta medida el cuello de botella. Funcionaría bien en un caso donde los paralelos tengan bastantes cupos y se trabajen con pocas go routinas (si se tienen 100 paralelos con 200 cupos y asumiendo un tiempo de inscripcion = 1[s] es poco probable que por ejemplo 3 go routinas terminen el trabajo rápidamente, por lo cual no es necesario consultar a la memoria compartida cada vez).

**¿Cómo podría mejorar el sistema B para que sea más eficiente?**
El sistema B reduciría su eficiencia cuando algunos


