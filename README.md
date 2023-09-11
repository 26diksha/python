import tkinter 
from tkinter import *
import pymysql
from tkinter import ttk
from tkinter import messagebox
import datetime
from time import strftime
from PIL import ImageTk,Image
import matplotlib.pyplot as plt
from matplotlib.backends.backend_tkagg import FigureCanvasTkAgg
import numpy as np

t=tkinter.Tk()

t.geometry('2000x800')

c4=Canvas(t,width=2000,height=100,bg='blue')
c4.place(x=0,y=0)
c5=Canvas(t,width=2000,height=700,bg='orange')
c5.place(x=0,y=100)
def adminloginscreen():
    def adminlogin():
        adminid = adminidentry.get() # define the variable name
        password = passwordentry.get() # define the variable password
        if adminid == '' or password == '':
            messagebox.showerror('error', 'All fields are required')
        else:
            try:
                db = pymysql.connect(host='localhost', user='root', password='root',database='polling')
                cur=db.cursor() # create a cursor object
            except:
                messagebox.showerror('Error', 'Connection is not established')
                return
            sql = 'use polling'
            cur.execute(sql) # use the cursor object to execute the SQL query
            sql = 'select * from admin where adminid=%s and password=%s'
            values = (adminid, password)
            cur.execute(sql, values)
            result = cur.fetchone()
            if result == None:
                messagebox.showerror('error', 'Invalid admin id & Password')
            else:
                messagebox.showinfo('WELCOME', 'login successful')
                
                c7=Canvas(t,width=2000,height=800,bg='skyblue')
                c7.place(x=0,y=0)
                
                def admin():
                    def showinsert():
                        def savedata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            a=adminidentry.get()
                            b=nameentry.get()
                            c=passwordentry.get()
                            if len(a)==0 or len(b)==0 or len(c)==0:
                                messagebox.showerror('Error','Blanks are not allowed')
                            elif len(c)<8:
                                messagebox.showerror('Error','Password must contain atleast 8 characters ')
                            
                            else:
                                sql="insert into admin values('%s','%s','%s')"%(a,b,c)
                                cur.execute(sql)
                                db.commit()
                                data=cur.fetchall()
                                messagebox.showinfo('save','Data Saved')
                            db.close()
                            
                        def checkdata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            a=adminidentry.get()
                            if len(a)==0:
                                messagebox.showerror('Error','Plz Enter Adminid')
                            else:
                                sql="select count(*) from admin where adminid='%s'"%(a)
                                cur.execute(sql)
                                data=cur.fetchall()
                                x=0
                                for res in data:
                                    print(res)
                                    x=res[0]
                                if x==0:
                                    lblstatus.config(text='GO AHEAD',fg='green')
                                else:
                                    lblstatus.config(text='Already Available',fg='red')
                        def closescreen():
                            c3.destroy()
                        def cleardata():
                            adminidentry.delete(0,100)
                            nameentry.delete(0,100)
                            passwordentry.delete(0,100)
                        
                        c3=Canvas(c7,width=1800,height=700,bg='orange')
                        c3.place(x=205,y=105)                 
                        lbladmin=Label(c3,text='ADMIN(INSERT PAGE)',font='bold 20 underline',bg='black',fg='yellow')
                        lbladmin.place(x=270,y=20)
                        lbladminid=Label(c3,text='Adminid',font='arial 15',bg='tan',fg='black')
                        lbladminid.place(x=150,y=100)
                        adminidentry=Entry(c3,width='40')
                        adminidentry.place(x=300,y=100,height=25)
                        lblname=Label(c3,text='Name',font='arial 15',bg='tan',fg='black')
                        lblname.place(x=150,y=170)
                        nameentry=Entry(c3,width=40)
                        nameentry.place(x=300,y=170,height=25)
                        lblpassword=Label(c3,text='Password',font='arial 15',bg='tan',fg='black')
                        lblpassword.place(x=150,y=240)
                        def show():
                            hide_button=Button(c3,image=hide_image,command=hide,relief=FLAT,activebackground='white',background='white')
                            hide_button.place(x=550,y=240)
                            passwordentry.config(show='')
                        def hide():
                            show_button=Button(c3,image=show_image,command=show,relief=FLAT,activebackground='white',background='white')
                            show_button.place(x=550,y=240)
                            passwordentry.config(show='*')
                        show_image=ImageTk.PhotoImage(file=r"C:\Users\Nabeel-IT\Pictures\show.png")
                        hide_image=ImageTk.PhotoImage(file=r"C:\Users\Nabeel-IT\Pictures\hide.png")
                        show_button=Button(c3,image=show_image,command=show,relief=FLAT,activebackground='white',bd=0,background='white')
                        show_button.place(x=550,y=240)
                        passwordentry=Entry(c3,width=40,show='*')
                        passwordentry.place(x=300,y=240,height=25)
                        btsave=Button(c3,text='Save',font='arial 15',bg='teal',fg='white',command=savedata)
                        btsave.place(x=300,y=340)
                        btclear=Button(c3,text='Clear',font='arial 15',bg='teal',fg='white',command=cleardata)
                        btclear.place(x=450,y=340)
                        btcheck=Button(c3,text='Check',font='arial 15',bg='teal',fg='white',command=checkdata)
                        btcheck.place(x=570,y=100)
                        lblstatus=Label(c3,text='Status',font='arial 20')
                        lblstatus.place(x=700,y=100)
                        btback=Button(c3,text='Back',font='arial 15',bg='teal',fg='white',command=closescreen)
                        btback.place(x=600,y=340)
                    def showfind():
                        elist=[]
                        def filldata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            sql="select adminid from admin"
                            cur.execute(sql)
                            data=cur.fetchall()
                            for res in data:
                                elist.append(res[0])
                            db.close()
                        def finddata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            a=adminidentry.get()
                            sql="select name,password from admin where adminid='%s'"%(a)
                            cur.execute(sql)
                            data=cur.fetchone()
                            if data==None:
                                messagebox.showinfo('status','Not found')
                            else:
                                nameentry.insert(0,data[0])
                                passwordentry.insert(0,data[1])
                        def closescreen():
                            c3.destroy()
                        def cleardata():
                            adminidentry.delete(0,100)
                            nameentry.delete(0,100)
                            passwordentry.delete(0,100)
                            
                        c3=Canvas(c7,width=1800,height=700,bg='orange')
                        c3.place(x=205,y=105)               
                        lbladmin=Label(c3,text='ADMIN(FIND PAGE)',font='bold 20 underline',bg='black',fg='yellow')
                        lbladmin.place(x=270,y=20)
                        lbladminid=Label(c3,text='Adminid',font='arial 15',bg='tan',fg='black')
                        lbladminid.place(x=150,y=100)
                        adminidentry=ttk.Combobox(c3,width='40')
                        filldata()
                        adminidentry['values']=elist
                        adminidentry.place(x=300,y=100,height=25)
                        btfind=Button(c3,text='Find',font='arial 15',bg='teal',fg='white',command=finddata)
                        btfind.place(x=570,y=100)
                        lblname=Label(c3,text='Name',font='arial 15',bg='tan',fg='black')
                        lblname.place(x=150,y=200)
                        nameentry=Entry(c3,width=40)
                        nameentry.place(x=300,y=200,height=25)
                        lblpassword=Label(c3,text='Password',font='arial 15',bg='tan',fg='black')
                        lblpassword.place(x=150,y=300)
                        def show():
                            hide_button=Button(c3,image=hide_image,command=hide,relief=FLAT,activebackground='white',background='white')
                            hide_button.place(x=550,y=300)
                            passwordentry.config(show='')
                        def hide():
                            show_button=Button(c3,image=show_image,command=show,relief=FLAT,activebackground='white',background='white')
                            show_button.place(x=550,y=300)
                            passwordentry.config(show='*')
                        show_image=ImageTk.PhotoImage(file=r"C:\Users\Nabeel-IT\Pictures\show.png")
                        hide_image=ImageTk.PhotoImage(file=r"C:\Users\Nabeel-IT\Pictures\hide.png")
                        show_button=Button(c3,image=show_image,command=show,relief=FLAT,activebackground='white',bd=0,background='white')
                        show_button.place(x=550,y=300)
                        passwordentry=Entry(c3,width=40,show='*')
                        passwordentry.place(x=300,y=300,height=25)
                        btback=Button(c3,text='Back',font='arial 15',bg='teal',fg='white',command=closescreen)
                        btback.place(x=300,y=400)
                        btclear=Button(c3,text='Clear',font='arial 15',bg='teal',fg='white',command=cleardata)
                        btclear.place(x=450,y=400)
                    def showupdate():
                        elist=[]
                        def filldata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            sql="select adminid from admin"
                            cur.execute(sql)
                            data=cur.fetchall()
                            for res in data:
                                elist.append(res[0])
                            db.close()
                        def finddata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            a=adminidentry.get()
                            sql="select name,password from admin where adminid='%s'"%(a)
                            cur.execute(sql)
                            data=cur.fetchone()
                            if data==None:
                                messagebox.showinfo('status','Not found')
                            else:
                                nameentry.insert(0,data[0])
                                passwordentry.insert(0,data[1])
                        def updatedata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            a=adminidentry.get()
                            b=nameentry.get()
                            c=passwordentry.get()
                            sql="update admin set name='%s',password='%s' where adminid='%s'"%(b,c,a)
                            cur.execute(sql)
                            messagebox.showinfo('update','updated')
                            db.commit()
                            db.close()
                            adminidentry.delete(0,100)
                            nameentry.delete(0,100)
                            passwordentry.delete(0,100)
                        def closescreen():
                            c3.destroy()
                        def cleardata():
                            adminidentry.delete(0,100)
                            nameentry.delete(0,100)
                            passwordentry.delete(0,100)
                            
                        c3=Canvas(c7,width=1800,height=700,bg='orange')
                        c3.place(x=205,y=105)               
                        lbladmin=Label(c3,text='ADMIN(UPDATE PAGE)',font='bold 20 underline',bg='black',fg='yellow')
                        lbladmin.place(x=270,y=20)
                        lbladminid=Label(c3,text='Adminid',font='arial 15',bg='tan',fg='black')
                        lbladminid.place(x=150,y=100)
                        filldata()
                        adminidentry=ttk.Combobox(c3,width='40')
                        adminidentry['values']=elist
                        adminidentry.place(x=300,y=100,height=25)
                        btfind=Button(c3,text='Find',font='arial 15',bg='teal',fg='white',command=finddata)
                        btfind.place(x=570,y=100)
                        lblname=Label(c3,text='Name',font='arial 15',bg='tan',fg='black')
                        lblname.place(x=150,y=200)
                        nameentry=Entry(c3,width=40)
                        nameentry.place(x=300,y=200,height=25)
                        lblpassword=Label(c3,text='Password',font='arial 15',bg='tan',fg='black')
                        lblpassword.place(x=150,y=300)
                        passwordentry=Entry(c3,width=40,show='*')
                        passwordentry.place(x=300,y=300,height=25)
                        def show_password():
                            if passwordentry.cget('show')=='*':
                                passwordentry.config(show='')
                            else:
                                passwordentry.config(show='*')
                        show_button=Checkbutton(c3,text="show password",command=show_password)
                        show_button.place(x=300,y=350)
                        btsave=Button(c3,text='Update',font='arial 15',bg='teal',fg='white',command=updatedata)
                        btsave.place(x=300,y=400)
                        btclear=Button(c3,text='Clear',font='arial 15',bg='teal',fg='white',command=cleardata)
                        btclear.place(x=450,y=400)
                        btback=Button(c3,text='Back',font='arial 15',bg='teal',fg='white',command=closescreen)
                        btback.place(x=600,y=400)
                    def showdelete():
                        elist=[]
                        def filldata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            sql="select adminid from admin"
                            cur.execute(sql)
                            data=cur.fetchall()
                            for res in data:
                                elist.append(res[0])
                            db.close()
                        def deletedata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            a=adminidentry.get()
                            sql="delete from admin where adminid='%s'"%(a)
                            cur.execute(sql)
                            data=cur.fetchall()
                            db.commit()
                            if data==None:
                                messagebox.showinfo('status','No record found')
                            else:
                                messagebox.showinfo('delete','Deleted')
                            db.close()
                            adminidentry.delete(0,100)
                        def closescreen():
                            c3.destroy()
        
                        c3=Canvas(c7,width=1800,height=700,bg='orange')
                        c3.place(x=205,y=105)
                        lbladmin=Label(c3,text='ADMIN(DELETE PAGE)',font='bold 20 underline',bg='black',fg='yellow')
                        lbladmin.place(x=270,y=20)
                        lbladminid=Label(c3,text='Adminid',font='arial 15',bg='tan',fg='black')
                        lbladminid.place(x=150,y=100)
                        adminidentry=ttk.Combobox(c3,width='40')
                        filldata()
                        adminidentry['values']=elist
                        adminidentry.place(x=300,y=100,height=25)
                        btdelete=Button(c3,text='Delete',font='arial 15',bg='teal',fg='white',command=deletedata)
                        btdelete.place(x=300,y=200)
                        btback=Button(c3,text='Back',font='arial 15',bg='teal',fg='white',command=closescreen)
                        btback.place(x=450,y=200)
        
                    c2=Canvas(c7,width=200,height=700,bg='teal')
                    c2.place(x=5,y=105)
                    lbladmin=Label(c2,text='Admin',font='arial 20 underline',bg='black',fg='yellow')
                    lbladmin.place(x=40,y=20)
                    btinsert=Button(c2,text='Insert Data',font='bold 20',bg='green',fg='white',command=showinsert)
                    btinsert.place(x=10,y=100)
                    btfind=Button(c2,text='Find Data',font='bold 20',bg='green',fg='white',command=showfind)
                    btfind.place(x=10,y=200)
                    btupdate=Button(c2,text='Update Data',font='bold 20',bg='green',fg='white',command=showupdate)
                    btupdate.place(x=10,y=300)
                    btdelete=Button(c2,text='Delete Data',font='bold 20',bg='green',fg='white',command=showdelete)
                    btdelete.place(x=10,y=400)
        
                def candidates():
                    def showinsert():
                        def savedata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            a=candidateidentry.get()
                            b=nameentry.get()
                            c=addressentry.get()
                            d=cityentry.get()
                            e=phoneentry.get()
                            f=emailentry.get()
                            g=govtidtypeentry.get()
                            h=govtidnumberentry.get()
                            i=wardnoentry.get()
                            if len(a)==0 or len(b)==0 or len(c)==0 or len(d)==0 or len(e)==0 or len(f)==0 or len(g)==0 or len(h)==0 or len(i)==0:
                                messagebox.showerror('Error','Blanks are not allowed')
                            elif len(e)<10 or len(e)>10:
                                messagebox.showerror('Error','Only 10 digits Phone No. allowed')
                            
                            else:
                                sql="insert into candidates values('%s','%s','%s','%s','%s','%s','%s','%s','%s')"%(a,b,c,d,e,f,g,h,i)
                                cur.execute(sql)
                                db.commit()
                                data=cur.fetchall()
                                messagebox.showinfo('save','saved')
                            db.close()
                            
                        def closescreen():
                            c3.destroy()
                        def cleardata():
                            candidateidentry.delete(0,100)
                            nameentry.delete(0,100)
                            addressentry.delete(0,100)
                            cityentry.delete(0,100)
                            phoneentry.delete(0,100)
                            emailentry.delete(0,100)
                            govtidtypeentry.delete(0,100)
                            govtidnumberentry.delete(0,100)
                            wardnoentry.delete(0,100)
                        def checkdata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            a=candidateidentry.get()
                            if len(a)==0:
                                messagebox.shoerror('Error','Plz Enter Candidateid')
                            else:
                                sql="select count(*) from candidates where candidateid='%s'"%(a)
                                cur.execute(sql)
                                data=cur.fetchall()
                                x=0
                                for res in data:
                                    print(res)
                                    x=res[0]
                                if x==0:
                                    lblstatus.config(text='GO AHEAD',fg='green')
                                else:
                                    lblstatus.config(text='NOT OK',fg='red')
                                
                        c3=Canvas(c7,width=1800,height=700,bg='orange')
                        c3.place(x=205,y=105)
                        lblcandidates=Label(c3,text='CANDIDATES(INSERT PAGE)',font='bold 20 underline',bg='black',fg='yellow')
                        lblcandidates.place(x=220,y=10)
                        lblcandidateid=Label(c3,text='Candidateid',font='arial 15',bg='tan',fg='black')
                        lblcandidateid.place(x=150,y=70)
                        candidateidentry=Entry(c3,width=40)
                        candidateidentry.place(x=300,y=70,height=25)
                        lblname=Label(c3,text='Name',font='arial 15',bg='tan',fg='black')
                        lblname.place(x=150,y=130)
                        nameentry=Entry(c3,width=40)
                        nameentry.place(x=300,y=130,height=25)
                        lbladdress=Label(c3,text='Address',font='arial 15',bg='tan',fg='black')
                        lbladdress.place(x=150,y=190)
                        addressentry=Entry(c3,width=40)
                        addressentry.place(x=300,y=190,height=25)
                        lblcity=Label(c3,text='City',font='arial 15',bg='tan',fg='black')
                        lblcity.place(x=150,y=250)
                        cityentry=Entry(c3,width=40)
                        cityentry.place(x=300,y=250,height=25)
                        lblphone=Label(c3,text='Phone',font='arial 15',bg='tan',fg='black')
                        lblphone.place(x=150,y=310,height=25)
                        phoneentry=Entry(c3,width=40)
                        phoneentry.place(x=300,y=310,height=25)
                        lblemail=Label(c3,text='Email',font='arial 15',bg='tan',fg='black')
                        lblemail.place(x=650,y=130)
                        emailentry=Entry(c3,width=40)
                        emailentry.place(x=780,y=130,height=25)
                        lblgovtidtype=Label(c3,text='Govtidtype',font='arial 15',bg='tan',fg='black')
                        lblgovtidtype.place(x=650,y=190)
                        govtidtypeentry=ttk.Combobox(c3,width=40)
                        govtidtypeentry['values']=['Adhar Card','Pan Card','Voter Card','Driving Lisence']
                        govtidtypeentry.place(x=780,y=190,height=25)
                        lblgovtidnumber=Label(c3,text='Govtidnumber',font='arial 15',bg='tan',fg='black')
                        lblgovtidnumber.place(x=650,y=250)
                        govtidnumberentry=Entry(c3,width=40)
                        govtidnumberentry.place(x=780,y=250,height=25)
                        lblwardno=Label(c3,text='Wardno',font='arial 15',bg='tan',fg='black')
                        lblwardno.place(x=650,y=310)
                        wardnoentry=Entry(c3,width=40)
                        wardnoentry.place(x=780,y=310,height=25)
                        btcheck=Button(c3,text='Check',bg='silver',fg='black',font='arial 15',command=checkdata)
                        btcheck.place(x=550,y=70)
                        lblstatus=Label(c3,text='Status',font='bold 20')
                        lblstatus.place(x=650,y=70)
                        btsave=Button(c3,text='Save',font='arial 15',bg='silver',fg='black',command=savedata)
                        btsave.place(x=300,y=410)
                        btback=Button(c3,text='Back',font='arial 15',fg='black',bg='silver',command=closescreen)
                        btback.place(x=400,y=410)
                        btclear=Button(c3,text='Clear',font='arial 15',bg='silver',fg='black',command=cleardata)
                        btclear.place(x=500,y=410)
        
                    def showfind():
                        elist=[]
                        def filldata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            sql="select candidateid from candidates"
                            cur.execute(sql)
                            data=cur.fetchall()
                            for res in data:
                                elist.append(res[0])
                            db.close()
                                
                        def finddata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            a=candidateidentry.get()
                            sql="select name,address,city,phone,email,govtidtype,govtidnumber,wardno from candidates where candidateid='%s'"%(a)
                            cur.execute(sql)
                            data=cur.fetchone()
                            if data==None:
                                messagebox.showinfo('status','Record not found')
                            else:
                                nameentry.insert(0,data[0])
                                addressentry.insert(0,data[1])
                                cityentry.insert(0,data[2])
                                phoneentry.insert(0,data[3])
                                emailentry.insert(0,data[4])
                                govtidtypeentry.insert(0,data[5])
                                govtidnumberentry.insert(0,data[6])
                                wardnoentry.insert(0,data[7])
                            db.close()
                            
                        def closescreen():
                            c3.destroy()
                        def cleardata():
                            candidateidentry.delete(0,100)
                            nameentry.delete(0,100)
                            addressentry.delete(0,100)
                            cityentry.delete(0,100)
                            phoneentry.delete(0,100)
                            emailentry.delete(0,100)
                            govtidtypeentry.delete(0,100)
                            govtidnumberentry.delete(0,100)
                            wardnoentry.delete(0,100)
                            
                        c3=Canvas(c7,width=1800,height=700,bg='orange')
                        c3.place(x=205,y=105)
                        lblcandidates=Label(c3,text='CANDIDATES(Find PAGE)',font='bold 20 underline',bg='black',fg='yellow')
                        lblcandidates.place(x=220,y=10)
                        lblcandidateid=Label(c3,text='Candidateid',font='arial 15')
                        lblcandidateid.place(x=150,y=70)
                        candidateidentry=ttk.Combobox(c3,width=40)
                        filldata()
                        candidateidentry['values']=elist
                        candidateidentry.place(x=300,y=70,height=25)
                        lblname=Label(c3,text='Name',font='arial 15')
                        lblname.place(x=150,y=130)
                        nameentry=Entry(c3,width=40)
                        nameentry.place(x=300,y=130,height=25)
                        lbladdress=Label(c3,text='Address',font='arial 15')
                        lbladdress.place(x=150,y=190)
                        addressentry=Entry(c3,width=40)
                        addressentry.place(x=300,y=190,height=25)
                        lblcity=Label(c3,text='City',font='arial 15')
                        lblcity.place(x=150,y=250)
                        cityentry=Entry(c3,width=40)
                        cityentry.place(x=300,y=250,height=25)
                        lblphone=Label(c3,text='Phone',font='arial 15')
                        lblphone.place(x=150,y=310,height=25)
                        phoneentry=Entry(c3,width=40)
                        phoneentry.place(x=300,y=310,height=25)
                        lblemail=Label(c3,text='Email',font='arial 15')
                        lblemail.place(x=650,y=130)
                        emailentry=Entry(c3,width=40)
                        emailentry.place(x=780,y=130,height=25)
                        lblgovtidtype=Label(c3,text='Govtidtype',font='arial 15')
                        lblgovtidtype.place(x=650,y=190)
                        govtidtypeentry=ttk.Combobox(c3,width=40)
                        govtidtypeentry['values']=['Adhar Card','Pan Card','Voter Card','Driving Lisence']
                        govtidtypeentry.place(x=780,y=190,height=25)
                        lblgovtidnumber=Label(c3,text='Govtidnumber',font='arial 15')
                        lblgovtidnumber.place(x=650,y=250)
                        govtidnumberentry=Entry(c3,width=40)
                        govtidnumberentry.place(x=780,y=250,height=25)
                        lblwardno=Label(c3,text='Wardno',font='arial 15')
                        lblwardno.place(x=650,y=310)
                        wardnoentry=Entry(c3,width=40)
                        wardnoentry.place(x=780,y=310,height=25)
                        btfind=Button(c3,text='Find',bg='silver',fg='black',font='arial 15',command=finddata)
                        btfind.place(x=570,y=70)
                        btback=Button(c3,text='Back',font='arial 15',fg='black',bg='silver',command=closescreen)
                        btback.place(x=300,y=410)
                        btclear=Button(c3,text='Clear',font='arial 15',bg='silver',fg='black',command=cleardata)
                        btclear.place(x=400,y=410)
                    def showupdate():
                        elist=[]
                        def filldata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            sql="select candidateid from candidates"
                            cur.execute(sql)
                            data=cur.fetchall()
                            for res in data:
                                elist.append(res[0])
                            db.close()
                                
                        def finddata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            a=candidateidentry.get()
                            sql="select name,address,city,phone,email,govtidtype,govtidnumber,wardno from candidates where candidateid='%s'"%(a)
                            cur.execute(sql)
                            data=cur.fetchone()
                            if data==None:
                                messagebox.showinfo('status','Record not found')
                            else:
                                nameentry.insert(0,data[0])
                                addressentry.insert(0,data[1])
                                cityentry.insert(0,data[2])
                                phoneentry.insert(0,data[3])
                                emailentry.insert(0,data[4])
                                govtidtypeentry.insert(0,data[5])
                                govtidnumberentry.insert(0,data[6])
                                wardnoentry.insert(0,data[7])
                            db.close()
                        def updatedata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            a=candidateidentry.get()
                            b=nameentry.get()
                            c=addressentry.get()
                            d=cityentry.get()
                            e=phoneentry.get()
                            f=emailentry.get()
                            g=govtidtypeentry.get()
                            h=govtidnumberentry.get()
                            i=wardnoentry.get()
                            sql="update candidates set name='%s',address='%s',city='%s',phone='%s',email='%s',govtidtype='%s',govtidnumber='%s',wardno='%s' where candidateid='%s'"%(b,c,d,e,f,g,h,i,a)
                            cur.execute(sql)
                            messagebox.showinfo('update','updated')
                            db.commit()
                            db.close()
                            candidateidentry.delete(0,100)
                            nameentry.delete(0,100)
                            addressentry.delete(0,100)
                            cityentry.delete(0,100)
                            phoneentry.delete(0,100)
                            emailentry.delete(0,100)
                            govtidtypeentry.delete(0,100)
                            govtidnumberentry.delete(0,100)
                            wardnoentry.delete(0,100)
                            
                        def closescreen():
                            c3.destroy()
                        def cleardata():
                            candidateidentry.delete(0,100)
                            nameentry.delete(0,100)
                            addressentry.delete(0,100)
                            cityentry.delete(0,100)
                            phoneentry.delete(0,100)
                            emailentry.delete(0,100)
                            govtidtypeentry.delete(0,100)
                            govtidnumberentry.delete(0,100)
                            wardnoentry.delete(0,100)
                            
                        c3=Canvas(c7,width=1800,height=700,bg='orange')
                        c3.place(x=205,y=105)
                        lblcandidates=Label(c3,text='CANDIDATES(UPDATE PAGE)',font='bold 20 underline',bg='black',fg='yellow')
                        lblcandidates.place(x=220,y=10)
                        lblcandidateid=Label(c3,text='Candidateid',font='arial 15')
                        lblcandidateid.place(x=150,y=70)
                        candidateidentry=ttk.Combobox(c3,width=40)
                        filldata()
                        candidateidentry['values']=elist
                        candidateidentry.place(x=300,y=70,height=25)
                        lblname=Label(c3,text='Name',font='arial 15')
                        lblname.place(x=150,y=130)
                        nameentry=Entry(c3,width=40)
                        nameentry.place(x=300,y=130,height=25)
                        lbladdress=Label(c3,text='Address',font='arial 15')
                        lbladdress.place(x=150,y=190)
                        addressentry=Entry(c3,width=40)
                        addressentry.place(x=300,y=190,height=25)
                        lblcity=Label(c3,text='City',font='arial 15')
                        lblcity.place(x=150,y=250)
                        cityentry=Entry(c3,width=40)
                        cityentry.place(x=300,y=250,height=25)
                        lblphone=Label(c3,text='Phone',font='arial 15')
                        lblphone.place(x=150,y=310,height=25)
                        phoneentry=Entry(c3,width=40)
                        phoneentry.place(x=300,y=310,height=25)
                        lblemail=Label(c3,text='Email',font='arial 15')
                        lblemail.place(x=650,y=130)
                        emailentry=Entry(c3,width=40)
                        emailentry.place(x=780,y=130,height=25)
                        lblgovtidtype=Label(c3,text='Govtidtype',font='arial 15')
                        lblgovtidtype.place(x=650,y=190)
                        govtidtypeentry=ttk.Combobox(c3,width=40)
                        govtidtypeentry['values']=['Adhar Card','Pan Card','Voter Card','Driving Lisence']
                        govtidtypeentry.place(x=780,y=190,height=25)
                        lblgovtidnumber=Label(c3,text='Govtidnumber',font='arial 15')
                        lblgovtidnumber.place(x=650,y=250)
                        govtidnumberentry=Entry(c3,width=40)
                        govtidnumberentry.place(x=780,y=250,height=25)
                        lblwardno=Label(c3,text='Wardno',font='arial 15')
                        lblwardno.place(x=650,y=310)
                        wardnoentry=Entry(c3,width=40)
                        wardnoentry.place(x=780,y=310,height=25)
                        btfind=Button(c3,text='Find',bg='silver',fg='black',font='arial 15',command=finddata)
                        btfind.place(x=570,y=70)
                        btupdate=Button(c3,text='Update',font='arial 15',bg='silver',fg='black',command=updatedata)
                        btupdate.place(x=300,y=410)
                        btback=Button(c3,text='Back',font='arial 15',fg='black',bg='silver',command=closescreen)
                        btback.place(x=400,y=410)
                        btclear=Button(c3,text='Clear',font='arial 15',bg='silver',fg='black',command=cleardata)
                        btclear.place(x=500,y=410)
                    def showdelete():
                        elist=[]
                        def filldata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            sql="select candidateid from candidates"
                            cur.execute(sql)
                            data=cur.fetchall()
                            for res in data:
                                elist.append(res[0])
                            db.close()
                        def deletedata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            a=candidateidentry.get()
                            sql="delete from candidates where candidateid='%s'"%(a)
                            cur.execute(sql)
                            db.commit()
                            data=cur.fetchall()
                            if data==None:
                                messagebox.showerror('error','No record')
                            else:
                                messagebox.showinfo('delete','Deleted')
                            db.close()
                            candidateidentry.delete(0,100)
                                
                        def closescreen():
                            c3.destroy()
                            
                        c3=Canvas(c7,width=1800,height=700,bg='orange')
                        c3.place(x=205,y=105) 
                        lblcandidates=Label(c3,text='CANDIDATES(DELETE PAGE)',font='bold 20 underline',bg='black',fg='yellow')
                        lblcandidates.place(x=220,y=10)
                        lblcandidateid=Label(c3,text='Candidateid',font='arial 15')
                        lblcandidateid.place(x=150,y=70)
                        candidateidentry=ttk.Combobox(c3,width=40)
                        filldata()
                        candidateidentry['values']=elist
                        candidateidentry.place(x=300,y=70,height=25)
                        btupdate=Button(c3,text='Delete',font='arial 15',bg='silver',fg='black',command=deletedata)
                        btupdate.place(x=300,y=170)
                        btback=Button(c3,text='Back',font='arial 15',fg='black',bg='silver',command=closescreen)
                        btback.place(x=450,y=170)
        
                    c2=Canvas(c7,width=200,height=700,bg='teal')
                    c2.place(x=5,y=105)
                    lblcandidates=Label(c2,text='Candidates',font='arial 20 underline',bg='black',fg='yellow')
                    lblcandidates.place(x=30,y=20)
                    btinsert=Button(c2,text='Insert Data',font='bold 20',bg='green',fg='white',command=showinsert)
                    btinsert.place(x=10,y=100)
                    btfind=Button(c2,text='Find Data',font='bold 20',bg='green',fg='white',command=showfind)
                    btfind.place(x=10,y=200)
                    btupdate=Button(c2,text='Update Data',font='bold 20',bg='green',fg='white',command=showupdate)
                    btupdate.place(x=10,y=300)
                    btdelete=Button(c2,text='Delete Data',font='bold 20',bg='green',fg='white',command=showdelete)
                    btdelete.place(x=10,y=400)
                    
                def electionschedule():
                    def showinsert():
                        def savedata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            a=electionidentry.get()
                            b=dateofelectionentry.get()
                            c=candidateidentry.get()
                            d=timefromentry.get()
                            e=timetoentry.get()
                            if len(a)==0 or len(b)==0 or len(c)==0 or len(d)==0 or len(e)==0:
                                messagebox.showerror('Error','Blanks are not allowed')
                            else:
                                sql="insert into electionschedule values('%s','%s','%s','%s','%s')"%(a,b,c,d,e)
                                cur.execute(sql)
                                db.commit()
                                data=cur.fetchall()
                                messagebox.showinfo('save','saved')
                            db.close()
                            
                        def closescreen():
                            c3.destroy()
                        def cleardata():
                            electionidentry.delete(0,100)
                            dateofelectionentry.delete(0,100)
                            candidateidentry.delete(0,100)
                            timefromentry.delete(0,100)
                            timetoentry.delete(0,100)
                            
                        def checkdata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            a=electionidentry.get()
                            if len(a)==0:
                                messagebox.showerror('Error','Plz Enter Electionid')
                            else:
                                sql="select count(*) from electionschedule where electionid='%s'"%(a)
                                cur.execute(sql)
                                data=cur.fetchall()
                                x=0
                                for res in data:
                                    print(res)
                                    x=res[0]
                                if x==0:
                                    lblstatus.config(text='GO AHEAD',fg='green')
                                else:
                                    lblstatus.config(text='NOT OK',fg='red')
                                
                        c3=Canvas(c7,width=1800,height=700,bg='orange')
                        c3.place(x=205,y=105)       
                        lblelectionschedule=Label(c3,text='ELECTIONSCHEDULE(INSERT PAGE)',font='bold 20 underline',bg='black',fg='yellow')
                        lblelectionschedule.place(x=200,y=10)
                        lblelectionid=Label(c3,text='Electionid',font='arial 15')
                        lblelectionid.place(x=150,y=70)
                        electionidentry=Entry(c3,width=40)
                        electionidentry.place(x=320,y=70,height=25)
                        lbldateofelection=Label(c3,text='Date Of Election',font='arial 15')
                        lbldateofelection.place(x=150,y=130)
                        dateofelectionentry=Entry(c3,width=40)
                        dateofelectionentry.place(x=320,y=130,height=25)
                        lblcandidateid=Label(c3,text='Candidateid',font='arial 15')
                        lblcandidateid.place(x=150,y=190)
                        candidateidentry=Entry(c3,width=40)
                        candidateidentry.place(x=320,y=190,height=25)
                        lbltimefrom=Label(c3,text='Time From',font='arial 15')
                        lbltimefrom.place(x=150,y=250)
                        timefromentry=Entry(c3,width=40)
                        timefromentry.place(x=320,y=250,height=25)
                        lbltimeto=Label(c3,text='Time To',font='arial 15')
                        lbltimeto.place(x=150,y=310,height=25)
                        timetoentry=Entry(c3,width=40)
                        timetoentry.place(x=320,y=310,height=25)
                        btcheck=Button(c3,text='Check',bg='silver',fg='black',font='arial 15',command=checkdata)
                        btcheck.place(x=570,y=70)
                        lblstatus=Label(c3,text='Status',font='arial 15')
                        lblstatus.place(x=670,y=70)
                        btsave=Button(c3,text='Save',font='arial 15',bg='silver',fg='black',command=savedata)
                        btsave.place(x=300,y=410)
                        btback=Button(c3,text='Back',font='arial 15',fg='black',bg='silver',command=closescreen)
                        btback.place(x=400,y=410)
                        btclear=Button(c3,text='Clear',font='arial 15',bg='silver',fg='black',command=cleardata)
                        btclear.place(x=500,y=410)
                        
                    def showfind():
                        elist=[]
                        def filldata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            sql="select electionid from electionschedule"
                            cur.execute(sql)
                            data=cur.fetchall()
                            for res in data:
                                elist.append(res[0])
                            db.close()
                                
                        def finddata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            a=electionidentry.get()
                            sql="select dateofelection,candidateid,timefrom,timeto from electionschedule where electionid='%s'"%(a)
                            cur.execute(sql)
                            data=cur.fetchone()
                            if data==None:
                                messagebox.showinfo('status','Record not found')
                            else:
                                dateofelectionentry.insert(0,data[0])
                                candidateidentry.insert(0,data[1])
                                timefromentry.insert(0,data[2])
                                timetoentry.insert(0,data[3])
                            db.close()
                            
                        def closescreen():
                            c3.destroy()
                        def cleardata():
                            electionidentry.delete(0,100)
                            dateofelectionentry.delete(0,100)
                            candidateidentry.delete(0,100)
                            timefromentry.delete(0,100)
                            timetoentry.delete(0,100)
                            
                        c3=Canvas(c7,width=1800,height=700,bg='orange')
                        c3.place(x=205,y=105)        
                        lblelectionschedule=Label(c3,text='ELECTIONSCHEDULE(FIND PAGE)',font='bold 20 underline',bg='black',fg='yellow')
                        lblelectionschedule.place(x=200,y=10)
                        lblelectionid=Label(c3,text='Electionid',font='arial 15')
                        lblelectionid.place(x=150,y=70)
                        electionidentry=ttk.Combobox(c3,width=40)
                        filldata()
                        electionidentry['values']=elist
                        electionidentry.place(x=320,y=70,height=25)
                        lbldateofelection=Label(c3,text='Date Of Election',font='arial 15')
                        lbldateofelection.place(x=150,y=130)
                        dateofelectionentry=Entry(c3,width=40)
                        dateofelectionentry.place(x=320,y=130,height=25)
                        lblcandidateid=Label(c3,text='Candidateid',font='arial 15')
                        lblcandidateid.place(x=150,y=190)
                        candidateidentry=Entry(c3,width=40)
                        candidateidentry.place(x=320,y=190,height=25)
                        lbltimefrom=Label(c3,text='Time From',font='arial 15')
                        lbltimefrom.place(x=150,y=250)
                        timefromentry=Entry(c3,width=40)
                        timefromentry.place(x=320,y=250,height=25)
                        lbltimeto=Label(c3,text='Time To',font='arial 15')
                        lbltimeto.place(x=150,y=310,height=25)
                        timetoentry=Entry(c3,width=40)
                        timetoentry.place(x=320,y=310,height=25)
                        btfind=Button(c3,text='Find',bg='silver',fg='black',font='arial 15',command=finddata)
                        btfind.place(x=590,y=70)
                        btback=Button(c3,text='Back',font='arial 15',fg='black',bg='silver',command=closescreen)
                        btback.place(x=300,y=410)
                        btclear=Button(c3,text='Clear',font='arial 15',bg='silver',fg='black',command=cleardata)
                        btclear.place(x=400,y=410)
                    
                    def showupdate():
                        elist=[]
                        def filldata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            sql="select electionid from electionschedule"
                            cur.execute(sql)
                            data=cur.fetchall()
                            for res in data:
                                elist.append(res[0])
                            db.close()
                                
                        def finddata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            a=electionidentry.get()
                            sql="select dateofelection,candidateid,timefrom,timeto from electionschedule where electionid='%s'"%(a)
                            cur.execute(sql)
                            data=cur.fetchone()
                            if data==None:
                                messagebox.showinfo('status','Record not found')
                            else:
                                dateofelectionentry.insert(0,data[0])
                                candidateidentry.insert(0,data[1])
                                timefromentry.insert(0,data[2])
                                timetoentry.insert(0,data[3])
                            db.close()
                            
                        def updatedata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            a=electionidentry.get()
                            b=dateofelectionentry.get()
                            c=candidateidentry.get()
                            d=timefromentry.get()
                            e=timetoentry.get()
                            sql="update electionschedule set dateofelection='%s',candidateid='%s',timefrom='%s',timeto='%s' where electionid='%s'"%(b,c,d,e,a)
                            cur.execute(sql)
                            db.commit()
                            data=cur.fetchall()
                            messagebox.showinfo('update','Data Updated')
                            db.close()
                            electionidentry.delete(0,100)
                            dateofelectionentry.delete(0,100)
                            candidateidentry.delete(0,100)
                            timefromentry.delete(0,100)
                            timetoentry.delete(0,100)
                            
                            
                        def closescreen():
                            c3.destroy()
                        def cleardata():
                            electionidentry.delete(0,100)
                            dateofelectionentry.delete(0,100)
                            candidateidentry.delete(0,100)
                            timefromentry.delete(0,100)
                            timetoentry.delete(0,100)
                            
                        c3=Canvas(c7,width=1800,height=700,bg='orange')
                        c3.place(x=205,y=105)            
                        lblelectionschedule=Label(c3,text='ELECTIONSCHEDULE(UPDATE PAGE)',font='bold 20 underline',bg='black',fg='yellow')
                        lblelectionschedule.place(x=180,y=10)
                        lblelectionid=Label(c3,text='Electionid',font='arial 15')
                        lblelectionid.place(x=150,y=70)
                        electionidentry=ttk.Combobox(c3,width=40)
                        filldata()
                        electionidentry['values']=elist
                        electionidentry.place(x=320,y=70,height=25)
                        lbldateofelection=Label(c3,text='Date Of Election',font='arial 15')
                        lbldateofelection.place(x=150,y=130)
                        dateofelectionentry=Entry(c3,width=40)
                        dateofelectionentry.place(x=320,y=130,height=25)
                        lblcandidateid=Label(c3,text='Candidateid',font='arial 15')
                        lblcandidateid.place(x=150,y=190)
                        candidateidentry=Entry(c3,width=40)
                        candidateidentry.place(x=320,y=190,height=25)
                        lbltimefrom=Label(c3,text='Time From',font='arial 15')
                        lbltimefrom.place(x=150,y=250)
                        timefromentry=Entry(c3,width=40)
                        timefromentry.place(x=320,y=250,height=25)
                        lbltimeto=Label(c3,text='Time To',font='arial 15')
                        lbltimeto.place(x=150,y=310,height=25)
                        timetoentry=Entry(c3,width=40)
                        timetoentry.place(x=320,y=310,height=25)
                        btfind=Button(c3,text='Find',bg='silver',fg='black',font='arial 15',command=finddata)
                        btfind.place(x=590,y=70)
                        btupdate=Button(c3,text='Update',font='arial 15',fg='black',bg='silver',command=updatedata)
                        btupdate.place(x=300,y=410)
                        btclose=Button(c3,text='Close',font='arial 15',fg='black',bg='silver',command=closescreen)
                        btclose.place(x=400,y=410)
                        btclear=Button(c3,text='Clear',font='arial 15',bg='silver',fg='black',command=cleardata)
                        btclear.place(x=500,y=410)
                        
                    def showdelete():
                        elist=[]
                        def filldata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            sql="select electionid from electionschedule"
                            cur.execute(sql)
                            data=cur.fetchall()
                            for res in data:
                                elist.append(res[0])
                            db.close()
                                
                        def deletedata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            a=electionidentry.get()
                            sql="delete from electionschedule where electionid='%s'"%(a)
                            cur.execute(sql)
                            db.commit()
                            data=cur.fetchall()
                            if data==None:
                                messagebox.showerror('Error','No Record')
                            else:
                                messagebox.showinfo('delete','Deleted')
                            db.close()
                            electionidentry.delete(0,100)
                            
                        def closescreen():
                            c3.destroy()
                            
                        c3=Canvas(c7,width=1800,height=700,bg='orange')
                        c3.place(x=205,y=105)          
                        lblelectionschedule=Label(c3,text='ELECTIONSCHEDULE(DELETE PAGE)',font='bold 20 underline',bg='black',fg='yellow')
                        lblelectionschedule.place(x=200,y=10)
                        lblelectionid=Label(c3,text='Electionid',font='arial 15')
                        lblelectionid.place(x=150,y=70)
                        electionidentry=ttk.Combobox(c3,width=40)
                        filldata()
                        electionidentry['values']=elist
                        electionidentry.place(x=320,y=70,height=25)
                        btback=Button(c3,text='Back',font='arial 15',fg='black',bg='silver',command=closescreen)
                        btback.place(x=450,y=170)
                        btdelete=Button(c3,text='Delete',font='arial 15',bg='silver',fg='black',command=deletedata)
                        btdelete.place(x=300,y=170)
        
                    c2=Canvas(c7,width=200,height=700,bg='teal')
                    c2.place(x=5,y=105)
                    lblelectionschedule=Label(c2,text='Election Schedule',font='arial 17 underline',bg='black',fg='yellow')
                    lblelectionschedule.place(x=10,y=20)
                    btinsert=Button(c2,text='Insert Data',font='bold 20',bg='green',fg='white',command=showinsert)
                    btinsert.place(x=10,y=100)
                    btfind=Button(c2,text='Find Data',font='bold 20',bg='green',fg='white',command=showfind)
                    btfind.place(x=10,y=200)
                    btupdate=Button(c2,text='Update Data',font='bold 20',bg='green',fg='white',command=showupdate)
                    btupdate.place(x=10,y=300)
                    btdelete=Button(c2,text='Delete Data',font='bold 20',bg='green',fg='white',command=showdelete)
                    btdelete.place(x=10,y=400)
                    
                def user():
                    def showinsert():
                        def savedata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            a=useridentry.get()
                            b=unameentry.get()
                            c=passwordentry.get()
                            d=addressentry.get()
                            e=cityentry.get()
                            f=emailentry.get()
                            g=wardnoentry.get()
                            if len(a)==0 or len(b)==0 or len(c)==0 or len(d)==0 or len(e)==0 or len(f)==0 or len(g)==0:
                                messagebox.showerror('Error','Blanks are not allowed')
                            elif len(c)<8:
                                messagebox.showerror('Error','Password must contain atleast 8 characters ')
                            
                            else:
                                sql="select * from user where userid='%s'"%(a)
                                cur.execute(sql)
                                result=cur.fetchone()
                                if result is not None:
                                    messagebox.showerror("status","User Id already exists")
                                    useridentry.delete(0,100)
                                    unameentry.delete(0,100)
                                    passwordentry.delete(0,100)
                                    addressentry.delete(0,100)
                                    cityentry.delete(0,100)
                                    emailentry.delete(0,100)
                                    wardnoentry.delete(0,100)
                                else:
                                    sql="insert into user values('%s','%s','%s','%s','%s','%s','%s')"%(a,b,c,d,e,f,g)
                                    cur.execute(sql)
                                    db.commit()
                                    data=cur.fetchall()
                                    
                                    messagebox.showinfo('save','saved')
                                db.close()
                                
                        def closescreen():
                            c3.destroy()
                        def cleardata():
                            useridentry.delete(0,100)
                            unameentry.delete(0,100)
                            passwordentry.delete(0,100)
                            addressentry.delete(0,100)
                            cityentry.delete(0,100)
                            emailentry.delete(0,100)
                            wardnoentry.delete(0,100)
                        def checkdata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            a=useridentry.get()
                            if len(a)==0:
                                messagebox.showerror('Error','Plz Enter Userid')
                            else:
                                sql="select count(*) from user where userid='%s'"%(a)
                                cur.execute(sql)
                                data=cur.fetchall()
                                x=0
                                for res in data:
                                    print(res)
                                    x=res[0]
                                if x==0:
                                    lblstatus.config(text='GO AHEAD',fg='green')
                                else:
                                    lblstatus.config(text='Already exist',fg='red')
                                
                        c3=Canvas(c7,width=1800,height=700,bg='orange')
                        c3.place(x=205,y=105)          
                        lblcandidates=Label(c3,text='USER(INSERT PAGE)',font='bold 20 underline',bg='black',fg='yellow')
                        lblcandidates.place(x=220,y=10)
                        lbluserid=Label(c3,text='userid',font='arial 15')
                        lbluserid.place(x=150,y=70)
                        useridentry=Entry(c3,width=40)
                        useridentry.place(x=300,y=70,height=25)
                        lbluname=Label(c3,text='UName',font='arial 15')
                        lbluname.place(x=150,y=130)
                        unameentry=Entry(c3,width=40)
                        unameentry.place(x=300,y=130,height=25)
                        lblpassword=Label(c3,text='Password',font='arial 15')
                        lblpassword.place(x=150,y=190)
                        def show():
                            hide_button=Button(c3,image=hide_image,command=hide,relief=FLAT,activebackground='white',background='white')
                            hide_button.place(x=550,y=190)
                            passwordentry.config(show='')
                        def hide():
                            show_button=Button(c3,image=show_image,command=show,relief=FLAT,activebackground='white',background='white')
                            show_button.place(x=550,y=190)
                            passwordentry.config(show='*')
                        show_image=ImageTk.PhotoImage(file=r"C:\Users\Nabeel-IT\Pictures\show.png")
                        hide_image=ImageTk.PhotoImage(file=r"C:\Users\Nabeel-IT\Pictures\hide.png")
                        show_button=Button(c3,image=show_image,command=show,relief=FLAT,activebackground='white',bd=0,background='white')
                        show_button.place(x=550,y=190)
                        passwordentry=Entry(c3,width=40,show='*')
                        passwordentry.place(x=300,y=190,height=25)
                        lbladdress=Label(c3,text='Address',font='arial 15')
                        lbladdress.place(x=150,y=250)
                        addressentry=Entry(c3,width=40)
                        addressentry.place(x=300,y=250,height=25)
                        lblcity=Label(c3,text='City',font='arial 15')
                        lblcity.place(x=150,y=310,height=25)
                        cityentry=Entry(c3,width=40)
                        cityentry.place(x=300,y=310,height=25)
                        lblemail=Label(c3,text='Email',font='arial 15')
                        lblemail.place(x=150,y=370)
                        emailentry=Entry(c3,width=40)
                        emailentry.place(x=300,y=370,height=25)
                        lblwardno=Label(c3,text='Wardno',font='arial 15')
                        lblwardno.place(x=150,y=430)
                        wardnoentry=Entry(c3,width=40)
                        wardnoentry.place(x=300,y=430,height=25)
                        btcheck=Button(c3,text='Check',bg='silver',fg='black',font='arial 15',command=checkdata)
                        btcheck.place(x=550,y=70)
                        lblstatus=Label(c3,text='Status',font='bold 20')
                        lblstatus.place(x=650,y=70)
                        btsave=Button(c3,text='Save',font='arial 15',bg='silver',fg='black',command=savedata)
                        btsave.place(x=300,y=500)
                        btback=Button(c3,text='Back',font='arial 15',fg='black',bg='silver',command=closescreen)
                        btback.place(x=400,y=500)
                        btclear=Button(c3,text='Clear',font='arial 15',bg='silver',fg='black',command=cleardata)
                        btclear.place(x=500,y=500)
                    def showfind():
                        elist=[]
                        def filldata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            sql="select userid from user"
                            cur.execute(sql)
                            data=cur.fetchall()
                            for res in data:
                                elist.append(res[0])
                            db.close()
                                
                        def finddata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            a=useridentry.get()
                            sql="select uname,password,address,city,email,wardno from user where userid='%s'"%(a)
                            cur.execute(sql)
                            data=cur.fetchone()
                            if data==None:
                                messagebox.showinfo('status','Record not found')
                            else:
                                unameentry.insert(0,data[0])
                                passwordentry.insert(0,data[1])
                                addressentry.insert(0,data[2])
                                cityentry.insert(0,data[3])
                                emailentry.insert(0,data[4])
                                wardnoentry.insert(0,data[5])
                            db.close()
                            
                        def closescreen():
                            c3.destroy()
                        def cleardata():
                            useridentry.delete(0,100)
                            unameentry.delete(0,100)
                            passwordentry.delete(0,100)
                            addressentry.delete(0,100)
                            cityentry.delete(0,100)
                            emailentry.delete(0,100)
                            wardnoentry.delete(0,100)
                            
                        c3=Canvas(c7,width=1800,height=700,bg='orange')
                        c3.place(x=205,y=105)                 
                        lblcandidates=Label(c3,text='USER(FIND PAGE)',font='bold 20 underline',bg='black',fg='yellow')
                        lblcandidates.place(x=220,y=10)
                        lbluserid=Label(c3,text='userid',font='arial 15')
                        lbluserid.place(x=150,y=70)
                        useridentry=ttk.Combobox(c3,width=40)
                        filldata()
                        useridentry['values']=elist
                        useridentry.place(x=300,y=70,height=25)
                        lbluname=Label(c3,text='UName',font='arial 15')
                        lbluname.place(x=150,y=130)
                        unameentry=Entry(c3,width=40)
                        unameentry.place(x=300,y=130,height=25)
                        lblpassword=Label(c3,text='Password',font='arial 15')
                        lblpassword.place(x=150,y=190)
                        def show():
                            hide_button=Button(c3,image=hide_image,command=hide,relief=FLAT,activebackground='white',background='white')
                            hide_button.place(x=550,y=190)
                            passwordentry.config(show='')
                        def hide():
                            show_button=Button(c3,image=show_image,command=show,relief=FLAT,activebackground='white',background='white')
                            show_button.place(x=550,y=190)
                            passwordentry.config(show='*')
                        show_image=ImageTk.PhotoImage(file=r"C:\Users\Nabeel-IT\Pictures\show.png")
                        hide_image=ImageTk.PhotoImage(file=r"C:\Users\Nabeel-IT\Pictures\hide.png")
                        show_button=Button(c3,image=show_image,command=show,relief=FLAT,activebackground='white',bd=0,background='white')
                        show_button.place(x=550,y=190)
                        passwordentry=Entry(c3,width=40)
                        passwordentry.place(x=300,y=190,height=25)
                        lbladdress=Label(c3,text='Address',font='arial 15')
                        lbladdress.place(x=150,y=250)
                        addressentry=Entry(c3,width=40)
                        addressentry.place(x=300,y=250,height=25)
                        lblcity=Label(c3,text='City',font='arial 15')
                        lblcity.place(x=150,y=310,height=25)
                        cityentry=Entry(c3,width=40)
                        cityentry.place(x=300,y=310,height=25)
                        lblemail=Label(c3,text='Email',font='arial 15')
                        lblemail.place(x=150,y=370)
                        emailentry=Entry(c3,width=40)
                        emailentry.place(x=300,y=370,height=25)
                        lblwardno=Label(c3,text='Wardno',font='arial 15')
                        lblwardno.place(x=150,y=430)
                        wardnoentry=Entry(c3,width=40)
                        wardnoentry.place(x=300,y=430,height=25)
                        btfind=Button(c3,text='Find',bg='silver',fg='black',font='arial 15',command=finddata)
                        btfind.place(x=580,y=70)
                        btback=Button(c3,text='Back',font='arial 15',fg='black',bg='silver',command=closescreen)
                        btback.place(x=300,y=500)
                        btclear=Button(c3,text='Clear',font='arial 15',bg='silver',fg='black',command=cleardata)
                        btclear.place(x=400,y=500)
                    def showupdate():
                        elist=[]
                        def filldata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            sql="select userid from user"
                            cur.execute(sql)
                            data=cur.fetchall()
                            for res in data:
                                elist.append(res[0])
                            db.close()
                                
                        def finddata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            a=useridentry.get()
                            sql="select uname,password,address,city,email,wardno from user where userid='%s'"%(a)
                            cur.execute(sql)
                            data=cur.fetchone()
                            if data==None:
                                messagebox.showinfo('status','Record not found')
                            else:
                                unameentry.insert(0,data[0])
                                passwordentry.insert(0,data[1])
                                addressentry.insert(0,data[2])
                                cityentry.insert(0,data[3])
                                emailentry.insert(0,data[4])
                                wardnoentry.insert(0,data[5])
                            db.close()
                        def updatedata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            a=useridentry.get()
                            b=unameentry.get()
                            c=passwordentry.get()
                            d=addressentry.get()
                            e=cityentry.get()
                            f=emailentry.get()
                            g=wardnoentry.get()
                            sql="update user set uname='%s',password='%s',address='%s',city='%s',email='%s',wardno='%s' where userid='%s'"%(b,c,d,e,f,g,a)
                            cur.execute(sql)
                            db.commit()
                            data=cur.fetchall()
                            messagebox.showinfo('update','Data Updated')
                            db.close()
                            useridentry.delete(0,100)
                            unameentry.delete(0,100)
                            passwordentry.delete(0,100)
                            addressentry.delete(0,100)
                            cityentry.delete(0,100)
                            emailentry.delete(0,100)
                            wardnoentry.delete(0,100)
                            
                            
                        def closescreen():
                            c3.destroy()
                        def cleardata():
                            useridentry.delete(0,100)
                            unameentry.delete(0,100)
                            passwordentry.delete(0,100)
                            addressentry.delete(0,100)
                            cityentry.delete(0,100)
                            emailentry.delete(0,100)
                            wardnoentry.delete(0,100)
                            
                        c3=Canvas(c7,width=1800,height=700,bg='orange')
                        c3.place(x=205,y=105)                        
                        lblcandidates=Label(c3,text='USER(UPDATE PAGE)',font='bold 20 underline',bg='black',fg='yellow')
                        lblcandidates.place(x=220,y=10)
                        lbluserid=Label(c3,text='userid',font='arial 15')
                        lbluserid.place(x=150,y=70)
                        useridentry=ttk.Combobox(c3,width=40)
                        filldata()
                        useridentry['values']=elist
                        useridentry.place(x=300,y=70,height=25)
                        lbluname=Label(c3,text='UName',font='arial 15')
                        lbluname.place(x=150,y=130)
                        unameentry=Entry(c3,width=40)
                        unameentry.place(x=300,y=130,height=25)
                        lblpassword=Label(c3,text='Password',font='arial 15')
                        lblpassword.place(x=150,y=190)
                        def show():
                            hide_button=Button(c3,image=hide_image,command=hide,relief=FLAT,activebackground='white',background='white')
                            hide_button.place(x=550,y=190)
                            passwordentry.config(show='')
                        def hide():
                            show_button=Button(c3,image=show_image,command=show,relief=FLAT,activebackground='white',background='white')
                            show_button.place(x=550,y=190)
                            passwordentry.config(show='*')
                        show_image=ImageTk.PhotoImage(file=r"C:\Users\Nabeel-IT\Pictures\show.png")
                        hide_image=ImageTk.PhotoImage(file=r"C:\Users\Nabeel-IT\Pictures\hide.png")
                        show_button=Button(c3,image=show_image,command=show,relief=FLAT,activebackground='white',bd=0,background='white')
                        show_button.place(x=550,y=190)
                        passwordentry=Entry(c3,width=40,show='*')
                        passwordentry.place(x=300,y=190,height=25)
                        lbladdress=Label(c3,text='Address',font='arial 15')
                        lbladdress.place(x=150,y=250)
                        addressentry=Entry(c3,width=40)
                        addressentry.place(x=300,y=250,height=25)
                        lblcity=Label(c3,text='City',font='arial 15')
                        lblcity.place(x=150,y=310,height=25)
                        cityentry=Entry(c3,width=40)
                        cityentry.place(x=300,y=310,height=25)
                        lblemail=Label(c3,text='Email',font='arial 15')
                        lblemail.place(x=150,y=370)
                        emailentry=Entry(c3,width=40)
                        emailentry.place(x=300,y=370,height=25)
                        lblwardno=Label(c3,text='Wardno',font='arial 15')
                        lblwardno.place(x=150,y=430)
                        wardnoentry=Entry(c3,width=40)
                        wardnoentry.place(x=300,y=430,height=25)
                        btfind=Button(c3,text='Find',bg='silver',fg='black',font='arial 15',command=finddata)
                        btfind.place(x=580,y=70)
                        btupdate=Button(c3,text='Update',font='arial 15',fg='black',bg='silver',command=updatedata)
                        btupdate.place(x=300,y=500)
                        btclose=Button(c3,text='Close',font='arial 15',fg='black',bg='silver',command=closescreen)
                        btclose.place(x=400,y=500)
                        btclear=Button(c3,text='Clear',font='arial 15',bg='silver',fg='black',command=cleardata)
                        btclear.place(x=500,y=500)
                    def showdelete():
                        elist=[]
                        def filldata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            sql="select userid from user"
                            cur.execute(sql)
                            data=cur.fetchall()
                            for res in data:
                                elist.append(res[0])
                            db.close()
                                
                        def deletedata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            a=useridentry.get()
                            sql="delete from user where userid='%s'"%(a)
                            cur.execute(sql)
                            db.commit()
                            data=cur.fetchall()
                            if data==None:
                                messagebox.showerror('Error','No Record')
                            else:
                                messagebox.showinfo('delete','Deleted')
                            db.close()
                            electionidentry.delete(0,100)
                            
                        def closescreen():
                            c3.destroy()
                            
                        c3=Canvas(c7,width=1800,height=700,bg='orange')
                        c3.place(x=205,y=105)          
                        lblelectionschedule=Label(c3,text='USER(DELETE PAGE)',font='bold 20 underline',bg='black',fg='yellow')
                        lblelectionschedule.place(x=200,y=10)
                        lblelectionid=Label(c3,text='userid',font='arial 15')
                        lblelectionid.place(x=150,y=70)
                        electionidentry=ttk.Combobox(c3,width=40)
                        filldata()
                        electionidentry['values']=elist
                        electionidentry.place(x=320,y=70,height=25)
                        btback=Button(c3,text='Back',font='arial 15',fg='black',bg='silver',command=closescreen)
                        btback.place(x=450,y=170)
                        btdelete=Button(c3,text='Delete',font='arial 15',bg='silver',fg='black',command=deletedata)
                        btdelete.place(x=300,y=170)
        
        
                    c2=Canvas(c7,width=200,height=700,bg='teal')
                    c2.place(x=5,y=105)
                    lblelectionschedule=Label(c2,text='User',font='arial 30 underline',bg='black',fg='yellow')
                    lblelectionschedule.place(x=30,y=20)
                    btinsert=Button(c2,text='Insert Data',font='bold 20',bg='green',fg='white',command=showinsert)
                    btinsert.place(x=10,y=100)
                    btfind=Button(c2,text='Find Data',font='bold 20',bg='green',fg='white',command=showfind)
                    btfind.place(x=10,y=200)
                    btupdate=Button(c2,text='Update Data',font='bold 20',bg='green',fg='white',command=showupdate)
                    btupdate.place(x=10,y=300)
                    btdelete=Button(c2,text='Delete Data',font='bold 20',bg='green',fg='white',command=showdelete)
                    btdelete.place(x=10,y=400)
                    
                def votting():
                    def showinsert():
                        def savedata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            a=useridentry.get()
                            b=candidateidentry.get()
                            c=candidatenameentry.get()
                            d=wardnoentry.get()
                            if len(a)==0 or len(b)==0 or len(c)==0 or len(d)==0:
                                messagebox.showinfo('error','Blanks are not allowed')
                            else:
                                sql="select * from votting where userid='%s'"%(a)
                                cur.execute(sql)
                                result=cur.fetchone()
                                if result is not None:
                                    messagebox.showerror("status","User Id already exists")
                                    useridentry.delete(0,100)
                                    candidateidentry.delete(0,100)
                                    candidatenameentry.delete(0,100)
                                    wardnoentry.delete(0,100)
                                    
                                else:
                                    sql="insert into votting values('%s','%s','%s','%s')"%(a,b,c,d)
                                    cur.execute(sql)
                                    db.commit()
                                    data=cur.fetchall()
                                    
                                    messagebox.showinfo('vote','voted')
                                db.close()
                                
                        def closescreen():
                            c3.destroy()
                        def cleardata():
                            useridentry.delete(0,100)
                            candidateidentry.delete(0,100)
                            candidatenameentry.delete(0,100)
                            wardnoentry.delete(0,100)
                            
                        def checkdata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            a=useridentry.get()
                            if len(a)==0:
                                messagebox.showerror('Error','Plz Enter Userid')
                            else:
                                sql="select count(*) from votting where userid='%s'"%(a)
                                cur.execute(sql)
                                data=cur.fetchall()
                                x=0
                                for res in data:
                                    print(res)
                                    x=res[0]
                                if x==0:
                                    lblstatus.config(text='GO AHEAD',fg='green')
                                else:
                                    lblstatus.config(text='Already Available',fg='red')
                                
                        c3=Canvas(c7,width=1800,height=700,bg='orange')
                        c3.place(x=205,y=105)                        
                               
                        lblvotting=Label(c3,text='VOTTING(INSERT PAGE)',font='bold 20 underline',bg='black',fg='yellow')
                        lblvotting.place(x=220,y=10)
        
                        lbluserid=Label(c3,text='Userid',font='arial 15')
                        lbluserid.place(x=150,y=70)
                        useridentry=Entry(c3,width=40)
                        useridentry.place(x=300,y=70,height=25)
                        lblcandidateid=Label(c3,text='Candidateid',font='arial 15')
                        lblcandidateid.place(x=150,y=130)
                        candidateidentry=Entry(c3,width=40)
                        candidateidentry.place(x=300,y=130,height=25)
        
                        lblcandidatename=Label(c3,text='Candidate Name',font='arial 15')
                        lblcandidatename.place(x=150,y=190)
                        candidatenameentry=Entry(c3,width=40)
                        candidatenameentry.place(x=300,y=190,height=25)
                        lblwardno=Label(c3,text='Wardno',font='arial 15')
                        lblwardno.place(x=150,y=250)
                        wardnoentry=Entry(c3,width=40)
                        wardnoentry.place(x=300,y=250,height=25)
                        btcheck=Button(c3,text='Check',bg='silver',fg='black',font='arial 15',command=checkdata)
                        btcheck.place(x=550,y=70)
                        lblstatus=Label(c3,text='Status',font='arial 15')
                        lblstatus.place(x=650,y=70)
                        btsave=Button(c3,text='Vote',font='arial 15',bg='silver',fg='black',command=savedata)
                        btsave.place(x=300,y=350)
                        btclose=Button(c3,text='Close',font='arial 15',fg='black',bg='silver',command=closescreen)
                        btclose.place(x=400,y=350)
                        btclear=Button(c3,text='Clear',font='arial 15',bg='silver',fg='black',command=cleardata)
                        btclear.place(x=500,y=350)
                    def showfind():
                        elist=[]
                        def filldata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            sql="select userid from votting"
                            cur.execute(sql)
                            data=cur.fetchall()
                            for res in data:
                                elist.append(res[0])
                            db.close()
                                
                        def finddata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            a=useridentry.get()
                            sql="select candidateid,candidatename,wardno from votting where userid='%s'"%(a)
                            cur.execute(sql)
                            data=cur.fetchone()
                            if data==None:
                                messagebox.showinfo('status','Record not found')
                            else:
                                candidateidentry.insert(0,data[0])
                                candidatenameentry.insert(0,data[1])
                                wardnoentry.insert(0,data[2])
                                
                            db.close()
        
                        def closescreen():
                            c3.destroy()
                        def cleardata():
                            useridentry.delete(0,100)
                            candidateidentry.delete(0,100)
                            candidatenameentry.delete(0,100)
                            wardnoentry.delete(0,100)
                            
                        c3=Canvas(c7,width=1800,height=700,bg='orange')
                        c3.place(x=205,y=105)                        
                        lblvotting=Label(c3,text='VOTTING(FIND PAGE)',font='bold 20 underline',bg='black',fg='yellow')
                        lblvotting.place(x=220,y=10)
                        lbluserid=Label(c3,text='Userid',font='arial 15')
                        lbluserid.place(x=150,y=70)
                        useridentry=ttk.Combobox(c3,width=40)
                        filldata()
                        useridentry['values']=elist
                        useridentry.place(x=300,y=70,height=25)
                        lblcandidateid=Label(c3,text='Candidateid',font='arial 15')
                        lblcandidateid.place(x=150,y=130)
                        candidateidentry=Entry(c3,width=40)
                        candidateidentry.place(x=300,y=130,height=25)
    
                        lblcandidatename=Label(c3,text='Candidate Name',font='arial 15')
                        lblcandidatename.place(x=150,y=190)
                        candidatenameentry=Entry(c3,width=40)
                        candidatenameentry.place(x=300,y=190,height=25)
                        lblwardno=Label(c3,text='Wardno',font='arial 15')
                        lblwardno.place(x=150,y=250)
                        wardnoentry=Entry(c3,width=40)
                        wardnoentry.place(x=300,y=250,height=25)
                        btcheck=Button(c3,text='Find',bg='silver',fg='black',font='arial 15',command=finddata)
                        btcheck.place(x=580,y=70)
                        btback=Button(c3,text='Back',font='arial 15',fg='black',bg='silver',command=closescreen)
                        btback.place(x=300,y=350)
                        btclear=Button(c3,text='Clear',font='arial 15',bg='silver',fg='black',command=cleardata)
                        btclear.place(x=400,y=350)
                    
                    def showupdate():
                        elist=[]
                        def filldata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            sql="select userid from votting"
                            cur.execute(sql)
                            data=cur.fetchall()
                            for res in data:
                                elist.append(res[0])
                            db.close()
                                
                        def finddata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            a=useridentry.get()
                            sql="select candidateid,candidatename,wardno from votting where userid='%s'"%(a)
                            cur.execute(sql)
                            data=cur.fetchone()
                            if data==None:
                                messagebox.showinfo('status','Record not found')
                            else:
                                candidateidentry.insert(0,data[0])
                                candidatenameentry.insert(0,data[1])
                                wardnoentry.insert(0,data[2])
                                
                            db.close()
                            
                        def updatedata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            a=useridentry.get()
                            b=candidateidentry.get()
                            c=candidatenameentry.get()
                            d=wardnoentry.get()
                            sql="update votting set candidateid='%s',candidatename='%s',wardno='%s' where userid='%s'"%(b,c,d,a)
                            cur.execute(sql)
                            db.commit()
                            data=cur.fetchall()
                            messagebox.showinfo('update','Data Updated')
                            db.close()
                            useridentry.delete(0,100)
                            candidateidentry.delete(0,100)
                            candidatenameentry.delete(0,100)
                            wardnoentry.delete(0,100)
                            
        
                        def closescreen():
                            c3.destroy()
                        def cleardata():
                            useridentry.delete(0,100)
                            candidateidentry.delete(0,100)
                            candidatenameentry.delete(0,100)
                            wardnoentry.delete(0,100)
                            
                        c3=Canvas(c7,width=1800,height=700,bg='orange')
                        c3.place(x=205,y=105)                               
                        lblvotting=Label(c3,text='VOTTING(UPDATE PAGE)',font='bold 20 underline',bg='black',fg='yellow')
                        lblvotting.place(x=220,y=10)
        
                        lbluserid=Label(c3,text='Userid',font='arial 15')
                        lbluserid.place(x=150,y=70)
                        useridentry=ttk.Combobox(c3,width=40)
                        filldata()
                        useridentry['values']=elist
                        useridentry.place(x=300,y=70,height=25)
                        lblcandidateid=Label(c3,text='Candidateid',font='arial 15')
                        lblcandidateid.place(x=150,y=130)
                        candidateidentry=Entry(c3,width=40)
                        candidateidentry.place(x=300,y=130,height=25)
        
                        lblcandidatename=Label(c3,text='Candidate Name',font='arial 15')
                        lblcandidatename.place(x=150,y=190)
                        candidatenameentry=Entry(c3,width=40)
                        candidatenameentry.place(x=300,y=190,height=25)
                        lblwardno=Label(c3,text='Wardno',font='arial 15')
                        lblwardno.place(x=150,y=250)
                        wardnoentry=Entry(c3,width=40)
                        wardnoentry.place(x=300,y=250,height=25)
                        btcheck=Button(c3,text='Find',bg='silver',fg='black',font='arial 15',command=finddata)
                        btcheck.place(x=580,y=70)
                        btupdate=Button(c3,text='Update',font='arial 15',fg='black',bg='silver',command=updatedata)
                        btupdate.place(x=300,y=350)
                        btback=Button(c3,text='Back',font='arial 15',fg='black',bg='silver',command=closescreen)
                        btback.place(x=400,y=350)
                        btclear=Button(c3,text='Clear',font='arial 15',bg='silver',fg='black',command=cleardata)
                        btclear.place(x=500,y=350)
                    def showdelete():
                        elist=[]
                        def filldata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            sql="select userid from votting"
                            cur.execute(sql)
                            data=cur.fetchall()
                            for res in data:
                                elist.append(res[0])
                            db.close()
                                
                        def deletedata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            a=useridentry.get()
                            sql="delete from votting where userid='%s'"%(a)
                            cur.execute(sql)
                            db.commit()
                            data=cur.fetchall()
                            if data==None:
                                messagebox.showerror('Error','No Record')
                            else:
                                messagebox.showinfo('delete','Deleted')
                            db.close()
                            useridentry.delete(0,100)
                            
                        def closescreen():
                            c3.destroy()
                            
                        c3=Canvas(c7,width=1800,height=700,bg='orange')
                        c3.place(x=205,y=105)          
                        lblelectionschedule=Label(c3,text='VOTTING(DELETE PAGE)',font='bold 20 underline',bg='black',fg='yellow')
                        lblelectionschedule.place(x=200,y=10)
                        lblelectionid=Label(c3,text='userid',font='arial 15')
                        lblelectionid.place(x=150,y=70)
                        electionidentry=ttk.Combobox(c3,width=40)
                        filldata()
                        electionidentry['values']=elist
                        electionidentry.place(x=320,y=70,height=25)
                        btback=Button(c3,text='Back',font='arial 15',fg='black',bg='silver',command=closescreen)
                        btback.place(x=450,y=170)
                        btdelete=Button(c3,text='Delete',font='arial 15',bg='silver',fg='black',command=deletedata)
                        btdelete.place(x=300,y=170)
        
        
                    c2=Canvas(c7,width=200,height=700,bg='teal')
                    c2.place(x=5,y=105)
                    lblelectionschedule=Label(c2,text='Votting',font='arial 30 underline',bg='black',fg='yellow')
                    lblelectionschedule.place(x=30,y=20)
                    btinsert=Button(c2,text='Insert Data',font='bold 20',bg='green',fg='white',command=showinsert)
                    btinsert.place(x=10,y=100)
                    btfind=Button(c2,text='Find Data',font='bold 20',bg='green',fg='white',command=showfind)
                    btfind.place(x=10,y=200)
                    btupdate=Button(c2,text='Update Data',font='bold 20',bg='green',fg='white',command=showupdate)
                    btupdate.place(x=10,y=300)
                    btdelete=Button(c2,text='Delete Data',font='bold 20',bg='green',fg='white',command=showdelete)
                    btdelete.place(x=10,y=400)
                def results():
                    elist=[]
                    def filldata():
                        db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                        cur=db.cursor()
                        sql="select candidateid from candidates"
                        cur.execute(sql)
                        data=cur.fetchall()
                        for res in data:
                            elist.append(res[0])
                        db.close()

                    def show():
                        db=pymysql.connect(host='localhost',password='root',user='root',database='polling')
                        cur=db.cursor()
                        a=candidateidentry.get()
                        
                        sql="select max(candidatename),count(candidateid),wardno from votting where candidateid='%s' group by wardno" %(a)
                        cur.execute(sql)
                        data=cur.fetchone()
                        db.commit()

                        candidatenameentry.delete(0,100)
                        scoreentry.delete(0,100)
                        wardnoentry.delete(0,100)

                        candidatenameentry.insert(0,data[0])
                        scoreentry.insert(0,data[1])
                        wardnoentry.insert(0,data[2])
                        db.close()
                    #------------------------------------------------------------------------------------------------------------------------------------
                    
                    def checkgraph():
                        
                        a=Toplevel(t)
                        a.geometry("900x800+200+10")
                        a.title("Graph Portal")
                        cn=Canvas(a,width=900,height=900,bg='pink')
                        cn.place(x=0,y=0)
                        
                        cn1=Canvas(cn,width=150,height=900,bg='skyblue')
                        cn1.place(x=0,y=0)
                        
                        def candidategraph():
                            
                            cn2=Canvas(cn,width=750,height=900,bg='pink')
                            cn2.place(x=151,y=0)
                            
                            names=[]
                            score=[]
                            
                            def show():
                                
                                db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                                cur=db.cursor()
                                
                                
                                sql="select max(candidatename),count(candidateid) from votting group by candidateid order by candidateid;" 
                                cur.execute(sql)
                                data=cur.fetchall()
                                
                                
                                for res in data:
                                    names.append(res[0])
                                    score.append(res[1])
                                
                                db.close()

                                ax.bar(names,score)
                                canvas.draw()
                            
                            fig, ax = plt.subplots()
                            
                            
                            canvas=FigureCanvasTkAgg(fig,master=cn2)
                            canvas.get_tk_widget().place(x=70,y=80)
                            
                            showbt=Button(cn2,text='Show',bg='silver',fg='black',font=('times new roman',14),command=show)
                            showbt.place(x=250,y=450)
                            
                            
                            
                            
                        
                        
                        candidategraphbt=Button(cn1,text='Candidates Graph',bg='yellow',fg='black',font=('times new roman',14),command=candidategraph)
                        candidategraphbt.place(x=2,y=200)



                        # c=candidatenameentry.get()
                        # d=wardnoentry.get()

                        # sql=""%b
                        # cur.execute(sql)
                        # data=cur.fetchone()   
                        # x=data[0]
                        # y=data[1]
                        # p=data[2]
                        # candidatenameentry.insert(0,data[0])
                        # #wardnoentry.insert(0,data[1])
                        # scoreentry.insert(0,data[2])
                        # c.config(text=y)
                        # d.config(text=p)
                    cu=Canvas(c7,width=1800,height=700,bg='orange')
                    cu.place(x=205,y=105)
                    cu.create_line(0,400,2000,400,fill='black',width=10)
                               
                    
                    lblvotting=Label(cu,text='CANDIDATE INDIVIDUAL RESULT',font=('times new roman',20,'bold underline'))
                    lblvotting.place(x=220,y=10)

                    lblcandidateid=Label(cu,text='Candidateid',font=('times new roman',15))
                    lblcandidateid.place(x=50,y=120)

                    lblcandidatename=Label(cu,text='Candidate Name',font=('times new roman',15))
                    lblcandidatename.place(x=50,y=180)

                    lblwardno=Label(cu,text='Ward No',font=('times new roman',15))
                    lblwardno.place(x=50,y=240)

                    wardnoentry=Entry(cu,width=20,font=('time new roman',12),justify='center')
                    wardnoentry.place(x=400,y=240,height=25)

                    lblscore=Label(cu,text='Score',font=('times new roman',15))
                    lblscore.place(x=50,y=300)

                    candidateidentry=ttk.Combobox(cu,width=20,font=('time new roman',12),justify='center')
                    filldata()
                    candidateidentry['values']=elist

                    candidateidentry.place(x=400,y=120,height=25)

                    candidatenameentry=Entry(cu,width=20,font=('time new roman',12),justify='center')
                    candidatenameentry.place(x=400,y=180,height=25)

                    scoreentry=Entry(cu,width=20,font=('time new roman',12),justify='center')
                    scoreentry.place(x=400,y=300,height=25)

                    btresult=Button(cu,text='Show',bg='silver',fg='black',font=('times new roman',15),command=show)
                    btresult.place(x=200,y=350)
                    graphbt=Button(c7,text="CHECK GRAPH",fg="black",bg="yellow",font=('Bold',10),width=15,height=2,command=checkgraph)  
                    graphbt.place(x=800,y=650)
                    

                    
                    def winner():
                        db=pymysql.connect(host='localhost',password='root',user='root',database='polling')
                        cur=db.cursor() 
                        a=wardnoentry1.get()
                        
                        sql="select max(candidatename) from votting where wardno='%s' group by candidateid order by count(candidatename) desc" %(a)
                        cur.execute(sql)
                        data=cur.fetchone()
                        db.commit()
                        winnerentry.delete(0,100)
                        
                        winnerentry.insert(0,data[0])
                        db.close()

                    alist=[]
                    def fill():
                        db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                        cur=db.cursor()
                        sql="select wardno from candidates group by wardno"
                        cur.execute(sql)
                        data=cur.fetchall()
                        for res in data:
                            alist.append(res[0])
                        db.close()
                    def closescreen():
                        c7.destroy()
                        cu.destroy()
                    

                    lblwardno=Label(cu,text='Ward No',font=('times new roman',15))
                    lblwardno.place(x=50,y=450)

                    wardnoentry1=ttk.Combobox(cu,width=20,font=('time new roman',12))

                    fill()

                    wardnoentry1['values']=alist
                    wardnoentry1.place(x=250,y=450,height=25)

                    winnerbt=Button(cu,text='Winner',bg='silver',fg='black',font=('times new roman',15),command=winner)
                    winnerbt.place(x=250,y=500)

                    winnerentry=Entry(cu,width=25,font=('times new roman',20),justify='center')
                    winnerentry.place(x=112,y=550)
                    btback=Button(cu,text='Back',font=('times new roman',15),fg='black',bg='silver',command=closescreen)
                    btback.place(x=300,y=350)
                    
                    
                    
                c1=Canvas(c7,width=2000,height=100,bg='blue')
                c1.place(x=5,y=5)
                btadmin=Button(c1,text='Admin',bg='silver',font='bold 20',bd=10,command=admin)
                btadmin.place(x=20,y=20)
                btcandidates=Button(c1,text='Candidates',bg='silver',font='bold 20',bd=10,command=candidates)
                btcandidates.place(x=170,y=20)
                btelectionschedule=Button(c1,text='Election Schedule',bg='silver',font='bold 20',bd=10,command=electionschedule)
                btelectionschedule.place(x=370,y=20)
                btuser=Button(c1,text='User',bg='silver',font='bold 20',bd=10,command=user)
                btuser.place(x=670,y=20)
                btvoting=Button(c1,text='Voting',bg='silver',font='bold 20',bd=10,command=votting)
                btvoting.place(x=800,y=20)
                btresults=Button(c1,text='Results',bg='silver',font='bold 20',bd=10,command=results)
                btresults.place(x=950,y=20)
                def closescreen():
                    c7.destroy()
                btexit=Button(c7,text='Exit',bg='silver',command=closescreen)
                btexit.place(x=1100,y=20)
                c2=Canvas(c7,width=200,height=700,bg='teal')
                c2.place(x=5,y=105)
                c3=Canvas(c7,width=1800,height=700,bg='orange')
                c3.place(x=205,y=105)
                image=Image.open(r"C:\Users\Nabeel-IT\Pictures\vote.image.jpeg")
                photo=ImageTk.PhotoImage(image)    
                imglbl=Label(c3,image=photo)
                imglbl.place(x=0,y=180)
                
                image2=Image.open(r"C:\Users\Nabeel-IT\Pictures\vote.image1.jpeg")
                photo2=ImageTk.PhotoImage(image2)    
                imglbl1=Label(c3,image=photo2)
                imglbl1.place(x=660,y=180)
                
           
                #def time():
                    
                    #tim=strftime('%H:%M:%S %p')
                    #lab.config(text=tim)
                    #lab.after(1000,time)
        
                   
                #lab=Label(c3,font=('ds-digital',50),bg='black',fg='silver')
                #lab.place(x=730,y=500)
                #time()
        
                lblpolling=Label(c3,text='ADMIN SECTION',bg='orange',fg='black',font='bold 100 underline')
                lblpolling.place(x=50,y=10)
                t.mainloop()
                
    def adminregister():
        def adminsignup():
            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
            cur=db.cursor()
            a=adminidentry.get()
            b=nameentry.get()
            c=passwordentry.get()
            if len(a)==0 or len(b)==0 or len(c)==0:
                messagebox.showerror('Error','Blanks are not allowed')
            elif len(c)<8:
                messagebox.showerror('Error','Password must contain atleast 8 characters ')
            
            else:
                sql="insert into admin values('%s','%s','%s')"%(a,b,c)
                cur.execute(sql)
                db.commit()
                data=cur.fetchall()
                messagebox.showinfo('save','Data Saved')
            db.close()
            
        def checkdata():
            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
            cur=db.cursor()
            a=adminidentry.get()
            if len(a)==0:
                messagebox.showerror('Error','Plz Enter Adminid')
            else:
                sql="select count(*) from admin where adminid='%s'"%(a)
                cur.execute(sql)
                data=cur.fetchall()
                x=0
                for res in data:
                    print(res)
                    x=res[0]
                if x==0:
                    lblstatus.config(text='GO AHEAD',fg='green')
                else:
                    lblstatus.config(text='Already Available',fg='red')
        def closescreen():
            c5.destroy()
        def cleardata():
            adminidentry.delete(0,100)
            nameentry.delete(0,100)
            passwordentry.delete(0,100)
            
        c5=Canvas(t,width=2000,height=700,bg='orange')
        c5.place(x=0,y=100)
        lbladmin=Label(c5,text='NEW ADMIN(REGISTRATION)',font='arial 20 bold underline',bg='black',fg='yellow')
        lbladmin.place(x=270,y=20)
        lbladminid=Label(c5,text='Adminid',font='arial 15 bold',bg='tan',fg='black')
        lbladminid.place(x=150,y=100)
        adminidentry=Entry(c5,width='40')
        adminidentry.place(x=300,y=100,height=25)
        lblname=Label(c5,text='Name',font='arial 15 bold',bg='tan',fg='black')
        lblname.place(x=150,y=170)
        nameentry=Entry(c5,width=40)
        nameentry.place(x=300,y=170,height=25)
        lblpassword=Label(c5,text='Password',font='arial 15 bold',bg='tan',fg='black')
        lblpassword.place(x=150,y=240)
        def show():
            hide_button=Button(c5,image=hide_image,command=hide,relief=FLAT,activebackground='white',background='white')
            hide_button.place(x=550,y=240)
            passwordentry.config(show='')
        def hide():
            show_button=Button(c5,image=show_image,command=show,relief=FLAT,activebackground='white',background='white')
            show_button.place(x=550,y=240)
            passwordentry.config(show='*')
        show_image=ImageTk.PhotoImage(file=r"C:\Users\Nabeel-IT\Pictures\show.png")
        hide_image=ImageTk.PhotoImage(file=r"C:\Users\Nabeel-IT\Pictures\hide.png")
        show_button=Button(c5,image=show_image,command=show,relief=FLAT,activebackground='white',bd=0,background='white')
        show_button.place(x=550,y=240)
        passwordentry=Entry(c5,width=40,show='*')
        passwordentry.place(x=300,y=240,height=25)
        btsave=Button(c5,text='Register',font='arial 15',bg='teal',fg='white',command=adminsignup)
        btsave.place(x=300,y=340)
        btclear=Button(c5,text='Clear',font='arial 15',bg='teal',fg='white',command=cleardata)
        btclear.place(x=450,y=340)
        btcheck=Button(c5,text='Check',font='arial 15',bg='teal',fg='white',command=checkdata)
        btcheck.place(x=570,y=100)
        lblstatus=Label(c5,text='Status',font='arial 20')
        lblstatus.place(x=700,y=100)
        btback=Button(c5,text='Back',font='arial 15',bg='teal',fg='white',command=closescreen)
        btback.place(x=600,y=340)

    c5=Canvas(t,width=2000,height=700,bg='orange')
    c5.place(x=0,y=100)
    logframe=Frame(c5,width=700,height=500,highlightbackground='black',highlightthickness=15)
    logframe.place(x=270,y=50)
    lbladminlogin=Label(logframe,text='Admin Login',font='arial 50 bold underline')
    lbladminlogin.place(x=100,y=20)
    lbladminid=Label(logframe,text='Adminid',font='arial 20 bold')
    lbladminid.place(x=50,y=150)
    adminidentry=Entry(logframe,width=20,font='arial 17 bold',bd=10)
    adminidentry.place(x=200,y=150,height=50)
    lblpassword=Label(logframe,text='Password',font='arial 20 bold')
    lblpassword.place(x=50,y=220)
    def show():
        hide_button=Button(logframe,image=hide_image,command=hide,relief=FLAT,activebackground='white',bd=6,background='white')
        hide_button.place(x=500,y=220)
        passwordentry.config(show='')
    def hide():
        show_button=Button(logframe,image=show_image,command=show,relief=FLAT,activebackground='white',bd=6,background='white')
        show_button.place(x=500,y=220)
        passwordentry.config(show='*')
    show_image=ImageTk.PhotoImage(file=r"C:\Users\Nabeel-IT\Pictures\show.png")
    hide_image=ImageTk.PhotoImage(file=r"C:\Users\Nabeel-IT\Pictures\hide.png")
    show_button=Button(logframe,image=show_image,command=show,relief=FLAT,activebackground='white',bd=6,background='white')
    show_button.place(x=500,y=220)
    
    passwordentry=Entry(logframe,width=20,show='*',font='arial 17 bold',bd=10)
    passwordentry.place(x=200,y=220,height=50)
    btlogin=Button(logframe,text='Login',font='arial  20',bg='teal',fg='white',bd=9,command=adminlogin)
    btlogin.place(x=200,y=320)
    def closescreen():
        c5.destroy()
    
    btback=Button(logframe,text='Back',font='arial  20',bg='teal',fg='white',bd=9,command=closescreen)
    btback.place(x=350,y=320)
    clickhere_label=Label(logframe,text='Click Here TO New Admin-',font=('arial',14))
    clickhere_label.place(x=100,y=420)
    
    register_button=Button(logframe,text='Sign Up',font=('arial',14),bg='teal',fg='white',command=adminregister)
    register_button.place(x=340,y=420)

