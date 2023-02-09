Code for Python, Flask server. Add route "/" with GET method and return index.html Add route "/jpgtopng" with GET method and return jpgtopng.html Add I route "/pngtojpg" with GET method and return pngtojpg.html Add route "/webptopng" with GET method and return webptopng.html Add route "/bmptopng" with GET method and return bmptopng.html Add route "/pngtopdf" with GET method and return pngtopdf.html

Here is an example of how you can create a Flask server with the routes and methods specified in your question:

```python
from flask import Flask, render_template

app = Flask(__name__)

@app.route("/", methods=["GET"])
def index():
    return render_template("index.html")

@app.route("/jpgtopng", methods=["GET"])
def jpgtopng():
    return render_template("jpgtopng.html")

@app.route("/pngtojpg", methods=["GET"])
def pngtojpg():
    return render_template("pngtojpg.html")

@app.route("/webptopng", methods=["GET"])
def webptopng():
    return render_template("webptopng.html")

@app.route("/bmptopng", methods=["GET"])
def bmptopng():
    return render_template("bmptopng.html")

@app.route("/pngtopdf", methods=["GET"])
def pngtopdf():
    return render_template("pngtopdf.html")

if __name__ == "__main__":
    app.run()
```

This is a simple example, you can improve this by adding the functionality to convert images, handling errors, and more. Keep in mind that this will only return the templates, but not perform any image conversion.

Code for index.html file. create a div and add these 5 links as list in it with href= appropiate route from our flask server. JPG to PNG converter PNG to JPG converter WEBP to PNG converter BMP to PNG converter PNG to PDF converter I

Here is an example of how you can create an `index.html` file with a div containing the links to the image conversion routes:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Image Converter</title>
  </head>
  <body>
    <div>
      <ul>
        <li><a href="/jpgtopng">JPG to PNG converter</a></li>
        <li><a href="/pngtojpg">PNG to JPG converter</a></li>
        <li><a href="/webptopng">WEBP to PNG converter</a></li>
        <li><a href="/bmptopng">BMP to PNG converter</a></li>
        <li><a href="/pngtopdf">PNG to PDF converter</a></li>
      </ul>
    </div>
  </body>
</html>
```

This is a basic example and you can add more styles, images, and information to your website. It's important to note that the routes specified in the href attributes should match the routes defined in your Flask server code.

Code for jpgtopng.html file. Display input form, so user can select image from their computer. Also add a "Convert to PNG" button. onclick button, check if image is selected and image is jpg format and less than 5 MB if true then send POST request to route "/api/jpgtopng" with image as parameter. else show relevant error like select a image or image is not jpg format

Here is an example of how you can create a `jpgtopng.html` file with an input form for selecting an image and a "Convert to PNG" button:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>JPG to PNG Converter</title>
    <script>
      function convertToPNG() {
        var fileInput = document.getElementById("fileInput");
        var file = fileInput.files[0];

        if (!file) {
          alert("Please select an image.");
          return;
        }

        if (file.type !== "image/jpeg") {
          alert("Invalid file format. Please select a JPG image.");
          return;
        }

        if (file.size > 5000000) {
          alert("File size is too large. Please select an image less than 5 MB.");
          return;
        }

        var formData = new FormData();
        formData.append("image", file);

        var xhr = new XMLHttpRequest();
        xhr.open("POST", "/api/jpgtopng", true);
        xhr.onreadystatechange = function() {
          if (xhr.readyState === 4 && xhr.status === 200) {
            alert("Image converted successfully!");
          }
        };
        xhr.send(formData);
      }
    </script>
  </head>
  <body>
    <form>
      <input type="file" id="fileInput" accept="image/jpeg">
      <br>
      <button type="button" onclick="convertToPNG()">Convert to PNG</button>
    </form>
  </body>
</html>
```

This is a basic example and you can add more functionality, such as displaying the converted image or handling errors. It's important to note that the route specified in the xhr.open method should match the route defined in your Flask server code, that handle the image conversion using libraries such as PIL and return the converted image.

Also, it's important to use a secure way of handling user's files and prevent any malicious behavior.

Code for bmptopng.html file. Display input form, so user can select image from their computer. Also add a "Convert to PNG" button. onclick button, check if image is selected and image is bmp format and less than 5 MB if true then send POST request to route "/api/bmptopng" with image as parameter. else show relevant error like select a image or image is not bmp format

