# Operation Dual Pulse
The challenge says 2 halves are encrypted with different **small public exponent**. So we divide the cipher text into 2 halves according to the note such that the first half has one more digit than the second. Thus we brute force values of public exponent(e). Here's a python script for the same -
```python
from Crypto.Util.number import long_to_bytes, bytes_to_long
from sympy import S, root
ae = 6816757091858450713085690252512347544504019090609136967666012536910668779032690738201677981582888147117790777001276494880477034335888222339694782504925201944152400204217868050833169761970085965835517836485347672132736182362048602797612388464234872640680203246344040430752729725866469640024804817401228056815264532786222257534137077857320585835468143799994851067893825516775648874242756556412751046504386805623134325053594726350972470892897034898778122459853506312633039838182406191024230543812462040887558898777350457961388462592725658336283298460462754161276349420659463594405833841506832238806897758048324572264352921168937532872240116078334406953509150666924242152008508964936855096077208991807624087321478302356034420558971103857098385561867562975635214236812800319136802018308447502622697477966093910155065332671

be = 284403448357012400487440815268570393629291516009482128935860645772817104242195770352886952199727634356396948946904292267800746086172110619652134530644005241578093373346987533481814478956995285094892473531918284742365038199219063895278285241311129050962208373723371805045291276504183072882423921817067005263443085375074582840968538966115900393261136046198045584007907022276019405263042572669644265445973170980765539862996063837828496703377851050924225781574419671909528649345189218547326701840547365637401411646187639905020200750542337358429141073104384731457123561178933963405630674052158068350587514709553433517916924726502866911807936827721753468888786760123822894176681874942836240564975529093940533209640415185370773665478992394074445215255219935337206612590049043325923653904985261439779180089254361804767607973

for i in range(1,100):   
   res = root(S(ae), i)
   try:
   # Attempt to convert the long number to bytes
         result_bytes = long_to_bytes(res)
         print(result_bytes)
   except Exception as e:
   # Print the error message and continue
         print(f"An error occurred: {e}")

```
Just replace `ae` with `be` to get the second half of the flag.

**`flag{DEcrYpT10N_sUccessFul$.@#?_.@#d_att4cK_3421}`**

# Meta or Google?
Since the challenge has an image attachment and meta in the challenge name, we need to look into the metadata of the image. Using exiftool for the same -
```bash
% exiftool img2.jpg

ExifTool Version Number         : 13.25

File Name                       : img2 (2).jpg

Directory                       : .

File Size                       : 161 kB

File Modification Date/Time     : 2025:04:16 22:09:19+05:30

File Access Date/Time           : 2025:04:16 22:09:21+05:30

File Inode Change Date/Time     : 2025:04:16 22:09:19+05:30

File Permissions                : -rw-r--r--

File Type                       : JPEG

File Type Extension             : jpg

MIME Type                       : image/jpeg

JFIF Version                    : 1.01

Resolution Unit                 : None

X Resolution                    : 1

Y Resolution                    : 1

Comment                         : UEsDBAoAAAAAAA1biFqugyAtHQAAAB0AAAAIABwAZmxhZy50eHRVVAkAAxG69GcTuvRndXgLAAEE9QEAAAQUAAAAcnZjZWN0ZnttZXQ0ZGF0YTRhNGFfSXRfaVNTfQpQSwECHgMKAAAAAAANW4haroMgLR0AAAAdAAAACAAYAAAAAAABAAAApIEAAAAAZmxhZy50eHRVVAUAAxG69Gd1eAsAAQT1AQAABBQAAABQSwUGAAAAAAEAAQBOAAAAXwAAAAAA

Image Width                     : 1200

Image Height                    : 720

Encoding Process                : Baseline DCT, Huffman coding

Bits Per Sample                 : 8

Color Components                : 3

Y Cb Cr Sub Sampling            : YCbCr4:4:4 (1 1)

Image Size                      : 1200x720

Megapixels                      : 0.864
```