#user login
def userloginscreen():
    def userlogin():
        userid = useridentry.get() # define the variable name
        password = passwordentry.get() # define the variable password
        if userid == '' or password == '':
            messagebox.showerror('error', 'All fields are required')
        else:
            try:
                db = pymysql.connect(host='localhost', user='root', password='root',database='polling')
                cur=db.cursor() # create a cursor object
            except:
                messagebox.showerror('Error', 'Connection is not established')
                return
            sql = 'use polling'
            cur.execute(sql) # use the cursor object to execute the SQL query
            sql = 'select * from user where userid=%s and password=%s'
            values = (userid, password)
            cur.execute(sql, values)
            result = cur.fetchone()
            if result == None:
                messagebox.showerror('error', 'Invalid user id & Password')
            else:
                messagebox.showinfo('WELCOME', 'Login Successful')
        
                c8=Canvas(t,width=2000,height=800,bg='skyblue')
                c8.place(x=0,y=0)
                #candidates detail   
                def candidates():
                    def showfind():
                        elist=[]
                        def filldata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            sql="select candidateid from candidates"
                            cur.execute(sql)
                            data=cur.fetchall()
                            for res in data:
                                elist.append(res[0])
                            db.close()
                                
                        def finddata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            a=candidateidentry.get()
                            sql="select name,address,city,phone,email,govtidtype,govtidnumber,wardno from candidates where candidateid='%s'"%(a)
                            cur.execute(sql)
                            data=cur.fetchone()
                            if data==None:
                                messagebox.showinfo('status','Record not found')
                            else:
                                nameentry.delete(0,100)
                                addressentry.delete(0,100)
                                cityentry.delete(0,100)
                                phoneentry.delete(0,100)
                                emailentry.delete(0,100)
                                govtidtypeentry.delete(0,100)
                                govtidnumberentry.delete(0,100)
                                wardnoentry.delete(0,100)
                                
                                nameentry.insert(0,data[0])
                                addressentry.insert(0,data[1])
                                cityentry.insert(0,data[2])
                                phoneentry.insert(0,data[3])
                                emailentry.insert(0,data[4])
                                govtidtypeentry.insert(0,data[5])
                                govtidnumberentry.insert(0,data[6])
                                wardnoentry.insert(0,data[7])
                            db.close()
                            
                        def closescreen():
                            c3.destroy()
                        def cleardata():
                            candidateidentry.delete(0,100)
                            nameentry.delete(0,100)
                            addressentry.delete(0,100)
                            cityentry.delete(0,100)
                            phoneentry.delete(0,100)
                            emailentry.delete(0,100)
                            govtidtypeentry.delete(0,100)
                            govtidnumberentry.delete(0,100)
                            wardnoentry.delete(0,100)
                            
                        c3=Canvas(c8,width=1800,height=700,bg='orange')
                        c3.place(x=205,y=105)
                        lblcandidates=Label(c3,text='CANDIDATES(Find PAGE)',font='bold 20 underline',bg='black',fg='yellow')
                        lblcandidates.place(x=220,y=10)
                        lblcandidateid=Label(c3,text='Candidateid',font='arial 15',bg='tan',fg='black')
                        lblcandidateid.place(x=150,y=70)
                        candidateidentry=ttk.Combobox(c3,width=40)
                        filldata()
                        candidateidentry['values']=elist
                        candidateidentry.place(x=300,y=70,height=25)
                        lblname=Label(c3,text='Name',font='arial 15',bg='tan',fg='black')
                        lblname.place(x=150,y=130)
                        nameentry=Entry(c3,width=40)
                        nameentry.place(x=300,y=130,height=25)
                        lbladdress=Label(c3,text='Address',font='arial 15',bg='tan',fg='black')
                        lbladdress.place(x=150,y=190)
                        addressentry=Entry(c3,width=40)
                        addressentry.place(x=300,y=190,height=25)
                        lblcity=Label(c3,text='City',font='arial 15',bg='tan',fg='black')
                        lblcity.place(x=150,y=250)
                        cityentry=Entry(c3,width=40)
                        cityentry.place(x=300,y=250,height=25)
                        lblphone=Label(c3,text='Phone',font='arial 15',bg='tan',fg='black')
                        lblphone.place(x=150,y=310,height=25)
                        phoneentry=Entry(c3,width=40)
                        phoneentry.place(x=300,y=310,height=25)
                        lblemail=Label(c3,text='Email',font='arial 15',bg='tan',fg='black')
                        lblemail.place(x=650,y=130)
                        emailentry=Entry(c3,width=40)
                        emailentry.place(x=780,y=130,height=25)
                        lblgovtidtype=Label(c3,text='Govtidtype',font='arial 15',bg='tan',fg='black')
                        lblgovtidtype.place(x=650,y=190)
                        govtidtypeentry=ttk.Combobox(c3,width=40)
                        govtidtypeentry['values']=['Adhar Card','Pan Card','Voter Card','Driving Lisence']
                        govtidtypeentry.place(x=780,y=190,height=25)
                        lblgovtidnumber=Label(c3,text='Govtidnumber',font='arial 15',bg='tan',fg='black')
                        lblgovtidnumber.place(x=650,y=250)
                        govtidnumberentry=Entry(c3,width=40)
                        govtidnumberentry.place(x=780,y=250,height=25)
                        lblwardno=Label(c3,text='Wardno',font='arial 15',bg='tan',fg='black')
                        lblwardno.place(x=650,y=310)
                        wardnoentry=Entry(c3,width=40)
                        wardnoentry.place(x=780,y=310,height=25)
                        btfind=Button(c3,text='Find',bg='silver',fg='black',font='arial 15',command=finddata)
                        btfind.place(x=570,y=70)
                        btback=Button(c3,text='Back',font='arial 15',fg='black',bg='silver',command=closescreen)
                        btback.place(x=300,y=410)
                        btclear=Button(c3,text='Clear',font='arial 15',bg='silver',fg='black',command=cleardata)
                        btclear.place(x=400,y=410)
                
                    c2=Canvas(c8,width=200,height=700,bg='teal')
                    c2.place(x=5,y=105)
                    lblcandidates=Label(c2,text='Candidates',font='arial 20 underline',bg='black',fg='yellow')
                    lblcandidates.place(x=30,y=20)
                    btfind=Button(c2,text='Find Data',font='bold 20',bg='green',fg='white',command=showfind)
                    btfind.place(x=10,y=200)
                    
                def electionschedule():
                    def showfind():
                        elist=[]
                        def filldata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            sql="select electionid from electionschedule"
                            cur.execute(sql)
                            data=cur.fetchall()
                            for res in data:
                                elist.append(res[0])
                            db.close()
                                
                        def finddata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            a=electionidentry.get()
                            sql="select dateofelection,candidateid,timefrom,timeto from electionschedule where electionid='%s'"%(a)
                            cur.execute(sql)
                            data=cur.fetchone()
                            if data==None:
                                messagebox.showinfo('status','Record not found')
                            else:
                                dateofelectionentry.delete(0,100)
                                candidateidentry.delete(0,100)
                                timefromentry.delete(0,100)
                                timetoentry.delete(0,100)
                                dateofelectionentry.insert(0,data[0])
                                candidateidentry.insert(0,data[1])
                                timefromentry.insert(0,data[2])
                                timetoentry.insert(0,data[3])
                            db.close()
                            
                        def closescreen():
                            c3.destroy()
                        def cleardata():
                            electionidentry.delete(0,100)
                            dateofelectionentry.delete(0,100)
                            candidateidentry.delete(0,100)
                            timefromentry.delete(0,100)
                            timetoentry.delete(0,100)
                            
                        c3=Canvas(c8,width=1800,height=700,bg='orange')
                        c3.place(x=205,y=105)        
                        lblelectionschedule=Label(c3,text='ELECTIONSCHEDULE(FIND PAGE)',font='bold 20 underline',bg='black',fg='yellow')
                        lblelectionschedule.place(x=200,y=10)
                        lblelectionid=Label(c3,text='Electionid',font='arial 15')
                        lblelectionid.place(x=150,y=70)
                        electionidentry=ttk.Combobox(c3,width=40)
                        filldata()
                        electionidentry['values']=elist
                        electionidentry.place(x=320,y=70,height=25)
                        lbldateofelection=Label(c3,text='Date Of Election',font='arial 15')
                        lbldateofelection.place(x=150,y=130)
                        dateofelectionentry=Entry(c3,width=40)
                        dateofelectionentry.place(x=320,y=130,height=25)
                        lblcandidateid=Label(c3,text='Candidateid',font='arial 15')
                        lblcandidateid.place(x=150,y=190)
                        candidateidentry=Entry(c3,width=40)
                        candidateidentry.place(x=320,y=190,height=25)
                        lbltimefrom=Label(c3,text='Time From',font='arial 15')
                        lbltimefrom.place(x=150,y=250)
                        timefromentry=Entry(c3,width=40)
                        timefromentry.place(x=320,y=250,height=25)
                        lbltimeto=Label(c3,text='Time To',font='arial 15')
                        lbltimeto.place(x=150,y=310,height=25)
                        timetoentry=Entry(c3,width=40)
                        timetoentry.place(x=320,y=310,height=25)
                        btfind=Button(c3,text='Find',bg='silver',fg='black',font='arial 15',command=finddata)
                        btfind.place(x=590,y=70)
                        btback=Button(c3,text='Back',font='arial 15',fg='black',bg='silver',command=closescreen)
                        btback.place(x=300,y=410)
                        btclear=Button(c3,text='Clear',font='arial 15',bg='silver',fg='black',command=cleardata)
                        btclear.place(x=400,y=410)
                    
                    c2=Canvas(c8,width=200,height=700,bg='teal')
                    c2.place(x=5,y=105)
                    lblelectionschedule=Label(c2,text='Election Schedule',font='arial 17 underline',bg='black',fg='yellow')
                    lblelectionschedule.place(x=10,y=20)
                    btfind=Button(c2,text='Find Data',font='bold 20',bg='green',fg='white',command=showfind)
                    btfind.place(x=10,y=200)
                    
                def user():
                    def showinsert():
                        def savedata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            a=useridentry.get()
                            b=unameentry.get()
                            c=passwordentry.get()
                            d=addressentry.get()
                            e=cityentry.get()
                            f=emailentry.get()
                            g=wardnoentry.get()
                            if len(a)==0 or len(b)==0 or len(c)==0 or len(d)==0 or len(e)==0 or len(f)==0 or len(g)==0:
                                messagebox.showerror('Error','Blanks are not allowed')
                            elif len(c)<8:
                                messagebox.showerror('Error','Password must contain atleast 8 characters ')
                            
                            else:
                                sql="select * from user where userid='%s'"%(a)
                                cur.execute(sql)
                                result=cur.fetchone()
                                if result is not None:
                                    messagebox.showerror("status","User Id already exists")
                                    useridentry.delete(0,100)
                                    unameentry.delete(0,100)
                                    passwordentry.delete(0,100)
                                    addressentry.delete(0,100)
                                    cityentry.delete(0,100)
                                    emailentry.delete(0,100)
                                    wardnoentry.delete(0,100)
                                else:
                                    sql="insert into user values('%s','%s','%s','%s','%s','%s','%s')"%(a,b,c,d,e,f,g)
                                    cur.execute(sql)
                                    db.commit()
                                    import smtplib
                                    from email.mime.multipart import MIMEMultipart
                                    from email.mime.text import MIMEText
                                
                                    from_address = "kumarrahul040699@gmail.com"
                                    to_address =f
                                
                                 # Create message container - the correct MIME type is multipart/alternative.
                                    msg = MIMEMultipart('alternative')
                                    msg['Subject'] = "You are registered"
                                    msg['From'] = from_address
                                    msg['To'] = to_address
                                
                                 # Create the message (HTML).
                                    text='userid='+str(useridentry.get())+'\n'+'uname='+str(unameentry.get())+'\n'+'password='+str(passwordentry.get())+'\n'+'address='+str(addressentry.get())+'\n'+'city='+str(cityentry.get())+'\n'+'email='+str(emailentry.get())+'\n'+'wardno'+str(wardnoentry.get())
                                    html = 'Hello'+'\n'+text
                                
                                
                                 # Record the MIME type - text/html.
                                    part1 = MIMEText(text, 'plain')
                                    part2 = MIMEText(html, 'html')
                                 # Attach parts into message container
                                    msg.attach(part1)
                                    msg.attach(part2)
                                 # Credentials
                                    username = 'kumarrahul040699@gmail.com'  
                                    password = 'uzkjgvfnwmibjalf'
                                
                                 # Sending the email
                                 ## note - this smtp config worked for me, I found it googling around, you may have to tweak the # (587) to get yours to work
                                    server = smtplib.SMTP('smtp.gmail.com', 587) 
                                    server.ehlo()
                                    server.starttls()
                                    server.login(username,password)  
                                    server.sendmail(from_address, to_address, msg.as_string())  
                                    server.quit()
                                    messagebox.showinfo('status','mail sent')
                                   
                                    import pyttsx3
                                    friend=pyttsx3.init()
                                    friend.say('you have registered successfully')
                                    friend.runAndWait()
                                    db.close()
                                    data=cur.fetchall()
                                    
                                    messagebox.showinfo('save','saved')
                                db.close()
                                #useridentry.delete(0,100)
                                #unameentry.delete(0,100)
                                #passwordentry.delete(0,100)
                                #addressentry.delete(0,100)
                                #cityentry.delete(0,100)
                                #emailentry.delete(0,100)
                                #wardnoentry.delete(0,100)
                            
                        def closescreen():
                            c3.destroy()
                        def cleardata():
                            useridentry.delete(0,100)
                            unameentry.delete(0,100)
                            passwordentry.delete(0,100)
                            addressentry.delete(0,100)
                            cityentry.delete(0,100)
                            emailentry.delete(0,100)
                            wardnoentry.delete(0,100)
                        def checkdata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            a=useridentry.get()
                            if len(a)==0:
                                messagebox.showerror('Error','Plz Enter Userid')
                            else:
                                sql="select count(*) from user where userid='%s'"%(a)
                                cur.execute(sql)
                                data=cur.fetchall()
                                x=0
                                for res in data:
                                    print(res)
                                    x=res[0]
                                if x==0:
                                    lblstatus.config(text='GO AHEAD',fg='green')
                                else:
                                    lblstatus.config(text='Already exist',fg='red')
                                
                        c3=Canvas(c8,width=1800,height=700,bg='orange')
                        c3.place(x=205,y=105)          
                        lblcandidates=Label(c3,text='USER(INSERT PAGE)',font='bold 20 underline',bg='black',fg='yellow')
                        lblcandidates.place(x=220,y=10)
                        lbluserid=Label(c3,text='userid',font='arial 15')
                        lbluserid.place(x=150,y=70)
                        useridentry=Entry(c3,width=40)
                        useridentry.place(x=300,y=70,height=25)
                        lbluname=Label(c3,text='UName',font='arial 15')
                        lbluname.place(x=150,y=130)
                        unameentry=Entry(c3,width=40)
                        unameentry.place(x=300,y=130,height=25)
                        lblpassword=Label(c3,text='Password',font='arial 15')
                        lblpassword.place(x=150,y=190)
                        def show():
                            hide_button=Button(c3,image=hide_image,command=hide,relief=FLAT,activebackground='white',background='white')
                            hide_button.place(x=550,y=190)
                            passwordentry.config(show='')
                        def hide():
                            show_button=Button(c3,image=show_image,command=show,relief=FLAT,activebackground='white',background='white')
                            show_button.place(x=550,y=190)
                            passwordentry.config(show='*')
                        show_image=ImageTk.PhotoImage(file=r"C:\Users\Nabeel-IT\Pictures\show.png")
                        hide_image=ImageTk.PhotoImage(file=r"C:\Users\Nabeel-IT\Pictures\hide.png")
                        show_button=Button(c3,image=show_image,command=show,relief=FLAT,activebackground='white',bd=0,background='white')
                        show_button.place(x=550,y=190)
                        passwordentry=Entry(c3,width=40,show='*')
                        passwordentry.place(x=300,y=190,height=25)
                        lbladdress=Label(c3,text='Address',font='arial 15')
                        lbladdress.place(x=150,y=250)
                        addressentry=Entry(c3,width=40)
                        addressentry.place(x=300,y=250,height=25)
                        lblcity=Label(c3,text='City',font='arial 15')
                        lblcity.place(x=150,y=310,height=25)
                        cityentry=Entry(c3,width=40)
                        cityentry.place(x=300,y=310,height=25)
                        lblemail=Label(c3,text='Email',font='arial 15')
                        lblemail.place(x=150,y=370)
                        emailentry=Entry(c3,width=40)
                        emailentry.place(x=300,y=370,height=25)
                        lblwardno=Label(c3,text='Wardno',font='arial 15')
                        lblwardno.place(x=150,y=430)
                        wardnoentry=Entry(c3,width=40)
                        wardnoentry.place(x=300,y=430,height=25)
                        btcheck=Button(c3,text='Check',bg='silver',fg='black',font='arial 15',command=checkdata)
                        btcheck.place(x=550,y=70)
                        lblstatus=Label(c3,text='Status',font='bold 20')
                        lblstatus.place(x=650,y=70)
                        btsave=Button(c3,text='Save',font='arial 15',bg='silver',fg='black',command=savedata)
                        btsave.place(x=300,y=500)
                        btback=Button(c3,text='Back',font='arial 15',fg='black',bg='silver',command=closescreen)
                        btback.place(x=400,y=500)
                        btclear=Button(c3,text='Clear',font='arial 15',bg='silver',fg='black',command=cleardata)
                        btclear.place(x=500,y=500)
                    def showfind():
                        elist=[]
                        def filldata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            sql="select userid from user"
                            cur.execute(sql)
                            data=cur.fetchall()
                            for res in data:
                                elist.append(res[0])
                            db.close()
                                
                        def finddata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            a=useridentry.get()
                            sql="select uname,password,address,city,email,wardno from user where userid='%s'"%(a)
                            cur.execute(sql)
                            data=cur.fetchone()
                            if data==None:
                                messagebox.showinfo('status','Record not found')
                            else:
                                unameentry.delete(0,100)
                                passwordentry.delete(0,100)
                                addressentry.delete(0,100)
                                cityentry.delete(0,100)
                                emailentry.delete(0,100)
                                wardnoentry.delete(0,100)
                                unameentry.insert(0,data[0])
                                passwordentry.insert(0,data[1])
                                addressentry.insert(0,data[2])
                                cityentry.insert(0,data[3])
                                emailentry.insert(0,data[4])
                                wardnoentry.insert(0,data[5])
                            db.close()
                            
                        def closescreen():
                            c3.destroy()
                        def cleardata():
                            useridentry.delete(0,100)
                            unameentry.delete(0,100)
                            passwordentry.delete(0,100)
                            addressentry.delete(0,100)
                            cityentry.delete(0,100)
                            emailentry.delete(0,100)
                            wardnoentry.delete(0,100)
                            
                        c3=Canvas(c8,width=1800,height=700,bg='orange')
                        c3.place(x=205,y=105)                 
                        lblcandidates=Label(c3,text='USER(FIND PAGE)',font='bold 20 underline',bg='black',fg='yellow')
                        lblcandidates.place(x=220,y=10)
                        lbluserid=Label(c3,text='userid',font='arial 15')
                        lbluserid.place(x=150,y=70)
                        useridentry=ttk.Combobox(c3,width=40)
                        filldata()
                        useridentry['values']=elist
                        useridentry.place(x=300,y=70,height=25)
                        lbluname=Label(c3,text='UName',font='arial 15')
                        lbluname.place(x=150,y=130)
                        unameentry=Entry(c3,width=40)
                        unameentry.place(x=300,y=130,height=25)
                        lblpassword=Label(c3,text='Password',font='arial 15')
                        lblpassword.place(x=150,y=190)
                        def show():
                            hide_button=Button(c3,image=hide_image,command=hide,relief=FLAT,activebackground='white',background='white')
                            hide_button.place(x=550,y=190)
                            passwordentry.config(show='')
                        def hide():
                            show_button=Button(c3,image=show_image,command=show,relief=FLAT,activebackground='white',background='white')
                            show_button.place(x=550,y=190)
                            passwordentry.config(show='*')
                        show_image=ImageTk.PhotoImage(file=r"C:\Users\Nabeel-IT\Pictures\show.png")
                        hide_image=ImageTk.PhotoImage(file=r"C:\Users\Nabeel-IT\Pictures\hide.png")
                        show_button=Button(c3,image=show_image,command=show,relief=FLAT,activebackground='white',bd=0,background='white')
                        show_button.place(x=550,y=190)
                        passwordentry=Entry(c3,width=40)
                        passwordentry.place(x=300,y=190,height=25)
                        lbladdress=Label(c3,text='Address',font='arial 15')
                        lbladdress.place(x=150,y=250)
                        addressentry=Entry(c3,width=40)
                        addressentry.place(x=300,y=250,height=25)
                        lblcity=Label(c3,text='City',font='arial 15')
                        lblcity.place(x=150,y=310,height=25)
                        cityentry=Entry(c3,width=40)
                        cityentry.place(x=300,y=310,height=25)
                        lblemail=Label(c3,text='Email',font='arial 15')
                        lblemail.place(x=150,y=370)
                        emailentry=Entry(c3,width=40)
                        emailentry.place(x=300,y=370,height=25)
                        lblwardno=Label(c3,text='Wardno',font='arial 15')
                        lblwardno.place(x=150,y=430)
                        wardnoentry=Entry(c3,width=40)
                        wardnoentry.place(x=300,y=430,height=25)
                        btfind=Button(c3,text='Find',bg='silver',fg='black',font='arial 15',command=finddata)
                        btfind.place(x=580,y=70)
                        btback=Button(c3,text='Back',font='arial 15',fg='black',bg='silver',command=closescreen)
                        btback.place(x=300,y=500)
                        btclear=Button(c3,text='Clear',font='arial 15',bg='silver',fg='black',command=cleardata)
                        btclear.place(x=400,y=500)
                    c2=Canvas(c8,width=200,height=700,bg='teal')
                    c2.place(x=5,y=105)
                    lblelectionschedule=Label(c2,text='User',font='arial 30 underline',bg='black',fg='yellow')
                    lblelectionschedule.place(x=30,y=20)
                    btinsert=Button(c2,text='Insert Data',font='bold 20',bg='green',fg='white',command=showinsert)
                    btinsert.place(x=10,y=100)
                    btfind=Button(c2,text='Find Data',font='bold 20',bg='green',fg='white',command=showfind)
                    btfind.place(x=10,y=200)
                    
                def votting():
                    elist1=[]
                    def filldata1():
                        db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                        cur=db.cursor()
                        sql="select userid from user"
                        cur.execute(sql)
                        data=cur.fetchall()
                        for res in data:
                            elist1.append(res[0])
                        db.close()
                        
                    
                        
                    def showinsert():
                        elist2=[]
                        def  filldata2():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            sql="select candidateid from candidates"                   
                            cur.execute(sql)
                            data=cur.fetchall()
                            for res in data:
                                elist2.append(res[0])
                            
                            db.close()
                            
                        def finddata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            a=candidateidentry.get()
                            sql="select name,wardno from candidates  where candidateid='%s'"%(a)
                            cur.execute(sql)
                            data=cur.fetchone()
                            if data==None:
                                messagebox.showinfo('status','Record not found')
                            else:
                                candidatenameentry.delete(0,100)
                                wardnoentry.delete(0,100)
                                candidatenameentry.insert(0,data[0])
                                wardnoentry.insert(0,data[1])
                            db.close()
                    
                        def savedata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            a=useridentry.get()
                            b=candidateidentry.get()
                            c=candidatenameentry.get()
                            d=wardnoentry.get()
                            if len(a)==0 or len(b)==0 or len(c)==0 or len(d)==0:
                                messagebox.showinfo('error','Blanks are not allowed')
                            else:
                                sql="select * from votting where userid='%s'"%(a)
                                cur.execute(sql)
                                result=cur.fetchone()
                                if result is not None:
                                    messagebox.showerror("status","This user vote already done")
                                    useridentry.delete(0,100)
                                    candidateidentry.delete(0,100)
                                    candidatenameentry.delete(0,100)
                                    wardnoentry.delete(0,100)
                                    
                                else:
                                    sql="insert into votting values('%s','%s','%s','%s')"%(a,b,c,d)
                                    cur.execute(sql)
                                    
                                    db.commit()
                                    data=cur.fetchall()
                                    
                                    messagebox.showinfo('vote','voted')
                                db.close()
                            
                                
                                
                        def closescreen():
                            c3.destroy()
                        def cleardata():
                            useridentry.delete(0,100)
                            candidateidentry.delete(0,100)
                            candidatenameentry.delete(0,100)
                            wardnoentry.delete(0,100)
                            
                        def checkdata():
                            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
                            cur=db.cursor()
                            a=useridentry.get()
                            if len(a)==0:
                                messagebox.showerror('Error','Plz Enter userid')
                            else:
                                sql="select count(*) from votting where userid='%s'"%(a)
                                cur.execute(sql)
                                data=cur.fetchall()
                                x=0
                                for res in data:
                                    print(res)
                                    x=res[0]
                                if x==0:
                                    lblstatus.config(text='GO AHEAD',fg='green')
                                else:
                                    lblstatus.config(text='Already Available',fg='red')
                                
                        c3=Canvas(c8,width=1800,height=700,bg='orange')
                        c3.place(x=205,y=105)                        
                               
                        lblvotting=Label(c3,text='VOTTING(INSERT PAGE)',font='bold 20 underline',bg='black',fg='yellow')
                        lblvotting.place(x=220,y=10)
        
                        lbluserid=Label(c3,text='Userid',font='arial 15',bg='tan')
                        lbluserid.place(x=150,y=70)
                        useridentry=ttk.Combobox(c3,width=40)
                        filldata1()
                        useridentry['values']=elist1
                        useridentry.place(x=300,y=70,height=25)
                        useridentry['state']='readonly'
                        useridentry.current(0)
                        
                        lblcandidateid=Label(c3,text='Candidateid',font='arial 15',bg='tan')
                        lblcandidateid.place(x=150,y=130)
                        candidateidentry=ttk.Combobox(c3,width=40)
                        filldata2()
                        candidateidentry['values']=elist2
                        
                        candidateidentry.place(x=300,y=130,height=25)
                        candidateidentry['state']='readonly'
                        
                        lblcandidatename=Label(c3,text='Candidate Name',font='arial 15',bg='tan')
                        lblcandidatename.place(x=150,y=190)
                        candidatenameentry=Entry(c3,width=40)
                        candidatenameentry.place(x=300,y=190,height=25)
                        lblwardno=Label(c3,text='Wardno',font='arial 15',bg='tan')
                        lblwardno.place(x=150,y=250)
                        wardnoentry=Entry(c3,width=40)
                        wardnoentry.place(x=300,y=250,height=25)
                        btcheck=Button(c3,text='Check',bg='silver',fg='black',font='arial 15',command=checkdata)
                        btcheck.place(x=570,y=70)
                        lblstatus=Label(c3,text='Status',font='arial 15')
                        lblstatus.place(x=650,y=70)
                        btsave=Button(c3,text='Vote',font='arial 15',bg='silver',fg='black',command=savedata)
                        btsave.place(x=300,y=350)
                        btclose=Button(c3,text='Close',font='arial 15',fg='black',bg='silver',command=closescreen)
                        btclose.place(x=400,y=350)
                        btclear=Button(c3,text='Clear',font='arial 15',bg='silver',fg='black',command=cleardata)
                        btclear.place(x=500,y=350)
                        btfind=Button(c3,text='Find',font='arial 15',bg='silver',fg='black',command=finddata)
                        btfind.place(x=570,y=130)
                    
                    c2=Canvas(c8,width=200,height=700,bg='teal')
                    c2.place(x=5,y=105)
                    lblelectionschedule=Label(c2,text='Votting',font='arial 30 underline',bg='black',fg='yellow')
                    lblelectionschedule.place(x=30,y=20)
                    btinsert=Button(c2,text='Vote Here',font='bold 20',bg='green',fg='white',command=showinsert)
                    btinsert.place(x=10,y=100)
                    
                c1=Canvas(c8,width=2000,height=100,bg='blue')
                c1.place(x=5,y=5)
                btcandidates=Button(c1,text='Candidates',bg='silver',font='bold 20',bd=10,command=candidates)
                btcandidates.place(x=170,y=20)
                btelectionschedule=Button(c1,text='Election Schedule',bg='silver',font='bold 20',bd=10,command=electionschedule)
                btelectionschedule.place(x=370,y=20)
                btuser=Button(c1,text='User',bg='silver',font='bold 20',bd=10,command=user)
                btuser.place(x=670,y=20)
                btvoting=Button(c1,text='Voting',bg='silver',font='bold 20',bd=10,command=votting)
                btvoting.place(x=800,y=20)
                def closescreen():
                    c8.destroy()
                btexit=Button(c8,text='Exit',bg='silver',command=closescreen)
                btexit.place(x=1100,y=20)
                
                c2=Canvas(c8,width=200,height=700,bg='teal')
                c2.place(x=5,y=105)
                c3=Canvas(c8,width=1800,height=700,bg='orange')
                c3.place(x=205,y=105)
                image=Image.open(r"C:\Users\Nabeel-IT\Pictures\vote.image.jpeg")
                photo=ImageTk.PhotoImage(image)    
                imglbl=Label(c3,image=photo)
                imglbl.place(x=0,y=180)

                
                image2=Image.open(r"C:\Users\Nabeel-IT\Pictures\vote.image1.jpeg")
                photo2=ImageTk.PhotoImage(image2)    
                imglbl1=Label(c3,image=photo2)
                imglbl1.place(x=660,y=180)
                
                lblpolling=Label(c3,text='USER SECTION',bg='orange',fg='black',font='bold 100 underline')
                lblpolling.place(x=50,y=10)
                

                t.mainloop()           
                #def time():
                    
                    #tim=strftime('%H:%M:%S %p')
                    #lab.config(text=tim)
                    #lab.after(1000,time)
        
                   
                #lab=Label(c3,font=('ds-digital',50),bg='black',fg='silver')
                #lab.place(x=730,y=500)
                #time()
        
                
    def userregister():
        def usersignup():
            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
            cur=db.cursor()
            a=useridentry.get()
            b=unameentry.get()
            c=passwordentry.get()
            d=addressentry.get()
            e=cityentry.get()
            f=emailentry.get()
            g=wardnoentry.get()
            if len(a)==0 or len(b)==0 or len(c)==0 or len(d)==0 or len(e)==0 or len(f)==0 or len(g)==0:
                messagebox.showerror('Error','Blanks are not allowed')
            elif len(c)<8:
                messagebox.showerror('Error','Password must contain atleast 8 characters ')
            
            else:
                sql="select * from user where userid='%s'"%(a)
                cur.execute(sql)
                result=cur.fetchone()
                if result is not None:
                    messagebox.showerror("status","User Id already exists")
                    useridentry.delete(0,100)
                    unameentry.delete(0,100)
                    passwordentry.delete(0,100)
                    addressentry.delete(0,100)
                    cityentry.delete(0,100)
                    emailentry.delete(0,100)
                    wardnoentry.delete(0,100)
                else:
                    sql="insert into user values('%s','%s','%s','%s','%s','%s','%s')"%(a,b,c,d,e,f,g)
                    cur.execute(sql)
                    db.commit()
                    import smtplib
                    from email.mime.multipart import MIMEMultipart
                    from email.mime.text import MIMEText
                
                    from_address = "kumarrahul040699@gmail.com"
                    to_address =f
                
                 # Create message container - the correct MIME type is multipart/alternative.
                    msg = MIMEMultipart('alternative')
                    msg['Subject'] = "You are registered"
                    msg['From'] = from_address
                    msg['To'] = to_address
                
                 # Create the message (HTML).
                    text='userid='+str(useridentry.get())+'\n'+'uname='+str(unameentry.get())+'\n'+'password='+str(passwordentry.get())+'\n'+'address='+str(addressentry.get())+'\n'+'city='+str(cityentry.get())+'\n'+'email='+str(emailentry.get())+'\n'+'wardno'+str(wardnoentry.get())
                    html = 'Hello'+'\n'+text
                
                
                 # Record the MIME type - text/html.
                    part1 = MIMEText(text, 'plain')
                    part2 = MIMEText(html, 'html')
                 # Attach parts into message container
                    msg.attach(part1)
                    msg.attach(part2)
                 # Credentials
                    username = 'kumarrahul040699@gmail.com'  
                    password = 'uzkjgvfnwmibjalf'
                
                 # Sending the email
                 ## note - this smtp config worked for me, I found it googling around, you may have to tweak the # (587) to get yours to work
                    server = smtplib.SMTP('smtp.gmail.com', 587) 
                    server.ehlo()
                    server.starttls()
                    server.login(username,password)  
                    server.sendmail(from_address, to_address, msg.as_string())  
                    server.quit()
                    messagebox.showinfo('mail','mail sent')
                   
                    messagebox.showinfo('status','You are registered')
                    import pyttsx3
                    friend=pyttsx3.init()
                    friend.say('you have registered successfully')
                    friend.runAndWait()
                    db.close()
                    data=cur.fetchall()
                    
                    messagebox.showinfo('save','saved')
                db.close()
                #useridentry.delete(0,100)
                #unameentry.delete(0,100)
                #passwordentry.delete(0,100)
                #addressentry.delete(0,100)
                #cityentry.delete(0,100)
                #emailentry.delete(0,100)
                #wardnoentry.delete(0,100)
            
        def closescreen():
            c6.destroy()
        def cleardata():
            useridentry.delete(0,100)
            unameentry.delete(0,100)
            passwordentry.delete(0,100)
            addressentry.delete(0,100)
            cityentry.delete(0,100)
            emailentry.delete(0,100)
            wardnoentry.delete(0,100)
        def checkdata():
            db=pymysql.connect(host='localhost',user='root',password='root',database='polling')
            cur=db.cursor()
            a=useridentry.get()
            if len(a)==0:
                messagebox.showerror('Error','Plz enter userid')
            else:
                sql="select count(*) from user where userid='%s'"%(a)
                cur.execute(sql)
                data=cur.fetchall()
                x=0
                for res in data:
                    print(res)
                    x=res[0]
                if x==0:
                    lblstatus.config(text='GO AHEAD',fg='green')
                else:
                    lblstatus.config(text='Already exist',fg='red')
            
        c6=Canvas(t,width=2000,height=800,bg='orange')
        c6.place(x=0,y=100)
        lblcandidates=Label(c6,text=' NEW USER(REGISTRATION)',font='arial 20 bold underline',bg='orange',fg='black')
        lblcandidates.place(x=420,y=10)
        lbluserid=Label(c6,text='USER ID',font='arial 18',bg='tan')
        lbluserid.place(x=180,y=70)
        useridentry=Entry(c6,width=40)
        useridentry.place(x=320,y=70,height=25)
        lbluname=Label(c6,text='U NAME',font='arial 18',bg='tan')
        lbluname.place(x=180,y=140)
        unameentry=Entry(c6,width=40)
        unameentry.place(x=320,y=140,height=25)
        lblpassword=Label(c6,text='PASSWORD',font='arial 18',bg='tan')
        lblpassword.place(x=620,y=140)
        def show():
            hide_button=Button(c6,image=hide_image,command=hide,relief=FLAT,activebackground='white',background='white')
            hide_button.place(x=1040,y=140)
            passwordentry.config(show='*')
        def hide():
            show_button=Button(c6,image=show_image,command=show,relief=FLAT,activebackground='white',background='white')
            show_button.place(x=1040,y=140)
            passwordentry.config(show='*')
        show_image=ImageTk.PhotoImage(file=r"C:\Users\Nabeel-IT\Pictures\show.png")
        hide_image=ImageTk.PhotoImage(file=r"C:\Users\Nabeel-IT\Pictures\hide.png")
        show_button=Button(c6,image=show_image,command=show,relief=FLAT,activebackground='white',bd=0,background='white')
        show_button.place(x=1040,y=140)
        passwordentry=Entry(c6,width=40)
        passwordentry.place(x=780,y=140,height=25)
        lbladdress=Label(c6,text='ADDRESS',font='arial 18',bg='tan')
        lbladdress.place(x=180,y=210)
        addressentry=Entry(c6,width=40)
        addressentry.place(x=320,y=210,height=25)
        lblcity=Label(c6,text='CITY',font='arial 18',bg='tan')
        lblcity.place(x=620,y=210,height=25)
        cityentry=Entry(c6,width=40)
        cityentry.place(x=780,y=210,height=25)
        lblemail=Label(c6,text='EMAIL',font='arial 15',bg='tan')
        lblemail.place(x=180,y=280)
        emailentry=Entry(c6,width=40)
        emailentry.place(x=320,y=280,height=25)
        lblwardno=Label(c6,text='WARD NO',font='arial 15',bg='tan')
        lblwardno.place(x=620,y=280)
        wardnoentry=Entry(c6,width=40)
        wardnoentry.place(x=780,y=280,height=25)
        btcheck=Button(c6,text='Check',bg='silver',fg='black',font='arial 15',command=checkdata)
        btcheck.place(x=580,y=70)
        lblstatus=Label(c6,text='Status',font='bold 20')
        lblstatus.place(x=680,y=70)
        signup_button=Button(c6,text='Register',font=('arial',15),fg='black',bg='silver',command=usersignup)
        signup_button.place(x=350,y=380)
        
        btback=Button(c6,text='Back',font='arial 15',fg='black',bg='silver',command=closescreen)
        btback.place(x=500,y=380)
        btclear=Button(c6,text='Clear',font='arial 15',bg='silver',fg='black',command=cleardata)
        btclear.place(x=600,y=380)
    

    c5=Canvas(t,width=2000,height=700,bg='orange')
    c5.place(x=0,y=100)
    logframe=Frame(c5,width=700,height=500,highlightbackground='black',highlightthickness=15)
    logframe.place(x=270,y=50)
    lbluserlogin=Label(logframe,text='User Login',font='arial 50 bold underline')
    lbluserlogin.place(x=100,y=20)
    lbluserid=Label(logframe,text='Userid',font='arial 20 bold')
    lbluserid.place(x=50,y=150)
    useridentry=Entry(logframe,width=20,font='arial 17 bold',bd=10,fg='green')
    useridentry.place(x=200,y=150,height=50)
    lblpassword=Label(logframe,text='Password',font='arial 20 bold')
    lblpassword.place(x=50,y=220)
    def show():
        hide_button=Button(logframe,image=hide_image,command=hide,relief=FLAT,activebackground='white',bd=6,background='white')
        hide_button.place(x=500,y=220)
        passwordentry.config(show='')
    def hide():
        show_button=Button(logframe,image=show_image,command=show,relief=FLAT,activebackground='white',bd=6,background='white')
        show_button.place(x=500,y=220)
        passwordentry.config(show='*')
    show_image=ImageTk.PhotoImage(file=r"C:\Users\Nabeel-IT\Pictures\show.png")
    hide_image=ImageTk.PhotoImage(file=r"C:\Users\Nabeel-IT\Pictures\hide.png")
    show_button=Button(logframe,image=show_image,command=show,relief=FLAT,activebackground='white',bd=6,background='white')
    show_button.place(x=500,y=220)
    
    passwordentry=Entry(logframe,width=20,show='*',font='arial 17 bold',bd=10,fg='green')
    passwordentry.place(x=200,y=220,height=50)
    
    btlogin=Button(logframe,text='Login',font='arial  20',bg='teal',fg='white',bd=9,command=userlogin)
    btlogin.place(x=200,y=320)
    def closescreen():
        c5.destroy()
    btback=Button(logframe,text='Back',font='arial  20',bg='teal',fg='white',bd=9,command=closescreen)
    btback.place(x=350,y=320)
    clickhere_label=Label(logframe,text='Click Here TO New User-',font='arial 14 bold',fg='green')
    clickhere_label.place(x=100,y=420)
    
    register_button=Button(logframe,text='Sign Up',font=('arial',14),bg='teal',command=userregister)
    register_button.place(x=330,y=420)

       

