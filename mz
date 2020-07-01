#!/usr/bin/env python3
#############################################################################
# mz - commandline package manager                                          #
# Copyright (C) 2019 Diego Sarzi <diegosarzi@gmail.com>                     #
#                                                                           #
# License: MIT                                                              #
# Permission is hereby granted, free of charge, to any person obtaining a   #
# copy of this software and associated documentation files (the "Software"),#
# to deal in the Software without restriction, including without limitation #
# the rights to use, copy, modify, merge, publish, distribute, sublicense,  #
# and/or sell copies of the Software, and to permit persons to whom the     #
# Software is furnished to do so, subject to the following conditions:      #
# .                                                                         #
# The above copyright notice and this permission notice shall be included   #
# in all copies or substantial portions of the Software.                    #
# .                                                                         #
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS   #
# OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF                #
# MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.    #
# IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY      #
# CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,      #
# TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE         #
# SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.                    #
#---------------------------------------------------------------------------#
#                                                                           #
# Copyright: 2019 Diego Sarzi <diegosarzi@gmail.com>                        #
#            2019 Vovolinux <suporte@viniciusalopes.com.br>                 #
#                                                                           #
#############################################################################

#------------------- VARIABLES ------------------------>

# Paths
url     = 'https://mazonos.com/packages/' # Official Repository
filecsv = '/var/lib/mz/mz_base.csv'      # Repository package list
dircsv  = '/var/lib/mz/'                 # Folder for the .csv file
dirlist = '/var/lib/banana/list/'        # Folder for the .list file

# Flags
found = False
done  = False

# Strings
choose            = ''
textAnimate       = ''
please_connect    = 'Please check your internet connection!'
package_not_found = 'Package not found.'

# Arrays
chooses = [
    's', 'search',
    'i', 'install',
    'r', 'remove',
    'w', 'show',
    'u', 'update',
    'g', 'upgrade',
    'c', 'checkdesc'
    ]

modules = ['requests', 'requests_html', 'bs4']

#------------------- END VARIABLES -------------------->


#------------------- IMPORTS -------------------------->

import sys, os, csv, threading, itertools, time, re

'''
Loop in modules:
    FUNCTION: Install python3 modules.
    DATE....: 01/07/2019
    CREDITS.: José Almeida <@joseafga>
              Rubens <@rubenss>
              Vovolinux <suporte@vovolinux.com.br>
'''

for module in modules:
    try:
        __import__(module)
    except Exception as e:
        print('Installing modules...')
        os.system('pip3 install ' + str(module))
        os.system('clear')
        
import requests, requests_html, bs4

from urllib.request import urlopen
from bs4 import BeautifulSoup
from requests_html import HTMLSession

#------------------- END IMPORTS ---------------------->



#------------------- FUNCTIONS ------------------------>

def check():
    # Checks if the folder exists
    if not os.path.isdir(dircsv):
        os.mkdir(dircsv)
        print('Created folder ' + dircsv + '.')

    # Checks if the file exists
    if not os.path.isfile(filecsv):
        os.system('touch ' + filecsv)
        print('Created file ' + filecsv + '.')
        os.system('clear')
        update()
        

def main():
    try:
        sys.argv[1]
    except IndexError:
        menu()
        exit(0)
    else:
        global choose
        choose = str(sys.argv[1])
        

def menu():
    print(''' mz 1.0.0.1 (amd64)
 'mz' is a commandline package manager and provides commands
 for searching and managing packages next to bananapkg.
 OPTIONS:
 s | search    - Search from package name.
 i | install   - Install package.
 r | remove    - Remove package.
 w | show      - Show description package.
 u | update    - Update list packages in repositore online. Need Internet.
 g | upgrade   - Automatic updating of system base and bug fixes.
 c | checkdesc - Search for packages without description.
 
 Usage: mz [option] package
''')


def internet_on():
    try:
        response = urlopen('https://www.google.com/', timeout=10)
        return True
    except:
        return False


def animate():

    for c in itertools.cycle(['|', '/', '-', '\\']):
        if done:
            break

        sys.stdout.write('\r' + textAnimate + c)
        sys.stdout.flush()
        time.sleep(0.1)

    sys.stdout.write('\r' + textAnimate + 'complete!   \n')


##------------------ CHOOSE-FUNCTIONS ----------------->

def s():
    search()
        
def search():
    try:
        sys.argv[2]
    except IndexError:
        menu()
        exit(0)
    else:
        global found
        package = str(sys.argv[2])

        ### OPEN CSV
        with open(filecsv, 'r') as csvfile:
            csv_reader = csv.reader(csvfile)

            count = 0
            for line in csv_reader:
                if package in line[1]:
                    count += 1
                    print(line[1].replace('.mz', ''))
                    found = True
            print(str(count) + ' package(s) found.')

            if not found:
                print(package_not_found)
                
def i():
    install()
                