The comment seems unusual (looks like base64). On putting it in [cyberchef](https://gchq.github.io/CyberChef/) we get the flag. If you couldn't identify the string as base64, magic on cyberchef would've helped

**`rvcectf{met4data4a4a_It_iSS}`**

# Discus throw
you need to mount the disk, look into the contents using ls -al as the flag is in hidden directories
```bash
# create a directory to mount the contents
% sudo mkdir /mnt/mydisk/
% cd Downloads/
# mount the disk to the created directory
% sudo mount -o loop mydisk.img /mnt/mydisk/

#navigate to the directory
% cd /mnt/mydisk/

#view the contents using ls -al to view even the hidden directories, ... is a hidden directory
/m/mydisk % ls -la
total 19
drwxr-xr-x 4 root root  1024 Apr  9 15:49 ./
drwxr-xr-x 3 root root  4096 Apr 17 03:06 ../
drwxr-xr-x 2 root root  1024 Apr  9 15:47 .../
-rw-r--r-- 1 root root    36 Apr  9 15:49 hello.txt
drwx------ 2 root root 12288 Apr  9 15:44 lost+found/
#view the contents of the hidden directory
/m/mydisk % cd ...
/m/m/... % ls -la
total 136675
drwxr-xr-x 2 root root      1024 Apr  9 15:47 ./
drwxr-xr-x 4 root root      1024 Apr  9 15:49 ../
-rw-r--r-- 1 root root        39 Apr  9 15:47 .secret.txt
-rw-r--r-- 1 root root     28107 Apr  9 15:47 dummy.dat
-rw-r--r-- 1 root root 139921497 Apr  9 15:47 rockyou.txt

#.secret.txt has the flag
/m/m/... % cat .secret.txt 
Ectr(@ru=0A2/.@?Y+J$1NISE:JtPDBJafq5CE
```
Use cyberchef to detect, this is a base85 string -

**`rvcectf{d1sk_exp3rt_OR_wh4t??}`**
# Message board
Step 1: is SQL injection to get admin access
Just type `' OR 1=1--` in the username or password to get through (the condition 1=1 always evaluates to True hence access is granted)

Step 2: XSS to get flag
The challenge simulates XSS. You can identify this on viewing the source code.

Note that the `/post` endpoint directly renders the `message` input without any checks. This means you can execute anything that you want in the input. 
all you need to do is hijack this control and execute anything for this challenge and then view the endpoint.
`<script>alert('hacked')</script>` in the message board input box would also work, or any other message enclosed with `<script>` and `</script>` will work.

This makes the `/api/check-flag` endpoint accessible.,i.e,
`https://viciousmoonnn.pythonanywhere.com/api/check-flag` gives you the flag

XSS challenges are usually not that easy, the endpoint will be blocked always and you need to get the flag directly. Here is the actual payload -

`<script>fetch('/api/check-flag').then(r=>r.json()).then(d=>alert(d.flag))</script>`

The flag pops up

**`rvcectf{XSS_4nd_SQLi_M4st3r}`**

# Balderdash

The text looks like base64, put the file as input on cyberchef. Even magic can help you with this if you can't recognize that is is base64.

you get 
```
Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub! Blub? Blub. Blub? Blub. Blub. Blub. Blub? Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub? Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub? Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub? Blub. Blub? Blub. Blub? Blub. Blub? Blub. Blub! Blub! Blub? Blub! Blub. Blub? Blub. Blub? Blub. Blub? Blub. Blub? Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub! Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub! Blub. Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub. Blub. Blub. Blub. Blub. Blub! Blub. Blub! Blub! Blub! Blub! Blub! Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub! Blub. Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub! Blub. Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub. Blub! Blub. Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub! Blub.&nbsp;
Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook! Ook? &nbsp;Ook. Ook? &nbsp;Ook. Ook. &nbsp;Ook. Ook? &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook? &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook? &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook? Ook. &nbsp;Ook? Ook. &nbsp;Ook? Ook. &nbsp;Ook? Ook. &nbsp;Ook! Ook! &nbsp;Ook? Ook! &nbsp;Ook. Ook? &nbsp;Ook. Ook? &nbsp;Ook. Ook? &nbsp;Ook. Ook? &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook! Ook. &nbsp;Ook? Ook. &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook. Ook. &nbsp;Ook! Ook. &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook. &nbsp;Ook. Ook? &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook! &nbsp;Ook! Ook.&nbsp;
iiisdsiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiioddddddddddddoiiiiiiiiiiiiiiiiiioiodddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddoiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiioooiiiiiiiiiiio
```
on decrypting.

These are esolangs, you can use [dcodefr's cipher identifier](https://www.dcode.fr/cipher-identifier) to identify the languages and decrypt accordingly. The first one is `Blub`, second esolang is `Ook` and the third is `Deadfish`

[dcode.fr](https://www.dcode.fr/) has compilers for all of these esolangs
[Blub](https://www.dcode.fr/blub-language)
[Ook](https://www.dcode.fr/ook-language)
[Deadfish](https://www.dcode.fr/deadfish-language)
On decrypting -
`rvcectf{esol4N9_mast3rrr}`


# Buggy Game
This is an executable, it doesn't run on all systems, but on running it keeps asking you to guess the flag. So it's a dead end.

You can either get the flag by reverse engineering it using [ghidra](https://ghidra-sre.org/) or by simply running strings on the file.
```bash
% strings game

Enter the flag: 

rvcectf{re_easy_peasy}

Correct!

Wrong!
```

**`rvcectf{re_easy_peasy}`**
