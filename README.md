# hospitalmanagementsystem
My school project, where i make a simple hospital management system wherein a user can sign in as staff patience and authenticate their identity using face detection. This data is used for future appointment fixtures and attendance.

import cv2
import winsound
import numpy as np
import tkinter as tk
from tkinter import ttk
import pandas as pd
import time
from datetime import datetime

df = pd.read_csv('doctor.csv')
att = pd.read_csv('Attendance.csv')
pf = pd.read_csv('patient.csv')
print(df)
print(pf)

#face detection
def Face(name_capture):
    global f
    f=0
    def warndone():
        t = time.localtime()
        cam = cv2.VideoCapture(0)
        detector = cv2.CascadeClassifier(cv2.data.haarcascades + 'haarcascade_frontalface_default.xml')
        a = 0
        Id= name_capture
        def sound():
            winsound.Beep(800,1000)
            a=0
            return a
            
        clicks = 0
        while(True):
            f=1
            ret , img = cam.read()
            gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
            faces = detector.detectMultiScale(gray, 1.3, 5)
            for (x,y,w,h) in faces:
                cv2.rectangle(img,(x,y),(x+w,y+h),(255,0,0),2)
                clicks=clicks+1
                #saving the captured face in the record folder
                cv2.imwrite("record/" + Id + "User."+Id + time.strftime("%d" + '_' +"%m" + '_' +"%Y", t) +'.'+ str(clicks) + ".jpg", gray[y:y+h,x:x+w])
                print('Face found!!')
                a = a+1
                if a>2:
                    sound()
                cv2.imshow('frame',img)
            #wait for 1000 milliseconds    
            if cv2.waitKey(1000) & 0xFF == ord('q'):
                 break
                # break if the clicks is more than 20
            elif clicks>2:
                break
      
    def congrats():
           temp = tk.Toplevel(m)
           temp.minsize(100,120)
           Label2 = tk.Label(temp, text='Congratulations your entry has been uploaded' , height=3)
           Label2.config(font=('Roboto' , 15))
           Label2.grid(row=1,column=0, columnspan = 3)
           button2 = tk.Button(temp, text = "Exit", pady=5,width=15, command = temp.destroy)
           button2.config(font=('Roboto' , 10))
           button2.grid(row=6,column=1, columnspan = 1)
           print("bruh")
           
    FaceIntro = tk.Toplevel(m)
    FaceIntro.minsize(100,300)
    Label1 = tk.Label(FaceIntro, text='Please smile at the camera till you hear a sound' , height=3)
    Label1.config(font=('Roboto' , 15))
    button1 = tk.Button(FaceIntro, text = "Start!", pady=10,width=15, command = warndone)
    button1.config(font=('Roboto' , 10))
    button1.grid(row=6,column=1, columnspan = 1)
    Label1.grid(row=1,column=0, columnspan = 3)
    time.sleep(5)
    if f==1:
       FaceIntro.destroy()
       congrats()
    
administrator = 'Admin'

m = tk.Tk()