Here is an example of how you can create a `bmptopng.html` file with an input form for selecting an image and a "Convert to PNG" button:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>BMP to PNG Converter</title>
    <script>
      function convertToPNG() {
        var fileInput = document.getElementById("fileInput");
        var file = fileInput.files[0];

        if (!file) {
          alert("Please select an image.");
          return;
        }

        if (file.type !== "image/bmp") {
          alert("Invalid file format. Please select a BMP image.");
          return;
        }

        if (file.size > 5000000) {
          alert("File size is too large. Please select an image less than 5 MB.");
          return;
        }

        var formData = new FormData();
        formData.append("image", file);

        var xhr = new XMLHttpRequest();
        xhr.open("POST", "/api/bmptopng", true);
        xhr.onreadystatechange = function() {
          if (xhr.readyState === 4 && xhr.status === 200) {
            alert("Image converted successfully!");
          }
        };
        xhr.send(formData);
      }
    </script>
  </head>
  <body>
    <form>
      <input type="file" id="fileInput" accept="image/bmp">
      <br>
      <button type="button" onclick="convertToPNG()">Convert to PNG</button>
    </form>
  </body>
</html>
```

This is a basic example and you can add more functionality, such as displaying the converted image or handling errors. It's important to note that the route specified in the xhr.open method should match the route defined in your Flask server code, that handle the image conversion using libraries such as PIL and return the converted image.

Also, it's important to use a secure way of handling user's files and prevent any malicious behavior.

Code for pngtojpg.html file. Display input form, so user can select image from their computer. Also add a "Convert to JPG" button. onclick button, check if image is selected and image is png format and less than 5 MB if true then send POST request to route "/api/pngtojpg" with image as parameter. else show relevant error like select a image or image is not png format

Here is an example of how you can create a `pngtojpg.html` file with an input form for selecting an image and a "Convert to JPG" button:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>PNG to JPG Converter</title>
    <script>
      function convertToJPG() {
        var fileInput = document.getElementById("fileInput");
        var file = fileInput.files[0];

        if (!file) {
          alert("Please select an image.");
          return;
        }

        if (file.type !== "image/png") {
          alert("Invalid file format. Please select a PNG image.");
          return;
        }

        if (file.size > 5000000) {
          alert("File size is too large. Please select an image less than 5 MB.");
          return;
        }

        var formData = new FormData();
        formData.append("image", file);

        var xhr = new XMLHttpRequest();
        xhr.open("POST", "/api/pngtojpg", true);
        xhr.onreadystatechange = function() {
          if (xhr.readyState === 4 && xhr.status === 200) {
            alert("Image converted successfully!");
          }
        };
        xhr.send(formData);
      }
    </script>
  </head>
  <body>
    <form>
      <input type="file" id="fileInput" accept="image/png">
      <br>
      <button type="button" onclick="convertToJPG()">Convert to JPG</button>
    </form>
  </body>
</html>
```

This is a basic example and you can add more functionality, such as displaying the converted image or handling errors. It's important to note that the route specified in the xhr.open method should match the route defined in your Flask server code, that handle the image conversion using libraries such as PIL and return the converted image.

Also, it's important to use a secure way of handling user's files and prevent any malicious behavior.

Code for pngtopdf.html file. Display input form, so user can select image from their computer. Also add a "Convert to PDF" button. onclick button, check if image is selected and image is png format and less than 5 MB if true then send POST request to route "/api/pngtopdf" with image as parameter. else show relevant error like select a image or image is not pdf format

Here is an example of how you can create a `pngtopdf.html` file with an input form for selecting an image and a "Convert to PDF" button:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>PNG to PDF Converter</title>
    <script>
      function convertToPDF() {
        var fileInput = document.getElementById("fileInput");
        var file = fileInput.files[0];

        if (!file) {
          alert("Please select an image.");
          return;
        }

        if (file.type !== "image/png") {
          alert("Invalid file format. Please select a PNG image.");
          return;
        }

        if (file.size > 5000000) {
          alert("File size is too large. Please select an image less than 5 MB.");
          return;
        }

        var formData = new FormData();
        formData.append("image", file);

        var xhr = new XMLHttpRequest();
        xhr.open("POST", "/api/pngtopdf", true);
        xhr.onreadystatechange = function() {
          if (xhr.readyState === 4 && xhr.status === 200) {
            alert("Image converted successfully!");
          }
        };
        xhr.send(formData);
      }
    </script>
  </head>
  <body>
    <form>
      <input type="file" id="fileInput" accept="image/png">
      <br>
      <button type="button" onclick="convertToPDF()">Convert to PDF</button>
    </form>
  </body>
