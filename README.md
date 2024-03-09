# AWS Image Processing Pipeline Using Cloud Watch

### Summary:
This repository contains 3 lambda files that make up an image processing pipeline built in AWS. The pipeline allows a user to upload an image to an S3 bucket via a web application interface and retrieve that image to be displayed on the web application. Although this is a simple task, it is the basic structure for completely automated image upload, storage, and retrieval. The image object is stored in an S3 database and the S3ObjectKey is stored in a DynamoDB database for quick access querying from the web application.


### AWS Pipeline Integration with CloudWatch
In my GitHub repository, I've leveraged AWS CloudWatch to enhance the efficiency and reliability of the image processing pipeline. CloudWatch serves as a vital monitoring and management tool throughout this process, providing real-time insights into the performance and health of my AWS resources.

Through the integration of CloudWatch, I'm able to monitor the execution of Lambda functions responsible for controlling the entire pipeline. This includes tracking metrics such as execution duration, error rates, and resource utilization, allowing me to detect and troubleshoot any issues promptly. Additionally, CloudWatch alarms are configured to automatically alert me to any anomalies or unexpected behavior, ensuring proactive monitoring and timely response to potential issues.

Moreover, CloudWatch logs capture detailed information about each step of the pipeline, facilitating thorough analysis and troubleshooting when needed. By centralizing logs and metrics within CloudWatch, I can gain a comprehensive understanding of the pipeline's performance and effectively optimize its operation over time.

Overall, CloudWatch plays a critical role in ensuring the reliability and efficiency of my image upload pipeline. Watch the demonstration video below to see how the pipeline works.

### Pipeline Description:
The pipeline starts on a static website (I used my personal website as a demo). When a user uploads an image via the "upload" interface. An http request is sent to the first lambda function `s3bucket-presignkey`. The lambda function requests a presigned upload key from the S3 bucket using an authorized IAM role. The S3 bucket returns a presigned key and the lambda function passes the key back to the static website in the http response. A javascript event handler is then able to use the presigned key to upload the image directly to the S3 bucket. This method was chosen because it utilizes AWS user authentication roles and applys the principle of least privaledge to each entity in the pipeline. 

When the image is uploaded, a second lambda function `extractPhotoMetadata` is triggered by the action of a new upload to the S3 bucket. This trigger is automatic as the ambda function is designed to watch for new uploads into this database to begin the metadata extraction automatically. This lambda function pulls the file path of the image and extracts from the simulated S3 bucket directories, the "bar name" and "Drink Name" as well as the S3ObjectKey that can be used for direct retrieval. The lambda function then stores this metadata in a dynamoDB where the "Bar Name" and "Drink Name" are the partition and sort keys respectively. The S3ObjectKey is an attribute associated with the partition and sort key unique identifier. This completes the image upload pipeline.

Image retrieval pipeline also starts in the static website. From the website, the Drink Name and Bar Name are entered by the user and passed as a json package to a third lambda function which executes a query of the dynamoDB database. The query returns the S3ObjectKey associated with the Bar and Drink name and the lambda function returns the ObjectKey to the static website via the http response. From within the website, the S3ObjectKey can now be used directly to show the image on the website. To see this pipeline, find the video below.

### Demonstration:

https://youtu.be/H9Uebmp_hKY
