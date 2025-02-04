# IT-HUSETS kompetenskvällar för IoT och AI

![Snodd bild med IoT + AI = AIoT](https://tm.shgstatic.com/prod/public/inline-images/AIoT.png)
## Instruktioner för första labbtillfället

[Här](https://youtu.be/YI772q5v3yI) är en bra genomgång av allt som tas upp nedan.

### Installera ett os

![Raspi imager](https://www.seeedstudio.com/blog/wp-content/uploads/2021/03/Screenshot-2021-03-22-at-11.49.41-AM.png)

Första steget är att installera senaste versionen av [Raspberry Pi OS](https://en.wikipedia.org/wiki/Raspberry_Pi_OS) på ett microSD minneskort. Du behöver 

1. En minneskortsläsare för microSD kort (Svante har med sig)
2. Ett microSD kort (Svante har med sig)
3. En dator (Du har med dig)
   1. med tillgång till internet 
   2. [Raspberry Pi Imager](https://www.raspberrypi.com/software/) installerat
   3. [VS Code](https://code.visualstudio.com/) installerat

Koppla ihop läsare-kort-dator och kör igång Raspberry Pi Imager. Du väljer först vilket os du vill installera. Förmodligen väljer du vanliga Raspberry Pi OS utan Desktop. Välj sedan vilket kort du vill skriva till. 

**Innan** du trycker på WRITE går du in i settings via kugghjulet och gör lite anpassningar enligt nedan. 

1. Sätt ett hostname (t.ex. <ditt_namn>_raspberry) så att du enkelt kan koppla upp dig mot din Raspberry från din laptop.
2. Kryssa i att SSH skpi
3. a vara påslaget med 'Use password authentication' ikryssat.
4. Kryssa i 'Set username and password'. Username 'pi' och Password 'raspberry' är vanliga värden, högst osäkert alltså men praktiskt.
5. Tänker du koppla dig mot din Raspberry via ett WLAN kan du konfigurera det redan här. Fyll i SSID och lösenord och vilket land vi sitter i om du är ambitiös. Sitter du på Rådhuset är SSID 'United Spaces' och lösenordet är 'USWFB2017'.
6. Locale kan också vara skönt att få rätt redan vid installation.

**Nu** kan du trycka på WRITE :-)

### Starta din Raspberry

![Raspberry Pi](https://upload.wikimedia.org/wikipedia/commons/thumb/f/f1/Raspberry_Pi_4_Model_B_-_Side.jpg/1200px-Raspberry_Pi_4_Model_B_-_Side.jpg)

Dags att pilla dit ditt nybrända minneskort och koppla i strömmen. Raspberryn bootar upp så fort du sätter i strömsladden och har du konfigurerat hostnamn och WLAN korrekt bör den nu vara synlig på nätverket under t.ex. 'svantes_raspberry.local'.

 Har du installerat en desktopversion av os:et så kommer du se ett vanligt skrivbord om du kopplar in en skärm till Raspberryn, annars ser du ett terminalfönster. 

 Slog du på SSH ovan så ska du kunna ssh:a in, t.ex. som 'pi@svantes_raspberry.local'. Imagen som du installerade är skapad i september 2022 så det har kommit lite uppdateringar sedan dess. Ssh:a in och kör en 'sudo apt-get update'.

 ### Koppla VSCode till din Raspberry

 ![VSCode och Raspberry Pi](https://cdn-learn.adafruit.com/guides/images/000/003/187/medium800/rpi_vscode.png)

Dra igång VSCode och - om du inte redan gjort det - installera tilläggen för **Python** och **Remote - SSH**, båda från Microsoft. **Remote - SSH** gör så att du kan utveckla 'remote' över ssh på en annan burk än den du kör VSCode på. Så, i VSCode

1. Tryck F1
2. Välj **Remote - SSH: Connect to host...**
3. Ange den användare och det hostnamn du använde tidigare t.ex. **pi@svantes_raspberry.local**
4. VSCode kommer att be om det lösenord du angav tidigare
5. Nu är du kopplad mot en VSCode som snurrar på din Raspberry, du behöver alltså installera de tillägg du vill ha tillgång till även här, kör på tillägget för **Python**!
6. Ta upp ett terminalfönster och skapa en folder för ditt projekt.
7. Öppna foldern i VSCode
8. Skapa en fil (t.ex. 'hello.py'), och lägg till lite python kod t.ex. `print("Hello IoT")`
9. Kör filen i ditt terminalfönster (`python3 hello.py`) och verifiera att allt ser ut som det ska.
10. Vackert! Nu kan du skriva, köra och debugga kod på din Raspberry från din laptomp precis som om det vore lokalt (kind of). 

### Koppla in din kamera
![Raspberry Camera module v3](https://cdn.windowsreport.com/wp-content/uploads/2020/05/Raspberry-Pi-camera-not-detected.jpg)

Plocka fram en [kamera](https://www.raspberrypi.com/products/camera-module-3/) och följ instruktionerna [här](https://projects.raspberrypi.org/en/projects/getting-started-with-picamera/2).

För att kolla att kameran verkligen fungerar kan du testa att
1. Öppna ett terminalfönster i din VSCode som kör mot Raspberryn
2. Ta en bild genom att köra `libcamera-jpeg -o test.jpg`
3. Markera test.jpg i VSCode och kolla att bilden ser ok ut.

Vill du labba mer med libcamera kan du hitta lite dokumentation [här](https://www.raspberrypi.com/documentation/computers/camera_software.html)

För att ta en bild m.h.a. lite python-kod kan du prova den här kodsnutten: 
```
from picamera2 import Picamera2, Preview
import time
picam2 = Picamera2()
camera_config = picam2.create_preview_configuration()
picam2.configure(camera_config)
picam2.start_preview(Preview.DRM)
picam2.start()
time.sleep(2)
picam2.capture_file("test.jpg")
```

Manualen för Picamera2 hitter du [här](https://datasheets.raspberrypi.com/camera/picamera2-manual.pdf).

### Använda CounterFit

Ett alternativ till att köra mot en Raspberry Pi är att istället använda sig av [CounterFit](https://pypi.org/project/CounterFit/) (gissa vem som utvecklar det...). CounterFit kan emulera diverse sensorer så istället för att koppla in fysiska kameror, knappar, fuktmätare eller vad det nu kan vara till din Raspberry Pi så definierar du upp dem m.h.a. en web-app. Du behöver även göra ett par mindre ändringar i koden. Testa gärna! 
   
* Om du råkar ut för felet `Attribute Error: module "dns.rdtypes" has no attribute ANY`, testa att installera om dnspython: `python3 -m pip install dnspython==2.2.1`