</html>
```

This is a basic example and you can add more functionality, such as displaying the converted pdf or handling errors. It's important to note that the route specified in the xhr.open method should match the route defined in your Flask server code, that handle the image conversion using libraries such as PIL or ReportLab and return the converted pdf.

Also, it's important to use a secure way of handling user's files and prevent any malicious behavior.

Code for webptopng.html file. Display input form, so user can select image from their computer. Also add a "Convert to PNG" button. onclick button, check if image is selected and image is webp format and less than 5 MB if true then send POST request to route "/api/webptopng" with image as parameter. else show relevant error like select a image or image is not webp format

```html
<!DOCTYPE html>
<html>
    <head>
        <title>WEBP to PNG Converter</title>
    </head>
    <body>
        <div>
            <form>
                <input type="file" id="input_image">
                <br>
                <button id="convert_button" onclick="handleConversion()">Convert to PNG</button>
            </form>
            <br>
            <div id="error_message"></div>
        </div>

        <script>
            function handleConversion() {
                var input_image = document.getElementById("input_image");
                var error_message = document.getElementById("error_message");
                
                if (!input_image.files[0]) {
                    error_message.innerHTML = "Please select an image";
                    return;
                }

                var file = input_image.files[0];
                var file_extension = file.name.split(".")[file.name.split(".").length - 1];

                if (file_extension != "webp") {
                    error_message.innerHTML = "Selected file is not a WEBP image";
                    return;
                }

                if (file.size > 5000000) {
                    error_message.innerHTML = "Selected image is larger than 5MB";
                    return;
                }

                var formData = new FormData();
                formData.append("input_image", file);

                fetch("/api/webptopng", {
                    method: "POST",
                    body: formData
                })
                .then(response => response.json())
                .then(response => {
                    if (response.status == "success") {
                        window.location.href = response.data;
                    } else {
                        error_message.innerHTML = response.message;
                    }
                })
                .catch(error => {
                    error_message.innerHTML = "An error occurred while converting the image";
                });
            }
        </script>
    </body>
</html>
```

This is a basic example of the code for the `webptopng.html` file. This code displays an input form that allows the user to select an image from their computer, and a "Convert to PNG" button. When the button is clicked, the code checks if an image is selected and if the selected image is in the WEBP format and less than 5MB in size. If these conditions are met, it sends a POST request to the route "/api/webptopng" with the selected image as a parameter. If the conditions are not met, it displays an error message to the user. Please note that this code requires a server-side script that can handle the POST request and convert the image from WEBP to PNG format. This script should be added to the flask app and should be located in the '/api/webptopng' route.

for flask server, create a route "/api/jpgtopng" with POST method. take the image from parameter and convert into PNG. make response object with image as attachment and return it

```python
from flask import Flask, request, Response
import imageio

app = Flask(__name__)

@app.route("/api/jpgtopng", methods=["POST"])
def jpg_to_png():
    image = request.files["image"]
    if image and image.content_type == "image/jpeg" and image.content_length <= 5000000:
        image_data = image.read()
        image_data = imageio.imread(image_data, as_gray=False, pilmode="RGB")
        image_data = imageio.imwrite(image_data, format='png')
        response = Response(image_data, content_type="image/png", content_disposition='attachment; filename="converted.png"')
        return response
    else:
        return "Please select a valid JPG image with size less than 5 MB"

if __name__ == "__main__":
    app.run(debug=True)
```

Please note that this code uses the `imageio` library, which needs to be installed in your environment. You can do this by running `pip install imageio`.

for flask server, create a route "/api/pngtojpg" with POST method. convert png image from parameter into JPG format dont save the image on server. just return JPG image as downloadable attachment.

```python
from flask import Flask, request, Response
import imageio

app = Flask(__name__)

