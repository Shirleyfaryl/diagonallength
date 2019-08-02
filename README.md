# diagonallength
import cv2
import cv2.aruco as aruco
import numpy as np

cap=cv2.VideoCapture(0)
while(True):
    ret,frame=cap.read()
    img=cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
    arucodict=aruco.Dictionary_get(aruco.DICT_6X6_250)
    parameters=aruco.DetectorParameters_create()
    corners,ids,rejectedImgPoints=aruco.detectMarkers(img,arucodict,parameters=parameters)
    imgcopy=frame.copy()
    if len(corners)==4:
        for i,x in enumerate(ids):
            if x[0]==30:
                co1x,co1y=int(corners[i][0][0][0]+ corners[i][0][2][0])//2,int(corners[i][0][0][1]+ corners[i][0][2][1])//2
            if x[0]==12:
                co2x,co2y=int(corners[i][0][0][0]+ corners[i][0][2][0])//2,int(corners[i][0][0][1]+ corners[i][0][2][1])//2
            if x[0]==26:
                co3x,co3y=int(corners[i][0][0][0]+ corners[i][0][2][0])//2,int(corners[i][0][0][1]+ corners[i][0][2][1])//2
            if x[0]==5:
                co4x,co4y=int(corners[i][0][0][0]+ corners[i][0][2][0])//2,int(corners[i][0][0][1]+ corners[i][0][2][1])//2
        cv2.line(imgcopy,(co1x,co1y),(co3x,co3y),(255,255,0),5)
        cv2.line(imgcopy,(co2x,co2y),(co4x,co4y),(255,255,0),5)
        dist1=abs((abs(co3x-co1x))**2-(abs(co3y-co1y))**2)**0.5
        dist4=abs((abs(co2x-co4x))**2-(abs(co2y-co4y))**2)**0.5
        font = cv2.FONT_HERSHEY_SIMPLEX
        cv2.putText(imgcopy,"{}".format(int(dist1)),((co3x+co1x)//4,(co3y+co1y)//4),font,0.8,(255, 255, 0), 2, cv2.LINE_AA)
        cv2.putText(imgcopy,"{}".format(int(dist4)),((co4x+co2x)//2,(co4y+co2y)//2),font,0.8,(255, 255, 0), 2, cv2.LINE_AA)
        if dist1<=dist4-3 or dist1>=dist4+3:
            cv2.putText(imgcopy,"hence proved",(350,450),font,0.8,(0, 255, 255), 2, cv2.LINE_AA)

    cv2.imshow("image",imgcopy)
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
cap.release()
cv2.destroyAllWindows()

