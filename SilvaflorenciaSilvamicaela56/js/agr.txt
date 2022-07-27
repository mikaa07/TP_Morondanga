
//declara varible boton cargar que en realidad es el agregar
let btncargar= document.querySelector(".agregar");
//al boton cargar le da funcionalidad, cada vez que hago click crea una fila de la tabla
btncargar.addEventListener("click",crearfilatabla);

//declara variable botoncargar3 que en realidad es el agregar3
//le da funcionalidad
let btncargar3= document.querySelector(".agregar3");
btncargar3.addEventListener("click",agregar3);

//declara variable boton vaciar que en realidad es el vaciar
//le doy funcionalidad, le asigno el evento click con la funcion vaciar tabla
let btnvaciar= document.querySelector(".vaciar");
btnvaciar.addEventListener("click",vaciartabla);

//url del grupo donde guardamos nuestro arreglo de JSON
let url="http://web-unicen.herokuapp.com/api/groups/56/fixture";
//llamo funcion cargar
cargar();



function agregarFila(id,eq1,eq2){
    let cuadro= document.querySelector(".tabla"); //cuadro es la tabla
    //declara variable fila que es un string, con el html  de una fila , de lo que se muezstra en una fila}
    //el id va en el value del boton editra y del boton borrrar para despuess poder asignarle el evento con la func borrar y editar
    let fila= "<tr class='fila'><td>"+eq1+"</td><td>"+eq2+"</td><td><button class='editar' value='"+id+"'>Editar</button></td><td><button class='borrar' value='"+id+"'>Borrar</button></td></tr>";
     cuadro.innerHTML+=fila; //aca lo concatena, agarro todo el html de la tabla y concatenno la fila
}

//funcion agregar 3 llama 3 veces a la funcion crear fila tabla
function agregar3(){
    crearfilatabla ();
    crearfilatabla ();
    crearfilatabla ();
}


function vaciartabla(){
    let tabla=document.querySelector(".tabla");
    tabla.innerHTML=""; //innerHTML atributo que guarda todo el html de la tabla, al asignarle comillas sin nada lo borro del html
}




//obtengo los valores ingresados por el usuario y los guardo en el objeto fila
//fila es una variable objeto
function crearfilatabla (){
    let equipo1= document.getElementById("equipo1").value;
    let equipo2= document.querySelector("#equipo2").value;
    let fila = {
        "thing": {
            "equipo1":equipo1,
            "equipo2":equipo2
        }
      };
      //eejecuto funcion  fetch que se enccarga de agregar el objeto( es un JSON) al arreglo de JSON en la url con el metodo POST
     fetch(url, {
        "method": "POST",
        "headers": { "Content-Type": "application/json" },
        "body": JSON.stringify(fila)
        //.then quiere decir que cuando termine lo anterior va a realizar otra cosa
        // lo que estoy haciendo es: llamo a la funcion load que me refresca la tabla
     }).then(function(r){
        cargar();
        return r.json();
        
     });
   
}

function editarfila(y){  //Y es el id de la fila que se va a editar
    //getelementbyid y query selector son equivalentes, los dos son id
    let equipo1= document.getElementById("equipo1").value; //son los valores que tomo por teclado
    let equipo2= document.querySelector("#equipo2").value;
    let fila = {
        "thing": {
            "equipo1":equipo1,
            "equipo2":equipo2
        }
      };
    

    let id=y; 
    let url2= url+"/"+id;  //declaro variable url2 ,, concateno url con id para poder hacer el PUT, si no paso id por parametro no va a saber a que objeto hacrele el PUT
    fetch(url2, {  //hago un fetch con el metodo PUT, que sirve para editar
      method: 'PUT',
      mode: 'cors',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(fila)  //le paso el objeto fila con los valores por los cuales se van a editar
    })
    .then(r=>{console.log(r);cargar();});  //llamo func cargar, para refrescar la tabla, generarla de nuevo
  }




  //borra la fila en el servidor con el id del json pasado por parametro
  //actualiza la vista de la tabla
  function borrarfila(z){

    let id=z;
    let url1= url+ "/" +id;
    fetch(url1, {
      method: 'DELETE',  //utilizo metodo delete
      mode: 'cors',
      headers: {
        'Content-Type': 'application/json'
      },
    })
    .then(r=>{console.log(r);cargar();});
  }