c5=Canvas(t,width=2000,height=700,bg='orange')
c5.place(x=0,y=100)
frame=Frame(c5,width=1367,height=605,highlightbackground='orange',highlightthickness=30)
frame.place(x=0,y=0)
image=Image.open(r"C:\Users\Nabeel-IT\Pictures\vote.image.jpeg")
photo=ImageTk.PhotoImage(image)    
imglbl=Label(frame,image=photo)
imglbl.place(x=0,y=150)

image2=Image.open(r"C:\Users\Nabeel-IT\Pictures\vote.image1.jpeg")
photo2=ImageTk.PhotoImage(image2)    
imglbl1=Label(frame,image=photo2)
imglbl1.place(x=663,y=150)
lblpolling=Label(frame,text='ELECTION POLLING ',bg='orange',fg='black',font='bold 100 underline')
lblpolling.place(x=0,y=0)

c4=Canvas(t,width=2000,height=100,bg='blue')
c4.place(x=0,y=0)          
def time():
    
    tim=strftime('%H:%M:%S %p')
    lab.config(text=tim)
    lab.after(1000,time)

   
lab=Label(c4,font=('ds-digital',50),fg='black',bg='blue')
lab.place(x=900,y=10)
time()

btadmin=Button(c4,text='ADMIN',font='arial 15',bd=9,command=adminloginscreen)
btadmin.place(x=50,y=35)
btuser=Button(c4,text='USER',font='arial 15',bd=9,command=userloginscreen)
btuser.place(x=200,y=35)
#btlogout=Button(c4,text='Log Out',font='arial 15')
#btlogout.place(x=300,y=35)


t.mainloop()
