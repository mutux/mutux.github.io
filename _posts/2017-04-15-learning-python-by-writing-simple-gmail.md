---
layout: post
title: 'Learning Python: Write a Simple Gmail Client & Extract its Texts into a JSON
  File'
date: '2017-04-15T11:46:00.004-07:00'
author: mutux
tags:
modified_time: '2017-04-17T15:08:47.329-07:00'
blogger_id: tag:blogger.com,1999:blog-7424095106938843032.post-2626268989855427504
blogger_orig_url: http://mutux.blogspot.com/2017/04/learning-python-by-writing-simple-gmail.html
---

To learn Python with practice, I write the code of a Simple Gmail Client. I implemented the functions as below:
+ Receive Your Email from your Gmail account with SMTP protocol;
+ Send email with or without an attachment through you gmail account with IMAP protocol;
+ Extract the email content into plain texts;
+ Transform the extracted text list into a JSON format, and save it into a local JSON file.

It works fine on my machine!

# Code:
{% highlight python linenos %}
# ===================================================
# Send mail out through gmail server with
# Function:
#    send(sender,password,recipient,subject,message,afilename,afilepath)
# Send email through your own gmail account, attachment is supported.
# Notice: Make Sure to Have "Allow Less Secure Apps" On Your Gmail Account
#
# Fetch gmail with
# Function:
#   fetch(account,password,cnt,diskfile)
# Fetch email from gmail, extract texts, and serialize as a JSON file.
#
# Execute in command line as
#   Usage:
#    python gmail.py [send|fetch]
#
# ====================================================
import sys
import smtplib
import time
import imaplib
import email
import json
from email.MIMEMultipart import MIMEMultipart
from email.MIMEText import MIMEText
from email.MIMEBase import MIMEBase
from email import encoders

def send(sender,password,recipient,subject,message,afilename,afilepath):

 msg = MIMEMultipart()
 msg['From'] = sender
 msg['To'] = recipient
 msg['Subject'] = subject

 msg.attach(MIMEText(message,'plain'))

 # attachment
 if afilename != None:
  filename = afilename
  attachment = open(afilepath,'rb')

  part = MIMEBase('application','octet-stream')
  part.set_payload((attachment).read())
  encoders.encode_base64(part)
  part.add_header('Content-Disposition',"attachment; filename= %s" % afilename)

  msg.attach(part)

 server = smtplib.SMTP('smtp.gmail.com',587)
 server.starttls()
 server.login(sender,password)
 text = msg.as_string()
 server.sendmail(sender,recipient,text)
 server.quit()

def fetch(account,password,cnt,diskfile):
 #try:
  ext_mails = []
  mail = imaplib.IMAP4_SSL("imap.gmail.com")
  mail.login(account,password)
  mail.select('inbox')

  type,data = mail.search(None,'ALL')
  mail_ids = data[0]

  id_list = mail_ids.split()
  first_email_id = int(id_list[0])
  latest_email_id = int(id_list[-1])
  count = 0

  for i in range(latest_email_id,first_email_id,-1):
   count = count + 1
   if count > cnt:
    break
   ext_mail = {}
   type, data = mail.fetch(i,'(RFC822)')

   for response_part in data:
    if isinstance(response_part,tuple):
     msg = email.message_from_string(response_part[1])
     email_subject = msg['subject']
     ext_mail['subject'] = email_subject
     email_from = msg['from']
     ext_mail['from'] = email_from
     email_to = msg['to']
     ext_mail['to'] = email_to
     email_date = msg['date']
     ext_mail['date'] = email_date
     email_message = getText(msg)
     ext_mail['message'] = email_message
     ext_mails.append(ext_mail)

     print 'From : ' + email_from + '\n'
     print 'To : ' + email_to + '\n'
     print 'Subject: ' + email_subject + '\n'
     print 'Date: ' + email_date + '\n'
     print 'Message: \n' + email_message + '\n'
     print '-'*60 + '\n'
  # Write to disk as a json file
  with open(diskfile,'w+') as f:
   json.dump(ext_mails,f)
 #except:
 # print sys.exc_info()


def getText(msg):
 maintype = msg.get_content_maintype()
 if maintype == 'multipart':
  for part in msg.get_payload():
   parttype = part.get_content_maintype()
   #print parttype
   if parttype == 'multipart':
    # ------Recursively find the text part-------
    getText(part.get_payload())
   elif parttype == 'text':
    return part.get_payload()
 elif maintype == 'text':
  return msg.get_payload()

#  =================================
#  Usage:
#  python gmail.py [send|fetch]
#  =================================

if __name__ == "__main__":
 if len(sys.argv) > 1:
  if sys.argv[1] == "send":
   send('xxxx@gmail.com','xxxx','yyyy@example.com','Python Tutorial','To test is my python script works for sending email from this gmail account',None,None)
  elif sys.argv[1] == "fetch":
   fetch('xxxx@gmail.com','xxxx',3,'out/'+str(time.time()) + '.gmail.json')
  else:
   print "Usage: python gmail.py [send|fetch] \n"
 else:
  print "Usage: python gmail.py [send|fetch] \n"
{% endhighlight %}

Since it's a practice of python language learning, I have resort to some references, either about the language details or about the Gmail features. Here listed the resources that I have resort to as I am writing the code and composing this post! Enjoy!



# References:

+ https://docs.python.org/2/tutorial/
+ https://docs.python.org/2/tutorial/inputoutput.html
+ https://docs.python.org/2/tutorial/datastructures.html
+ http://naelshiab.com/tutorial-send-email-python/
+ http://stackoverflow.com/questions/10147455/how-to-send-an-email-with-gmail-as-provider-using-python
+ https://yuji.wordpress.com/2011/06/22/python-imaplib-imap-example-with-gmail/
+ http://codehandbook.org/how-to-read-email-from-gmail-using-python/
+ http://stackoverflow.com/questions/15075786/getting-emails-from-gmail-via-python