#Registration
def createnewwindow():
  global administrator
  def mainsign1(): 
    def dnewentry():
        flag = False
        for i in range(0, len(df)):
            if ((entryadmin.get() == administrator) and (entryusernew.get() == df.Name[i])):
                flag = True

        if flag == True:
            Label5 = tk.Label(create, text='Please use a different username(try your full name)!!' , height=1, bg="#1D5D1F")
            Label5.config(font=('Roboto' , 20))
            Label5.grid(row=10,column=0,columnspan=2)
        else:
            if (entryadmin.get() == administrator):
                nocounter = False
                for i in range(len(df)):
                    if (entryusernew.get() == df.Name[i]):
                        nocounter = True
                if nocounter == False:
                    df.loc[len(df)] = [entryusernew.get(), entrypasswordnew.get(),entryposition.get(),entryspecial.get(),entryEOT.get()]
                    df.to_csv("doctor.csv", index=False) 
                    print(df)
                    Label56 = tk.Label(create, text='Congratulations!! You have made your account!!      ' , height=1, bg="#1D5D1F")
                    Label56.config(font=('Roboto' , 20))
                    Label56.grid(row=10,column=0,columnspan=2)
                    Face(entryusernew.get())
                else:
                    Label55 = tk.Label(create, text='Please use a different username(try your full name)!!  ', height=1, bg="#1D5D1F")
                    Label55.config(font=('Roboto' , 20))
                    Label55.grid(row=10,column=0,columnspan=2) 
            else:
                Label57 = tk.Label(create, text='Administrator, your entered password is incorrect!!  ', height=1, bg="#1D5D1F")
                Label57.config(font=('Roboto' , 20))
                Label57.grid(row=10,column=0,columnspan=2)
        
    create = tk.Toplevel(m)
    create.minsize(1300,700)
    create.config(bg="#1D5D1F")
    Label0 = tk.Label(create, text='Staff Entry', height=1, bg="#1D5D1F", fg='#FFFFFF')
    Label1 = tk.Label(create, text='Let us sign you into the system!!' , height=3, bg="#1D5D1F")
    Label2 = tk.Label(create, text='Administrator Password:', height=1, bg="#1D5D1F")
    Label3 = tk.Label(create, text='Username:' , height=1, bg="#1D5D1F")
    Label4 = tk.Label(create, text='Password:' , height=1, bg="#1D5D1F")
    Label6 = tk.Label(create, text='Position:' , height=1, bg="#1D5D1F")
    Label7 = tk.Label(create, text='Specialization' , height=1, bg="#1D5D1F")
    Label8 = tk.Label(create, text='End of tenure:' , height=1, bg="#1D5D1F")
    LabelSpace1 = tk.Label(create, text='' , height=1, bg="#1D5D1F")
    entryadmin = tk.Entry(create)
    entryusernew = tk.Entry(create)
    entrypasswordnew = tk.Entry(create)
    entryposition = tk.Entry(create)
    entryspecial = tk.Entry(create)
    entryEOT = tk.Entry(create)
    button1 = tk.Button(create, text = "Submit", pady=10,width=15, command =dnewentry, bg="#1D5D1F")
    button2 = tk.Button(create, text = "Back to main menu", pady=10,width=15, command =create.destroy, bg="#1D5D1F")
    Label5 = tk.Label(create, text=' ' , height=1, bg="#1D5D1F")
    Label9 = tk.Label(create, text=' ' , height=1, bg="#1D5D1F")
    Label0.config(font=('Roboto' , 40))
    Label1.config(font=('Roboto' , 20))
    Label2.config(font=('Roboto' , 20))
    Label3.config(font=('Roboto' , 20))
    Label4.config(font=('Roboto' , 20))
    Label6.config(font=('Roboto' , 20))
    Label7.config(font=('Roboto' , 20))
    Label8.config(font=('Roboto' , 20))
    entryadmin.config(font=('Roboto' , 20))
    entryusernew.config(font=('Roboto' , 20))
    entrypasswordnew.config(font=('Roboto' , 20))
    entryposition.config(font=('Roboto' , 20))
    entryspecial.config(font=('Roboto' , 20))
    entryEOT.config(font=('Roboto' , 20))
    button1.config(font=('Roboto' , 20))
    button2.config(font=('Roboto' , 20))
    Label0.grid(row=1,column=0, rowspan=9, padx=175)
    Label1.grid(row=1,column=1, columnspan = 2)
    Label2.grid(row=2,column=1)
    Label3.grid(row=3,column=1)
    Label4.grid(row=4,column=1)
    Label5.grid(row=11,column=1)
    Label9.grid(row=12,column=1)
    Label6.grid(row=5,column=1)
    Label7.grid(row=6,column=1)
    Label8.grid(row=7,column=1)
    LabelSpace1.grid(row=0,column=1,columnspan=3)
    entryadmin.grid(row=2,column=2, columnspan = 2, pady=5)
    entryusernew.grid(row=3,column=2, columnspan = 2, pady=5)
    entrypasswordnew.grid(row=4,column=2, columnspan = 2, pady=5)
    entryposition.grid(row=5,column=2, columnspan = 2, pady=5)
    entryspecial.grid(row=6,column=2, columnspan = 2, pady=5)
    entryEOT.grid(row=7,column=2, columnspan = 2, pady=5)
    button1.grid(row=9,column=2, columnspan = 2, pady=10)
    button2.grid(row=10,column=2, columnspan = 2, pady=10)

  def mainsign2():
    def pnewentry():
        flag = False
        for i in range(0, len(pf)):
            if ((entryadmin.get() == administrator) and (entryusernew.get() == pf.Name[i])):
                flag = True

        if flag == True:
            Label5 = tk.Label(accept, text='Please use a different username(try your full name)!!' , height=1, bg="#EA8B31")
            Label5.config(font=('Roboto' , 20))
            Label5.grid(row=10,column=0,columnspan=2)
            
        else:
            if (entryadmin.get() == administrator):
                pf.loc[len(pf)] = [entryusernew.get(), entrypasswordnew.get(),entryage.get(),entrygen.get(),entryAL.get(),entryBG.get()]
                pf.to_csv("patient.csv", index=False) 
                print(pf)
                Label5 = tk.Label(accept, text='Congratulations!! You have made your account!!  ' , height=1, bg="#EA8B31")
                Label5.config(font=('Roboto' , 20))
                Label5.grid(row=10,column=0,columnspan=2)
                Face(entryusernew.get())
            else:
                Label5 = tk.Label(accept, text='Administrator your entered password is incorrect!! ' , height=1, bg="#EA8B31")
                Label5.config(font=('Roboto' , 20))
                Label5.grid(row=10,column=0,columnspan=2)

    accept = tk.Toplevel(m)
    accept.minsize(1300,700)
    accept.config(bg="#EA8B31")
    Label0 = tk.Label(accept, text='Patient Entry', height=1,fg="#FFFFFF", bg="#EA8B31")
    Label1 = tk.Label(accept, text='Let us sign you into the system!!' , height=3, bg="#EA8B31")
    Label2 = tk.Label(accept, text='Administrator Password:', height=1, bg="#EA8B31")
    Label3 = tk.Label(accept, text='Username:' , height=1, bg="#EA8B31")
    Label4 = tk.Label(accept, text='Password:' , height=1, bg="#EA8B31")
    Label5 = tk.Label(accept, text='Age' , height=1, bg="#EA8B31")
    Label6 = tk.Label(accept, text='Gender' , height=1, bg="#EA8B31")
    Label7 = tk.Label(accept, text='Allergies(if any)' , height=1, bg="#EA8B31")
    Label8 = tk.Label(accept, text='Blood group' , height=1, bg="#EA8B31")
    entryadmin = tk.Entry(accept)
    entryusernew = tk.Entry(accept)
    entrypasswordnew = tk.Entry(accept)
    entryage = tk.Entry(accept)
    entrygen = tk.Entry(accept)
    entryAL = tk.Entry(accept)
    entryBG = tk.Entry(accept)
    button1 = tk.Button(accept, text = "Submit", pady=10,width=15, command =pnewentry, bg="#EA8B31")
    button2 = tk.Button(accept, text = "Back to main menu", pady=10,width=15, command =accept.destroy, bg="#EA8B31")
    Label9 = tk.Label(accept, text='' , height=1, bg="#EA8B31")
    Label10 = tk.Label(accept, text='' , height=1, bg="#EA8B31")
    Label0.config(font=('Roboto' , 40))
    Label1.config(font=('Roboto' , 20))
    Label2.config(font=('Roboto' , 20))
    Label3.config(font=('Roboto' , 20))
    Label4.config(font=('Roboto' , 20))
    Label5.config(font=('Roboto' , 20))
    Label6.config(font=('Roboto' , 20))
    Label7.config(font=('Roboto' , 20))
    Label8.config(font=('Roboto' , 20))
    button1.config(font=('Roboto' , 20))
    button2.config(font=('Roboto' , 20))
    entryadmin.config(font=('Roboto' , 20 ))
    entryusernew.config(font=('Roboto' , 20))
    entrypasswordnew.config(font=('Roboto' , 20))
    entryage.config(font=('Roboto' , 20))
    entrygen.config(font=('Roboto' , 20))
    entryAL.config(font=('Roboto' , 20))
    entryBG.config(font=('Roboto' , 20))
    Label0.grid(row=1,column=0, rowspan=9, padx=175)
    Label1.grid(row=1,column=1, columnspan = 3)
    Label2.grid(row=2,column=1)
    Label3.grid(row=3,column=1)
    Label4.grid(row=4,column=1)
    Label5.grid(row=5,column=1)
    Label6.grid(row=6,column=1)
    Label7.grid(row=7,column=1)
    Label8.grid(row=8,column=1)
    Label9.grid(row=12,column=1)
    Label10.grid(row=13,column=1)
    entryadmin.grid(row=2,column=2, columnspan = 2,pady=5)
    entryusernew.grid(row=3,column=2, columnspan = 2,pady=5)
    entrypasswordnew.grid(row=4,column=2, columnspan = 2,pady=5)
    entryage.grid(row=5,column=2, columnspan = 2,pady=5)
    entrygen.grid(row=6,column=2, columnspan = 2,pady=5)
    entryAL.grid(row=7,column=2, columnspan = 2,pady=5)
    entryBG.grid(row=8,column=2, columnspan = 2,pady=5)
    button1.grid(row=9,column=2, columnspan = 1,pady=10)
    button2.grid(row=10,column=2, columnspan = 1)

  mainentry= tk.Toplevel(m)
  mainentry.minsize(1100,600)
  mainentry.config(bg='#0D294F')
  LabelSpace2 = tk.Label(mainentry, text=' ',bg='#0D294F')
  LabelX = tk.Label(mainentry, text='User type',bg='#0D294F', fg='#FFFFFF')
  buttonD=tk.Button(mainentry, text ='STAFF', pady=20,width=20, command = mainsign1,bg='#0D294F')
  buttonP=tk.Button(mainentry, text ='PATIENT', pady=20,width=20, command = mainsign2,bg='#0D294F')
  buttonDes=tk.Button(mainentry, text ='Back to main menu',width=18, command= mainentry.destroy,bg='#0D294F')
  LabelX.config(font=('Roboto' , 40))
  buttonD.config(font=('Roboto' , 25))
  buttonP.config(font=('Roboto' , 25))
  buttonDes.config(font=('Roboto' , 15))
  LabelSpace2.grid(row=0,column=0, columnspan=3, pady=50)
  LabelX.grid(row=1,column=0, rowspan=3, padx=200)
  buttonD.grid(row=1,column=1,columnspan=1, pady=10)
  buttonP.grid(row=2,column=1,columnspan=1, pady=10)
  buttonDes.grid(row=3,column=1, pady=15)


