'''javascript
/**
 * Created by wac on 16-8-16.
 */

    var text = window.atob(basestr.split(",")[1]);
    var buffer = new Uint8Array(text.length);
    var pecent = 0, loop = null;
    for (var i = 0; i < text.length; i++) {
        buffer[i] = text.charCodeAt(i);
    }
    var type = basestr.split(",")[0];
    type = type.split(":")[1].split(";")[0];
    var blob = getBlob([buffer], type);
    var xhr = new XMLHttpRequest();
    var formdata = getFormData();
    formdata.append('imagefile', blob);

    posturl = "http://host/api/FaceRecognition";
    xhr.open('post', posturl, true);
    xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
    //your accessToken after Bearer
    xhr.setRequestHeader("Authorization", "Bearer 156d9018d20c8c2eab32a883034ad0242f4bf2e9a753960eb5ebae3733b810df");
    xhr.send("img=" + encodeURIComponent(basestr));
    xhr.onreadystatechange = function () {
        if (xhr.readyState == 4 && xhr.status == 200) {
            //for example:
            //xhr.responseText = '{"FaceRecognition": {"face0": [89,34,22,22]}}';
            // you can deal with xhr.responseText like this:
            // var jsonData = JSON.parse(xhr.responseText);
            // var str = JSON.stringify(jsonData, undefined, 2);
        }
    };
'''