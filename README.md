# Prueba técnica Zoho
Esta función permite procesar los datos introducidos en los campos de un formulario de Zoho y, a continuación, actualizar con ellos la información que ya poseemos (si es que la tenemos).

# Descripción de la función
Teniendo en cuenta que tenemos los siguientes campos de registro:
- Aux_Newsletter (campo auxiliar, entrada del formulario)
- Newsletter (campo principal)
- Historico_Newsletter (campo de histórico)

  
Esta función toma un dato tipo map como entrada (formData) y devuelve la información actualizada en el mismo tipo de dato:

map updateNewsletterHistory(map formData) {

    // Primero declaramos las variables indicando el tipo, en este caso string, ya que trabajamos con cadenas de texto. 
    // Obtenemos los valores con el método .get():
    
    string auxNewsletter = formData.get("Aux_Newsletter");
    string currentNewsletter = formData.get("Newsletter");
    string historico = formData.get("Historico_Newsletter");

    // Asignamos como vacíos los posibles valores nulos de "currentNewsletter" e "historico" 
    // (los valores ya existentes) para evitar tanto errores
    // como excesos de anidaciones de if/else, lo que hará el código más fácil de mantener.
    
    if(currentNewsletter == null) { currentNewsletter = ""; }
    if(historico == null) { historico = ""; }

    // Comprobamos si Newsletter está vacío, de ser así, lo actualizamos con Aux_Newsletter con el método .put()
    // Utilizamos el método .trim() en currentNewsletter para eliminar los espacios en blanco 
    // al principio y al final de la cadena de texto.
    
    if(currentNewsletter.trim() == "") {
        formData.put("Newsletter", auxNewsletter);
    }

    // Si historico esta vacío, le asignamos Aux_Newsletter, ya que es el primer dato que aloja.
    // Si ya poseemos datos en historico, concatenamos un punto y coma (;) seguido de Aux_Newsletter.
    
    if(historico.trim() == "") {
        historico = auxNewsletter;
    }
    else {
        historico = historico + ";" + auxNewsletter;
    }

    // Finalmente, actualizamos el histórico con el nuevo valor y devolvemos formData modificado.

    formData.put("Historico_Newsletter", historico);

    return formData;
}

## Ejemplo de entrada

```json
{
  "Aux_Newsletter": "Celia",
  "Newsletter": "",  
  "Historico_Newsletter": "Jacobo;DepartamentoDesarrollo"  
}
```

## Ejemplo de salida

```json
{
  "Aux_Newsletter": "Celia",
  "Newsletter": "Celia",
  "Historico_Newsletter": "Jacobo;DepartamentoDesarrollo;Celia"
}
```
