## Inyección de dependencias
### ¿Qué es una dependencia?
Para empezar definamos qué es una dependencia, en la vida real una dependencia entre objetos puede ser la de un auto y una gasolinera. Para que el automóvil funcione, es necesario que el tanque tenga gasolina y es necesario que hacer uso de una gasolinera de manera regular para abastecerlo. Pero la dependencia aunque existente no se reduce únicamente a un solo establecimiento.

Pues siempre que la gasolinera tenga el combustible y funcione correctamente, el automóvil se mantendrá con una fuente de energía y no estará ligado por completo a un solo establecimiento(no se encontrará con una dependencia fuertemente acoplada).

Para responder qué es una dependencia desde el punto de vista de código, usaremos un ejemplo en Java. En este ejemplo tenemos la clase Schumacher  que implementa la interface Piloto, que tiene el método conducir.
```markdown
public class Schumacher implements&nbsp;Piloto {
 
   private Vehiculo vehiculo;
 
   public Schumacher(){
      vehiculo = new Ferrari();
   }
 
   @Override
 
   public void pilotar() {
      vehiculo.conducir();
   }
}
```
En el ejemplo anterior,  la clase Schumacher es dependiente de la interfaz Vehiculo que es implementada por la clase Ferrari en el constructor. Por lo anterior, se puede decir que Shumacher se encuentra fuertemente acoplado a Ferrari, pero qué pasa si queremos que Shumacher sea capaz de pilotar un Lamborghini o un BMW, para este caso la forma de la implementación se encuentra limitada y no es muy flexible. Entonces es cuando la inyección de dependencias llega a rescatarnos, o más bien a Schumacher…
### Definición de inyeccíon de dependencia
  Es un patrón de diseño orientado a objetos, en el que se suministran objetos a una clase en lugar de ser la propia clase la que cree dichos objetos. Esos objetos cumplen contratos que necesitan nuestras clases para poder funcionar (de ahí el concepto de dependencia). Nuestras clases no crean los objetos que necesitan, sino que se los suministra otra clase 'contenedora' que inyectará la implementación deseada a nuestro contrato.
  Un ejemplo sencillo

```markdown
public class ServicioImpresion {
  ServicioEnvio servicioA;
  ServicioPDF servicioB;
  
       public ServicioImpresion() {
    
    this.servicioA= new ServicioEnvio();
    this.servicioB= new ServicioPDF();
  }
  public void imprimir() {
    
    servicioA.enviar();
    servicioB.pdf();
  }
}
```
```markdown
public class ServicioEnvio {
  public void enviar() {
    
    System.out.println("enviando el documento a imprimir");
  }
}
```
```markdown
public class ServicioPDF {
  public void pdf() {
    
    System.out.println("imprimiendo el documento en formato pdf");
  }
}
```
Se puede realizar la misma operación inyectando las dependencias al ServicioImpresión y que no sea él el que tenga que definirlas en el constructor.

Este parece en principio un cambio sin importancia, el código quedaría:
```markdown
package com.arquitecturajava.parte3;
public class ServicioImpresion {
  ServicioEnvio servicioA;
  ServicioPDF servicioB;
  
  public ServicioImpresion(ServicioEnvio servicioA,ServicioPDF servicioB) {
    
    this.servicioA= servicioA;
    this.servicioB= servicioB;
  }
  public void imprimir() {
    
    servicioA.enviar();
    servicioB.pdf();
  }
}
```
```markdown
public class Principal {
  public static void main(String[] args) {
  
         ServicioImpresion miServicio=
         new ServicioImpresion(new ServicioEnvio(),new ServicioPDF());
    
  miServicio.imprimir();
  }
}
```
El resultado sigue siendo el mismo. Acabamos de inyectar las dependencias en el servicio desde nuestro programa main . ¿Qué ventajas aporta esto? . La realidad es que en principio parece que ninguna . Pero hay algo que ha cambiado ya no es el propio servicio el responsable de definir sus dependencias sino que lo es el programa principal.  Esto abre las puertas a la extensibilidad. Es decir ¿tenemos nosotros siempre que inyectar las mismas dependencias al servicio?. La respuesta parece obvia… claro que sí están ya definidas. Sin embargo la respuesta es NO , nosotros podemos cambiar el tipo de dependencia que inyectamos , simplemente extendiendo una de nuestras clases y cambiando el comportamiento, vamos a verlo.
```markdown
public class ServicioEnvioAspecto extends ServicioEnvio {
  @Override
  public void enviar() {
    
  System.out.println("haciendo log del correo que vamos a enviar");
    
  super.enviar();
  }
  
}
```
Acabamos de crear una clase que extiende ServicioEnvio y añade una funcionalidad adicional de log que hace un “log” del correo que enviamos . Ahora es tan sencillo como decirle al programa principal que cuando inyecte la dependencia no inyecte el ServicioEnvio sino el ServicioEnvioAspecto de esta forma habremos cambiado el comportamiento de forma considerable.
```markdown
public class ServicioEnvioAspecto extends ServicioEnvio {
  @Override
  public void enviar() {
    
  System.out.println("haciendo log del correo que vamos a enviar");
    
  super.enviar();
  }
  
}
```
Acabamos de modificar el comportamiento de nuestro programa de forma significativa gracias al uso del concepto de inyección de dependencia.

La inyección de dependencia nos permite inyectar otras clases y añadir funcionalidad transversal a medida. Este patrón de diseño es el que abre la puerta a frameworks como Spring utilizando el concepto de inyección de dependencia de una forma más avanzada. En estos framework los aspectos que se añaden a nuestras clases son múltiples y la complejidad alta.
