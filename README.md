# Project
Main Branch.

from tkinter import *
from PIL import ImageTk, Image, ImageDraw
from datetime import *
from math import *
import os
import pymysql
from tkinter import messagebox


root = Tk()


class Login:
        def __init__(self, root):
            self.root = root
            self.root.title(" MGM SIM Login System")
            self.root.geometry("1372x760+0+0")
            self.root.config(bg='#021e2f')
            self.loginform()

            # ========= Background color ===============
        def loginform(self):
            left_lbl = Label(self.root, bg='#A100FF', bd=0)
            left_lbl.place(x=0, y=0, relheight=1, width=600)

            right_lbl = Label(self.root, bg='#A100FF', bd=10)
            right_lbl.place(x=600, y=0, relheight=1, relwidth=1)

            # #########################All_ variables ###########################

            self.Password_var = StringVar()
            self.confirm_pass_var = StringVar()
            self.username_var = StringVar()

            # ================= Frame =============================
            frame_input = Frame(self.root, bg='#c1c8e4')
            frame_input.place(x=410, y=100, height=500, width=800)

            label1 = Label(frame_input, text="Login Here", font=('impact', 32, "bold"), fg="#03a9f4", bg="#c1c8e4")
            label1.place(x=130, y=30)


            label2 = Label(frame_input, text="Email ID", font=("Goudy old style", 20, "bold"), fg='black',
                           bg='#c1c8e4')
            label2.place(x=131, y=150)

            self.email_txt = Entry(frame_input, font=("times new roman", 15, "bold"), bg='light gray')
            self.email_txt.place(x=135, y=190, width=320, height=35)


            label3 = Label(frame_input, text="Password", font=("Goudy old style", 20, "bold"),fg='black',
                            bg='#c1c8e4')
            label3.place(x=131, y=230)

            self.password = Entry(frame_input ,font=("times new roman", 15, "bold"), show="*", bg='light gray')
            self.password.place(x=135, y=275, width=320, height=35)


            btn1 = Button(frame_input, text="forgot password?", cursor='hand2', font=('calibri', 10, "bold"),
                          bg='#c1c8e4', fg='red', bd=0, command=self.Setpass, width=20, height=1)
            btn1.place(x=133, y=430)

            btn2 = Button(frame_input, text="Login", command=self.login, cursor="hand2", font=("times new roman", 15),
                          fg="white", bg="green", activebackground='green',
                          bd=2, width=15, height=1)
            btn2.place(x=133, y=350)

            btn3 = Button(frame_input,text="Not Registered?register here !", cursor="hand2",  font=("calibri", 10, 'bold'),
                          bg='#c1c8e4', fg="black", bd=0, command=self.Register, width=25, height=1 )

            btn3.place(x=300, y=430)




            # ========= clock ========
            self.lbl = Label(self.root, text="MGM OFFICAL TIME", font=("Book Antiqua", 22, "bold"),
                             compound=BOTTOM, fg="white", bg='#081923', bd=0)
            self.lbl.place(x=90, y=120, height=450, width=350)
            self.working()

        def clock_img(self, hr, min_, sec_):
            clock = Image.new("RGB", (400, 400), (8, 25, 35))
            draw = ImageDraw.Draw(clock)

            bg = Image.open("c.png")
            bg = bg.resize((300, 300), Image.ANTIALIAS)
            clock.paste(bg, (50, 50))

            origin = 200, 200
            draw.line((origin, 200 + 50 * sin(radians(hr)), 200 - 50 * cos(radians(hr))), fill='#DF005E', width=4)
            draw.line((origin, 200 + 80 * sin(radians(min_)), 200 - 80 * cos(radians(min_))), fill='white', width=3)
            draw.line((origin, 200 + 100 * sin(radians(sec_)), 200 - 100 * cos(radians(sec_))), fill='yellow', width=2)
            draw.ellipse((195, 195, 210, 210), fill='#1AD5D5')
            clock.save("clock_new.png")

        def working(self):
            h = datetime.now().time().hour
            m = datetime.now().time().minute
            s = datetime.now().time().second

            hr = (h / 12) * 360
            min_ = (m / 12) * 360
            sec_ = (s / 12) * 360

            self.clock_img(hr, min_, sec_)
            self.img = ImageTk.PhotoImage(file="clock_new.png")
            self.lbl.config(image=self.img)
            self.lbl.after(200, self.working)


        def Setpass(self):
            Frame_login7 = Frame(self.root, bg="#c1c8e4")
            Frame_login7.place(x=440, y=100, height=500, width=800)

            label1 = Label(Frame_login7, text="set your password Here", font=('impact', 32, 'bold'), fg="black", bg='#c1c8e4')
            label1.place(x=45, y=20)

            label2 = Label(Frame_login7, text="Username", font=(" old style", 20, "bold"), fg='red',
                           bg='#c1c8e4')
            label2.place(x=180, y=95)

            self.entry = Entry(Frame_login7, font=("times new roman", 15, "bold"),textvariable=self.username_var , bg='light gray')
            self.entry.place(x=150, y=145, width=270, height=35)

            label3 = Label(Frame_login7, text="Set New Password", font=(" old style", 20, "bold"), fg='red',
                           bg='#c1c8e4')
            label3.place(x=30, y=205)

            self.entry2 = Entry(Frame_login7, font=("times new roman", 15, "bold"),textvariable=self.Password_var, show="*", bg='light gray')
            self.entry2.place(x=30, y=255, width=270, height=35)

            label5 = Label(Frame_login7, text="Confirm New Password", font=(" old style", 20, "bold"), fg='red',
                           bg='#c1c8e4')
            label5.place(x=330, y=205)

            self.entry4 = Entry(Frame_login7, font=("times new roman", 15, "bold"),textvariable=self.confirm_pass_var, show="*", bg='light gray')
            self.entry4.place(x=350, y=255, width=270, height=35)

            btn2 = Button(Frame_login7, text="UPDATE PASSWORD", command=self.up_password, cursor="hand2", font=("times new roman", 15),
                          fg="white", bg="green", activebackground='green',
                          bd=2, width=25, height=1)
            btn2.place(x=133, y=350)

        def Register(self):
            Frame_login1 = Frame(self.root, bg="#c1c8e4")
            Frame_login1.place(x=440, y=100, height=500, width=800)

            label1 = Label(Frame_login1, text="Register Here", font=('impact', 32, 'bold'), fg="black", bg='#c1c8e4')
            label1.place(x=45, y=20)

            label2 = Label(Frame_login1, text="Username", font=(" old style", 20, "bold"), fg='red',
                           bg='#c1c8e4')
            label2.place(x=30, y=95)

            self.entry = Entry(Frame_login1, font=("times new roman", 15, "bold"), bg='light gray')
            self.entry.place(x=30, y=145, width=270, height=35)

            label3 = Label(Frame_login1, text="Password", font=(" old style", 20, "bold"), fg='red',
                           bg='#c1c8e4')
            label3.place(x=30, y=195)

            self.entry2 = Entry(Frame_login1, font=("times new roman", 15, "bold"), show="*", bg='light gray')
            self.entry2.place(x=30, y=245, width=270, height=35)

            label4 = Label(Frame_login1, text="Email-id", font=(" old style", 20, "bold"), fg='red',
                           bg='#c1c8e4')
            label4.place(x=330, y=95)

            self.entry3 = Entry(Frame_login1, font=("times new roman", 15, "bold"), bg='light gray')
            self.entry3.place(x=330, y=145, width=270, height=35)

            label5 = Label(Frame_login1, text="Confirm Password", font=(" old style", 20, "bold"), fg='red',
                           bg='#c1c8e4')
            label5.place(x=330, y=195)

            self.entry4 = Entry(Frame_login1, font=("times new roman", 15, "bold"), show="*", bg='light gray')
            self.entry4.place(x=330, y=245, width=270, height=35)

            self.var_chk=IntVar()
            chk = Checkbutton(Frame_login1, text="I Agree The Terms & Conditions",variable=self.var_chk, onvalue=1, offvalue=0, bg='#c1c8e4')
            chk.place(x=70, y=300)


            btn2 = Button(Frame_login1, text="Register", command=self.register,  cursor="hand2",
                          font=("times new roman", 15), fg="white",
                          bg="dark green", bd=0, width=15, height=1)

            btn2.place(x=90, y=340)

            btn3 = Button(Frame_login1, command=self.loginform, text="Already Registered?Login", cursor="hand2",
                          font=("calibri", 10), bg='#c1c8e4', fg="black", bd=0)
            btn3.place(x=110, y=390)



        def login(self):
            if self.email_txt.get() == "" or self.password.get() == "":
                messagebox.showerror("Error", "All fields are required to signin", parent=self.root)
            else:
                global uname
                uname = str(self.email_txt.get())
                paswd = str(self.password.get())

                try:
                    con = pymysql.connect(host='localhost', user='root', password='', database='signup')
                    cur = con.cursor()

                    cur.execute("select * from registration where emailid = %s and password = %s",
                                (self.email_txt.get(),
                                 self.password.get()))
                    row = cur.fetchone()


                    if row == None:
                        messagebox.showerror('Error', 'Invalid Username And Password', parent=self.root)
                        self.loginclear()
                        self.email_txt.focus()

                    elif row != None:
                        root.destroy()
                        os.system('python home.py')
                        con.close()


                except Exception:
                    messagebox.showerror('Error', "server down please try again later...!!!")



        def register(self):
            global z
            z = self.entry2.get()
            x = self.entry3.get()
            if self.entry.get() == "" or self.entry2.get() == "" or self.entry3.get() == "" or self.entry4.get() == "":
                messagebox.showerror("Error", "All Fields Are Required", parent=self.root)

            elif len(self.entry.get())<=3:
                messagebox.showerror("Error", "Usernmae must contain at list 4 characters", parent=self.root)

            elif self.entry.get():
                for char in self.entry.get():
                    if not (('A' <= char and char <= 'Z') or ('a' <= char and char <= 'z')):
                        messagebox.showerror("Error", "Name field must contain proper character")
                        break
                    elif len(self.entry3.get()) < 12:
                        messagebox.showerror("Error", "Enter Proper Email ID")
                        break
                    elif len(self.entry3.get()) >= 12 and x[-10:] != "@gmail.com":
                        messagebox.showerror("Error", "Enter Proper mail extension")
                        break

                    elif self.entry2.get() != self.entry4.get():
                        messagebox.showerror("Error", "Password and Confirm Password Should Be Same", parent=self.root)
                        break

                    elif len(self.entry2.get())<2:
                        messagebox.showerror("Error", "Pasword must be 2 characters long", parent=self.root)
                        break

                    elif self.var_chk.get() == 0:
                        messagebox.showerror("Error", "Please accept  our terms & condition", parent=self.root)
                        break
                    else:

                        try:

                            con = pymysql.connect(host="localhost", user="root", password="", database="signup")
                            cur = con.cursor()

                            cur.execute("select * from registration where emailid=%s", self.entry3.get())
                            row = cur.fetchone()

                            if row != None:
                                messagebox.showerror("Error", "User already Exist,Please try with another Email",
                                                     parent=self.root)
                                self.regclear()
                                self.entry.focus()
                                break
                            else:
                                cur.execute("insert into registration values(%s,%s,%s,%s)",
                                            (self.entry.get(), self.entry3.get(),self.entry2.get(), self.entry4.get()))
                                con.commit()
                                con.close()

                                messagebox.showinfo("Success", "Registration successful ", parent=self.root)
                                messagebox.showwarning("Warning", "Please dont share your username and password with anyone ! ")
                                self.regclear()
                                break

                        except Exception as es:
                            messagebox.showerror("Error", f"Error due to:{str(es)}", parent=self.root)


        def regclear(self):

            self.entry.delete(0, END)
            self.entry2.delete(0, END)
            self.entry3.delete(0, END)
            self.entry4.delete(0, END)
            self.var_chk.set('0')
            self.loginform()

        def loginclear(self):
            self.email_txt.delete(0, END)
            self.password.delete(0, END)



        def up_password(self):
            if self.entry.get() == "" or self.entry2.get() == "" or self.entry4.get() == "":
                messagebox.showerror("Error", "All fields are required to signin", parent=self.root)

            elif self.entry2.get() != self.entry4.get():
                messagebox.showerror("Error", "password and confirm must be same", parent=self.root)

            else:
                try:
                        con = pymysql.connect(host='localhost', user='root', password='', database='signup')
                        cur = con.cursor()
                        cur.execute("select * from registration where username=%s",
                                    (self.entry.get(),
                                     ))
                        row = cur.fetchone()

                        if row == None:
                            messagebox.showerror('Error', 'Invalid Username ', parent=self.root)
                            self.entry.focus()

                        else:
                            p = self.Password_var.get()
                            x=print(p.get())

                            # here variable get correct value but...... but not properly update in database.                            q = print( self.confirm_pass_var.get())
                            cur.execute('UPDATE registration SET password = %s and confirm_password = %s WHERE username=%s',
                                       (p.get(),
                                        self.confirm_pass_var.get(), self.username_var.get()))

                            con.commit()
                            con.close()
                            messagebox.showinfo("success", "Details are successfully Updated ")
                            self.loginform()

                except Exception:
                    messagebox.showerror("success", "Please try again later ")
                    self.loginform()


obj = Login(root)
root.mainloop()


