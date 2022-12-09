# Courier-Management-System

from tkinter import *
from tkinter import messagebox as ms
from tkinter import ttk
import sqlite3
import random
from tkinter import Button
from PIL import ImageTk, Image 

#################################  CONNECTION AND DATABASE IMPLEMENTATION #########################################

with sqlite3.connect('cms.db') as db:
    c = db.cursor()
try:
    c.execute('CREATE TABLE IF NOT EXISTS user (username TEXT NOT NULL ,password TEX  NOT NULL,mobile TEX NOT NULL);')
except:
    pass
db.commit()
db.close()
  


class main:
    def __init__(self,master): 
  
        self.master = master
        
        self.username = StringVar()
        self.password = StringVar()
        self.n_username = StringVar()
        self.n_password = StringVar()
        self.n_reg=StringVar()
        self.n_mobile=StringVar()
        self.mobile11=StringVar()
        self.widgets()


    
    def login(self):
  
        with sqlite3.connect('cms.db') as db:
            c = db.cursor()


        
        find_user = ('SELECT * FROM user WHERE username = ? and password = ?')
        c.execute(find_user,[(self.username.get()),(self.password.get())])
        result = c.fetchall()
        
        if result:
            self.track()
        else:
            ms.showerror('Oops!','Username Not Found.')
            
    def new_user(self):
    
        
        with sqlite3.connect('cms.db') as db:
            c = db.cursor()
        if self.n_username.get()!=' ' and self.n_password.get()!=' ' and self.n_mobile.get()!=' ':
            find_user = ('SELECT * FROM user WHERE username = ?')
            c.execute(find_user,[(self.n_username.get())])        

            if c.fetchall():
                    ms.showerror('Error!','Username Taken Try a Diffrent One.')
            else:
                    insert = 'INSERT INTO user(username,password,mobile) VALUES(?,?,?)'
                    c.execute(insert,[(self.n_username.get()),(self.n_password.get()),(self.n_mobile.get())])
                    db.commit()

                    ms.showinfo('Success!','Account Created!')
                    self.log()
        else:
             ms.showerror('Error!','Please Enter the details.')
        
     
        
    def consignment(self):
        
       
        
        try:
            with sqlite3.connect('cms.db') as db:
                c = db.cursor()


            #Find user If there is any take proper action
            find_user = ('SELECT * FROM user WHERE mobile= ?')
            c.execute(find_user,[(self.mobile11.get())])
            result = c.fetchall()

            if result:
                    self.track()
                    self.crff.pack_forget()
                    self.head['text'] = self.username.get() + '\n Your Product Details'
                    self.consi.pack()
            else:
                    ms.showerror('Oops!','Mobile Number Not Found.')
        except:
            ms.showerror('Oops!','Mobile Number Not Found.')
                
    




    #######################################    DECLARING   FUCTIONS    ################################
   
    def track1(self):
        self.consi.pack_forget()
        self.head['text'] = self.username.get() + '\n Track your Product'
        self.crff.pack()
    def log(self):
        self.username.set('')
        self.password.set('')
        self.crf.pack_forget()
        self.head['text'] = 'Login'
        self.logf.pack()
        
    def cr(self):
        self.n_username.set('')
        self.n_password.set('')
        self.logf.pack_forget()
        self.head['text'] = 'Create Account'
        self.crf.pack()
        
    def track(self):
             self.logf.pack_forget()
             self.head['text'] = self.username.get() + '\n Track your Product'
            
             self.crff.pack()
           

  
    def widgets(self):
        self.head = Label(self.master,text = ' LOGIN ',font = ('aerial',40),bg="orange",fg="white",pady = 10)
        self.head.pack()
        
    
        self.logf = Frame(self.master,padx =10,pady = 10)



        ############## LOGO AND IMAGES ##################
        
        
        image2 = Image.open("C:/Users/91984/Desktop/CMS/templates/cmsfavicon.png")
        img2=image2.resize((300,100))
        test1 = ImageTk.PhotoImage(img2)
        label2 = Label(self.master,image=test1)
        label2.image = test1
        label2.place(x=50,y=50)


        image1 = Image.open("C:/Users/91984/Desktop/CMS/templates/log.png")
        test = ImageTk.PhotoImage(image1)
        label1 = Label(self.master,image=test)
        label1.image = test
        label1.place(x=50,y=250)
        
        self.logf.configure(background='lightblue')
        




        ########### LOGIN PAGE #########################



        Label(self.logf,text = 'Username: ',font = ('',15),pady=5,padx=5).grid(sticky = W)
        Entry(self.logf,textvariable = self.username,bd = 3,font = ('',15)).grid(row=0,column=1)
        Label(self.logf,text = 'Password: ',font = ('',15),pady=5,padx=5).grid(sticky = W)
        Entry(self.logf,textvariable = self.password,bd = 3,font = ('',15),show = '*').grid(row=1,column=1)
        Button(self.logf,text = ' Login ',background='lightgrey',bd = 2 ,font = ('',13),padx=6,pady=6,command=self.login).grid(row=8,column=0)
        Button(self.logf,text = ' New user ',background='lightgrey',bd = 2 ,font = ('',13),padx=6,pady=6,command=self.cr).grid(row=8,column=1)
       
        self.logf.pack()




        ####################### REGISTRATION PAGE ##########################


        
        self.crf = Frame(self.master,padx =10,pady = 10)
        Label(self.crf,text = 'Username: ',font = ('',15),pady=5,padx=5).grid(sticky = W)
        Entry(self.crf,textvariable = self.n_username,bd = 3,font = ('',15)).grid(row=0,column=1)
        
        Label(self.crf,text = 'Password: ',font = ('',15),pady=5,padx=5).grid(sticky = W)
        Entry(self.crf,textvariable = self.n_password,bd = 3,font = ('',15),show = '*').grid(row=1,column=1)
        
        Label(self.crf,text = 'Reg No.: ',font = ('',15),pady=5,padx=5).grid(sticky = W)
        Entry(self.crf,textvariable = self.n_reg,bd = 3,font = ('',15)).grid(row=2,column=1)
        Label(self.crf,text = 'Gender: ',font = ('',15),pady=5,padx=5).grid(sticky = W)
        var = IntVar()
        R1 = Radiobutton(self.crf, text="Male", variable=var, value=1).grid(sticky=W)
        
        R2 = Radiobutton(self.crf, text="Female", variable=var, value=2 ).grid(row=4,column=1)
        Label(self.crf,text = 'Mobile No.: ',font = ('',15),pady=5,padx=5).grid(sticky = W)
        Entry(self.crf,textvariable = self.n_mobile,bd = 3,font = ('',15)).grid(row=5,column=1)
        
        Label(self.crf,text = 'Email Id: ',font = ('',15),pady=5,padx=5).grid(sticky = W)
        Entry(self.crf,bd = 3,font = ('',15)).grid(row=6,column=1)
        
        
        
        Button(self.crf,text = 'Create Account',background='lightgrey',bd = 2,font = ('',13),padx=6,pady=6,command=self.new_user).grid(row=11,column=0)
        Button(self.crf,text = 'Go to Login',background='lightgrey',bd = 2,font = ('',13),padx=6,pady=6,command=self.log).grid(row=11,column=1)

    
        ############################ TRACKING DETAILS ##########################




        self.crff = Frame(self.master,padx =10,pady = 10)
        
        Label(self.crff,text = ' Consignment No: ',font = ('',15),pady=5,padx=5).grid(sticky = W)
        Entry(self.crff,bd = 3,font = ('',15)).grid(row=0,column=1)
        Label(self.crff,text = 'Mobile no:',font = ('',15),pady=5,padx=5).grid(sticky = W)
        Entry(self.crff,bd = 3,textvariable = self.mobile11,font = ('',15)).grid(row=1,column=1)
        Button(self.crff,text = 'Track',background='lightgrey',bd = 2,font = ('',13),padx=6,pady=6,command=self.consignment).grid(row=4,column=0)
        Button(self.crff,text="Exit",background='lightgrey',bd = 2,font = ('',13),padx=6,pady=6).grid(row=4,column=1)
        self.consi = Frame(self.master,padx =10,pady = 10)
        
        Label(self.consi,text = ' Product ID:',font = ('',15),pady=5,padx=5).grid(sticky = W)
        Label(self.consi,text =random.randint(565154,99994216) ,font = ('',13),pady=5,padx=5).grid(row=0,column=1)
        L = ['Bag','Colgate','Shoe','Laptop','Jeans','Mouse','Mac','Ipad','Pen and Books','Book','shirt']
        f=random.randint(0,10)
        Label(self.consi,text = 'Product name: ',font = ('',15),pady=5,padx=5).grid(sticky = W)
        Label(self.consi,text =L[f] ,font = ('',13),pady=5,padx=5).grid(row=1,column=1)
        Label(self.consi,text = 'Product Status: ',font = ('',15),pady=5,padx=5).grid(sticky = W)
        Label(self.consi,text ='Pending' ,font = ('',13),pady=5,padx=5).grid(row=2,column=1)
        Label(self.consi,font = ('',13), text = 'Thanks for Exploring!').grid(row = 4, column = 0)
       
        Label(self.consi, text = 'Comments:',font = ('',13)).grid(row = 5, column = 0, padx = 5, sticky = 'sw')
        Entry(self.consi,bd = 3,font = ('',15)).grid(row=5,column=1)

        Button(self.consi,text = 'Back',background='lightgrey',bd = 2,font = ('',13),padx=6,pady=6,command=self.track1).grid(row=6,column=0)
    
        
        
        
    ############################################################ Track Consignments ########################################    

if __name__ == '__main__':

    root = Tk()
    root.title('Track Consignment')
    root.geometry('800x450+300+300')
    main(root)

    root.mainloop()
