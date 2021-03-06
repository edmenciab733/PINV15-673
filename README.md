# PINV15-673

En google colab se encuentra todo el codigo de studio para el procesamiento de imagenes.

El Índice de vegetación de diferencia normalizada, también conocido como NDVI por sus siglas en inglés, es un índice usado para estimar la cantidad, calidad y desarrollo de la vegetación con base a la medición, por medio de sensores remotos instalados comúnmente desde una plataforma espacial, de la intensidad de la radiación de ciertas bandas del espectro electromagnético que la vegetación emite o refleja.

![alt text](https://wikimedia.org/api/rest_v1/media/math/render/svg/d76fc98a8d6c852f9aa4ea36b947e2134d294a3e)

Ejemplo de satelite LANDSAT 8 

![alt text](https://upload.wikimedia.org/wikipedia/commons/thumb/5/57/NDVI_2017-09-10.png/440px-NDVI_2017-09-10.png)

La falta de una camara infraroja nos hizo buscar otras tecnicas para analizar las imagenes, que presentamos abajo en Google Colab

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/gist/edmenciab733/ba995dba7b1a940fe25342acea10309c/pinv.ipynb)




#   Funcion de procesamiento de imagenes

- Carga de datos en Google maps
```javascript
function initMap() {    
    map = new google.maps.Map(document.getElementById("map"), {
        center: {
            lat: -26.500121,
            lng: -55.791047
        },
        mapTypeId: "satellite",
        zoom: 16
    });
    google.maps.event.addListener(map, "drag", function() {
        marker.setPosition(this.getCenter()); // set marker position to map center
        console.log(this.getCenter().lat(), this.getCenter().lng())
    });
    var infowindow = new google.maps.InfoWindow();
    var marker, i;
    for (i = 0; i < locations.length; i++) {
        marker = new google.maps.Marker({
            position: new google.maps.LatLng(locations[i][1], locations[i][2]),
            map: map
        });
        google.maps.event.addListener(marker, "click", (function(marker, i) {
            var content = "<img border="0" width="500" height="600" class="img-fluid" align="Left" src="http://senuelo.net/pinv/DSC04148.JPG"> "
            content += "<h4>Latitud: " + marker.getPosition() + "</h4>"
            return function() {
                infowindow.setContent(content);
                infowindow.open(map, marker);
            }
       })(marker, i));
    }
}
function getExif(img) {
    debugger;
    EXIF.getData(img, function() {
        var allMetaData = EXIF.getAllTags(this);
        var allMetaDataSpan = document.getElementById("allMetaDataSpan");
        console.log(JSON.stringify(allMetaData, null, "\t"));
    });
}
$("body").on("click", "img", function() {
    var src = $(this).clone()[0].src;
    var img = $("<img />").attr({
        "id": "myImage",
        "src": src,
        "class": "img-fluid"
    });
    $("#modal_image .modal-body").empty();
    $("#modal_image .modal-body").html(img);
    var img1 = document.getElementById("myImage");
    getExif(img1);
    $("#modal_image").modal("show");
})
```

#GeoEtiquetado de imagenes
- Instalamo la libreria gpsphoto para manejo de metada de gps
```bash
pip install gpsphoto
```

```python
from GPSPhoto import gpsphoto
#Se agregar una imagen de la carpeta
files = [".pinv/DSC04160.JPG"]
for file in files:
    photo = gpsphoto.GPSPhoto(file)
     #los parametros de ubicacion se agrega como metadata de la imagen
    info = gpsphoto.GPSInfo((-26.502982, -55.808099), timeStamp='1970:01:01 09:05:05')
    photo.modGPSData(info, file)
    data = gpsphoto.getGPSData(file)
    for tag in data.keys():
        print("%s: %s" % (tag, data[tag]))
        print("-------------------\n")


```


