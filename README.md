# Check-for-unseen-mails
This code checks if there are any unseen emails in your account
'''This code prints 0 until it finds an unseen email
    If it finds an unseen email,it prints 1'''
import email
import imaplib
import ctypes
import getpass
mail = imaplib.IMAP4_SSL('imap.gmail.com',993)
unm = input("insert Email : ")
pwd = getpass.getpass("enter password : ")
mail.login(unm,pwd)
mail.select("INBOX")
def loop():
   mail.select("INBOX")
   n=0
   (retcode, messages) = mail.search(None, '(UNSEEN)')
   if retcode == 'OK':
     
      for num in messages[0].split() :
         n=n+1
         print (n)
         typ, data = mail.fetch(num,'(RFC822)')
         for response_part in data:
            if isinstance(response_part, tuple):
                original = email.message_from_string(response_part[1])
                print (original['From'])
                data = original['Subject']
                print (data)
                if data == 'eject':
                   ctypes.windll.WINMM.mciSendStringW(u"set cdaudio door open", None, 0, None)                  
                typ, data = mail.store(num,'+FLAGS','\\Seen')

   print (n)

if __name__ == '__main__':
      try:
       
        while True:
            loop()
      finally:
         print("thank you!!")
