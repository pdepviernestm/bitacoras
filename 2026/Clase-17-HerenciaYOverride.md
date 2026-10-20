# Herencia y Override

<img width="40%" height=auto alt="image" src="https://github.com/user-attachments/assets/60d11750-188b-4d5d-964b-b0fd8d441f73" />

**Fecha**: viernes 9/10/2026
## Teoría vista durante la clase
### Herencia
La herencia es una herramienta que nos permite compartir código en distintas clases con estructura jerárquica de super clases y sub clases. 
### Override
Esta herramienta nos permite sobrescribir un comportamiento definido por la clase padre más cercana.
### Method lookup

Es el mecanismo por el cual se determina qué **método** se debe ejecutar ante el envío de un **mensaje**. 

**¿Qué pasa cuando a un objeto `i` (instancia de la clase `C`) se le envía un mensaje de nombre `m`?**

>  **Nota:** A efectos de entender el mecanismo (ya que el ambiente o máquina virtual lo hace internamente), tenemos que mantener una **referencia a la clase** en la que estamos buscando en un momento determinado.

Al principio, la **clase actual es `C`**, por ser la clase a partir de la cual instanciamos al objeto receptor del mensaje (el objeto `i`).

#### Algoritmo

**1. Buscar:** Se busca en la **clase actual** un método con el nombre `m`.

*   ✅ **a) Si se encuentra:** 
    Se ejecuta el método encontrado en el objeto `i`. 
    🏁 *Fin del Method Lookup.*

*   ⬆️ **b) Si no se encuentra (y la clase actual NO es `Object`):** 
    La clase actual pasa a ser la **superclase** de la clase actual, y **se vuelve al paso 1** (se busca más arriba en la jerarquía).

*   ❌ **c) Si no se encuentra (y la clase actual ES `Object`):** 
    Llegamos a la raíz de la jerarquía de clases. El objeto `i` **no entiende** el mensaje `m` *(lo que generalmente lanza una excepción o error)*.
    <img width="441" height="491" alt="image" src="https://github.com/user-attachments/assets/d3320c33-767d-4134-81b8-e6ff657ba1b2" />

### Ejercicio de la clase
1) Se define la clase base con su estado interno expuesto mediante propiedades. Se centraliza la lógica matemática en métodos específicos. Se introduce un método auxiliar `dineroPendiente()` para evitar repetición de lógica y mantener la cohesión interna al momento de calcular los intereses.
```wollok
class Deuda {
	var property dineroPrestado
	var property tasaInteres
	var property dineroDevuelto = 0

	method esCara() = dineroPrestado > 10000

	method dineroPendiente() = dineroPrestado - dineroDevuelto

	method interesesAPagar() = tasaInteres * self.dineroPendiente()

	method amortizar() {
		dineroDevuelto = dineroDevuelto + 1000
	}
}
```
2,3)
Se utiliza herencia para reutilizar el estado y comportamiento de la superclase. En `esBajoRiesgo()`, el uso de `self.esCara()` demuestra el mecanismo de method lookup, donde el objeto busca la definición del mensaje subiendo en la jerarquía de clases. Se utiliza override para alterar la lógica de `amortizar()`, manteniendo la firma original estricta para garantizar el polimorfismo.

```wollok

class DeudaAlemana inherits Deuda {
	var property duracionPeriodos

	method esLargoPlazo() = duracionPeriodos > 10

	method pagoExcepcional(cantidad) {
		dineroDevuelto = dineroDevuelto + cantidad
	    }

	method esBajoRiesgo() = self.esLargoPlazo() and self.esCara()

	override method amortizar() {
		dineroDevuelto = dineroDevuelto + (dineroPrestado / duracionPeriodos)
	}
}
```

4) Creamos otro tipo de deuda que en este caso es una deuda americana. En este caso usamos un objeto como variable global para mandarle el mensaje `anioActual()`
```wollok
object calendario {
	var property anioActual = 2024
    }

class DeudaAmericana inherits Deuda {
	var property anioFinalizacion

	override method amortizar() {
        if (calendario.anioActual() == anioFinalizacion) {
            dineroDevuelto = dineroDevuelto + dineroPrestado
        }
    }
}
```
5) El cliente conoce una colección de deudas genéricas. Al iterar con `forEach()`, el cliente envía el mensaje `amortizar()` desconociendo la clase concreta de cada elemento. El polimorfismo y el method lookup determinan en tiempo de ejecución qué porción de código ejecutar según la instancia referenciada.
```wollok
class Cliente {
	const property deudas = []
	var property tipoCliente

	method amortizarDeudas() {
		deudas.forEach({ deuda => deuda.amortizar() })
	}
```
6)Para modelar los tipos de cliente, se descarta la herencia (clases ClienteNormal y ClientePreferencial). Si un cliente cambiara de tipo mediante herencia, requeriría instanciar un nuevo objeto, perdiendo la referencia de memoria original y la lista de deudas asociadas. Con la variable tipoCliente podemos definir que algoritmo vamos a usar para consultar el plus. Los objetos normal y preferencial son polimórficos. Cambiar de tipo en tiempo de ejecución se resuelve con una reasignación directa de la variable, manteniendo intacta la identidad del cliente.
```wollok
class Cliente {
	const property deudas = []
	var property tipoCliente

	method amortizarDeudas() {
		deudas.forEach({ deuda => deuda.amortizar() })
	}

	method cuotaMensual() = 1000 + tipoCliente.plus()
}

object normal {
	method plus() = 200
}

object preferencial {
	method plus() = 0
}

```
### Diagrama estático
Este es el diagrama estático autoría del compañero docente gabriel galatro
<img width="871" height="655" alt="image" src="https://github.com/user-attachments/assets/a0947728-b817-48d3-a65e-3456feb8eb91" />

chauuu
