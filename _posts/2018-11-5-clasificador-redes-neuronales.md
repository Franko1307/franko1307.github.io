---
layout: post
title:  "clasificador de celulas blancas usando redes convolucionales"
subtitle: "Mi experiencia y consejos"
background: '/img/posts/01.jpg'
---

## El inicio

Todo empezó cuando el profesor [Waissman](https://github.com/juliowaissman) nos dejó como proyecto hacer una red convolucional
para la clase de Redes Neuronales. Teníamos que usar [Tensorflow](https://www.tensorflow.org/) o algo similar. 
Terminé eligiendo [keras](https://keras.io/)


![keras+tensorflow](/img/posts/clasificador-redes-neuronales/keras-tensorflow-logo.jpg){:class="img-responsive"}

Al principio todo iba bien, cada quien eligió un tema y yo termine eligiendo el de clasificador de células blancas. 
Mi plan original era hacer una red que le dieras una imagen con células blancas en ella y te dijera cuántas había y de qué tipo. 
Así que me puse a investigar proyectos que hubieran hecho algo similar y encontré los siguientes

*   [wbc-classification](https://github.com/dhruvp/wbc-classification)
    
    Este fue el primer proyecto que chequé y en el que más me basé. 
    
    Me pareció bastante completo, y reportaba resultados del 98% de presición en clasificación binaria y 93% de presición en clasificación
    no binaria de 4 tipos de células blancas.
   
*   [white-blood-cells-classification](https://github.com/deadskull7/White-Blood-Cells-Classification)

    Este proyecto era un fork del anterior donde simplemente le añadió un par de detalles pero no hizo gran cosa. Aún así
    traté de sacar cosas interesantes de aquí y terminé forkeando este proyecto y usándolo como base aunque en realidad
    el primero era el original. De igual manera este proyecto reportaba los mismos resultados de presición que el anterior
    
*   [white_blood_cells_classification_via_convolutional_neural_network](https://github.com/JLewiste/white_blood_cells_classification_via_convolutional_neural_network)
    
    Este fue el último proyecto que chequé y también hacía algo similar a los anteriores aunque no lo miré mucho. En este momento
    entendí que el primero y el segundo eran básicamente lo mejor que podía encontrar en ese momento.
    
    
## Haciendo la red

Una vez elegido el tema y el proyecto en el cuál basarme y ya que la base de datos la obtuve de los proyectos anteriores
intenté resolver el problema de 3 maneras diferentes:

1.  Agarrar una red convolucional entrenada y ver qué tan buenos resultados me daba. 
    Elegí la [VGG16](https://gist.github.com/baraldilorenzo/07d7802847aaad0a35d3) ya que venía ya en keras entrenada
    con las imágenes de [image-net](http://image-net.org/) así que fue fácil de implementar. 
    
    Este método me dio resultados terribles ya que al clasificar una célula blanca lo más cercano que encontró fue
    una cortina de baño. El resultado está [aquí](https://github.com/Franko1307/Contador-de-celulas-blancas-con-redes-neuronales-convolucionals/blob/master/libretas/Modelo%20por%20default.ipynb)
    
2.  De nuevo utilizar la red pre-entrenada VGG16 pero ahora, quitándole la última capa y entrenándola por mi cuenta utilizando
    la misma base de datos. Este método fue bastante eficaz ya que básicamente no costó mucho tiempo entrenar la red y obtuve 
    resultados del 78% de presición. El resultado está [aquí](https://github.com/Franko1307/Contador-de-celulas-blancas-con-redes-neuronales-convolucionals/blob/master/libretas/Clasificador-modelo-basico.ipynb)
        
3.  Al ver que los resultados que obtenía no eran mejores que los que se planteaban en los proyectos de arriba, probé a hacer mi propia
    red, modificando un poco la que ellos utilizaban. Sólo que en vez de clasificar entre 4 lo hace entre 5.
    
    Este fue el método que mejor resultados me dio llegando hasta un 92% de presición que si comparamos con el 93% de presición en clasificación de 4
    es decir, estamos obteniendo casi los mismos resultados clasificando con 5.
    
    El resultado se encuentra [aquí](https://github.com/Franko1307/Contador-de-celulas-blancas-con-redes-neuronales-convolucionals/blob/master/libretas/clasificador-modelo-avanzado.ipynb)
    
    Y esta es la estructura de la red:
    
    ![red-estructura](/img/posts/clasificador-redes-neuronales/model.png){:class="img-responsive"}
    
    
## Conclusión

Fue una experiencia divertida, logré entender cómo funciona una red convolucional y ahora sé que soy capaz de agarrar
cualquier proyecto de red convolucional en internet, modificarlo un poco para aplicarlo a algún area de interés que tenga y obtener
resultados buenos o decentes. 

Los mayores problemas que tuve fue a la hora de hacer mi propia red y entrenarla desde 0. Se tardaba bastante y me daba resultados malísimos. 
Esto lo pude solucionar cambiando la base de datos que estaba utilizando. Al parecer, las imágenes originales o eran muy abstractas para que 
la red pudiera aprender o eran demasiado difíciles. Al usar esta nueva base de datos parece que la dificultad bajó dando mejores resultados.

Como consejo daría que cuando algo no está funcionando prueben a usar diferentes datos, probar nuevos algoritmos. Quizá el problema está atascado
en un mínimo local y ya sea por los datos o por tu algoritmo no se puede resolver tan fácilmente. 