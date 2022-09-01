# Real Time Object Detection using MATLAB

 
## Introduction:

Real-time object detection is the task of doing object detection in real time with fast inference while maintaining a base level of accuracy. Let us see some of the examples in Object detection in real life. An item/object detection framework is additionally utilized in tracking the objects, for instance tracking a ball during a match in the football world cup, tracking the swing of a cricket bat, tracking an individual in a video.
The main purpose of object detection is to identify and locate one or more effective targets from still image or video data. It comprehensively includes a variety of important techniques, such as image processing, pattern recognition, artificial intelligence and machine learning
Today Object detection plays a huge role in understanding the scenes associated with military, security, medical, and transportation. Driverless or self-driving cars use object detection to identify traffic signs, pedestrians, other vehicles, etc. For example, Hyundai IONIQ 5 is the latest robotaxi unveiled by Motional.

## Project Description

Our project is an application developed using MATLAB, in this project we tried to integrate image processing techniques and technologies such as: Preprocessing, Segmentation, extraction and real-time object detection, The project consists in taking real-time photos of each object passes in front of the camera, the first photo of each object will be qualified as reference and the calculation of their intensity , then with each movement of this object the application calculates the intensity of the image it will find that it is larger than the reference image which implies the presence of a movement, these movements are presented in a two-dimensional graphed  , then each SIGN in the graph represents a movement of this object.


## Code Of project 
```
%%  Activating the Camera
cam = imaqfind; % Find the object in memory (if it exists!)
if(~exist('cam'))
    cam=videoinput('winvideo',1);           % Reporting(Déclaration) 
    set(cam,'ReturnedColorSpace','GRAY');   %GRAY 
    triggerconfig(cam, 'manual');           %allows you to configure the triggering properties of the video input object this function increases the speed of image acquisition 
    start(cam);                             %Camera trip (Déclanchement de camera) 
else
    delete(cam);                            %delete object if it exesite and in reopen the camera
    cam=videoinput('winvideo');
    set(cam,'ReturnedColorSpace','GRAY');
    triggerconfig(cam, 'manual');
    start(cam);
end;
 
%% Program Settings
M=256; N=256;             %resize image size  
Nfil=4;                    %Buffer Size: local storage of 4 images    
im_A=zeros(Nfil,M,N);
i=1;j=1;
Iter=1;                   %count iteration number  
 
K=64;                      %64 table to store the presence or absence of the object
I_RMSE=zeros(1,K);
Det=zeros(1,K);
Seuil=0.05;                 %detection threshold detects detector sensitivity  
 
 
%% Detection & Display
while 1
    %% Reading the current image
    im=imadjust(im2double(getsnapshot(cam))); % recover an image from the camera and adjust the contrast
    im_in=imresize(im,[M N]);                 %resize the image
    
    %% Intensity Calculation / Detection
    
    % Intensity/Detection
    Moy=squeeze(mean(im_A)) ; %squeeeze remove the unitiles and unique attributes of the image after the image average calculation 
    I_diff=Moy(:)-im_in(:);   %the difference between the image taken and the actual image 
    RMSE=sqrt(mean(I_diff.^2));     %RMSE f & g erreur quadratique 
    I_RMSE(i)=RMSE;                 % Intensity
    Det(i)=0.25*(I_RMSE(i)>Seuil);  % detection signal
    
    % Index Update (Detector)
    i=i+1;
    if i==K+1
        i=1;
        I_RMSE=0*I_RMSE;
        Det=0*Det;
    end;
    
    %% buffer update
    im_A(j,:,:)=im_in;
    j=j+1;
    if j==Nfil+1
        j=1;
    end;
    
    Iter=Iter+1; % Frame number
    
     %% display
     if Iter<2*Nfil
         Det=Det*0;
     else
         figure(1);
         subplot(121); imagesc([im_in]);colormap(gray);colorbar;
         subplot(122);
         plot(I_RMSE,'-*','linewidth',3);xlim([1 K]); grid on; hold on;
         stem(Det,'r-*','linewidth',2); hold off;
         if RMSE>Seuil
             title('Objet Détecté', 'fontsize',16);
         else
             title('Objet Non Détecté', 'fontsize',16);
        end;
     end;
 
    %%
    % Taking photos of the detected object
      if RMSE>Seuil
          for m=1:2
              s=strcat('im_',num2str(Iter),'_',num2str(m),'.png');
              im0=getsnapshot(cam);
              imwrite(im0,s);
          end;
      end;
end
```

##  Results:
The first photo shows no objects detected

![image](https://user-images.githubusercontent.com/85867562/187918511-743be269-0119-4f0b-89ea-a110e055f2e7.png)

The second one is an example of demonstrating of the application : when opening the camera it takes the first photos of all objects as reference as you see the red line is null once object moves application detects the presence of the moving and that represented by the blue line as an impulsion   

![image](https://user-images.githubusercontent.com/85867562/187918621-a6099f66-0702-4ff1-9bd8-37c8d24150a0.png)

For more viewing I invite to show the demonstrate of the application indown video:

https://user-images.githubusercontent.com/85867562/183650268-58e6ef74-64be-427a-a954-5ef339e0ba96.mp4

## Applications

Our project we can applied in many services in our daily life in many sectors:
* We can use the camera with the application to detect criminals in homes in any movements.
* Use it in street to take the movements of population.  
* Use to take pictures in any movement inside the body of patients because it will help to detect diseases.
* In military to flow the trace of enemies.
* Count the number of times the car has passed this way.

## 6.	Future Enhancements

We hope in the future to enhance our application by adding computer vision, deep learning and prediction to make it more robust.

## Conclusion

A smart city is a technologically modern urban area that uses different types of electronic methods and sensors to collect specific data. Information gained from that data is used to manage assets, resources and services efficiently; in return, that data is used to improve operations across the city. computer vision and image Preprocessing are one of technologies aim to build this city.
Computer Vision is a critical enabling technology for modern armies, as it aids security systems in detecting enemy forces or traitors and improves the targeting capabilities of directed missile systems.
Develop a robot can detect movements or counts numbers of cars has passd this way or more applications it helps society and increase economy, environment protection and more impacts. 

