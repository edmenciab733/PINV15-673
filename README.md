# PINV15-673

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/gist/edmenciab733/ba995dba7b1a940fe25342acea10309c/pinv.ipynb)


#   Funcion de procesamiento de imagenes
function initMap() {    
    
    map = new google.maps.Map(document.getElementById('map'), {
        center: {
            lat: -26.500121,
            lng: -55.791047
        },
        mapTypeId: 'satellite',
        zoom: 16
    });

    google.maps.event.addListener(map, 'drag', function() {
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

        google.maps.event.addListener(marker, 'click', (function(marker, i) {
            var content = '<img border="0" width="500" height="600" class="img-fluid" align="Left" src="http://senuelo.net/pinv/DSC04148.JPG"> '
            content += '<h4>Latitud: ' + marker.getPosition() + '</h4>'
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

$('body').on('click', 'img', function() {


    var src = $(this).clone()[0].src;
    var img = $('<img />').attr({
        'id': 'myImage',
        'src': src,

        'class': "img-fluid"
    });


    $('#modal_image .modal-body').empty();


    $('#modal_image .modal-body').html(img);
    var img1 = document.getElementById("myImage");
    getExif(img1);
    $('#modal_image').modal('show');

})

