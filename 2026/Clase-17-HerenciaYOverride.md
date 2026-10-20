# Herencia y Override

<img width="35%" height="50%" alt="image" src="https://github.com/user-attachments/assets/60d11750-188b-4d5d-964b-b0fd8d441f73" />

**Fecha**: viernes 9/10/2026
## Visto durante la clase
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