//que hace la funcion cargar
  //refresca la tabla, primero vaciandola y ejecuta la funcion fetch que por defecto
  //hace un get de la url(agarro todos los valores del json, traigo todo lo que esta en json)
  
function cargar(){
    vaciartabla();
    let container=document.querySelector(".tabla");
     fetch(url).then(r => r.json())
    .then(json =>mostrar(container,json))//llamo a la fuuncion mostrar y le paso por parametro el container(que seria donde va la tabla) y el arreglo  de json(en el arreglo de json esta todo la info de la tabla que voy cargando)
    .catch(error =>container.innerHTML = "error");//si ocurre un error muestra "error"
}


function mostrar(container,json){
    for(let i=0; i<json.fixture.length; i++){//por cada elem del fixture llamo a la funcion agregar fila
      let eq1= json.fixture[i].thing.equipo1;//arreglo en esa posicion.cosa.equipo
      let eq2= json.fixture[i].thing.equipo2;
      let id=json.fixture[i]._id;
      agregarFila(id,eq1,eq2);//paso los datos que me devuelven las variables
      //las variables declaradas son string
      //le paso el id para que los botones editar y borrar , cada fila sepa a que objeto referirse
    }
    //llamo a la funcion evento edit y evento borrar
    asignarEventoEdit();
    asignarEventoBorrar();
 }

  //asigna evento recorriendo el arreglo de la clase de los botones de borrar
  function asignarEventoBorrar(){
    var botones = document.querySelectorAll('.borrar');//botones es un arreglo de todos los objeetos que tienen la clase borrar
    //selecciono todos los botones que tienen la clase borrar
    botones.forEach(function(boton) {//por cada boton borrar
      var z = boton.value;//z es el valor del boton que es en realidad el id de la fila
      //console log no hace nada en particular, es de guia para saber que las cosas andan
      boton.addEventListener('click', function() {borrarfila(z); console.log("sss");});//le agrego le agregoo el evento con la func borrar fila

    });
  }

  //asigna evento recorriendo el arreglo de la clase de los botones de editar
  function asignarEventoEdit(){
    var botonesEDIT = document.querySelectorAll('.editar');
    botonesEDIT.forEach(function(boton) {
      var z = boton.value;
      boton.addEventListener('click', function() {editarfila(z); console.log("sss");});
    });
  }


  let btnfiltro= document.querySelector(".filtro");
  btnfiltro.addEventListener("click",filtro);
  
  //sacado de la pagina w3school
  function filtro() {

  let input = document.querySelector('.equipofiltro');
  let filter = input.value.toUpperCase(); //filtro, aagarro el valor de lo que sale del teclado y con toUpperCase lo pasa a mayuscula para dps comparar mayus con mayus
  let table = document.getElementById("tabla"); //tomo la tabla
  let tr = table.getElementsByTagName("tr"); //filas de la tabla, agarra elementos segun el tag,segun la etiqueta tr

  
  for (i = 0; i < tr.length; i++) { //itera las filas de la tabla
    td = tr[i].getElementsByTagName("td")[0];  //toma la primer columna
    if (td) { //si dicha columna
      txtValue = td.textContent || td.innerText; //agarra el valor  de texto de la celda
      if (txtValue.toUpperCase().indexOf(filter) > -1) { //vuelve a hacer toUpperCase, pregunta si los textos son iguales
        tr[i].style.display = "";
      } else {
        tr[i].style.display = "none"; //oculta la fila
      }
    } 
  }

  }
    


  