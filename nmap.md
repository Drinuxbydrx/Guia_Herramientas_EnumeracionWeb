
# Nmap

![](imagenes/img1.png)

<div >


<h2 id="sintaxis">Sintaxis</h2>

La sintaxis principal de <b>Nmap</b> se compone de tres elementos:
<br>
<b>* Tipo de escaneo</b><br>
<b>* Opciones</b><br>
<b>* Objetivo</b><br>

</div>

```sh
nmap   -sS   -p- -sV -O -T4 -Pn -oN escaneo.txt   X.X.X.X
        |     |_______________________________|        |
 [Tipo de Escaneo]          [Opciones]             {Objetivo}
``` 

<div >

<h2 id="objetivo">Especificacion del objetivo</h2>

En especificaciones del objetivo principalmente se centra en como implementar el escaneo directo dentro de una ip o una red dada con CIDR tambien se centra en excluir algunos objetivos.<br>

<b>1.-  Direccion de Ip individual</b><br>

Escaneo basico de una sola direccion ip dada

```sh
nmap X.X.X.X

```

<b>2.- Nombre de dominio</b><br>

Nmap resuelve el Dns y escanea la ip asociada a ese dominio

```sh
nmap  scanme.com
``` 
<b>3.- Rango de Ips con CIDR</b><br>

Escanea de acuerdo al CIDR que se ponga 

```sh
nmap  X.X.X.X/24
``` 

<b>4.- Rango especifico de Ip's</b><br>

Escanea desde X.X.X.1 hasta X.X.X.254

```sh
nmap  X.X.X-255.1-254
``` 
<b>5.-Multiples objetivos separados por espacio</b><br>

Escanea tres Ip's especificas en solo comando

```sh
nmap  X.X.X.X X.X.X.X X.X.X.X
```
<b>6.- Excluir sistemas especificos</b><br>

Se excluye dentro e un cidr un grupo de ip's deseignado


```sh
nmap  X.X.X.X/24 --exclude X.X.X.100,X.X.X.200
```

<b>7.-Usando archivos con lista de objetivos</b><br>

Creamos una lista de ip's y las guardamos dentro de un archivo con formato txt

echo "X.X.X.100" >> archivo.txt
echo "X.X.X.200" >> archivo.txt
echo "X.X.X.300" >> archivo.txt

```sh
nmap  -iL archivo.txt 
```
<b>8.- Excluir desde archivo</b><br>

Creamos un archivo.txt el cual va a contener las ip's que deseamos excluir

echo "X.X.X.100" >> archivo.txt
echo "X.X.X.200" >> archivo.txt
echo "X.X.X.300" >> archivo.txt

```sh
nmap  X.X.X.X/24 --excludefile archivo.txt 
```

<b>9.-Objetivos aleatorios</b> <br>

La bandera <b>-iR</b> realiza un escaneo aleatorio en internte de un numero designado de ip's

```sh
nmap  -iR 10 --open 

Donde: Realiza un escaneo de 10 ip's aleatorias
```

<h2 id="hosts"> Descubrimiento de Hosts</h2>

Principalmente esta seccion se enfoca en el descubrimiento del host incluyendo distintos metodos de ping y resolucion de nombres.<br><br>

<b>1.- Sondeo de lista : -sL</b><br>

Solo lista las ip's que se encuntren dentro del rango sin enviar ningun paquete.

```sh
nmap  -sL  X.X.X.X/24  
```

<b>2.- Sondeo de ping : -sP</b><br>

Envia paquetes ICMP echo request y tcp ack para determinar que hosts estan activos.

```sh
nmap  -sP  X.X.X.X  
```

<b>3.- Asume que el objetivo esa vido : -P0</b><br>

Asume que el objetivo esta vivo y salta la fase de descubrimiento.Util cuando el objetivo bloquea los pings.

```sh
nmap  -P0  X.X.X.X  
```
<b>4.- TCP/SYN : -PS</b><br>

Envia paquetes TCP SYN a puerto 80 por defecto si recibe SYN/ZCK o RST el host esta vivo.

```sh
nmap  -PS  X.X.X.X  
```

<b>5.- TCP/ACK : -PA</b><br>

Util para evadir firewall que filtran SYN pero permiten ACK.

```sh
nmap  -PA  X.X.X.X  
```

<b>6.- UDP Ping : -PU</b><br>

Envia paquetes UDP a puerto 40125 por defecto.Si recibe ICMP port unreachable el host esta vivo

```sh
nmap  -PU  X.X.X.X  
```
<b>7.- ICMP Ping : -PE,-PP,-PM</b><br>

ICMP Echo request (ping tradicional)

```sh
nmap  -PE  X.X.X.X  
```
ICMP Timestamp request

```sh
nmap  -PP  X.X.X.X  
```
ICMP Netmask request

