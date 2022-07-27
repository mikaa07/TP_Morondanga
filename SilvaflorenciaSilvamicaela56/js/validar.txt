//random variable a la que le deposito un num aleatorio

let random= Math.floor(Math.random() *50 + 1);


//asocio captchagenerado al id captchagenerado(boton).

//busco en document el elemento id:captchagenerado

let captchaGenerado = document.getElementById("captchaGenerado");

//agarro nuevo captchagenerado y le doy el boton aleatorio

//que tenia en random

captchaGenerado.value =random;

// los botones son los unicos que llevan value

// creo resultado y lo asocio al boton enviar

let resultado = document.getElementById("comparar");

//comparar es el boton enviar

//creo un evento(que cada vez que clickeo valide)

resultado.addEventListener("click",validacion);


function validacion(){
    let captchaUsuario = document.getElementById("captchaUsuario");
    if(random !=captchaUsuario.value){
        alert("El captcha es invalido");
    }
    else{
        alert("su respuesta fue enviada con exito");
    }    
}


    