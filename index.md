## Inyección de dependencias
### Introducción

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
For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/ZoraidaRiosAjdie/Inyecci-n-de-dependencias/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