@app.route("/api/pngtojpg", methods=["POST"])
def png_to_jpg():
    image = request.files["image"]
    if image and image.content_type == "image/png" and image.content_length <= 5000000:
        image_data = image.read()
        image_data = imageio.imread(image_data, as_gray=False, pilmode="RGB")
        image_data = imageio.imwrite(image_data, format='jpg')
        response = Response(image_data, content_type="image/jpeg", content_disposition='attachment; filename="converted.jpg"')
        return response
    else:
        return "Please select a valid PNG image with size less than 5 MB"

if __name__ == "__main__":
    app.run(debug=True)
```

Please note that this code uses the `imageio` library, which needs to be installed in your environment. You can do this by running `pip install imageio`.

for flask server, create a route "/api/webptopng" with POST method. convert webp image from parameter into PNG format dont save the image on server. just return png image as downloadable attachment.

```python
from flask import Flask, request, Response
import imageio

app = Flask(__name__)

@app.route("/api/webptopng", methods=["POST"])
def webp_to_png():
    image = request.files["image"]
    if image and image.content_type == "image/webp" and image.content_length <= 5000000:
        image_data = image.read()
        image_data = imageio.imread(image_data, plugin='webp')
        image_data = imageio.imwrite(image_data, format='png')
        response = Response(image_data, content_type="image/png", content_disposition='attachment; filename="converted.png"')
        return response
    else:
        return "Please select a valid WEBP image with size less than 5 MB"

if __name__ == "__main__":
    app.run(debug=True)
```

Please note that this code uses the `imageio` library, which needs to be installed in your environment. You can do this by running `pip install imageio`. Additionally, `imageio[webp]` may be needed to support the webp format.

for flask server, create a route "/api/bmptopng" with POST method. convert bmp image from parameter into PNG format dont save the image on server. just return png image as downloadable attachment.

```python
from flask import Flask, request, send_file
import cv2

app = Flask(__name__)

@app.route("/api/bmptopng", methods=["POST"])
def bmptopng():
    if "image" not in request.files:
        return "No image selected"

    image = request.files["image"]
    if not image.filename.endswith(".bmp"):
        return "Invalid image format. Please select a BMP image."

    image_data = image.read()
    image_np = cv2.imdecode(np.fromstring(image_data, np.uint8), cv2.IMREAD_UNCHANGED)

    ret, buffer = cv2.imencode('.png', image_np)
    response = make_response(buffer.tobytes())
    response.headers.set('Content-Type', 'image/png')
    response.headers.set('Content-Disposition', 'attachment', filename='converted.png')
    return response

if __name__ == "__main__":
    app.run(debug=True)
```

This is a basic example of how you could create a route in a Flask server to convert a BMP image to PNG format. The code first checks if an image was sent in the request and if the image is in BMP format. Then it reads the image data, converts it to a numpy array using cv2 library, and encodes it as a PNG image using cv2.imencode. Finally, it creates a response object with the PNG image data and sets the content type and disposition headers to make it a downloadable attachment. Please note that, this is just a skeleton code and you may need to handle the other error like image size, handling exceptions and also installing the required libraries like cv2, numpy etc.

for flask server, create a route "/api/pngtopdf" with POST method. convert png image from parameter into PDF format dont save the pdf on server. just return PDF file as downloadable attachment.

Here's an example of how you could implement the "/api/pngtopdf" route in a Flask server:

```python
from flask import Flask, request, Response
from PIL import Image
import io

app = Flask(__name__)

@app.route("/api/pngtopdf", methods=["POST"])
def png_to_pdf():
    # Get the image from the request
    image = request.files["image"]
    
    # Open the image using PIL
    img = Image.open(image)
    
    # Convert the image to PDF format
    pdf_buffer = io.BytesIO()
    img.save(pdf_buffer, "PDF", resolution=100.0)
    pdf_buffer.seek(0)
    
    # Create the response object with the PDF as an attachment
    response = Response(pdf_buffer, mimetype="application/pdf", content_type="attachment;filename=image.pdf")
    response.headers["Content-Disposition"] = "attachment; filename=image.pdf"
    
    return response
```

This code uses the PIL library to convert the PNG image to a PDF format and then creates a response object with the PDF file as an attachment. The filename for the PDF file is set as 'image.pdf'.

Keep in mind that this is a basic example and it will not handle validation for the image type, size,etc.