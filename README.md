# Real-Time-Object-Detection-and-plotting-a-signal-in-any-detection-using-MATLAB

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