#Staff sign in
def staffenter():
    def enterstaffdata():
        a=0
        for i in range(0, len(df)):
            if ((entrypassword.get() == df.Password[i]) and (entryuser.get() == df.Name[i])): 
                if (time.strftime("%h"+':'+"%m") < '08:30'):
                    k = 'Yes'
                else:
                    k = 'No'
                now = datetime.now()
                att.loc[len(att)] = [entryuser.get(),time.strftime("%d" + '-' +"%m" + '-' +"%Y"),now.strftime("%H"+':'+"%M")]
                att.to_csv("Attendance.csv", index=False)
                print(att)
                Face(entryuser.get())
                Label5 = tk.Label(denter, text='You have sucessfully signed in!!!         ' , height=1, bg="#1D5D1F")
                Label5.config(font=('Roboto' , 20))
                Label5.grid(row=6,column=0, padx=75)
                a=1
                
                
            else:
                if a==0:
                  Label5 = tk.Label(denter, text='Try again or contact the administrator!!' , height=1, bg="#1D5D1F")
                  Label5.config(font=('Roboto' , 20))
                  Label5.grid(row=6,column=0, padx=75)
                
    denter = tk.Toplevel(m)
    denter.minsize(1300,650)
    denter.config(bg="#1D5D1F")
    LabelSpace = tk.Label(denter, text ="", bg="#1D5D1F")
    Label1 = tk.Label(denter, text='Staff Sign in', bg="#1D5D1F", fg='#FFFFFF')
    Label2 = tk.Label(denter, text='Username:', bg="#1D5D1F")
    Label3 = tk.Label(denter, text='Password:', bg="#1D5D1F")
    entryuser = tk.Entry(denter)
    entrypassword = tk.Entry(denter)
    button1 = tk.Button(denter, text = "Submit", pady=10,width=15, command=enterstaffdata, bg="#1D5D1F")
    button2 = tk.Button(denter, text = "Back to main menu", pady=10,width=20, command =denter.destroy, bg="#1D5D1F")
    Label5 = tk.Label(denter, text='' , height=1, bg="#1D5D1F")
    Label1.config(font=('Roboto' , 35))
    Label2.config(font=('Roboto' , 25))
    Label3.config(font=('Roboto' , 25))
    entryuser.config(font=('Roboto' , 25))
    entrypassword.config(font=('Roboto' , 25))
    button1.config(font=('Roboto' , 15))
    button2.config(font=('Roboto' , 15))
    LabelSpace.grid(row=0, column=0, columnspan=2, pady=75)
    Label1.grid(row=1,column=0, rowspan = 3, padx=200)
    Label2.grid(row=1,column=1, pady=15)
    Label3.grid(row=2,column=1, pady=15)
    Label5.grid(row=6,column=0)
    entryuser.grid(row=1,column=2, pady=15)
    entrypassword.grid(row=2,column=2, pady=15)
    button1.grid(row=4,column=2, columnspan = 2, pady=15)
    button2.grid(row=5,column=2, columnspan = 2, pady=15)


