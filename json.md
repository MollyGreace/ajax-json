<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>

<body style="text-align:center" onload="callCustemer();">

  <h1>Customer</h1>
  <div>
    ID:
    <input type="number" id="txtId">
    <br>
    <br> Name:
    <input type="text" id="txtName">
    <br>
    <br> Age:
    <input type="number" id="txtAge">
    <br>
    <br>
    <button id="btnSave">Guardar</button>
    <button id="btnCall" onclick="callCustemer();">Mostrar</button>
    <button id="btnReset" onclick="reset();">Reset</button>
    <br><br>
    <table style="width:100%" id="tabla"> </table>
  </div>

  <script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
  <script>
    $(document).ready(function () {
      $("#btnSave").click(function () { 
        var formData = {
          id: parseInt($("#txtId").val()),
        name: $("#txtName").val(),
        age: parseInt($("#txtAge").val())
    }

    $.ajax({
        type: "POST",
        contentType: "application/json",
        url: "http://54.202.16.58/saveCustomer",
        data: JSON.stringify(formData),
        dataType: 'json',
        complete: function(data) {
          alert('Registro añadido');
          callCustemer();
        }
    });

});
});

function reset(){
  $.ajax({
        type: "POST",
        contentType: "application/json",
        url: "http://54.202.16.58/resetCustomer",
        data: "",
        dataType: 'json',
        complete: function(data) {
          alert('Reset');
        }
    });
    callCustemer();
}

function callCustemer(){
      $.ajax({
        url: "http://54.202.16.58/allCustomer",
        /* Llamamos a tu archivo */
        data: "",
        /* Ponemos los parametros de ser necesarios */
        type: "GET",
        contentType: "text/json",
        dataType: "json",
        /* Esto es lo que indica que la respuesta será un objeto JSon */
        success: function (data) {
          /* Supongamos que #contenido es el tbody de tu tabla */
          /* Inicializamos tu tabla */
          $("#tabla").html('');
          $('#tabla').css({
            'border': '2px solid',
          });
          /* Vemos que la respuesta no este vacía y sea una arreglo */
          if (data != null && $.isArray(data)) {
            /* Recorremos tu respuesta con each */
            $.each(data, function (index, value) {
              /* Vamos agregando a nuestra tabla las filas necesarias */
              $("#tabla").append("<tr><td>" + value.id + "</td><td>" + value.name + "</td><td>" + value.age +
                "</td></tr>");
            });
          }
        }
      });
    }

  </script>
</body>

</html>