## Enunciado
el...[enunciado](https://docs.google.com/document/d/1c58_uwz7xEZqu09FqaGCb6YKSszrmAa_M8b751Kono0/edit?usp=sharing)
## Posible solución
### verdadero o falso
a) f

b) f

c) v 

d) f

e) f
### Diagrama
![](https://www.plantuml.com/plantuml/png/dLN1aXet4BtFLwYuPB2BhUHARIZF0h5nEK0OrbNEhP5wGKIZfbeQAkNOdv1JlYDViJ_cOHYq0yDIot9Xf6vrw_LxtNhdFB3FCnEvlxJT04Ccf07d4kzPBtAF8qasrLzH_5npoSYIN35bA3pd6AsqGY4rIuDHz9FuzSM_mZcTz4V3GJ8GNyHazZuPZ_vC-gF7_yRNnIalBLAI6XKeyKyaH4Tic2GwSbtnIJj_EzqlYp-LIw-tr2kytWfOCYw1AsjS7UgAXdjxvnw2zbbxBSd5xSlXryGM50pmKovN41ClmUgl83NPhfZKLfC58qCx-XvLQ9h62XSwp-1ngQ4hXk6xZBz7j35Q0l-EIYku2EZxrNHDa_ciO4Uayrao7xx_8kBVY3DmGg4mknkqhukehKT-08cdICJjRR3rUYNojmBNA7CFV15KAE277CtoKBGLCRWfS75cd6SBv5S47EEIf2u9IS40Qp07gH5Bj1xMcEHPyV6wB8qIVBkvgL2SjvsvK1I5BcZzqXATJa-qthIQ2KKtIFdfnCrDxvpikYSgZfe-PJBgT7R_3IgxDScUErkaFNl18o5VBTMFmrao--F-yTz438PYEflqX-aatNqCxllpqUT9-fePqxXdfG3Fi80rA68y39nvs3gQD4_RiU-AfzL-lcJWuC1bt3wIqHOCyJZ7BSt9DwmRaxl2GyF9lWS7mxytXdnPyXEhArFju6IeD9rQjLLQWYdeKc7KDO5jmIvpOANXMHTq7X9pHwnmWFUsJ_R1QEdBX7T41i6MVaLk1XFCfrXgHHxQKbmNCbOZkfuYjF9z4R8IOKMWf4oxOXaIlfIo2xo4T2aMOFWEh0JM1yaio5A1T8jMKpXoGGENwnDozS6OFbgF790vJsc-GTwM2AV4dmDZ5pGKuZOrV_kjqwdNy3cNOzpFUDLkV7MJ5y_GNV8n6KnchsdcmEIXL5Kx1F3XvKWn0mi6NTrKOtvA_84OwjPIysczWuwbNXxUbqAhKwKpxhRBc2wMO7rbhIG7vIi3l5S9nRltvzX3eOC7RJHmN1q9gxECXldzfbKlI_UevnglK36vZxrZjDl7_q5aQ1siqGaGXfnOxHwnN3iFhXr5xz2gF3C_0000)
### Codigo
```wollok
class Trabajador {
  var property salario
  var property horasSemanales
  var property valorNuevoPorHora
  var property estaSindicalizado

  method valorProducido() = horasSemanales * valorNuevoPorHora

  method desafiliar() { estaSindicalizado = false }

  method reducirHoras(cant) {
    horasSemanales = (horasSemanales - cant).max(0)
  }

  method aumentarSalario(porc) {
    salario += salario * (porc / 100)
  }
}

class ProcesoProductivo {
  const property trabajadores = []

  method agregarTrabajador(t) { trabajadores.add(t) }

  method masaSalarial() = trabajadores.sum({ t => t.salario() })

  method valorNuevoTotal() = trabajadores.sum({ t => t.valorProducido() })

  method plusvalia() = self.valorNuevoTotal() - self.masaSalarial()

  method tasaExplotacion() {
    if (self.masaSalarial() == 0) {
      throw new DomainException(message = "Sin masa salarial no es posible calcular la tasa de explotación.")
    }
    return self.plusvalia() / self.masaSalarial()
  }

  method salarioPromedio() = 
    if (trabajadores.isEmpty()) 0 else self.masaSalarial() / trabajadores.size()

  method sindicalizados() = trabajadores.filter({ t => t.estaSindicalizado() })

  method vanguardia() {
    const promedio = self.salarioPromedio()
    return trabajadores.filter({ t => t.salario() < promedio })
  }

  // Hook method polimórfico para el umbral
  method umbralConflicto() = 1.5

  method estaAlBordeDeUnConflicto() {
    if (trabajadores.isEmpty()) return false
    const mitad = trabajadores.size() / 2
    const masaCritica = self.sindicalizados().size() > mitad or self.vanguardia().size() > mitad
    return (self.tasaExplotacion() > self.umbralConflicto()) and masaCritica
  }

  // Resuelve internamente quiénes van a la lucha respetando el encapsulamiento
  method trabajadoresEnLucha() {
    if (!self.sindicalizados().isEmpty()) return self.sindicalizados()
    if (!self.vanguardia().isEmpty()) return self.vanguardia()
    throw new DomainException(message = "No hay masa laboral para sostener la medida.")
  }
}

class Uberizacion inherits ProcesoProductivo {
  var property comision = 0.20

  override method agregarTrabajador(t) {
    t.desafiliar()
    super(t)
  }

  override method valorNuevoTotal() = super() * (1 + comision)
}

class SectorBancario inherits ProcesoProductivo {
  var property bonoConvenio = 500

  override method masaSalarial() = 
    super() + (self.sindicalizados().size() * bonoConvenio)

  override method umbralConflicto() = 1.0
}

class OrganizacionObrera {
  var property estrategia

  method aplicarMedida(proceso) {
    estrategia.ejecutar(proceso)
  }
}

class Huelga {
  var property horasParo

  method ejecutar(proceso) {
    proceso.trabajadoresEnLucha().forEach({ t => t.reducirHoras(horasParo) })
  }
}

class NegociacionSalarial {
  var property porcentajeAumento

  method ejecutar(proceso) {
    proceso.trabajadoresEnLucha().forEach({ t => t.aumentarSalario(porcentajeAumento) })
  }
}

class Sociedad {
  const property procesos = []

  method agregarProceso(p) { procesos.add(p) }

  method jornadaDeLucha(organizacion) {
    procesos.forEach({ p => organizacion.aplicarMedida(p) })
  }
}
```