def install():
    if internet_on():
        try:
            sys.argv[2]
        except IndexError:
            menu()
            exit(0)
        else:
            global found
            package = str(sys.argv[2])
            links = []
            packages = []
            ### OPEN CSV
            with open(filecsv, 'r') as csvfile:
                csv_reader = csv.reader(csvfile)

                for line in csv_reader:
                    if package in line[1]:
                       found = True
                       links.append(url + line[0] + line[1])         # Links for download
                       packages.append(line[1])   # Package names

                if found:
                    pkgcount = packages.__len__()
                    pkglist = ''

                    if pkgcount == 1:
                        install = input('Do you like install ' + packages[0].replace('.mz', '') + ' ? [Y/n] : ')
                        if install == 'Y' or install == 'y':
                            os.system('wget -O /tmp/' + packages[0] + ' ' + links[0])
                            os.system('banana install ' + '/tmp/' + packages[0])
                            os.system('rm ' + '/tmp/' + packages[0])
                        else:
                            exit(0)
                    else:
                        # Make pkglist
                        if pkgcount == 2:
                            pkglist = "'" + packages[0].replace('.mz', '') + "' and '" + packages[1].replace('.mz', '') + "'."
                        else:
                            for p in packages:
                                 pkglist += (p + ', ')

                            pkglist = pkglist[:-2] + '.'

                        print(str(pkgcount) + ' packages found: ' + pkglist.replace('.mz', ''))

                        install = input('Do you like install ALL packages ? [Y/n] : ')
                        if install == 'Y' or install == 'y':
                            for p in range(pkgcount):
                                os.system('wget -O /tmp/' + packages[p] + ' ' + links[p])
                                os.system('banana install ' +  '/tmp/' + packages[p])
                                os.system('rm ' + '/tmp/' + packages[p])
                            else:
                                exit(0)

                else: # if not found
                    print(package_not_found)

    else:
        print(please_connect)
        exit(0)

def r():
    remove()

def remove():
    try:
        sys.argv[2]
    except IndexError:
        menu()
        exit(0)
    else:
        global found
        onlyfiles = [f for f in os.listdir(dirlist) if os.path.isfile(os.path.join(dirlist, f))]
        r = re.compile(sys.argv[2] + '.*')
        newlist = list(filter(r.match, onlyfiles))

        if newlist:
            found = True
            for pack in newlist:
                package = pack.replace('.list', '')
                remove  = input('You like remove ' + package + '? [Y/n] : ')
                if remove == 'y' or remove == 'Y':
                    os.system('banana remove ' + package + ' -y')
                else:
                    exit(0)

        if not found:
            print(package_not_found)

def w():
    show()
    
def show():
    try:
        sys.argv[2]
    except IndexError:
        menu()
        exit(0)
    else:
        global found
        package = str(sys.argv[2])

        ### OPEN CSV
        with open(filecsv, 'r') as csvfile:
            csv_reader = csv.reader(csvfile)

            for line in csv_reader:
                if package in line[1]:
                    found = True

                    pkgname = line[1].split('-')[0]
                    version = line[1].replace(pkgname + '-','').replace('.mz','')

                    # Trying obtains .desc
                    internet = internet_on()
                    nodesc = True
                    if not line[2] == '':
                        if internet:
                            r = requests.get(url + line[0] + line[2])
                            if not r.status_code == 404:
                                # File not found (404 error) - pkgname-version.desc
                                nodesc = False
                                text = r.text

                    # Set maintainer and desc
                    if nodesc:
                        maintainer = '(unknown)'
                        desc = 'Description not available for this package!'

                        if not internet:
                            desc += '\n' + please_connect
                    else:
                        maintainer = (re.findall('maintainer.*', text)[0]).replace("'", '').replace('maintainer=', '').replace('"', '')
                        desc = ((text.split('|')[2]).replace('#', '').replace('=', '').replace('desc"', ''))[:-2]

                    print('Package Name: ' + pkgname)
                    print('Version.....: ' + version)
                    print('Maintainer..: ' + maintainer)
                    print(desc)
                    print('-------------------------------------------------------------------------\n')

            if not found:
                print(package_not_found)

def u():
    update()

def update():
    if internet_on():
        global textAnimate
        global done

        textAnimate = 'Updating '

        t = threading.Thread(target=animate)
        t.start()

        ### UPDATE WEB
        # r = requests.get(url)
        session = HTMLSession()

        r = session.get('https://mazonos.com/packages/')
        
        soup = BeautifulSoup(r.content, 'html.parser')
        links = soup.find_all('a')
        
        if os.path.isfile(filecsv):
            os.system('rm ' + filecsv)

        for link in links:
            if '/' in link.text:
                urls = url + link.text
                #r = requests.get(urls)
                r = session.get(urls)
                soups = BeautifulSoup(r.content, 'html.parser')
                linkss = soups.find_all('a')
                
                folder = link.text
                mz = ''
                desc = ''
                sha256 = ''

                for l in linkss:
                    if '.mz' in l.text:
                        if l.text.endswith(('.mz')):
                            mz = l.text
                       
                        if l.text.endswith(('.desc')):
                            desc = l.text

                        if l.text.endswith(('.sha256')):
                            sha256 = l.text
                        
                        if mz != '' and sha256 != '':
                            with open(filecsv, 'a') as new_file:
                                csv_writer = csv.writer(new_file)
                                csv_writer.writerow([folder,mz,desc,sha256])
                                mz = ''
                                desc = ''
                                sha256 = ''
        done = True
    else:
        print(please_connect)

def g():
    upgrade()

def upgrade():
    print('Upgrading...')

def c():
    checkdesc()

def checkdesc():
    if internet_on():
        update()

        global textAnimate
        global done

        found = False
        nodescs = []

        textAnimate = 'Searching '
        t = threading.Thread(target=animate)
        t.start()

        ### OPEN CSV
        with open(filecsv, 'r') as csvfile:
            csv_reader = csv.reader(csvfile)

            for line in csv_reader:
                if line[2] == '':
                    found = True
                    nodescs.append(' ' + line[0] + line[1] + '.desc -> not found!')
            done = True

        if found:
            print('The following packages do not have the .desc file:')
            for n in nodescs:
                print(n)
        else:
            print('All packages are OK!')

    else:
        print(please_connect)
        exit(0)

##------------------ END CHOOSE-FUNCTIONS ------------->

#------------------- END FUNCTIONS -------------------->

try:
    check()

    main()

    if choose in chooses:
        functions = locals()
        functions[choose]()
    else:
        menu()
        print('Invalid \"' + choose + '\" operation!')

except KeyboardInterrupt:
    print('\n')
    exit(0)

