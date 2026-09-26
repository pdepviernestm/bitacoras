# Bloques y Wollok Games 🎮

> **Fecha**: Viernes 25/09/2026


## 📌 Sobre lo visto en clase

1. **Repaso Teórico**: Concepto de **polimorfismo**
2. **Introducción a Wollok Game**: Presentación del motor interactivo y su importancia.
3. **Bloques, tick y colisiones**: Desarrollando el código del [Juego Básico de Pepita](https://github.com/wollok/elJuegoDePepita/tree/3-game-bloques).

| Concepto| Definición | Ejemplo |
| --- | --- | --- |
| **Bloque** | Un objeto que engloba una porción de código que solo será ejecutada cuando recibe el mensaje `apply`. Al ser un objeto también podemos pasarlo parámetro en algún mensaje, hacer que una variable lo referencie <br> **Bloque con parámetros** <br> Se especifica una o más variables de entrada especificadas antes del símbolo `=>` para ejecutar su lógica. | **Bloque sin parámetros** <br> `var b = { 2 + 2 }` <br> `b.apply()` <br>**Bloque con parámetros** <br> `var sumar = { x, y => x + y}`<br>`sumar.apply(4, 3)` |
| **Colisión** | Evento que recibe un bloque de código como 2do parámetro en `game.onCollideDo(...)` para que reaccione ante interacciones entre objetos del juego. [Más info](https://www.wollok.org/documentation/wollok_game/#colisiones) | `game.onCollideDo(pepita, { elemento => elemento.teEncontro() })` |
|**Tick**|Evento que ocurre cada cierto tiempo| [Más info](https://www.wollok.org/documentation/wollok_game/#eventos-autom%C3%A1ticoss)|

4. **Chequeo de TP game**: Entrega 0 según el [Cronograma TP](https://docs.google.com/document/d/14axH8tRPGwCBfm3dVypzbikml5k-lJGqsrFteHT36BA/edit?tab=t.0#heading=h.cr9wf5xn9w1r) cuyo objetivo fue definir la idea del game para las próximas entregas.


## 💻 Ejercitaciones

**Que se hizo en clase**: 
Utilizar el evento `onTick` para agregar gravedad, haciendo que pepita pierda altura cada 800 milisegundos, es decir, descienda su coordenada y en 1, pero sin perder energía. Detalle en el [tutorial 2](https://github.com/wollok/elJuegoDePepita/tree/master)

**Se recomienda hace**: Cuando Pepita colisiona con una comida no debería pasar nada (ni siquiera fallar con un error). Detalle del [tutorial 3](https://github.com/wollok/elJuegoDePepita/tree/master)


## 🔗 Resumen links

* [Documentación oficial de Wollok Games](https://www.wollok.org/documentation/wollok_game/)
* [Enunciado de Pepita](https://github.com/wollok/elJuegoDePepita/tree/master): Contiene los diferentes tutoriales vistos en clase
* [Pepita Game (rama: 3-game-bloques)](https://github.com/wollok/elJuegoDePepita/tree/3-game-bloques): Posee el código hecho en clase
* [Juegos desarrollados en Wollok Game](https://www.wollok.org/material/games/)
*  [Cronograma y requerimientos del TP](https://docs.google.com/document/d/14axH8tRPGwCBfm3dVypzbikml5k-lJGqsrFteHT36BA/edit?tab=t.0#heading=h.cr9wf5xn9w1r)