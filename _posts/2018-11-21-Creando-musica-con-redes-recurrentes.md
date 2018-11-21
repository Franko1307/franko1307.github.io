---
layout: post
title:  "Creando música con redes recurrentes"
subtitle: "Todos unos DJ's"
background: '/img/posts/02.jpg'
---

# Inicio

Para iniciar este proyecto, como es costumbre me di a la tarea de buscar proyectos en
*Github* para poder basarme en ellos. Ya que empezar un proyecto tan ambicioso desde cero es algo
muy complicado e innecesario. Entre todos los proyectos que logré encontrar dos fueron los que más
me llamaron la atención.

# Primer proyecto

1. [deep-improvisation](https://github.com/tatsuyah/deep-improvisation)

Este proyecto está bastante interesante. La idea es agarrar un archivo MIDI, extraer sus características
principales y plasmarlas en texto. Cuando se abre un archivo MIDI tiene la siguiente estructura:

```python
midi.NoteOffEvent(tick=48, channel=0, data=[79, 64])
midi.NoteOnEvent(tick=10, channel=0, data=[79, 10])
midi.NoteOffEvent(tick=13, channel=0, data=[79, 64])
midi.NoteOnEvent(tick=62, channel=0, data=[76, 51])
midi.NoteOnEvent(tick=35, channel=0, data=[79, 52])
midi.NoteOffEvent(tick=13, channel=0, data=[76, 64])
midi.NoteOnEvent(tick=22, channel=0, data=[74, 23])
midi.NoteOnEvent(tick=1, channel=0, data=[62, 28])
```

Aquí lo que nos interesa es la secuencia que se crea de `NoteOffEvent` con los `NoteOnEvent`y evidentemente
sus valores `tick` y `data`. Se puede notar que `channel`siempre es 0 así que obviaremos ese dato.

Ahora, convertiremos esta secuencia en algo más sencillo para nuestra red, la cual tendrá la siguiente
forma:

```python
0_no_77_57
1_no_69_24
1_no_62_25
0_no_65_28
4_nf_74_64
13_nf_79_64
```

Aquí se puede notar que el primer valor representa el `tick`. `no` hace referencia a `NoteOnEvent` y
evidentemente `nf`hace referencia a `NoteOffEvent`. Y los 2 valores posteriores igual representan los
valores que se ilustran en `data`.

```python
model = Sequential()
model.add(LSTM(128, input_shape=(maxlen, len(unique_chunks))))
model.add(Dense(len(unique_chunks))) # Número de caracteres únicos en nuestra cadena.
model.add(Activation('softmax'))

optimizer = RMSprop(lr=0.01)
model.compile(loss='categorical_crossentropy', optimizer=optimizer)

model.fit(X, y,
        batch_size=24,
        epochs=25
)
```

Ahora, a partir de ese texto se aplica la red recurrente, en nuestro caso utilizamos un modelo que utiliza
una [LSTM](https://colah.github.io/posts/2015-08-Understanding-LSTMs/) de 128 nodos (células) en la primera capa. Luego tenemos una capa densa con nodos iguales a la cantidad única de caracteres en nuestras notas.
Y finalmente una activación con softmax con RMSProp como optimizador.

Se utilizó un `batch_size`de 24 y 25 `epochs`.

Lo anterior son los únicos cambios considerables que se le aplicaron al proyecto anterior y se obtuvo
el siguiente resultado.

<audio src="/extras/cancion_1.mpeg" controls preload></audio>
