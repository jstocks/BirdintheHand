A Bird in the Hand
The goal of this device is to identify, analyze, and store historical bird tracking by using:
-	Track environmental conditions (temperature, humidity, light)
-	Track presence of bird(s) via proximity sensor
-	Take photo of bird & store in cloud database
-	Compare bird via computer vision model
-	Store activity in historical database
-	Trigger notifications and update dashboard
-	Use stored data to build statistics 

Project components:

1. Bird Feeder – is a main “hardware” component of the application. It is custom made bird feeder structure which has Raspberry Pi attached along with its ultrasound, light, temperature, and humidity sensors. The bird feeder is installed outside and supplied with sunflower seeds. The feeder is open for a bird to visit at any time. 

2. Node-Red, a flow-based development tool for visual programming, is used to build logic and setup Raspberry Pi components. In order to check the application setup and get access to our Raspberry Pi, use the following link: https://98.237.171.67:1880/#flow 

3. Once a bird lands the bird feeder, then the ultrasounic distance sensor decrease and indicates the presence of a bird.  The birdPresent flag is set to ‘yes’.  

4. Ultrasound sensor triggers a camera to take a picture. Once the bird flies away, the birdPresent flag is reset to 'no' and the process loops.  

5. Next, the picture is automatically sent to Azure Computer Custom Vision cloud service to identify a type of a bird.

6. Azure Custom Vision responds back with the following information:
a.	type of bird
b.	probability of species

7. Light sensor – records a light value automatically (no user interactions are necessary)

8. Temperature sensor – gets automatically the temperature and humidity data 

9. The data received from steps 7 and 8 is automatically stored in InfluxDB database every minute. From step 6, if the probability of the bird species is greater than 50%, the bird species, count increment of one, and the photo are stored in InfluxDB and S3, respectively.  

10. The application uses AWS S3 to save images (as an alternative for InfluxDB) and hosts a static HTML website for photo viewing at  https://tcss573project.s3.us-west-2.amazonaws.com/index.html.

11. The application outputs, values and runs statistics using Grafana, a multi-platform open-source analytics and interactive visualization web application. In order to see our dashboard, go to: http://98.237.171.67:3000.


Database schema:
The schema of the database:
Time: data value
Count: integer value (1 for each bird)
Heat: integer value
Hum: integer value 
Light: integer value
Probability: double value
Species: String with a bird’s type name
Temp: integer value
isBirdPresent: “Yes”/ “No”


AWS Credentials:
AccessKeyId=AKIA2VWMEKFHQQB4EMPL
AWSSecretKey=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


Microsoft Azure Credentials:
URL: https://tcss573project-prediction.cognitiveservices.azure.com/customvision/v3.0/Prediction/24e86e88-bd93-43a4-82f0-cd42911e5c11/detect/iterations/Iteration2/image
Prediction-Key: ee691b3db4da45608f2fbe61b5913faf 