```sh
nmap  -PM  X.X.X.X  
```

<b>8.- Resolucion DNS : -n,-r</b><br>

No realiza resolucion DNS

```sh
nmap  -n  X.X.X.X  
```
Siempre realiza resolucion DNS

```sh
nmap  -r  X.X.X.X  
```
<b>9.- Busqueda dns servers especificos : --dns-servers</b><br>

Se puede utilizar servidores de google,cloudflare o cualquier servidor para realizar la busqueda dns.<br>

Google DNS 8.8.8.8<br>
CloudFlareDNS 1.1.1.1<br>

```sh
nmap  --dns-servers 8.8.8.8 , 1.1.1.1 pagina.com 
```
<b>10.- Dns del sistema : --system-dns</b><br>

Se puede utilizar un dns del mismo sistema para realizar la consulta

```sh
nmap  --system-dns X.X.X.X 
```
<h2 id="puertos"> Puertos</h2><br>

Esta seccion de nmap se utiliza principalmente para especificar el rango de puertos que se van auditar en el objetivo.<br>

<b>1.-Puertos especificos: -p </b><br>

Escanea un rango de puertos que nosostros le especifiquemos.<br>

```sh
nmap  -p 22,80,443 X.X.X.X 
```
```sh
nmap  -p 1-1000 X.X.X.X 
```
Escanea todos los puertos<br>

```sh
nmap  -p- X.X.X.X 
```
Escaneo mixto con tcp/udp

```sh
nmap  -p U:53,111,T:21-25,80 X.X.X.X 
```

<b>2.-Escaneo rapido: -F </b><br>

Escanea los 100 puertos mas comunes.<br>

```sh
nmap  -F X.X.X.X 
```
Escanea los 500 puertos mas comunes.<br>

```sh
nmap --top-ports 500 X.X.X.X
```

<h2 id="servicios">Deteccion de Servicios</h2>

Principalmente se enfoca en detectar que servicio utiliza el objetivo por ejemplo pueder ser un servicio Apache con version 2.1.1.<br>

<b>1.-Deteccion de servicio y version: -sV </b><br>

```sh
nmap -sV X.X.X.X
```

<h2 id="os">Sistema Operativo</h2>

Se enfoca en obtener el sistema operativo del objetivo.<br>

<b>1.-Detecccion de sistema operativo : -O</b><br>

```sh
nmap  -O X.X.X.X 
```
<b>2.-Deteccion agresiva del sistema operativo : -o</b><br>

```sh
nmap -o --osscan-guess X.X.X.X
```

<h2 id="analisis">Tecnicas de analisis</h2>

En esta seccion se presenta las principales tecnicas de analisis con las que se cuentra dentro de nmap.

<b>1.-TCP/SYN : -sS</b><br>

Envia paquetes SYN-> si recibe SYN/ACK puerto abierto<br>
no completa el 3 way-handshake

```sh
nmap -sS X.X.X.X
```
<b>2.-TCP Connect Scan : -sT</b><br>
Completa el 3 way-handshake

```sh
nmap -sT X.X.X.X
```
<b>3.-TCP Ack-Scan : -sA</b><br>
Envia paquetes ACK

```sh
nmap -sA X.X.X.X
```
<b>4.-TCP window : -sW</b><br>
Similar a ACK scan pero analiza el tamaño de la ventana TCP.

```sh
nmap -sW X.X.X.X
```
<b>5.-TCP Maimon Scan : -sM</b><br>

Envia paquetes Fin/Ack
```sh
nmap -sM X.X.X.X
```
<b>6.-TCP Maimon Scan : -sN,-sF,-sX</b><br>

Null scnan no flags envia banderas nulas:-sN 

```sh
nmap -sN X.X.X.X
```
Solo se envia la ultima bandera FIN/ACK Fin Scan :-sF

```sh
nmap -sF X.X.X.X
```
Se envian las tres banderas del 3way handshake y adememas en estado urgente
Xmass Scan : -sX

```sh
nmap -sX X.X.X.X
```
<b>7.-Custom TCPFlags : --scanflags</b><br>

SYN --- Sincronización (inicio de conexión)<br>ACK --- Confirmación<br>FIN --- Finalizacion<br>RST --- Reset<br>PSH --- Push (enviar datos inmediatamente)<br>URG --- Urgente<br>ECE, CWR --- Control de congestión (menos usadas en pentesting)

```sh
nmap --scanflags SYN X.X.X.X
nmap --scanflags SYNFIN X.X.X.X
nmap --scanflags URG,PSH,FIN X.X.X.X
```


<h2 id="evasion">Evasion de firewall</h2>

<b>1.-Fragmentacion de paquetes : -f</b><br>

Divide los paquetes en fragmentos pequeños de 8 bytes.

