2do Parcial - Parte Practica
Que se solicita:

El codigo tiene 10 errores. Recae en usted analizar que es un error dentro del codigo.
Los Alumnos tendran que forkear este repo como propio, hacer un issue desde Github con Comentarios refiriendo en que linea esta el error, y como se debe solucionar.
La respuesta sera con el link a ese Fork, y adentro deben estar los issues. Los profesores tenemos que poder ingresar al mismo. Recae en los alumnos asegurarse de que los profesores puedan ingresar.
Tambien pueden editar el Archivo Readme y poner los resultados dentro de sus propios forks.
https://github.com/ExBattou/SimpsonsApp

Problemas encontrados:

1: archivo mainScreen linea 103.La linea episodes.itemSnapshotList.indexOfFirst.Esto es procesamiento de datos y rompe la separación de responsabilidades, ya que la Vista solo debe emitir intenciones (intents) y reaccionar. no debe manejar ningun tipo de peticion o logica
Solucion: La logica deberia comunicarle el evento al view model y el viewmodel encargarse de la logica y buscar el episodio, si lo encuentra tendria que emitir un evento para avisarle a la ui.

2: archivo datamodule: se construye una instancia de retrofit pero falta indicar la base url antes de invocar al build.
la solucion: añadir la url base de la api. dentor del bloque retrofit.builder

3:Ruta absoluta del JDK hardcodeada que hizo que la app no sincronize ni me corriera, la solucion fue simplemente borrarlo o comentarlo y ahi la app me pudo correr. La linea comentada fue la siguiente:  y fue en el file glade.propierties. org.gradle.java.home=/opt/homebrew/Cellar/openjdk@17/17.0.15/libexec/openjdk.jdk/Contents/Home

4:La version del SDK decia 36, peno el maximo es 35 o 34, entonces simplemente cambie eso a 35 y la app pudo correr.
Este cambio se hizo en el build.grade y alli la app me corrio.

5:El problema lo encontre en el archivo model/episode, basicamente habia un codigo fuera de la clase, entonces tira error:init {
    return Episode; //NO BORRAR
}
la solucion fue borrarlo ya que no deberia haber un init fuera de la clase.

6: hay errores en los nombres de una funcion. en episode repository implementes la funcion es getEpisodes utilizando camel case mientras que en la interfaz la funcion se llama get_episodes entonces no  estaria implementando la misma funcion lo que dara errores.
Solucion: hacer que la impl respete el nombre impuesto por la interfaz

7: borrar archivos que no aportan nada:data repository, default data repository mainscreenviewmodel  y mainscren view model test no estan acoplados a la logica de la app y son generados automaticamente.

8:El error esta en el main activity, con el theme, la app tiene un theme personalizado pero no lo esta llamando, en el set content usa matherial theme  pero deberia usar simpson app theme. esto seria linea 17, dentro del sent content.
la solucion: cambiarlo por el simpson app theme.

9: La lista de temporadas es no reactiva en la base de datos. esto es en el archivo episode repository getAvaiableSeasons() devuelve  una list de enteros. entonces los cambios en lso datos no se notificaran en la ui.
Soluccion: envolverlo en el flow el return asi es reactivo a los cambios.

10:EpisodeRepositoryImpl, aca el problema lo encontre en la linea 33 dentro de get episodesBySeasons(), la arquitectura exige offline first. en getEpisodes el repositorio crea el pager inyectando el episode remote mediator correctamente.
pero  en la de getEpisodesBySeasons se instancia el repositorio sin el remote mediator  si el usuario filtra por una temprada que no esta en local  o sale una nueva temporada, no va a poder filtrarlo por esa temporada. o pero aun, si hay un fallo de red y justo esa temporada no estaba guardada, va a retornar vacio.
La solucion: agregar el remote mediator al pager, dentro de ese getEpisodesBySeasons() tambien.
