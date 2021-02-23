# Dogker test cases
✅  Passing
❌   Failing

## Showdown
| Case | Outcome |
| ------ | ------ |
|Las cartas del jugador NO se muestran porque dicho jugador ya se había retirado de dicha mano (las personas que se retiran no tienen opción de mostrar nada) |✅   |
|Las cartas del jugador NO se muestran porque dicho jugador no tenía combinación ganadora momentánea |✅   |
|Las cartas del jugador NO se muestran porque nadie igualó su apuesta (todos se retiraron) y el jugador decide NO mostrar sus cartas |✅   |
|Las cartas del jugador SI se muestran porque nadie igualó su apuesta (todos se retiraron) y el jugador decide SI mostrar sus cartas |✅  |
|Las cartas del jugador SI se muestran porque dicho jugador tenía combinación ganadora momentánea |✅   |
|Se pueden hacer raises ilimitados|✅  |


## Showdown order
| Case | Outcome |
| ---- | ------- |
|Si es que en la última ronda uno o varios hicieron raise, se empieza por el ultimo que hizo raise (no por los que igualaron)|✅  |
|Si es que en la ultima ronda nadie hizo raise, se empieza por el que está a la izquierda del dealer|✅  |

## All-in scenarios

Jugador A tiene 3150 fichas, jugador B tiene 1850, jugador C tiene 800.
| Case | Outcome |
| ---- | ------- |
|Si el jugador C hace all-in, el jugador A y B solo necesitan apostar 800. Ni A ni B pueden perder toda la partida si pierden la mano, en cambio C sí. | El escenario se cumple, pero hay un bug en el que A y B quedan apostando cero infinitamente tras igualar el all-in de C. Tras salir forzadamente de ese bucle, C pierde, A y B quedan en partida.|

Jugador A tiene 3350 fichas, Jugador B tiene 1650, Jugador C tiene 400.
| Case | Outcome |
| ---- | ------- |
|C y B hacen all-in. A solo necesita apostar 1650. C tiene la mejor mano, y B tiene mejor mano que A. El outcome debería ser C=1200, B=2500 y A=1700|❌  El outcome es C=1800, B=2400, A=1650 |