def Admin_Space():
    global doctorlist
    doctorlist= np.array(df)
    global patientlist
    patientlist=np.array(pf)
    print(doctorlist)
    global tessst1
    tessst1 = False
    global tessst2
    tessst2 = False
    def Ad_Screen():
        def SAttend():
            global tessst1
            tessst1 = False
            def staffshow():
              global tessst1
              if tessst1 == True:
                print("Okay")
              else:  
                  for i,(Name,Password,Position,Specialization,Tenure) in enumerate(doctorlist, start=1): #used to add an serial no
                    listBox.insert('',"end", values=(i,Name,Password,Position,Specialization,Tenure))
                  tessst1 = True
            def sel_staffshow():
                
              def display():
                L = []
                for i in range(0, len(att)):
                    if (entryuser.get() == att.Name[i]):
                        L.append(i)
                print(L)
                if len(L) > 0:
                    def Check_list():
                        global uniqueval
                        try:
                            uniqueval = uniqueval*1
                        except:
                            uniqueval = -1
                        try:
                            Label1 = tk.Label(attenddis,text='                                                                                         ' , bg="#D6DCE5",height=1)
                            uniqueval = uniqueval + 1
                            i = L[uniqueval]
                            print(uniqueval)
                            print(att.Date[i])
                            Label4 = tk.Label(attenddis, text='date entered' , bg="#D6DCE5", height=1)
                            Label4.config(font=('Roboto' , 15))
                            Label4.grid(row=3,column=0)
                            Label5 = tk.Label(attenddis, text='Time entered' , bg="#D6DCE5", height=1)
                            Label5.config(font=('Roboto' , 15))
                            Label5.grid(row=3,column=1)
                            Label2 = tk.Label(attenddis, text=att.Date[i] , bg="#D6DCE5", height=10)
                            Label2.config(font=('Roboto' , 15))
                            Label2.grid(row=4,column=0)
                            Label3 = tk.Label(attenddis, text=att.Late[i] , bg="#D6DCE5", height=10)
                            Label3.config(font=('Roboto' , 15))
                            Label3.grid(row=4,column=1)
                            button3 = tk.Button(attenddis, text = "Next Entry", pady=10,width=15, bg="#D6DCE5",command=Check_list)
                            button3.config(font=('Roboto' , 13))
                            button3.grid(row=5,column=0)
                            Label1.grid(row=2,column=0)
                            
                        except:
                            Label2 = tk.Label(attenddis, text='No more user data left' , height=1, bg="#D6DCE5")
                            Label3 = tk.Label(attenddis, text='                 ' , height=1, bg="#D6DCE5")
                            button3 = tk.Button(attenddis, text = "Quit", pady=10,width=15, bg="#D6DCE5",command=attenddis.destroy)
                            button3.config(font=('Roboto' , 13))
                            Label2.config(font=('Roboto' , 15))
                            Label2.grid(row=4,column=0)
                            Label3.grid(row=4,column=1)
                            button3.grid(row=5,column=0)
                            #return 2
                    Check_list()
                else:
                    Label2 = tk.Label(attenddis, text='User data unavailable' , bg="#D6DCE5", height=1)
                    Label2.config(font=('Roboto' , 15))
                    Label2.grid(row=2,column=0)
              attenddis = tk.Toplevel(m)
              attenddis.config(bg="#D6DCE5")
              attenddis.minsize(900,650)
              Label1 = tk.Label(attenddis, text='Name:' , height=3, bg="#D6DCE5")
              entryuser = tk.Entry(attenddis)
              button1 = tk.Button(attenddis, text = "Submit", pady=10,width=15, bg="#D6DCE5", command=display)
              button1.config(font=('Roboto' , 13))
              button2 = tk.Button(attenddis, text = "Back to Admin menu",padx=8, pady=10,width=15, bg="#D6DCE5", command =attenddis.destroy)
              button2.config(font=('Roboto' , 13))
              button3 = tk.Button(attenddis, text = "",bg="#D6DCE5", pady=10,width=15)
              entryuser.config(font=('Roboto' , 15))
              Label1.config(font=('Roboto' , 15))
              Label2 = tk.Label(attenddis, text='' , height=1, bg="#D6DCE5")
              Label2.config(font=('Roboto' , 15))
              Label2.grid(row=2,column=0)
              Label3 = tk.Label(attenddis, text='' , height=1, bg="#D6DCE5")
              Label3.config(font=('Roboto' , 15))
              Label3.grid(row=3,column=0)
              Label4 = tk.Label(attenddis, text='' , height=1, bg="#D6DCE5")
              Label4.config(font=('Roboto' , 15))
              Label4.grid(row=3,column=1)
              Label1.grid(row=1,column=0)
              entryuser.grid(row=1,column=1)
              button1.grid(row=1,column=2,padx=10)
              button2.grid(row=1,column=3)
              global uniqueval
              uniqueval = 1
              del uniqueval
                     

            docrec = tk.Tk()
            docrec.config(bg="#D6DCE5")
            style = ttk.Style(docrec)
            style.theme_use("alt")
            label = tk.Label(docrec, text="Doctors record", font=("Roboto",32), bg="#D6DCE5")
            label.grid(row=0, columnspan=5)
            # create Treeview with 6 columns
            cols = ('Sno','Name','Password','Position','Specialization','Tenure')
            listBox = ttk.Treeview(docrec,columns=cols, show='headings')
            # set column headings
            for col in cols:
              listBox.heading(col,text=col)    
            listBox.grid(row=1, column=0, columnspan=3)
            showall_rec= tk.Button(docrec, text="Show all records", width=12, bg="#D6DCE5", command=staffshow)
            showall_rec.grid(row=4, column=0)
            showsingle_rec= tk.Button(docrec, text="Show staff attendance", width=18, bg="#D6DCE5", command=sel_staffshow)
            showsingle_rec.grid(row=4, column=1)
            close_rec= tk.Button(docrec, text="Back to menu", width=12, bg="#D6DCE5", command=docrec.destroy)
            close_rec.grid(row=4, column=2)
            docrec.mainloop()
            

        def PAttend():
            global tessst2
            tessst2 = False
            def patshow():
                global tessst2
                if tessst2 == True:
                   print("Okay")
                else:
                  for i,(Name,Password,Age,Gender,Allergies,Blood_Group) in enumerate(patientlist, start=1): #used to add an serial no
                    listBox.insert('',"end", values=(i,Name,Password,Age,Gender,Allergies,Blood_Group))
                  tessst2 = True
            patrec = tk.Tk()
            patrec.config(bg="#D6DCE5")
            style = ttk.Style(patrec)
            style.theme_use("alt")
            patrec.config(bg="#D6DCE5")
            label = tk.Label(patrec, text="Patients record", font=("Roboto",32),bg="#D6DCE5")
            label.grid(row=0, columnspan=5)
            # create Treeview with 6 columns
            cols = ('Sno','Name','Password','Position','Specialization','Tenure','sike')
            listBox = ttk.Treeview(patrec,columns=cols, show='headings')
            # set column headings
            for col in cols:
              listBox.heading(col,text=col)    
            listBox.grid(row=1, column=0, columnspan=3)
            showall_rec= tk.Button(patrec, text="Show all records", width=12, command=patshow,bg="#D6DCE5")    
            showall_rec.grid(row=4, column=0)
            close_rec= tk.Button(patrec, text="Back to menu", width=12, command=patrec.destroy,bg="#D6DCE5")
            close_rec.grid(row=4, column=2)
            patrec.mainloop()
            
        admin.destroy()
        adsc = tk.Toplevel(m)
        adsc.config(bg="#D6DCE5")
        adsc.minsize(1200,600)
        LabelSpace1 = tk.Label(adsc, text=' ', bg="#D6DCE5")
        Label1 = tk.Label(adsc, text='Choose what you want to view' , height=3, bg="#D6DCE5")
        button1 = tk.Button(adsc, text = "Staff Data", pady=20,width=25, command =SAttend, bg="#D6DCE5")
        button2 = tk.Button(adsc, text = "Patient Data", pady=20,width=25, command =PAttend, bg="#D6DCE5")
        button3 = tk.Button(adsc, text = "Logout", pady=10,width=15, command =adsc.destroy, bg="#D6DCE5")
        Label1.config(font=('Roboto' , 25))
        button1.config(font=('Roboto' , 25))
        button2.config(font=('Roboto' , 25))
        button3.config(font=('Roboto' , 15))
        LabelSpace1.grid(row=0,column=0, columnspan=3, pady=50)
        Label1.grid(row=1,column=0, rowspan = 3, padx=100)
        button1.grid(row=2,column=1, columnspan = 1, pady=20)
        button2.grid(row=3,column=1, columnspan = 1, pady=20)
        button3.grid(row=4,column=1, columnspan = 1)

    def Ad_check():
          if entryadmin.get()==administrator:
            Ad_Screen()
          elif (entryadmin.get()!=administrator):
            button1 = tk.Button(admin, text = "Submit",padx=10,pady=7,command=Ad_check, bg="#D6DCE5")
            admin.minsize(800,500)
            button1.config(font=('Roboto' , 15))
            button1.grid(row=3,column=1, columnspan = 1, padx=20)
            LabelSpace = tk.Label(admin, text ="", bg="#D6DCE5")
            Label3=tk.Label(admin, text="            Administrator's password is incorrect",height=1, bg="#D6DCE5")
            Label3.config(font=('Roboto' , 15))
            Label3.grid(row=5,column=0,padx=50)
            LabelSpace.grid(row=4, column=0,padx=20,pady=20)
        
    admin = tk.Toplevel(m)
    admin.config(bg="#D6DCE5")
    admin.minsize(800,500)
    Label1 = tk.Label(admin, text="Administrator's sign in" , height=3, bg="#D6DCE5")
    Label2 = tk.Label(admin, text='Administrator Password:', height=1, bg="#D6DCE5")
    LabelSpace = tk.Label(admin, text ="", bg="#D6DCE5")
    entryadmin = tk.Entry(admin)
    button1=tk.Button(admin,text="Submit",padx=10,pady=7,command=Ad_check, bg="#D6DCE5")
    button2 = tk.Button(admin, text = "Back to main menu",padx=10,pady=7,command =admin.destroy, bg="#D6DCE5")
    Label1.config(font=('Roboto' , 30))
    Label2.config(font=('Roboto' , 18))
    button1.config(font=('Roboto' , 15))
    button2.config(font=('Roboto' , 15))
    entryadmin.config(font=('Roboto' , 18))
    entryadmin.grid(row=2,column=1, columnspan = 3, pady=10)
    Label1.grid(row=1,column=0, columnspan = 3)
    Label2.grid(row=2,column=0)
    LabelSpace.grid(row=4, column=0, columnspan=2,padx=20,pady=20)
    button1.grid(row=3,column=1, columnspan = 1, padx=20)
    button2.grid(row=3,column=2, columnspan = 1)