```sh
nmap -f X.X.X.X
```
<b>2.-DecoyScanning : -D </b><br>

El firewall ve peticiones desde multiples ip's

```sh
nmap -D RND:S X.X.X.X
```
```sh
nmap -D X.X.X.X,X.X.X.X,ME,X.X.X.X,X.X.X.X
```
Donde:

Me: nuestra propia ip<br>
RND:S Genera ip's aleatorias<br>
S: se puede cambiar por un numero<br>
RND:10 Genera 10 ip's aleatorias<br>

Nota:<br>
Los decoys funcionan mejor con técnicas de escaneo tipo -sS, -sF, -sX, -sN (escaneos "crudos" que no requieren respuesta bidireccional completa). No funcionan bien con -sT (TCP Connect), porque ese escaneo sí requiere una conexión completa desde tu IP real, y tampoco es compatible con detección de versión (-sV) ni con NSE en muchos casos.<br>

<b>3.-Ip Spoofing : -S </b><br>

```sh
nmap -S X.X.X.X .e eth0 X.X.X.X
```
Donde :<br>
-S:Falsifica ip origen<br>
-e:Especifica interfaz de salida<br>

<b>4.-Source port spoofing : -g </b><br>
Puede evadir reglas de firewall que permiten DNS.

```sh
nmap -g 53 X.X.X.X
```
<b>5.-Agregar datos aleatorios : --data-length </b><br>
Agrega 100 byts de datos aleatorios

```sh
nmap --data-length 100 X.X.X.X
```
<b>6.-Manipulacion ttl : --ttl </b><br>

```sh
nmap --ttl 255 X.X.X.X
```
<b>7.-Macc Adress Spoofing : --spoof-mac </b><br>

Mac Especifica

```sh
nmap --spoof-mac 00:11:22:33:44:55 X.X.X.X
```

Por nombre de fabricante 
```sh
nmap --spoof-mac Dell X.X.X.X
nmap --spoof-mac Apple X.X.X.X
nmap --spoof-mac Cisco X.X.X.X
```
Completamente aleatoria
```sh
 nmap --spoof-mac 0 X.X.X.X
```
Donde:

0:Indica que genere una mac completamente aleatoria.
<h2 id="timing">Temporizado</h2>

<b>1.-Temporizado : -T </b><br>
Con la bandera -T podemos controlar el temprizado de las peticio9nes que realiza nmap.

```sh
 nmap -T0 X.X.X.X   # Lento
 nmap -T1 X.X.X.X
 nmap -T2 X.X.X.X
 nmap -T3 X.X.X.X   # Normal
 nmap -T4 X.X.X.X
 nmap -T5 X.X.X.X   # Muy rapido
```
<b>2.-Maximo de reintentos : --max-retries </b><br>
Con el parametro --max-retries controlamos cuantas veces debe intentar reenviar un paquete a un puerto.

```sh
nmap --max-retries 3 X.X.X.X
```

<b>3.-Tiempo limite por hosts : --host-timeout </b><br>
Con esta bandera establecemos un tiempo para el escaneo de un objetivo si el host tarda mas del tiempo estipulado nmap cierra conxion.

```sh
nmap --host-timeout 1m X.X.X.X
```
Donde : <br>
ms:milisegundos<br>
s:segundos<br>
m:minutos<br>

<b>4.-Retraso entre escaneos : --scan-delay </b><br>
Este parametro es de gran utilidad cuando queremos retrsar el tiempo por cada peticion que se hace con nmap.<br>

```sh
nmap --scan-delay 200ms X.X.X.X
```
<h2 id="output"> Formatos de salida</h2>

<b>1.-Texto plano : -oN </b><br>

```sh
nmap -oN X.X.X.X
```
<b>2.-XML : -oX </b><br>

```sh
nmap -oX X.X.X.X
```
<b>3.-Grepable : -oG </b><br>

```sh
nmap -oG X.X.X.X
```
<b>4.-Script kiddie : -oS </b><br>

```sh
nmap -oS X.X.X.X
```
<b>5.-Todos menos -oS : -oA </b><br>

```sh
nmap -oA X.X.X.X
```
Notas:

Muy independientemente de estos formatos de salida tambien uno puede agregar el escaneo en un archivo.A qui se vieron de forma unica los comandos pero se pueden realizar diferentes combinaciones de estos como se enmarca en el apartado de sintaxis.
<br>
<b>></b> Un solo signo: Borra y escribe. Si el archivo mi_reporte.txt ya existia con informacion vieja, la borra por completo y guarda lo nuevo.<br>
<b>>></b>Doble signo: Añade al final. Si el archivo ya existia, respeta lo que tiene y agrega el nuevo escaneo al final del documento.

```sh
nmap -sV -sS -n -T 2 X.X.X.X >> reporte_nmap.txt
```

</div>
