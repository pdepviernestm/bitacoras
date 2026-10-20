# Clases abstactas y super
<img width="736" height="686" alt="image" src="https://github.com/user-attachments/assets/f87e32c0-7063-41c1-8385-57787e99850b" />
![Views](https://github-views-counter-scnx.onrender.com/github/pdepviernestm/bitacoras/2026/Clase-18-ClasesAbstractasYSuper.md.svg?color=green&style=flat-square)

fecha: 16/10/2026

## Explicación de lo visto en clase
1)
  a)
    Falta de delegación (Declaratividad): El método lanzarEchizo del estudiante realiza dos tareas distintas: buscar la mejor varita y luego indicarle a esa varita que lance el hechizo. Para mejorar la declaratividad, la lógica de encontrar la mejor varita debería extraerse a un método propio (por ejemplo, mejorVarita()).
  b)
    ¿Estudiante y Varita son polimórficos?
    No, no son polimórficos entre sí. El polimorfismo ocurre cuando un objeto (el emisor) puede enviarle el mismo mensaje a distintos objetos (los receptores) sin importar de qué clase sean, y estos responden de manera diferente. En este caso, Estudiante y Varita no comparten una interfaz común ni son intercambiables; el Estudiante conoce y usa a la Varita. Los que sí serán polimórficos más adelante son los distintos tipos de varitas (Estrella, Juguete, Corazón) respecto al Estudiante, y los distintos tipos de nubes respecto a las varitas.
  c)


```wollok

class Estudiante {
    var nivelDeFelicidad
    const varitasMagicas
    
    method lanzarHechizo(nube) {
        self.mejorVarita().lanzarHechizo(nube)
    }
    
    method mejorVarita() = varitasMagicas.max({ varita => varita.potencia() })
}

class Varita {
    const potencia
    
    method potencia() = potencia
    
    method lanzarHechizo(nube) {
        // Lógica a implementar
    }
}

```
### Súper en el Requerimiento 6 (Nube de Tormenta)

Enunciado:
>"Las nubes de tormenta tienen resistencia. Pierden tristeza como cualquier nube gris, pero la tristeza que pierden es la cantidad menos la resistencia".
Resolución:
La clase NubeTormenta hereda de NubeGris. Para modificar la cantidad de tristeza a perder sin descartar la regla base (que la tristeza no baje de cero), debe redefinir el método mediante override method `perderTristeza(cantidad)`. En su interior, calcula la cantidad neta a restar (cantidad - resistencia) y utiliza la instrucción super(cantidadNeta). Esto invoca explícitamente el método perderTristeza definido en la superclase NubeGris, inyectando el nuevo valor procesado y reutilizando la lógica original de asignación y límite en cero.
```wollok
class NubeGris {
    var tristeza

    method perderTristeza(cantidad) {
        tristeza = 0.max(tristeza - cantidad)
    }
}

class NubeTormenta inherits NubeGris {
    const resistencia

    override method perderTristeza(cantidad) {
        super(0.max(cantidad - resistencia))
    }
}
```
### Clase Abstracta en los Requerimientos 8 y 9 (Tipos de varita y Efecto Especial)

Enunciado:
>"Existen distintos tipos de varitas, y cada una tiene su potencia... (Acá se toca tema clase-abstracta) Además de reducir la tristeza, las varitas pueden aplicar un efecto especial al hechizar una nube... Cada tipo de varita aplica un efecto distinto".

Explicación:
La clase Varita centraliza la lógica general en el método `lanzarHechizo(nube)`. Sin embargo, las reglas para calcular la potencia y aplicar el efecto especial cambian drásticamente según el tipo de varita. Como la clase Varita genérica no posee una implementación por defecto válida para estas acciones, declara los métodos `potencia()` y `efectoEspecial(nube)` sin cuerpo. Esto convierte a Varita en una clase abstracta, impidiendo su instanciación directa y forzando contractualmente a las subclases (Estrella, Juguete, Corazon) a proveer el código específico para esos mensajes.




```wollok
class Varita {
    var brillo

    method lanzarHechizo(nube) {
        if (self.puedeLanzarHechizo()) {
            if (brillo > 80) {
                self.efectoEspecial(nube)
            }
            nube.perderTristeza(self.potencia())
            self.generarDesgaste()
        }
    }

    // Métodos abstractos (sin cuerpo)
    method potencia()
    method efectoEspecial(nube)
}

class Estrella inherits Varita {
    override method potencia() = 100 - brillo

    override method efectoEspecial(nube) {
        brillo = brillo * 2
    }
}

```
Diagrama de clases final
<img width="847" height="737" alt="image" src="https://github.com/user-attachments/assets/22913466-14c1-4959-ba19-de36568de001" />