#patient sign in and appointment fixture
def Patiententry():
    global tessst3
    tessst3 = False
    def enterpatientdata():
        def spAttendanceCheck():
            doctorlist= np.array(df.Name)
            print(doctorlist)
            avail=[]
            global tessst3
            tessst3 = False
            global rec
            rec = 0
            count=0
            for j in range(0,len(df)):
               if(entrysptype.get() == df.Specialization[j]):
                  avail.append(doctorlist[j])
                  print("nnnnnnnnnndjdd")
                  rec=2
                  for i in range(0,len(pf)):
                     if ((entrypassword.get() == pf.Password[i]) and (entryuser.get() == pf.Name[i])):
                        rec=1
                        count=count+1
                        if count==1:
                          def show():
                              global tessst3
                              if tessst3 == True:
                                 print("Okay")
                              else:
                                  for i,(Name) in enumerate(avail, start=1): #used to add an serial no
                                      listBox.insert('',"end", values=(i,Name,entrysptype.get()))
                                  tessst3=True
                          def final():
                              z=0
                              for i in range(0, len(att)):
                                 if (entryfinal.get() == att.Name[i]):
                                     Face(entryfinal.get())
                                     Label12=tk.Label(patrec, text="Your appointment has been fixed!!         ",height=1, bg="#EA8B31")
                                     Label12.config(font=('Roboto' , 15))
                                     Label12.grid(row=8,column=0)
                                     z=2
                                     break
                              
                                 else:
                                     Label13=tk.Label(patrec, text="        Please enter a valid entry!!         ",height=1, bg="#EA8B31")
                                     Label13.config(font=('Roboto' , 15))
                                     Label13.grid(row=8,column=0)
                                 print(avail)
                          patrec = tk.Tk()
                          patrec.minsize(600,450)
                          style = ttk.Style(patrec)
                          style.theme_use("clam")
                          patrec.config(bg="#EA8B31")
                          Label9 = tk.Label(patrec, text="Here is a list of specialists, select whom you want to consult:", font=("Roboto",15), bg="#EA8B31")
                          Label9.config(font=('Roboto' ,18))
                          Label9.grid(row=0,columnspan=7)
                          # create Treeview with 3 columns
                          cols = ('Sno','Name','Specialist')
                          listBox = ttk.Treeview(patrec,columns=cols, show='headings')
                          # set column headings
                          for col in cols:
                              listBox.heading(col,text=col)    
                          listBox.grid(row=1, column=0, columnspan=3,padx=15)
                          show_rec = tk.Button(patrec, text="Show records", width=12, command=show, bg="#EA8B31")
                          show_rec.grid(row=4, column=0)
                          close_rec = tk.Button(patrec, text="Back to menu", width=12, command=patrec.destroy, bg="#EA8B31")
                          close_rec.grid(row=4, column=1)
                          entryfinal = tk.Entry(patrec)
                          entryfinal.config(font=('Roboto' , 18))
                          entryfinal.grid(row=6,column=1, columnspan =5)
                          Label10 = tk.Label(patrec, text="Enter whom you want to consult:", height=2, bg="#EA8B31")
                          Label10.config(font=('Roboto' ,18))
                          Label10.grid(row=6,columnspan=1)
                          button0= tk.Button(patrec, text="Submit", width=12, command=final, bg="#EA8B31")
                          button0.grid(row=7, column=2)
                          break                
               else:
                   if rec==2:
                       Label5.grid_forget()
                       Label5 = tk.Label(penter, text='     Username or password is incorrect          ' , height=1, bg="#EA8B31")
                       Label5.config(font=('Roboto' ,20))
                       Label5.grid(row=5,column=0)
                       break
                   else:
                     if rec!=1:
                       Label5 = tk.Label(penter, text='The Desired specialist is unavailible              '  , height=1, bg="#EA8B31")
                       Label5.config(font=('Roboto' ,20))
                       Label5.grid(row=5,column=0)
                       print("hello")
                     else:
                        Label5 = tk.Label(penter, text='                                                                   ' , height=1, bg="#EA8B31")
                        Label5.config(font=('Roboto' ,20))
                        Label5.grid(row=5,column=0)   
                        print("hi")
                        rec=1
        spAttendanceCheck()           
    penter = tk.Toplevel(m)
    penter.minsize(1300,650)
    penter.config(bg="#EA8B31")
    LabelSpace = tk.Label(penter, text ="", bg="#EA8B31")
    Label1 = tk.Label(penter, text='Patient Sign in' , height=3,fg="#FFFFFF", bg="#EA8B31")
    Label2 = tk.Label(penter, text='Username:' , height=2, bg="#EA8B31")
    Label3 = tk.Label(penter, text='Password:' , height=2, bg="#EA8B31")
    Label4 = tk.Label(penter, text='Specialist Type:' , height=2, bg="#EA8B31")
    Label5 = tk.Label(penter, text='                                ' , height=1, bg="#EA8B31")
    entryuser = tk.Entry(penter)
    entrypassword = tk.Entry(penter)
    entrysptype = tk.Entry(penter)
    button1 = tk.Button(penter, text = "Submit", pady=10,width=15, command= enterpatientdata, bg="#EA8B31")
    button2 = tk.Button(penter, text = "Back to main menu", pady=10,width=20, command =penter.destroy, bg="#EA8B31")
    Label1.config(font=('Roboto' , 35))
    Label2.config(font=('Roboto' , 25))
    Label3.config(font=('Roboto' , 25))
    Label4.config(font=('Roboto' , 25))
    entryuser.config(font=('Roboto' , 25))
    entrypassword.config(font=('Roboto' , 25))
    entrysptype.config(font=('Roboto' , 25))
    button1.config(font=('Roboto' , 15))
    button2.config(font=('Roboto' , 15))
    LabelSpace.grid(row=0, column=0, columnspan=2, pady=60)
    Label1.grid(row=1,column=0, rowspan = 3, padx=175)
    Label2.grid(row=1,column=1, pady=10)
    Label3.grid(row=2,column=1, pady=10)
    Label4.grid(row=3,column=1)
    Label5.grid(row=5,column=1)
    entryuser.grid(row=1,column=2, columnspan = 1, pady=5)
    entrypassword.grid(row=2,column=2, columnspan = 1, pady=5)
    entrysptype.grid(row=3,column=2, columnspan = 1, pady=5)
    button1.grid(row=5,column=2, columnspan = 1, pady=5)
    button2.grid(row=6,column=2, columnspan = 1, pady=5)

