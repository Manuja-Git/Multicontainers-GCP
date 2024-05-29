# Microservice-GCP
Above given are the files we can use for setting up a Microservice-GCP. Here i have also added a HTML page which will help to upload the files online on the GCP.
Step 1: You need to have Google Cloud Account- <a href="https://console.cloud.google.com/freetrial/signup"> Signup</a>
Once done you need to create a <a href="https://console.cloud.google.com/projectcreate">New project</a> 

![image](https://github.com/suragsp/Microservice-GCP/assets/104720115/cf3872e9-096a-4e57-b840-266b1dc24a22)

Step 2: You will be getting this:

![image](https://github.com/suragsp/Microservice-GCP/assets/104720115/07e4ddae-2bb2-4d6a-a304-0b46186052c2)

Step 3: Click on the 3 lines(ham-burger) on left side --> Select Cloud Storage --> Buckets.

![image](https://github.com/suragsp/Microservice-GCP/assets/104720115/299ed9a3-3ff8-4517-a654-b0db7d6a7185)

Step 4: Click on Create.

![image](https://github.com/suragsp/Microservice-GCP/assets/104720115/1b32e9da-2abf-4b05-ad1b-3db729f17b31)

Step 5.1: Give Name to your bucket.

![image](https://github.com/suragsp/Microservice-GCP/assets/104720115/3b4bf305-5d91-43c5-acd5-5324f027c529)

Step 5.2 : Choose where to store your data
            Location: us-central1 (Iowa)
            Location type: Region
            
![image](https://github.com/suragsp/Microservice-GCP/assets/104720115/fe6c1020-e02f-4f94-b509-b964ed374c92)

Step 5.3 : Click Create.

![image](https://github.com/suragsp/Microservice-GCP/assets/104720115/312dfc09-813d-4d63-8e11-9a02cb4c3492)

Step 5.4 : Public access will be prevented
This bucket is set to prevent exposure of its data on the public internet.
Keep this setting enabled unless you have a use case that requires public access (such as static website hosting). You can change it now or later. Learn more 
select : "Enforce public access prevention on this bucket" and click - "Confirm".

![image](https://github.com/suragsp/Microservice-GCP/assets/104720115/5d649552-498e-4210-a04e-960e3bc42a12)

Step 5.6 : Check your Bucket(Upload a file.

![image](https://github.com/suragsp/Microservice-GCP/assets/104720115/7ce9397a-ae20-42a9-bfa1-da21d86bd9ed)

Step 5.7 : It's Working.If not working <a href= "https://cloud.google.com/storage/docs/creating-buckets">Google guide here</a>

![image](https://github.com/suragsp/Microservice-GCP/assets/104720115/50b0a5d8-abee-44ea-baa2-bf664298678b)

<h1>For Key</h1>

Step 1:Click on the 3 lines(ham-burger) on left side --> I AM & Admin --> Service Accounts

![image](https://github.com/suragsp/Microservice-GCP/assets/104720115/1a12ce80-1b05-4e3e-8a7d-5427b69e98fe)

Step 2: Click on "Create Service Account".

![image](https://github.com/suragsp/Microservice-GCP/assets/104720115/987b4941-8d8e-4403-b5c7-fca5a917036a)

Step 3 : Service Account Name. --> Click on "Create and Continue".

![image](https://github.com/suragsp/Microservice-GCP/assets/104720115/1d698b31-e799-4758-9fab-4a99ab080e75)

Step 4: 

![image](https://github.com/suragsp/Microservice-GCP/assets/104720115/c056aa9c-179e-47b4-a733-71df54a05f49)

![image](https://github.com/suragsp/Microservice-GCP/assets/104720115/0afb5ca0-144e-4e46-b933-4198833e6434)


Note - As we are just making a demo, we are making the uploader as a owner else we need to give only the rights needed to them.(Based on their role) <a href="https://cloud.google.com/iam/docs/overview">IAM overview</a>
Optional Step - Grant users access to this service account.
If you want to add someone's Email ID you can do here else leave it blank.

![image](https://github.com/suragsp/Microservice-GCP/assets/104720115/fce28bac-abba-4d75-89af-38370da517eb)

Click Done.

![image](https://github.com/suragsp/Microservice-GCP/assets/104720115/c18a32f9-0ff8-4d6a-ac9c-10dc7f3adb8b)

Now Get the keys.

![image](https://github.com/suragsp/Microservice-GCP/assets/104720115/cd2a7afa-b594-4f52-8e39-971424a8cf3b)

Click on Add key --> Create new key.

![image](https://github.com/suragsp/Microservice-GCP/assets/104720115/1f7ba849-a4ce-4b86-bf0e-c34489333e4c)

Select .JSON --> Cleck "Create". (The key will be Downloaded auto matically-Keep it safe as it contains important data.)

![image](https://github.com/suragsp/Microservice-GCP/assets/104720115/5ec59b69-b440-4ce9-ae32-aec820140457)


I am using VScode for making Further code. You can just use normal notepad to create file and Powershell to execute.
<h1>I will First make app.py file and execute it to check if it works and then i will make a container and use docker to execute it</h1>

File name "<b>app.py</b>"
from flask import Flask, request, jsonify, render_template
from google.cloud import storage
import os

app = Flask(__name__)

# Set up Google Cloud Storage
os.environ['GOOGLE_APPLICATION_CREDENTIALS'] = 'cloudstorageuploader-c9d203ce6c6e.json'
client = storage.Client()
bucket = client.get_bucket('bucket-quickstart_cloudstorageuploader')

@app.route('/', methods=['GET'])
def index():
    return render_template('index.html')

@app.route('/upload', methods=['POST'])
def upload_file():
    file = request.files['file']
    if not file:
        return 'No file found', 400

    blob = bucket.blob(file.filename)
    blob.upload_from_string(file.read(), content_type=file.content_type)

    return jsonify({'message': 'File uploaded successfully', 'filename': file.filename})

if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0', port=5000)


<h1>Errors that might occur while you run</h1>
Error 1:Python not Found, 
Soltion Install Python and while installing python Include it in the path.

![image](https://github.com/suragsp/Microservice-GCP/assets/104720115/ddd9d80b-b371-4d65-a4ea-189ba156d49b)


<b>I will use Power shell to run "app.py".</b>

https://github.com/suragsp/Microservice-GCP/assets/104720115/c8dfd8ce-8e0e-47b6-b838-ca671f3c3e2b




Use Commands:
1. pip install flask google-cloud-storage
2. python app.py
Open the local host anc check (I have made a HTML file for uploading files you can also use postman app.)


https://github.com/suragsp/Microservice-GCP/assets/104720115/d16f92ec-3acb-4cab-b891-cdab7c8911e9


Using Postman to upload File.
Step 1: Select Send an API Request.

![image](https://github.com/suragsp/Microservice-GCP/assets/104720115/e75c7a2f-b695-416d-a697-789464c61984)

Step 2:Create a New Request:
Click on the "New" button and select "HTTP Request".
Set the request type to POST.
Enter the URL for your Flask endpoint (e.g., http://localhost:5000/upload).
Set Up the Body of the Request:

![image](https://github.com/suragsp/Microservice-GCP/assets/104720115/2fd2d28b-9111-4141-b9ac-ed23e090251d)


Click on the "Body" tab.

![image](https://github.com/suragsp/Microservice-GCP/assets/104720115/d3e137f2-9e09-4fc4-b1cc-c6fc2091c852)

Select the "form-data" option.
Add a new key for the file:
Set the key name to file.
Change the type from "Text" to "File".

Click the "Choose Files" button and select a file from your computer.
Send the Request:

Click the "Send" button to send the POST request to your Flask application.

![image](https://github.com/suragsp/Microservice-GCP/assets/104720115/c09f7fcf-31fa-435b-a160-47d1fcab0d20)

<h1>I have made a website which will be used by me to upload file instead of the Postman App.</h1>
index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>File Upload</title>
    <style>
        body {
            background-color: #f2f2f2; /* Set background color */
            font-family: Arial, sans-serif; /* Set default font family */
            margin: 0; /* Remove default margin */
            padding: 0; /* Remove default padding */
        }
        .banner {
            background-image: url('banner.jpg'); /* Set background image */
            background-size: cover; /* Cover the entire container with the background image */
            background-position: center; /* Center the background image */
            height: 300px; /* Set the height of the banner */
            display: flex; /* Use flexbox for alignment */
            justify-content: center; /* Center horizontally */
            align-items: center; /* Center vertically */
            color: white; /* Set text color to white */
            text-align: center; /* Center text */
        }
        .upload-form {
            margin-top: 20px; /* Add some space between the banner and the form */
            text-align: center; /* Center the form */
        }
        .upload-btn {
            background-color: #4CAF50; /* Set button background color */
            color: white; /* Set button text color */
            padding: 15px 30px; /* Add padding to the button */
            font-size: 16px; /* Set button font size */
            border: none; /* Remove button border */
            border-radius: 5px; /* Add border radius to button */
            cursor: pointer; /* Change cursor to pointer on hover */
            transition: background-color 0.3s; /* Add transition effect to button background color */
        }
        .upload-btn:hover {
            background-color: #45a049; /* Change button background color on hover */
        }
    </style>
</head>
<body>
    <div class="banner">

    </div>
    <div class="upload-form">
        <h1>Welcome to File Upload</h1>
        <form action="/upload" method="post" enctype="multipart/form-data">
            <input type="file" name="file" required>
            <button class="upload-btn" type="submit">Upload</button>
        </form>
    </div>
</body>
</html>

<h1>Now Let us make a Docker File.(Making Container)</h1>
File Name - "Dockerfile"

# Use an official Python runtime as a parent image
FROM python:3.8-slim

# Set the working directory in the container
WORKDIR /app

# Copy the service account credentials file into the container
ADD . /app

# Install any needed packages specified in requirements.txt
RUN pip install flask google-cloud-storage


# Make port 5000 available to the world outside this container
EXPOSE 5000

# Run app.py when the container launches
CMD ["python", "app.py"]

<h1>Let us Run this now.<br><hr><b>Note: I am using Powershell.</b></h1>
https://github.com/suragsp/Microservice-GCP/assets/104720115/390e580e-284f-46f2-a2cf-43808ebb776c






















 