#main window
def main():
    
    m.minsize(1300,650)
    m.configure(bg='#2A313C')
    Label1 = tk.Label(m, text='Welcome to the XYZ clinic!!' , height=3, bg="#2A313C", fg="#FFFFFF")
    button1 = tk.Button(m, text = "Staff", width=20, height= 2, command = staffenter, bg='#2A313C')
    button2 = tk.Button(m, text = "Patient", width=20, height= 2, command = Patiententry, bg="#2A313C")
    button3 = tk.Button(m, text = "Administrator", width=20, height= 2 ,command = Admin_Space, bg="#2A313C")
    button4 = tk.Button(m, text = "Create an account", width=20, height= 2, command = createnewwindow, bg="#2A313C")
    Label1.config(font=('Roboto' , 40,))
    button1.config(font=('Roboto' , 25))
    button2.config(font=('Roboto' , 25))
    button3.config(font=('Roboto' , 25))
    button4.config(font=('Roboto' , 25))
    Label1.grid(row=1,column=0, rowspan = 5, padx=100)
    button1.grid(row=2,column=1,padx=100,pady=25)
    button2.grid(row=3,column=1,padx=100,pady=25)
    button3.grid(row=4,column=1,padx=100,pady=25)
    button4.grid(row=5, column=1,padx=100,pady=25)
    m.mainloop()

main()

